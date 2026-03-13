# OOP Game Project

README này hướng dẫn cách tải project, kiểm tra Java, compile và chạy game để test.

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

## 6. Quy trình test ngắn gọn cho thành viên nhóm

Mỗi người trong nhóm chỉ cần làm theo thứ tự này:

### Bước 1
Tải repo về hoặc tải ZIP rồi giải nén.

### Bước 2
Mở PowerShell tại thư mục `OOP`.

### Bước 3
Kiểm tra Java:

```powershell
java -version
javac -version
```

### Bước 4
Compile:

```powershell
.\gradlew.bat core:compileJava --console=plain
```

### Bước 5
Chạy game:

```powershell
.\gradlew.bat lwjgl3:run
```

---

## 7. Nếu gặp lỗi thường gặp

### Lỗi 1: `JAVA_HOME is not set`
Nguyên nhân:
- chưa cài Java
- hoặc Java chưa vào PATH

Cách xử lý:
- cài JDK 17 bằng lệnh:

```powershell
winget install EclipseAdoptium.Temurin.17.JDK
```

- sau đó mở lại terminal và kiểm tra:

```powershell
java -version
javac -version
```

---

### Lỗi 2: `File not found: ...`
Nguyên nhân:
- thiếu asset trong project
- hoặc giải nén/clone chưa đầy đủ

Cách xử lý:
- kiểm tra lại repo đã tải đủ chưa
- nếu là ZIP, giải nén lại đầy đủ
- nếu là git clone, thử pull lại repo mới nhất

---

### Lỗi 3: Game compile được nhưng chạy bị crash
Nguyên nhân:
- thường là thiếu asset hoặc lỗi runtime

Cách xử lý:
- chụp đúng dòng lỗi trong terminal
- gửi lại cho người phụ trách code để check

---

## 8. Ghi chú

- Chạy game trên **Windows PowerShell** thường ổn định hơn WSL.
- Nếu chỉ cần kiểm tra code có compile không, dùng:

```powershell
.\gradlew.bat core:compileJava --console=plain
```

- Nếu cần test game thật, dùng:

```powershell
.\gradlew.bat lwjgl3:run
```

---

## 9. Lệnh nhanh tổng hợp

### Kiểm tra Java
```powershell
java -version
javac -version
```

### Cài Java 17
```powershell
winget install EclipseAdoptium.Temurin.17.JDK
```

### Vào thư mục project
```powershell
cd D:\OOP
```

### Compile
```powershell
.\gradlew.bat core:compileJava --console=plain
```

### Build toàn bộ
```powershell
.\gradlew.bat build --console=plain
```

### Chạy game
```powershell
.\gradlew.bat lwjgl3:run
```
