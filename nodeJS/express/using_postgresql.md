### Giới thiệu

Bây giờ chúng ta đã thiết lập và chạy PostgreSQL trên máy của mình, đã đến lúc làm việc với nó.

Để cho ngắn gọn, từ bây giờ chúng ta sẽ gọi cơ sở dữ liệu là db.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thiết lập một db mới trong shell PostgreSQL.
- Thiết lập và truy vấn với node-postgres.
- Sử dụng tập lệnh để điền vào db.

### Thiết lập ứng dụng Express

<div class="lesson-note lesson-note--critical" markdown="1">

####Bài học tiên quyết

Đảm bảo bạn đã hoàn thành [khóa học SQL](https://www.theodinproject.com/paths/full-stack-javascript/courses/databases).

Bài học này và tất cả các bài học tiếp theo sẽ giả sử bạn hiểu cú pháp và khái niệm SQL.

</div>

Hãy bắt đầu bằng cách tạo một ứng dụng Express. Nó sẽ chỉ có một tính năng - thêm tên người dùng do người dùng cung cấp vào db. Dưới đây là các tuyến đường dự kiến ​​và chức năng của chúng:

- `GET /` - để đăng nhập tên người dùng có sẵn trong DB vào thiết bị đầu cuối. Hiện tại, chỉ cần nhập `console.log("usernames will be logged here - wip")` đơn giản.
- `GET /new` - để hiển thị biểu mẫu HTML cho người dùng với một trường văn bản nhập tên người dùng. Nó sẽ gửi đến tuyến đường tiếp theo.
- `POST /new` - sẽ lưu dữ liệu tên người dùng đến vào DB. Hiện tại, chỉ cần đăng nhập `console.log("username to be saved: ", req.body.username)`.

Chuyển sang phần tiếp theo khi bạn đã sử dụng được chức năng trên. Phạm vi mã có liên quan đến các thư mục `routes` và `controllers`. Vì chỉ có một chế độ xem (`GET /new`) để xử lý nên bạn muốn sử dụng `ejs` hay sử dụng HTML đơn giản là tùy thuộc vào bạn.

### Thiết lập cơ sở dữ liệu

Nhập shell PostgreSQL bằng cách chạy `psql` trong terminal của bạn. Bạn có thể xem dbs hiện tại bằng lệnh `\l`. Hãy tạo một db mới bằng cách chạy câu lệnh SQL sau:

```sql
CREATE DATABASE top_users;
```

`\l` một lần nữa để xem db đã được tạo chưa. Bây giờ hãy kết nối với db:

```sql
\c top_users
```

Xác minh rằng lời nhắc `psql` phải là:

```sql
top_users=#
```

Bây giờ hãy tạo một bảng và các cột của nó để lưu trữ dữ liệu `username`:

```sql
CREATE TABLE usernames (
   id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
   username VARCHAR ( 255 ) 
);
```

Xác minh rằng bảng đã được tạo bằng cách chạy `\d`. Bạn sẽ thấy hai bảng sau ở đầu ra (chúng tôi đã bỏ qua một số chi tiết đầu ra để cho ngắn gọn):

```bash
|       Name       |   Type   |
+ -----------------+----------+
| usernames        | table    |
| usernames_id_seq | sequence |
```

<div class="lesson-note" markdown="1">

#### Cột nhận dạng

Đợi chút, `usernames_id_seq` này là gì vậy?

Mệnh đề `GENERATED ALWAYS AS IDENTITY` là thủ phạm. Nó định nghĩa cột `id` là [cột nhận dạng](https://www.postgresql.org/docs/current/sql-createtable.html#SQL-CREATETABLE-PARMS-GENERATED-IDENTITY). PostgreSQL hiện tự động tạo giá trị cho cột này. Theo mặc định, nó bắt đầu từ 1 và tăng dần thêm 1 cho mỗi hàng mới. Ngoài ra, PostgreSQL ngầm tạo `usernames_id_seq`, một đối tượng chuỗi, theo dõi giá trị tiếp theo sẽ được sử dụng.

</div>

Woohoo, bây giờ chúng ta có một db và một bảng... một bảng cô đơn. Không lâu:

```sql
INSERT INTO usernames (username)
VALUES ('Mao'), ('nevz'), ('Lofty');
```

Xác minh:

```sql
SELECT * FROM usernames;
```

### Sử dụng nút-postgres trong Express

Chúng tôi có thể làm việc với PostgreSQL trong ứng dụng Express của mình thông qua [`node-postgres`](https://github.com/brianc/node-postgres) (hoặc gọi tắt là `pg`). Đó là một thư viện mà chúng ta sẽ sử dụng để giao tiếp với cơ sở dữ liệu PostgreSQL. Cài đặt nó với:

```bash
npm install pg
```

Sau đó, chúng tôi có thể khởi tạo nó trong ứng dụng của mình với thông tin kết nối cần thiết. Tạo thư mục `db` và tệp mới `db/pool.js` với:

```javascript
const { Pool } = require("pg");

// All of the following properties should be read from environment variables
// We're hardcoding them here for simplicity
module.exports = new Pool({
  host: "localhost", // or wherever the db is hosted
  user: "<role_name>",
  database: "top_users",
  password: "<role_password>",
  port: 5432 // The default port
});
```

Hãy nhớ rằng bạn nên thay đổi `<role_name>` và `<role_password>` (Chúng ta đã thiết lập chúng ở bài học trước)!

Một cách khác để xác định thông tin kết nối là thông qua [Kết nối URI](https://node-postgres.com/features/connecting#connection-uri). Bạn có thể sẽ sử dụng URI kết nối khi kết nối với dịch vụ cơ sở dữ liệu được lưu trữ trên máy chủ. Đây là giao diện của nó dựa trên các thuộc tính trên:

```javascript
const { Pool } = require("pg");

// Again, this should be read from an environment variable
module.exports = new Pool({
  connectionString: "postgresql://<role_name>:<role_password>@localhost:5432/top_users"
});
```

Chọn bất kỳ phương pháp nào bạn muốn sử dụng và tiếp tục.

<div class="lesson-note" markdown="1">

#### Hai cách kết nối với pg

`pg` có hai cách để kết nối với db: máy khách và nhóm.

Máy khách là một kết nối riêng lẻ tới DB mà bạn quản lý theo cách thủ công. Bạn mở một kết nối, thực hiện truy vấn của mình, sau đó đóng nó lại. Điều này phù hợp với các truy vấn một lần nhưng có thể trở nên tốn kém nếu bạn phải xử lý nhiều truy vấn. Vấn đề này sẽ không được giảm bớt nếu bằng cách nào đó chúng ta có thể giữ được khách hàng? Đúng!

Vào bể bơi. Đúng như tên gọi, đó là một nhóm khách hàng. Một nhóm giữ các kết nối. Và khi bạn truy vấn, nó sẽ lập trình mở một kết nối mới trừ khi có kết nối dự phòng hiện có. Hoàn hảo cho các máy chủ web.

</div>

### Truy vấn bằng pg

Với `Pool` đã khởi tạo, chúng ta có thể sử dụng phương pháp `query`. Tạo một tệp `db/queries.js` mới. Sau khi sửa đổi các yêu cầu của dự án, chúng tôi hiểu rằng chúng tôi cần hai tương tác db: lấy tất cả tên người dùng và chèn tên người dùng mới. Hãy xác định các chức năng này:

```javascript
const pool = require("./pool");

async function getAllUsernames() {
  const { rows } = await pool.query("SELECT * FROM usernames");
  return rows;
}

async function insertUsername(username) {
  await pool.query("INSERT INTO usernames (username) VALUES ($1)", [username]);
}

module.exports = {
  getAllUsernames,
  insertUsername
};
```

<div class="lesson-note" markdown="1">

#### Tham số hóa

Có chuyện gì với `$1` trong truy vấn chèn?

Ngoài ra, truy vấn có thể trông giống như:

```javascript
await pool.query("INSERT INTO usernames (username) VALUES ('" + username + "')");
```

Chúng ta đang đưa trực tiếp giá trị do người dùng nhập, tức là `username`, vào truy vấn. Một người dùng ác ý có thể nhập thứ gì đó như `sike'); DROP TABLE usernames; --` và gây ra hậu quả nghiêm trọng. Đáng sợ thật. Đây được gọi là [SQL injection](https://en.wikipedia.org/wiki/SQL_injection).

`pg` cung cấp [tham số truy vấn](https://node-postgres.com/features/queries#parameterized-query) để ngăn chặn điều này. Thay vì truyền trực tiếp dữ liệu nhập của người dùng, chúng tôi chuyển dữ liệu đó vào một mảng làm đối số thứ hai. `pg` xử lý phần còn lại.

</div>

Gọi hai hàm trên trong các bộ điều khiển cụ thể (bạn có thể có các tên hàm khác nhau, v.v. Điều quan trọng là phải hiểu cách gọi các hàm db):

```javascript
const db = require("../db/queries");

async function getUsernames(req, res) {
  const usernames = await db.getAllUsernames();
  console.log("Usernames: ", usernames);
  res.send("Usernames: " + usernames.map(user => user.username).join(", "));
}

async function createUsernameGet(req, res) {
  // render the form
}

async function createUsernamePost(req, res) {
  const { username } = req.body;
  await db.insertUsername(username);
  res.redirect("/");
}

module.exports = {
  getUsernames,
  createUsernameGet,
  createUsernamePost
};
```

Hãy dùng thử ứng dụng của bạn, hy vọng nó hoạt động như mong đợi.

### Điền vào db thông qua tập lệnh

Bạn có thể nhận thấy việc tạo một bảng và điền dữ liệu vào bảng đó cồng kềnh như thế nào. May mắn thay, chúng ta có sức mạnh của (c\|n)ode bên cạnh, hãy tự động hóa nó thông qua một tập lệnh. Tạo một tệp mới `db/populatedb.js`.

```javascript
#! /usr/bin/env node

const { Client } = require("pg");

const SQL = `
CREATE TABLE IF NOT EXISTS usernames (
  id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  username VARCHAR ( 255 )
);

INSERT INTO usernames (username) 
VALUES
  ('Bryan'),
  ('Odin'),
  ('Damon');
`;

async function main() {
  console.log("seeding...");
  const client = new Client({
    connectionString: "postgresql://<role_name>:<role_password>@localhost:5432/top_users",
  });
  await client.connect();
  await client.query(SQL);
  await client.end();
  console.log("done");
}

main();
```

Đăng nhập vào PostgreSQL shell, kết nối với db `top_users` và thả bảng `usernames`:

```sql
DROP TABLE usernames;
```

Sau đó, bạn có thể chạy tập lệnh này qua `node db/populatedb.js` hoặc thêm nó dưới dạng [tập lệnh trong pack.json](https://stackoverflow.com/a/36433748).

Xin lưu ý rằng tập lệnh được thiết kế để chỉ chạy một lần.

#### db cục bộ và db sản xuất

Tuyệt vời! Bạn vừa học cách thiết lập và sử dụng db cục bộ với PostgreSQL. Cơ sở dữ liệu cục bộ rất lý tưởng để phát triển vì chúng cung cấp khả năng tương tác nhanh hơn, sửa đổi dễ dàng hơn và không yêu cầu kết nối Internet. Điều này khiến chúng đặc biệt hữu ích khi tạo nguyên mẫu hoặc thử nghiệm các tính năng mới.

Khi bạn đã sẵn sàng công khai dự án của mình, bạn sẽ cần chuyển sang cơ sở dữ liệu sản xuất được lưu trữ trên máy chủ bên ngoài độc lập với máy cục bộ của bạn. Cơ sở dữ liệu sản xuất cho phép khả năng truy cập toàn cầu, khả năng mở rộng và bảo mật mạnh mẽ hơn. Hầu hết các nhà cung cấp dịch vụ lưu trữ được giới thiệu trong [bài học triển khai](https://www.theodinproject.com/lessons/node-path-nodejs-deployment) cũng cung cấp dịch vụ cơ sở dữ liệu. Bây giờ chúng ta đã biết sự khác biệt, hãy xem cách chúng ta có thể điền vào cơ sở dữ liệu sản xuất.

#### Điền dbs sản xuất

Chúng tôi đã mã hóa cứng thông tin kết nối db cục bộ của mình trong tập lệnh. Do đó, tập lệnh chỉ điền vào db cục bộ của chúng tôi. Chúng tôi cũng cần một cách để điền vào db sản xuất của mình. Một cách để làm điều này là sử dụng các biến môi trường, mặc dù điều này dẫn đến những rắc rối không cần thiết. Tại sao? Bởi vì hiện tại, tập lệnh chỉ có thể điền db sản xuất trên máy chủ sản xuất, tức là chúng ta sẽ cần truy cập vào cli của máy chủ sản xuất để chạy tập lệnh. Hoặc, chúng tôi có thể lén lút chỉnh sửa tệp môi trường của mình để trỏ đến db sản xuất và chạy tập lệnh trên máy của chúng tôi rồi hoàn nguyên nó.

Chúng ta nên đặt mục tiêu làm cho tập lệnh của mình độc lập với cơ sở mã nhất có thể.

Một cách tiếp cận dễ dàng hơn nhiều là cung cấp thông tin kết nối làm đối số cho tập lệnh. Bằng cách này, chúng tôi có thể chạy tập lệnh cho db cục bộ cũng như db sản xuất trên máy của mình. Bạn có thể truy cập các đối số thông qua [process.argv](https://nodejs.org/docs/latest/api/process.html#processargv).

```bash
# populating local db 
node db/populatedb.js <local-db-url>

# populating production db
# run it from your machine once after deployment of your app & db
node db/populatedb.js <production-db-url>
```

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc lướt qua [tài liệu của trang](https://node-postgres.com/). Bản thân thư viện rất nhẹ và tài liệu của họ cũng vậy. Bạn không cần phải đọc hết mọi thứ, chủ yếu sử dụng nó để tham khảo.
1. Cập nhật dự án trên mà chúng tôi đang thực hiện.
    1. Triển khai các biến môi trường cho thông tin kết nối db.
    1. Thêm chức năng tìm kiếm thông qua các tham số truy vấn trên lộ trình chỉ mục. Ví dụ: `GET /?search=sup` sẽ trả về tất cả tên người dùng có chứa `sup`. KHÔNG triển khai điều này trong JavaScript, việc tìm kiếm phải được thực hiện bằng SQL.
    1. Thêm tuyến đường mới `GET /delete` để xóa tất cả tên người dùng khỏi db.
1. Trong dự án Bảng tin mini trước đây của chúng tôi, chúng tôi đã triển khai các tin nhắn tạm thời bằng cách sử dụng một mảng, tức là các tin nhắn sẽ được đặt lại khi máy chủ khởi động lại. Chúng tôi muốn sự kiên trì của dữ liệu. Quay lại dự án này và triển khai nó với db PostgreSQL và `pg`.
   - Triển khai một db mới trên dịch vụ lưu trữ mà bạn chọn và lấy thông tin kết nối của nó.
   - Tạo bảng `messages`, điền dữ liệu vào bảng nếu bạn muốn. Điều này nên được thực hiện thông qua một kịch bản.
   - Thêm các biến môi trường cần thiết, tạo nhóm và triển khai các hàm db cần thiết.
   - Trong khi thực hiện, hãy thêm xác thực phía máy chủ thích hợp cho thông tin đầu vào của người dùng.

</div>
