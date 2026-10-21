### Giới thiệu

Trong phần trước, bạn đã bắt đầu và chạy với Node. Bạn đã học cách đọc và ghi dữ liệu từ các tệp trên máy chủ, cho phép bạn phục vụ một trang web nhiều trang. Bạn có thể thấy quy trình này hơi dài dòng và rườm rà, thậm chí có thể tự hỏi làm cách nào chúng ta có thể quản lý các trường hợp sử dụng phức tạp hơn.

Trong khóa học này, chúng tôi sẽ sử dụng khung phụ trợ có tên [Express](https://expressjs.com/), khung này sẽ xử lý nhiều chi tiết triển khai cho chúng tôi. Bản thân Express là một khuôn khổ có chủ ý và không có ý kiến; nó cho phép chúng tôi làm nhiều việc theo cách chúng tôi muốn và chỉ mở rộng nó với những tính năng chúng tôi cần. Tuy nhiên, mặc dù điều này mang lại cho chúng tôi sự linh hoạt cao trong cách thực hiện mọi việc nhưng đôi khi có thể hơi khó khăn khi quyết định giữa nhiều lựa chọn khả thi.

Trong thời gian tới, chúng ta sẽ tìm hiểu sâu hơn về nhiều cách sử dụng Express, chẳng hạn như tạo ứng dụng full-stack theo mẫu Model View Controller (MVC), cũng như tạo REST API tương tự những API bạn từng dùng trong các dự án Weather App hoặc React Shopping Cart. Có rất nhiều nội dung cần tiếp thu, vì vậy hãy học từ tốn và đừng ngại nhờ cộng đồng trợ giúp trong [máy chủ Discord của TOP](https://discord.gg/fbFCkYabZB)!

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thiết lập máy chủ Express cơ bản.
- Mô tả cách xử lý một yêu cầu đến và cuối cùng được máy chủ phản hồi.
- Mô tả chức năng của phần mềm trung gian là gì.
- Mô tả cách tự động khởi động lại ứng dụng của bạn khi có thay đổi.

### Thiết lập Express

Hãy tạo một ứng dụng Express cơ bản. Trong một thư mục mới, hãy bắt đầu bằng cách chạy `npm init -y` để tạo `package.json`. Khi đã tạo xong, chúng ta có thể cài đặt phần phụ thuộc Express.

```bash
npm install express
```

Bây giờ chúng ta có thể tạo tệp `app.js` sẽ đóng vai trò là điểm khởi đầu cho máy chủ Express của chúng ta. Chúng tôi có thể đặt tên tệp này theo bất kỳ tên nào chúng tôi muốn, chẳng hạn như `index.js` hoặc `main.js`, nhưng chúng tôi sẽ sử dụng cùng tên được sử dụng trong tài liệu Express. Bên trong `app.js`, chúng ta sẽ có những thứ sau:

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => res.send("Hello, world!"));

const PORT = 3000;
app.listen(PORT, (error) => {
  // This is important!
  // Without this, any startup errors will silently fail
  // instead of giving you a helpful error message.
  if (error) {
    throw error;
  }
  console.log(`My first Express app - listening on port ${PORT}!`);
});
```

Hãy phá vỡ điều này. Chúng ta import `express` sau đó gọi nó để khởi tạo biến `app`. Đây là máy chủ của chúng tôi.

Sau đó chúng ta có `route` - dòng bắt đầu bằng `app.get`. Chúng ta sẽ quay lại vấn đề này trong giây lát.

Cuối cùng, chúng tôi yêu cầu máy chủ lắng nghe các yêu cầu đến trên bất kỳ cổng nào chúng tôi chỉ định, thông qua [localhost](https://simple.wikipedia.org/wiki/Localhost) (về cơ bản chỉ là kết nối cục bộ của máy tính). Mặc dù cổng 3000 là lựa chọn mặc định nhưng bạn có thể sử dụng bất kỳ cổng nào không được sử dụng (ví dụ: máy chủ phát triển của Vite sử dụng cổng 5173 theo mặc định). Quay lại thiết bị đầu cuối của bạn, nếu bạn chạy `node app.js` thì mọi thứ đều ổn, bạn sẽ thấy `My first Express app - listening on port 3000!` được ghi lại. Nếu có sự cố xảy ra, chẳng hạn như cổng đã được sử dụng, thay vào đó, nó sẽ báo lỗi.

Chúc mừng! Máy chủ Express đầu tiên của bạn hiện đang chạy.

<div class="lesson-note" markdown="1">

#### Biến cổng

Với mục đích trình diễn, chúng tôi đã mã hóa cứng một số cổng cố định ở trên. Thông thường, số cổng sẽ đến từ biến môi trường có giá trị dự phòng trong trường hợp biến môi trường không tồn tại.

```javascript
const PORT = process.env.PORT || 3000;
```

Nếu cổng được chỉ định đã được sử dụng, chúng ta có thể thay đổi giá trị biến môi trường mà không cần chỉnh sửa mã nguồn. Ngoài ra, một số dịch vụ lưu trữ định cấu hình cổng riêng của chúng, cổng này có thể khác với giá trị cố định được mã hóa cứng.

</div>

### Hành trình của một yêu cầu

Bây giờ máy chủ của chúng tôi đã hoạt động và chạy trên cổng 3000, hãy gửi yêu cầu cho nó! Trong trình duyệt, điều hướng đến `http://localhost:3000/` (đừng lo lắng nếu bạn quên dấu gạch chéo `/` ở cuối; trình duyệt sẽ âm thầm thêm nó cho bạn nếu vậy). Hành động này yêu cầu trình duyệt gửi yêu cầu `GET` tới đường dẫn `/` của bất kỳ máy chủ nào đang nghe ở cổng 3000 trên localhost của chúng tôi (là máy chủ Express của chúng tôi!) và hiển thị trong cửa sổ bất cứ điều gì nó nhận được phản hồi.

Bất cứ khi nào bạn điều hướng đến bất kỳ URL web nào theo cách này, đây thực chất là những gì bạn đang làm. Điều hướng đến `https://theodinproject.com/paths` qua thanh địa chỉ chỉ yêu cầu trình duyệt gửi yêu cầu `GET` đến đường dẫn `/paths` tại `https://theodinproject.com`, sau đó hiển thị những gì nó nhận được trong phản hồi.

Khi bạn điều hướng đến `http://localhost:3000/`, bạn sẽ thấy `Hello, world!` xuất hiện trong cửa sổ. Phép thuật, phải không?

Khi máy chủ của chúng tôi nhận được yêu cầu `GET` của chúng tôi, Express sẽ lưu yêu cầu đó trong [request object](https://expressjs.com/en/5x/api/request). Yêu cầu này được chuyển qua một chuỗi hàm mà chúng tôi gọi là `middleware functions` cho đến khi cuối cùng, một hàm phần mềm trung gian yêu cầu Express phản hồi yêu cầu.

Trong ví dụ của chúng tôi, yêu cầu được gửi dưới dạng yêu cầu `GET` tới đường dẫn `/`. Điều này khớp với tuyến đường chúng tôi có trong tệp `app.js`.

```javascript
app.get("/", (req, res) => res.send("Hello, world!"));
```

Chúng ta sẽ thảo luận về các tuyến đường chi tiết hơn trong bài học sau, nhưng để tóm tắt dòng trên, nó báo cho Express: "nếu một yêu cầu `GET` đi qua đường dẫn `/`, hãy chuyển yêu cầu đó qua chuỗi chức năng phần mềm trung gian sau đây". Ở đây, chúng tôi chỉ có một chức năng duy nhất.

Nếu chúng tôi đã xác định nhiều tuyến, Express sẽ chuyển yêu cầu qua tuyến đầu tiên khớp với động từ HTTP được yêu cầu (ví dụ: `GET`) và đường dẫn (`/`). Thứ tự của các tuyến đường rất quan trọng!

Express nhận chức năng gọi lại mà chúng tôi đã cung cấp và chuyển đối tượng yêu cầu vào tham số đầu tiên (thường được đặt tên là `req`) và [đối tượng phản hồi](https://expressjs.com/en/5x/api/response) vào tham số thứ hai (`res`). Cuộc gọi lại của chúng tôi yêu cầu đối tượng phản hồi phản hồi yêu cầu bằng cách gửi (thông qua `res.send`) chuỗi `"Hello, world!"`.

Không còn mã để chạy và hàm trả về. Vì Express đã được yêu cầu phản hồi yêu cầu nên nó sẽ kết thúc chu trình phản hồi yêu cầu. Trong khi đó, trình duyệt nhận được phản hồi từ máy chủ của chúng tôi và hiển thị nó trên màn hình, đó là chuỗi `"Hello, world!"` của chúng tôi. Chúng tôi có thể gửi gần như bất cứ điều gì trong phản hồi của chúng tôi. Chúng tôi thậm chí có thể [yêu cầu Express gửi tệp](https://expressjs.com/en/5x/api/response/#ressendfile).

### Tự động khởi động lại máy chủ của bạn khi thay đổi tệp

Khi bạn chạy máy chủ của mình bằng `node app.js`, mọi thay đổi đối với mọi tệp JavaScript và JSON trong thư mục dự án của bạn sẽ không được phản ánh tự động trừ khi bạn ngắt và chạy lại `node app.js` theo cách thủ công. Để tránh quy trình thủ công này, bạn có thể sử dụng [Chế độ đồng hồ của Node](https://nodejs.org/docs/latest-v20.x/api/cli.html#--watch) bằng cách thêm cờ `--watch`, ví dụ: `node --watch app.js`. Nút sẽ theo dõi `app.js` để biết các thay đổi cũng như bất kỳ tệp nào mà cuối cùng nó phụ thuộc vào. Khi phát hiện thay đổi, nó sẽ tự động khởi động lại máy chủ giống như với máy chủ dev của Webpack và Vite.

Bạn cũng có thể gặp [Nodemon](https://www.npmjs.com/package//nodemon), một gói có cấu hình cao, cũng có thể theo dõi các thay đổi và khởi động lại máy chủ cho bạn. Node không phải lúc nào cũng có chế độ đồng hồ tích hợp ổn định, vì vậy bạn có thể thấy Nodemon ở khắp mọi nơi. Khuyến nghị của chúng tôi là nên sử dụng chế độ đồng hồ tích hợp của Node thông qua cờ `--watch`, vì đây là phương pháp đơn giản nhất cho đến nay.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Dành vài phút khám phá [Tài liệu nhanh](https://expressjs.com/en/api.html) để cảm nhận mọi thứ. Chúng tôi sẽ tham khảo nhiều nội dung từ tài liệu trong các bài học sắp tới.
1. Quay lại [Dự án Trang thông tin cơ bản](https://www.theodinproject.com/lessons/nodejs-basic-informational-site) của bạn, cài đặt Express và viết lại dự án bằng cách sử dụng nó! Bạn có thể thực hiện hầu hết việc này chỉ với một vài `app.get()`.

</div>
