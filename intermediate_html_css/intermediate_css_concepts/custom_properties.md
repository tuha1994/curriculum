### Giới thiệu

Custom properties (còn gọi là biến CSS) có thể là một công cụ rất hữu ích và mạnh mẽ khi viết các file CSS. Nói ngắn gọn, chúng cho phép chúng ta tham chiếu đến một giá trị CSS bao nhiêu lần tùy ý trong một file. Nhờ custom properties, thay vì phải cập nhật từng nơi xuất hiện của một giá trị cụ thể ("Màu đỏ này quá nhạt, hãy đổi màu ở cả bảy selector này"), chúng ta chỉ cần cập nhật một nơi duy nhất: chính custom property đó. Không chỉ vậy, custom properties còn giúp giữ màu sắc nhất quán trong toàn dự án, điều sẽ rất hữu ích khi dự án lớn dần.

Chúng ta thậm chí có thể định nghĩa lại custom properties trong những ngữ cảnh khác nhau, cực kỳ hữu ích khi tạo theme, chẳng hạn theme tối và sáng mà bạn thấy trên nhiều website hiện nay.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Cách khai báo custom properties.
- Cách sử dụng custom property trong một khai báo rule.
- Phạm vi của custom properties.
- Selector gốc.
- Sử dụng media query.

### Sử dụng custom properties

Cú pháp khai báo và truy cập custom property không khác quá nhiều so với cách chúng ta viết các khai báo rule thông thường:

```css
.error-modal {
  --color-error-text: red;
  --modal-border: 1px solid black;
  --modal-font-size: calc(2rem + 5vw);

  color: var(--color-error-text);
  border: var(--modal-border);
  font-size: var(--modal-font-size);
}
```

Vậy là xong! Trước tiên, chúng ta khai báo custom property bằng hai dấu gạch ngang, theo sau là tên property phân biệt chữ hoa chữ thường và các từ được ngăn cách bằng dấu gạch ngang (`color-error-text` không giống `Color-Error-Text`), tên này có thể tùy ý. Việc dùng Kebab case (dấu gạch ngang đơn để ngăn cách các từ) rất quan trọng vì khoảng trắng không hợp lệ (`--color error text` sẽ không hoạt động). Sau đó, chúng ta có thể lưu bất kỳ giá trị CSS hợp lệ nào vào custom property vừa khai báo, dù đó là giá trị màu, giá trị viết tắt hay thậm chí một hàm phức tạp hơn.

Khi muốn truy cập custom property, chúng ta dùng hàm `var()` làm giá trị của một thuộc tính CSS, rồi đặt custom property bên trong cặp dấu ngoặc đơn (bao gồm cả hai dấu gạch ngang ở đầu).

#### Giá trị dự phòng

Hàm `var()` thực ra nhận hai tham số. Tham số đầu tiên là custom property chúng ta muốn gán, như đã trình bày. Tham số thứ hai là giá trị dự phòng tùy chọn. Khi cung cấp giá trị dự phòng cùng với custom property, giá trị dự phòng sẽ được dùng nếu custom property không hợp lệ hoặc chưa được khai báo. Chúng ta thậm chí có thể truyền một custom property *khác* làm giá trị dự phòng, và custom property đó cũng có thể có giá trị dự phòng *riêng*!

```css
.fallback {
  --color-text: white;

  background-color: var(--undeclared-property, black);
  color: var(--undeclared-again, var(--color-text, yellow));
}
```

Trong ví dụ trên, thuộc tính `background-color` sẽ có giá trị `black` và thuộc tính `color` sẽ có giá trị `white`. Nếu custom property `--color-text` không hợp lệ hoặc không tồn tại, giá trị dự phòng cho giá trị dự phòng sẽ được dùng, và thuộc tính `color` sẽ có giá trị `yellow`.

### Phạm vi

Trong ví dụ đầu tiên, có thể bạn đã nhận thấy chúng ta khai báo rồi truy cập custom properties trong cùng một declaration block. Đó là vì phạm vi của custom property được xác định bởi selector. Phạm vi này bao gồm selector nơi custom property được khai báo cùng mọi hậu duệ của selector đó. Nếu bạn quen với cách hoạt động của scope trong JavaScript, hành vi này sẽ khá tương tự.

Trong ví dụ dưới đây, chỉ phần tử có class `cool-paragraph` được tạo nền màu đỏ, vì nó là hậu duệ của phần tử nơi custom property được khai báo.

```html
<div class="cool-div">
  <p class="cool-paragraph">Check out my cool, red background!</p>
</div>

<p class="boring-paragraph">I'm not in scope so I'm not cool.</p>
```

```css
.cool-div {
  --main-bg: red;
}

.cool-paragraph {
  background-color: var(--main-bg);
}

.boring-paragraph {
  background-color: var(--main-bg);
}
```

#### Selector :root

Dù đôi khi bạn muốn giới hạn phạm vi của custom property, có thể bạn cũng muốn dùng các custom property khác trên nhiều selector không liên quan. Một cách giải quyết là khai báo cùng một custom property trên nhiều selector, nhưng như vậy lại đi ngược một trong những mục đích của custom properties (dễ thay đổi nhiều nơi chứa một giá trị cùng lúc).

Cách tốt hơn là khai báo các custom property trên selector `:root`, về cơ bản tương đương selector `html` nhưng có specificity cao hơn.

```html
<p class="cool-paragraph">Lorem ipsum dolor sit amet.</p>

<p class="exciting-paragraph">Lorem ipsum dolor sit amet.</p>
```

```css
:root {
  --main-color: red;
}

.cool-paragraph {
  color: var(--main-color);
}

.exciting-paragraph {
  background-color: var(--main-color);
}
```

Khi khai báo custom property trên selector `:root` trong ví dụ trên, chúng ta có thể truy cập nó trên *bất kỳ* selector hợp lệ nào khác trong file CSS, vì mọi selector khác đều được xem là hậu duệ của selector `:root`.

### Tạo theme bằng custom properties

Ngoài việc cho phép truy cập custom properties một cách toàn cục hơn, selector `:root` còn cung cấp một cách để thêm theme vào các trang của chúng ta:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="PojVRMb" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PojVRMb">
Theme Toggle | CSS Custom Properties</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Trước tiên, vì không có quyền truy cập trực tiếp vào phần tử gốc trong tab HTML trên codepen, chúng ta đã áp dụng class mặc định `dark` bằng menu cài đặt. Bạn cứ mở pen và thay đổi cài đặt này để xem nó hoạt động thế nào! Tiếp theo, trong CSS, chúng ta tạo hai scope cho custom properties trên selector `:root`: một scope dành cho trường hợp phần tử `html` (hoặc root) có class `dark`, và một scope dành cho trường hợp nó có class `light`. Sau đó, các selector khác sử dụng giá trị của những custom properties tùy theo class nào hiện có trên phần tử root.

#### Media query

Cho người dùng khả năng tự chuyển theme là điều tuyệt vời, nhưng còn một cách khác để đặt theme mà bạn có thể đã gặp trên một số website hoặc ứng dụng: dùng cài đặt theme của người dùng từ hệ điều hành hoặc user agent (chẳng hạn trình duyệt). Việc này có thể thực hiện bằng media query `prefers-color-scheme`, cho phép bạn áp dụng các style khác nhau dựa trên thiết bị hoặc cài đặt của người dùng, như kích thước màn hình hoặc tùy chọn theme (chế độ sáng/tối).

Query `prefers-color-scheme` kiểm tra xem người dùng đã chọn theme trên hệ điều hành hoặc trình duyệt hay chưa. Đừng lo, chúng ta sẽ tìm hiểu media query chi tiết hơn sau. Hiện tại, hãy thử thay đổi theme của hệ điều hành hoặc trình duyệt để xem ví dụ cập nhật theo thời gian thực!

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="powGZzE" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/powGZzE">
Theme Query | CSS Custom Properties</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Trước tiên, chúng ta thêm custom properties trên phần tử `:root`, bên ngoài media query. Việc này tạo theme mặc định trong trường hợp người dùng chưa đặt tùy chọn trên hệ điều hành hoặc user agent, hoặc trình duyệt không hỗ trợ media query. Trong trường hợp này, chúng ta dùng màu của theme "light" làm mặc định. Sau đó, chúng ta thêm media query `prefers-color-scheme` cho trường hợp người dùng đặt theme tối trong tùy chọn của họ.

Sử dụng media query `prefers-color-scheme` khá hữu ích cho người dùng vì họ không cần tự chuyển sang theme mong muốn. Tuy vậy, bạn cần lưu ý một số điều khi dùng media query này:

1. Chỉ `dark` và `light` là các giá trị hợp lệ cho media query, nên bạn không thể dùng nó để triển khai theme nào ngoài hai theme cơ bản này.
1. Giá trị `light` của media query thực sự dùng cho trường hợp người dùng chỉ định theme sáng *hoặc* chưa đặt tùy chọn nào.
1. Nó không cho phép người dùng tự thay đổi theme, điều vẫn có thể quan trọng khi người dùng muốn dùng theme đối lập với theme ưa thích của hệ điều hành/user agent vì bất kỳ lý do nào.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [Video về custom properties trong CSS](https://www.youtube.com/watch?v=PHO6TBq_auI) là phần giới thiệu tuyệt vời. Hãy xem video này.
1. Đọc trang MDN [Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties#inheritance_of_custom_properties), bắt đầu từ phần "Inheritance of custom properties".
1. Xem [Using CSS custom properties](https://www.youtube.com/watch?v=_2LwjfYc1x8) của Kevin Powell để tìm hiểu những cách hay dùng custom properties.
1. Mở inspector trên trang này để kiểm tra các style và xem Odin sử dụng một số custom properties như thế nào.

</div>