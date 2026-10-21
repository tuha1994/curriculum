### Hướng dẫn: Cài đặt PostgreSQL trên Linux

#### Bước 1: Đảm bảo hệ thống đã được cập nhật

Trước khi cài đặt PostgreSQL, bạn nên đảm bảo hệ điều hành được cập nhật. Để cập nhật hệ thống của chúng tôi, hãy chạy lệnh này:

```bash
sudo apt update && sudo apt upgrade
```

#### Bước 2: Cài đặt gói PostgreSQL

Sau khi hệ thống của chúng tôi được cập nhật, chúng tôi sẽ cài đặt các gói cho PostgreSQL.

```bash
sudo apt install postgresql postgresql-contrib libpq-dev
```

Sau khi cài đặt hoàn tất, hãy khởi động máy chủ bằng lệnh này:

```bash
sudo systemctl start postgresql.service && systemctl status postgresql.service
```

Có lỗi hoặc không thấy dịch vụ đang hoạt động? Hãy đến thăm [máy chủ Discord](https://discord.gg/V75WSQG) của chúng tôi để được trợ giúp!

Nếu `postgresql` đang hoạt động, bạn có thể nhấn `Q` để thoát khỏi màn hình trạng thái và chuyển sang bước tiếp theo.

#### Bước 3: Thiết lập PostgreSQL

PostgreSQL hiện đang chạy, nhưng chúng tôi phải định cấu hình nó để có thể sử dụng nó với các ứng dụng Express cục bộ của mình.

PostgreSQL xác thực thông qua các vai trò. Vai trò giống như người dùng, đó là cách chúng ta tương tác với dịch vụ. Cài đặt PostgreSQL mặc định đã thiết lập vai trò `postgres` mà chúng ta có thể sử dụng. Điều này thật tuyệt, nhưng điều đó có nghĩa là phải chuyển sang vai trò đó mỗi khi chúng ta muốn làm điều gì đó với máy chủ cơ sở dữ liệu.

Thay vào đó, chúng tôi sẽ thiết lập vai trò của riêng mình để tránh phải chuyển sang vai trò `postgres` liên tục.

1. Chúng tôi sẽ tạo một vai trò mới có cùng tên với tên người dùng Linux của chúng tôi. Nếu không chắc chắn về tên người dùng Linux của mình, bạn có thể chạy lệnh `whoami` trong thiết bị đầu cuối của mình để lấy nó. Khi bạn đã có sẵn thông tin đó, hãy tạo một vai trò trong PostgreSQL. Lệnh để làm như vậy là:

   ```bash
   sudo -i -u postgres createuser --interactive
   ```

   Hãy nhớ rằng chúng tôi muốn tên vai trò giống với tên người dùng Linux của chúng tôi và đảm bảo đặt vai trò mới đó thành siêu người dùng. Thiết lập một vai trò như thế này có nghĩa là chúng ta có thể tận dụng "xác thực ngang hàng" khiến việc sử dụng cơ sở dữ liệu cục bộ trở nên rất dễ dàng.

1. Một bước quan trọng khác trong việc thiết lập PostgreSQL là mỗi vai trò phải có cơ sở dữ liệu riêng cùng tên. Nếu không có nó, vai trò chúng ta vừa tạo sẽ không thể đăng nhập hoặc tương tác với PostgreSQL.

   Bạn có thể thử chạy `psql` ngay bây giờ, nhưng bạn sẽ gặp lỗi cho biết cơ sở dữ liệu không tồn tại. Đừng lo lắng, hãy tạo cơ sở dữ liệu đó để khắc phục vấn đề. Lưu ý rằng nếu tên người dùng có chứa chữ in hoa, bạn **phải** đặt tên người dùng trong dấu ngoặc kép khi chạy lệnh bên dưới.

   ```bash
   sudo -i -u postgres createdb <linux_username>
   ```

   Bây giờ vai trò của chúng tôi đã được thiết lập đầy đủ: chúng tôi có `<role_name>` và vai trò đó có cơ sở dữ liệu.

1. Một điều quan trọng mà chúng ta phải làm là thiết lập mật khẩu cho vai trò mới của mình để dữ liệu được bảo vệ. Bây giờ vai trò của chúng ta đã được thiết lập, chúng ta thực sự có thể sử dụng nó để quản trị PostgreSQL. Tất cả những gì bạn phải làm là nhập lệnh này để vào dấu nhắc PostgreSQL:

   ```bash
   psql
   ```

   Bạn sẽ thấy lời nhắc PostgreSQL xuất hiện với vai trò mới mà chúng ta vừa tạo, như sau:

   ```sql
   role_name=#
   ```

   Nếu bạn không thấy lời nhắc tương tự, hãy vào [máy chủ Discord](https://discord.gg/V75WSQG) của chúng tôi để được trợ giúp. Nếu bạn **thấy** lời nhắc tương tự, chúng ta có thể tạo mật khẩu cho role như sau:

   ```sql
   \password <role_name>
   ```

   Bạn sẽ được nhắc nhập mật khẩu và xác minh nó. Khi bạn thực hiện xong, lời nhắc sẽ trở lại bình thường.

1. Bây giờ, chúng tôi sẽ định cấu hình các quyền cho vai trò mới của mình (lưu ý dấu chấm phẩy ở cuối):

   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <role_database_name> TO <role_name>;
   ```

   Hãy nhớ rằng bạn nên thay đổi `<role_database_name>` và `<role_name>` (cả hai đều phải giống nhau)! Nếu bạn thấy `GRANT` phản hồi lệnh thì bạn có thể nhập `\q` để thoát khỏi lời nhắc.

1. Sau khi cấu hình xong, bước cuối cùng là lưu nó vào môi trường để truy cập sau.

   Để lưu mật khẩu của chúng tôi vào môi trường, chúng tôi có thể chạy lệnh này:

   ```bash
   echo 'export DATABASE_PASSWORD="<role_password>"' >> ~/.bashrc
   ```

   Lưu ý ở đây tên chúng tôi đã chọn cho biến môi trường của mình: `DATABASE_PASSWORD`. Ngoài ra, hãy nhớ cập nhật `<role_password>` trong lệnh thành những gì đã được đặt ở trên!

   Bây giờ, biến này tồn tại trong môi trường của chúng ta để chúng ta sử dụng. Vì biến này là biến mới nên chúng tôi sẽ muốn tải lại môi trường để có thể truy cập vào nó. Để tải lại môi trường, bạn có thể đóng và mở lại thiết bị đầu cuối của mình.

Khi đã xong, chúng ta có thể chuyển sang thử nghiệm nó!

[Quay lại bài Cài đặt PostgreSQL](https://www.theodinproject.com/lessons/nodejs-installing-postgresql#assignment)
