### Hướng dẫn: Cài đặt PostgreSQL trên Linux

#### Bước 1: Đảm bảo hệ thống được cập nhật

Trước khi cài đặt PostgreSQL, bạn nên đảm bảo hệ điều hành đã được cập nhật. Để cập nhật hệ thống, hãy chạy lệnh sau:

```bash
sudo apt update && sudo apt upgrade
```

#### Bước 2: Cài đặt các gói PostgreSQL

Sau khi hệ thống được cập nhật, chúng ta sẽ cài đặt các gói cho PostgreSQL.

```bash
sudo apt install postgresql postgresql-contrib libpq-dev
```

Sau khi cài đặt hoàn tất, hãy khởi động máy chủ bằng lệnh sau:

```bash
sudo systemctl start postgresql.service && systemctl status postgresql.service
```

Bạn gặp lỗi hoặc không thấy dịch vụ đang hoạt động? Hãy ghé [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để được trợ giúp!

Nếu `postgresql` đang hoạt động, bạn có thể nhấn `Q` để thoát màn hình trạng thái và chuyển sang bước tiếp theo.

#### Bước 3: Thiết lập PostgreSQL

PostgreSQL hiện đang chạy, nhưng chúng ta phải cấu hình nó để có thể sử dụng với các ứng dụng Rails cục bộ.

PostgreSQL xác thực thông qua các role. Role giống như một người dùng; đây là cách chúng ta tương tác với dịch vụ. Bản cài đặt PostgreSQL mặc định đã thiết lập một role `postgres` mà chúng ta có thể sử dụng. Điều này rất tiện, nhưng như vậy chúng ta sẽ phải chuyển sang role đó mỗi khi muốn thực hiện thao tác với máy chủ cơ sở dữ liệu.

Thay vào đó, chúng ta sẽ thiết lập role riêng để không phải liên tục chuyển sang role `postgres`.

1. Chúng ta sẽ tạo một role mới có cùng tên với tên người dùng Linux của mình. Nếu không chắc tên người dùng Linux là gì, bạn có thể chạy lệnh `whoami` trong terminal để kiểm tra. Sau khi đã có thông tin đó, hãy tạo role trong PostgreSQL. Lệnh thực hiện việc này là:

   ```bash
   sudo -i -u postgres createuser --interactive
   ```

   Hãy nhớ đặt tên role giống với tên người dùng Linux và nhớ cấp cho role mới đó quyền superuser. Thiết lập role theo cách này cho phép chúng ta tận dụng cơ chế "peer authentication", nhờ đó việc sử dụng cơ sở dữ liệu cục bộ trở nên rất dễ dàng.

1. Một bước quan trọng khác khi thiết lập PostgreSQL là mỗi role phải có một cơ sở dữ liệu riêng cùng tên. Nếu không có cơ sở dữ liệu đó, role vừa tạo sẽ không thể đăng nhập hoặc tương tác với PostgreSQL.

   Bạn có thể thử chạy `psql` ngay bây giờ, nhưng sẽ nhận được lỗi cho biết cơ sở dữ liệu không tồn tại. Đừng lo, hãy tạo cơ sở dữ liệu để giải quyết vấn đề này. Lưu ý rằng nếu đặt tên người dùng có chứa chữ hoa, bạn **phải** đặt tên người dùng trong dấu ngoặc kép khi chạy lệnh bên dưới.

   ```bash
   sudo -i -u postgres createdb <linux_username>
   ```

   Bây giờ role của chúng ta đã được thiết lập đầy đủ: chúng ta có `<role_name>` và role đó có một cơ sở dữ liệu.

1. Một việc quan trọng cần làm là đặt mật khẩu cho role mới để bảo vệ dữ liệu. Khi role đã được thiết lập, chúng ta có thể dùng chính role đó để quản trị PostgreSQL. Bạn chỉ cần nhập lệnh sau để mở dấu nhắc PostgreSQL:

   ```bash
   psql
   ```

   Bạn sẽ thấy dấu nhắc PostgreSQL xuất hiện với role mới tạo, như sau:

   ```sql
   role_name=#
   ```

   Nếu không thấy dấu nhắc tương tự, hãy liên hệ [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để được trợ giúp. Nếu bạn **có** thấy dấu nhắc tương tự, chúng ta có thể tạo mật khẩu cho role như sau:

   ```sql
   \password <role_name>
   ```

   Bạn sẽ được yêu cầu nhập mật khẩu rồi xác nhận lại. Khi hoàn tất, dấu nhắc sẽ trở về trạng thái bình thường.

1. Bây giờ, chúng ta sẽ cấu hình quyền cho role mới (hãy lưu ý dấu chấm phẩy ở cuối lệnh):

   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <role_database_name> TO <role_name>;
   ```

   Hãy nhớ thay đổi `<role_database_name>` và `<role_name>` (hai giá trị này phải giống nhau)! Nếu thấy phản hồi `GRANT` sau khi chạy lệnh, bạn có thể nhập `\q` để thoát dấu nhắc.

1. Sau khi hoàn tất cấu hình, bước cuối cùng là lưu mật khẩu vào môi trường để có thể truy cập sau này.

   Để lưu mật khẩu vào môi trường, chúng ta có thể chạy lệnh sau:

   ```bash
   echo 'export DATABASE_PASSWORD="<role_password>"' >> ~/.bashrc
   ```

   Hãy chú ý tên biến môi trường mà chúng ta đã chọn: `DATABASE_PASSWORD`. Đồng thời, nhớ cập nhật `<role_password>` trong lệnh bằng mật khẩu đã đặt ở trên!

   Bây giờ biến này đã tồn tại trong môi trường để chúng ta sử dụng. Vì đây là biến mới, chúng ta cần tải lại môi trường để có thể truy cập nó. Để tải lại môi trường, bạn có thể đóng rồi mở lại terminal.

Sau khi hoàn tất, chúng ta có thể chuyển sang kiểm tra thử!

[Quay lại bài học Cài đặt PostgreSQL](https://www.theodinproject.com/lessons/ruby-on-rails-installing-postgresql#assignment)