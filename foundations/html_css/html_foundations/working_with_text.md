### Giới thiệu

Phần lớn nội dung trên web dựa trên văn bản, vì vậy bạn sẽ thường xuyên cần làm việc với các phần tử văn bản HTML.

Trong bài học này, chúng ta sẽ tìm hiểu những phần tử dựa trên văn bản mà bạn có khả năng sử dụng nhiều nhất.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Cách tạo đoạn văn.
- Cách tạo tiêu đề.
- Cách tạo văn bản in đậm.
- Cách tạo văn bản in nghiêng.
- Mối quan hệ giữa các phần tử lồng nhau.
- Cách tạo chú thích HTML.

### Đoạn văn

Bạn dự đoán đoạn văn bản sau sẽ hiển thị thế nào trên một trang HTML?

```html
<body>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor
  incididunt ut labore et dolore magna aliqua.

  Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.
</body>
```

Nó trông giống như hai đoạn văn, vì vậy bạn có thể mong đợi nó hiển thị theo cách đó. Tuy nhiên, thực tế không phải vậy, như bạn có thể thấy trong kết quả bên dưới:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="xxrKqeV" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/xxrKqeV">
  no-paragraphs-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Khi trình duyệt gặp những dòng mới như vậy trong HTML, nó sẽ nén chúng thành một khoảng trắng duy nhất. Kết quả là toàn bộ văn bản bị dồn lại thành một dòng dài.

Nếu muốn tạo các đoạn văn trong HTML, chúng ta cần dùng phần tử paragraph, phần tử này sẽ thêm một dòng mới sau mỗi đoạn văn. Phần tử paragraph được định nghĩa bằng cách bao quanh nội dung văn bản với thẻ `<p>`.

Thay đổi ví dụ trước để dùng các phần tử paragraph sẽ khắc phục vấn đề:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="mdwbmdp" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/mdwbmdp">
  pargraph-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Tiêu đề

Tiêu đề khác với các phần tử văn bản HTML khác: chúng được hiển thị lớn hơn và đậm hơn để cho biết đó là tiêu đề.

Có 6 cấp độ tiêu đề, bắt đầu từ `<h1>` đến `<h6>`. Con số trong thẻ tiêu đề biểu thị cấp độ của tiêu đề đó. Tiêu đề lớn nhất và quan trọng nhất là h1, còn h6 là tiêu đề nhỏ nhất ở cấp thấp nhất.

Tiêu đề được định nghĩa tương tự đoạn văn. Ví dụ, để tạo tiêu đề h1, chúng ta bao quanh văn bản tiêu đề bằng thẻ `<h1>`.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="LYLPLbg" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/LYLPLbg">
  HTML-headings-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Sử dụng đúng cấp độ tiêu đề rất quan trọng vì các cấp tạo ra thứ bậc cho nội dung. Tiêu đề h1 luôn nên được dùng cho tiêu đề của toàn bộ trang, còn các tiêu đề cấp thấp hơn nên được dùng cho những phần nội dung nhỏ hơn trong trang.

### Phần tử strong

Phần tử `<strong>` làm văn bản in đậm. Nó cũng đánh dấu về mặt ngữ nghĩa rằng văn bản là quan trọng; điều này ảnh hưởng đến các công cụ như trình đọc màn hình mà người dùng khiếm thị dựa vào để sử dụng website. Giọng đọc của một số trình đọc màn hình sẽ thay đổi nhằm truyền đạt tầm quan trọng của văn bản nằm trong phần tử strong. Để định nghĩa một phần tử strong, chúng ta bao quanh nội dung văn bản bằng thẻ `<strong>`.

Bạn có thể dùng strong riêng:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="qBjWXrB" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/qBjWXrB">
  HTML-single-strong-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Tuy nhiên, có lẽ bạn sẽ thấy mình dùng phần tử strong thường xuyên hơn khi kết hợp với các phần tử văn bản khác, như sau:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="wvewqJr" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/wvewqJr">
  HTML-strong-with-paragraph-exmample</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Đôi khi bạn muốn làm văn bản in đậm mà không gán cho nó ý nghĩa quan trọng. Bạn sẽ học cách làm điều đó bằng CSS trong các bài học CSS sau này của chương trình học.

### Phần tử em

Phần tử `<em>` làm văn bản in nghiêng. Nó cũng đánh dấu về mặt ngữ nghĩa rằng văn bản được nhấn mạnh, điều này một lần nữa có thể ảnh hưởng đến trình đọc màn hình. Để định nghĩa một phần tử được nhấn mạnh, hãy bao quanh nội dung văn bản bằng thẻ `<em>`.

Để dùng `<em>` riêng:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="wvewqpp" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/wvewqpp">
  HTML-single-em-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Cũng như phần tử strong, bạn sẽ thường dùng phần tử `<em>` với các phần tử văn bản khác:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="VwWZzyj" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/VwWZzyj">
  HTML-em-with-paragraph-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Lồng nhau và thụt lề

Có thể bạn đã nhận thấy trong mọi ví dụ của bài học này, chúng ta thụt lề các phần tử nằm bên trong những phần tử khác. Đây được gọi là lồng phần tử.

Khi lồng các phần tử bên trong những phần tử khác, chúng ta tạo ra mối quan hệ cha-con giữa chúng. Các phần tử được lồng là phần tử con, còn phần tử chứa chúng là phần tử cha.

Trong ví dụ sau, phần tử body là phần tử cha và paragraph là phần tử con:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="oNwjEvO" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/oNwjEvO">
  HTML-nesting-parent-child</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Cũng như trong các mối quan hệ giữa con người, phần tử HTML cha có thể có nhiều phần tử con. Các phần tử ở cùng một cấp độ lồng nhau được xem là các phần tử anh em.

Ví dụ, hai đoạn văn trong đoạn mã sau là các phần tử anh em vì cả hai đều là con của phần tử body và nằm ở cùng một cấp độ lồng nhau:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="ZEybrYx" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/ZEybrYx">
  HTML-nesting-siblings</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Chúng ta dùng thụt lề để làm cho cấp độ lồng nhau trở nên rõ ràng và dễ đọc đối với bản thân cũng như những nhà phát triển khác sẽ làm việc với HTML của chúng ta trong tương lai. Trong các ví dụ, chúng ta thụt lề mỗi phần tử con thêm hai khoảng trắng cho mỗi cấp độ lồng nhau.

Mối quan hệ cha, con và anh em giữa các phần tử sẽ trở nên quan trọng hơn nhiều khi chúng ta bắt đầu tạo kiểu cho HTML bằng CSS và thêm hành vi bằng JavaScript. Tuy nhiên, hiện tại điều quan trọng chỉ là biết sự khác nhau giữa các mối quan hệ của phần tử và thuật ngữ dùng để mô tả chúng.

### Chú thích HTML

Chú thích HTML không hiển thị trong trình duyệt; chúng cho phép chúng ta *chú thích* mã để các nhà phát triển khác hoặc chính chúng ta trong tương lai có thể đọc và hiểu thêm ngữ cảnh về những điều có thể chưa rõ trong mã.

Để viết một chú thích HTML, chỉ cần bao quanh chú thích bằng các thẻ `<!--` và `-->`. Ví dụ:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="abwoyBg" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/abwoyBg">
  HTML-comments-example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<div class="lesson-note" markdown="1">

#### Phím tắt bàn phím VSCode

Nếu thấy việc gõ cú pháp chú thích quá mệt, phím tắt sau sẽ giúp bạn nhanh chóng tạo chú thích mới, chuyển bất kỳ dòng nào thành chú thích hoặc bỏ chú thích cho một dòng:

- Người dùng Mac: <kbd>Cmd</kbd> + <kbd>/</kbd>
- Người dùng Windows và Linux: <kbd>Ctrl</kbd> + <kbd>/</kbd>

Điều quan trọng cần biết là các phím tắt này dành cho bố cục bàn phím tiếng Anh và có thể không hoạt động với các bố cục không phải tiếng Anh khác. Chúng tôi tin rằng bạn sẽ tìm được tổ hợp tương đương với bố cục bàn phím của mình.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem video [Đoạn văn và tiêu đề HTML](https://www.youtube.com/watch?v=yqcd-XkxZNM&t=35s) của Kevin Powell.
1. Xem video [Văn bản in đậm và in nghiêng trong HTML](https://www.youtube.com/watch?v=gW6cBZLUk6M&t=5s) của Kevin Powell.
1. Để luyện tập làm việc với văn bản trong HTML, hãy tạo một trang bài viết blog đơn giản sử dụng các tiêu đề khác nhau, các đoạn văn, cùng một số văn bản in đậm và in nghiêng trong đoạn văn. Bạn có thể dùng [Lorem Ipsum](https://en.wikipedia.org/wiki/Lorem_ipsum) để tạo văn bản mẫu thay cho văn bản thật khi xây dựng website. VS Code có một phím tắt để tạo Lorem Ipsum. Để kích hoạt phím tắt, nhập `lorem` vào dòng muốn đặt văn bản mẫu, sau đó nhấn phím <kbd>Enter</kbd>, và thế là bạn đã tạo được văn bản mẫu dễ dàng.

</div>