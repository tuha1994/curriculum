### Giới thiệu

Bạn đã làm quen với một số thao tác xử lý văn bản trong các bài học Foundations. Bài học này sẽ đề cập thêm một số thuộc tính CSS hữu ích khi làm việc với văn bản.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài.

- Bạn sẽ học cách sử dụng font tùy chỉnh trong các dự án web.
- Bạn sẽ học thêm một số thuộc tính CSS liên quan đến văn bản.

### Fonts

Trong bài học Foundations, chúng ta đã tìm hiểu cách thay đổi `font-family` của một phần tử, nhưng khi đó còn bỏ qua một số sắc thái và chi tiết.

#### The system font stack

 Nếu bạn dùng thuộc tính `font-family` để đổi sang một font như `Impact` hoặc `Times New Roman`, nhưng những font đó không được cài trên máy tính của người dùng, một font dự phòng sẽ được hiển thị. Nếu bạn chưa định nghĩa font dự phòng, font HTML mặc định sẽ được dùng; font này thường khá xấu. Vì lý do đó, bạn thường thấy các dự án liệt kê một danh sách font tương đối dài.

Một danh sách phổ biến là [system font stack](https://css-tricks.com/snippets/css/system-font-stack/) của CSS Tricks.

```css
body {
  font-family: system-ui, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}
```

Mục đích của chuỗi font-family có vẻ hơi vô lý này là thử dùng font mặc định của giao diện người dùng trên hệ thống. Nó sẽ lần lượt đi qua từng font cho đến khi tìm thấy font được cài trên hệ thống, rồi sử dụng font đó. Dùng một danh sách như vậy thường cho kết quả dễ chịu, đặc biệt khi bạn hướng đến kiểu font tương đối "trung tính".

#### Web fonts

Nếu muốn dùng một font không có sẵn trên thiết bị của người dùng, bạn cần import font từ một nguồn trực tuyến, có thể là thư viện font hoặc một tài nguyên trên trang web của bạn. Cả hai cách đều import font và giúp font đó có thể được sử dụng trong thuộc tính CSS `font-family`.

Hãy nhớ rằng việc thêm font dự phòng rất quan trọng. Nếu bạn liên kết đến một API bên ngoài, bạn không thể đảm bảo URL sẽ không thay đổi hoặc API bên ngoài sẽ không ngừng hoạt động vào một thời điểm nào đó. Có một font dự phòng hợp lý có nghĩa là nếu có sự cố xảy ra thì ít nhất trang web của bạn cũng không trông như bị hỏng hoàn toàn.

Việc sử dụng các tệp được import có thể gây ra vấn đề về hiệu năng, bạn sẽ đọc thêm về điều này trong phần bài tập. Vì vậy, nếu thiết kế cho phép, tốt nhất nên dùng một danh sách font thay vì font được import. Tuy nhiên, có những lúc bạn sẽ muốn dùng font được import.

#### Online font libraries

Một cách phổ biến và dễ dàng để lấy những font *chưa được cài* trên thiết bị của người dùng là dùng thư viện font trực tuyến.

Để dùng font từ một trong những thư viện này, hãy truy cập trang web, chọn một font rồi sao chép đoạn mã từ trang web để import font đó từ máy chủ của họ vào trang web của bạn. Bạn sẽ được cung cấp một thẻ `<link>` để đặt vào HTML như sau....

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
```

... hoặc một thẻ `@import` có thể đặt ở đầu tệp CSS.

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
```

Một số thư viện font trực tuyến phổ biến gồm (nhưng không chỉ có) [Font Library](https://fontlibrary.org/), [Font Bunny](https://fonts.bunny.net/) và [Google Fonts](https://fonts.google.com/).

Tuy nhiên, một điều quan trọng cần cân nhắc khi dùng thư viện font là chính sách quyền riêng tư của thư viện và mọi quy định mà bạn cần tuân thủ. Ví dụ, [việc dùng Google Fonts API vi phạm GDPR của châu Âu](https://thehackernews.com/2022/01/german-court-rules-websites-embedding.html). Nếu lo ngại về việc tuân thủ các quy định như vậy, bạn có thể tải font từ thư viện xuống và tự lưu trữ font.

#### Self hosted fonts

Bạn cũng có thể dùng font đã tải xuống từ web. Trong tệp CSS, bạn import và định nghĩa font tùy chỉnh bằng quy tắc `@font-face`, rồi dùng nó như mọi font-family khác. Có nhiều loại định dạng tệp font; bạn có thể đọc thêm chi tiết về chúng trên [trang của fileinfo.com về các định dạng tệp font](https://fileinfo.com/filetypes/font). Tuy nhiên, hãy cẩn thận khi chọn định dạng tệp font vì không phải định dạng nào cũng được trình duyệt hỗ trợ rộng rãi. Bạn có thể xem danh sách trình duyệt và các định dạng font mà chúng hỗ trợ trên [trang CSS Web Fonts của W3 Schools](https://www.w3schools.com/css/css3_fonts.asp).

```css
@font-face {
  font-family: my-cool-font;
  src: url(../fonts/the-font-file.woff);
}

h1 {
  font-family: my-cool-font, sans-serif;
}
```

Phương pháp này *có thể* đáng tin cậy hơn việc phụ thuộc vào API font của bên thứ ba, nhưng luôn nên có một font dự phòng.

### Text styles

Bạn đã học những kiến thức cơ bản về thao tác với font trong các bài học Foundations, nhưng CSS còn làm được nhiều hơn thế khi nói đến việc thao tác với kiểu văn bản. Các quy tắc này nhìn chung khá đơn giản và dễ hiểu. Nếu có câu hỏi, bạn có thể xem tài liệu.

#### font-style

Thuộc tính này thường được dùng để làm font in nghiêng. Bạn đã học về thẻ HTML `<em>`, thẻ này sử dụng font in nghiêng, nhưng `<em>` *cũng* biểu thị rằng văn bản nó bao quanh có ý nghĩa hoặc cần được nhấn mạnh theo một cách nào đó. Một nguyên tắc đơn giản là: nếu bạn chỉ muốn văn bản in nghiêng (hoặc in đậm, gạch chân, tô sáng, v.v.), hãy dùng thuộc tính CSS. Nếu văn bản cần một dạng nhấn mạnh mang tính ngữ nghĩa, hãy dùng phần tử HTML phù hợp.

Ví dụ, nếu muốn toàn bộ văn bản tiêu đề in nghiêng, bạn nên dùng `font-style` để thực hiện. Nếu muốn một phần văn bản *ở giữa câu* hiển thị in nghiêng nhằm nhấn mạnh phần đó, dùng phần tử `em` là phù hợp. [Tài liệu MDN về phần tử Emphasis](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/em) cũng nhấn mạnh quan điểm trên.

Chúng ta nên dùng `font-style: italic;` nếu chữ in nghiêng là yêu cầu về kiểu hiển thị.

```css
h1 {
  font-style: italic;
}
```

Chúng ta nên dùng phần tử `em` nếu chữ in nghiêng là yêu cầu về việc nhấn mạnh.

```html
<p>I <em>never</em> said he stole your money</p>
<p>I never said <em>he</em> stole your money</p>
<p>I never said he stole <em>your</em> money</p>
```

#### letter-spacing

Letter spacing làm đúng như bạn dự đoán.... nó thay đổi khoảng cách giữa các chữ cái trong một từ. Điều này hữu ích để điều chỉnh những font tùy chỉnh mà bạn cảm thấy có khoảng cách quá lớn hoặc quá nhỏ. Trong một số trường hợp, nó cũng có thể tạo hiệu ứng đẹp mắt, chẳng hạn ở tiêu đề.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="MWomjGr" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/MWomjGr">
  Letter Spacing | CSS Text Styles</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Rõ ràng, hãy dùng thuộc tính này tiết chế và cẩn thận. Đừng khiến trang web khó đọc!

#### line-height

Line height điều chỉnh khoảng cách giữa các dòng trong văn bản được ngắt dòng. Thêm một chút line-height có thể tăng khả năng đọc.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="vYZmXzY" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/vYZmXzY">
  Line Height | CSS Text Styles</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### text-transform

Text transform thay đổi kiểu chữ của văn bản đã cho. Ví dụ, bạn có thể dùng thuộc tính này để buộc các thẻ tiêu đề viết toàn bộ bằng chữ hoa hoặc viết hoa chữ cái đầu của mọi từ.

Bạn có thể xem cách sử dụng trong ví dụ rõ ràng trên [tài liệu web MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform).

#### text-shadow

Đúng như bạn có thể đoán, `text-shadow` thêm bóng xung quanh văn bản trong phần tử được chọn. Thuộc tính này nên được dùng tiết chế, nhưng có thể tạo hiệu quả tốt cho tiêu đề hoặc văn bản mang tính trình bày khác.

Các ví dụ trên [trang tham khảo MDN về text-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow) cho thấy cách sử dụng thuộc tính này.

#### ellipsis

Đây không phải là một thuộc tính đơn lẻ, nhưng là một mẹo hữu ích nên có trong bộ công cụ của bạn. Với thuộc tính `text-overflow`, bạn có thể cắt ngắn văn bản bị tràn bằng dấu ba chấm. Tuy nhiên, để tạo ra tình trạng tràn, bạn cần dùng thêm một vài thuộc tính khác vì hành vi mặc định là văn bản hiển thị bên ngoài phần tử chứa về mặt kỹ thuật không được xem là một `overflow` (khá khó hiểu, chúng tôi biết. Xin lỗi.)

Đoạn đầy đủ là:

```css
.overflowing {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

Bạn có thể xem thêm chi tiết và ví dụ trong [bài viết CSS Tricks về ellipsis overflow](https://css-tricks.com/snippets/css/truncate-string-with-ellipsis/). (Hãy chuẩn bị tinh thần sẽ phải tra cứu bài viết đó mỗi lần muốn dùng mẹo này.)

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [bài viết về web fonts của MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text/Web_fonts) và làm các bài tập.
1. Đọc [bài viết của web.dev về các phương pháp hay nhất cho font](https://web.dev/articles/font-best-practices) để hiểu một số cân nhắc về hiệu năng của font được import và cách giảm thiểu vấn đề.
1. Đọc [bài viết của web.dev về typography](https://web.dev/learn/design/typography) để hiểu một số cân nhắc quan trọng đối với lập trình viên về typography.

</div>