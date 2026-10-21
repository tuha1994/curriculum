### Giới thiệu

Controller thực sự đóng vai trò là người trung gian cuối cùng. Nó biết mình cần hỏi model điều gì, nhưng để model đảm nhiệm phần lớn công việc giải quyết các câu hỏi đó. Nó biết view nào cần render và gửi về trình duyệt, nhưng để view tự lo việc ghép toàn bộ HTML. Vì vậy, nó là một "controller": đủ thông minh để biết phải làm gì rồi ủy thác mọi phần việc nặng nhọc.

Tóm lại, controller là bộ não của hoạt động: bảo đảm mỗi thành phần hoàn thành đúng vai trò để tạo ra kết quả cuối cùng. Về bản chất, controller chỉ là một hàm có các trách nhiệm được xác định rõ ràng trong mô hình MVC.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Khám phá các phương pháp phản ứng khác nhau.
- Triển khai middleware xử lý lỗi để bắt và xử lý lỗi một cách khéo léo.
- Mô tả các trường hợp sử dụng phổ biến của middleware, chẳng hạn như validation và authentication.

### Xử lý phản hồi

Khi nói đến việc gửi phản hồi từ controller, chúng tôi có một số phương pháp tùy ý sử dụng. Hãy cùng khám phá một số phương pháp thường được sử dụng và trường hợp sử dụng của chúng.

- [res.send](https://expressjs.com/en/5x/api/response/#ressend) - Một phương pháp có mục đích chung để gửi phản hồi, nó linh hoạt với dữ liệu chúng tôi có thể gửi vì nó sẽ tự động đặt tiêu đề `Content-Type` dựa trên dữ liệu bạn chuyển. Ví dụ: nếu chúng ta truyền vào một đối tượng, nó sẽ xâu chuỗi nó thành JSON và đặt tiêu đề `Content-Type` thành `application/json`.
- [res.json](https://expressjs.com/en/5x/api/response/#resjson) - Đây là cách rõ ràng hơn để phản hồi yêu cầu bằng JSON. Điều này luôn đặt tiêu đề `Content-Type` thành `application/json` và gửi dữ liệu dưới dạng JSON.
- [res.redirect](https://expressjs.com/en/5x/api/response/#resredirect) - Khi chúng tôi muốn chuyển hướng máy khách đến một URL khác, phương pháp này cho phép khả năng đó.
- [res.render](https://expressjs.com/en/5x/api/response/#resrender) - `res.render` cho phép bạn hiển thị mẫu view và gửi HTML kết quả làm phản hồi. Chúng ta sẽ đề cập đến điều này trong bài học sau.

Ngoài ra còn có một phương pháp hữu ích mà chúng ta có thể sử dụng để đặt mã trạng thái theo cách thủ công.

- [res.status](https://expressjs.com/en/5x/api/response/#resstatus) - Lệnh này đặt mã trạng thái của phản hồi **nhưng không tự kết thúc chu kỳ yêu cầu-phản hồi**. Chúng ta có thể xâu chuỗi các phương thức khác thông qua điều này (ví dụ: `res.status(404).send(...)` nhưng lưu ý rằng chúng ta không thể thực hiện `res.send(...).status(404)`). Chúng tôi có thể bỏ qua điều này nếu muốn sử dụng mã trạng thái mặc định của `200`.

<div class="lesson-note" markdown="1">

#### res.send và res.json

Nếu `res.send` tự động đặt `Content-Type` dựa trên dữ liệu được truyền, tại sao chúng ta vẫn sử dụng `res.json`? `res.json` thực thi JSON và sẽ tự động chuyển đổi các giá trị không phải đối tượng thành JSON, nhưng `res.send` thì không. `res.json` chỉ là một phương thức thuận tiện cũng gọi nội bộ `res.send`. `res.send` sẽ chỉ xử lý mọi thứ dưới dạng JSON khi xử lý các boolean và đối tượng (bao gồm mảng).

Vì vậy, để thuận tiện, sẽ thích hợp hơn khi sử dụng `res.json` thay vì `res.send` và nếu chúng tôi gửi JSON, chúng tôi cũng có thể sử dụng một phương thức có tên theo nghĩa đen là "json". Nó giống như một sự kết hợp hoàn hảo!

</div>

Chúng ta cũng cần lưu ý rằng các phương thức phản hồi này chỉ kết thúc chu trình phản hồi yêu cầu. Chúng không kết thúc việc thực thi hàm. Ví dụ: nếu chúng ta làm điều này:

```javascript
app.use((req, res) => {
  // This works and this ends the request-response cycle
  res.send("Hello");

  // However, it does not exit the function so this will still run
  console.log('will still run!!');

  // This will then throw an error that we cannot send again after sending to the client already
  res.send("Bye");
});
```

### Middleware

Các hàm middleware là khái niệm cốt lõi trong Express và đóng vai trò quan trọng trong việc xử lý các yêu cầu và phản hồi. Chúng hoạt động giữa yêu cầu đến và trình xử lý route dự định cuối cùng.

Một hàm middleware thường có ba đối số (tuy nhiên, có một đối số mà chúng ta sẽ đề cập sau có bốn đối số):

- `req` - Đối tượng yêu cầu, đại diện cho yêu cầu HTTP đến.
- `res` - Đối tượng phản hồi, thể hiện phản hồi HTTP sẽ được gửi lại cho máy khách.
- `next` - Hàm chuyển quyền kiểm soát cho hàm middleware tiếp theo trong chuỗi (chúng ta sẽ đề cập đến vấn đề này sau). Đây là tùy chọn.

<div class="lesson-note" markdown="1">

#### Quy ước đặt tên

Tên gọi chỉ mang tính quy ước, chúng ta có thể đặt tên tùy thích như `req` -> `request`, `res` -> `response`, v.v.

</div>

Chức năng middleware có thể thực hiện nhiều tác vụ khác nhau, chẳng hạn như:

- Sửa đổi đối tượng yêu cầu hoặc phản hồi (ví dụ: một số gói sẽ thực hiện việc này, như thêm thuộc tính mới vào đối tượng yêu cầu hoặc đặt `res.locals` được sử dụng trong các mẫu được hiển thị bằng `res.render`).
- Thực thi mã bổ sung (các hàm middleware xác thực để xác thực yêu cầu trước khi đi đến logic chính của chúng tôi, các hàm middleware xác thực, v.v.).
- Gọi hàm middleware tiếp theo trong chuỗi.
- Kết thúc chu trình yêu cầu-phản hồi (nghĩa là không còn hàm middleware nào được gọi nữa, ngay cả khi có nhiều chức năng hơn trong chuỗi).

Express có hệ sinh thái phong phú nên bạn có thể sẽ tìm thấy gói giải quyết được vấn đề bạn đang gặp phải. Ví dụ: một số gói cung cấp các hàm middleware để xử lý xác thực, CORS, giới hạn tốc độ, phiên, ghi nhật ký, xác thực, v.v.! Trong suốt bài học này (và khóa học), chúng tôi sẽ giới thiệu các middleware cần thiết để xây dựng dự án trong khóa học này, nhưng bạn có thể tự mình khám phá.

#### Middleware cấp ứng dụng

Middleware cấp ứng dụng được liên kết với một *phiên bản Express* bằng cách sử dụng các chức năng `app.use` hoặc `app.METHOD` (ví dụ: `app.get`, `app.post`). Express thực thi các hàm middleware này cho mọi yêu cầu đến phù hợp với đường dẫn đã chỉ định. Nếu chúng tôi không chỉ định đường dẫn, Express sẽ mặc định đường dẫn đến `/`, khớp với mọi yêu cầu đến. Giống như bất kỳ hàm middleware nào, chúng sẽ không chạy nếu chu kỳ phản hồi yêu cầu kết thúc trước khi tiếp cận chúng. Thông thường, các hàm middleware này được đặt ở đầu mã ứng dụng của chúng tôi để đảm bảo chúng luôn chạy trước.

Express cung cấp một số hàm middleware tích hợp cần thiết mà chúng ta sẽ thường xuyên sử dụng trong các ứng dụng của mình. Chúng bao gồm:

- Trình phân tích cú pháp nội dung (ví dụ: `express.json`, `express.urlencoded`) - Các trình phân tích cú pháp này cho phép chúng tôi phân tích cú pháp chính xác nội dung của yêu cầu đến để chúng tôi có thể sử dụng nó thông qua `req.body`.
- Cung cấp các tệp tĩnh (ví dụ: `app.use(express.static('public'))`) - Chức năng middleware này phục vụ các tệp tĩnh như HTML, CSS, JavaScript và hình ảnh. Chúng ta có thể truyền một đối số để chỉ định thư mục nào sẽ phục vụ các tệp tĩnh.

#### Middleware cấp router

Middleware cấp router hoạt động tương tự như middleware cấp ứng dụng, nhưng nó bị ràng buộc với một *phiên bản của router Express* bằng cách sử dụng các chức năng `router.use` hoặc `router.METHOD` (ví dụ: router.get). Do đó, Express chỉ thực thi các middleware này khi yêu cầu khớp và đi qua router đó.

Dưới đây là một ví dụ về hàm middleware cơ bản:

```javascript
function myMiddleware(req, res, next) {
  // Perform some operations
  console.log("Middleware function called");

  // Modify the request object
  req.customProperty = "Hello from myMiddleware";

  // Call the next middleware/route handler
  next();
}

app.use(myMiddleware);
```

Trong ví dụ này, hàm middleware ghi lại thông báo, thêm thuộc tính tùy chỉnh vào đối tượng yêu cầu, sau đó gọi hàm `next()` để chuyển quyền điều khiển cho hàm middleware tiếp theo hoặc trình xử lý route. Chúng tôi cũng đăng ký hàm middleware thông qua việc sử dụng `app.use`, điều này làm cho phần mềm này trở thành middleware cấp ứng dụng. Các hàm middleware theo sau `myMiddleware` trong chuỗi này hiện có thể truy cập `req.customProperty`, chứa giá trị `"Hello from myMiddleware"`.

Một điều cần lưu ý là Express thực thi các chức năng của middleware theo thứ tự chúng ta xác định hoặc đăng ký chúng trong ứng dụng của mình. Điều này có nghĩa là trình tự mà chúng ta xác định các chức năng của middleware là quan trọng, vì nó xác định thứ tự chúng sẽ được gọi trong chu kỳ phản hồi yêu cầu. Vì vậy, chúng ta cần đảm bảo và lưu ý rằng các hàm middleware của chúng ta được đặt theo đúng thứ tự. Ví dụ: một số gói có hàm middleware thay đổi đối tượng `Request` và do đó, các hàm middleware này phải được đặt ở đầu ứng dụng của chúng tôi để chúng tôi có thể thấy những thay đổi của chúng trong tất cả các hàm middleware bên dưới nó.

Ngoài ra còn có một loại hàm middleware đặc biệt để xử lý lỗi mà chúng ta sẽ thảo luận sau.

### Controller

Như đã nói trước đó, controller chỉ là chức năng. Họ cũng phân loại là middleware (ít nhất là trong thế giới Express) được sử dụng bởi trình xử lý route. Tuy nhiên, điều quan trọng cần lưu ý là controller và middleware là những khái niệm riêng biệt. Controller là thành phần chính của mẫu MVC (Model-View-Controller), một phương pháp thiết kế để tổ chức phần mềm. Mặt khác, Middleware là tính năng cốt lõi của Express cho phép bạn chạy mã, sửa đổi yêu cầu hoặc kết thúc chu trình tại các điểm cụ thể trong chu trình phản hồi yêu cầu. Vì vậy, chúng tôi đang sử dụng middleware trong Express để triển khai phần "Controller" của mẫu MVC.

Controller sẽ hoạt động bất cứ khi nào có yêu cầu truy cập vào máy chủ và route khớp với động từ và đường dẫn HTTP được yêu cầu. Route xác định controller nào sẽ xử lý yêu cầu dựa trên chuỗi middleware đã xác định. Sau đó, controller thích hợp sẽ tiếp quản, thực hiện các hành động cần thiết để thực hiện yêu cầu. Điều này có thể liên quan đến việc truy xuất dữ liệu từ mô hình, xử lý dữ liệu, đưa ra quyết định dựa trên logic nghiệp vụ hoặc cập nhật mô hình bằng dữ liệu mới.

Sau khi controller hoàn thành nhiệm vụ của mình, nó sẽ chuyển dữ liệu đã xử lý sang view. Sau đó, view sẽ hiển thị dữ liệu này thành định dạng phù hợp để gửi lại cho khách hàng. Thông thường, đây sẽ là HTML. Sau này, khi đề cập đến việc xây dựng API, chúng tôi cũng có thể gửi phản hồi JSON giống như với các API mà chúng tôi đã sử dụng trước đây, ví dụ: API Giphy.

Quy ước đặt tên cho các controller này thường dựa trên route mà chúng sẽ được gắn vào, ví dụ: Tuyến `GET` -> `getSomething`, `POST` tuyến -> `createSomething`, tuyến `DELETE` -> `deleteSomething`, v.v. Tuy nhiên, không có quy tắc cố định nào vì Express không có ý kiến. Nó sẽ luôn dựa trên quy ước của bạn hoặc của người khác và các yêu cầu của chức năng.

Bạn đã thiết lập sẵn ứng dụng của mình từ bài học trước. Trước đây, chúng tôi đã xác định route cho `/authors/:authorId`, nhưng lần này chúng tôi sẽ xác định controller cho route đó và kết nối nó với dữ liệu mẫu. Nhưng để làm cho ví dụ tạo ra thứ gì đó, trước tiên hãy tạo một cơ sở dữ liệu mô phỏng với một hàm mà chúng ta có thể sử dụng để truy xuất tác giả theo ID của họ.

Tạo tệp sau `db.js` ở thư mục gốc của dự án và thêm mã sau:

```javascript
// db.js

const authors = [
  { id: 1, name: "Bryan" },
  { id: 2, name: "Christian" },
  { id: 3, name: "Jason" },
];

async function getAuthorById(authorId) {
  return authors.find(author => author.id === authorId);
};

module.exports = { getAuthorById };
```

Tên tệp, nội dung và vị trí không quan trọng ở đây. Đây chỉ là một "cơ sở dữ liệu" mô phỏng nhanh và hàm truy vấn để chúng ta có thể gọi nó trong controller.

Sau đó, đối với controller, hãy tạo tệp `authorController.js` sau trong thư mục `controllers`:

```javascript
// controllers/authorController.js

const db = require("../db");

async function getAuthorById(req, res) {
  const { authorId } = req.params;

  const author = await db.getAuthorById(Number(authorId));

  if (!author) {
    res.status(404).send("Author not found");
    return;
  }

  res.send(`Author Name: ${author.name}`);
};

module.exports = { getAuthorById };
```

Trong ví dụ này, hàm `getAuthorById` là controller xử lý một hành động cụ thể liên quan đến việc truy xuất tác giả bằng ID của họ. Chúng tôi sẽ sử dụng controller này bằng cách nhập nó vào tệp nơi chúng tôi xác định route của mình và sử dụng nó như thế này:

```javascript
// routes/authorRouter.js

const { Router } = require("express");
const { getAuthorById } = require('../controllers/authorController');

const authorRouter = Router();

// ... other route handlers
authorRouter.get("/:authorId", getAuthorById);
```

Hãy chia nhỏ những gì đang xảy ra trong controller này:

1. Đường dẫn tuyến chứa tham số tuyến (`/authors/:authorId`). Controller trích xuất `authorId` từ `req.params`.
1. Sau đó, nó gọi hàm truy vấn cơ sở dữ liệu `getAuthorById` để truy xuất dữ liệu tác giả dựa trên `authorId`.
1. Nếu controller không tìm thấy tác giả, nó sẽ gửi phản hồi có mã trạng thái 404 và thông báo `"Author not found"`, sử dụng `res.status(404).send(...)`. Sau đó, nó trả về từ hàm controller để tránh gọi bất kỳ logic nào khác trong controller, vì việc gửi phản hồi không tự động dừng việc thực thi hàm.
1. Nếu controller tìm thấy tác giả, nó sẽ gửi phản hồi với mã trạng thái 200 kèm theo văn bản hiển thị tên tác giả bằng cách sử dụng `res.send(...)`.

Rất đơn giản phải không?

### Xử lý lỗi

Khi xây dựng các ứng dụng mạnh mẽ, điều quan trọng là phải xử lý các lỗi trong ứng dụng của chúng ta một cách khéo léo. Việc triển khai xử lý lỗi thích hợp cho phép bạn cung cấp các phản hồi lỗi có ý nghĩa cho máy khách và ngăn ứng dụng của bạn gặp sự cố bất ngờ. Và như chúng tôi biết, lỗi có thể xảy ra trong quá trình hoạt động không đồng bộ và chúng tôi không xử lý chính xác lỗi đó trong các đoạn mã trước đó. Hãy xem chúng ta có thể làm gì để xử lý những khả năng này.

#### thử/bắt

Sử dụng cùng một mã từ trước đó, chúng ta có thể nhanh chóng xử lý lỗi bằng cách gói logic controller của mình vào khối `try/catch`.

```javascript
async function getAuthorById(req, res) {
  const { authorId } = req.params;

  try {
    const author = await db.getAuthorById(Number(authorId));

    if (!author) {
      res.status(404).send("Author not found");
      return;
    }

    res.send(`Author Name: ${author.name}`);
  } catch (error) {
    console.error("Error retrieving author:", error);
    res.status(500).send("Internal Server Error");

    // or we can call next(error) instead of sending a response here
    // Using `next(error)` however will only render an error page in the express' default view and respond with the whole html to the client.
    // So we will need to create a special type of middleware function if we want a different response and we will get to that... next.
  }
};
```

Điều gì sẽ xảy ra nếu chúng ta có nhiều hàm middleware không đồng bộ và nhiều chức năng trong số đó phản hồi theo cùng một cách chỉ với những thứ như mã trạng thái và thông báo phản hồi khác nhau? Thay vì `try/catch` trong mỗi middleware, Express tự động phát hiện bất kỳ lỗi nào xảy ra và gọi `next(error)`, điều này sẽ chuyển quyền kiểm soát sang một "hàm middleware lỗi" đặc biệt, nghĩa là chúng ta sẽ không cần phải bắt và xử lý trong chính controller gốc.

#### Middleware xử lý lỗi

Chức năng middleware xử lý lỗi xử lý tất cả các lỗi trong ứng dụng của chúng tôi xuất phát từ các hàm middleware khác. Chúng tôi muốn đặt hàm middleware báo lỗi này ở cuối mã ứng dụng để đảm bảo rằng đó là hàm middleware cuối cùng được thực thi và chỉ xử lý các lỗi xuất hiện từ các hàm middleware trước đó.

Thêm mã sau vào `app.js` vào cuối tất cả các chức năng của middleware trong ứng dụng của chúng tôi:

```javascript
// Every thrown error in the application or the previous middleware function calling `next` with an error as an argument will eventually go to this middleware function
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).send(err);
});
```

Tuy nhiên, hãy lưu ý rằng đây là chức năng của middleware yêu cầu *bốn tham số* mà chúng tôi sẽ cần cung cấp ngay cả khi chúng không được sử dụng. Ví dụ: nếu chúng tôi loại trừ một trong các tham số, thì tham số đó sẽ không được nhận dạng là hàm middleware có lỗi. Bạn có thể tự mình thử nhé ;)

Đây là một trường hợp kỳ lạ trong đó đối tượng lỗi *phải* là tham số đầu tiên trong lệnh gọi lại.

Hàm middleware này xử lý *lỗi xảy ra* trong các hàm middleware khác hoặc thứ gì đó được gửi bởi hàm middleware trước đó bằng cách sử dụng hàm `next` (ví dụ: `next(err)`).

Vì vậy, cách Express phân biệt hàm middleware này một lần nữa là thông qua việc thêm *bốn tham số*, không thiếu một tham số nào. Chức năng middleware định tuyến hoặc hàm middleware có *ít hơn bốn tham số* sẽ luôn được coi là hàm middleware yêu cầu thay vì hàm middleware lỗi này, ngay cả khi chúng tôi đặt nó ở cuối cùng.

```javascript
app.use((req, res, next) => {
  throw new Error("OH NO!");
  // or next(new Error("OH NO!"));
});

app.use((err, req, res, next) => {
  console.error(err);
  // You will see an OH NO! in the page, with a status code of 500 that can be seen in the network tab of the dev tools
  res.status(500).send(err.message);
});
```

#### Tạo lỗi tùy chỉnh

Với các giải pháp trên, hàm middleware báo lỗi chỉ có thể thực sự phản hồi bằng mã trạng thái `500` cho dù đó là lỗi gì. Nhưng nếu chúng ta thực sự muốn gửi `404` thì sao? Một cách phổ biến để làm điều này là tạo ra lỗi tùy chỉnh của riêng chúng ta bằng cách mở rộng đối tượng Error.

Tạo tệp sau `CustomNotFoundError.js` trong thư mục `errors`:

```javascript
// errors/CustomNotFoundError.js

class CustomNotFoundError extends Error {
  constructor(message) {
    super(message);
    this.statusCode = 404;
    // So the error is neat when stringified. NotFoundError: message instead of Error: message
    this.name = "NotFoundError";
  }
}

module.exports = CustomNotFoundError;
```

Sau đó, chúng tôi có thể sử dụng lớp lỗi tùy chỉnh này và cấu trúc lại phiên bản `getAuthorById` trước đó như sau:

```javascript
const CustomNotFoundError = require("../errors/CustomNotFoundError");

const getAuthorById = async (req, res) => {
  const { authorId } = req.params;

  const author = await db.getAuthorById(Number(authorId));

  if (!author) {
    throw new CustomNotFoundError("Author not found");
  }

  res.send(`Author Name: ${author.name}`);
};
```

Vì Express sẽ tự động phát hiện bất kỳ lỗi nào xảy ra trong hàm middleware không đồng bộ nên chúng ta không cần gửi phản hồi lỗi bên trong hàm này mà thay vào đó chỉ cần đưa ra lỗi. Express gọi `next()`, chuyển lỗi đã bắt được làm đối số, chuyển quyền kiểm soát cho trình xử lý lỗi tùy chỉnh của chúng tôi!

Cuối cùng nó sẽ gặp lỗi ở hàm middleware và chúng ta cũng có thể sửa đổi:

```javascript
app.use((err, req, res, next) => {
  console.error(err);
  // We can now specify the `err.statusCode` that exists in our custom error class and if it does not exist it's probably an internal server error
  res.status(err.statusCode || 500).send(err.message);
});
```

Đây là một mẫu hữu ích và chúng ta có thể tạo thêm các lớp lỗi tùy chỉnh cho các trường hợp sử dụng khác nhau nếu cần.

#### Chức năng tiếp theo là gì

Chúng tôi đã sử dụng hàm `next` một vài lần. Nhưng chính xác thì nó là gì? Nói một cách đơn giản, nó được sử dụng để chuyển quyền kiểm soát cho hàm middleware tiếp theo trong chu trình phản hồi yêu cầu của ứng dụng. Để đưa ra một ví dụ:

```javascript
function middleware1(req, res, next) {
  console.log("Middleware 1");
  next(); // Pass control to the next middleware
};

function middleware2(req, res, next) {
  console.log("Middleware 2");
  res.send("Response from Middleware 2");
  // request-response cycle ends here
};

function middleware3(req, res, next) {
  console.log("Middleware 3");
  res.send("Response from Middleware 3");
};

app.use(middleware1);
app.use(middleware2);
app.use(middleware3);
// will log `Middleware 1` -> `Middleware 2` and send a response with the text "Response from Middleware 2"
```

Ở đây chúng ta có `middleware1`, `middleware2` và `middleware3`. `middleware1` gọi hàm `next` và vì chúng tôi chưa gửi phản hồi nên chúng tôi chuyển quyền điều khiển cho hàm middleware tiếp theo - `middleware2` (như được biểu thị theo thứ tự các lệnh gọi `app.use`). Trong `middleware2`, chúng tôi gửi phản hồi kết thúc chu trình phản hồi yêu cầu. Vì nó đã kết thúc nên hàm middleware thứ ba (`middleware3`) không chạy. Nhưng nếu bằng cách nào đó chúng ta không gọi hàm `next` trong `middleware1`, bạn có biết điều gì sẽ xảy ra không? Có lẽ hãy truy cập [máy chủ TOP Discord](https://discord.gg/fbFCkYabZB) và cho chúng tôi biết suy nghĩ của bạn!

Ngoài ra, như chúng ta đã thảo luận trước đó về cách gọi hàm `next`. Chúng tôi có các đối số sau đây mà chúng tôi có thể chuyển cho nó:

1. Không có đối số `next()` - Sẽ chuyển quyền điều khiển cho hàm middleware tiếp theo. Rất đơn giản và dễ hiểu.
1. Với đối số lỗi `next(new Error(...))` - Sẽ chuyển quyền điều khiển trực tiếp đến hàm middleware lỗi.
1. Với chuỗi `next('route')` - Sẽ chuyển quyền điều khiển cho bộ xử lý tuyến tiếp theo có cùng đường dẫn khớp (nếu có). Điều này chỉ hoạt động với `app.METHOD` hoặc `router.METHOD`. Có khả năng, nó cũng có thể giống như việc chỉ gọi `next` mà không có đối số.
1. Với chuỗi `next('router')` - Sẽ bỏ qua tất cả các chức năng của middleware được gắn vào phiên bản router cụ thể và chuyển quyền điều khiển trở lại phiên bản router. Về cơ bản, chúng tôi thoát khỏi router và quay trở lại router chính, ví dụ: `app` (vâng, ứng dụng Express cũng chỉ là một router ngầm).

Trong số bốn cái, chúng tôi có thể sẽ chỉ sử dụng hai cái đầu tiên, trừ khi chúng tôi có nhu cầu rất cụ thể yêu cầu hai cái còn lại.

### Củng cố những gì chúng ta đã học được

Sau đây là cấu trúc thư mục mà bạn sẽ có nếu bạn đã viết mã:

```text
express-app/
├─ errors/
│  ├─ CustomNotFoundError.js
├─ controllers/
│  ├─ authorController.js
├─ routes/
│  ├─ authorRouter.js
│  ├─ ... other routers
├─ app.js
├─ db.js
```

Như một bài tập, vui lòng tạo thêm dữ liệu mẫu trong tệp `db.js` và controller cho các route khác mà chúng tôi đã tạo trước đó.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bài viết này [Express Middlewares, Demystified](https://medium.com/@viral_shah/express-middlewares-demystified-f0c2c37ea6a1) Viết bởi Viral Shah sẽ giúp hiểu sâu hơn về cách thức hoạt động của middleware. Bài viết cũng khuyến khích bạn đọc mã nguồn của Express mà bây giờ bạn có thể làm được.

<div class="lesson-note" markdown="1">

### # Trình xử lý không đồng bộ

Trong khi đọc bài viết đính kèm ở trên, bạn có thể nhận thấy đoạn này:

>— Express không được xây dựng để chờ đợi các trình xử lý hoặc xử lý các lời hứa được trả lại. Do đó, khi hàm xử lý là hàm không đồng bộ, nó sẽ trả về một lời hứa. Khi chúng tôi đưa ra một lỗi, chúng tôi chỉ đơn giản là từ chối lời hứa đó. Vì vậy, cách duy nhất để vượt qua lỗi là thông qua chức năng 'next(err)'

Kể từ phiên bản hiện tại của Express (v5), trình xử lý route không đồng bộ hiện được hỗ trợ nguyên bản. Xin lưu ý rằng bài viết này đã lỗi thời về vấn đề này.

</div>

1. Xem đoạn ngắn này trong 10 phút [video hướng dẫn về mẫu MVC](https://www.youtube.com/watch?v=Cgvopu9zg8Y). Không có gì nhiều ngoài việc củng cố những gì bạn đã biết.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- Tài liệu Express về [Sử dụng Express Middleware](https://expressjs.com/en/guide/using-middleware.html) là một tài liệu ngắn có cùng thông tin nhưng có nhiều ví dụ hơn mà bạn có thể xem qua.
