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

## 7) Giải thích logic cũ: vì sao nhìn đúng nhưng vẫn lỗi

### 7.1 Mục tiêu mà người viết cũ có thể đang nghĩ tới

- Khi người chơi chạm portal, hệ thống sẽ "đảm bảo trạng thái sạch" rồi mới vào map kế.
- Nếu có checkpoint thì nạp lại để tránh mất dữ liệu.
- Nếu cần thì mở lại intro/cutscene để giữ mạch truyện.

Ý tưởng này **không sai về mặt ý định**, nhưng bị đặt vào **sai ngữ cảnh** (portal transition trong lúc đang chơi).

### 7.2 Ý nghĩa từng cụm dòng đã xóa ở `case 0` (map 1 -> 2)

```java
game.saveManager.clearCheckpoint();
```
- Xóa checkpoint đang có trong Preferences (`mapIndex`, `playerX`, `playerY`, `hp`, `mp`, `route`).
- Hành vi này giống "khởi động game mới", không phải hành vi "qua map kế".

```java
game.storyState.resetFlags();
game.storyState.setCurrentRoute(com.paradise_seeker.game.story.RouteType.NORMAL);
```
- Xóa toàn bộ cờ truyện và ép route về `NORMAL`.
- Điều này làm mất liên tục trạng thái cốt truyện khi người chơi chỉ đang đi từ map 1 sang map 2.

```java
game.currentGame = null;
game.inventoryScreen = null;
game.currentGame = new GameScreen(game);
```
- Hủy màn chơi hiện tại rồi tạo `GameScreen` mới hoàn toàn.
- Toàn bộ runtime state của màn cũ (map manager hiện tại, tham chiếu object theo frame, trạng thái tức thời) bị thay thế.

```java
game.setScreen(new IntroCutScene(game));
```
- Chuyển sang intro mở đầu game.
- Đây là lý do trực tiếp khiến người chơi có cảm giác "đi portal map 1 nhưng quay về Start/Intro".

### 7.3 Ý nghĩa từng cụm dòng đã xóa ở `case 1` (map 2 -> 3)

```java
game.currentGame = null;
game.inventoryScreen = null;
game.currentGame = new GameScreen(game);
```
- Tương tự `case 0`: phá continuity bằng cách tạo màn chơi mới ngay lúc chạm portal.

```java
if (game.saveManager != null && game.saveManager.hasCheckpoint()) {
    game.saveManager.loadCheckpoint(game.currentGame, game);
}
```
- Nạp checkpoint ngay tại thời điểm portal vừa trigger.
- Nếu checkpoint lưu map cũ (thường là map 2 hoặc vị trí trước đó), người chơi sẽ bị kéo ngược trạng thái nên trông như "không qua map 3".

```java
game.setScreen(game.currentGame);
```
- Chuyển màn hình sang instance vừa tạo và vừa load checkpoint.
- Về thực chất đang ưu tiên "phục hồi save" thay vì "tiếp tục luồng chuyển map hiện tại".

### 7.4 Điểm mấu chốt gây lỗi

- Các lệnh cũ **đúng cú pháp**, và từng lệnh riêng lẻ đều có lý do tồn tại.
- Nhưng chúng thuộc nhóm logic của:
  - `New Game` (xóa save/reset story/tạo game mới), hoặc
  - `Continue` (load checkpoint).
- Trong khi portal cần nhóm logic khác: **chuyển map liền mạch trong cùng phiên chơi**.

=> Sai không phải ở câu lệnh đơn lẻ, mà sai ở **đúng việc nhưng đặt sai chỗ**.

### 7.5 Vì sao logic mới ổn định hơn

- Giữ nguyên `GameScreen` hiện tại.
- Chỉ phát cutscene cuối chapter một lần (`EndMap1`, `EndMap2`).
- Sau đó chuyển map bằng `mapManager.switchToNextMap()` và lưu checkpoint mới.
- Cách này đồng nhất với luồng map 3->4, 4->5, 5->ending nên không còn nhảy ngược intro/map cũ.
# Báo cáo đổi tên danh sách thành viên ở màn hình chiến thắng

## 1 Vị trí thay đổi

Phần cần sửa nằm trong `core/src/main/java/com/paradise_seeker/game/screen/WinScreen.java`, tại mảng `members` dùng để hiển thị credit ở màn hình chiến thắng.

## 2 Nội dung trước khi đổi tên

```java
String[] members = {
    "Nguyen Hoang Hiep", "Le Thanh An", "Pham Yen Nhi",
    "Bui Tuan Anh", "Nguyen Hoang Long", "Trinh Van Minh"
};
```
- ![img.png](img.png)

## 3 Nội dung sau khi đổi tên

```java
String[] members = {
        "Nguyen Thanh Trung - 202417056", "Pham Van An - 202417226", "Pham Van An - 202416841",
        "Ha Tien Dat - 202417231", "Tran Minh Duong - 202417234"
};
```
-![img_1.png](img_1.png)
