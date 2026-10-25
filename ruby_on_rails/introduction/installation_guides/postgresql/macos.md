### Hướng dẫn: Cài đặt PostgreSQL trên macOS

#### Bước 1: Đảm bảo hệ thống đã được cập nhật

Trước khi chạy các lệnh với homebrew, bạn nên đảm bảo mọi thứ đã được cập nhật. Hãy chạy lần lượt từng lệnh sau:

```bash
brew update
brew upgrade
```

Nếu terminal không nhận diện được `brew`, bạn sẽ cần cài đặt homebrew. Bạn có thể tìm homebrew và các trình cài đặt khác trong [phụ lục cài đặt](https://www.theodinproject.com/guides/installations).

#### Bước 2: Cài đặt PostgreSQL thông qua PostgresApp

Cài đặt PostgreSQL thông qua Postgres.app rất đơn giản. Hãy truy cập [Postgres.app](https://postgresapp.com/) và làm theo các hướng dẫn được cung cấp. Điều quan trọng là bạn cần cấu hình `$PATH` để có thể truy cập các công cụ đi kèm PostgreSQL. Sau khi cài đặt Postgres.app, chúng ta có thể cài đặt một máy chủ PostgreSQL. Chúng ta muốn cài đặt phiên bản PostgreSQL mới nhất hiện có.

Sau khi cài đặt PSQL thông qua Postgres.app, bạn có thể chạy lệnh này để cập nhật path nhằm truy cập toàn bộ công cụ:

```bash
sudo mkdir -p /etc/paths.d &&
echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```

Sau khi chạy lệnh này và khởi động lại terminal, bạn có thể chạy `which psql`; kết quả dự kiến sẽ là:

```bash
/Applications/Postgres.app/Contents/Versions/latest/bin/psql
```

Nếu không thấy kết quả tương tự, hãy ghé [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để được trợ giúp!

<div class="lesson-note lesson-note--warning" markdown="1">

#### Bảo mật của Postgres.app

Lưu ý rằng Postgres.app cài đặt các máy chủ mà không cấu hình bất kỳ biện pháp bảo mật nào và sẽ chấp nhận mọi kết nối cục bộ mà không cần mật khẩu. Điều này phù hợp với môi trường phát triển của chúng ta, nhưng hãy nhớ rằng đây sẽ không phải là một chiến lược tốt khi bạn đưa ứng dụng lên môi trường production.

</div>

Theo mặc định, Postgres.app tạo một role và cơ sở dữ liệu người dùng trùng với người dùng macOS của bạn. Bạn có thể xác nhận điều này bằng cách chạy lệnh sau sau khi cài đặt Postgres.app và cập nhật path:

```bash
psql
```

Bạn sẽ thấy lời nhắc PostgreSQL xuất hiện như sau:

```bash
<your_user>=#
```

Vì Postgres.app đã cấu hình cơ chế xác thực đáng tin cậy cho mọi kết nối cục bộ, chúng ta không cần thiết lập xác thực bằng mật khẩu cho môi trường phát triển cục bộ. Chỉ cần nhớ rằng `role_name` chính là người dùng xuất hiện trong lời nhắc `psql` ở trên. Bạn có thể nhập `\q` để thoát.

Sau khi hoàn tất, chúng ta có thể chuyển sang kiểm tra thử!

[Quay lại bài học Cài đặt PostgreSQL](https://www.theodinproject.com/lessons/ruby-on-rails-installing-postgresql#assignment)