### Giới thiệu

Thuộc tính `transform` là một công cụ mạnh mẽ để thay đổi diện mạo của các phần tử mà không ảnh hưởng đến luồng tự nhiên của tài liệu.

Có lẽ bạn đã thấy nó hoạt động trên nhiều website yêu thích của mình! Transforms thường được dùng để tạo các hiệu ứng animation. Chúng tôi tin rằng bạn sẽ thích tự tạo những animation đẹp mắt, nhưng trước tiên chúng ta cần hiểu cách transforms hoạt động.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Cách sử dụng transforms 2D.
- Cách sử dụng transforms 3D.
- Cách xâu chuỗi nhiều transform.
- Lợi ích của việc sử dụng thuộc tính `transform`.

### Những điều cơ bản về transforms

Thuộc tính `transform` nhận một hoặc nhiều [hàm CSS transform](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/transform-function) làm giá trị; các hàm đó lại nhận giá trị riêng, thường là một góc hoặc một con số.

Gần như mọi phần tử đều có thể áp dụng thuộc tính `transform`, ngoại trừ `<col>`, `<colgroup>` và các phần tử inline không được thay thế. "Không được thay thế" chỉ những phần tử có nội dung nằm trong tài liệu HTML (chẳng hạn `<span>`, `<b>` và `<em>`), trái ngược với nội dung của phần tử "được thay thế" nằm bên ngoài tài liệu, còn bản thân phần tử bị "thay thế" bởi nội dung bên ngoài (`<video>`, `<iframe>` và `<img>` là các ví dụ). Bạn không cần ghi nhớ mọi phần tử không được thay thế, mà chỉ cần lưu ý điều này khi thử áp dụng thuộc tính `transform` cho một phần tử nhưng không hiểu tại sao nó không hoạt động.

### Transforms hai chiều

Trong phần này, chúng ta sẽ đi qua các transform 2D với những hàm transform sau: `rotate`, `scale`, `skew` và `translate`.

#### Rotate

Đây là giá trị hàm transform dùng để xoay một phần tử trên mặt phẳng 2D:

```css
.element {
  transform: rotate();
}
```

Bên dưới là một CodePen cho thấy giá trị này ảnh hưởng đến phần tử đích như thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="GRMgKeE" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/GRMgKeE">
2D Rotate | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Scale

Đây là các giá trị hàm transform dùng để thay đổi tỷ lệ của một phần tử trên mặt phẳng 2D:

```css
.element {
  transform: scaleX();
  transform: scaleY();
  transform: scale();
}
```

Bên dưới là một CodePen cho thấy từng giá trị ảnh hưởng đến phần tử đích như thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="XWeJrGL" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/XWeJrGL">
2D Scale | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Skew

Đây là các giá trị hàm transform dùng để làm nghiêng một phần tử trên mặt phẳng 2D:

```css
.element {
  transform: skewX();
  transform: skewY();
  transform: skew();
}
```

Bên dưới là một CodePen cho thấy từng giá trị ảnh hưởng đến phần tử đích như thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="mdBybgm" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/mdBybgm">
2D Skew | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Translate

Đây là các giá trị hàm transform dùng để dịch chuyển một phần tử trên mặt phẳng 2D:

```css
.element {
  transform: translateX();
  transform: translateY();
  transform: translate();
}
```

Bên dưới là một CodePen cho thấy từng giá trị ảnh hưởng đến phần tử đích như thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="PoJwYrO" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PoJwYrO">
2D Translate | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Xâu chuỗi nhiều transform

Bây giờ bạn đã nắm được transforms 2D, chúng ta sẽ học cách xâu chuỗi chúng. Để xâu chuỗi nhiều transform, hãy thêm nhiều hàm transform và đặt một khoảng trắng giữa mỗi hàm. Hãy xem đoạn code dưới đây:

```html
<div class="red-box"></div>
<div class="blue-box"></div>
```

```css
.red-box,
.blue-box {
  position: absolute;
  width: 100px;
  height: 100px;
}

.red-box {
  background: red;
  transform: rotate(45deg) translate(200%);
}

.blue-box {
  background: blue;
  transform: translate(200%) rotate(45deg);
}
```

Có hai hộp nằm ở cùng một vị trí. Chúng ta đã xâu chuỗi các giá trị hàm `rotate` và `translate` cho cả hai hộp, nhưng theo thứ tự khác nhau. Hãy đoán xem điều gì xảy ra với mỗi hộp, sau đó nhấp vào liên kết "Result" trong CodePen bên dưới để xem bạn đoán đúng không.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css" data-slug-hash="XWeJWWr" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/XWeJWWr">
Chaining | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Nếu bạn đoán đúng thì xin chúc mừng! Hộp màu xanh dịch chuyển trên trục X 200%, đi thẳng sang phải, sau đó xoay tại chỗ 45 độ. Hộp màu đỏ xoay 45 độ trước, rồi dịch chuyển 200% trên trục X đã xoay, khiến nó đi xuống dưới bên phải.

Nhìn chung, bạn có thể xâu chuỗi nhiều transform theo bất kỳ thứ tự nào để tạo ra nhiều kết quả khác nhau, nhưng có một ngoại lệ: `perspective`. Điều này đưa chúng ta đến phần tiếp theo, nơi `perspective` được sử dụng.

### Transforms ba chiều

Các hàm transform `rotate`, `scale` và `translate` không chỉ giới hạn ở mặt phẳng 2D. Chúng cũng hoạt động trên mặt phẳng 3D! Tuy nhiên, để cảm nhận hiệu ứng 3D ở một số giá trị hàm này, cần có `perspective`.

Từ đây trở đi, các ví dụ sẽ phức tạp hơn. Bạn cứ thoải mái thử các thuộc tính này, nhưng cẩn thận đừng để chúng khiến bạn đi quá xa khỏi mục tiêu chính.

#### Perspective

Đây là giá trị hàm transform dùng để đặt khoảng cách từ người dùng đến mặt phẳng z = 0:

```css
.element {
  transform: perspective();
}
```

Về cơ bản, khi đặt giá trị `perspective`, chúng ta yêu cầu đối tượng được hiển thị như thể ta đang nhìn nó từ một khoảng cách cụ thể trên trục z.

Không giống các giá trị hàm transform khác, `perspective` phải được khai báo đầu tiên (ở ngoài cùng bên trái) khi có nhiều giá trị hàm transform. Trong các ví dụ sắp tới về `rotate`, `scale` và `translate`, chúng ta sẽ thấy nó ảnh hưởng đến phần tử đích như thế nào.

#### Rotate theo trục cụ thể

Đây là các giá trị hàm transform bổ sung để xoay một phần tử trong không gian 3D:

```css
.element {
  transform: rotateX();
  transform: rotateY();
  transform: rotateZ();
  transform: rotate3d();
}
```

Bên dưới là một CodePen cho thấy ba giá trị đầu tiên ảnh hưởng đến phần tử đích như thế nào.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="PoJwozR" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PoJwozR">
3D Rotate | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Scale theo trục cụ thể

Đây là các giá trị hàm transform bổ sung để thay đổi tỷ lệ của một phần tử trong không gian 3D:

```css
.element {
  transform: scaleZ();
  transform: scale3d();
}
```

Hãy xem khối lập phương 3D của MDN hoạt động với [`scaleZ`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/scaleZ()) và [`scale3d`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/scale3d()).

#### Translate theo trục cụ thể

Đây là các giá trị hàm transform bổ sung để dịch chuyển một phần tử trong không gian 3D:

```css
.element {
  transform: translateZ();
  transform: translate3d();
}
```

`translateZ` không làm được nhiều điều nếu không có `perspective`. Thay vào đó, `perspective` và `translateZ` phối hợp với nhau để tạo ảo giác về khoảng cách ba chiều từ đối tượng được hiển thị, như trong ví dụ dưới đây.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="MWEYWpN" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/MWEYWpN">
TranslateZ | CSS Transform</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Matrix

Dù không hẳn là một hàm transform 3D, matrix được nhắc đến cuối bài vì rất ít khi được sử dụng. Đây là các giá trị hàm transform cho nó.

```css
.element {
  transform: matrix();
  transform: matrix3d();
}
```

Matrix là cách kết hợp tất cả các hàm transform thành một. Nó hiếm khi được sử dụng vì khó đọc, và gần như không bao giờ được viết bằng tay. Trừ khi cần áp dụng một phép biến đổi rất phức tạp, bạn nên dùng các giá trị hàm transform khác.

Bạn chỉ cần biết *rằng* các hàm này tồn tại và nhìn chung chúng hoạt động như thế nào. Tuy nhiên, việc bạn cảm thấy thoải mái khi xây dựng với chúng là không quan trọng.

### Lợi ích của transforms

Để hiểu tại sao thuộc tính `transform` lại hữu ích, bạn cần biết về các CSS trigger. Bạn có thể tìm hiểu về chúng trong phần [The Pixel Pipeline](https://developers.google.com/web/fundamentals/performance/rendering/#the_pixel_pipeline) của Web Fundamentals từ Google.

Lợi ích chính của việc dùng `transform` là nó diễn ra trong giai đoạn **composition**. Điều này khiến nó rẻ hơn khi sử dụng so với nhiều thuộc tính CSS khác. Bạn có thể xem các trigger được thực thi với từng thuộc tính CSS trong [bảng CSS triggers](https://web.archive.org/web/20220727225220/https://csstriggers.com/) này.

Một lợi ích khác của `transform` là nó có thể được tăng tốc bằng phần cứng thông qua [GPU](https://en.wikipedia.org/wiki/Graphics_processing_unit) của thiết bị (bạn không cần hiểu GPU hoạt động thế nào, nhưng biết thuật ngữ này và ý nghĩa của nó là điều hữu ích). Lợi ích này thể hiện rõ hơn khi nói đến transitions và animations, những chủ đề bạn sẽ học trong các bài tiếp theo.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem [minh họa `rotate3d` của MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/rotate3d()), sau đó đọc thêm về thuộc tính này trong [bài viết về `rotate3d` của QHMIT](https://www.qhmit.com/css/functions/css_rotate3d_function.cfm).
1. Tìm hiểu thêm về [perspective trong CSS](https://3dtransforms.desandro.com/perspective).
1. MDN còn có một [minh họa tuyệt vời dùng `translate3d`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translate3d()).
1. Đọc [The World of CSS Transforms](https://www.joshwcomeau.com/css/transforms/) của Josh Comeau.

</div>