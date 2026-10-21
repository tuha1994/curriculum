### Giới thiệu

Trước đây, rất nhiều dự án bạn thực hiện sẽ là SPA: tệp HTML được cung cấp về cơ bản sẽ trống với thẻ tập lệnh cho phép máy khách hiển thị trang bằng JavaScript. Bây giờ chúng tôi đang tập trung vào việc hiển thị HTML trên máy chủ, tức là sử dụng máy chủ để tạo các tệp HTML thực sự chứa nội dung của trang.

Chế độ xem là phần hướng tới người dùng của ứng dụng, trong trường hợp này là các tệp HTML. Chúng tôi đã xử lý các chế độ xem trong một dự án trước đó, trong đó máy chủ sẽ gửi tệp HTML cho người dùng, nhưng các tệp này là tĩnh và nhiều trường hợp sử dụng của chúng tôi yêu cầu chế độ xem phải động đối với dữ liệu.

Do đó, chúng tôi sẽ sử dụng công cụ tạo mẫu để tạo chế độ xem của mình. Như tên cho thấy, chúng tôi viết các tệp mẫu trong cơ sở mã của mình để được chuyển đổi thành HTML khi chúng tôi phản hồi yêu cầu của máy chủ. Bất kỳ biến nào được xác định trong tệp mẫu của chúng tôi đều được thay thế bằng dữ liệu thực tế. Ngoài ra, chúng tôi có thể chèn logic có điều kiện và/hoặc vòng lặp vào tệp mẫu của mình, ví dụ: hiển thị tên người dùng của người dùng sau khi họ đăng nhập. Điều này không thể thực hiện được với HTML đơn giản. Mọi JavaScript phía máy khách vẫn có thể được thêm thông qua thẻ tập lệnh (mặc dù hiện tại chúng tôi sẽ không quan tâm nhiều đến phía máy khách).

Trong khóa học này, chúng tôi sẽ sử dụng [EJS](https://ejs.co/). Cú pháp của EJS rất giống với cú pháp của HTML, nghĩa là thời gian học tập tương đối thấp so với các công cụ tạo mẫu khác.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Cách thiết lập EJS trong dự án Express.
- Cách sử dụng EJS.

### Thiết lập EJS

<div class="lesson-note" markdown="1">

####Tại sao không phản ứng?

Chúng tôi chưa dạy cách sử dụng các Thành phần máy chủ React, vốn khá khác với những gì bạn biết về React cho đến nay. Việc tự thiết lập những thứ này không hề đơn giản, không giống như một công cụ tạo mẫu như EJS và sẽ khó tập trung vào việc tìm hiểu các nguyên tắc cơ bản về mặt sau nếu sử dụng khung ngăn xếp đầy đủ được cấu hình sẵn nhưng có tính trừu tượng cao (làm mờ ranh giới giữa cả hai đầu để phát triển hợp lý hơn).

</div>

Hãy bắt đầu với EJS! Cài đặt EJS vào ứng dụng của bạn bằng cách nhập nội dung sau vào thiết bị đầu cuối của bạn:

```bash
npm install ejs
```

Ở thư mục gốc của dự án, hãy tạo một thư mục con có tên `views`.

Tiếp theo, chúng tôi cần cho ứng dụng của mình biết rằng chúng tôi dự định sử dụng `EJS` làm công cụ mẫu cũng như nơi tìm tệp xem.

Trong tệp `app.js` của bạn, hãy đặt các thuộc tính ứng dụng sau:

```javascript
// app.js
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");
```

Và nhập mô-đun Path CommonJS từ Node ở gần đầu:

```javascript
// app.js
const path = require("node:path");
```

Điều này sẽ bật EJS làm công cụ xem và ứng dụng của chúng tôi sẽ tìm kiếm các mẫu trong thư mục con `/views`.

### Cú pháp EJS

Trong EJS, thẻ `<%` và `%>` cho phép chúng tôi sử dụng JavaScript. Điều này cho phép chúng ta viết các câu lệnh có điều kiện, vòng lặp `for`, cũng như sử dụng các biến.

Để xuất một biến dưới dạng giá trị, chúng tôi sử dụng thẻ `<%=`.

Đây là một ví dụ nhanh bao gồm mảng và logic vòng lặp.

```ejs
<% const animals = ["Cat", "Dog", "Lemur", "Hawk"] %>

<ul>
  <% animals.map((animal) => { %>
    <li><%= animal %>s are cute</li>
  <% }) %>
</ul>
```

### Sử dụng EJS với Express

Hãy sử dụng EJS với Express. Đầu tiên, tạo tệp mẫu EJS có tên `index.ejs` trong thư mục con `views` và thêm thông tin sau:

```ejs
<!-- index.ejs -->
<html>
  <body>
    <%= message %>
  </body>
</html>
```

Và trong `app.js`, hiển thị tệp mẫu này theo một trong các tuyến đường của bạn:

```javascript
// app.js
app.get("/", (req, res) => {
  res.render("index", { message: "EJS rocks!" });
});
```

Lý tưởng nhất là chúng ta sẽ xác định tuyến đường trong một tệp bộ định tuyến riêng biệt, nhưng với mục đích của bài học này, chúng ta sẽ xác định nó trong `app.js`

Khởi động máy chủ và đi tới tuyến đường `/` trong trình duyệt. Bạn nên xem:

```html
EJS rocks!
```

Nếu bạn kiểm tra HTML trong các công cụ phát triển của trình duyệt, bạn có thể thấy HTML có cấu trúc giống hệt như cách chúng tôi viết mẫu EJS với biến `message` được thay thế bằng giá trị của nó.

Khi bạn nhấn tuyến đường `/`, `res.render("index", { message: "EJS rocks!" });` là dòng gửi phản hồi. Vì chúng ta đã xác định thuộc tính ứng dụng `views` và `view engine` nên đối số đầu tiên của `res.render` được lập trình để tìm kiếm "mẫu có tên là chỉ mục trong thư mục đã chỉ định", trong khi đối số thứ hai là đối tượng của các biến được cung cấp cho mẫu cụ thể đó.

### Biến cục bộ trong EJS

Trong ví dụ trên, làm thế nào tệp mẫu biết về biến `message`? Khi chúng tôi hiển thị chế độ xem, EJS có quyền truy cập vào bất kỳ thuộc tính nào từ đối tượng mà chúng tôi chuyển vào `res.render`, cũng như mọi thuộc tính trên [Express's res.locals object](https://expressjs.com/en/5x/api/response/#reslocals) (`res.locals` có thể hữu ích nếu bạn cần chuyển giá trị cho chế độ xem trong một chức năng phần mềm trung gian, nhưng sẽ không gọi `res.render` cho đến sau này trong chuỗi phần mềm trung gian).

EJS sẽ lưu trữ các thuộc tính này trong một đối tượng có tên `locals` mà bạn có thể truy cập trong dạng xem. Tương tự như đối tượng `window` toàn cầu trong trình duyệt, điều này cho phép bạn truy cập biến `message` trong chế độ xem thông qua `locals.message` hoặc đơn giản chỉ là `message`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Biến không xác định trong local

Nếu chúng tôi cố gắng truy cập một biến trong tệp mẫu được hiển thị chưa được xác định trong đối số `locals` của `res.render` hoặc `res.locals`, điều này có thể gây ra lỗi tham chiếu. Ví dụ: nếu chúng ta cố truy cập vào một biến `foo` không xác định, `locals.foo` sẽ trả về không xác định, trong khi `foo` sẽ dẫn đến lỗi tham chiếu. Xác minh điều này bằng cách xuất `locals.foo` trong `index.ejs`, sau đó thay thế nó bằng`foo`:

```ejs
<!-- index.ejs -->
<html>
  <body>
    <%= message %>
    <!-- replace the below with the output of just foo -->
    <%= locals.foo %>
  </body>
</html>
```

</div>

### Mẫu có thể tái sử dụng

Bạn có thể muốn bao gồm các thành phần trang web được chia sẻ trên các trang khác nhau, chẳng hạn như thanh bên hoặc tiêu đề. Để chèn các thành phần như vậy vào trang của bạn, chúng tôi sử dụng lệnh `include`. Điều này yêu cầu tên của tệp được chèn và tùy chọn một đối tượng dữ liệu bạn muốn chuyển.

Tạo thành phần thanh điều hướng sau có tên `"navbar.ejs"`:

```ejs
<!-- navbar.ejs -->
<nav>
  <ul>
    <% for (let i = 0; i < links.length; i++) { %>
      <li>
        <a href="<%= links[i].href %>">
          <span> <%= links[i].text %> </span>
        </a>
      </li>
    <% } %>
  </ul>
</nav>
```

Lưu ý rằng thanh điều hướng mong đợi một biến `links`. Để chuyển dữ liệu này vào thanh điều hướng, hãy chuyển nó dưới dạng biến `locals` khi hiển thị tệp mẫu tại `res.render()`. Chúng tôi sẽ kết xuất `index.ejs` để bao gồm thanh điều hướng. Để thực hiện việc này, hãy sửa đổi `app.js` sao cho đối tượng `links` được xác định và chuyển vào hàm `render` hiển thị `index.ejs` trong trình xử lý tuyến đường `"/"`:

```javascript
// app.js
const links = [
  { href: "/", text: "Home" },
  { href: "/about", text: "About" },
];

app.get("/", (req, res) => {
  res.render("index", { links: links });
});
```

Sau đó, sửa đổi `index.ejs` đã tạo trước đó để xóa dòng `<%= message %>` và thay vào đó là `include` `navbar` với biến `links` như sau:

```ejs
<!-- index.ejs -->
<html>
  <head>
    <title>Homepage</title>
  </head>
  <body>
    <%- include('navbar', {links: links}) %>
  </body>
</html>
```

Điều này sẽ bao gồm thanh điều hướng với các liên kết trong `index.ejs`. Chạy máy chủ của bạn và xem! Các mẫu có thể sử dụng lại có thể được sử dụng để bao gồm đầu trang và chân trang trong tất cả các trang của bạn.

Lưu ý việc sử dụng thẻ đầu ra thô `<%-` với `include` được sử dụng để tránh thoát kép đầu ra HTML.

Hãy sử dụng `include` để tự động hiển thị danh sách các biến theo cách khác. Trong `app.js`, thêm mảng `users` sau ngay bên dưới biến `links` và chuyển nó tới hàm `render` khi hiển thị `index`:

```javascript
// app.js

const links = [
  { href: "/", text: "Home" },
  { href: "/about", text: "About" },
];

const users = ["Rose", "Cake", "Biff"];

app.get("/", (req, res) => {
  res.render("index", { links: links, users: users });
});
```

Sau đó tạo chế độ xem mới có tên `user.ejs` trong thư mục `views`:

```ejs
<!-- views/user.ejs -->
<li><%= user %></li>
```

Sau đó thêm khối sau vào `index.ejs`:

```ejs
<!-- index.ejs -->
<ul>
  <% users.forEach((user) => { %>
    <%- include('user', {user: user}); %>
  <% }); %>
</ul>
```

Nếu thành công, Rose, Cake và Biff sẽ hiển thị khi hiển thị `index.ejs`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Các thư mục trong thư mục dạng xem

Hãy tạo các thư mục lồng nhau của các tệp mẫu EJS trong các khung nhìn. Thay đổi `user.ejs` trong thư mục `views` thành `users/user.ejs` và trong `index.ejs` thay đổi đường dẫn từ `user` thành `users/user` trong khối `users.forEach`:

```ejs
<!-- index.ejs -->
<ul>
  <% users.forEach((user) => { %>
    <%- include('users/user', {user: user}); %>
  <% }); %>
</ul>
```

</div>

### Cung cấp nội dung tĩnh

Việc phân phát nội dung tĩnh bằng EJS tương tự như cách chúng tôi phân phát nội dung trước đây khi làm việc trực tiếp với HTML, trong đó chúng tôi có thể thêm các tệp bên ngoài vào phần đầu của tệp mẫu bằng thẻ `link`. Điều chính cần chỉ ra là ứng dụng cần biết phân phát nội dung từ đâu. Giả sử `express` đã được cài đặt, hãy đặt các dòng sau trong `app.js`:

```javascript
// app.js
const assetsPath = path.join(__dirname, "public");
app.use(express.static(assetsPath));
```

`express.static()` là một chức năng phần mềm trung gian cho phép sử dụng nội dung tĩnh và chúng tôi yêu cầu nó tìm kiếm nội dung có thư mục `public` làm gốc.

Tạo tệp `styles.css` sau trong thư mục gốc của `public`:

```css
/* public/styles.css */
body {
  color: red;
}
```

Để phục vụ `styles.css` trong `index.ejs`, hãy đặt thẻ `link` sau đây trong phần đầu:

```ejs
<!-- index.ejs -->
<head>
  <link rel="stylesheet" href="/styles.css">
</head>
```

Tệp `index.ejs` của bạn bây giờ sẽ hiển thị văn bản màu đỏ!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc lại [EJS docs](https://ejs.co/#docs).
1. Đọc qua [Tài nguyên Express trên công cụ mẫu](https://expressjs.com/en/guide/using-template-engines.html). Tài nguyên sử dụng Pug cho các ví dụ có cú pháp khác, tuy nhiên thông tin vẫn phải là phần bổ sung hữu ích cho bài học này.
1. Hãy thêm vào ứng dụng mà chúng ta đang phát triển bài học này nhé! Thêm chế độ xem cho trang giới thiệu, trang này sẽ hiển thị trên tuyến `/about`.
1. Tạo mẫu chân trang có thể sử dụng lại và hiển thị nó trong tất cả các tuyến ứng dụng.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- [Cách sử dụng EJS để tạo mẫu cho ứng dụng Node.js của bạn](https://blog.logrocket.com/how-to-use-ejs-template-node-js-application/)
