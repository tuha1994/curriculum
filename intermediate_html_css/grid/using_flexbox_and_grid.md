### Giới thiệu

Một số người có thể nói rằng có một cuộc tranh luận về việc dùng Grid hay Flexbox, và liệu cái nào có vượt trội hơn cái kia hay không. Thực tế là đây là những công cụ bổ trợ cho nhau, có thể hoạt động cùng nhau, và mỗi công cụ đều có vị trí riêng trong thế giới CSS.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Biết khi nào nên dùng Flexbox thay cho Grid.
- Biết khi nào nên dùng Grid thay cho Flexbox.
- Biết khi nào nên dùng cả hai cùng nhau.

### Thiết kế ưu tiên nội dung và ưu tiên bố cục

Một cách để quyết định giữa Grid và Flexbox là xem thiết kế của bạn bắt đầu từ nội dung hay từ bố cục.

Trong thiết kế ưu tiên nội dung (Content First Design), bạn bắt đầu bằng việc làm rõ nội dung nên như thế nào, rồi bố cục đi theo sau. Đây là cơ hội tuyệt vời để dùng Flexbox. Bản chất linh hoạt của nó cho phép bạn kiểm soát *hành vi* của các item thông qua những quy tắc logic. Cách chúng tăng, co, kích thước lý tưởng và vị trí tương quan với nhau đều khiến bố cục trở nên động. Flexbox cho phép bạn kiểm soát nội dung, còn bố cục cuối cùng chỉ là hệ quả. Tùy theo kích thước của flex container, bố cục tổng thể có thể thay đổi rất nhiều.

Trong thiết kế ưu tiên bố cục (Layout First Design), bạn quyết định cách sắp xếp các phần, rồi điền nội dung vào sau. Đây là lúc Grid tỏa sáng. Việc định nghĩa các row track và column track cho phép bạn kiểm soát hoàn toàn bố cục. Nội dung trong grid chỉ có thể lấp đầy không gian của các track tường minh hoặc ngầm định. Vì vậy, khi bạn đã hình dung được bức tranh lớn của một container, Grid là lựa chọn rõ ràng.

Thiết kế ưu tiên nội dung hay bố cục không buộc chúng ta chỉ dùng Flexbox hoặc Grid! Hãy thử bố cục một tập hợp item một chiều – một công việc thường được dành cho Flexbox – bằng Grid.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="mdByJRV" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/mdByJRV">
  1-Dimensional Grid | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Nó hoạt động và trông tuyệt đẹp! Nhưng hãy tưởng tượng sau này chúng ta muốn di chuyển các hộp này. Hoặc chẳng hạn, chúng ta muốn hộp thứ ba kéo dài sang hàng thứ hai để các hộp không phải co quá nhiều nhằm vừa trong một hàng. Cả hai việc này đều *có thể* thực hiện bằng Grid. Nhưng nếu kiểm soát bố cục không phải ưu tiên của chúng ta, Flexbox trực quan hơn và phù hợp hơn với nhiệm vụ.

### Kết hợp flexbox và grid

Nếu bạn có nội dung một chiều, Flexbox có thể giúp kiểm soát cách nội dung đó được định vị trong một Flex container. Ngược lại, nếu muốn đặt nội dung chính xác trong một bố cục hai chiều phức tạp, Grid có thể dễ dùng hơn.

Giả sử bạn muốn bố cục tổng thể là một grid, nhưng muốn các grid item hoạt động như flex parent. Nhờ vậy, các grid item có thể được di chuyển bằng khả năng định vị hai chiều chính xác của Grid, đồng thời nội dung bên trong grid item vẫn có thể tự do di chuyển bằng Flex. Hãy xem ví dụ này từ CSS-Tricks.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="vYeEOxN" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/vYeEOxN">
  Combine Grid and Flexbox</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Quan điểm cuối

Bài học này đưa ra các khuyến nghị, chứ không phải cách dùng Flexbox hay Grid “đúng” hoặc “sai”. Cuối cùng, điều đó phụ thuộc vào sở thích cá nhân và việc developer thấy cách nào dễ hơn cho nhiệm vụ cụ thể. Đến đây, bạn đã có cả hai công cụ trong bộ công cụ của mình và đã thấy trước cách chúng có thể được dùng cùng nhau hoặc thay thế nhau. Cách tốt nhất để học Flexbox và Grid là xây dựng nhiều dự án với cả hai, rồi tự hình thành quan điểm về việc dùng công cụ nào trong từng hoàn cảnh.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết về việc [CSS Grid không thay thế flexbox](https://css-tricks.com/css-grid-replace-flexbox/).

1. Video này của Kevin Powell trình bày [các trường hợp sử dụng Grid và Flexbox trong thực tế](https://www.youtube.com/watch?v=3elGSZSWTbM).

1. Đọc thêm về [khi nào nên dùng Grid hoặc Flexbox và lý do](https://webdesign.tutsplus.com/flexbox-vs-css-grid-which-should-you-use--cms-30184a).

</div>