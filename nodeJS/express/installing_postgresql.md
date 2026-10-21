### Giới thiệu

Tính bền vững của dữ liệu là không thể thiếu đối với phần lớn các ứng dụng web. Tính bền vững của dữ liệu đạt được thông qua cơ sở dữ liệu. Có thể hiểu cách cấu trúc, xây dựng và truy vấn cơ sở dữ liệu của riêng bạn là những kỹ năng quan trọng mà bất kỳ nhà phát triển full-stack nào cũng phải có.

Ví dụ, hãy xem xét Dự án Odin. Tên người dùng của bạn được lưu trữ ở đâu? Hồ sơ dự án được lưu trữ ở đâu? Nội dung bài học này, bài bạn đang đọc, được lưu trữ ở đâu? Vâng, một cơ sở dữ liệu!

Chúng tôi đã chọn [PostgreSQL](https://www.postgresql.org/) làm cơ sở dữ liệu mà chúng tôi lựa chọn cho chương trình giảng dạy. Tất cả kiến ​​thức SQL mà bạn đã học trong khóa học trước đó đều có thể chuyển sang PostgreSQL. Chúng ta cũng sẽ tìm hiểu những điều cụ thể mới của PostgreSQL. Hãy xem qua [video giới thiệu ngắn gọn và thú vị về PostgreSQL của Fireship](https://www.youtube.com/watch?v=n2Fluyr3lbc).

Trong bài học này, chúng ta sẽ cài đặt PostgreSQL và trong bài học tiếp theo, chúng ta sẽ tìm hiểu cách sử dụng nó trong ứng dụng Express của mình.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Vỏ PostgreSQL.
- Cách cài đặt PostgreSQL.

### Vỏ PostgreSQL

<div class="lesson-note lesson-note--critical" markdown="1">

#### Hoàn thành khóa học SQL

Đảm bảo bạn đã hoàn thành [khóa học SQL](https://www.theodinproject.com/paths/full-stack-javascript/courses/databases).

Bài học này và tất cả các bài học tiếp theo sẽ giả sử bạn hiểu cú pháp và khái niệm SQL.

</div>

Shell PostgreSQL, còn được gọi là psql, là giao diện người dùng dựa trên thiết bị đầu cuối của PostgreSQL cho phép người dùng thực hiện các truy vấn SQL và quản lý cơ sở dữ liệu một cách tương tác.

Nó thường được bao gồm trong quá trình cài đặt PostgreSQL và có thể được truy cập thông qua dòng lệnh. Phạm vi của nó bao gồm chạy truy vấn, tạo và sửa đổi cấu trúc cơ sở dữ liệu cũng như tương tác với cơ sở dữ liệu trong môi trường dòng lệnh, psql cung cấp một số lệnh meta và các tính năng giống shell khác nhau để tạo điều kiện thuận lợi cho việc viết tập lệnh và tự động hóa nhiều tác vụ khác nhau.

Cơ sở dữ liệu sử dụng thiết bị đầu cuối psql được đặt bên trong hệ thống tệp hoặc máy cục bộ, vì PostgreSQL lưu trữ cơ sở dữ liệu trong các tệp thông thường trong hệ thống tệp. Vị trí mặc định của thư mục dữ liệu có thể khác nhau giữa các hệ điều hành khác nhau.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Tùy thuộc vào hệ điều hành bạn đang chạy, hãy làm theo hướng dẫn cài đặt PostgreSQL thích hợp bên dưới:
   - [Linux](https://github.com/TheOdinProject/curriculum/tree/main/nodeJS/express/installation_guides/postgresql/linux.md "PostgreSQL installation for Linux")
   - [macOS](https://github.com/TheOdinProject/curriculum/tree/main/nodeJS/express/installation_guides/postgresql/macos.md "PostgreSQL installation for macOS")

</div>
