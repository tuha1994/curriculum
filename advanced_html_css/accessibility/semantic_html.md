### Giới thiệu

Dù `<div>` và `<span>` hữu ích như các container tổng quát, chúng không phải lúc nào cũng thân thiện với a11y. Có thể dùng chúng cho mọi thứ, từ container đến vùng tương tác, nhưng bạn nên kiểm tra xem có phần tử phù hợp hơn không và mình có đang dùng phần tử đúng cách không.

### Tổng quan bài học

- Giải thích vì sao semantic HTML quan trọng với accessibility.
- Kể tên tám phần tử HTML định nghĩa landmark trên trang.

### Tầm quan trọng của semantics

Trong web accessibility, dùng semantic HTML quan trọng vì nó cung cấp ý nghĩa và ngữ cảnh. Một số phần tử có *ý nghĩa ngữ nghĩa* nhưng không cung cấp nhiều *ngữ cảnh* khi assistive technologies đọc, chẳng hạn phần tử `<p>`. Một số phần tử vừa có ý nghĩa ngữ nghĩa vừa được đọc kèm ngữ cảnh giúp người dùng nhận biết hoặc thao tác, như `<button>`.

`<div>` và `<span>` là hai phần tử phổ biến nhưng trung tính về ngữ nghĩa. Bản thân chúng không có ý nghĩa ngữ nghĩa và không tự cung cấp ngữ cảnh cho assistive technologies, khiến người dùng khó nhận biết, thao tác và hiểu hơn. Điều đó không có nghĩa bạn không bao giờ được dùng `<div>` hoặc `<span>`; chúng vẫn hữu ích làm container tổng quát cho layout hoặc văn bản.

Hãy xét một ví dụ. Nghĩ tới dự án cần người dùng nhấp vào thứ gì đó: Rock, Paper, Scissors; Calculator; Tic-Tac-Toe. Nếu dùng `<div>` hoặc `<span>` cho vùng có thể nhấp, mọi thứ có thể vẫn hoạt động *theo dự định*. Chẳng hạn UI Rock, Paper, Scissors trong Foundations:

```html
<!-- These are buttons... right? -->
<div class="button-container">
  <div class="rock">Rock</div>
  <div class="paper">Paper</div>
  <div class="scissors">Scissors</div>
</div>
```

Người nhìn thấy có thể chơi nếu phần tử trông giống thứ có thể tương tác. Nhưng người dùng screen reader không biết các phần tử này có ý nghĩa gì. Screen reader chỉ đọc nội dung ("Rock", "Paper" hoặc "Scissors"), và người dùng có thể nghĩ đó chỉ là văn bản rồi bỏ qua. Không có ngữ cảnh cho biết họ phải, hoặc thậm chí có thể, tương tác.

Có thể sửa bằng semantic HTML:

```html
<!-- Okay, these are *definitely* buttons -->
<div class="button-container">
  <button class="rock">Rock</button>
  <button class="paper">Paper</button>
  <button class="scissors">Scissors</button>
</div>
```

Vì `<button>` có ý nghĩa ngữ nghĩa và cung cấp ngữ cảnh, screen reader sẽ đọc nội dung cùng role: "Rock, button".

#### Dùng semantic HTML đúng cách

Hãy nghĩ về ý định của mình với người dùng và ngữ cảnh cần cung cấp. Tình huống khác nhau có thể khác nhau, nhưng luôn nên kiểm tra:

- Nếu người dùng cần nhấp vào một thứ, dù đó có phải button thật hay không, thường nên dùng `<button>`. Nó cho biết phần tử có thể tương tác bằng cách nhấp.
- Nếu muốn cung cấp dữ liệu dạng bảng, dùng `<table>` cùng các phần tử liên quan để người dùng dễ điều hướng và hiểu dữ liệu.
- Khi dùng input, luôn tạo quan hệ giữa input và `<label>`. `<label>` cung cấp ngữ cảnh, được đọc mỗi lần input được đọc; một `<label>` đúng còn tăng vùng có thể nhấp. Có hai cách tạo quan hệ:

  ```html
  <!-- Useful when you need the input itself to have an ID -->
  <label for="name">Name</label>
  <input type="text" id="name">

  <!-- Look, Ma, no ID! -->
  <label>
    Name
    <input type="text">
  </label>
  ```

- Với input, luôn dùng `type` và thuộc tính *type* phù hợp mục đích. Dùng đúng `type` giúp trình duyệt hỗ trợ người dùng. `type="text"` hợp với tên hoặc địa chỉ, còn `type="email"` hay `type="tel"` hợp với email hoặc điện thoại. Trên một số thiết bị, type đúng sẽ hiển thị các phím cần thiết; `type="tel"` có thể giúp nhập số điện thoại dễ hơn trên mobile hoặc tablet.
- Khi trình bày danh sách, dùng phần tử phù hợp (`<ol>`, `<ul>` hoặc `<dl>`) và phần tử item liên quan. Người dùng biết khi vào/ra danh sách và danh sách có bao nhiêu item.

### Heading và landmark

Heading là các phần tử `<h1>` đến `<h6>`, dùng làm tiêu đề cho các section. Landmark là phần tử HTML đại diện cho các vùng trên trang. Có tám phần tử HTML gốc định nghĩa các vùng landmark:

- `<header>` - [MDN reference page for the header element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/header)
- `<main>` - [MDN reference page for the main element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/main)
- `<footer>` - [MDN reference page for the footer element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/footer)
- `<nav>` - [MDN reference page for the nav element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/nav)
- `<section>` - [MDN reference page for the section element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/section)
- `<aside>` - [MDN reference page for the aside element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/aside)
- `<search>` - [MDN reference page for the search element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/search)
- `<form>`

Hình sau cho thấy bài học này dùng semantic HTML cho các vùng khác nhau ra sao. Bạn có thể dùng developer tools để xem chi tiết hơn.

![How a typical Odin lesson uses semantic HTML by including the nav, header, main, section, aside, and footer elements](https://cdn.statically.io/gh/TheOdinProject/curriculum/b71f39e8c0934cc6cc80daaae9ed375f00753b09/advanced_html_css/accessibility/semantic_html/imgs/semantic_html_example.png)

Dùng landmark và heading đúng cách giúp trang dễ thao tác và dễ hiểu hơn với người dùng assistive technologies: người dùng screen reader có thể điều hướng bằng lệnh bàn phím hoặc menu của screen reader, đồng thời role của các phần tử cũng được đọc để cung cấp thêm ngữ cảnh.

Nếu chỉ dùng `<div>` làm landmark và heading rồi trang trí bằng CSS, người dùng screen reader phải đi qua toàn bộ trang để tới section cụ thể và có thể không biết đâu là heading hay landmark.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Cách tốt nhất để hiểu screen reader là dùng một cái! Hãy khám phá tài liệu phù hợp và thử nghiệm. Trải nghiệm trực tiếp các khái niệm accessibility có thể rất hữu ích và giúp bạn trân trọng accessibility hơn.

   Có nhiều screen reader miễn phí và trả phí; loại bạn dùng phụ thuộc OS:

   - Người dùng Windows (kể cả WSL2) có thể cài [NVDA](https://www.nvaccess.org/download/) miễn phí.
   - macOS tích hợp [VoiceOver](https://support.apple.com/en-gb/guide/voiceover/welcome/mac).
   - Linux tích hợp [Orca](https://gnome.pages.gitlab.gnome.org/orca/help/).
   - ChromeOS tích hợp [ChromeVox](https://support.google.com/chromebook/answer/7031755).

1. Đọc [How screen readers navigate data tables](https://tink.uk/how-screen-readers-navigate-data-tables/) để thấy `<table>` đúng cung cấp nhiều ngữ cảnh thế nào.
1. Xem [Introduction to accessible tables and a screen reader demo](https://youtu.be/ACmYzyN0b3U?si=o5PptrjVGJGj2OT7&t=83) để nghe screen reader đọc table.
1. Xem [Why headings and landmarks are so important](https://www.youtube.com/watch?v=vAAzdi1xuUY&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=19) để thấy screen reader tương tác với heading và landmark ra sao.

</div>