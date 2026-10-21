### Giới thiệu

Kiểm thử unit quan trọng vì nhiều lý do mà có lẽ hiện tại chúng ta không cần đề cập hết. Nếu bạn đã học khóa JavaScript cơ bản của chúng tôi, bạn đã từng gặp kiểm thử unit; mục đích của *bài học này* không phải dạy triết lý hay cơ chế viết test, mà là cách áp dụng chúng vào các ứng dụng và API Express.

Nếu bạn chưa hoàn thành [khóa JavaScript front-end](https://www.theodinproject.com/paths/full-stack-javascript/courses/javascript) của chúng tôi, hãy quay lại xem những bài học đó trước khi tiếp tục.

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về những chủ đề bạn sẽ học trong bài học này.

- Sử dụng module `supertest` để kiểm thử route/controller của Express.
- Mô tả cách SuperTest xử lý ứng dụng express của chúng ta.
- Giải thích chức năng mà `superagent` cung cấp cho SuperTest.
- Mô tả tham số `done` được dùng để làm gì.
- Giải thích và hiểu vững chức năng của method `.expect()`.
- Làm quen với tài liệu và các method của `supertest`.

### Kiểm thử route bằng supertest

Yêu cầu cơ bản và quan trọng nhất để kiểm thử một phần nào đó trong mã của bạn là phần đó phải nằm trong một module được export. Điều này đúng với cả custom middleware lẫn route/controller, vì vậy việc đầu tiên bạn cần làm là tách chúng thành các module riêng nếu chúng chưa được tách.

Với route, bạn đã biết cách thực hiện việc này bằng Express.Router. Dưới đây là một ví dụ rất cơ bản.

```javascript
//// app.js
const express = require("express");
const app = express();

app.use(express.urlencoded({ extended: false }));

const indexRouter = require("./index");
app.use("/", indexRouter);

app.listen(3000, (error) => {
  if (error) {
    throw error;
  }
  console.log("running");
});
```

```javascript
//// index.js
const express = require("express");
const index = express.Router();

const array = [];

index.get("/", (req, res) => {
  res.json({ name: "frodo" });
});

index.get("/test", (req, res) => res.json({ array }));

index.post("/test", (req, res) => {
  array.push(req.body.item);
  res.send('success!');
});

module.exports = index;
```

Hai file `app.js` và `index.js` này định nghĩa một vài route, sau đó thiết lập và khởi chạy ứng dụng express. Hiện tại chúng ta *không* cần kiểm thử `app.js` vì file này chỉ chứa mã khởi động và chạy ứng dụng express! Nó không chứa logic do chúng ta tự viết, nên không cần kiểm thử. Tuy nhiên, `index.js` *có* chứa một số phần chúng ta muốn kiểm thử.

Để thực sự kiểm thử các route này, chúng ta sẽ dùng một thư viện có tên [SuperTest](https://github.com/visionmedia/supertest); để làm quen với cú pháp test, trong các ví dụ của bài học này chúng ta sẽ kết hợp nó với Jest. Hãy chạy `npm install jest supertest --save-dev` và trong lúc cài đặt, dành vài phút đọc README trong repo Git của SuperTest (đã liên kết ở trên).

Đây là file test của chúng ta:

```javascript
const index = require("../index");

const request = require("supertest");
const express = require("express");
const app = express();

app.use(express.urlencoded({ extended: false }));
app.use("/", index);

test("index route works", done => {
  request(app)
    .get("/")
    .expect("Content-Type", /json/)
    .expect({ name: "frodo" })
    .expect(200, done);
});

test("testing route works", done => {
  request(app)
    .post("/test")
    .type("form")
    .send({ item: "hey" })
    .then(() => {
      request(app)
        .get("/test")
        .expect({ array: ["hey"] }, done);
    });
});
```

Hãy cùng đi qua từng phần.

Trước tiên, chúng ta phải import module đang kiểm thử; trong trường hợp này là file `index.js` ở trên.

```javascript
const index = require("../index");
```

Tiếp theo, chúng ta đưa cả `supertest` và `express` vào. Chúng ta thiết lập một ứng dụng Express mới, sau đó sử dụng index router đã import trước đó. Lý do phải thực hiện phần thiết lập này ở đây là vì chúng ta không thực sự chạm vào file `app.js` gốc. Lý do chính của cách làm này là để tránh gọi lệnh `app.listen` và khởi động server, nhưng nó cũng hữu ích vì trong các ứng dụng lớn hơn, chúng ta có thể bỏ qua một số bước cấu hình tùy chọn và chỉ đưa vào những phần cần thiết cho mục đích kiểm thử. Trong một test suite lớn hơn, có lẽ sẽ hữu ích nếu tách phần này thành file riêng để import vào từng file test.

```javascript
const request = require("supertest");
const express = require("express");
const app = express();

app.use(express.urlencoded({ extended: false }));
app.use("/", index);
```

Bản thân các test khá dễ hiểu nhờ thư viện SuperTest! Hãy nhớ rằng chúng ta đã import supertest dưới dạng hàm `request`, và sử dụng nó như bên dưới. Chúng ta gọi hàm này trên ứng dụng express vừa tạo, truyền route cho nó, rồi dùng nó để đảm bảo các response khớp với kiểu và nội dung mà chúng ta mong đợi.

Hãy chú ý tham số `done` được truyền vào callback của test. Hầu hết thư viện kiểm thử dùng tham số này để báo hiệu test đã hoàn thành trong trường hợp có thao tác bất đồng bộ. Trong trường hợp này, SuperTest cho phép chúng ta truyền nó vào `.expect` cuối cùng và tự gọi nó. Cảm ơn SuperTest!

```javascript
test("index route works", done => {
  request(app)
    .get("/")
    .expect("Content-Type", /json/)
    .expect({ name: "frodo" })
    .expect(200, done);
});
```

Test thứ hai rất giống test đầu tiên, nhưng kiểm thử method `post`. Bạn có thể (và nên) đọc về tất cả các hàm có thể dùng trong README của SuperTest, nên chúng ta sẽ không đi vào chi tiết từng bước ở đây. Tuy nhiên, phần cuối rất quan trọng. Đến giai đoạn này trong quá trình học JavaScript, bạn hẳn đã quen với Promise, vì vậy cú pháp `.then()` cũng không xa lạ. Trong trường hợp này, chúng ta chờ request POST hoàn tất, sau đó gọi request GET khi promise đó được resolve để kiểm tra xem item đã được thêm vào array hay chưa.

```javascript
test("testing route works", done => {
  request(app)
    .post("/test")
    .type("form")
    .send({ item: "hey" })
    .then(() => {
      request(app)
        .get("/test")
        .expect({ array: ["hey"] }, done);
    });
});
```

Nếu ở đây dùng một cơ sở dữ liệu thật, chúng ta sẽ muốn làm điều tương tự bằng cơ sở dữ liệu kiểm thử hoặc cơ sở dữ liệu giả. Chúng ta sẽ nói về cách thiết lập một hệ thống như vậy trong một bài học riêng. Tạm thời chỉ cần nhớ rằng bạn không muốn chạy mã test trên cơ sở dữ liệu production!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy đọc kỹ [tài liệu SuperTest](https://github.com/forwardemail/supertest)
1. SuperTest thực tế lấy chức năng từ một dự án liên quan khác có tên SuperAgent. Bất kỳ method nào bạn có thể gọi trong SuperAgent cũng có thể gọi từ SuperTest, vì vậy bạn cũng cần xem qua [tài liệu SuperAgent](https://forwardemail.github.io/superagent/).

</div>