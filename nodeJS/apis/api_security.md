### Giới thiệu

Bảo mật API là một bước quan trọng. Khi dùng Express để phục vụ view template, chúng ta dùng PassportJS cùng username và password để xác thực người dùng. Tuy nhiên, đó không phải cách duy nhất để bảo mật một app Express, và trong bối cảnh API, thường nên dùng chiến lược khác. Mô hình session bằng username và password đã học trước đây dĩ nhiên vẫn hoạt động, dù trở nên phức tạp hơn một chút vì code front-end đã được tách khỏi back-end.

Một chiến lược khác là tạo và truyền một **token** bảo mật giữa code back-end và front-end. Cách này giúp bảo đảm username và password của người dùng không bị lộ, đồng thời cho phép ta đặt thời hạn cho session để tăng cường bảo mật. Ý tưởng cơ bản là khi người dùng đăng nhập app, một token bảo mật được tạo ra; sau đó, trong mọi request tiếp theo, token được truyền trong header của request object. Nhìn chung, quy trình khá đơn giản vì bạn đã quen dùng passport để xác thực người dùng.

Chiến lược này đặc biệt hữu ích với API nhưng cũng có thể dùng cho project view-template truyền thống. Điểm khác biệt chính là thay vì đặt và kiểm tra cookie, ta truyền một token đặc biệt trong header của request. Trong Authentication Tutorial trước đây, middleware Passport kiểm tra cookie được gửi đến rồi xác thực hoặc từ chối người dùng. Ở đây ta sẽ làm điều rất tương tự, nhưng thay cookie bằng token.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích sự khác nhau giữa xác thực bằng token và xác thực dựa trên session.
- Tìm hiểu về JSON Web Token.
- Đọc về authorization header và cách sử dụng nó.
- Nhận diện và giải thích các method dùng để ký và xác minh token.
- Viết middleware tùy chỉnh để xác minh token trên một route.
- Làm quen với việc token JWT hết hạn.
- Mở rộng triển khai PassportJS để dùng JSON Web Token.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Video này là tài nguyên tuyệt vời, giải thích mọi điều bạn cần biết về [tạo và xác minh JSON Web Token](https://www.youtube.com/watch?v=7nafaH9SddU).
1. Video này trình bày [những cách khác nhau JWT có thể hữu ích](https://www.youtube.com/watch?v=7Q17ubqLfaM).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ sung.

- Bài viết này là [hướng dẫn xác thực JWT bằng Node.js và Express](https://web.archive.org/web/20230207144457/https://laptrinhx.com/a-practical-guide-for-jwt-authentication-using-node-js-and-express-917791379/).
- Bài viết này là [hướng dẫn ngắn gọn hơn về cách dùng JWT trong Express](https://medium.com/@paul.allies/stateless-auth-with-express-passport-jwt-7a55ffae0a5c).
- [Không phải ai cũng đồng ý JWT là cách tốt nhất để lưu dữ liệu xác thực](https://www.youtube.com/watch?v=JdGOb7AxUo0). Video này trình bày một lập luận phản đối việc sử dụng JWT và một số cạm bẫy bạn có thể gặp phải khi dùng chúng.