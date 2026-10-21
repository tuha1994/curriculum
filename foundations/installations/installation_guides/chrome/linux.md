### Hướng dẫn: Cài đặt Chrome trên Linux

#### Bước 1: Tải Google Chrome

1. Mở **Terminal**
1. Chạy lệnh sau để tải gói `.deb` **Google Chrome** mới nhất

   ```bash
   wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
   ```

<div class="lesson-note" markdown="1">

#### Phím tắt sao chép và dán

Có lẽ bạn đã nhận thấy phím tắt bàn phím thông dụng: <kbd>Ctrl</kbd> + <kbd>V</kbd> để dán nội dung không hoạt động trong terminal. Để dán nội dung văn bản vào terminal, thay vào đó bạn có thể dùng tổ hợp phím tắt <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>V</kbd>. Bạn cũng nên biết rằng tổ hợp phím <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> sẽ sao chép mọi văn bản được bôi đen trong terminal để bạn có thể dán vào lúc khác.

</div>

#### Bước 2: Cài đặt Google Chrome

1. Nhập lệnh sau vào terminal để cài đặt gói `.deb` **Google Chrome**

   ```bash
   sudo apt install ./google-chrome-stable_current_amd64.deb
   ```

1. Nhập mật khẩu nếu được yêu cầu

<div class="lesson-note lesson-note--tip" markdown="1">

#### Lưu ý về việc nhập mật khẩu trong terminal

Khi sử dụng một lệnh trong terminal yêu cầu bạn nhập mật khẩu để xác thực (chẳng hạn như `sudo`), bạn sẽ nhận thấy các ký tự không hiển thị khi nhập. Dù có vẻ terminal không phản hồi, bạn đừng lo!

Đây là một tính năng bảo mật nhằm bảo vệ thông tin bí mật, tương tự như cách các trường mật khẩu trên website sử dụng dấu hoa thị hoặc dấu chấm. Bằng cách không hiển thị các ký tự bạn nhập, terminal giữ cho mật khẩu của bạn được an toàn.

Bạn vẫn có thể nhập mật khẩu như bình thường rồi nhấn <kbd>Enter</kbd> để gửi mật khẩu.

</div>

<div class="lesson-note lesson-note--tip" markdown="1">

#### Tệp tải xuống được thực hiện mà không có sandbox

Bạn có thể [bỏ qua mọi ghi chú có nội dung `N: Download is performed unsandboxed (...)`](https://www.reddit.com/r/linux4noobs/comments/ux6cwx/comment/i9x2twx/).

</div>

#### Bước 3: Xóa tệp cài đặt

```bash
rm google-chrome-stable_current_amd64.deb
```

#### Bước 4: Sử dụng Google Chrome

Bạn có thể khởi động chrome theo hai cách:

- Nhấp vào **Google Chrome** trong menu Applications
- **Hoặc**, dùng lệnh `google-chrome` trong terminal

  ```bash
  google-chrome
  ```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Google Chrome dưới dạng tiến trình nền

Chrome sẽ sử dụng terminal này để xuất nhiều thông báo khác nhau và không cho bạn chạy các lệnh khác. Bạn có thể bỏ qua những thông báo đó. Nếu muốn dùng chính terminal đã khởi chạy Chrome để chạy các lệnh khác, hãy dùng `google-chrome &` thay thế.

</div>

[Quay lại bài học Cài đặt](https://www.theodinproject.com/lessons/foundations-installations#google-chrome-installation)