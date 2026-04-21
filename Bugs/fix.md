# Fix lỗi chuyển map 1 -> 2 và 2 -> 3

## 1) Hiện tượng lỗi

Theo ghi nhận trong `Bugs/Bugs.md`:

- Vào cổng dịch chuyển của map 1 thì quay về luồng mở đầu game, không sang map 2 đúng cách.
- Cổng dịch chuyển map 2 không qua được map 3, có lúc quay lại map hiện tại/cũ.

## 2) Nguyên nhân kỹ thuật

Vấn đề nằm trong `handlePortalsEvent()` của `core/src/main/java/com/paradise_seeker/game/screen/GameScreen.java`.

### Trước khi sửa

- Ở `case 0` (map 1 -> 2), code cũ **xóa checkpoint + reset story + tạo mới `GameScreen` + mở `IntroCutScene`**.
- Ở `case 1` (map 2 -> 3), code cũ **tạo mới `GameScreen` + load checkpoint ngay khi chạm cổng**.

Hai việc này làm luồng portal bị phá:

1. Chạm cổng nhưng lại chạy như "New Game" (đặc biệt ở map 1).
2. Vừa chạm cổng đã nạp checkpoint cũ, nên vị trí/map có thể bị kéo ngược.
3. Mất tính liên tục state hiện tại vì khởi tạo `GameScreen` mới giữa lúc chuyển map.

## 3) Đã sửa như thế nào

### 3.1 Các dòng đã xóa

#### A. Xóa trong `case 0` (map 1 -> 2)

```java
game.saveManager.clearCheckpoint();
game.storyState.resetFlags();
game.storyState.setCurrentRoute(com.paradise_seeker.game.story.RouteType.NORMAL);

game.currentGame = null;
game.inventoryScreen = null;
game.currentGame = new GameScreen(game);

game.setScreen(new IntroCutScene(game));
```

#### B. Xóa trong `case 1` (map 2 -> 3)

```java
game.currentGame = null;
game.inventoryScreen = null;
game.currentGame = new GameScreen(game);

if (game.saveManager != null && game.saveManager.hasCheckpoint()) {
    game.saveManager.loadCheckpoint(game.currentGame, game);
}

game.setScreen(game.currentGame);
```

#### C. Xóa import không còn dùng

```java
import com.paradise_seeker.game.screen.cutscene.IntroCutScene;
```

### 3.2 Các dòng đã thêm

#### A. Thêm trong `case 0` (map 1 -> 2)

```java
if (mapcutsceneIndicesEnd[0] == 0) {
    mapcutsceneIndicesEnd[0] = 1;
    game.setScreen(new EndMap1(game));
}
```

#### B. Thêm trong `case 1` (map 2 -> 3)

```java
if (mapcutsceneIndicesEnd[1] == 0) {
    mapcutsceneIndicesEnd[1] = 1;
    game.setScreen(new EndMap2(game));
}
```

## 4) Logic sau khi sửa

Portal map 1->2 và 2->3 giờ hoạt động cùng kiểu với các map sau:

1. Kiểm tra điều kiện qua cổng (đã dọn quái/boss/key).
2. Nếu chưa phát cutscene chuyển chapter thì mở `EndMap1` hoặc `EndMap2` một lần.
3. Tiếp tục luồng chuyển map tại chỗ:
   - `mapManager.switchToNextMap();`
   - `switchMusicAndShowMap();`
   - `game.saveManager.saveCheckpoint(this, game);`
4. Không tạo mới `GameScreen` trong lúc chạm cổng.

## 5) Kết quả mong đợi

- Map 1 qua map 2 không còn quay về intro/start.
- Map 2 qua map 3 không còn bị load ngược checkpoint cũ.
- Trải nghiệm chuyển map mượt và đồng nhất với map 3->4, 4->5, 5->kết game.

## 6) Ghi chú

- Việc reset route/checkpoint phù hợp hơn khi bấm `New Game` ở `MainMenuScreen`, không nên đặt trong logic portal.
- Nếu còn hiện tượng "đụng cổng bị giật/lặp", có thể bổ sung thêm cooldown portal (debounce) để chặn trigger liên tiếp theo frame.

