### Hướng dẫn: Cài đặt PostgreSQL trên macOS

#### Bước 1: Đảm bảo hệ thống đã được cập nhật

Trước khi chạy các lệnh với homebrew, bạn cần đảm bảo mọi thứ đều được cập nhật. Chạy lần lượt các lệnh sau:

```bash
brew update
brew upgrade
```

Nếu thiết bị đầu cuối của bạn không nhận ra `brew` thì bạn sẽ cần phải cài đặt homebrew. Bạn có thể tìm thấy nó và các bản cài đặt khác trong [phụ lục cài đặt](https://www.theodinproject.com/guides/installations).

#### Bước 2: Cài đặt PostgreSQL qua PostgresApp

Cài đặt PostgreSQL qua Postgres.app rất đơn giản. Truy cập [Postgres.app](https://postgresapp.com/) và làm theo hướng dẫn đã nêu. Điều quan trọng là bạn sẽ muốn định cấu hình `$PATH` của mình để có thể truy cập công cụ đi kèm với PostgreSQL. Sau khi cài đặt Postgres.app, chúng ta có thể cài đặt máy chủ PostgreSQL. Chúng tôi muốn cài đặt phiên bản PostgreSQL mới nhất hiện có.

Sau khi cài đặt PSQL qua Postgres.app, bạn có thể chạy lệnh này để cập nhật đường dẫn truy cập tất cả công cụ:

```bash
sudo mkdir -p /etc/paths.d &&
echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```

Sau khi chạy lệnh này và khởi động lại thiết bị đầu cuối của mình, bạn có thể chạy `which psql` và chúng tôi mong đợi kết quả này:

```bash
/Applications/Postgres.app/Contents/Versions/latest/bin/psql
```

Nếu bạn không thấy kết quả tương tự, hãy truy cập [máy chủ Discord](https://discord.gg/fbFCkYabZB) của chúng tôi để được trợ giúp!

<div class="lesson-note lesson-note--warning" markdown="1">

#### Không có cấu hình bảo mật

Xin lưu ý rằng Postgres.app cài đặt máy chủ mà không có bất kỳ cấu hình bảo mật nào và sẽ chấp nhận tất cả các kết nối cục bộ mà không cần mật khẩu. Điều này tốt cho môi trường phát triển của chúng tôi, nhưng hãy lưu ý rằng khi bạn chuyển mọi thứ sang sản xuất, đây sẽ không phải là một chiến lược tốt.

</div>

Postgres.app mặc định tạo vai trò và cơ sở dữ liệu người dùng phù hợp với người dùng macOS của bạn. Bạn có thể xác nhận điều này bằng cách chạy lệnh này sau khi cài đặt Postgres.app và cập nhật đường dẫn:

```bash
psql
```

Bạn sẽ thấy lời nhắc PostgreSQL xuất hiện như thế này:

```bash
<your_user>=#
```

Vì Postgres.app đã định cấu hình xác thực đáng tin cậy cho tất cả các kết nối cục bộ nên chúng tôi không cần thiết lập bất kỳ xác thực mật khẩu nào cho môi trường phát triển cục bộ của mình. Chỉ cần nhớ rằng `role_name` của bạn là người dùng xuất hiện trong lời nhắc `psql` ở trên. Bạn có thể gõ `\q` để thoát.

[Quay lại bài Cài đặt PostgreSQL](https://www.theodinproject.com/lessons/nodejs-installing-postgresql#assignment)
