### Giới thiệu

Một số người không thể dùng chuột để điều hướng hoặc điều khiển máy tính và các website họ truy cập. Họ có thể dựa vào bàn phím hoặc công nghệ hỗ trợ mô phỏng thao tác bàn phím, chẳng hạn phần mềm nhận dạng giọng nói. Một số người khác thích dùng bàn phím hơn chuột hoặc kết hợp cả hai. Những người này cần keyboard navigation đúng cách, điều rất dễ bị bỏ qua khi phát triển website.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Biết hai điều mà phần tử tương tác phải có cho người dùng bàn phím.
- Hiểu focus style là gì và vì sao không nên xóa hoàn toàn.
- Hiểu tab order là gì.
- Biết cách ẩn nội dung đúng cách khỏi assistive technologies.

### Focus

Hãy nhớ ví dụ Rock, Paper, Scissors từng *không* dùng semantic HTML trong bài Semantic HTML. Một vấn đề khác của việc dùng `<div>` và `<span>` là mặc định chúng không focus được và không có event handling. Để sửa ví dụ Rock, Paper, Scissors phi ngữ nghĩa cho người dùng bàn phím, ta cần thêm các bước như trong các đoạn code dưới đây:

```html
<!-- The `tabindex` attribute makes the `<div>` elements focusable. -->
<div class='button-container'>
  <div class='rock button' tabindex='0'>Rock</div>
  <div class='paper button' tabindex='0'>Paper</div>
  <div class='scissors button' tabindex='0'>Scissors</div>
</div>
```

```javascript
// We also need to manually add in event handling for both mouse and keyboard events.
const buttons = document.querySelectorAll('.button');

function nameAlerter(e) {
  if (e.type === 'click' || e.key === ' ' || e.key === 'Enter') {
    alert(e.target.textContent);
  }
}

buttons.forEach(button => {
  button.addEventListener('click', nameAlerter)
  button.addEventListener('keydown', nameAlerter)
})
```

Dĩ nhiên, ví dụ này lại khiến screen reader khó hiểu hơn (hãy nhớ các "button" này không cung cấp ngữ cảnh). Dùng phần tử `<button>` không chỉ cung cấp ngữ cảnh cần thiết cho screen reader mà còn mặc định có thể focus và có event handling cho bàn phím: nhấn phím <kbd>Space</kbd> hoặc <kbd>Enter</kbd> khi `<button>` đang focus sẽ kích hoạt event "click".

Điểm cần nhớ là mọi phần tử tương tác phải focus được và có event handling cho bàn phím. Dùng semantic HTML đúng cách giúp mục tiêu này dễ đạt hơn; nếu cần dùng phần tử không focus được hoặc không có event handling mặc định, bạn phải tự thêm cả hai chức năng.

### Focus style

Một khía cạnh khác của phần tử có thể focus là focus style, thường là outline hoặc border bao quanh phần tử khi nó nhận focus. Có thể bạn từng xóa hoàn toàn focus style bằng CSS như ví dụ sau:

```css
/* These are so ugh-ly! Let's get rid of them. */
*:focus {
  outline: none;
  border: none;
}
```

Bạn có thể đoán rằng mình sắp được khuyên không làm vậy. Đúng thế... **Bạn không bao giờ nên xóa hoàn toàn focus style**. Hãy giữ focus style mặc định hoặc thay thế bằng focus style của riêng mình. Thêm `transform: scale()` vào button, thêm outline cho link, hoặc tăng độ rộng và opacity của border trên input đều là các lựa chọn thay thế nên cân nhắc.

Vì sao? Xóa focus style hoàn toàn có thể khiến trang không thể điều hướng và thao tác bằng bàn phím, vì người dùng không có dấu hiệu trực quan cho biết phần tử nào đang focus. Họ phải tự đếm số lần nhấn phím Tab và đoán phần tử nào *thực sự* focus được. Hãy *đúng* lúc giữ focus style và tránh làm mất dấu *tương tác*. Hãy tưởng tượng duyệt website với con trỏ vô hình, không có dấu hiệu trực quan khi con trỏ đi qua link hoặc button. Không dễ chịu chút nào, đúng không?

### Tab order

Tab order là thứ tự các phần tử trên trang nhận focus khi nhấn phím <kbd>Tab</kbd>; mặc định nó trùng với thứ tự các phần tử trong file HTML:

```html
<!-- This element is first in the tab order. -->
<div tabindex='0'>This is the first element listed in the HTML.</div>

<!-- This element is second in the tab order. -->
<div tabindex='0'>This is the second element listed in the HTML.</div>
```

Đôi khi bạn cần đổi thứ tự hiển thị bằng CSS (chẳng hạn thuộc tính `float` hoặc `order`) hoặc đổi tab order bằng thuộc tính `tabindex`. Dù dùng cách nào, hãy bảo đảm tab order khớp thứ tự trực quan. Nếu khác nhau, người dùng bàn phím có thể bối rối hoặc khó chịu vì mong một phần tử nhận focus theo layout nhưng phần tử khác lại nhận focus.

Cách tốt nhất để tránh vấn đề này là đặt các phần tử trong file HTML theo đúng thứ tự bạn muốn chúng nhận focus.

### Nội dung ẩn

Đôi khi bạn muốn ẩn nội dung cho đến khi một event cụ thể xảy ra, chẳng hạn người dùng nhấp button để mở menu hoặc modal box. Khi ẩn nội dung như vậy, phải bảo đảm nó không chỉ bị ẩn về mặt thị giác mà còn bị ẩn khỏi assistive technologies cho đến khi cần hiển thị.

Nếu không ẩn đúng, người dùng bàn phím có thể Tab vào nội dung quá sớm và mất dấu focus trực quan. Họ sẽ bối rối hoặc khó chịu khi Tab qua trang nhưng focus indicator biến mất vào nội dung đang ẩn.

Một cách ngăn hành vi này là đặt `tabindex` bằng -1 cho từng item trong nội dung ẩn, vì phần tử đó không nhận focus qua bàn phím (dù vẫn có thể focus bằng JavaScript `focus()`). Cách này sửa vấn đề cho người dùng bàn phím, nhưng assistive technologies khác vẫn có thể truy cập và đọc nội dung ẩn.

Giải pháp tốt hơn là đặt CSS `display: none` hoặc `visibility: hidden` cho container của nội dung ẩn khi nó bị ẩn, rồi gỡ hoặc override thuộc tính đó khi cần hiển thị. Cách này vừa loại item khỏi tab order vừa ngăn assistive technologies đọc chúng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem [What is Focus?](https://www.youtube.com/watch?v=EFv9ubbZLKw&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=3) để thấy các vấn đề khi thay đổi tab order, sau đó xem [Controlling focus with tabindex](https://www.youtube.com/watch?v=Pe0Ce1WtnUM&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=4) để hiểu thuộc tính `tabindex` ảnh hưởng tab order ra sao.
1. Đọc về [Skip Links](https://webaim.org/techniques/skipnav/), một dạng accessibility khác dành cho người dùng bàn phím, đặc biệt hữu ích với người phải tốn nhiều công sức để Tab qua nội dung trang.

</div>