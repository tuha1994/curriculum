### Giới thiệu

Có lẽ bạn từng tự hỏi làm thế nào để theo dõi dữ liệu của tất cả người dùng. Ai mà nhớ nổi mật khẩu đăng nhập của bạn là `CatLover1985` để bạn có thể đăng nhập vào trang web? Tầng nền tảng của mọi ứng dụng web là cơ sở dữ liệu, và nó thay bạn ghi nhớ mọi thứ (chúng ta sẽ tìm hiểu về bộ nhớ đệm vào phần sau). Cơ sở dữ liệu có thể tương đối đơn giản, giống như một bảng tính Excel, hoặc cực kỳ phức tạp và được chia thành nhiều phần khổng lồ, như cơ sở dữ liệu của Facebook.

Cơ sở dữ liệu thường nằm khuất phía sau ứng dụng web, nên mọi người thường tiếp cận chúng với cảm giác vừa dè chừng vừa kinh ngạc. Tuy nhiên, đừng lo lắng hay cảm thấy bị choáng ngợp. Khi tìm hiểu sâu hơn về chủ đề này, bạn và cơ sở dữ liệu của mình sẽ trở thành những người bạn rất thân (hoặc ít nhất là bạn bè nhưng vẫn có chút đối đầu). Hãy yên tâm rằng đến cuối chương trình học này, bạn sẽ hiểu cơ sở dữ liệu của mình đang hoạt động như thế nào và có thể tương tác với chúng như một chuyên gia (và có lẽ còn giỏi hơn một số người bạn sẽ làm việc cùng). Bài học này chỉ là phần giới thiệu sơ lược về những điều đó.

So với một ngôn ngữ lập trình thông thường mà bạn đã học, SQL (Structured Query Language, ngôn ngữ truy vấn có cấu trúc), được dùng để truy vấn cơ sở dữ liệu, có cú pháp rất ngắn gọn... và chỉ cần học một số ít động từ. Điều khiến mọi người gặp khó khăn là bạn phải hình dung được trong đầu câu lệnh sẽ thực hiện những gì. Chúng ta sẽ dành khá nhiều thời gian cho SQL và cơ sở dữ liệu vì chúng là nền tảng, nhưng trước mắt, chúng ta chỉ tìm hiểu đủ để bạn làm quen với những gì đang diễn ra bên trong đó.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Cơ sở dữ liệu là gì.
- Cơ sở dữ liệu quan hệ là gì.
- Cơ sở dữ liệu quan hệ khác XML ở điểm nào.
- SQL là gì.
- SQL được dùng để làm gì.
- Cách lấy tất cả bản ghi từ một bảng bằng SQL.
- Cách chèn một bản ghi bằng SQL.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Xem phần giới thiệu về [cách sử dụng SQL để sắp xếp và quản lý một lượng dữ liệu khổng lồ](https://launchschool.com/books/sql/read/introduction).
      - Bạn không cần đọc quá trang đầu tiên của phần giới thiệu.

  1. Xem [video ngắn giới thiệu về cơ sở dữ liệu quan hệ](http://www.youtube.com/watch?v=z2kbsG8zsLM) để hiểu vì sao chủ đề này hữu ích và làm quen thêm với các thuật ngữ chúng ta sẽ sử dụng.
  1. Học qua [hướng dẫn SQL của Khan Academy](https://www.khanacademy.org/computing/hour-of-code/hour-of-sql/v/welcome-to-sql) để làm quen với việc thực sự tạo và thao tác trên cơ sở dữ liệu.
  1. Cơ sở dữ liệu quan hệ không phải là cách duy nhất để lưu trữ dữ liệu. Cơ sở dữ liệu phi quan hệ (còn gọi là NoSQL) đã xuất hiện trong vài thập kỷ qua. Mặc dù chúng ta sẽ sử dụng SQL trong chương trình học này, hãy xem bài viết về [sự khác biệt giữa SQL và NoSQL](https://circleci.com/blog/SQL-vs-NoSQL-databases/) để tìm hiểu thêm.

</div>
