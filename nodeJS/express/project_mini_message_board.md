### Giới thiệu

Tại thời điểm này, bạn nên biết cách sử dụng Express để tạo một số ứng dụng web tương tác thú vị! Chúng ta sẽ tạo một bảng tin.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Thiết lập ứng dụng Express cơ bản bằng cách cài đặt Express và EJS. Thiết lập lộ trình chỉ mục cơ bản và chạy máy chủ của bạn. Tạo các thư mục và tập tin cần thiết như đã thảo luận trong các bài học trước.
1. Chúng ta sẽ có 2 tuyến, chỉ mục (`"/"`) và biểu mẫu "tin nhắn mới" (`"/new"`).
1. Tạo một mảng ở đầu bộ định tuyến chỉ mục của bạn có tên là `messages` và đặt một vài thông báo mẫu bên trong mảng đó như thế này:

   ```javascript
   const messages = [
     {
       text: "Hi there!",
       user: "Amando",
       added: new Date()
     },
     {
       text: "Hello World!",
       user: "Charles",
       added: new Date()
     }
   ];
   ```

1. Tiếp theo, trong mẫu chỉ mục của bạn (trong thư mục `"views"`), lặp qua mảng tin nhắn và đối với mỗi mảng, hãy hiển thị người dùng, văn bản và ngày tin nhắn được thêm vào. Đừng quên cung cấp tin nhắn của bạn cho mẫu của bạn bằng cách đưa nó vào đối tượng 'địa phương' `res.render` (ví dụ: `res.render("index", { title: "Mini Messageboard", messages: messages })`).
1. Tiếp theo hãy thiết lập mẫu tin nhắn mới.  Trong bộ định tuyến, thêm `router.get()` cho tuyến `"/new"` và trỏ nó vào mẫu có tên `"form"`. Trong thư mục dạng xem, hãy tạo mẫu `form` của bạn. Thêm tiêu đề, 2 đầu vào (một cho tên tác giả và một cho nội dung tin nhắn) và nút gửi. Để yêu cầu biểu mẫu thực hiện một yêu cầu mạng, bạn sẽ cần xác định nó bằng cả một phương thức và một hành động như vậy (chúng ta sẽ tìm hiểu cách xử lý các biểu mẫu trong bài học sau):

   ```html
   <form method="POST" action="/new">
      put your inputs and buttons in here!
   </form>
   ```

1. Với biểu mẫu của bạn được thiết lập như thế này, khi bạn nhấp vào nút gửi, nó sẽ gửi yêu cầu POST tới url được chỉ định bởi thuộc tính hành động, vì vậy hãy quay lại bộ định tuyến chỉ mục của bạn và thêm `router.post()` cho `"/new"`.
1. Để nhận và sử dụng dữ liệu từ biểu mẫu của bạn, bạn sẽ cần truy cập nội dung của biểu mẫu bên trong `router.post()` dưới dạng đối tượng có tên `req.body`. Các trường riêng lẻ bên trong đối tượng nội dung được đặt tên theo thuộc tính `name` trên đầu vào của bạn (giá trị của `<input name="messageText">` sẽ hiển thị dưới dạng `req.body.messageText` bên trong hàm `router.post`). Để tính năng này hoạt động như dự định, bạn cần sử dụng phần mềm trung gian Express cấp ứng dụng có tên là `express.urlencoded()` để phân tích dữ liệu biểu mẫu thành `req.body`. Bạn có thể thiết lập điều này bằng cách thêm dòng sau vào thiết lập ứng dụng của mình:

   ```javascript
   app.use(express.urlencoded({ extended: true }));
   ```

1. Trong `router.post()` của bạn, lấy nội dung của việc gửi biểu mẫu và đẩy chúng vào mảng tin nhắn dưới dạng một đối tượng trông giống như thế này:

   ```javascript
   messages.push({ text: messageText, user: messageUser, added: new Date() });
   ```

1. Khi kết thúc chức năng `router.post()`, hãy sử dụng `res.redirect("/")` để đưa người dùng quay lại trang chỉ mục sau khi gửi tin nhắn mới.
1. Tại thời điểm này, bạn có thể truy cập `/new` (có thể là một ý tưởng hay nếu thêm liên kết đến tuyến đường đó trên trang chỉ mục của bạn), điền vào biểu mẫu, gửi nó và sau đó xem nó hiển thị trên trang chỉ mục!
1. Thêm nút "mở" hoặc liên kết bên cạnh mỗi tin nhắn để mở một trang mới có thông tin chi tiết về tin nhắn.
1. Đẩy dự án của bạn lên GitHub.
1. Bạn sẽ tìm hiểu cách triển khai ứng dụng của mình lên web trong bài học tiếp theo, đừng quên quay lại và gửi ứng dụng đó cho các bài gửi bên dưới sau khi bạn hoàn tất!

</div>
