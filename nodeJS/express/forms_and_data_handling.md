### Giới thiệu

Trong bài học này, chúng ta sẽ đi sâu vào các biểu mẫu và cách quản lý chúng bằng Express. Chúng ta sẽ khám phá các cách bảo mật ứng dụng Node.js / Express của bạn bằng cách sử dụng `express-validator` để xác thực và làm sạch. Hiểu cách xử lý biểu mẫu thích hợp là rất quan trọng để duy trì tính toàn vẹn dữ liệu và bảo vệ các ứng dụng web của bạn khỏi các rủi ro bảo mật.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiểu các thành phần biểu mẫu HTML và hành động của chúng.
- Sử dụng `express-validator` để xác thực và làm sạch.
- Thực tiễn tốt nhất về thiết kế biểu mẫu và tính toàn vẹn dữ liệu.

### Tổng quan về biểu mẫu HTML

Đầu tiên, hãy giới thiệu ngắn gọn về HTML thực tế.

Hãy tạo một biểu mẫu HTML đơn giản, với một trường văn bản duy nhất để thu thập tên đầy đủ và thành phần nhãn liên quan của nó.

Cấu trúc HTML sẽ trông giống như thế này:

```html
<form action="/create" method="POST">
  <label for="fullName">Full Name:</label>
  <input placeholder="John Doe" type="text" name="fullName" id="fullName">
  <button type="submit">Submit</button>
</form>
```

Chúng tôi có chính phần tử biểu mẫu HTML, với một hành động trỏ đến một số tài nguyên trên máy chủ của chúng tôi và một phương thức được xác định. Lưu ý cách phương thức này tương ứng với một động từ HTTP, thường là `GET` hoặc `POST`.

Biểu mẫu bao gồm nội dung nhập văn bản có nhãn liên quan và nút gửi.

Thuộc tính `name` của đầu vào đóng một vai trò quan trọng vì nó xác định cách xác định đầu vào của chúng tôi trong dữ liệu biểu mẫu được gửi đến máy chủ của chúng tôi. Điều này đặc biệt quan trọng khi xử lý việc gửi biểu mẫu ở phần phụ trợ.

Sau đó, nút `type="submit"` cho phép người dùng tải dữ liệu đã nhập lên máy chủ.

Thuộc tính `form` xác định cách giao tiếp với máy chủ:

- `action`: Tài nguyên/URL nơi dữ liệu được gửi để xử lý khi biểu mẫu được gửi. Nếu điều này không được đặt hoặc là một chuỗi trống, biểu mẫu sẽ được gửi trở lại URL trang hiện tại
- `method`: Xác định phương thức HTTP sẽ sử dụng (`POST` hoặc `GET`).

`POST` thường an toàn hơn vì nó loại bỏ thông tin nhạy cảm khỏi URL, điều đó có nghĩa là chúng sẽ không hiển thị trong nhật ký máy chủ và là lựa chọn tiêu chuẩn để tạo hoặc cập nhật dữ liệu ở phía máy chủ. `GET` dành cho các biểu mẫu không sửa đổi dữ liệu, chẳng hạn như biểu mẫu tìm kiếm hoặc khi bạn muốn việc gửi biểu mẫu có thể đánh dấu trang hoặc có thể chia sẻ qua URL. Dữ liệu biểu mẫu ở đây được gửi dưới dạng chuỗi truy vấn như một phần của url yêu cầu.

#### Quy trình xử lý biểu mẫu

`action` của biểu mẫu sẽ trỏ đến một số điểm cuối trên máy chủ của chúng tôi, điều này sẽ cho phép controller của chúng tôi xử lý yêu cầu. Controller sau đó giao tiếp với cơ sở dữ liệu để xử lý dữ liệu.

Sau đó, chúng tôi tạo view mới hoặc cập nhật với phản hồi của controller và chuyển hướng máy khách. Đây được gọi là mẫu thiết kế [Đăng/Chuyển hướng/Nhận (PRG)](https://en.wikipedia.org/wiki/Post/Redirect/Get) giúp ngăn chặn các yêu cầu `POST` trùng lặp.

### Xác thực và làm sạch

Trước khi dữ liệu từ một biểu mẫu được gửi đến máy chủ của chúng tôi, chúng tôi nên xem xét hai bước quan trọng:

- *Xác thực* đảm bảo thông tin đầu vào của người dùng đáp ứng các tiêu chí đã chỉ định (ví dụ: các trường bắt buộc, định dạng đúng).
- *Làm sạch* làm sạch đầu vào của người dùng để ngăn chặn việc xử lý dữ liệu độc hại bằng cách xóa hoặc mã hóa các ký tự có khả năng độc hại.

Không phải lúc nào chúng ta cũng phải làm sạch dữ liệu ngay khi nhận được - đôi khi, thay vào đó, việc làm sạch ngay trước khi sử dụng dữ liệu đó là điều hợp lý.

Chúng tôi sẽ sử dụng thư viện có tên `express-validator` để giúp chúng tôi giải quyết cả hai điều này. Mặc dù nó làm cho các quy trình này đơn giản hơn nhiều nhưng điều quan trọng là phải hiểu các khái niệm cơ bản của hai thao tác này.

<div class="lesson-note" markdown="1">

#### Định nghĩa làm sạch

Đôi khi bạn sẽ thấy "làm sạch" được sử dụng theo nghĩa chặt chẽ hơn nhiều, chỉ liên quan đến việc loại bỏ các ký tự khỏi dữ liệu đến, phân biệt nó với "mã hóa" đang thay thế một số ký tự nhất định. Đôi khi nó cũng được sử dụng một cách lỏng lẻo hơn để chỉ toàn bộ quá trình thay đổi dữ liệu đến theo một cách nào đó, bao gồm cả việc loại bỏ và mã hóa các ký tự. Đây là cách chúng ta sẽ sử dụng thuật ngữ này trong nội dung của bài học này.

</div>

#### Cài đặt express-validator

Chúng tôi bắt đầu như thường lệ bằng cách cài đặt đúng gói trong thư mục *root* của dự án.

```bash
npm install express-validator
```

Để bắt đầu sử dụng gói, trước tiên chúng ta cần `require` nó trong dự án của mình

```javascript
const { body, validationResult } = require("express-validator");
```

`express-validator` có nhiều hàm cho hầu hết mọi thao tác trên biểu mẫu mà bạn có thể nghĩ tới, nhưng đối với bài học này, chúng ta sẽ tập trung vào `body()` và `validationResult()`.

### Tìm hiểu hàm body()

Hàm `body()` cho phép bạn chỉ định trường nào trong nội dung yêu cầu cần được xác thực và làm sạch cũng như cách xử lý trường đó.

```javascript
[
  body("birthdate", "Must be a valid date.")
    .optional({ values: "falsy" })
    .isISO8601() // Enforce a YYYY-MM-DD format.
];
```

Ví dụ này đánh dấu trường `birthdate` là tùy chọn nhưng vẫn thực thi định dạng ngày ISO8601 trên đầu vào. Điều này là do `{ values: "falsy" }` có nghĩa là các giá trị không phải là `undefined`, `null`, `false`, `0` hoặc các chuỗi trống `""` vẫn sẽ được xác thực.

### Xác thực chuỗi

Bạn cũng có thể xâu chuỗi nhiều phương thức xác thực với các thông báo lỗi duy nhất nếu quá trình kiểm tra không thành công.

```javascript
[
  body("name")
    .trim()
    .notEmpty()
    .withMessage("Name can not be empty.")
    .isAlpha()
    .withMessage("Name must only contain alphabet letters."),  
];
```

Điều này đảm bảo rằng `name` không chỉ hiện diện và được cắt bớt mà còn chỉ chứa các chữ cái trong bảng chữ cái.

### Thoát khỏi đầu vào của người dùng

Mặc dù điều này có thể hiệu quả đối với các kết quả đầu ra mà chúng tôi biết sẽ không có các ký tự đặc biệt, như tên hoặc tuổi, nhưng chúng tôi cũng phải xem xét các tình huống cho phép các ký tự đó. Ví dụ: khi viết mô tả "Giới thiệu về tôi", điều gì sẽ xảy ra nếu khách hàng quyết định chèn mã JavaScript thay thế?

```ejs
<div>
  About Me: <%- description %>!
</div>

// The client then inputs the following as their page's About Me:
<script>alert("Hacked!");</script>
```

Khi không thoát, nó sẽ được hiển thị thành HTML dưới dạng:

```html
<div>
  About Me: <script>alert("Hacked!");</script>!
</div>
```

Để ngăn chặn [cuộc tấn công tập lệnh chéo trang (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting), chúng tôi có thể *thoát* đầu ra (bạn cũng có thể thấy điều này được gọi là *mã hóa*). HTML thoát sẽ thay thế các ký tự đặc biệt, như `<`, bằng các thực thể HTML tương ứng của chúng, trong trường hợp này là `&lt;`. Trong EJS, chúng ta có thể thoát đầu ra bằng `<%= %>`.

```ejs
<div>
  About Me: <%= username %>!
</div>

// The escaped output is now rendered harmless:
// About Me: &lt;script&gt;alert(&quot;Hacked!&quot;);&lt;/script&gt;!
```

Vậy tại sao không thoát dữ liệu khi chúng ta nhận được nó bằng cách thêm .escape() vào cuối chuỗi xác thực body() của chúng ta? "Nhân vật nguy hiểm" chỉ nguy hiểm ở điểm sử dụng và cũng chỉ trong một số bối cảnh nhất định. Điều "nguy hiểm" đối với HTML có thể không nguy hiểm đối với SQL và ngược lại, đồng thời chúng sẽ không gây rủi ro cho đến khi được sử dụng trong các bối cảnh đó.

Ngoài ra, nếu chúng ta có dữ liệu với các thực thể HTML sau khi thoát chúng, nếu chúng ta sử dụng đầu ra thoát với (`<%= %>`), thì `&lt;` sẽ không trở thành `<` mà xuất ra theo nghĩa đen là văn bản `&lt;`. Chúng ta cần phải hủy thoát chúng sau đó thoát lại chúng thông qua đầu ra thoát hoặc cuối cùng sử dụng đầu ra không thoát (`<%- %>`), giống như sử dụng `.innerHTML`, là điều không mong muốn như được hiển thị ở trên.

### Kết quả xác thực

Khi các quy tắc xác thực được áp dụng, bạn có thể sử dụng `validationResult` để xử lý mọi lỗi xác thực:

```javascript
const controller = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).render("index", {
      errors: errors.array(),
    });
  }

  // do stuff if successful
  res.redirect("/success");
};
```

Thiết lập này sẽ kiểm tra mọi lần kiểm tra xác thực không thành công và nếu có bất kỳ lỗi nào (mảng lỗi KHÔNG trống), thì máy chủ sẽ gửi [mã trạng thái 400](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/400), cùng với mọi lỗi có thể xuất hiện, tới view `index` của chúng tôi. Nếu không, chúng tôi sẽ được chuyển hướng đến tuyến `/success` trong router của mình.

### Các route biểu mẫu và Express

Điều cuối cùng cần đề cập là cách xử lý các route trong Express. Rốt cuộc, biểu mẫu của chúng tôi cần một nơi nào đó để gửi dữ liệu đến.

```javascript
exports.userUpdateGet = (req, res, next) => {};
exports.userUpdatePost = (req, res, next) => {};
```

Sau đó, bên trong router của mình, chúng tôi có thể chỉ định các tuyến tương ứng với các chức năng của controller:

```javascript
const { Router } = require("express");
const usersRouter = Router();
const usersController = require("../controllers/usersController");

// User update routes
usersRouter.get("/:id/update", usersController.userUpdateGet);
usersRouter.post("/:id/update", usersController.userUpdatePost);

module.exports = usersRouter;
```

Trong biểu mẫu của chúng tôi, hành động sẽ trông giống như thế này:

```ejs
<!-- Example using EJS with POST to submit an update to our Express server. -->
<form action="/users/<%= user.userId %>/update" method="POST"></form>
```

`/users/:id/update` là *điểm cuối* mà chúng tôi đã tạo trên máy chủ Express của mình.

### Kết hợp nó lại với nhau

Hãy tạo một ứng dụng Express nhanh để trình bày cách máy khách gửi dữ liệu đến máy chủ bằng biểu mẫu.

Thiết lập ứng dụng Express mới với khuôn mẫu EJS:

```bash
npm init -y
npm install express ejs express-validator
```

Tạo thư mục cho `routes`, `views`, `controllers`, `storages` và tệp `app.js`:

```javascript
// app.js
const express = require("express");
const app = express();
const path = require("node:path");
const usersRouter = require("./routes/usersRouter");

app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");
app.use(express.urlencoded({ extended: true }));
app.use("/", usersRouter);

const PORT = process.env.PORT || 3000;
app.listen(PORT, (error) => {
  if (error) {
    throw error;
  }
  console.log(`Express app listening on port ${PORT}!`);
});
```

Hầu hết các biểu mẫu đơn giản sẽ sử dụng tiêu đề HTTP `Content-Type: application/x-www-form-urlencoded` khi gửi dữ liệu đến máy chủ. Tuy nhiên, Express không thể phân tích dữ liệu đó một cách tự nhiên. Chúng tôi có thể sử dụng middleware `express.urlencoded()` để xử lý việc này cho chúng tôi và tự động đặt dữ liệu của biểu mẫu thành trường `req.body`. Khi `extended` là `false`, máy chủ của chúng tôi sẽ chỉ chấp nhận `string` hoặc một mảng dữ liệu, vì vậy chúng tôi đặt nó thành `true` để tăng thêm tính linh hoạt. Lưu ý rằng nếu `Content-Type` không khớp với `application/x-www-form-urlencoded` thì máy chủ của bạn sẽ hiển thị dữ liệu dưới dạng đối tượng trống `{}`.

Hãy tạo một router mới có tên `usersRouter.js` trong thư mục route:

```javascript
// routes/usersRouter.js
const { Router } = require("express");
const usersController = require("../controllers/usersController");
const usersRouter = Router();

usersRouter.get("/", usersController.usersListGet);
usersRouter.get("/create", usersController.usersCreateGet);
usersRouter.post("/create", usersController.usersCreatePost);

module.exports = usersRouter;
```

Tiếp theo chúng ta sẽ tạo hai view:

- `index.ejs` sẽ liệt kê tất cả người dùng mà chúng ta đã tạo.
- `createUser.ejs` sẽ hiển thị biểu mẫu tạo người dùng của chúng tôi.

```ejs
<!-- views/index.ejs -->
<!DOCTYPE html>
<html>
<head>
  <title><%= title %></title>
</head>
<body>
  <h1><%= title %></h1>
  <ul>
    <% if (locals.users) { %>
      <% users.forEach(user => { %>
        <li>
          ID: <%= user.id %>, Name: <%= user.firstName %> <%= user.lastName %>
        </li>
      <% }); %>
    <% } %>
  </ul>
  <a href="/create">Create a user</a>
</body>
</html>
```

```ejs
<!-- views/createUser.ejs -->
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
  </head>
<body>
  <h1><%= title %></h1>
  <form action="/create" method="POST">
    <label for="firstName">First Name: </label>
    <input type="text" name="firstName" id="firstName" required>
    <label for="lastName">Last Name: </label>
    <input type="text" name="lastName" id="lastName" required>
    <button type="submit">Submit</button>
  </form>
  <a href="/">Back to home</a>
</body>
</html>
```

Logic cho router này sẽ nằm bên trong controller của chúng tôi:

```javascript
// controllers/usersController.js
const usersStorage = require("../storages/usersStorage");

exports.usersListGet = (req, res) => {
  res.render("index", {
    title: "User list",
    users: usersStorage.getUsers(),
  });
};

exports.usersCreateGet = (req, res) => {
  res.render("createUser", {
    title: "Create user",
  });
};

exports.usersCreatePost = (req, res) => {
  const { firstName, lastName } = req.body;
  usersStorage.addUser({ firstName, lastName });
  res.redirect("/");
};
```

Và chúng tôi sẽ sử dụng lớp lưu trữ để giữ những người dùng mà chúng tôi tạo. Trong các tình huống thực tế, bạn gần như chắc chắn sẽ sử dụng cơ sở dữ liệu cho việc này và bạn sẽ khám phá thêm cơ sở dữ liệu này trong các bài học sắp tới. Lớp học này chỉ nhằm mục đích trình diễn trước khi chúng ta đến đó.

```javascript
// storages/usersStorage.js
// This class lets us simulate interacting with a database.
class UsersStorage {
  constructor() {
    this.storage = {};
    this.id = 0;
  }

  addUser({ firstName, lastName }) {
    const id = this.id;
    this.storage[id] = { id, firstName, lastName };
    this.id++;
  }

  getUsers() {
    return Object.values(this.storage);
  }

  getUser(id) {
    return this.storage[id];
  }

  updateUser(id, { firstName, lastName }) {
    this.storage[id] = { id, firstName, lastName };
  }

  deleteUser(id) {
    delete this.storage[id];
  }
}
// Rather than exporting the class, we can export an instance of the class by instantiating it.
// This ensures only one instance of this class can exist, also known as the "singleton" pattern.
module.exports = new UsersStorage();
```

Chúng tôi có thể kiểm tra máy chủ của mình bằng: `node --watch app.js`.

Bạn sẽ thấy chúng tôi có thể thêm người dùng mới tại `http://localhost:3000/create`, cũng như liệt kê tất cả người dùng hiện tại tại `http://localhost:3000/`.

Hãy thêm một số phương thức vào `usersController.js` để xác thực và làm sạch biểu mẫu của chúng tôi nhằm nhận được loại dữ liệu mà chúng tôi muốn.

```javascript
// This just shows the new stuff we're adding to the existing contents
const { body, validationResult, matchedData } = require("express-validator");

const alphaErr = "must only contain letters.";
const lengthErr = "must be between 1 and 10 characters.";

const validateUser = [
  body("firstName").trim()
    .isAlpha().withMessage(`First name ${alphaErr}`)
    .isLength({ min: 1, max: 10 }).withMessage(`First name ${lengthErr}`),
  body("lastName").trim()
    .isAlpha().withMessage(`Last name ${alphaErr}`)
    .isLength({ min: 1, max: 10 }).withMessage(`Last name ${lengthErr}`),
];

// We can pass an entire array of middleware validations to our controller.
exports.usersCreatePost = [
  validateUser,
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).render("createUser", {
        title: "Create user",
        errors: errors.array(),
      });
    }
    const { firstName, lastName } = matchedData(req);
    usersStorage.addUser({ firstName, lastName });
    res.redirect("/");
  }
];
```

Chúng tôi truy xuất tất cả dữ liệu đã xác thực thông qua chức năng [`matchedData()`](https://express-validator.github.io/docs/api/matched-data) để đảm bảo tất cả dữ liệu chúng tôi nhận được sẽ bao gồm mọi dữ liệu đã được làm sạch (chẳng hạn như dữ liệu đã được cắt bớt).

Và bây giờ, chúng ta cần cập nhật view `createUser.ejs` để hiển thị những lỗi này. Hãy tạo một phần mới. Bên trong thư mục `views`, tạo một thư mục mới có tên `partials` và bên trong thư mục đó, tạo `errors.ejs`:

```ejs
<!-- views/partials/errors.ejs -->
<% if (locals.errors) {%>
  <ul>
    <% errors.forEach(function(error) { %>
      <li><%= error.msg %></li>
    <% }); %>
  </ul>
<% } %>
```

Và chúng tôi sẽ bao gồm phần ngay phía trên biểu mẫu của chúng tôi trong `createUser.ejs`.

```ejs
<%- include("partials/errors.ejs") %>
```

Nếu biểu mẫu được điền không chính xác, bạn sẽ thấy trang chứa tất cả các lỗi.

Bây giờ chúng ta có thể tạo người dùng, chúng ta cũng cần một cách để sửa đổi họ. Hãy tạo một biểu mẫu cho phép chúng tôi chỉ định ID và các giá trị mà chúng tôi muốn cập nhật bằng cách tạo view mới: `updateUser.ejs`.

```ejs
<!-- views/updateUser.ejs -->
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
  </head>
<body>
  <h1><%= title %></h1>
  <%- include("partials/errors.ejs") %>
  <form action="/<%= user.id %>/update" method="POST">
    <input type="text" name="firstName" value="<%= user.firstName %>" required>
    <input type="text" name="lastName" value="<%= user.lastName %>" required>
    <button type="submit">Update User</button>
  </form>
  <a href="/">Back to home</a>
</body>
</html>
```

Chúng tôi sẽ sửa đổi `index.ejs` để bao gồm nút "Cập nhật" mới bên cạnh mỗi người dùng nhằm thực hiện yêu cầu `GET` của chúng tôi đối với route:

```ejs
<li>
  ID: <%= user.id %>, Name: <%= user.firstName %> <%= user.lastName %>
  <a href="/<%= user.id %>/update">Update</a>
</li>
```

Giống như cách chúng tôi thêm lộ trình tạo người dùng, chúng tôi sẽ thêm lộ trình để cập nhật họ:

```javascript
usersRouter.get("/:id/update", usersController.usersUpdateGet);
usersRouter.post("/:id/update", usersController.usersUpdatePost);
```

Sau đó, chúng ta sẽ thêm logic cho các yêu cầu vào controller của mình:

```javascript
exports.usersUpdateGet = (req, res) => {
  const user = usersStorage.getUser(req.params.id);
  res.render("updateUser", {
    title: "Update user",
    user: user,
  });
};

exports.usersUpdatePost = [
  validateUser,
  (req, res) => {
    const user = usersStorage.getUser(req.params.id);
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).render("updateUser", {
        title: "Update user",
        user: user,
        errors: errors.array(),
      });
    }
    const { firstName, lastName } = matchedData(req);
    usersStorage.updateUser(req.params.id, { firstName, lastName });
    res.redirect("/");
  }
];
```

Bạn sẽ thấy bây giờ bạn có thể cập nhật người dùng và điều này trông rất giống với cách chúng tôi tạo người dùng.

Cuối cùng, hãy thêm một cách để xóa người dùng bằng cách bắt đầu với một biểu mẫu khác trong view `index.ejs` của chúng tôi:

```ejs
<!-- In views/index.ejs -->
<ul>
  <% if (locals.users) { %>
    <% users.forEach(function(user) { %>
      <li>
        ID: <%= user.id %>, Name: <%= user.firstName %> <%= user.lastName %>
        <a href="/<%= user.id %>/update">Update</a>
        <!-- This time we're sending a POST request to our route, so we need a form. -->
        <form action="/<%= user.id %>/delete" method="POST" style="display:inline;">
          <button type="submit" onclick="return confirm('Are you sure you want to delete this user?');">Delete</button>
        </form>
      </li>
    <% }); %>
  <% } %>
</ul>
```

Sau đó, chúng tôi thêm logic để xử lý yêu cầu vào controller của mình:

```javascript
exports.usersDeletePost = (req, res) => {
  usersStorage.deleteUser(req.params.id);
  res.redirect("/");
};
```

Đừng quên thêm route mới vào router của bạn!

```javascript
usersRouter.post("/:id/delete", usersController.usersDeletePost);
```

Bạn sẽ nhận thấy rằng chúng tôi không cần route `GET` ở đây vì chúng tôi đã chuyển hướng đến `/`.

Chúng ta có thể đi sâu hơn vào cách làm việc an toàn với biểu mẫu nhưng chúng ta sẽ dừng ở đó. Đến bây giờ bạn có thể thấy `express-validator` hữu ích như thế nào và bạn có thể làm hầu hết mọi thứ bạn muốn với đối tượng `req.body` như thế nào.

Để cung cấp cho bạn cái nhìn tổng quan về toàn bộ quy trình này trông như thế nào về mặt trực quan, đây là một ví dụ do MDN cung cấp:

![Sơ đồ hiển thị phác thảo về cách một biểu mẫu tương tác với máy chủ bằng cách sử dụng các yêu cầu GET và POST.](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/web_server_form_handling.png)

### Phân công

<div class="lesson-content__panel" markdown="1">

#### Thêm chi tiết người dùng

Mở rộng mô hình `User` mà chúng tôi đã tạo để bao gồm nhiều chi tiết hơn. Triển khai các trường và xác thực sau cho mô hình của bạn:

- Email (bắt buộc, phải có định dạng đúng)
- Tuổi (tùy chọn, phải là số từ 18 đến 120)
- Tiểu sử (tùy chọn, tối đa 200 ký tự)

Đừng quên cập nhật view để hiển thị các trường mới này!

#### Thực hiện tìm kiếm

Điều gì sẽ xảy ra nếu chúng ta muốn tìm kiếm một người dùng cụ thể trong danh sách hàng nghìn người? Chúng tôi sẽ cần một lộ trình và view mới cho phép khách hàng tìm kiếm danh sách người dùng của chúng tôi.

1. Thêm biểu mẫu bằng phương pháp `GET` (trong `createUser.ejs` hoặc view khác) chấp nhận `name` hoặc `email` (hoặc cả hai!)
1. Tạo route mới `/search` chấp nhận yêu cầu `GET`.
1. Thêm logic tìm kiếm vào controller để tìm kiếm người dùng phù hợp trong danh sách của bạn. Dữ liệu biểu mẫu đã được gửi qua yêu cầu `GET` sẽ không có sẵn qua `req.body`. Thay vào đó, bạn sẽ cần sử dụng `req.query`.
   - Yêu cầu `GET` của bạn sẽ xử lý việc tìm kiếm người dùng và sau đó hiển thị kết quả tìm kiếm.
1. Hiển thị kết quả tìm kiếm ở view mới: `search.ejs`.

#### Đọc thêm

- Bài viết này mở rộng về [ý nghĩa của việc làm sạch và thoát cũng như cách chúng có thể giúp bảo mật hơn nữa các ứng dụng web của bạn](https://blog.presidentbeef.com/blog/2020/01/14/injection-prevention-sanitizing-vs-escaping/).
- [express-validator](https://express-validator.github.io/docs/) chứa tài liệu đầy đủ về `validator`, với một số phần quan trọng là:
  - [Bắt đầu với express-validator](https://express-validator.github.io/docs/guides/getting-started)
  - [Chuỗi xác thực](https://express-validator.github.io/docs/guides/validation-chain)

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- Web Dev Simplified có [hướng dẫn gửi biểu mẫu và phân tích dữ liệu biểu mẫu trong Express](https://youtu.be/SccSCuHhOw0?si=2dZ5Y4dvxyh7jpcy) nếu bạn muốn ôn lại.
- tài liệu của express-validator có bài viết hay về [triển khai trình xác thực tùy chỉnh trong Express](https://express-validator.github.io/docs/guides/customizing#implementing-a-custom-validator).
