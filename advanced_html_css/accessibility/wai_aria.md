### Giới thiệu

Các bài trước đã trình bày nhiều cách làm website dễ tiếp cận hơn, nhưng đó mới chỉ là phần nổi của tảng băng accessibility. WAI-ARIA cung cấp các thuộc tính giúp website dễ tiếp cận hơn bằng cách thay đổi semantics và context của phần tử, cho bạn nhiều quyền kiểm soát hơn đối với cách assistive technologies nhận biết chúng. Vì chủ đề này phức tạp, bài chỉ tập trung vào một số thuộc tính hữu ích.

### Tổng quan bài học

- Biết mục đích của WAI-ARIA.
- Biết ARIA không thể làm gì.
- Hiểu năm quy tắc dùng ARIA.
- Hiểu accessibility tree là gì.
- Hiểu ba thuộc tính ARIA có thể thêm label cho phần tử.
- Hiểu thuộc tính `aria-hidden` làm gì.

### WAI-ARIA là gì?

WAI-ARIA là viết tắt của Web Accessibility Initiative's Accessible Rich Internet Applications specification. Mục đích của WAI-ARIA (thường gọi ngắn là ARIA) là định nghĩa cách làm web content dễ tiếp cận hơn khi HTML gốc không đáp ứng được. Hãy xem ARIA như phần lấp các khoảng trống accessibility của HTML gốc.

ARIA chỉ có thể thay đổi semantics hoặc context của phần tử. ARIA *không thể*:

- Thay đổi appearance của phần tử
- Thay đổi behavior của phần tử
- Thêm khả năng focus
- Thêm keyboard event handling

Khi dùng ARIA, thường bạn phải tự thêm semantics hoặc functionality còn thiếu. Hãy nhớ bài Keyboard Navigation và cách phải thêm functionality cho các "button" `<div>`.

#### Năm quy tắc của ARIA

ARIA rất mạnh khi dùng đúng nhưng cũng nguy hiểm khi dùng sai. Hãy nhớ **không có ARIA vẫn tốt hơn ARIA tệ**. WCAG có "The five rules of ARIA", một bộ quy tắc *thường* được dùng khi làm việc với ARIA:

1. Khi có thể, luôn dùng HTML element và attribute gốc thay cho ARIA.
1. Không thay đổi semantics gốc trừ khi không còn lựa chọn.
1. Mọi ARIA control tương tác phải dùng được bằng bàn phím.
1. Không dùng `role="presentation"` hoặc `aria-hidden="true"` trên phần tử focus được.
1. Mọi phần tử tương tác phải có accessible name.

### Accessibility tree

Accessibility tree dựa trên DOM. DOM biểu diễn node và object tạo nên trang, còn accessibility tree chỉ chứa thông tin liên quan đến accessibility được assistive technologies sử dụng. ARIA hoạt động bằng cách thay đổi thuộc tính của các object trong cây này. Bài học tập trung vào hai thuộc tính:

- **Name:** còn gọi là accessible name, là nội dung assistive technologies đọc cho người dùng và giúp phân biệt các phần tử cùng loại. Name có thể do label gốc, nội dung phần tử, `<label>`, `alt`...
- **Description:** nội dung assistive technologies đọc thêm bên cạnh accessible name.

### ARIA label

Trong bài Semantic HTML, bạn có thể xem thêm [Semantic HTML](https://www.theodinproject.com/lessons/node-path-advanced-html-and-css-semantic-html) để ôn lại semantics và label.

ARIA label giúp người dùng assistive technologies hiểu nội dung trên trang bằng cách override label gốc hoặc thêm văn bản mô tả. Khác `<label>`, ARIA label không chỉ dùng cho một số ít phần tử, dù vẫn có giới hạn.

Một số thuộc tính ARIA yêu cầu phần tử khác có `id`. Bạn đặt `id` cho một phần tử rồi truyền giá trị đó vào thuộc tính ARIA của phần tử kia, tạo liên kết tương tự `for` của `<label>` với `id` của input. Các `id` này có thể được dùng nhiều lần theo đúng ngữ cảnh, nhưng phải hiểu cách từng thuộc tính xử lý `id`. Trong ba loại ARIA label dưới đây, `aria-labelledby` và `aria-describedby` yêu cầu phần tử khác có `id`; `aria-labelledby` liên kết label, còn `aria-label` cung cấp tên trực tiếp.

#### aria-label

Thuộc tính `aria-label` override native label và thay đổi name property trong accessibility tree. Nên dùng khi phần tử chưa có native label. Giá trị chuỗi của `aria-label` trở thành accessible name. **Tuy nhiên**, [`aria-label` không có tác dụng trên một số HTML element](https://github.com/w3c/aria/issues/756), như `<div>` hoặc `<span>`.

Một cách dùng phổ biến là button đóng menu hoặc modal:

```html
<button type="button" aria-label="Close menu">X</button>
```

Thay vì đọc "X, button", screen reader sẽ đọc "Close menu, button". Cũng có thể dùng `aria-label` trên landmark:

```html
<nav aria-label="main navigation">...</nav>
```

Screen reader sẽ đọc "Main navigation, navigation landmark". Nếu có nhiều navigation, giá trị `aria-label` khác nhau giúp phân biệt và dễ hiểu hơn.

**Không** dùng `aria-label` để thay đổi cách phát âm. Cách sửa phát âm cho một screen reader có thể vô nghĩa với assistive technology khác như [braille](https://en.wikipedia.org/wiki/Braille) reader.

#### aria-labelledby

`aria-labelledby` override cả native label và `aria-label`. Accessible name được tạo bằng cách nối nội dung hoặc `alt` của các phần tử có `id` được tham chiếu. Các `id` này tạo ra tên *dễ hiểu* và giúp người dùng *phân biệt* các phần tử.

Bạn có thể truyền nhiều `id` và cho phần tử tham chiếu chính nó bằng `aria-labelledby`, nhưng không truyền cùng một reference nhiều lần vì các reference sau lần đầu sẽ bị bỏ qua. Mỗi `id` nên trỏ tới một phần tử phù hợp.

```html
<!-- Here's the labeling element -->
<h2 id="label">Shirts</h2>

<!-- And here's the labeled element. Note the order of the ID references passed in -->
<button type="button" id="shop-btn" aria-labelledby="label shop-btn">Shop Now</button>
```

Screen reader sẽ đọc "Shirts, shop now, button". Nhờ vậy nhiều button "shop now" có thể phân biệt và có thêm ngữ cảnh.

Ngay cả khi phần tử label bị ẩn bằng thuộc tính `hidden` hoặc CSS, nó vẫn thay đổi accessible name. Điều này hữu ích khi muốn label cho assistive technologies nhưng không muốn người nhìn thấy label.

Dù tương tự `<label>`, `aria-labelledby` không có event handling mặc định; bạn phải tự thêm bằng JavaScript. Hãy nhớ `aria-hidden` cũng là một thuộc tính riêng, không thay thế `aria-labelledby`. Thuộc tính này cần được dùng *cẩn thận*.

```html
<!-- Clicking the <label> element gives focus to the input element -->
<label for="name">Name:</label>
<input id="name" type="text">

<!-- Clicking the <div> element won't give focus to the input element -->
<div id="label">Name:</div>
<input type="text" aria-labelledby="label">
```

#### aria-describedby

`aria-describedby` thay đổi description property trong accessibility tree. Tương tự `aria-labelledby`, truyền các `id` khác vào giá trị `aria-describedby`; các phần tử được tham chiếu cũng có thể bị ẩn về mặt thị giác.

```html
<label>
  Password:
  <input type="password" aria-describedby="password-requirements">
</label>

<!-- Meaningful text + ARIA! -->
<span id="password-requirements">Password must be at least 10 characters long.</span>
```

Khi `<input>` nhận focus, screen reader sẽ đọc "Password, edit protected, password must be at least ten characters long." Người dùng được biết yêu cầu password mỗi lần input nhận focus.

### Ẩn nội dung khỏi accessibility tree

Tương tự ẩn phần tử bằng thuộc tính `hidden` hoặc CSS `display` và `visibility`, có thể dùng `aria-hidden` để ẩn phần tử như ảnh và icon trang trí khỏi accessibility tree. Khác biệt là phần tử vẫn hiển thị với người nhìn thấy. Điều này hữu ích khi đặt icon trong button, chẳng hạn Material Icons:

```html
<!-- Example 1 -->
<button type="button">
  <span class="material-icons">add</span>
  Add Book
</button>

<!-- Example 2 -->
<button type="button">
  <span class="material-icons" aria-hidden="true">add</span>
  Add Book
</button>
```

Hai ví dụ trông giống nhau, nhưng screen reader đọc Ví dụ 1 là "Add add book, button" vì text của `<span>` và button được nối vào accessible name. Ở Ví dụ 2, `<span>` bị ẩn khỏi accessibility tree nên screen reader đọc đúng "Add book, button".

Hãy cẩn thận: `aria-hidden="true"` trên phần tử cũng ẩn mọi phần tử con khỏi accessibility tree. Thêm `aria-hidden="false"` cho phần tử con không có tác dụng nếu cha vẫn có `aria-hidden="true"`.

Không đặt `aria-hidden="true"` trên phần tử focus được. Khi phần tử nhận focus, không có gì được đọc, khiến người dùng screen reader điều hướng bằng bàn phím bối rối.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc sections 1-5 của [W3's "ARIA in HTML"](https://www.w3.org/TR/html-aria/). Đây là tài liệu chi tiết nhưng bạn không cần ghi nhớ mọi thứ; chỉ cần nắm ý chính. Về sau, nhiều công cụ front end testing dùng ARIA roles để khuyến khích xây dựng accessibility ngay từ đầu, nên bạn có thể quay lại tài liệu này.
1. [ARIA live regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions) là một loại ARIA attribute khác, rất hữu ích để assistive technologies đọc các cập nhật động trên trang.

</div>