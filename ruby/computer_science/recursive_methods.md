### Giới thiệu

Đệ quy là ý tưởng trong đó một hàm tự gọi chính nó. Khá dễ hiểu, đúng không? Đệ quy được dùng để lấy một bài toán lớn rồi bắt đầu chia nó thành những phần ngày càng nhỏ hơn ("Chia để trị"), đồng thời liên tục đưa lời giải của chúng trở lại hàm ban đầu cho đến khi đạt được một dạng đáp án nào đó và toàn bộ chuỗi lời gọi được gỡ ngược lại.

Theo [bài viết trên Wikipedia về các thuật toán Chia để trị](http://en.wikipedia.org/wiki/Divide_and_conquer_algorithm):

> Trong khoa học máy tính, chia để trị (D&C) là một mô hình thiết kế thuật toán quan trọng dựa trên đệ quy nhiều nhánh. Một thuật toán chia để trị hoạt động bằng cách đệ quy chia một bài toán thành hai hoặc nhiều bài toán con cùng loại (hoặc có liên quan), cho đến khi chúng trở nên đủ đơn giản để giải trực tiếp. Sau đó, các lời giải của những bài toán con được kết hợp để tạo thành lời giải cho bài toán ban đầu.

Đệ quy cũng có cách dùng đúng và cách dùng sai. Sự thật là bất kỳ bài toán nào bạn có thể giải bằng đệ quy thì bạn cũng có thể giải bằng những iterator mà bạn đã quen thuộc và yêu thích. Nếu bạn thấy mình tự hỏi "sao lúc nãy mình không dùng vòng lặp `while` nhỉ?" thì có lẽ bạn nên dùng nó. Bạn sẽ không thường xuyên dùng lời giải đệ quy cho một bài toán, nhưng nên hình thành cảm nhận về lúc nào nó có thể là một lựa chọn tốt. Một số bài toán còn bị chia thành quá nhiều phần, khiến bộ nhớ máy tính của bạn hoàn toàn quá tải. Cần có sự cân bằng.

Trong bài học ngắn này, bạn sẽ có cơ hội tìm hiểu thêm về thời điểm và cách sử dụng đệ quy; sau đó, ở dự án tiếp theo, bạn sẽ được áp dụng một phần kiến thức đó (vì có lẽ kiến thức sẽ chưa thực sự ghi nhớ cho đến khi bạn có cơ hội tự thử).

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Vì sao đệ quy là một kỹ thuật hữu ích để giải các bài toán lớn.
- Những hạn chế của việc sử dụng lời giải đệ quy.
- Những loại bài toán nào nên được giải bằng vòng lặp thay vì đệ quy.
- Khái niệm "độ sâu đệ quy".
- "Tràn ngăn xếp" là gì (khái niệm này, không phải trang web).
- Vì sao tràn ngăn xếp có liên quan đến các bài toán đệ quy.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương về Đệ quy](http://ruby.bastardsbook.com/chapters/recursion/) trong Bastards Book of Ruby của Dan Nguyen.
1. Xem [phần giải thích về đệ quy của Web Dev Simplified](https://www.youtube.com/watch?v=6oDQaB2one8).
1. Xem [video về Đệ quy](https://www.youtube.com/watch?v=mz6tAJMVmfM) từ CS50.
1. Đọc [phần "Các vấn đề triển khai" của bài viết trên wiki](http://en.wikipedia.org/wiki/Divide_and_conquer_algorithm#Implementation_issues) để có cái nhìn tổng quan về một số hạn chế của đệ quy.
1. Nếu chưa làm, hãy fork và clone [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises/) (có lẽ bạn đã làm việc này khi thực hiện các bài tập `/ruby_basics/` trước đó trong khóa Ruby). Dùng `cd` để vào thư mục `/recursion` và làm theo hướng dẫn trong README ở đó. Hoàn thành lần lượt từng bài tập về đệ quy và vượt qua các bài kiểm thử.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Hãy dùng trình gỡ lỗi!

   Việc theo dõi từng lớp của một hàm đệ quy đang diễn ra như thế nào có thể khá khó khăn, nên đừng tự làm khổ mình! Nếu trước đây bạn chưa dùng trình gỡ lỗi khi giải một số bài toán (nếu chưa thì bạn đã bỏ lỡ rất nhiều!), thì đây là thời điểm thích hợp để làm quen với nó, vì trình gỡ lỗi có thể giúp việc theo dõi luồng thực thi của mã dễ dàng hơn nhiều.

   </div>

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ trợ.

- ["Đệ quy trong Ruby là gì và hoạt động như thế nào?"](http://stackoverflow.com/questions/6418017/what-is-ruby-recursion-and-how-does-it-work) trên Stack Overflow
- [Đệ quy hiệu quả từ U of Alberta](http://webdocs.cs.ualberta.ca/~holte/T26/efficient-rec.html)
- [Bài viết trên blog của Natashatherobot về Đệ quy trong Ruby](https://web.archive.org/web/20210121055221/http://natashatherobot.com/recursion-factorials-fibonacci-ruby/)
- [Bài viết về các kỹ thuật lập trình hàm với Ruby](http://www.sitepoint.com/functional-programming-techniques-with-ruby-part-iii/), trong đó có đề cập đến đệ quy ở phần đầu
