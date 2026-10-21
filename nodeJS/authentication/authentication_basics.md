### Giới thiệu

Tạo người dùng và cho phép họ đăng nhập, đăng xuất khỏi ứng dụng web là một chức năng quan trọng mà cuối cùng chúng ta cũng sẵn sàng tìm hiểu! Có khá nhiều bước thiết lập, nhưng may mắn là không bước nào quá phức tạp. Bạn sẽ nhanh chóng có một ứng dụng chạy được! Trong bài học này, chúng ta sẽ sử dụng [Passport.js](https://www.passportjs.org), một middleware tuyệt vời giúp xử lý việc xác thực và session.

Chúng ta sẽ xây dựng một ứng dụng Express tối giản cho phép người dùng đăng ký, đăng nhập và đăng xuất. Để minh họa dễ hơn, hiện tại chúng ta sẽ giữ mọi thứ, ngoại trừ các view, trong một tệp. Tuy nhiên, trong dự án thực tế, cách làm tốt nhất là tách các mối quan tâm và chức năng thành những mô-đun riêng.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiểu rõ thứ tự sử dụng các middleware cần thiết cho Passport.js.
- Mô tả chiến lược Passport.js là gì.
- Sử dụng LocalStrategy để xác thực người dùng.
- Giải thích mục đích của cookie trong xác thực.
- Xem lại tài liệu học tập trước đó (route, mẫu, middleware, async/await và lời hứa).
- Sử dụng Passport.js để thiết lập xác thực người dùng bằng Express.
- Mô tả bcrypt là gì và công dụng của nó.
- Mô tả hàm băm là gì và giải thích tầm quan trọng của việc băm mật khẩu.
- Mô tả chức năng `compare` của bcrypt.

### Cài đặt

Trước khi bắt đầu, hãy tạo cơ sở dữ liệu mới trong `psql`.

Để bắt đầu, hãy thiết lập bảng `users`. Vì vậy, thay vì `usernames` chúng ta sẽ tạo bảng `users`.

```sql
CREATE TABLE users (
   id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
   username VARCHAR ( 255 ),
   password VARCHAR ( 255 )
);
```

Tiếp theo, hãy thiết lập một ứng dụng Express tối giản. Tạo một thư mục mới và sử dụng `npm init` để khởi tạo tệp package.json, sau đó chạy lệnh sau để cài đặt mọi dependency cần thiết:

```bash
npm install express express-session pg passport passport-local ejs
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Bảo mật mật khẩu

Hiện tại, chúng tôi đang lưu người dùng của mình chỉ bằng một mật khẩu văn bản đơn giản. Đây là một ý tưởng *thực sự* tồi tệ đối với bất kỳ dự án thực tế nào. Ở cuối bài học này, bạn sẽ học cách bảo mật đúng cách các mật khẩu này bằng bcrypt. Đừng bỏ qua phần đó.

</div>

```javascript
/////// app.js

const path = require("node:path");
const { Pool } = require("pg");
const express = require("express");
const session = require("express-session");
const passport = require("passport");
const LocalStrategy = require('passport-local').Strategy;

const pool = new Pool({
  // add your configuration
});

const app = express();
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

app.use(session({ secret: "cats", resave: false, saveUninitialized: false }));
app.use(passport.session());
app.use(express.urlencoded({ extended: false }));

app.get("/", (req, res) => res.render("index"));

app.listen(3000, (error) => {
  if (error) {
    throw error;
  }
  console.log("app listening on port 3000!");
});
```

Hầu hết những điều này hiện giờ có vẻ quen thuộc với bạn, ngoại trừ các mô-đun mới được yêu cầu là `express-session` và `passport`. Chúng tôi thực sự sẽ không trực tiếp sử dụng `express-session`, đây là một phần phụ thuộc được `passport.js` sử dụng ở chế độ nền. Bạn có thể [xem gói express-session hoạt động như thế nào trong tài liệu Express](https://github.com/expressjs/session).

Công cụ xem của chúng tôi đang tìm kiếm `index.ejs` bên trong `views`, vì vậy hãy tiếp tục và tạo nó:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Home</title>
</head>
<body>
  <h1>hello world!</h1>
</body>
</html>
```

### Tạo người dùng

Điều đầu tiên chúng tôi cần là một biểu mẫu đăng ký để chúng tôi thực sự có thể tạo người dùng để xác thực! Để cho ngắn gọn, chúng ta sẽ bỏ qua việc làm sạch và xác nhận ở đây. Nhưng đừng quên khi đạt đến điểm đó, chúng ta sẽ có cơ hội áp dụng chúng sau này.

Tạo một mẫu mới có tên `sign-up-form` và route cho `/sign-up` trỏ đến nó:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Sign Up</title>
</head>
<body>
  <h1>Sign Up</h1>
  <form action="/sign-up" method="POST">
    <label for="username">Username</label>
    <input id="username" name="username" placeholder="username" type="text" />
    <label for="password">Password</label>
    <input id="password" name="password" type="password" />
    <button type="submit">Sign Up</button>
  </form>
</body>
</html>
```

```javascript
//// app.js

app.get("/sign-up", (req, res) => res.render("sign-up-form"));
```

Tiếp theo, tạo `app.post` cho biểu mẫu đăng ký để chúng tôi có thể thêm người dùng vào cơ sở dữ liệu của mình (hãy nhớ các lưu ý của chúng tôi về việc làm sạch và sử dụng văn bản thuần túy để lưu trữ mật khẩu...).

```javascript
app.post("/sign-up", async (req, res, next) => {
  try {
    await pool.query("INSERT INTO users (username, password) VALUES ($1, $2)", [
      req.body.username,
      req.body.password,
    ]);
    res.redirect("/");
  } catch(err) {
    return next(err);
  }
});
```

Hãy nhắc lại: đây không phải là cách đặc biệt an toàn để tạo người dùng trong cơ sở dữ liệu của bạn... NHƯNG bây giờ bạn có thể truy cập `/sign-up` và gửi biểu mẫu. Nếu mọi việc suôn sẻ, nó sẽ chuyển hướng bạn đến chỉ mục và bạn sẽ có thể xem người dùng mới tạo trong cơ sở dữ liệu của mình. Mở cơ sở dữ liệu của bạn trong `psql` và chạy truy vấn để xem người dùng đầu tiên của bạn!

### Xác thực

Bây giờ chúng ta có khả năng đưa người dùng vào cơ sở dữ liệu của mình, hãy cho phép họ đăng nhập để xem thông báo đặc biệt trên trang chủ của chúng ta! Chúng ta sẽ thực hiện từng bước một của quy trình, nhưng trước tiên, hãy dành một phút để xem qua [trang web Passport.js](http://www.passportjs.org/). Tài liệu ở đây có khá nhiều thứ bạn cần để thiết lập. Bạn sẽ muốn tham khảo lại điều này khi thực hiện dự án của mình.

Passport.js sử dụng cái mà họ gọi là *Chiến lược* để xác thực người dùng. Họ có hơn 500 chiến lược như vậy, nhưng chúng tôi sẽ tập trung vào chiến lược cơ bản nhất (và phổ biến nhất), tên người dùng và mật khẩu hoặc cái mà họ gọi là `LocalStrategy` ([tài liệu cho LocalStrategy](http://www.passportjs.org/docs/username-password/)). Chúng tôi đã cài đặt và yêu cầu các mô-đun thích hợp, vì vậy hãy thiết lập nó!

Chúng ta cần thêm 3 hàm vào tệp app.js, sau đó thêm `app.post` cho đường dẫn `/log-in` của chúng ta.

#### Chức năng một: thiết lập LocalStrategy

```javascript
passport.use(
  new LocalStrategy(async (username, password, done) => {
    try {
      const { rows } = await pool.query("SELECT * FROM users WHERE username = $1", [username]);
      const user = rows[0];

      if (!user) {
        return done(null, false, { message: "Incorrect username" });
      }
      if (user.password !== password) {
        return done(null, false, { message: "Incorrect password" });
      }
      return done(null, user);
    } catch(err) {
      return done(err);
    }
  })
);
```

Hàm này sẽ được gọi khi chúng ta sử dụng hàm `passport.authenticate()` sau này.  Về cơ bản, nó lấy tên người dùng và mật khẩu, cố gắng tìm người dùng trong DB của chúng tôi và sau đó đảm bảo rằng mật khẩu của người dùng khớp với mật khẩu đã cho. Nếu tất cả những điều đó đều thành công (có một người dùng trong DB và mật khẩu trùng khớp) thì nó sẽ xác thực người dùng của chúng tôi và tiếp tục! Chúng tôi sẽ không gọi trực tiếp hàm này nên bạn sẽ không phải cung cấp hàm `done`.  Chức năng này hoạt động hơi giống một middleware và sẽ được gọi cho chúng tôi khi chúng tôi yêu cầu Passport thực hiện xác thực sau này.

### Chức năng hai và ba: phiên và tuần tự hóa

Để đảm bảo người dùng của chúng tôi đã đăng nhập và cho phép họ *duy trì* đăng nhập khi họ di chuyển xung quanh ứng dụng của chúng tôi, Passport gọi nội bộ một hàm từ `express-session` sử dụng một số dữ liệu để tạo cookie có tên `connect.sid` được lưu trữ trong trình duyệt của người dùng. Hai hàm tiếp theo này xác định bit thông tin mà Passport đang tìm kiếm khi nó tạo và sau đó giải mã cookie. Lý do họ yêu cầu chúng tôi xác định các hàm này là để chúng tôi có thể đảm bảo rằng mọi dữ liệu mà nó đang tìm kiếm đều thực sự tồn tại trong Cơ sở dữ liệu của chúng tôi! `passport.serializeUser` thực hiện một cuộc gọi lại chứa thông tin chúng tôi muốn lưu trữ trong dữ liệu phiên. `passport.deserializeUser` được gọi khi truy xuất một phiên, trong đó nó sẽ trích xuất dữ liệu mà chúng tôi đã "tuần tự hóa" trong đó, sau đó đính kèm thứ gì đó vào thuộc tính `.user` của đối tượng yêu cầu (`req.user`) để sử dụng trong phần còn lại của yêu cầu.

Vì mục đích của chúng tôi, các chức năng được liệt kê trong tài liệu Passport sẽ hoạt động tốt:

```javascript
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser(async (id, done) => {
  try {
    const { rows } = await pool.query("SELECT * FROM users WHERE id = $1", [id]);
    const user = rows[0];

    done(null, user);
  } catch(err) {
    done(err);
  }
});
```

Khi một phiên được tạo, `passport.serializeUser` sẽ nhận đối tượng người dùng được tìm thấy từ lần đăng nhập thành công và lưu trữ thuộc tính id của nó trong dữ liệu phiên. Theo một số yêu cầu khác, nếu tìm thấy phiên phù hợp cho yêu cầu đó, `passport.deserializeUser` sẽ truy xuất id mà chúng tôi đã lưu trữ trong dữ liệu phiên. Sau đó, chúng tôi sử dụng id đó để truy vấn cơ sở dữ liệu của mình để tìm người dùng được chỉ định, sau đó `done(null, user)` gắn đối tượng người dùng đó vào `req.user`. Bây giờ trong phần còn lại của yêu cầu, chúng tôi có quyền truy cập vào đối tượng người dùng đó thông qua `req.user`.

Một lần nữa, chúng ta sẽ không tự gọi các hàm này mà chỉ cần xác định chúng, chúng được Passport sử dụng ở chế độ nền.

### Mẫu đăng nhập

Hãy tiếp tục và thêm biểu mẫu đăng nhập trực tiếp vào mẫu chỉ mục của chúng tôi. Biểu mẫu sẽ trông giống như biểu mẫu đăng ký của chúng tôi, nhưng thay vì `POST` chuyển thành `/sign-up`, chúng tôi sẽ thêm `action` vào biểu mẫu để nó chuyển từ `POST` thành `/log-in`. Thêm phần sau vào mẫu chỉ mục của bạn:

```html
<h1>please log in</h1>
<form action="/log-in" method="POST">
  <label for="username">Username</label>
  <input id="username" name="username" placeholder="username" type="text" />
  <label for="password">Password</label>
  <input id="password" name="password" type="password" />
  <button type="submit">Log In</button>
</form>
```

... và bây giờ là phần phép thuật! Thêm route này vào tệp `app.js` của bạn:

```javascript
app.post(
  "/log-in",
  passport.authenticate("local", {
    successRedirect: "/",
    failureRedirect: "/",
    failureMessage: true,
  })
);
```

Như bạn có thể thấy, tất cả những gì chúng ta phải làm là gọi `passport.authenticate()`. Middleware này thực hiện nhiều chức năng ở hậu trường. Trong số những thứ khác, nó xem xét nội dung yêu cầu cho các tham số có tên `username` và `password`, sau đó chạy hàm `LocalStrategy` mà chúng ta đã xác định trước đó để xem tên người dùng và mật khẩu có trong cơ sở dữ liệu hay không. Sau đó, nó tạo một cookie phiên được lưu trữ trong trình duyệt của người dùng và được sử dụng trong tất cả các yêu cầu trong tương lai để xem liệu người dùng đó có đăng nhập hay không. Nó cũng có thể chuyển hướng bạn đến các tuyến khác nhau dựa trên việc đăng nhập thành công hay thất bại. Để truy cập bất kỳ thông báo lỗi nào mà chúng tôi đã xác định trước đó, chúng tôi có thể chuyển `failureMessage` một boolean và truy cập nó sau trong middleware tiếp theo thông qua mảng `req.session.messages`.

Nếu chúng tôi có một trang đăng nhập riêng, chúng tôi có thể muốn quay lại trang đó nếu đăng nhập không thành công hoặc chúng tôi có thể muốn đưa người dùng đến trang tổng quan người dùng của họ nếu đăng nhập thành công. Vì chúng tôi đang giữ mọi thứ trong chỉ mục nên chúng tôi muốn quay lại `/` bất kể điều gì.

Nếu bạn điền và gửi biểu mẫu ngay bây giờ, mọi thứ sẽ hoạt động về mặt kỹ thuật, nhưng bạn thực sự sẽ không THẤY bất kỳ điều gì khác biệt trên trang... hãy khắc phục điều đó.

Middleware Passport sẽ kiểm tra xem có người dùng nào đăng nhập hay không (bằng cách kiểm tra các cookie đi kèm với đối tượng `req`) và nếu có, nó sẽ thêm người dùng đó vào đối tượng yêu cầu cho chúng tôi.  Vì vậy, tất cả những gì chúng ta cần làm là kiểm tra xem `req.user` có thay đổi view của chúng ta hay không tùy thuộc vào việc người dùng có đăng nhập hay không.

Chỉnh sửa `app.get("/")` của bạn để gửi đối tượng người dùng tới view của chúng tôi như sau:

```javascript
app.get("/", (req, res) => {
  res.render("index", { user: req.user });
});
```

và sau đó chỉnh sửa view của bạn để sử dụng đối tượng đó như thế này:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Log In</title>
</head>
<body>
  <% if (locals.user) {%>
    <h1>WELCOME BACK <%= user.username %></h1>
    <a href="/log-out">LOG OUT</a>
  <% } else { %>
    <h1>please log in</h1>
    <form action="/log-in" method="POST">
      <label for="username">Username</label>
      <input id="username" name="username" placeholder="username" type="text" />
      <label for="password">Password</label>
      <input id="password" name="password" type="password" />
      <button type="submit">Log In</button>
    </form>
  <%}%>
</body>
</html>
```

Vì vậy, mã này sẽ kiểm tra xem liệu có người dùng nào được xác định hay không... nếu có, nó sẽ đưa ra thông báo chào mừng và nếu KHÔNG thì nó sẽ hiển thị biểu mẫu đăng nhập.  Gọn gàng!

Bước cuối cùng... hãy làm cho liên kết đăng xuất đó thực sự có tác dụng với chúng ta. Như bạn có thể thấy, nó đang đưa chúng ta đến `/log-out`, vì vậy tất cả những gì chúng ta cần làm là thêm một route cho điều đó trong app.js.  Thuận tiện, middleware Passport thêm chức năng đăng xuất vào đối tượng `req`, vì vậy việc đăng xuất dễ dàng như thế này:

```javascript
app.get("/log-out", (req, res, next) => {
  req.logout((err) => {
    if (err) {
      return next(err);
    }
    res.redirect("/");
  });
});
```

Bây giờ bạn có thể truy cập `/sign-up` để tạo người dùng mới, sau đó đăng nhập bằng tên người dùng và mật khẩu của người dùng đó, sau đó đăng xuất bằng cách nhấp vào nút đăng xuất!

<div class="lesson-note lesson-note--tip" markdown="1">

#### Mẹo nhanh

Nói cách khác, bạn có thể đặt và truy cập các biến cục bộ khác nhau trong toàn bộ ứng dụng của mình (ngay cả trong view) bằng đối tượng `locals`. Chúng ta có thể sử dụng kiến ​​thức này để viết cho mình một middleware tùy chỉnh nhằm đơn giản hóa cách chúng ta truy cập vào người dùng hiện tại trong view của mình.

Các hàm middleware là các hàm lấy các đối tượng `req` và `res`, thao tác với chúng và chuyển chúng qua phần còn lại của ứng dụng.

```javascript
app.use((req, res, next) => {
  res.locals.currentUser = req.user;
  next();
});
```

Nếu bạn chèn mã này vào đâu đó giữa nơi bạn khởi tạo middleware Passport và trước khi hiển thị các view của mình, bạn sẽ có quyền truy cập vào biến `currentUser` trong tất cả các view của mình và bạn sẽ không phải chuyển mã đó vào tất cả các controller mà bạn cần theo cách thủ công.

</div>

### Bảo mật mật khẩu bằng bcrypt

Bây giờ, hãy quay lại và tìm hiểu cách lưu trữ mật khẩu người dùng một cách an toàn để nếu có sự cố xảy ra hoặc nếu ai đó có quyền truy cập vào cơ sở dữ liệu của chúng tôi, mật khẩu người dùng của chúng tôi sẽ được an toàn.  Điều này *cực kỳ* quan trọng, ngay cả đối với những ứng dụng cơ bản nhất.

`npm install bcryptjs` đầu tiên. Có một mô-đun khác tên là `bcrypt` thực hiện chức năng tương tự, nhưng nó được viết bằng C++ và đôi khi rất khó cài đặt. C++ `bcrypt` nhanh hơn về mặt kỹ thuật, vì vậy trong tương lai có thể đáng để chạy nó, nhưng hiện tại, các mô-đun hoạt động giống nhau nên chúng ta chỉ có thể sử dụng `bcryptjs`.

Sau khi cài đặt xong, bạn cần yêu cầu nó ở đầu app.js của mình và sau đó chúng tôi sẽ đưa nó vào sử dụng ở nơi chúng tôi lưu mật khẩu vào DB và nơi chúng tôi so sánh chúng trong LocalStrategy.

#### Lưu trữ mật khẩu băm

Băm mật khẩu là kết quả của việc chuyển mật khẩu của người dùng thông qua hàm băm một chiều, hàm này ánh xạ các đầu vào có kích thước thay đổi thành các đầu ra giả ngẫu nhiên có kích thước cố định.

Chỉnh sửa `app.post("/sign-up")` của bạn để sử dụng hàm bcrypt.hash hoạt động như sau:

```javascript
//Don't forget to import bcrypt!
const bcrypt = require("bcryptjs");

//...

app.post("/sign-up", async (req, res, next) => {
 try {
  const hashedPassword = await bcrypt.hash(req.body.password, 10);
  await pool.query("INSERT INTO users (username, password) VALUES ($1, $2)", [req.body.username, hashedPassword]);
  res.redirect("/");
 } catch (error) {
    console.error(error);
    next(error);
   }
});
```

Đối số thứ hai là độ dài của "muối" được sử dụng trong hàm băm; thêm muối cho mật khẩu có nghĩa là thêm các ký tự ngẫu nhiên vào mật khẩu, mật khẩu cộng với các ký tự ngẫu nhiên bổ sung sau đó sẽ được đưa vào hàm băm. Việc tạo muối được sử dụng để tạo đầu ra băm mật khẩu duy nhất, ngay cả đối với những người dùng sử dụng cùng một mật khẩu và để bảo vệ khỏi [bảng cầu vồng](https://en.wikipedia.org/wiki/Rainbow_table) và [tấn công từ điển](https://en.wikipedia.org/wiki/Dictionary_attack).

Thông thường, muối được lưu trữ trong cơ sở dữ liệu cùng với giá trị băm. Tuy nhiên, trong trường hợp của chúng tôi, không cần phải lưu trữ muối riêng biệt vì thuật toán băm `bcryptjs` tự động kết hợp muối trong chính hàm băm.

Hàm băm hơi chậm nên tất cả nội dung lưu trữ DB cần được đưa vào bên trong hàm gọi lại. Kiểm tra xem liệu tính năng này có hoạt động hay không bằng cách đăng ký người dùng mới bằng mật khẩu, sau đó xem các mục nhập DB của bạn để xem nó được lưu trữ như thế nào. Nếu bạn làm đúng, mật khẩu của bạn sẽ được chuyển thành một chuỗi ngẫu nhiên thực sự dài.

Điều quan trọng cần lưu ý là *cách* hoạt động của hàm băm, đặc biệt là trong bối cảnh mật khẩu, nằm ngoài phạm vi của bài học này.

#### So sánh mật khẩu đã băm

Chúng tôi sẽ sử dụng chức năng `bcrypt.compare()` để xác thực việc nhập mật khẩu. Hàm so sánh mật khẩu văn bản thuần túy trong đối tượng yêu cầu với mật khẩu băm.

Bên trong hàm `LocalStrategy` của bạn, chúng ta cần thay thế biểu thức `user.password !== password` bằng hàm `bcrypt.compare()`.

```javascript
const match = await bcrypt.compare(password, user.password);
if (!match) {
  // passwords do not match!
  return done(null, false, { message: "Incorrect password" })
}
```

Bây giờ bạn có thể đăng nhập bằng người dùng mới mà bạn đã tạo (người dùng có mật khẩu băm).  Thật không may, những người dùng đã được lưu TRƯỚC KHI bạn thêm bcrypt sẽ không còn hoạt động nữa, nhưng đó chỉ là một cái giá nhỏ phải trả cho sự bảo mật! (và một lý do chính đáng để đưa bcrypt vào ngay từ đầu trong dự án tiếp theo của bạn)

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Xem video 1, 2, 3, 5 và 6 của [Danh sách phát Youtube về các phiên trong Express và xác thực chiến lược cục bộ với Passport.js](https://www.youtube.com/playlist?list=PLYQSCk-qyTW2ewJ05f_GKHtTIzjynDgjK).
   - Các bạn có thể để ý ở một số điểm trong video, ứng dụng Express có chứa dòng `app.use(passport.initialize())`. Dòng này không còn bắt buộc phải có trong các phiên bản Passport hiện tại nữa.
   - Họ đang sử dụng MongoDB và bạn có thể thay thế bất kỳ phiên bản nào của nó bằng PostgreSQL.
   - Trong [video 3: "Hướng dẫn đầy đủ để bạn hiểu về thư viện phiên cấp tốc"](https://youtu.be/J1qXK66k1y4?list=PLYQSCk-qyTW2ewJ05f_GKHtTIzjynDgjK) và [video 5: "Cấu hình cục bộ Passport (Node + Passport + Express)"](https://youtu.be/xMEOT9J0IvI?list=PLYQSCk-qyTW2ewJ05f_GKHtTIzjynDgjK), nó hiển thị việc sử dụng thư viện `connect-mongo` để sử dụng kết nối MongoDB của bạn để lưu trữ các phiên, thay vì lưu trữ chúng trong bộ nhớ. Tuy nhiên, vì chúng ta đang sử dụng PostgreSQL nên chúng ta cần thay thế nó bằng một thư viện có tên `connect-pg-simple`. Bạn có thể xem cách triển khai để thực hiện việc này trên [trang npm dành cho connect-pg-simple](https://www.npmjs.com/package/connect-pg-simple).
   - Xin lưu ý rằng bảng cần sử dụng cho kho lưu trữ phiên không được tạo tự động theo mặc định, hãy nhớ kiểm tra các tùy chọn có sẵn trong trang npm của chúng.
1. Trong [Passport: Sổ tay hướng dẫn ẩn](https://github.com/jwalton/passport-api-docs), bạn có thể khám phá những giải thích toàn diện hơn về một số chức năng chính của Passport, hiểu sâu hơn về chức năng của từng chức năng.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- Video này cung cấp cái nhìn tổng quan về một số [các phương pháp khác nhau để lưu trữ mật khẩu trong cơ sở dữ liệu và những rủi ro có thể xảy ra](https://www.youtube.com/watch?v=8ZtInClXe1Q).
- Nếu bạn muốn tìm hiểu sâu hơn một chút về băm mật khẩu, hãy đọc bài viết Wikipedia sau đây để [tìm hiểu thêm về cách hoạt động của hàm băm mật mã](https://en.wikipedia.org/wiki/Cryptographic_hash_function).
