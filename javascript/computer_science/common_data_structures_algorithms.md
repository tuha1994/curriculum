### Giới thiệu

Ý tưởng cơ bản của một **cấu trúc dữ liệu** là lưu trữ dữ liệu theo cách đáp ứng nhu cầu của ứng dụng cụ thể. Bạn có thể muốn lưu một loại dữ liệu nào đó trong một array khổng lồ, nhưng việc tìm một giá trị cụ thể sẽ khá tốn thời gian nếu có rất nhiều phần tử ở nhiều cấp. Vì vậy, bạn cần tìm những lựa chọn khác.

Tùy ứng dụng, có nhiều cấu trúc dữ liệu cơ bản khác có thể hỗ trợ bạn. Sự khác biệt giữa chúng thường liên quan đến những đánh đổi giữa thời gian cần để khởi tạo cấu trúc, thời gian thêm hoặc tìm phần tử, và kích thước cấu trúc trong bộ nhớ.

Chúng ta sẽ dành phần chi tiết về cấu trúc dữ liệu cho các khóa học thiên về computer science hơn, nhưng phần giới thiệu này một lần nữa sẽ mở rộng bộ công cụ của bạn để bạn có thể nhận diện và giải quyết một số vấn đề mà Array, Hash và Set thông thường không đủ sức xử lý. Những cấu trúc và chiến lược mới đặc biệt hữu ích khi bạn cần tìm một giá trị trong một tập dữ liệu lớn hoặc lập chiến lược trước vài bước.

Bạn đã được giới thiệu sơ lược về **algorithm** trong một số bài học trước và thậm chí đã tự viết algorithm Merge Sort trong project gần nhất. Bạn sẽ thấy algorithm sắp xếp xuất hiện khá thường xuyên. Một lĩnh vực lớn khác của algorithm là tìm kiếm, nơi từng mili-giây đều quan trọng. Khi tìm kiếm trong những kho dữ liệu khổng lồ, chất lượng của algorithm tìm kiếm cực kỳ quan trọng. Duyệt một cây dữ liệu để tìm một phần tử cụ thể cũng là một vấn đề liên quan, thường gặp trong các ứng dụng xử lý nhiều dữ liệu.

May mắn là những bài toán algorithm phức tạp này đã được giải nhiều lần trong quá khứ. Hiểu *cách* chúng được giải sẽ cung cấp cho bạn những công cụ tuyệt vời để tự áp dụng vào các vấn đề khác (tương tự). Algorithm thực chất chỉ là những cách giải quyết vấn đề một cách có hệ thống. Trong phần giới thiệu ngắn này, chúng ta sẽ tập trung vào hai algorithm bạn có thể gặp khi tự code -- breadth-first-search và depth-first-search.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài học này.

- Làm quen với cấu trúc dữ liệu.
- Tìm hiểu về stack và queue.
- Tìm hiểu tại sao nên học algorithm.
- Khám phá các algorithm tìm kiếm khác nhau.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Xem qua [Wikipedia entry on Data Structures](http://en.wikipedia.org/wiki/Data_structure) để có cái nhìn tổng quan.
  1. Xem 10 phút đầu của [Why Study Algorithms](https://www.youtube.com/watch?v=u2TwK3fED8A). Phần còn lại thiên về toán học hơn nếu bạn thấy hứng thú.
  1. Tìm hiểu [how binary search works](https://www.youtube.com/watch?v=DSffdCT5Cx4) từ CS50 của Harvard trên YouTube.
  1. Bây giờ chúng ta sẽ tập trung tìm hiểu binary search tree. Hãy bắt đầu bằng video về [how a binary search tree is constructed from an unordered array](https://www.youtube.com/watch?v=FvdPo8PBQtc).
  1. Tiếp theo, tìm hiểu [principles of queues and stacks](https://www.youtube.com/watch?v=6QS_Cup1YoI), những khái niệm được dùng lần lượt trong breadth-first search và depth-first search.
  1. Cuối cùng, tìm hiểu breadth-first search và depth-first search của binary search tree qua loạt video trên YouTube:
     - [Binary tree traversal](https://www.youtube.com/watch?v=9RHO6jU--GU)
     - [Breadth-first traversal](https://www.youtube.com/watch?v=86g8jAQug04)
     - [Depth-first traversal](https://www.youtube.com/watch?v=gm8DUJJhmY4)

</div>