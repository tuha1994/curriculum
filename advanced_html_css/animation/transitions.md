### Giới thiệu

Đã đến lúc bước vào thế giới thú vị của CSS transitions và tạo cho các phần tử HTML những biến đổi mượt mà!

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- CSS transition là gì và khi nào nên sử dụng nó.
- Biết những thuộc tính CSS nào có thể tạo animation và những thuộc tính nào không thể.
- Cách đảm bảo transitions mượt mà và có hiệu năng tốt.

### Transitions

CSS transitions cho phép bạn tạo animation cho một thay đổi từ trạng thái ban đầu của phần tử đến trạng thái kết thúc. Hãy hình dung một phần tử button thông thường có nền màu trắng. Khi chuột rời khỏi button, nó chỉ nằm yên ở đó. Thật nhàm chán. Sau đó, khi bạn di con trỏ chuột lên button, màu nền sẽ chuyển mượt từ trắng sang xám rồi sang đen trong một khoảng thời gian. Đó là CSS transition. Hãy xem CodePen bên dưới để thấy nó hoạt động.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="eYGmYRm" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/eYGmYRm">
  CSS Transition (longhand)</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Khi con trỏ chuột rời khỏi button, button đang ở trạng thái ban đầu. Khi bạn di chuột lên nó, bạn đưa nó vào trạng thái kết thúc — trạng thái hover — khiến màu nền chuyển mượt từ trắng sang đen.

Điều này được thực hiện bằng thuộc tính `transition`, thực chất là thuộc tính viết tắt của `transition-property`, `transition-duration`, `transition-timing-function` và `transition-delay`.

```css
button {
  /* ... other CSS properties ... */
  transition-property: background-color;
  transition-duration: 1s;
  transition-timing-function: ease-out;
  transition-delay: 0.25s;
}
```

- `transition-property` - Xác định thuộc tính CSS nào sẽ được chuyển tiếp. Trong trường hợp này là `background-color`.
- `transition-duration` - Xác định thời lượng transition diễn ra. Trong trường hợp này, việc đổi màu sẽ diễn ra dần trong 1 giây.
- `transition-timing-function` - Cho phép thay đổi tốc độ của transition trong suốt thời lượng của nó. Ở đây, nó sẽ `ease-out`, nghĩa là việc đổi màu nhanh hơn ở đầu transition và chậm hơn ở cuối.
- `transition-delay` - Xác định thời gian chờ trước khi transition bắt đầu. Trong trường hợp này, việc đổi màu bắt đầu một phần tư giây sau khi con trỏ dừng trên button.

Thuộc tính viết tắt trông như sau:

```css
button {
  /* ... other CSS properties ... */
  background-color: white;
  transition: background-color 1s ease-out 0.25s;
}

button:hover {
  background-color: black;
}
```

Transition trên sẽ diễn ra khi người dùng di chuột lên button, như được chỉ ra bởi pseudo-class `:hover`. Ngoài `:hover` và các pseudo-class khác, bạn cũng có thể kích hoạt transitions bằng cách thêm hoặc xóa class với JavaScript. Ví dụ, khi nhấp vào button, bạn có thể thêm class "open" vào một menu dropdown, từ đó kích hoạt transition mở menu.

### Hiệu năng

Nhìn chung, việc giữ cho CSS transitions có hiệu năng tốt sẽ không phải vấn đề. Tuy nhiên, có một vài điều bạn cần lưu ý.

Điều đầu tiên là "stacking context". "Stacking context" chỉ việc xếp chồng hoặc xếp lớp các phần tử HTML trong một container cha dọc theo trục z (chiều sâu). Vị trí của một phần tử trong stacking context chỉ có quan hệ tương đối với các phần tử khác trong cùng stacking context. Thuộc tính `z-index` kiểm soát việc một phần tử xuất hiện ở phía sau hay phía trước đến mức nào trên trang web khi các phần tử chồng lên nhau. Ngoài việc đặt `z-index` cho một phần tử, một số thuộc tính CSS cũng có thể kích hoạt việc tạo stacking context mới, chẳng hạn `transform`, `opacity`, `filter`, v.v. Một tình huống liên quan ở đây là chuyển tiếp thuộc tính `transform` như sau:

```css
div {
  width: 100px;
  height: 100px;
  transition: transform 2s 1s;
}

div:hover {
  transform: rotate(180deg);
}
```

Đoạn code này đã tạo ra một stacking context. Nếu tạo thêm nhiều stacking context bằng nhiều cách khác nhau, khi trình duyệt render transform ban đầu, nó sẽ repaint không chỉ phần tử `div` mà còn **mọi phần tử được xếp chồng lên trên nó trong stacking context**. Nếu không kiểm soát, điều này có thể khiến transition từng mượt mà trở nên chậm và giật.

Điều thứ hai cần ghi nhớ là nếu muốn đạt hiệu năng tốt nhất cho các animation trên trang web, bạn nên giới hạn animation chỉ tác động đến `opacity` và `transform`. Đúng là ví dụ đầu tiên chỉ thay đổi `background-color`, nhưng ngay cả thao tác đó cũng vốn đã tốn kém.

Điều quan trọng là bạn hiểu vững các khái niệm này và có thể áp dụng khi cần, bởi vì nếu muốn biến một button thành cầu vồng khi được hover, bạn phải có khả năng tạo transition cho cầu vồng đó!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết MDN về [cách sử dụng CSS transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions), bao gồm các liên kết trong phần [Defining transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions#defining_transitions) của bài viết. Hãy nhớ viết code theo các ví dụ để làm quen với cú pháp.
1. Đọc bài viết này về [stacking context](https://www.joshwcomeau.com/css/stacking-contexts/).
1. Thử [tiện ích CSS Stacking Context inspector](https://chromewebstore.google.com/detail/apjeljpachdcjkgnamgppgfkmddadcki?utm_source=item-share-cb) trên trình duyệt để khám phá các stacking context trên một trang, xem cấu trúc phân cấp của chúng và xử lý các vấn đề về bố cục hoặc repaint.
1. Tìm hiểu về [CSS Triggers](https://web.archive.org/web/20220727225220/https://csstriggers.com/) để hiểu các thuộc tính CSS có thể tạo animation ảnh hưởng thế nào đến quá trình render và hiệu năng. So sánh các thuộc tính như `background-color` và `transform` để xem những thay đổi CSS khác nhau tác động thế nào đến pipeline render của trình duyệt.
1. Đọc [hướng dẫn tương tác về transitions](https://www.joshwcomeau.com/animation/css-transitions/) này.
1. Tìm hiểu cách [bắt và gỡ lỗi các vấn đề repaint](https://dzhavat.github.io/2021/02/18/debugging-layout-repaint-issues-triggered-by-css-transition.html).

</div>