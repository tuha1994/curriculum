### Giới thiệu

Trong bài học trước, chúng ta đã chia nhỏ một ứng dụng Express cơ bản và hành trình của một yêu cầu thông qua ứng dụng đó. Tuy nhiên, trong một ứng dụng thực, chúng tôi muốn xử lý nhiều loại yêu cầu khác nhau cho những mục đích khác nhau.

Về cơ bản, các tuyến chỉ khớp với động từ HTTP của yêu cầu (ví dụ: GET hoặc POST) và đường dẫn URL tới tập hợp các chức năng phần mềm trung gian thích hợp - bộ điều khiển. Chúng ta sẽ khám phá bộ điều khiển và phần mềm trung gian chi tiết hơn trong bài học tiếp theo. Bây giờ, hãy đi sâu vào cách chúng ta có thể sử dụng các tuyến đường.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Trình bày cách xác định đường đi.
- Giải thích các tham số tuyến đường và các tham số truy vấn.
- Thiết lập bộ định tuyến mới trên một đường dẫn.

### Cấu trúc của một tuyến đường

Hãy quay lại lộ trình duy nhất của chúng ta từ ứng dụng Express cơ bản trước đây.

```javascript
app.get("/", (req, res) => res.send("Hello, world!"));
```

`app.get("/" ...` cho chúng tôi biết rằng tuyến đường này sẽ khớp với mọi yêu cầu GET đi qua bộ định tuyến `app` (là toàn bộ máy chủ của chúng tôi!) với đường dẫn `/`. Nếu thay vào đó chúng ta có những điều sau đây:

```javascript
app.post("/messages", (req, res) => res.send("This is where you can see any messages."));
```

Điều đó sẽ cho chúng ta biết tuyến đường khớp với bất kỳ yêu cầu POST nào với đường dẫn `/messages` của `app` của chúng ta. Nếu bạn gửi yêu cầu GET tới đường dẫn `/messages`, nó sẽ không khớp với tuyến đường này. Mỗi động từ HTTP có phương thức lộ trình Express riêng và bạn cũng có thể sử dụng [app.all()](https://expressjs.com/en/5x/api/application/#appall) để tạo một lộ trình khớp với tất cả các động từ.

<div class="lesson-note" markdown="1">

#### Động từ HTTP

[Có nhiều động từ HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods), nhưng hiện tại, chúng tôi sẽ chủ yếu sử dụng GET (chỉ được sử dụng để truy xuất dữ liệu từ máy chủ) và POST (được sử dụng để gửi dữ liệu đến máy chủ, ví dụ: biểu mẫu). Sau này, khi chúng tôi đề cập đến API REST, bạn cũng sẽ thường gặp các động từ như PUT và DELETE.

</div>

### Đường dẫn

Đối số đầu tiên chúng ta truyền vào một tuyến đường là đường dẫn cần khớp, có thể là một chuỗi hoặc một biểu thức chính quy. `/messages` khớp chính xác với điều đó, trong khi `/messages/all` chỉ khớp nếu đường dẫn là `/messages/all` (không phải `/messages` hay `/messages/new`).

Với đường dẫn chuỗi, chúng ta cũng có thể sử dụng `{}` để tạo các ký tự tùy chọn. Ví dụ:

```javascript
// Matches both /message and /messages
"/message{s}"

// Matches both / and /messages
"/{messages}"

// Matches both /foo/baz and /foo/bar/baz
"/foo{/bar}/baz"
```

Với `*` ("splat" hoặc "ký tự đại diện"), chúng ta có thể khớp bất kỳ số lượng ký tự nào. [Các biểu tượng trong đường dẫn Express phải luôn được theo sau bởi tên](https://expressjs.com/en/5x/guide/routing/#wildcards).

Trường hợp sử dụng phổ biến cho biểu tượng sẽ là trường hợp tổng hợp cho tất cả các đường dẫn chưa từng có, ví dụ: để xử lý lỗi 404 tùy chỉnh.

```javascript
// Matches / and /odin as well as /sdds8fjsdifhj98sdfh
"/{*splat}"
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Thứ tự rất quan trọng!

Các tuyến đường của bạn sẽ được thiết lập trong máy chủ theo thứ tự chúng được xác định.

```javascript
app.get("/{*splat}", (req, res) => {
  res.send("/{*splat} is a great way to catch all otherwise unmatched paths, e.g. for custom 404 error handling.");
});
app.get("/messages", (req, res) => {
  res.send("This route will not be reached because the previous route's path matches first.");
});
```

Để yêu cầu `GET /messages` của chúng tôi khớp với tuyến đường `/messages`, chúng tôi sẽ cần đảo ngược thứ tự các tuyến đường mà chúng tôi đã xác định. Làm như vậy sẽ ngăn nó tiếp cận tuyến đường `/{*splat}` vì trước tiên nó sẽ khớp với tuyến đường `/messages`.

</div>

#### Tham số tuyến đường

Điều gì sẽ xảy ra nếu chúng ta muốn có lộ trình cho tất cả tin nhắn cho bất kỳ tên người dùng nào, ví dụ: `/odin/messages` hoặc `/thor/messages` hoặc thậm chí `/theodinproject79687378/messages`? Giống như với React Router, chúng ta có thể sử dụng `route parameters` và một đường dẫn có thể chứa bao nhiêu tham số này mà chúng ta cần.

Để biểu thị một tham số tuyến đường, chúng tôi bắt đầu một phân đoạn bằng `:`, theo sau là tên của tham số (chỉ có thể bao gồm các ký tự chữ và số phân biệt chữ hoa chữ thường hoặc `_`). Dù chúng ta đặt tên cho tham số tuyến đường đó là gì, Express sẽ tự động điền đối tượng `req.params` vào bất kỳ hàm phần mềm trung gian nào sau đây với bất kỳ giá trị nào mà đường dẫn được truyền vào tham số, sử dụng tên tham số làm khóa.

```javascript
/**
 * GET /odin/messages will have this log
 * { username: "odin" }
 *
 * GET /theodinproject79687378/messages would instead log
 * { username: "theodinproject79687378" }
 */
app.get("/:username/messages", (req, res) => {
  console.log(req.params);
  res.end();
});

/**
 * GET /odin/messages/79687378 will have this log
 * { username: "odin", messageId: "79687378" }
 */
app.get("/:username/messages/:messageId", (req, res) => {
  console.log(req.params);
  res.end();
});
```

Bây giờ chúng ta có thể dễ dàng trích xuất các giá trị từ đường dẫn yêu cầu để sử dụng trong chuỗi phần mềm trung gian của mình!

#### Tham số truy vấn

Tham số truy vấn là phần duy nhất và tùy chọn của URL xuất hiện ở cuối. `?` biểu thị sự bắt đầu của các tham số truy vấn, với mỗi truy vấn là một cặp khóa-giá trị có định dạng `key=value` và mỗi truy vấn được phân tách bằng `&`. Chúng đặc biệt vì chúng không thực sự được coi là một phần của con đường mà về cơ bản giống như những đối số mà chúng ta có thể truyền vào một con đường nhất định.

Ví dụ: `/odin/messages?sort=date&direction=ascending` vẫn sẽ khớp tuyến với đường dẫn `/:username/messages`, nhưng chúng ta có thể truy cập các cặp khóa-giá trị `sort=date` và `direction=ascending` bên trong chuỗi phần mềm trung gian.

Express tự động phân tích cú pháp bất kỳ tham số truy vấn nào trong một yêu cầu và sẽ điền vào đối tượng `req.query` bất kỳ cặp khóa-giá trị nào mà nó tìm thấy. Nếu bất kỳ khóa nào được lặp lại, Express sẽ đưa tất cả giá trị của khóa đó vào một mảng.

```javascript
/**
 * GET /odin/messages?sort=date&direction=ascending will log
 * Params: { username: "odin" }
 * Query: { sort: "date", direction: "ascending" }
 *
 * GET /odin/messages?sort=date&sort=likes&direction=ascending will log
 * Params: { username: "odin" }
 * Query: { sort: ["date", "likes"], direction: "ascending" }
 */
app.get("/:username/messages", (req, res) => {
  console.log("Params:", req.params);
  console.log("Query:", req.query);
  res.end();
});
```

Bạn có thể đã thấy điều này trên các trang web như YouTube. Mỗi video YouTube đều được cấp một mã và để xem video đó, bạn điều hướng đến `https://www.youtube.com/watch`, chuyển mã video đó dưới dạng tham số truy vấn bằng phím `v`. Bạn thậm chí có thể chỉ định dấu thời gian tính bằng giây mà video sẽ bắt đầu thông qua phím `t`. Vì vậy, việc thêm `?v=xm3YgoEiEDc&t=424s` sẽ [yêu cầu `/watch` từ YouTube cho video `xm3YgoEiEDc` bắt đầu từ `424` giây trong](https://www.youtube.com/watch?v=xm3YgoEiEDc&t=424s).

### Bộ định tuyến

Cho đến nay, chúng tôi chưa sử dụng nhiều tuyến đường và tất cả các tuyến đường chúng tôi hiển thị đều được gắn vào `app`, chính máy chủ của chúng tôi. Trong một ứng dụng thực tế có nhiều tuyến đường, có lẽ chúng ta muốn sắp xếp các tuyến đường của mình thành các nhóm và trích xuất từng nhóm ra tệp riêng của chúng. Sau đó, chúng tôi cũng có thể dễ dàng viết những thứ chỉ ảnh hưởng đến các tuyến đường trong tệp đó chứ không ảnh hưởng đến bất kỳ tuyến đường nào khác.

Giả sử chúng tôi đang tạo một ứng dụng thư viện và chúng tôi muốn các trang liên quan đến sách và các trang liên quan đến tác giả. Nó nằm ở đầu trang chủ và bất kỳ trang linh tinh nào khác như "giới thiệu" hoặc "liên hệ".

Chúng tôi có thể muốn máy chủ của mình xử lý các tuyến sau:

```text
GET /
GET /about
GET /contact
POST /contact

GET /books
GET /books/:bookId
GET /books/:bookId/reserve
POST /books/:bookId/reserve

GET /authors
GET /authors/:authorId
```

Sẽ thật tuyệt nếu chúng ta có thể trích xuất các nhóm định tuyến vào các tệp của riêng chúng và chúng ta có thể làm điều đó bằng cách sử dụng bộ định tuyến! Quay trở lại ứng dụng Express cơ bản của chúng ta trước đó, hãy thêm một số bộ định tuyến để xử lý từng nhóm tuyến đường của chúng ta.

Trước tiên, chúng ta sẽ cần một bộ định tuyến để có thể đặt bộ định tuyến này vào thư mục `routes` mới. Ví dụ: `routes/authorRouter.js`:

```javascript
// routes/authorRouter.js
const { Router } = require("express");

const authorRouter = Router();

authorRouter.get("/", (req, res) => res.send("All authors"));
authorRouter.get("/:authorId", (req, res) => {
  const { authorId } = req.params;
  res.send(`Author ID: ${authorId}`);
});

module.exports = authorRouter;
```

Ở phần trên, chúng ta hủy cấu trúc đối tượng Express để lấy chức năng Bộ định tuyến và sử dụng nó để tạo `authorRouter` của mình. Chúng ta có thể sử dụng các phương thức `.get` hoặc `.post` tương tự trên bộ định tuyến này thay vì trên toàn bộ đối tượng máy chủ, nghĩa là chúng ta có thể viết các tuyến và phần mềm trung gian trong phạm vi bộ định tuyến này (chúng ta sẽ đi sâu hơn vào những điều này trong bài học tiếp theo). Vì chúng tôi sẽ làm cho bộ định tuyến này chỉ có thể sử dụng được cho các đường dẫn bắt đầu bằng `/authors` nên các đường dẫn định tuyến của chúng tôi ở đây không cần bao gồm nó. Thay vào đó, chúng *extend* là đường dẫn gốc (chúng tôi không muốn tuyến đường của mình khớp với `/authors/authors/:authorId`).

Tạo hai bộ định tuyến khác cho các nhóm tuyến khác - `routes/bookRouter.js` và `routes/indexRouter.js`! Các chức năng phần mềm trung gian của họ không cần phải làm gì nhiều, chỉ cần gửi nội dung nào đó duy nhất cho từng tuyến để bạn biết tuyến nào đang được khớp.

Khi bạn đã tạo xong hai bộ định tuyến còn lại, hãy thêm chúng vào máy chủ của chúng tôi trong `app.js`:

```javascript
// app.js
const express = require("express");
const app = express();
const authorRouter = require("./routes/authorRouter");
const bookRouter = require("./routes/bookRouter");
const indexRouter = require("./routes/indexRouter");

app.use("/authors", authorRouter);
app.use("/books", bookRouter);
app.use("/", indexRouter);

const PORT = 3000;
app.listen(PORT, (error) => {
  if (error) {
    throw error;
  }
  console.log(`My first Express app - listening on port ${PORT}!`);
});
```

Chúng tôi chỉ định rằng mọi yêu cầu có đường dẫn bắt đầu bằng `/authors` sẽ được chuyển qua `authorRouter` để khớp tuyến đường. Nếu yêu cầu của chúng tôi bắt đầu bằng `/books`, nó sẽ bỏ qua các tuyến tác giả này và sau đó kiểm tra các tuyến trong `bookRouter`. Bất kỳ yêu cầu nào khác không bắt đầu bằng một trong hai yêu cầu này sẽ chạy qua `indexRouter`.

Để kiểm tra các tuyến này, hãy sử dụng [Postman](https://www.postman.com/downloads/) để cho phép bạn gửi yêu cầu `GET` và `POST` mà không cần trình duyệt (chúng tôi không thể gửi yêu cầu `POST` từ thanh địa chỉ trình duyệt).

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc qua Express' [mồi về Routing](https://expressjs.com/en/guide/routing.html) để biết tổng quan về các chủ đề của bài học này. Hãy nhớ tham khảo [Tài liệu nhanh](https://expressjs.com/en/api.html) để biết thêm thông tin về các phương pháp cụ thể.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- Video này về [Tuyến tốc hành](https://youtu.be/0Hu27PoloYw?si=LZ8wQkOTP-e50Zvi) cung cấp cái nhìn tổng quan toàn diện về các tuyến tốc hành.
