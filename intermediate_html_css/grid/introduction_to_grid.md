### Giới thiệu

Trong vài bài học tiếp theo, chúng ta sẽ tìm hiểu CSS Grid và làm cho việc bố cục trang trở nên dễ dàng hơn nhiều. Trước tiên, chúng ta sẽ ôn nhanh về Flexbox (còn nhớ Flexbox chứ!?) rồi tìm hiểu về Grid.

Các bài học sau sẽ chỉ cho bạn cách tạo một grid, định vị các phần tử trong grid và sử dụng một số thuộc tính nâng cao. Sau đó, chúng ta sẽ xem xét sâu hơn sự khác nhau giữa Flexbox và Grid. Mục tiêu cuối cùng là xây dựng một dự án dashboard bằng Grid.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- So sánh những điểm cơ bản giữa Flexbox và Grid.
- Mô tả một tình huống nên dùng Grid thay cho Flexbox.

### Ôn lại về flex

Trong khóa Foundations, bạn đã học rất nhiều về Flexbox. Nếu bạn đã sử dụng Flex ở đây đó trong quá trình học, phần này sẽ là một bài ôn nhanh trước khi chúng ta bắt đầu với Grid. Nếu bạn hoàn toàn chưa nắm được Flex, bạn có thể xem lại [các bài học về Flex](https://www.theodinproject.com/lessons/foundations-introduction-to-flexbox) để bắt kịp.

Các bài học về Flex đã đề cập đến việc định vị các phần tử theo [hai trục flex (trục chính và trục phụ)](https://www.theodinproject.com/lessons/foundations-axes) và cách thiết lập [căn chỉnh flex](https://www.theodinproject.com/lessons/foundations-alignment). Bạn cũng đã học [cách làm cho các flex item tăng, co hoặc thay đổi kích thước](https://www.theodinproject.com/lessons/foundations-growing-and-shrinking). Đây chính là điểm thú vị của Flexbox: các phần tử có thể “flex”, tức là kéo giãn ra hoặc co lại.

Bạn sẽ nhớ rằng mình có thể xếp một hàng flex item đẹp mắt như sau:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="XWeJbRy" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/XWeJbRy">
  Single Row | CSS Flexbox</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hoặc một cột như sau:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="MWEYwoX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/MWEYwoX">
  Single Column | CSS Flexbox</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Đối với bố cục một chiều, Flex cung cấp một công cụ thuận tiện mà không cần dựa vào float hoặc các mẹo CSS để căn chỉnh đúng các phần tử.

Đối với bố cục hai chiều, bạn đã học một chút về `flex-wrap`, cho phép đưa các flex item xuống dòng tiếp theo. Việc này có thể thực hiện với một hàng chuyển sang hàng khác hoặc một cột chuyển sang cột khác.

Bạn còn nhớ mình đã vui thế nào khi giải bài tập bố cục card này không?

[![flex-exercise-desired-outcome.png](https://i.postimg.cc/vZ81HMkB/flex-exercise-desired-outcome.png)](https://github.com/TheOdinProject/css-exercises/tree/main/foundations/flex/07-flex-layout-2)

Chúng ta biết bài đó khá khó chịu, nhưng đó là một phần của mục đích bài tập. Mặc dù Flexbox cho phép bạn xây dựng bố cục gồm cả hàng và cột, việc này không phải lúc nào cũng dễ.

Nhưng việc thiết lập bố cục hai chiều cho các card sẽ dễ hơn nhiều khi dùng CSS Grid:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="KKXwpZR" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/KKXwpZR">
  2D Layout | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Grid là gì?

Mặc dù Grid là một module mới hơn của CSS, công cụ bố cục này đã được phát triển từ lâu. Một sự thật thú vị: đồng sáng tạo viên CSS, Tiến sĩ Bert Bos (không có họ hàng với Wes Bos), bắt đầu làm việc trên mô hình bố cục này từ năm 1996. Ý tưởng được truyền cảm hứng từ việc sử dụng các bố cục dạng lưới trong những hình thức truyền thông khác như báo và tạp chí. Sau nhiều năm trình diễn và phát triển kỹ lưỡng, CSS Grid cuối cùng cũng được giới thiệu trên tất cả các trình duyệt lớn vào năm 2017.

Như đã thấy ở trên, Grid thường được đánh giá cao vì khả năng dễ dàng đặt các phần tử trong bố cục hai chiều. Nhưng Grid cũng có thể dùng cho bố cục một chiều. Một lợi thế đối với developer là nếu ban đầu họ chỉ có một hàng phần tử, họ vẫn có thể thêm các hàng khác về sau.

Bạn sẽ nhận thấy Flex và Grid có nhiều điểm tương đồng. Cả hai đều dùng container cha với các item con. Chúng có tên thuộc tính tương tự để căn chỉnh và định vị. Nhưng bạn cũng sẽ gặp nhiều khác biệt giữa hai module này, cùng các quan điểm về cách nên sử dụng mỗi module. Ví dụ, nếu bạn đang chật vật để làm cho tất cả flex item có kích thước đều nhau, Grid có thể khiến kiểu bố cục này dễ hơn nhiều.

Khi xem các tài liệu cũ, hãy nhớ rằng khác biệt giữa Flex và Grid cũng có thể thay đổi khi các module này được cập nhật. Một điểm nổi bật của CSS Grid là thuộc tính `gap`, chúng ta sẽ học ở bài tiếp theo. Trước đây thuộc tính này chỉ có trong Grid, nhưng hiện nay Flex cũng hỗ trợ.

Một số người từng nghĩ CSS Grid ra đời để thay thế Flexbox, nhưng đến cuối các bài học này bạn sẽ thấy Grid chỉ là thêm một công cụ vào bộ công cụ. Thực tế, mỗi module đều có trường hợp sử dụng riêng, và bạn cũng sẽ thấy việc kết hợp Flex với Grid rất hữu ích. Chúng ta sẽ tìm hiểu tất cả điều đó trong bài tổng kết. Trước hết, bạn sẽ học cách thực sự tạo một grid!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bất ngờ chưa! Không có bài tập! Bạn sẽ học thêm rất nhiều trong các bài sắp tới, nên hãy tiếp tục.

</div>