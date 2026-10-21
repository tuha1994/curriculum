### Giới thiệu

Dữ liệu là cốt lõi của mọi ứng dụng web tốt, và việc nắm vững SQL sẽ giúp bạn tiến rất xa. Kiến thức đó không chỉ giúp bạn hiểu điều gì đang diễn ra phía sau khi dùng Object-Relational Mapper (ORM; chẳng hạn như Active Record trong Rails hoặc Prisma trong Node.js), mà còn giúp bạn tự tin đặt những câu hỏi phức tạp hơn về dữ liệu của mình. Và đó chính là trọng tâm của SQL -- đặt câu hỏi cho cơ sở dữ liệu, đôi khi thêm hoặc thay đổi dữ liệu trong đó. Khả năng truy vấn có thể cực kỳ hữu ích với bạn.

Trong những trường hợp đơn giản hơn, bạn có thể muốn hiển thị tất cả người dùng đã đăng ký vào tháng 12 bằng mã khuyến mãi "FREESTUFF." Bạn có thể muốn hiển thị mọi bình luận do người dùng hiện tại tạo, rồi sắp xếp theo chủ đề và ngày tạo. Trong những trường hợp phức tạp hơn, bạn có thể muốn hiển thị danh sách tất cả đơn hàng được giao đến các bang có hơn 1.000 người dùng, sắp xếp theo số lượng và tổng giá trị đơn hàng. Hoặc vì mục đích nội bộ, bạn có thể đặt những câu hỏi phân tích tiếp thị như kênh khuyến mãi nào tạo ra những người dùng đáp ứng tiêu chí tương tác bạn đề ra: đọc năm bài viết mỗi tuần làm việc.

Tất cả ví dụ trên đều liên quan đến việc tương tác với cơ sở dữ liệu. May mắn là nhiều cơ sở dữ liệu (những loại chúng ta sẽ tập trung vào) sử dụng SQL, và may mắn hơn nữa là SQL thực ra chỉ có khoảng một tá từ mà bạn sẽ dùng thường xuyên, trong tổng số vài chục từ. Vì vậy, đây không phải là một ngôn ngữ lớn; điều quan trọng hơn nằm ở những khái niệm phía sau nó.

Bạn sẽ bắt đầu với những câu hỏi như trên, rồi phải tìm cách đặt chúng một cách chính xác cho cơ sở dữ liệu của mình, nơi có thể chứa rất nhiều bảng khác nhau. Mỗi người có thể hình dung điều này theo một cách khác nhau, nhưng việc tìm ra cách hình dung những gì đang diễn ra khi bạn thực hiện truy vấn SQL là khá quan trọng. Chúng tôi thường hình dung các bảng Excel đang di chuyển trong đầu, kết hợp với nhau và xáo trộn khi cần. Mỗi người một cách.

Chúng ta sẽ vượt ra ngoài những truy vấn kiểu `SELECT users.* FROM users LIMIT 1` để tìm hiểu các chủ đề năng động hơn như kết hợp các bảng, thực hiện phép tính trên kết quả và nhóm kết quả theo những cách mới.

<!-- markdownlint-disable-next-line TOP004 -->
#### Lưu ý về tài nguyên học tập

SQL là một trong những chủ đề từng bị cất trong những cuốn sổ tay kỹ thuật phủ bụi và các website mang phong cách thập niên 90. Ngay cả những cuốn sách hay nhất cũng có thể khiến nó có vẻ phức tạp một cách khó hiểu, vì chúng thường viết cho các kỹ sư cơ sở dữ liệu, những người thực sự cần biết mọi chi tiết nhỏ nhặt.

Dù sự phổ biến của web app ngày nay đã làm tăng nhu cầu để người mới tập trung tìm hiểu *các khái niệm* về SQL, những công cụ học tập vẫn chưa thực sự theo kịp. Chúng tôi sẽ cố gắng hết sức để truyền đạt các khái niệm đó bằng những công cụ hiện có.

### Tổng quan bài học

Phần này cung cấp tổng quan khái quát về những chủ đề bạn sẽ học trong bài này.

- Primary Key là gì.
- Foreign Key là gì.
- Schema là gì.
- Cách sử dụng nhiều câu lệnh SQL như `SELECT`, `CREATE TABLE`, `UPDATE`, `DELETE` và các câu lệnh khác.
- Cách sử dụng nhiều mệnh đề SQL như `WHERE`, `LIKE`, `DISTINCT` và các mệnh đề khác.
- Cách sử dụng nhiều hàm SQL như `AVG`, `COUNT`, `SUM` và các hàm khác.
- Index hữu ích cho việc gì.
- `WHERE` khác `HAVING` như thế nào.

### Lời giải thích SQL bán hoàn chỉnh nhanh nhất thế giới

Đây là phần giải thích SQL từ đầu đến cuối nhưng rất ngắn gọn. Phần này sẽ không đặc biệt giỏi trong việc dạy bạn những kỹ thuật mới cụ thể, nhưng sẽ cung cấp một cái nhìn tổng quan để bạn có trước khi bắt đầu đọc bài được giao. Bắt đầu thôi...

SQL là ngôn ngữ dùng để giao tiếp với nhiều cơ sở dữ liệu quan hệ. Những cơ sở dữ liệu này dùng nhiều bảng để lưu trữ các loại dữ liệu khác nhau (ví dụ: bảng `users` và `posts`). Bảng là những danh sách dài giống như bảng tính, trong đó mỗi hàng là một bản ghi khác nhau (hoặc một đối tượng, chẳng hạn một người dùng), còn mỗi cột là một thuộc tính của bản ghi đó (như tên, email, v.v.). Một cột mà mọi bảng đều có là cột `ID`, dùng để cung cấp số hàng duy nhất và được gọi là "primary key" của bản ghi.

Bạn có thể "liên kết" các bảng với nhau bằng cách cho một cột trong bảng này trỏ đến ID của bảng khác. Chẳng hạn, một hàng trong bảng `posts` có thể chứa ID của tác giả trong cột tên là `user_id`. Vì bảng `posts` chứa ID của một bảng khác nên cột đó được gọi là "foreign key."

#### Thiết lập mọi thứ

SQL cho phép bạn làm mọi việc. Nhóm câu lệnh đầu tiên dùng để thiết lập cơ sở dữ liệu (`CREATE DATABASE`), thiết lập một bảng riêng lẻ (`CREATE TABLE`) và thực hiện các thao tác tương tự để thay đổi hoặc xóa chúng. Thông tin thiết lập cho cơ sở dữ liệu được lưu trong một tệp đặc biệt gọi là "Schema", và tệp này được cập nhật mỗi khi bạn thay đổi cấu trúc cơ sở dữ liệu. Hãy hình dung schema như đang nói rằng: "Đây là cơ sở dữ liệu của chúng ta, trong đó có một vài bảng. Bảng đầu tiên là 'users', có các cột 'ID' (kiểu số nguyên), 'name' (một chuỗi ký tự), 'email' (một chuỗi ký tự)..."

Ngoài việc thiết lập bảng, bạn có thể yêu cầu cơ sở dữ liệu chỉ cho phép các giá trị duy nhất trong một cột cụ thể (ví dụ như tên người dùng), hoặc lập chỉ mục cho một cột để tìm kiếm nhanh hơn về sau bằng `CREATE INDEX`. Hãy tạo index cho những cột mà sau này bạn có khả năng sẽ dùng để tìm kiếm (như username); index về cơ bản sẽ thực hiện trước phần việc nặng là sắp xếp bảng, nhờ đó cơ sở dữ liệu của bạn sẽ nhanh hơn rất nhiều.

SQL thích dấu chấm phẩy ở cuối dòng và dùng dấu nháy đơn (') thay vì dấu nháy kép(").

#### Loay hoay với dữ liệu

Sau khi đã thiết lập cơ sở dữ liệu và có các bảng trống để làm việc, bạn dùng các câu lệnh SQL để bắt đầu điền dữ liệu vào đó. Những thao tác chính bạn muốn thực hiện là CRUD (chúng ta đã gặp trước đây) -- Create, Read, Update và Destroy. Phần lớn câu lệnh bạn chạy sẽ thuộc nhóm "Read", vì bạn sẽ dành nhiều thời gian đặt câu hỏi về dữ liệu và cố gắng hiển thị dữ liệu đó.

Mỗi câu lệnh CRUD trong SQL gồm một vài phần -- hành động ("statement"), bảng mà câu lệnh sẽ tác động lên và các điều kiện ("clauses"). Nếu bạn chỉ thực hiện một hành động trên bảng mà không chỉ định điều kiện, hành động đó sẽ áp dụng cho toàn bộ bảng và có thể bạn sẽ làm hỏng một thứ gì đó.

Với các truy vấn "Destroy", lỗi kinh điển là gõ `DELETE FROM users` mà không có mệnh đề `WHERE`, khiến toàn bộ người dùng trong bảng bị xóa. Có lẽ bạn chỉ cần xóa một người dùng, được xác định bằng một thuộc tính (hy vọng là duy nhất) như `name` hoặc `id` trong mệnh đề điều kiện, chẳng hạn `DELETE FROM users WHERE users.id = 1`. Bạn có thể làm đủ loại việc hợp lý, chẳng hạn dùng các toán tử so sánh (`>`, `<`, `<=` v.v.) để chỉ định nhóm hàng mà câu lệnh sẽ tác động lên, hoặc dùng các toán tử logic (`AND`, `OR`, `NOT` v.v.) để nối nhiều mệnh đề, chẳng hạn `DELETE FROM users WHERE id > 12 AND name = 'foo'`.

Các truy vấn "Create" dùng `INSERT INTO`, và bạn cần chỉ định những cột sẽ chèn giá trị vào, sau đó là các giá trị tương ứng:

```sql
INSERT INTO users (name, email) VALUES ('foobar', 'foo@bar.com');
```

Lưu ý: Về mặt kỹ thuật, bạn có thể bỏ qua tên cột, nhưng đây được xem là cách làm không tốt và thường không được khuyến khích.

Đây là một trong số ít truy vấn mà bạn không cần cẩn thận về việc đã chọn những hàng nào, vì thực tế bạn chỉ đang thêm các hàng mới vào bảng.

Các truy vấn "Update" dùng `UPDATE`, và bạn cần cho biết dữ liệu nào sẽ được `SET` (dùng các cặp key="value"), cũng như những hàng nào sẽ được cập nhật. Hãy cẩn thận, vì nếu mệnh đề `WHERE` tìm thấy nhiều hàng (ví dụ khi bạn tìm theo một tên phổ biến), tất cả các hàng đó sẽ được cập nhật. Một truy vấn tiêu chuẩn để cập nhật email của người dùng có thể trông như sau (dù trong thực tế bạn nên tìm theo ID vì ID luôn là duy nhất):

```sql
  UPDATE users
  SET name='barfoo', email='bar@foo.com'
  WHERE email='foo@bar.com';
```

Các truy vấn "Read" dùng `SELECT` và là loại phổ biến nhất, chẳng hạn `SELECT * FROM users WHERE created_at < '2013-12-11 15:35:59 -0800'`. Dấu `*` bạn thấy chỉ có nghĩa là "tất cả các cột." Hãy chỉ định một cột bằng cả tên bảng và tên cột. Với truy vấn chỉ có một bảng, bạn có thể chỉ dùng tên cột, nhưng ngay khi có nhiều hơn một bảng, SQL sẽ báo lỗi, vì vậy hãy luôn chỉ định tên bảng: `SELECT users.id, users.name FROM users`.

Một họ hàng gần của `SELECT`, dùng khi bạn chỉ muốn các giá trị duy nhất của một cột, là `SELECT DISTINCT`. Giả sử bạn muốn có danh sách tất cả tên khác nhau của người dùng mà không bị trùng lặp... hãy thử `SELECT DISTINCT users.name FROM users`.

#### Ghép các bảng lại với nhau

Nếu muốn lấy tất cả bài viết do một người dùng nhất định tạo, bạn cần cho SQL biết những cột nào sẽ được dùng để ghép các bảng với nhau bằng mệnh đề `ON`. Thực hiện việc "ghép" bằng câu lệnh `JOIN`. Nhưng nếu bạn ghép hai bảng mà dữ liệu không khớp hoàn toàn (ví dụ có nhiều bài viết của một người dùng), thực tế bạn sẽ giữ lại những hàng nào? Có bốn khả năng khác nhau:

<div class="lesson-note lesson-note--tip" markdown="1">

#### Làm rõ LEFT trong JOIN

Bảng "left" là bảng ban đầu (bảng mà mệnh đề `FROM` đứng trước `ON`), ví dụ như `users` trong các ví dụ bên dưới.

</div>

1. `INNER JOIN`, hay còn gọi là `JOIN` -- Người bạn thân nhất của bạn và là loại chiếm 95% những gì bạn sẽ dùng. Chỉ giữ lại các hàng từ cả hai bảng khi chúng khớp nhau. Nếu bạn yêu cầu lấy tất cả bài viết của tất cả người dùng (`SELECT * FROM users JOIN posts ON users.id = posts.user_id`), kết quả chỉ gồm những người dùng thực sự đã viết bài và những bài viết có chỉ định tác giả trong cột `user_id`. Nếu một tác giả viết nhiều bài, sẽ có nhiều hàng được trả về (nhưng các cột chứa dữ liệu người dùng chỉ được lặp lại).
1. `LEFT OUTER JOIN` -- Giữ lại tất cả hàng từ bảng bên trái và thêm những hàng từ bảng bên phải khớp với hàng của bảng bên trái. Đặt mọi ô trống được tạo ra thành `NULL`. Ví dụ, trả về tất cả người dùng dù họ có viết bài hay không. Nếu có bài viết, liệt kê các bài đó như trên. Nếu không, đặt các cột được yêu cầu từ bảng `posts` thành `NULL`.
1. `RIGHT OUTER JOIN` -- Ngược lại... giữ lại tất cả hàng trong bảng bên phải.
1. `FULL OUTER JOIN` -- Giữ lại tất cả hàng từ mọi bảng, ngay cả khi chúng không khớp nhau. Đặt mọi ô không khớp thành `NULL`.

Các phép join cũng tự nhiên cho phép bạn chỉ định điều kiện, chẳng hạn nếu chỉ muốn lấy bài viết của một người dùng cụ thể: `SELECT * FROM users JOIN posts ON users.id = posts.user_id WHERE users.id = 42`.

*Xem ["Giải thích trực quan về SQL Joins"](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins) của Jeff Atwood để xem các hình minh họa hữu ích.*

Hãy đọc bài học về [Joins của W3 Schools](http://www.w3schools.com/sql/sql_join.asp) để có phần giải thích rõ hơn.

#### Dùng hàm để tổng hợp dữ liệu

Khi chạy một truy vấn SQL thông thường, bạn thường nhận lại rất nhiều hàng. Đôi khi bạn chỉ muốn trả về một giá trị liên quan duy nhất, dùng để tổng hợp một cột, chẳng hạn `COUNT` số bài viết mà một người dùng đã viết. Trong trường hợp này, hãy dùng một trong các hàm "aggregate" hữu ích mà SQL cung cấp (phần lớn đúng như bạn mong đợi -- các hàm như `SUM`, `MIN` và `MAX` v.v.). Bạn đưa hàm vào trong câu lệnh `SELECT`, chẳng hạn `SELECT MAX(users.age) FROM users`. Hàm sẽ chỉ hoạt động trên một cột, trừ khi bạn chỉ định `*`; cách này chỉ hoạt động với một số hàm như `COUNT` (vì `COUNT(*)` đếm tất cả các hàng, còn một thứ như `MAX(*)` không có ý nghĩa, vì lấy giá trị lớn nhất của mọi thứ thì nghĩa là gì?).

Bạn thường thấy alias (`AS`) được dùng để đổi tên các cột hoặc hàm tổng hợp, để sau đó có thể gọi chúng bằng alias đó, chẳng hạn `SELECT MAX(users.age) AS highest_age FROM users` sẽ trả về một cột tên là `highest_age`, chứa tuổi lớn nhất.

Bây giờ chúng ta đến phần thú vị. Các hàm tổng hợp như `COUNT`, trả về một giá trị duy nhất cho toàn bộ tập dữ liệu, rất hữu ích khi bạn muốn dùng chúng trên những phần dữ liệu thật cụ thể rồi nhóm các phần đó lại, chẳng hạn hiển thị `COUNT` số bài viết của TỪNG người dùng (thay vì đếm tất cả bài viết của mọi người dùng). Câu lệnh sẽ như sau:

```sql
  SELECT users.id, users.name, COUNT(posts.id) AS posts_written
  FROM users
  JOIN posts ON users.id = posts.user_id
  GROUP BY users.id, users.name;
```

Lưu ý rằng việc nhóm theo `users.name` cùng với `users.id` giúp làm rõ ý nghĩa và phù hợp với các phương pháp tốt nhất, vì đã đưa rõ ràng mọi cột được chọn nhưng không tổng hợp vào mệnh đề `GROUP BY`, dù với phần lớn cơ sở dữ liệu thì điều này có thể không thực sự bắt buộc.
Hãy xem [sân chơi SQL trên trình duyệt của W3Schools](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby) để tương tác trực quan.

Mẹo hữu ích cuối cùng là khi bạn chỉ muốn hiển thị một phần dữ liệu. Trong tình huống thông thường, bạn sẽ dùng mệnh đề `WHERE` để thu hẹp kết quả. Nhưng nếu đã dùng một hàm tổng hợp như `COUNT` (chẳng hạn để đếm số bài viết mỗi người dùng đã viết trong ví dụ trên), `WHERE` sẽ không còn hoạt động. Vì vậy, để truy xuất có điều kiện các bản ghi dựa trên hàm tổng hợp, bạn dùng mệnh đề `HAVING`, về bản chất là `WHERE` dành cho các phép tổng hợp. Ví dụ, nếu chỉ muốn hiển thị những người dùng đã viết hơn 10 bài:

```sql
  SELECT users.id, users.name, COUNT(posts.id) AS posts_written
  FROM users
  JOIN posts ON users.id = posts.user_id
  GROUP BY users.id, users.name
  HAVING COUNT(posts.id) >= 10;
```

Hãy quay lại [sân chơi SQL trên trình duyệt của W3Schools](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby), rồi nối các bảng `Customers` và `Orders` để lấy số đơn hàng ở mỗi quốc gia và thêm dòng `HAVING COUNT(*) > 10;` sau `GROUP BY` (đồng thời xóa dấu chấm phẩy thừa ở dòng trước).

Có lẽ bạn đã bị lạc ở đâu đó trong phần giải thích trên, và điều đó hoàn toàn bình thường... phần này bao quát nhiều hơn rất nhiều so với những gì bất kỳ ai có thể tiếp thu trong 10 phút. Phần đọc được giao sẽ giải thích tốt hơn, nhưng quan trọng hơn là bạn sẽ có nhiều cơ hội củng cố hiểu biết bằng cách áp dụng vào dự án. Đừng lo và hãy kiên trì!

### SQL nhanh hơn code của bạn!

Việc học những nội dung này đặc biệt quan trọng, vì xây dựng các truy vấn sử dụng SQL một cách thông minh NHANH hơn RẤT NHIỀU so với việc lấy cả đống dữ liệu ra khỏi cơ sở dữ liệu rồi dùng một ngôn ngữ lập trình (như Ruby hoặc JavaScript) để xử lý. Chẳng hạn, nếu muốn lấy tất cả tên duy nhất của người dùng, bạn CÓ THỂ lấy toàn bộ danh sách từ cơ sở dữ liệu bằng SQL như `SELECT users.name FROM users`, rồi dùng một phương thức của JavaScript/Ruby để loại bỏ các giá trị trùng lặp... nhưng cách đó buộc bạn phải lấy toàn bộ dữ liệu ra khỏi cơ sở dữ liệu, đưa nó vào bộ nhớ rồi lặp qua nó trong mã của mình. Thay vào đó, hãy dùng `SELECT DISTINCT users.name FROM users` để SQL thực hiện tất cả chỉ trong một bước.

SQL được thiết kế để nhanh. Nó có một bộ tối ưu hóa truy vấn đặc biệt, xem xét toàn bộ truy vấn bạn sắp chạy, rồi xác định chính xác những bảng nào cần được nối với nhau và cách thực thi truy vấn nhanh nhất. Sự khác biệt giữa việc dùng `SELECT` và `SELECT DISTINCT` không đáng kể so với chi phí thời gian khi bạn tự làm việc đó. Học SQL sẽ giúp bạn viết những truy vấn tốt hơn, có thể làm được nhiều việc hơn và khiến ứng dụng của bạn nhanh hơn đáng kể.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Hãy hoàn thành [bài hướng dẫn SQL tương tác này của SQL Teaching](https://www.sqlteaching.com/).
  1. Hãy hoàn thành [bài hướng dẫn SQL tương tác chuyên sâu hơn này của SQL Bolt](http://sqlbolt.com/).

</div>

### Kết luận

SQL có thể là một tập hợp khái niệm khó nắm bắt, đặc biệt khi phải hiển thị có điều kiện và nhóm kết quả của nhiều phép join. Mọi nội dung từ các phép join thông thường đến các hàm tổng hợp thông thường đều là kiến thức cốt lõi mà bạn thực sự nên cố gắng tiếp thu.

Nếu bạn không bao giờ hoàn toàn thoải mái với những khái niệm thực sự nâng cao thì cũng không sao; may mắn là trong tương lai bạn sẽ không cần dùng chúng, ngoại trừ một phần nhỏ các tình huống. Học tất cả ngay từ đầu là điều tốt, nhưng có lẽ khi cần, bạn vẫn sẽ tìm trên Google cách thực hiện một số truy vấn nâng cao.

Bước tiếp theo, sau khi đã có cơ hội thực hành tất cả nội dung này trong dự án, là áp dụng nó vào mã nguồn của bạn trong các khóa học sắp tới. Bạn sẽ nhanh chóng nhận ra rằng dùng các công cụ ORM giúp cuộc sống dễ dàng hơn rất, rất, rất nhiều. Chỉ cần đừng quên SQL thân thuộc khi đã chuyển sang những điều tốt đẹp và sáng sủa hơn, được chứ?
