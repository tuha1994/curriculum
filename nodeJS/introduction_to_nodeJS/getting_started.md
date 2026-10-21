### Giới thiệu

Như đã học trong bài giới thiệu, Node.js thực chất chỉ là JavaScript. Vì vậy, để hiểu Node, bạn cần có kiến thức nền tảng về JavaScript. Do đó, chúng tôi đặc biệt khuyến nghị bạn hoàn thành [khóa học JavaScript](https://www.theodinproject.com/paths/full-stack-javascript/courses/javascript) tiên quyết trước khi tiếp tục khóa học này.

Bài học này sẽ hướng dẫn bạn qua một tutorial về các module và hàm cơ bản cần thiết để bắt đầu với Node.js. Project ở cuối phần này yêu cầu bạn dùng Node để tạo một website cơ bản gồm các trang `Index`, `About` và `Contact Me`. Vì vậy, trong lúc học các chủ đề của bài, hãy chú ý đến những điều có thể giúp bạn hoàn thành project.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích một số việc Node.js thường được dùng để làm.
- Tạo và sử dụng module trong Node.js (cả module tích hợp sẵn lẫn module do người dùng tạo).
- Thiết lập webserver cơ bản bằng Node.js với module HTTP.
- Đọc, tạo, cập nhật và xóa tệp bằng Node.js.
- Dùng module URL để phân tích địa chỉ url và tách thành các phần dễ đọc.
- Hiểu cách sử dụng NPM.
- Tạo, phát và lắng nghe các event của riêng bạn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Lưu ý quan trọng

Gần đây, nhóm NodeJS.dev đã gỡ bỏ một lượng lớn nội dung khỏi website. Một số trang bị gỡ đã được liên kết trong bài học này. Cho đến khi tìm được nội dung thay thế, chúng tôi sẽ liên kết trực tiếp đến các tệp markdown trong repository GitHub của họ. Định dạng có thể trông hơi lạ, nhưng nội dung vẫn phải hữu ích tương đương.

</div>

Hãy bắt đầu tìm hiểu code phía máy chủ Node! Chúng ta sẽ lần lượt xem các bài trong tài liệu [NodeJS.org](https://nodejs.org/en/learn); bạn nên học theo các bài đó.

1. Học [cách chạy script Node.js từ terminal](https://nodejs.org/en/learn/command-line/run-nodejs-scripts-from-the-command-line).
1. HTTP Module
   - Học [cách thực hiện HTTP request bằng Node](https://nodejs.org/learn/getting-started/fetch).
   - Xem tài liệu về [module http của Node](https://nodejs.org/api/http.html). Đặc biệt, hãy xem `http.createServer`. Tài liệu cho biết các tham số tùy chọn mà method này có thể nhận, bạn có thể sẽ dùng hoặc không. Hiện tại, bạn chỉ cần biết method `createServer` tạo một HTTP server nhận các handler được thực thi mỗi khi có request.
1. File System
   - Trước hết, xem [module fs](https://github.com/nodejs/nodejs.dev/blob/aa4239e87a5adc992fdb709c20aebb5f6da77f86/content/learn/node-js-modules/node-module-fs.en.md), module được dùng nhiều để làm việc với tệp trong Node.
   - Sau đó, bắt đầu [ghi tệp bằng Node](https://nodejs.org/en/learn/manipulating-files/writing-files-with-nodejs).
   - Cuối cùng, học [đọc tệp bằng Node](https://nodejs.org/en/learn/manipulating-files/reading-files-with-nodejs).
1. The URL Class
   - Xem [tài liệu về lớp URL](https://nodejs.org/api/url.html#url_the_whatwg_url_api). Hãy thử các mẫu code để xem cách hoạt động!
1. Events
   - Học theo phần [Event Emitter](https://nodejs.org/en/learn/asynchronous-work/the-nodejs-event-emitter).
   - Xem [module events của Node](https://github.com/nodejs/nodejs.dev/blob/aa4239e87a5adc992fdb709c20aebb5f6da77f86/content/learn/node-js-modules/node-module-events.en.md).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ sung.

- [Playlist crash course Node gồm 12 tập](https://www.youtube.com/watch?v=zb3Qk8SG5Ms&list=PL4cUxeGkcC9jsz4LDYc6kv3ymONOKxwBU) của Net Ninja là tài nguyên tuyệt vời để học Node.js. Playlist có 12 video; bạn có thể xem hết.