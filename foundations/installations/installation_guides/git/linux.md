### Hướng dẫn: Thiết lập Git trên Linux

#### Bước 1: Cập nhật hệ thống

Chạy các lệnh sau trong terminal để cập nhật hệ thống Linux:

```bash
sudo apt update
sudo apt upgrade
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Nhập mật khẩu trong terminal

Khi sử dụng một lệnh trong terminal yêu cầu bạn nhập mật khẩu để xác thực (chẳng hạn như `sudo`), bạn sẽ nhận thấy các ký tự không hiển thị khi nhập. Dù có vẻ như terminal không phản hồi, đừng lo!

Đây là một tính năng bảo mật để bảo vệ thông tin bí mật, tương tự như cách các trường mật khẩu trên website dùng dấu hoa thị hoặc dấu chấm. Không hiển thị các ký tự bạn nhập giúp terminal giữ mật khẩu của bạn an toàn.

Bạn vẫn có thể nhập mật khẩu như bình thường rồi nhấn <kbd>Enter</kbd> để gửi.

</div>

#### Bước 2: Cài đặt Git

Có thể Git đã được cài đặt trên máy bạn, nhưng để chắc chắn chúng ta có phiên bản mới nhất, hãy chạy các lệnh sau:

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
```

#### Bước 3: Kiểm tra phiên bản Git

Hãy đảm bảo phiên bản Git của bạn **ít nhất** là 2.28 bằng cách chạy lệnh sau:

```bash
git --version
```

Nếu số phiên bản nhỏ hơn 2.28, hãy làm lại hướng dẫn từ đầu.

[Quay lại bài học Thiết lập Git](https://www.theodinproject.com/lessons/foundations-setting-up-git#assignment)