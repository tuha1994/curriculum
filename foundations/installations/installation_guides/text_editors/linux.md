### Hướng dẫn: Cài đặt VSCode trên Linux

#### Bước 1: Tải VSCode

Mở **terminal**, sau đó chạy lệnh sau để tải gói **VSCode** `.deb` mới nhất (`-O` là chữ "o" viết hoa, không phải số 0):

```bash
wget -O code-latest.deb 'https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64'
```

#### Bước 2: Cài đặt VSCode

Nhập các lệnh sau trong terminal để cài gói **VSCode** `.deb`:

```bash
sudo apt update
sudo apt upgrade
sudo apt install ./code-latest.deb
```

Sau đó, nếu được yêu cầu, hãy nhập mật khẩu của bạn.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Thêm kho VSCode của Microsoft (tùy chọn)

Nếu muốn VSCode tự động cập nhật cùng với các bản cập nhật hệ thống khác, bạn có thể thêm kho VSCode chính thức của Microsoft vào các nguồn apt. Việc này hoàn toàn ổn và an toàn. Kho này cho phép VSCode được cập nhật mỗi khi bạn chạy `sudo apt update && sudo apt upgrade` cùng các gói hệ thống khác.

</div>

<div class="lesson-note lesson-note--tip" markdown="1">

#### Nhập mật khẩu trong terminal

Khi dùng một lệnh trong terminal yêu cầu bạn nhập mật khẩu để xác thực (chẳng hạn như `sudo`), bạn sẽ thấy các ký tự không hiển thị khi gõ. Dù terminal có vẻ không phản hồi, đừng lo!

Đây là tính năng bảo mật nhằm bảo vệ thông tin bí mật, tương tự như cách các trường mật khẩu trên trang web dùng dấu hoa thị hoặc dấu chấm. Không hiển thị các ký tự bạn gõ giúp terminal giữ an toàn cho mật khẩu của bạn.

Bạn vẫn cứ nhập mật khẩu như bình thường rồi nhấn Enter để gửi.

</div>

<div class="lesson-note lesson-note--tip" markdown="1">

#### Lưu ý về việc tải xuống không có sandbox

Nếu thấy thông báo dạng `N: Download is performed unsandboxed (...)`, bạn không cần lo lắng vì [`N:` chỉ có nghĩa đây là một thông báo chứ không phải cảnh báo](https://www.reddit.com/r/linux4noobs/comments/ux6cwx/comment/i9x2twx/).

</div>

#### Bước 3: Xóa tệp cài đặt

Chạy lệnh sau:

```bash
rm code-latest.deb
```

#### Bước 4: Sử dụng VSCode

Bạn có thể khởi động VSCode theo một trong hai cách:

- Nhấp **Visual Studio Code** trong menu Applications
- Trong terminal, chạy lệnh sau:

  ```bash
  code
  ```

[Quay lại bài học Trình soạn thảo văn bản](https://www.theodinproject.com/lessons/foundations-text-editors#assignment)