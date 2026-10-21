### Giới thiệu

Hình ảnh cần được xử lý đặc biệt cẩn thận trên các website responsive. Bài học này trình bày một số vấn đề phát sinh khi làm việc với hình ảnh responsive và những lựa chọn bạn có khi xử lý chúng.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học cách khiến hình ảnh hoạt động đúng khi thay đổi kích thước bằng CSS.
- Bạn sẽ học cách cung cấp các hình ảnh khác nhau dựa trên kích thước màn hình.

### Kiến thức cơ bản

Vấn đề cơ bản nhất bạn sẽ gặp khi làm việc với hình ảnh responsive là aspect ratio, hay mối quan hệ giữa chiều rộng và chiều cao. Nếu thu nhỏ chiều rộng của hình ảnh trên màn hình nhỏ hơn mà không điều chỉnh chiều cao, hình ảnh sẽ bị méo!

Giải pháp cho vấn đề này cực kỳ đơn giản, và chúng ta đã đề cập đến nó trong một bài học trước: không định nghĩa cả chiều rộng lẫn chiều cao. Nếu hình ảnh được đặt chiều rộng linh hoạt và chiều cao là `auto`, nó sẽ giữ đúng aspect ratio.

### background-size, background-position và object-fit

Nếu bạn không muốn hình ảnh thu nhỏ (cả chiều cao lẫn chiều rộng) thì sao? `background-size` và `object-fit` là hai thuộc tính có thể mang đến thêm một chút linh hoạt (đúng là một cách chơi chữ) trong việc xử lý aspect ratio.

`background-position` và `background-size` là các thuộc tính hoạt động trên phần tử có background image, không hoạt động trên tag `img` thông thường. Bạn sẽ thấy một số ví dụ và chi tiết cụ thể trong bài đọc được giao sau đây, nhưng bằng cách dùng các thuộc tính này, bạn có thể kiểm soát khá nhiều cách hiển thị và vị trí của background image. Ví dụ, `background-position: center` sẽ bảo đảm hình ảnh luôn được căn giữa trong container, ngay cả khi container quá nhỏ để chứa toàn bộ hình ảnh. `background-size: cover` sẽ thay đổi kích thước hình ảnh để nó luôn phủ kín container, đồng thời cắt ít nhất có thể.

Đây là một demo nhỏ để bạn thử:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="powxJXV" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/powxJXV">
  background | CSS Responsiveness</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

`object-fit` hoạt động tương tự, nhưng được thiết kế cho tag `img`. Với thuộc tính `object-fit`, bạn *có thể* chỉ định chiều rộng và chiều cao cho hình ảnh, sau đó cho hình ảnh biết nó cần tự khớp với các kích thước đó như thế nào. Giá trị mặc định của `object-fit` là `fill`, làm giãn hình ảnh để vừa các kích thước, nhưng tương tự `background-size`, bạn cũng có thể yêu cầu nó `cover` hoặc `contain` hình ảnh.

Đây là demo về `object-fit` trên một hình ảnh. Hãy mở nó trên CodePen và thay đổi kích thước trình duyệt để xem hình ảnh phản ứng thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="NWgOGGX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/NWgOGGX">
  object-fit</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Kiểm soát nhiều hơn nữa!

Bạn cũng có thể dùng đúng nghĩa là những hình ảnh *khác nhau* cho các kích thước màn hình khác nhau. Điều này cho phép kiểm soát tối đa cách hiển thị hình ảnh ở các độ phân giải khác nhau. Ví dụ, thay vì chỉ tin tưởng `object-fit` sẽ giữ chủ thể của một bức ảnh trong khung, bạn có thể đưa ra một phiên bản đã cắt cho màn hình nhỏ hơn. Có hai cách để thực hiện, nhưng cách linh hoạt nhất là dùng tag `<picture>`, nội dung bạn sẽ học thêm trong bài tập.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Trước tiên, hãy xem tài liệu về [background-size](https://developer.mozilla.org/en-US/docs/Web/CSS/background-size), [background-position](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position) và [object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit). Các demo trên những trang đó sẽ giúp cách sử dụng trở nên rõ ràng.
1. [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images) của MDN là phần giới thiệu về việc cung cấp hình ảnh responsive trong HTML.
1. [Guide to the Responsive Images Syntax in HTML](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/) của CSS Tricks là một bài viết hay, đi sâu hơn vào cách thực sự triển khai hình ảnh responsive.

</div>