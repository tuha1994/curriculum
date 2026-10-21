### Giới thiệu

Với media query, bạn có thể thay đổi hoàn toàn kiểu hiển thị của dự án web dựa trên kích thước màn hình của người dùng. Tất cả bài học trước đây tập trung vào việc bảo đảm từng phần tử trong bố cục linh hoạt hết mức có thể, nhưng đôi khi bạn sẽ cần thực sự *thay đổi* một số giá trị CSS để phù hợp với một kích thước màn hình cụ thể. Những thay đổi này có thể rất tinh tế, chẳng hạn điều chỉnh margin, padding hoặc font-size để đưa thêm nội dung lên màn hình, hoặc có thể là những thay đổi lớn, rõ ràng trong bố cục. Bản chất của thay đổi cụ thể sẽ phụ thuộc vào thiết kế của bạn, nhưng kỹ thuật nền tảng vẫn giống nhau.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Áp dụng style có điều kiện bằng media query.
- Container query.

### Cú pháp media query

Cú pháp cơ bản của media query như sau:

```css
body {
  margin: 24px;
}

@media (max-width: 600px) {
  body {
    margin: 8px;
  }
}
```

Trong ví dụ trên, margin được thay đổi dựa trên kích thước màn hình. Cụ thể, trên mọi màn hình *nhỏ hơn hoặc bằng* 600px, margin sẽ là `8px`, còn trên mọi màn hình *lớn hơn* `600px`, nó sẽ là `24px`.

Thực sự thì chỉ có vậy. Chỉ với kiến thức này, bạn đã có thể tạo ra những bố cục chuyển đổi khá phức tạp. Bạn có thể tạo không giới hạn số lượng media query trong một tài liệu (hãy nhấp vào nút 'Edit on CodePen' để thay đổi kích thước màn hình và xem sự thay đổi):

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="yLzYgZw" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/yLzYgZw">
  Media Queries 1 | CSS Responsiveness</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn cũng có thể đặt bao nhiêu định nghĩa style tùy ý bên trong một media query:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="XWempGr" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/XWempGr">
  Media Queries 2 | CSS Responsiveness</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Các query khác

Trong tất cả ví dụ trên, query của chúng ta chỉ định `max-width`, thuộc tính này sẽ áp dụng style cho mọi độ phân giải màn hình *nhỏ hơn* hoặc bằng giá trị được chỉ định. Nói cách khác: query `max-width` sẽ áp dụng cho mọi màn hình có chiều rộng không vượt quá `max-width` đã định nghĩa. Bạn cũng có thể định nghĩa `min-width`, áp dụng cho mọi độ phân giải màn hình *lớn hơn* hoặc bằng giá trị đã cho. `max-height` và `min-height` cũng hợp lệ.

### Hạn chế media query

Như đã đề cập, bạn có thể tạo không giới hạn số lượng media query cho mọi kích thước màn hình có thể có. Tuy nhiên, tốt nhất là nên giảm việc sử dụng media query và dựa nhiều hơn vào tính linh hoạt tự nhiên của bố cục. Hãy xem xét ví dụ nhúng thứ hai ở trên ("my cool site"). Nó chỉ *cần* một media query để phù hợp với mọi kích thước desktop và mobile, và không thực sự cần tạo thêm.

### Breakpoint thường gặp

'Breakpoint' là thuật ngữ chỉ kích thước màn hình kích hoạt media query của bạn. Bạn sẽ thấy rất nhiều ý kiến khác nhau về chính xác breakpoint nên là gì. Nhìn chung, sẽ hữu ích nếu bạn nghĩ đến loại thiết bị và màn hình mà người dùng sẽ sử dụng. Điện thoại thường nhỏ hơn `500px`. Tablet thường nằm trong khoảng `500px` đến `1000px`. Mọi thứ lớn hơn `1000px` có khả năng là màn hình trình duyệt thông thường. Màn hình siêu rộng cũng ngày càng phổ biến, nghĩa là website *có thể* được xem trên màn hình rộng hơn `2000px`!

Điều này *không* có nghĩa là bạn nên bắt đầu dự án bằng media query cho từng thiết bị. Mỗi dự án sẽ có yêu cầu khác nhau dựa trên thiết kế bạn muốn đạt được. Như đã nói ở trên, hãy cố gắng giới hạn breakpoint ở mức bạn *cần*. Với nhiều bố cục tương đối cơ bản, bạn có thể chỉ cần một breakpoint tập trung vào mobile ở khoảng `500`-`600px`. Bố cục phức tạp hơn có thể hưởng lợi từ bố cục đầy đủ phía trên `1200px`, bố cục "tablet" đã điều chỉnh trong khoảng `600px` đến `1200px`, và mobile dưới `600px`. Điều thực sự cần rút ra ở đây là vị trí chính xác của breakpoint không quá quan trọng; chỉ cần làm điều hợp lý cho dự án của bạn.

### Phóng to!

Trong hầu hết trình duyệt, **phóng to một trang web sẽ thay đổi độ phân giải hiệu dụng của trang đó**. Nếu cửa sổ trình duyệt rộng chính xác `1000px`, việc phóng to sẽ khiến trang hoạt động như thể màn hình *nhỏ hơn*, đồng thời kích hoạt media query dựa trên độ phân giải màn hình được mô phỏng/phóng to. Phóng to *thu nhỏ* có thể hữu ích khi gỡ lỗi các vấn đề phát sinh trên màn hình lớn hơn màn hình máy tính của bạn. Quên rằng bạn đã phóng to hoặc thu nhỏ một trang web có thể gây ra nhiều nhầm lẫn khi breakpoint không kích hoạt đúng thời điểm.

### Style khi in

Bạn thường sẽ thấy media query được định nghĩa với từ khóa `screen` như sau:

```css
@media screen and (max-width: 480px) {
}
```

Điều này không bắt buộc, nhưng *có* gợi mở một khả năng rất hữu ích khác của media query: thay đổi style dựa trên loại môi trường. Mọi thứ chúng ta đã đề cập cho đến nay đều dành riêng cho việc xem trên một loại màn hình nào đó, nên việc chỉ định `screen` là dư thừa. Tuy nhiên, bạn có thể tạo một bộ style khác cho website khi gửi tới máy in hoặc xem ở chế độ xem trước khi in bằng cách dùng từ khóa `print`.

```css
@media print {
  /* print styles go here! */
}
```

Đây không phải nội dung chúng ta sẽ tập trung trong curriculum, nhưng trong một số trường hợp, bạn có thể muốn tận dụng nó. Việc thay đổi một số màu (ví dụ: chuyển thành đen/trắng) và thêm `display: none` để ẩn các phần tử vô ích trong môi trường in (nút, liên kết điều hướng, v.v.) khá phổ biến.

### Container query

Trong khi media query cho phép bạn áp dụng style có điều kiện dựa trên các đặc điểm như viewport hoặc thiết bị, container query (`@container`) cho phép bạn làm điều tương tự dựa trên đặc điểm của một phần tử "container". Tùy thuộc vào style cụ thể bạn muốn áp dụng có điều kiện, việc dựa trên bên này hay bên kia có thể hợp lý hơn. Tất nhiên, cú pháp có những khác biệt nhỏ, nhưng về cơ bản khái niệm là giống nhau và như thường lệ, có rất nhiều tài liệu.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem qua [Using media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) trên MDN. Có một vài việc bổ sung bạn có thể làm với media query đáng để biết, dù chúng ít được sử dụng hơn nhiều.

</div>