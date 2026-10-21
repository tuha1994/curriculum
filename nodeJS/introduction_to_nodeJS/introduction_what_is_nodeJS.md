### Giới thiệu

NodeJS (hay chỉ gọi là 'Node') đã liên tục trở nên phổ biến kể từ khi được tạo ra vào năm 2009. Internet tràn ngập các khóa học và bài viết về nó, cài đặt nó là bước chuẩn bị cần thiết cho hầu hết công việc phát triển back-end, và dĩ nhiên số lượng việc làm yêu cầu kiến thức về nó cũng đang tăng lên.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Mô tả mục đích của server.
- Mô tả sự khác nhau giữa website tĩnh và website động.
- Giải thích vì sao project có thể cần back-end.
- Giải thích khi nào project không cần back-end.
- Giải thích event loop.
- Hiểu nguồn gốc của Node.js runtime.
- Viết một ứng dụng "hello world" cơ bản và chạy nó trong console trên máy của bạn.
- Hiểu Node.js thực sự là gì.

### Node là gì?

[Website Node.js](https://nodejs.org/en/about/) định nghĩa:

> Là một JavaScript runtime bất đồng bộ, hướng sự kiện, Node được thiết kế để xây dựng các ứng dụng mạng có khả năng mở rộng.

Đây là một định nghĩa cần được giải thích thêm một chút.

Điều quan trọng cần hiểu ngay từ đầu là Node là một "JavaScript runtime." Khi JavaScript mới được tạo ra, nó được thiết kế để chạy *trong trình duyệt*. Điều đó có nghĩa là không thể dùng JavaScript để viết bất kỳ loại chương trình nào không phải website. Node đưa JavaScript *ra khỏi* thế giới trình duyệt. Nhờ vậy, developer có thể dùng JavaScript để làm gần như mọi việc mà các ngôn ngữ phía máy chủ phổ biến khác như Ruby, PHP, C# và Python có thể làm. Ở mức cơ bản nhất, Node cho phép bạn chạy code JavaScript trên một máy như máy tính cá nhân hoặc server mà không cần thông qua trình duyệt web.

Để hỗ trợ việc này, Node có thêm một số chức năng không có trong JavaScript chạy trên trình duyệt, chẳng hạn khả năng đọc và ghi tệp cục bộ, tạo kết nối HTTP và lắng nghe network request.

### Hướng sự kiện

Quay lại định nghĩa trên website Node: Node là một runtime JavaScript **asynchronous event driven**. Trong ngữ cảnh này, **asynchronous** nghĩa là khi viết code, bạn không cố dự đoán chính xác thứ tự mọi dòng sẽ chạy. Thay vào đó, bạn viết code thành tập hợp các hàm nhỏ được gọi để phản hồi những event cụ thể, chẳng hạn network request (**event driven**).

Ví dụ, giả sử bạn đang viết một chương trình cần làm những việc sau: đọc văn bản từ một tệp, in văn bản đó ra console, truy vấn cơ sở dữ liệu để lấy danh sách người dùng và lọc người dùng theo độ tuổi.

Thay vì yêu cầu code thực hiện tuần tự như sau:

1. Đọc tệp
1. In nội dung tệp
1. Truy vấn cơ sở dữ liệu
1. Lọc kết quả truy vấn cơ sở dữ liệu

Bạn có thể chia nhiệm vụ như sau:

1. Đọc tệp *RỒI SAU ĐÓ* in nội dung tệp
1. Truy vấn cơ sở dữ liệu *RỒI SAU ĐÓ* lọc kết quả truy vấn cơ sở dữ liệu.

Khi chạy chương trình, Node bắt đầu từ trên cùng và bắt đầu đọc tệp. Nhưng vì đây là thao tác mất thời gian, nó sẽ lập tức bắt đầu bước thứ hai (truy vấn cơ sở dữ liệu) trong khi chờ đọc xong tệp.

Trong lúc cả hai process đang chạy, Node ngồi chờ một *event*. Ở đây, nó chờ cả hai process hoàn tất: đọc tệp và truy vấn cơ sở dữ liệu. Khi một trong hai nhiệm vụ hoàn tất, Node sẽ phát event để chạy hàm tiếp theo mà ta đã định nghĩa. Nếu process đọc tệp xong trước, nó sẽ in nội dung tệp. Nếu truy vấn cơ sở dữ liệu xong trước, nó sẽ bắt đầu quá trình lọc. Với vai trò programmer, ta không biết và cũng không cần quan tâm hai process hoàn tất theo thứ tự nào. Nếu code được xử lý đồng bộ (thay vì bất đồng bộ), ta phải chờ từng bước trong chương trình trước khi chuyển sang bước tiếp theo, điều này có thể làm mọi thứ chậm đi đáng kể. Nếu tệp cần đọc rất dài, ta có thể phải chờ vài giây trước khi truy vấn cơ sở dữ liệu bắt đầu.

Quá trình này gần như giống hệt cách bạn dùng `addEventListener` trong JavaScript front-end để chờ hành động của người dùng như click chuột hoặc nhấn phím. Khác biệt chính là event ở đây sẽ là những thứ như network request và truy vấn cơ sở dữ liệu. Chức năng này được hỗ trợ thông qua callback. Callback cực kỳ quan trọng với Node, vì vậy hãy dành ít phút đọc [bài viết về callback](https://dev.to/i3uckwheat/understanding-callbacks-2o9e) để bảo đảm bạn đã nắm được.

Hãy xem một ví dụ thực tế nhanh:

```javascript
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World!');
}).listen(8080);
```

Đoạn code này nằm trong bài đầu tiên của một tutorial mà bạn sẽ sớm học. Về cơ bản, code tạo một server và nói rằng: "Mỗi khi nhận được network request, hãy chạy callback function này." Hàm này trả về văn bản 'Hello World!'. Vì vậy, nếu mở trình duyệt và truy cập đúng địa chỉ cùng port, bạn sẽ thấy văn bản đó trên màn hình.

<div class="lesson-note" markdown="1">

#### Dùng React trong khóa học Node.js

Chúng ta sẽ bắt đầu bằng việc tập trung vào các khái niệm phía máy chủ, vì vậy những nội dung phía client như React sẽ để lại sau, khi bạn bắt đầu kết hợp các front-end và back-end phức tạp hơn. Bạn nên học theo đúng nội dung khóa học; đi chệch hướng dẫn có thể khiến mọi việc khó hơn không cần thiết.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [Module ngắn về "The Server Side" của MDN](https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps) là nguồn tuyệt vời cho kiến thức nền cần thiết. Hãy đọc hai bài viết đầu tiên trong phần 'Tutorials': Introduction to the server-side và Client-Server overview.
1. Để hiểu sâu hơn bản chất của Node và giải thích phần còn lại trong định nghĩa trên, hãy đọc [bài viết về NodeJS](https://medium.freecodecamp.org/what-exactly-is-node-js-ae36e97449f5). Hãy nhớ xem lại video *thực sự* hay về event loop ở cuối bài; có thể bạn còn nhớ video này từ phần trước của khóa học!
1. [Video ngắn về NodeJS](https://www.youtube.com/watch?v=uVwtVBpw7RQ) này cũng là một phần giới thiệu tuyệt vời!

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ sung.

- Đọc bài viết về [7 điều tuyệt vời bạn có thể xây dựng bằng Node.js](https://blog.teamtreehouse.com/7-awesome-things-can-build-node-js).