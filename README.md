# OOP Game Project

README này hướng dẫn cách tải project, kiểm tra Java, compile và chạy game để test.

Đồng thời mọi người khi chạy game thì hộ trợ test game xem còn chỗ nào có bugs, chỗ nào chưa hợp lý với logic game thì mọi người điền bugs và miêu tả bugs vào file Bugs.md có ở repo. Nếu có bugs như tự dưng đang chơi đến đoạn nào mà game tự tắt hãy vào terminal mà mọi người dùng để chạy lệnh ```.\gradlew.bat lwjgl3:run``` và chụp ảnh hoặc gửi logs failed vào file Bugs.md.

## 1. Tải project về máy

### Cách 1: Tải file ZIP từ GitHub
1. Vào repo GitHub của nhóm.
2. Bấm **Code**.
3. Chọn **Download ZIP**.
4. Giải nén file ZIP ra máy.

Sau khi giải nén, bạn sẽ thấy thư mục project, ví dụ:

```text
D:\OOP
```

### Cách 2: Dùng git clone
Nếu máy đã có git, có thể clone trực tiếp:

```powershell
git clone https://github.com/minhduong06-td/OOP.git
```

Sau đó vào thư mục project:

```powershell
cd .\OOP
```

---

## 2. Vào đúng thư mục root của project

Sau khi giải nén hoặc clone xong, mở **PowerShell** hoặc **CMD** tại thư mục root `OOP`.

Ví dụ với PowerShell:

```powershell
cd D:\OOP
```

Kiểm tra nhanh thư mục hiện tại đã đúng chưa:

```powershell
dir
```

Nếu đúng, bạn phải thấy các file/thư mục như:
- `gradlew.bat`
- `build.gradle`
- `settings.gradle`
- `core`
- `lwjgl3`

---

## 3. Kiểm tra máy đã có Java chưa

Trong PowerShell, chạy:

```powershell
java -version
javac -version
```

### Nếu máy đã có Java
Bạn sẽ thấy kết quả kiểu:

```text
openjdk version "17.x.x"
javac 17.x.x
```

Khi đó chuyển sang bước 4.

### Nếu máy báo lỗi kiểu:
```text
'java' is not recognized...
```

hoặc

```text
ERROR: JAVA_HOME is not set...
```

thì máy chưa có Java hoặc chưa cấu hình PATH đúng. Hãy làm bước 3.1.

---

## 3.1. Cài Java 17 trên Windows

Khuyến nghị dùng **JDK 17**.

### Cài bằng winget
Mở **PowerShell với quyền bình thường hoặc admin**, chạy:

```powershell
winget install EclipseAdoptium.Temurin.17.JDK
```

Cài xong:
1. Đóng PowerShell.
2. Mở lại PowerShell mới.
3. Kiểm tra lại:

```powershell
java -version
javac -version
```

Nếu vẫn chưa nhận, hãy mở lại máy hoặc đăng xuất rồi đăng nhập lại.

---

## 4. Compile project

Sau khi đã vào đúng thư mục `OOP` và máy có Java, chạy lệnh compile:

```powershell
.\gradlew.bat core:compileJava --console=plain
```

### Nếu compile thành công
Bạn sẽ không thấy lỗi Java source, import, class missing, v.v.

### Nếu muốn build toàn bộ project
Có thể dùng:

```powershell
.\gradlew.bat build --console=plain
```

---

## 5. Chạy game

Khuyến nghị chạy trực tiếp trên **Windows PowerShell**, không nên ưu tiên WSL để test game desktop.

Chạy:

```powershell
.\gradlew.bat lwjgl3:run
```

Nếu mọi thứ ổn, game sẽ mở cửa sổ và vào menu chính.

---

## 6. Debug mode 
Project có hỗ trợ một số phím debug để giúp test game nhanh hơn mà không cần chơi lại từ đầu nhiều lần.
Các phím này chỉ hoạt động khi file `DebugHotkeys.java` đang bật debug.

- **Bật debug:** `public static final boolean ENABLED = true;`
- **Tắt debug:** `public static final boolean ENABLED = false;`

---

### ⌨️ Danh sách phím debug

| Phím | Tên chức năng | Mục đích sử dụng | Tác dụng |
| :---: | :--- | :--- | :--- |
| **F1** | **Hồi đầy máu và mana** | Dùng để hồi lại nhân vật ngay lập tức khi đang test combat. | - Hồi đầy HP<br>- Hồi đầy MP |
| **F2** | **Giết toàn bộ quái ở map hiện tại** | Dùng để test nhanh việc qua màn, mở cổng, hoặc kiểm tra ending mà không cần đánh từng con quái. | - Tất cả quái ở map hiện tại sẽ bị hạ ngay |
| **F3** | **Dịch chuyển tới cổng của map hiện tại** | Dùng để test logic cổng, chuyển map, hoặc kiểm tra điều kiện mở cổng. | - Nhân vật được đưa ngay tới vị trí portal của map đang đứng |
| **F4** | **Cấp đủ key fragments** | Dùng để test cổng khóa hoặc các màn cần chìa khóa / mảnh khóa. | - Thêm đủ các mảnh key vào inventory |
| **F5** | **Chuyển ngay sang map kế tiếp** | Dùng để test nhanh nhiều map mà không cần tự đi qua từng màn. | - Nhảy sang map tiếp theo<br>- Đổi nhạc / trạng thái map tương ứng |
| **F9** | **Ép route về NORMAL** | Dùng để test normal ending. | - Route hiện tại được set thành NORMAL |
| **F10**| **Ép route về TRUE** | Dùng để test true ending mà không cần mở route thật bằng gameplay. | - Route hiện tại được set thành TRUE |
| **F11**| **Ép route về OBSERVER** | Dùng để test observer ending mà không cần làm đủ các điều kiện đặc biệt. | - Route hiện tại được set thành OBSERVER |

---

### Cách test nhanh đề xuất

#### 1. Test chuyển map nhanh
* Vào game.
* Bấm **F5** để nhảy sang map tiếp theo.
* Nếu cần tới cổng nhanh, bấm **F3**.

#### 2. Test cổng khóa / key
* Vào map có cổng khóa.
* Bấm **F4** để nhận đủ key fragments.
* Đi tới cổng và kiểm tra logic mở cổng.

#### 3. Test boss / ending
* Dùng **F5** để tới nhanh map cần test.
* Bấm **F2** để dọn quái hoặc giết boss nhanh.
* **Trước khi kết thúc màn:**
  * Bấm **F9** để test NORMAL.
  * Bấm **F10** để test TRUE.
  * Bấm **F11** để test OBSERVER.

#### 4. Test sống / chết / combat
* Để quái đánh thử nhân vật.
* Khi cần hồi máu & mana nhanh, bấm **F1**.

