### Giới thiệu

Ở bài học trước, chúng ta đã phát hiện rằng các kiểu hiển thị khác nhau có mô hình hộp riêng, và có thể thay đổi cách tính hộp bằng cách đổi thuộc tính `box-sizing`. CSS có hai kiểu hộp: hộp `block` và hộp `inline`, quyết định cách phần tử hoạt động và tương tác. Thuộc tính `display` kiểm soát cách các phần tử HTML xuất hiện trên trang web. Trong bài này, chúng ta sẽ tìm hiểu thêm về các tùy chọn khác nhau của thuộc tính đó.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Bạn sẽ tìm hiểu về "Normal flow".
- Bạn sẽ tìm hiểu sự khác nhau giữa các phần tử `block` và `inline`.
- Bạn sẽ tìm hiểu những phần tử nào mặc định là `block` và những phần tử nào mặc định là `inline`.
- Bạn sẽ tìm hiểu div và span là gì.

### Block và inline

Hầu hết những phần tử bạn đã học cho đến nay đều là phần tử block. Nói cách khác, kiểu hiển thị mặc định của chúng là `display: block`. Theo mặc định, các phần tử block sẽ xuất hiện trên trang, xếp chồng lên nhau; mỗi phần tử mới bắt đầu trên một dòng mới.

Tuy nhiên, các phần tử inline không bắt đầu trên một dòng mới. Chúng nằm cùng dòng với những phần tử được đặt bên cạnh. Một ví dụ rõ ràng về phần tử inline là một liên kết, hay thẻ `<a>`. Nếu đặt một thẻ như vậy vào giữa đoạn văn, [liên kết sẽ hoạt động như một phần của đoạn văn](https://www.youtube.com/watch?v=dQw4w9WgXcQ). Ngoài ra, padding và margin cũng hoạt động khác trên các phần tử inline. Nhìn chung, bạn không nên cố thêm padding hoặc margin vào các phần tử inline.

#### Inline-block: điểm trung gian

Các phần tử inline-block hoạt động như phần tử inline, nhưng có padding và margin kiểu block. `display: inline-block` là một công cụ hữu ích cần biết, nhưng trên thực tế, nếu muốn xếp nhiều hộp thành hàng, có lẽ bạn sẽ thường dùng flexbox hơn. Flexbox sẽ được trình bày chi tiết trong bài học tiếp theo.

### Div và span

Khi xây dựng một trang web, không phải mọi mẩu nội dung đều có một thẻ ngữ nghĩa cụ thể. Đôi khi, bạn chỉ cần một vùng chứa có thể tạo kiểu hoặc định vị! Đó là lúc `div` và `span` phát huy tác dụng.

`div` là một vùng chứa cấp block chung. Theo mặc định, nó hoạt động như một khu vực hình chữ nhật kéo dài hết chiều rộng của phần tử cha và bắt đầu trên một dòng mới. Thông thường, bạn sẽ dùng `div` để nhóm các phần tử có liên quan trong những thành phần của trang, chẳng hạn như các thẻ, khu vực, thanh bên hoặc thanh điều hướng.

Trong trường hợp này, khi bọc nội dung trong một `div` rồi gán cho nó một `class` hoặc `id`, bạn tạo ra một điểm móc thuận tiện để CSS bố cục và tạo kiểu!

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="KKXXbwR" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/KKXXbwR">
  block-inline-lesson-div-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

`span` là một vùng chứa inline chung. Nó nằm trong một dòng văn bản và chỉ chiếm đúng lượng không gian bằng nội dung bên trong. Khác với `div`, nó không bắt đầu trên một dòng mới. Bạn dùng `span` khi muốn tạo kiểu hoặc nhắm đến chỉ một phần của câu. Chẳng hạn, khi muốn làm nổi bật một từ hoặc gắn chú giải cho một cụm từ, bạn có thể bọc nó trong `span`, sau đó thêm `class` hoặc `id` để CSS tìm và thao tác với nó.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="abLLPor" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/abLLPor">
  block-inline-lesson-span-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Cả `div` lẫn `span` đều không bổ sung ý nghĩa cho nội dung như các thẻ ngữ nghĩa, vì chúng tồn tại để cung cấp cho bạn những "khối xây dựng" linh hoạt. Điểm khác biệt cốt lõi là cách chúng hoạt động trong bố cục: `div` tham gia vào luồng như một phần tử cấp block, còn `span` hoạt động như một phần tử inline. Khi đã hiểu điều đó, việc chọn giữa hai loại trở nên đơn giản! Bạn có thể dùng `div` khi nhóm và định vị những khối nội dung lớn hơn, còn dùng `span` khi tạo kiểu hoặc viết script cho những phần nhỏ trong một dòng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Khái niệm "Normal flow" được ngầm đề cập trong các tài liệu về mô hình hộp, nhưng chưa được trình bày thật cụ thể. Hãy đọc ["Normal Flow" của MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Normal_Flow) để chắc chắn rằng bạn hiểu cách các phần tử tự bố trí theo mặc định.
1. ["HTML Block and Inline Elements"](https://www.w3schools.com/html/html_blocks.asp) của W3 schools mô tả và liệt kê tất cả các phần tử block và inline mặc định.
1. Bài hướng dẫn ["Inline vs Inline-block Display in CSS"](https://www.digitalocean.com/community/tutorials/css-display-inline-vs-inline-block) của Digital Ocean có một vài ví dụ rất hay giúp làm rõ sự khác nhau giữa `inline` và `inline-block`.
1. Hãy làm các bài tập trong [thư mục `foundations/block-and-inline` của kho bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/foundations/block-and-inline) của chúng ta (nhớ rằng hướng dẫn nằm trong README), theo thứ tự:
   - `01-margin-and-padding-1`
   - `02-margin-and-padding-2`

   Lưu ý: Bạn có thể tìm lời giải cho các bài tập này trong thư mục `solution` của từng bài.
1. Bạn còn nhớ trang Recipe đã tạo để thực hành trong phần HTML Foundations chứ? Trông nó khá *đơn điệu*, đúng không? Hãy chỉnh sửa nó bằng cách thêm CSS!
   - Cách bạn tạo kiểu hoàn toàn tùy ý, nhưng bạn nên dùng phương pháp CSS bên ngoài (cho bài thực hành này và về sau). Bạn cũng nên thử dùng một số thuộc tính đã đề cập trong bài trước (màu chữ, màu nền, các thuộc tính kiểu chữ, v.v.). Hãy dành thời gian thử nghiệm các thuộc tính khác nhau để hiểu tác dụng của chúng. Hiện tại, đừng lo làm cho trang trông *đẹp*. Bài này chỉ nhằm giúp bạn luyện tập và quen với việc viết CSS, không phải để tạo ra thứ gì đó đưa vào hồ sơ xin việc.
   - Chúng ta chưa học cách dùng phông chữ tùy chỉnh cho thuộc tính `font-family`, nên trước mắt hãy xem [CSS Fonts](https://www.w3schools.com/Css/css_font.asp) để biết các họ phông chữ chung có thể dùng, và [CSS Web Safe Fonts](https://www.w3schools.com/cssref/css_websafe_fonts.asp) để xem danh sách phông chữ an toàn cho web. Phông chữ an toàn cho web là những phông chữ được cài trên hầu hết mọi máy tính hoặc thiết bị (nhưng bạn vẫn nên thêm một họ phông chữ chung làm phương án dự phòng).

</div>