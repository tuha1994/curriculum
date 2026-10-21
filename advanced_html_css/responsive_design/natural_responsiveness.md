### Giới thiệu

Bước đầu tiên để tạo website responsive là sử dụng những kỹ thuật *tự nhiên* linh hoạt. Trong một bài học sau, bạn sẽ học cách sắp xếp lại hoàn toàn các phần tử trên trang dựa trên kích thước màn hình, nhưng trong hầu hết trường hợp, trước tiên nên dựa vào các công cụ như flexbox và grid để trang hoạt động trên nhiều loại màn hình.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học một số mẹo để giữ cho website responsive một cách tự nhiên.

### Không phải mọi thứ đều cần CSS

HTML thuần, không có CSS, vốn đã responsive. Hãy truy cập [trang chỉ có HTML và không có CSS này](https://codyloyd.github.io/responsive-html/) rồi thu nhỏ trình duyệt xuống kích thước điện thoại. Trang hoạt động hoàn hảo! Bạn thậm chí có thể đọc nó trên một chiếc Apple Watch.

Việc mọi website bạn tạo đều đơn giản như văn bản thuần trên một trang là không thực tế, nhưng điều quan trọng là nhớ rằng hầu hết phần tử bạn dùng để xây dựng dự án đều responsive cho đến khi **bạn** thay đổi điều đó bằng CSS. Nếu tiếp cận dự án với tư duy này và cố gắng duy trì tính responsive tự nhiên, bạn có thể thấy rằng mình không cần làm *quá* nhiều việc thêm để website thực sự responsive.

Phần còn lại của bài học là danh sách các mẹo bạn có thể dùng để duy trì tính responsive tự nhiên.

### Thẻ meta viewport

Khi điện thoại di động mới bắt đầu có trình duyệt web, hầu hết website *không* được tối ưu cho độ phân giải màn hình nhỏ như vậy. Để giải quyết, hầu hết trình duyệt điện thoại mô phỏng một màn hình lớn hơn và hiển thị phiên bản thu nhỏ của trang. Tuy nhiên, ngày nay chúng ta gần như không bao giờ muốn hành vi đó, vì vậy phải chỉ định rằng website được xem ở độ phân giải thực tế, không phóng to hay thu nhỏ.

Vì lý do này, bạn nên thêm đoạn mã sau vào `<head>` của file HTML trong gần như mọi dự án.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Đoạn mã đặt chiều rộng ban đầu của webpage bằng kích thước màn hình thực tế đang xem, đồng thời yêu cầu không phóng to hay thu nhỏ. Đơn giản phải không!

Rất có thể bạn đã dùng thẻ này nếu sử dụng Emmet để tạo HTML boilerplate. (<kbd>!</kbd> + <kbd>ENTER</kbd>)

### Tránh chiều rộng và chiều cao cố định

Kẻ thù số một của tính linh hoạt là chiều rộng cố định trên một phần tử. Nếu đặt `width: 600px` cho bất kỳ thứ gì, nó sẽ không bao giờ thu nhỏ xuống dưới chiều rộng đó, làm hỏng khả năng khiến phần tử vừa với hầu hết màn hình điện thoại. Tương tự, đặt chiều cao cố định cho phần tử có thể gây vấn đề nếu nội dung bên trong không còn đủ chỗ.

Rõ ràng, ngữ cảnh sẽ quyết định điều gì phù hợp trong từng tình huống, nhưng trong nhiều trường hợp, cách sửa đơn giản là thay `width` hoặc `height` bằng `max-width` hoặc `min-height` (`min-width` và `max-height` cũng hợp lệ và có thể hữu ích tùy ngữ cảnh).

Trong ví dụ sau, hãy chú ý cách chiều rộng cố định khiến div tràn khỏi màn hình! Đổi nó thành `max-width` và xem nó thu nhỏ để vừa màn hình xem trước! Nhấp vào 'Fork on CodePen', sau đó 'Save', rồi chuyển đến biểu tượng 'Open Live View in a New Window' ở phía dưới để xem ở kích thước cửa sổ đầy đủ. Thử thay đổi kích thước cửa sổ để xem `max-width` hoạt động thế nào. Khi `max-width` được định nghĩa, phần tử sẽ không vượt quá chiều rộng đó nhưng sẽ thu nhỏ nếu màn hình quá nhỏ để chứa nó.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="GRMpreM" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/GRMpreM">
  max-width | CSS Responsiveness</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Ví dụ dưới đây minh họa vấn đề với chiều cao tĩnh. Trên màn hình đầy đủ, văn bản có thể vừa vặn trong hộp màu hồng, nhưng trong phần nhúng chật chội này, nó bị tràn! Điều chúng ta muốn là div có `300px` trong hầu hết trường hợp, nhưng sẽ phát triển thay vì gây tràn khi văn bản bị chèn ép. Trong trường hợp này, đổi `height: 300px` thành `min-height: 300px` sẽ khắc phục vấn đề.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="qBjxVYg" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/qBjxVYg">
  height | CSS Responsiveness</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Tránh hoàn toàn việc đặt chiều cao

Trong *hầu hết* trường hợp, bạn nên tránh đặt height hoàn toàn. Có một số ngoại lệ cho quy tắc này (có thể là header và footer), nhưng bạn nên ưu tiên dùng margin và padding để tăng khoảng trống quanh nội dung. Dùng margin và padding sẽ giữ cho phần tử linh hoạt bất kể nội dung bên trong thay đổi thế nào.

#### Khi nào chiều rộng cố định phù hợp

Có những trường hợp chiều rộng cố định là phù hợp. Khó đưa ra một quy tắc chung, nhưng nhìn chung, chiều rộng càng nhỏ thì càng có thể chấp nhận việc đặt cố định. Ví dụ, một icon `32px` trên trang sẽ không hưởng lợi từ `max-width` vì có lẽ bạn *không* muốn nó thu nhỏ. Tương tự, một sidebar `250px` có lẽ cần *luôn* là `250px`. Như với mọi thứ khác, bạn cần cân nhắc các lựa chọn và chọn điều có vẻ phù hợp nhất.

### Dùng flex và grid

Sau đây là một nhận định hiển nhiên đến mức nghe như một trò đùa: flexbox được *tạo ra* để cho phép xây dựng các bố cục linh hoạt. Dùng flex và grid không đảm bảo responsive hoàn hảo, nhưng chúng thực sự là những công cụ hữu ích. Bạn đã học các thuộc tính liên quan ở đây, nhưng những thứ như `flex-wrap` và `minmax`, `auto-fill` cùng các thuộc tính tương tự của grid có thể tạo ra những bố cục responsive ấn tượng mà không cần nhiều công sức thêm.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc ["Using the viewport meta tag"](https://developer.mozilla.org/en-US/docs/Web/HTML/Viewport_meta_tag) trên MDN để hiểu thêm về thẻ meta viewport và bản chất của độ phân giải màn hình.
1. [Using Percentages in CSS](https://web.archive.org/web/20251116005914/https://codyloyd.com/2021/percentages/) là một bài viết đề cập đến một cạm bẫy phổ biến khác. Đừng tập trung quá nhiều vào phần `@media`, vì chúng ta sẽ sớm học về media query.

</div>