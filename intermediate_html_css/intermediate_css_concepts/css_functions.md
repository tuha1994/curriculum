### Giới thiệu

Có thể bạn đã nhận thấy một số giá trị thuộc tính từng làm việc trong CSS có cú pháp hơi khác. Khi giá trị là một từ đi kèm một cặp dấu ngoặc đơn () chứa thông tin bên trong — như trong hàm `rgb(0, 0, 0)` của thuộc tính background — bạn đang sử dụng các hàm CSS.

Trong bài này, chúng ta sẽ tìm hiểu những điều cơ bản về hàm là gì và một số cách thường dùng hàm trong CSS.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Nhận biết các thành phần cơ bản của một hàm CSS.
- Tìm hiểu các hàm `calc()`, `min()`, `max()` và `clamp()` cũng như cách sử dụng từng hàm.

### Hàm là gì và được dùng như thế nào trong CSS?

Tương tự các ngôn ngữ lập trình, hàm trong CSS là những đoạn mã có thể tái sử dụng để thực hiện các tác vụ cụ thể. Hàm nhận các “đối số” giữa hai dấu ngoặc đơn; mỗi đối số được hàm sử dụng theo một cách cụ thể. Một số ví dụ phổ biến là:

```css
color: rgb(0, 42, 255);
background: linear-gradient(90deg, blue, red);
```

Ở đây, giá trị của `color` là hàm `rgb()`, nhận các đối số dưới dạng số. Hàm xử lý những số đó để tính màu rgb tương ứng với ba giá trị đã cho. Tương tự, thuộc tính `background` có giá trị là `linear-gradient(90deg, blue, red)`. `linear-gradient` tạo ra ảnh chuyển màu bằng các tham số được cung cấp. Hàm cần ít nhất hai đối số màu: những màu sẽ chuyển tiếp với nhau. Ngoài ra, bạn có thể đặt góc của đường chuyển màu (như trong ví dụ), thêm nhiều giá trị màu hơn, v.v.

Khác với các ngôn ngữ lập trình bạn sẽ dùng trong TOP, CSS không cho phép chúng ta tự tạo hàm. Thay vào đó, ngôn ngữ này đi kèm một danh sách các hàm dựng sẵn giúp bạn giải quyết những vấn đề tạo kiểu phổ biến nhất.

Ngoài việc định nghĩa màu, có một số hàm CSS hữu ích khi thiết kế bố cục và kích thước website. Chúng trở nên quan trọng khi suy nghĩ về responsive design.

Hãy cùng xem qua một vài hàm: `calc()`, `min()`, `max()` và `clamp()`.

### calc()

Những trường hợp sử dụng mạnh mẽ nhất của calc gồm:

- Trộn các đơn vị
- Khả năng lồng `calc( calc () - calc () )`

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="OJxNxya" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/OJxNxya">
  calc() | CSS Functions</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy xem cách `calc()` được sử dụng ở đây:

```css
:root {
--header: 3rem;
--footer: 40px;
--main: calc(100vh - calc(var(--header) + var(--footer)));
}
```

- `--header`, `--footer` và `--main` đều là ví dụ về biến CSS. Bạn sẽ học về chúng trong bài tiếp theo.
  
Đặt main bằng kết quả của: `100vh - (3rem + 40px)`.
Nói cách khác:  `main = 100vh - (header + footer)`.
`calc()` thực hiện phép tính giúp chúng ta, dù đang trộn các đơn vị vh, rem và px.
Kết hợp với biến CSS, `calc()` có thể giúp chúng ta tránh phải lặp lại các quy tắc CSS.

Bạn có thể hiểu cách `calc()` được sử dụng trong phần nhúng CodePen ở trên. Chúng tôi khuyến khích bạn thử thay đổi các đơn vị và kích thước của các phần tử để xem kết quả trước khi chuyển tiếp.

<div class="lesson-note lesson-note--tip" markdown=1>

#### Ví dụ calc()

Phần trên chỉ là ví dụ về cách `calc()` có thể tác động đến bố cục, nhưng hãy nhớ rằng `calc()` có lẽ không phải cách tốt nhất để thực hiện việc này. Chúng ta sẽ nói thêm về bố cục trong các bài học sau.

</div>

### min()

`min()` làm rất tốt việc giúp chúng ta tạo các website responsive. Hãy xem ví dụ này:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="RwLaLay" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/RwLaLay">
  min() | CSS Functions</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

```css
#iconHolder {
  width: min(150px, 100%);
  height: min(150px, 100%);
  box-sizing: border-box;
  border: 6px solid blue;
}
```

Hàm `min()` hoạt động giống hệt `Math.min()` của JavaScript và method `Array#min` của Ruby. Hàm nhận một danh sách các giá trị được phân tách bằng dấu phẩy và trả về giá trị nhỏ nhất.

Hàm kiểm tra xem `100%` chiều rộng của phần tử cha có nhỏ hơn `150px` hay không. Nếu `100%` hẹp hơn `150px`, phần tử sẽ chiếm toàn bộ chiều rộng của container (`100%`). Nếu không, nó sẽ rộng `150px`.

Bạn có thể thực hiện phép tính cơ bản bên trong `min()`. Ví dụ: `width: min(80ch, 100vw - 2rem);` (trong trường hợp này bạn thậm chí không cần `calc()`).

### max()

`max()` hoạt động giống `min()`, chỉ theo chiều ngược lại, tương tự các method `Math.max()` của JavaScript và `Array#max` của Ruby. Hàm chọn giá trị lớn nhất có thể trong cặp dấu ngoặc đơn.

```css
width: max(100px, 4em, 50%);
```

Đoạn trên so sánh cả ba giá trị và đặt chiều rộng của phần tử thành giá trị lớn nhất. Nếu `50%` của container cha lớn hơn `100px` và `4em`, chiều rộng sẽ là `50%`. Nếu `4em` lớn hơn các giá trị còn lại, `4em` sẽ được dùng.

Hàm max hữu ích nhất khi cửa sổ xem cực kỳ nhỏ hoặc khi người dùng tăng kích thước nội dung bằng tính năng zoom của trình duyệt.
Ban đầu bạn có thể không dùng max nhiều, nhưng đây là một công cụ đáng biết trong các dự án coi trọng khả năng tiếp cận.

### clamp()

`clamp()` là một cách tuyệt vời để làm cho các phần tử linh hoạt và responsive.
`clamp()` nhận 3 giá trị:

```css
h1 {
  font-size: clamp(1.5rem, 5vw, 3rem);
}
```

1. giá trị tối thiểu `1.5rem`
1. giá trị điều chỉnh theo tỷ lệ `5vw`
1. giá trị tối đa `3rem`

Hàm CSS `clamp()` dùng các giá trị này để đặt giá trị tối thiểu, giá trị điều chỉnh theo tỷ lệ và giá trị tối đa. Trong ví dụ trên, điều đó có nghĩa là cỡ chữ nhỏ nhất được chấp nhận là `1.5rem` và lớn nhất là `3rem`.

Cỡ chữ `5vw` được đặt ở khoảng giữa. Giá trị `5vw` cho phép cỡ chữ thay đổi theo chiều rộng viewport, nhưng kích thước bị giới hạn bởi các giá trị tối thiểu và tối đa đã đặt.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem [danh sách đầy đủ các hàm CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions) và cách sử dụng chúng để hình dung những gì có thể làm được.
1. Tìm hiểu sâu hơn về [các hàm CSS `min`, `max` và `clamp`](https://web.dev/min-max-clamp/) qua các ví dụ thực tế.

</div>