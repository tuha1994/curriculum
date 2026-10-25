### Giới thiệu

Ý tưởng cơ bản của một **cấu trúc dữ liệu** là lưu trữ dữ liệu theo cách đáp ứng nhu cầu của ứng dụng cụ thể. Bạn có thể muốn lưu một loại dữ liệu nhất định trong một mảng khổng lồ, nhưng nếu có số lượng phần tử lớn và cấu trúc lồng nhau sâu, việc tìm một giá trị cụ thể sẽ khá tốn thời gian. Vì vậy, bạn cần cân nhắc những lựa chọn khác.

Tùy vào ứng dụng, có nhiều cấu trúc dữ liệu cơ bản khác có thể hỗ trợ bạn. Sự khác biệt giữa chúng thường nằm ở sự đánh đổi giữa thời gian cần để khởi tạo cấu trúc, thời gian cần để thêm hoặc tìm phần tử, và lượng bộ nhớ mà cấu trúc chiếm dụng.

Chúng ta sẽ dành phần chi tiết về cấu trúc dữ liệu cho các khóa học thiên về khoa học máy tính hơn, nhưng phần giới thiệu này sẽ tiếp tục mở rộng một chút bộ công cụ của bạn, để bạn có thể nhận diện và giải quyết một số vấn đề mà Arrays, Hashes và Sets thông thường không đủ sức xử lý. Những cấu trúc và chiến lược mới sẽ đặc biệt hữu ích, chẳng hạn khi bạn cố tìm một giá trị cụ thể trong một lượng dữ liệu lớn hoặc lập chiến lược trước vài nước đi.

Bạn đã được giới thiệu sơ lược về **thuật toán** trong một số bài học trước, và thậm chí còn tự viết thuật toán Merge Sort trong dự án gần nhất. Bạn sẽ thấy các thuật toán sắp xếp xuất hiện khá phổ biến. Một lĩnh vực lớn khác của thuật toán là tìm kiếm, nơi từng mili-giây đều đáng kể. Khi tìm kiếm trong những kho dữ liệu khổng lồ, chất lượng của thuật toán tìm kiếm cực kỳ quan trọng. Duyệt qua một cây dữ liệu để tìm một phần tử cụ thể là một bài toán liên quan, thường gặp trong các ứng dụng xử lý nhiều dữ liệu.

May mắn là những bài toán thuật toán phức tạp này đã được giải quyết nhiều lần trong quá khứ. Hiểu *cách* chúng được giải quyết sẽ mang lại cho bạn những công cụ hữu ích để tự áp dụng vào các bài toán khác (tương tự). Về bản chất, thuật toán chỉ là những cách giải quyết vấn đề một cách có hệ thống. Trong phần giới thiệu ngắn này, chúng ta sẽ tập trung vào một vài thuật toán mà bạn có thể gặp khi tự viết mã -- tìm kiếm theo chiều rộng và tìm kiếm theo chiều sâu.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Cấu trúc dữ liệu.
- Stack và queue, cách sử dụng và điểm khác biệt giữa chúng.
- Cách tốt nhất để cài đặt stack và queue trong Ruby.
- Vì sao chúng ta có rất nhiều thuật toán tìm kiếm khác nhau.
- Thuật toán tìm kiếm theo chiều rộng (BFS) và tìm kiếm theo chiều sâu (DFS), cùng các trường hợp sử dụng.
- Khi nào BFS không thực tế.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem qua [bài viết Wikipedia về Cấu trúc dữ liệu](http://en.wikipedia.org/wiki/Data_structure) để có cái nhìn tổng quan ở mức cao.
1. Xem 10 phút đầu của [Tại sao nên học thuật toán](https://www.youtube.com/watch?v=u2TwK3fED8A). Phần còn lại thiên về toán học hơn, nếu bạn quan tâm.
1. Đọc bài [Thuật toán là gì và nó giúp bạn trở thành lập trình viên giỏi hơn như thế nào](https://web.archive.org/web/20231223233808/http://blog.thefirehoseproject.com/posts/what-is-an-algorithm/) để có thêm một cách nhìn cơ bản về thuật toán.
1. Tìm hiểu [tìm kiếm nhị phân hoạt động như thế nào](https://www.youtube.com/watch?v=DSffdCT5Cx4) qua CS50 của Harvard trên YouTube.
1. Bây giờ, chúng ta sẽ tập trung tìm hiểu về cây tìm kiếm nhị phân. Hãy bắt đầu bằng cách xem video này để tìm hiểu [cây tìm kiếm nhị phân được xây dựng từ một mảng chưa sắp xếp như thế nào](https://www.youtube.com/watch?v=FvdPo8PBQtc).
1. Tiếp theo, tìm hiểu về [nguyên lý của queue và stack](https://www.youtube.com/watch?v=6QS_Cup1YoI), những khái niệm lần lượt được sử dụng trong tìm kiếm theo chiều rộng và tìm kiếm theo chiều sâu.
1. Cuối cùng, tìm hiểu về việc tìm kiếm theo chiều rộng và tìm kiếm theo chiều sâu trên cây tìm kiếm nhị phân qua loạt video này trên YouTube:
   - [Duyệt cây nhị phân](https://www.youtube.com/watch?v=9RHO6jU--GU)
   - [Duyệt theo chiều rộng](https://www.youtube.com/watch?v=86g8jAQug04)
   - [Duyệt theo chiều sâu](https://www.youtube.com/watch?v=gm8DUJJhmY4)

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ trợ.

- [Khóa học Thuật toán tuyệt vời của Khan Academy](https://www.khanacademy.org/computing/computer-science/algorithms)
- [Khóa học Thuật toán 4 phần trên Coursera của Stanford](https://www.coursera.org/specializations/algorithms)
- [Trực quan hóa thuật toán của Mike Bostock](http://bost.ocks.org/mike/algorithms/)
- [Một khóa học miễn phí khác về thuật toán của Udacity](https://www.udacity.com/course/intro-to-algorithms--cs215)
- [Ghi chú ngắn về việc đưa Sorting, Tries và Heaps vào Ruby, của Ilya Grigorik](http://www.igvita.com/2009/03/26/ruby-algorithms-sorting-trie-heaps/)
- [Video chi tiết hơn về stack và queue](https://www.youtube.com/watch?v=idrrIMXXeHM)
- Một thảo luận trên Stack Overflow về [ưu điểm tương đối của BFS và DFS](https://stackoverflow.com/questions/3332947/what-are-the-practical-factors-to-consider-when-choosing-between-depth-first-sea).