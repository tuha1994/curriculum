### Hướng dẫn: Thiết lập Git trên ChromeOS

#### Bước 0: Điều kiện tiên quyết

Trước khi cài đặt Git, bạn cần bật Linux Development Environment trên Chromebook. Nếu chưa thiết lập, hãy làm theo [hướng dẫn bật Linux trên ChromeOS của Google](https://support.google.com/chromebook/answer/9145439).

#### Bước 1: Cập nhật hệ thống

Chạy các lệnh sau trong terminal để cập nhật hệ thống Linux:

```bash
sudo apt update
sudo apt upgrade
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Nhập mật khẩu trong terminal

Khi sử dụng một lệnh trong terminal yêu cầu bạn nhập mật khẩu để xác thực (chẳng hạn như sudo), bạn sẽ nhận thấy các ký tự không hiển thị khi nhập. Dù có vẻ terminal không phản hồi, bạn đừng lo!

Đây là một tính năng bảo mật nhằm bảo vệ thông tin bí mật, tương tự như cách các trường mật khẩu trên website sử dụng dấu hoa thị hoặc dấu chấm. Bằng cách không hiển thị các ký tự bạn nhập, terminal giữ cho mật khẩu của bạn được an toàn.

Bạn vẫn có thể nhập mật khẩu như bình thường rồi nhấn Enter để gửi mật khẩu.

</div>

#### Bước 2: Cài đặt Git

Bây giờ bạn có thể cài đặt Git bằng trình quản lý gói tiêu chuẩn:

```bash
sudo apt install git
```

#### Bước 3: Kiểm tra phiên bản Git

Đảm bảo phiên bản Git của bạn **ít nhất** là 2.28 bằng cách chạy lệnh sau:

```bash
git --version
```

Nếu số phiên bản nhỏ hơn 2.28, bạn có thể cần thêm Git PPA để có phiên bản mới hơn:

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
```

[Quay lại bài học Thiết lập Git](https://www.theodinproject.com/lessons/foundations-setting-up-git#assignment)