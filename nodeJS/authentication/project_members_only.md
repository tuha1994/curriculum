### Giới thiệu

Trong dự án này, bạn sẽ xây dựng một câu lạc bộ độc quyền nơi các thành viên của bạn có thể viết những bài đăng ẩn danh. Bên trong hội quán, các thành viên có thể biết tác giả của bài đăng là ai, nhưng ở bên ngoài họ chỉ có thể xem câu chuyện và tự hỏi ai đã viết nó.

Đây sẽ là cơ hội để bạn sử dụng các kỹ năng xác thực mà chúng ta đã học ở bài trước. Bạn cũng sẽ thực hành các kỹ năng cơ sở dữ liệu của mình vì vậy hãy sẵn sàng!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bắt đầu bằng cách suy nghĩ về cách thiết lập các mô hình cơ sở dữ liệu mà bạn sẽ cần để hoàn thành mục tiêu của mình. Bạn sẽ cần người dùng có tên đầy đủ (họ và tên), tên người dùng (bạn có thể sử dụng email cho việc này), mật khẩu và trạng thái thành viên. Người dùng có thể tạo tin nhắn có tiêu đề, dấu thời gian và một số văn bản.  Cơ sở dữ liệu của bạn sẽ theo dõi xem ai đã tạo từng tin nhắn.
1. Thiết lập cơ sở dữ liệu trên PostgreSQL và tạo hoặc thiết lập bộ khung dự án, bao gồm các model bạn đã thiết kế ở bước trước.
1. Bắt đầu với biểu mẫu đăng ký để bạn có thể thu hút một số người dùng vào cơ sở dữ liệu của mình! Đừng quên làm sạch và xác thực các trường biểu mẫu cũng như bảo mật mật khẩu bằng `bcrypt`. Bạn nên thêm trường confirmPassword vào biểu mẫu đăng ký của mình rồi xác thực nó bằng [trình xác thực tùy chỉnh](https://express-validator.github.io/docs/guides/customizing).
1. Khi người dùng đăng ký, họ sẽ không tự động được cấp trạng thái thành viên! Câu lạc bộ tư nhân có gì thú vị nếu ai cũng có thể tham gia? Thêm một trang nơi các thành viên có thể "tham gia câu lạc bộ" bằng cách nhập mật mã bí mật. Nếu họ nhập đúng mật mã thì hãy cập nhật trạng thái thành viên của họ.
1. Tạo biểu mẫu đăng nhập bằng Passport.js giống như chúng ta đã làm trong bài tập trước.
1. Khi người dùng đăng nhập, hãy cung cấp cho họ liên kết đến "Tạo tin nhắn mới" (nhưng chỉ hiển thị nếu họ đã đăng nhập!).  Tạo mẫu tin nhắn mới.
1. Hiển thị tất cả tin nhắn của thành viên trên trang chủ, nhưng chỉ hiển thị tác giả và ngày tháng của tin nhắn cho các thành viên câu lạc bộ khác.
1. Thêm trường tùy chọn vào mô hình người dùng có tên Quản trị viên, sau đó thêm khả năng xóa tin nhắn, nhưng chỉ cho phép người dùng có `admin == true` nhìn thấy nút xóa và xóa tin nhắn.  Bạn sẽ cần thêm một cách để thực sự đánh dấu người dùng là 'quản trị viên', bằng cách thêm một trang mật mã bí mật khác hoặc chỉ cần đặt hộp kiểm "là quản trị viên" trên biểu mẫu đăng ký.
1. Đến thời điểm này, bất kỳ ai truy cập trang web đều có thể xem danh sách tất cả các tin nhắn, với tên tác giả bị ẩn. Người dùng có thể đăng ký và tạo tin nhắn, nhưng CHỈ người dùng là thành viên mới có thể xem tác giả và ngày của mỗi tin nhắn. Cuối cùng, bạn phải có một người dùng Quản trị viên có thể xem mọi thứ và cũng có khả năng xóa tin nhắn. Rõ ràng đây là một ứng dụng nhỏ ngớ ngẩn, nhưng những điều bạn đang thực hành (tạo và xác thực người dùng cũng như cấp cho người dùng các khả năng và quyền khác nhau) là những điều sẽ *rất* hữu ích cho bạn!
1. Khi bạn hài lòng với công việc của mình, hãy triển khai dự án trên PaaS đã chọn ([danh sách các nhà cung cấp PaaS từ bài học Triển khai](https://www.theodinproject.com/lessons/node-path-nodejs-deployment)) và chia sẻ dự án đó bên dưới!

</div>
