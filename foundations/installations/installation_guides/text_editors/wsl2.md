### Hướng dẫn: Cài đặt VSCode trên WSL2

#### Bước 1: Cài đặt VSCode

Làm theo hướng dẫn [cài đặt Visual Studio Code trên Windows](https://code.visualstudio.com/docs/setup/windows).

**Lưu ý quan trọng**: Trình cài đặt Windows mặc định trên trang được cung cấp giả định bạn đang sử dụng máy x64. Nếu dùng máy ARM64, bạn cần nhấp vào liên kết bên dưới bảng rồi chọn đúng phiên bản trình cài đặt cho máy của mình.

#### Bước 2: Xóa tệp cài đặt

1. Mở **File Explorer**.
1. Đi tới thư mục **Downloads**.
1. Kéo `VSCodeUserSetup-{version}.exe` vào thùng rác.

#### Bước 3: Cài đặt tiện ích WSL

1. Mở Visual Studio Code.
1. Điều hướng đến tab tiện ích mở rộng.
1. Tìm và cài đặt [tiện ích mở rộng WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl).

#### Bước 4: Bảo đảm WSL2 có thể mở VSCode đúng cách

1. Mở một terminal WSL2 mới.
1. Chạy lệnh sau để mở một cửa sổ VSCode mới trong WSL2:

   ```bash
   code
   ```

Sau một lát, một cửa sổ VSCode mới sẽ mở ra và VSCode sẽ hiển thị thông báo cho biết chương trình đang mở trong WSL2.

Từ nay về sau, bạn nên mở VSCode theo cách này để bảo đảm chương trình sử dụng WSL2 thay vì hệ điều hành/hệ thống tệp Windows.

[Quay lại bài học Trình soạn thảo văn bản](https://www.theodinproject.com/lessons/foundations-text-editors#assignment)