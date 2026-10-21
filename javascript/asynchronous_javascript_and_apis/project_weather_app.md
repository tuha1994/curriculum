### Giới thiệu

Hãy sử dụng mọi thứ chúng ta đã thảo luận để tạo một trang dự báo thời tiết bằng API Visual Crossing từ các bài học trước. Bạn có thể tìm kiếm một địa điểm cụ thể và chuyển đổi cách hiển thị dữ liệu giữa Fahrenheit và Celsius.

Bạn nên thay đổi giao diện trang dựa trên dữ liệu, chẳng hạn đổi màu nền hoặc thêm hình ảnh mô tả thời tiết. (Bạn thậm chí có thể dùng API Giphy để tìm các GIF phù hợp với thời tiết và hiển thị chúng). Bạn có thể tự do dùng promise hoặc async/await trong mã, nhưng nên cố gắng làm quen với cả hai.

### API key, secret và bảo mật

Không phải API nào cũng miễn phí, và tùy cách thiết lập, chúng có thể tính phí theo mỗi lần sử dụng. Điều này khiến chúng trở thành mục tiêu hấp dẫn của những người muốn sử dụng API mà không trả tiền bằng cách dùng **API key của bạn**. API cũng có thể bị giới hạn tốc độ, và nếu ai đó có API key của bạn, họ có thể dùng hết số lượt sử dụng của bạn. Một cách để ngăn vấn đề này là lưu API key trên máy chủ và không bao giờ gửi chúng đến frontend ngay từ đầu; việc này thường được thực hiện bằng biến môi trường và khiến key chỉ có thể được sử dụng trên máy chủ nơi mã được triển khai.

Khi nói về API key và bảo mật, bạn thường nghe câu “Never trust the client” (client nghĩa là frontend). Thường thì câu này có nghĩa là không tin dữ liệu *đến từ* client là hợp lệ, nhưng nó cũng có nghĩa là bạn không thể tin bất cứ thứ gì chúng ta gửi *đến* client. Vì vậy, khi để lộ API key, GitHub sẽ cảnh báo rằng bạn đã commit công khai một API key. Điều này hoàn toàn ổn trong project này vì API key đó có sẵn công khai và việc để lộ nó không gây hậu quả. Điều này không có nghĩa là TẤT CẢ key đều như vậy. Trong các khóa học backend sau này, bạn sẽ học cách xử lý an toàn những chủ đề này.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Thiết lập một tài liệu HTML trống với các liên kết phù hợp đến các file JavaScript và CSS của bạn.
1. Viết các hàm gọi API. Bạn sẽ cần các hàm nhận một địa điểm và trả về dữ liệu thời tiết cho địa điểm đó. Trước mắt, chỉ cần `console.log()` thông tin.
1. Viết các hàm *xử lý* dữ liệu JSON nhận được từ API và trả về một đối tượng chỉ chứa dữ liệu ứng dụng cần.
1. Thiết lập một form cho phép người dùng nhập địa điểm và lấy thông tin thời tiết (vẫn chỉ `console.log()` thông tin đó).
1. Hiển thị thông tin trên trang web với một nút chuyển đổi để hiển thị dữ liệu nhiệt độ theo Fahrenheit hoặc Celsius.
   - Dù không bắt buộc, nếu muốn hiển thị biểu tượng thời tiết thì có thể có rất nhiều biểu tượng cần import, vì vậy hãy xem [dynamic imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import). Không giống template string thông thường không có import, [Webpack có thể đọc dynamic imports](https://webpack.js.org/api/module-methods/#dynamic-expressions-in-import) và vẫn bundle tất cả asset liên quan.
1. Thêm bất kỳ kiểu dáng nào bạn thích!
1. Tùy chọn: thêm một component “loading” hiển thị từ lúc form được gửi cho đến khi thông tin trở về từ API. Dùng DevTools để mô phỏng tốc độ mạng.
1. Đẩy project lên GitHub và chia sẻ solution của bạn bên dưới!

</div>