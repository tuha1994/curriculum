### Giới thiệu

Cho đến nay, bạn đã học cách tạo grid, điều chỉnh kích thước các track và định vị grid element trong những hàng, cột cụ thể. Hiện tại, bạn đã có mọi công cụ cần thiết để tạo bất kỳ bố cục grid tĩnh nào, nhưng sẽ làm gì khi muốn grid responsive, động hoặc có thể tái sử dụng hơn?

Trong bài học này, bạn sẽ tìm hiểu một số thuộc tính Grid nâng cao giúp bạn làm được điều đó.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Dễ dàng tạo nhiều grid track bằng hàm `repeat`.
- Tạo grid track bằng đơn vị `fr` thay vì kích thước tường minh.
- Thiết lập giới hạn kích thước tối thiểu, tối đa và lý tưởng cho track.
- Dùng `auto-fit` và `auto-fill` để tạo grid có số hàng hoặc cột động.
- Kết hợp `auto-fit`/`auto-fill` với `minmax()` để tạo grid responsive.

### Thiết lập

Chúng ta sẽ tiếp cận bài học này theo hướng thực hành. Hãy thiết lập một grid với năm cột và hai hàng, đồng thời áp dụng một chút style để mọi thứ dễ nhìn.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="wvrBBXK" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/wvrBBXK">
  Advanced Properties | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Trong ví dụ này, HTML khá đơn giản, nhưng hãy nói nhanh về một số CSS đang được dùng mà *không* liên quan đến bài học, để bạn hiểu lý do chúng xuất hiện.

#### Các selector grid-item, p, img

Tất cả thuộc tính ở đây nhằm làm cho grid item trông đẹp hơn một chút. Chúng tương đối dễ hiểu, nên không cần đi sâu vào chi tiết.

#### grid-container

Đối với container, rõ ràng chúng ta dùng `display: grid` để hiển thị container dưới dạng CSS Grid. Nhưng thuộc tính tiếp theo có thể còn xa lạ: `resize: both`. Thuộc tính này cho phép người dùng thay đổi kích thước container bằng cách nhấp và kéo từ góc dưới bên phải. Nó sẽ hữu ích khi chúng ta bắt đầu dùng các thuộc tính thay đổi kích thước grid track dựa trên kích thước grid.

**Thay đổi mức zoom:** Hãy xem các CodePen embed ở mức 0.5x hoặc 0.25x để có chỗ thay đổi kích thước container.

Chúng ta dùng `overflow: auto` để bật cuộn nếu thay đổi kích thước container nhỏ hơn mức grid có thể chứa.

Chúng ta dùng các thuộc tính `gap` và `padding` để tạo vùng “gutter”, giúp nhìn rõ grid item hơn.

`border` và `background-color` được thêm vào để container trông đẹp hơn.

```css
grid-template-rows: 150px 150px;
grid-template-columns: 150px 150px 150px 150px 150px;
```

Đây là nơi chúng ta bắt đầu tập trung vào bài học. Để tạo hai hàng và năm cột, chúng ta định nghĩa thủ công kích thước track của từng hàng và cột.

Ôi, thật tẻ nhạt!

Được rồi, có lẽ nó không tẻ nhạt đến thế. Nhưng trong trường hợp này, chúng ta chỉ tạo grid 2x5, có thể chứa tổng cộng mười item. Hãy tưởng tượng phải dùng cách này để định nghĩa từng cột và hàng cho một grid có thể chứa hàng trăm item!

Hãy dùng `repeat()`.

### Repeat

`repeat()` là một hàm CSS có sẵn cho các thuộc tính template của CSS Grid, cho phép định nghĩa số lượng hàng hoặc cột và kích thước của chúng mà không phải gõ thủ công kích thước từng track. Ví dụ, với thiết lập ở trên:

```css
.grid-container {
  grid-template-rows: 150px 150px;
  grid-template-columns: 150px 150px 150px 150px 150px;
}
```

có thể viết lại thành:

```css
.grid-container {
  grid-template-rows: repeat(2, 150px);
  grid-template-columns: repeat(5, 150px);
}
```

Hãy tự thử nhé!

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="wvrBBxK" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/wvrBBxK">
  repeat | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Đơn vị phân số

Bây giờ đã biết cách nhanh chóng tạo nhiều grid track, đã đến lúc học cách làm chúng *động*. Trong ngữ cảnh này, động nghĩa là “linh hoạt” hoặc “responsive theo một cách nào đó”. Trái nghĩa của động là *tĩnh*, tức cố định ở một chiều cao nhất định như `150px`, giá trị chúng ta đã dùng khi thiết lập grid mẫu.

Cách cơ bản nhất để làm grid item động là dùng **đơn vị phân số**, còn gọi là `fr`.

Đơn vị `fr` là cách phân phối phần *không gian còn lại* trong grid. Ví dụ, nếu có grid bốn cột với tổng chiều rộng `400px` và bốn grid item, mỗi item nằm trên một column track có kích thước `1fr`, thì mỗi grid item sẽ nhận **một phần** của `400px`, tức 100 pixel.

Hãy xem điều gì xảy ra khi cho các row track và column track trong grid mẫu một chiều rộng động `1fr` thay vì chiều rộng tĩnh 150px:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="WNZbbgG" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/WNZbbgG">
  fr Unit 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn có thấy tất cả grid item giờ lấp đầy toàn bộ chiều rộng và chiều cao của grid không? Hay nhỉ? Bây giờ hãy thử thay đổi kích thước ví dụ đó để xem điều gì xảy ra. Còn thú vị hơn nữa!

Chúng ta cũng có thể yêu cầu grid item phân phối không gian còn lại theo các tỷ lệ khác nhau. Ví dụ, nếu chia 5 cột bằng cách đặt kích thước track của hai cột đầu là `2fr` và ba cột còn lại là `1fr`, hai track đầu sẽ nhận lượng không gian còn lại gấp đôi các track khác. Hãy so sánh ví dụ này với ví dụ trước:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="QWqwwJG" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/QWqwwJG">
  fr Unit 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Trong ví dụ này, `grid-template-columns` có khá nhiều thứ đang diễn ra, nhưng hãy dành thời gian hiểu dòng lệnh:

```css
grid-template-columns: repeat(2, 2fr) repeat(3, 1fr);
```

<div class="lesson-note" markdown="1">

#### Các cách khác nhau để định nghĩa grid track

Ở đây chúng ta tiếp tục dùng hàm `repeat()`, nhưng cũng *có thể* viết theo cách cũ!

</div>

Điểm mấu chốt là hai cột đầu được gán đơn vị `2fr`, còn ba cột còn lại được gán `1fr`. Điều này có nghĩa là khi grid động mở rộng và thu nhỏ, không gian sẽ được phân phối khác nhau giữa các cột, cụ thể là hai cột đầu nhận số pixel gấp đôi ba cột còn lại.

Bạn sẽ nhận thấy khi thay đổi kích thước, grid item tăng theo tỷ lệ số đơn vị `fr` được cấp, như đã đề cập ở trên.

Bạn cũng có thể trộn các đơn vị tĩnh (như `px`) và đơn vị động (như `fr`):

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="RwLNNqX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/RwLNNqX">
  fr and px Units | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Có thể bạn đã nhận thấy khi phóng to grid hết mức, không có giới hạn nào cho việc grid item lớn đến đâu. Tuy nhiên, khi thu nhỏ hết mức, có một kích thước “nhỏ nhất” rõ ràng mà grid cho phép item đạt tới. Trong trường hợp này, đó là kích thước nhỏ nhất mà phần tử `<p>` hoặc `<img>` có thể có trước khi tràn. Điểm ngắt này là giá trị `min-content` của item. Đây là keyword CSS **rất** hữu ích, nhưng nằm ngoài phạm vi bài học. Để biết thêm, hãy xem [tài liệu MDN về min-content](https://developer.mozilla.org/en-US/docs/Web/CSS/min-content).

### Kích thước track tối thiểu và tối đa: min() và max()

Khi thu nhỏ grid đến mức rất nhỏ, thật yên tâm khi biết trình duyệt sẽ dừng item trước khi nó co vượt quá giá trị `min-content`. Tuy nhiên, phần lớn thời gian chúng ta không muốn phụ thuộc vào điều đó. Tốt hơn là developer nên chủ động quyết định nội dung có thể nhỏ và lớn đến đâu, kể cả trong những tình huống cực đoan nhất.

Chúng ta đã học về `min()` và `max()` trong [bài học về CSS function](https://www.theodinproject.com/lessons/intermediate-html-and-css-css-functions) trước đó, nhưng ôn lại cũng không hại gì. Cả hai hàm đều trả về một giá trị dựa trên các đối số bạn cung cấp. `min()` trả về giá trị nhỏ nhất trong tất cả giá trị được truyền vào, còn `max()` trả về giá trị lớn nhất. Ví dụ, `min(100px, 200px)` luôn trả về `100px`, còn `max(100px, 200px)` luôn trả về `200px`.

Bạn có thể cung cấp bao nhiêu đối số tùy ý cho các hàm này:

```css
.grid-container {
  grid-template-rows: repeat(2, min(100px, 200px, 300px, 400px, 500px));
  grid-template-columns: repeat(5, max(100px, 200px, 300px, 400px, 500px));
}
```

Tất nhiên, việc đưa các đơn vị tĩnh vào những hàm này khá vô nghĩa vì phép tính không có ý nghĩa: giá trị nhỏ nhất hoặc lớn nhất luôn được trả về. Trong ví dụ trên, các hàng grid luôn có kích thước `100px` (nhỏ nhất trong năm giá trị), còn các cột grid luôn có kích thước `500px` (lớn nhất trong năm). Nhưng khi cung cấp một giá trị động làm một trong các đối số, chúng ta khai mở tiềm năng thực sự của các hàm này, đặc biệt trong bối cảnh Grid:

```css
.grid-container {
  grid-template-rows: repeat(2, min(200px, 50%));
  grid-template-columns: repeat(5, max(120px, 15%));
}
```

Trong trường hợp này, kích thước row grid được tính từ `200px` và `50%` chiều cao của grid container. Theo thời gian thực, trình duyệt so sánh hai giá trị và áp dụng giá trị nhỏ hơn cho kích thước row grid. Về cơ bản, điều này nói rằng track phải có kích thước bằng 50% tổng không gian dọc của grid (vì chúng ta đang định nghĩa kích thước hàng), *trừ khi* con số đó vượt quá `200px`. Nói cách khác, bạn đang đặt max-height cho track.

Ngược lại, kích thước column grid được tính dựa trên giá trị lớn hơn giữa `120px` và `15%` chiều rộng của grid container. Như vậy, về cơ bản chúng ta đặt chiều rộng *tối thiểu* cho column grid là `120px`. Hãy xem ví dụ và thử nhấp kéo để thay đổi kích thước grid, xem các grid item phản hồi ra sao:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="PoJwwLq" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PoJwwLq">
  min and max | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Kích thước tối thiểu và tối đa động

#### minmax()

`minmax()` là một hàm CSS được dùng riêng với Grid. Nó chỉ có thể dùng với các thuộc tính CSS sau:

- `grid-template-columns`
- `grid-template-rows`
- `grid-auto-columns`
- `grid-auto-rows`

Đây là một hàm tương đối đơn giản, chỉ nhận hai đối số:

1. Kích thước tối thiểu mà grid track có thể có
1. Kích thước tối đa mà grid track có thể có

Không giống `min()` và `max()`, việc dùng giá trị tĩnh cho cả hai đối số ở đây có thể *hợp lý*. Đây là ví dụ về grid chúng ta đang dùng, viết bằng `minmax()` và một số giá trị tĩnh:

```css
.grid-container {
  grid-template-rows: repeat(2, 1fr);
  grid-template-columns: repeat(5, minmax(150px, 200px));
}
```

Với `grid-template-columns` được thiết lập bằng các giá trị `minmax()`, chiều rộng của mỗi grid item sẽ tăng và giảm cùng grid container khi nó thay đổi kích thước theo chiều ngang. Tuy nhiên, khi grid thu nhỏ, các column track sẽ dừng co ở `150px`, còn khi grid lớn lên, chúng sẽ dừng tăng ở `200px`. Thật linh hoạt! Hãy tự xem bên dưới:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="poWvvmr" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/poWvvmr">
  minmax | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### clamp()

Không giống `minmax()`, `clamp()` là một hàm CSS có thể dùng ở bất kỳ đâu, không chỉ trong grid container. Cũng như `min()` và `max()`, chúng ta đã học về nó ở bài trước, nhưng hãy ôn nhanh. Cú pháp như sau:

`clamp(minimum-size, ideal-size, maximum-size)`

Hàm này cho phép item tự thay đổi kích thước cho đến khi chạm một trong các ngưỡng tối thiểu hoặc tối đa.

Vì mục đích của `clamp()` là tạo track có kích thước linh hoạt nhưng có ràng buộc, chúng ta muốn dùng một giá trị động cho đối số “ideal size”, và *thường* dùng kích thước tĩnh cho kích thước tối thiểu và tối đa, dù cũng có thể dùng giá trị động ở đây.

Đây là một ví dụ không dùng grid. Một lát nữa chúng ta sẽ quay lại grid:

```css
.non-grid-example {
  width: clamp(500px, 80%, 1000px);
}
```

Phần tử này, giả sử chỉ là một `div`, sẽ hiển thị với chiều rộng bằng 80% chiều rộng của phần tử cha, trừ khi con số đó nhỏ hơn `500px` hoặc lớn hơn `1000px`; khi đó nó sẽ dùng các giá trị đó làm chiều rộng.

Được rồi, giờ quay lại grid:

```css
.grid-container {
  grid-template-columns: repeat(5, clamp(150px, 20%, 200px));
}
```

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="dyVPPEL" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/dyVPPEL">
  clamp | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn có thấy các track giữ ở `20%` chiều rộng của container cho đến khi chạm ngưỡng tối thiểu hoặc tối đa không?

Dùng `clamp()` và `minmax()` là những cách tuyệt vời để làm grid responsive hơn, đồng thời bảo đảm chúng ta không chạm các breakpoint quan trọng khiến website trông xấu. Điều này đặc biệt cần thiết khi dùng hình ảnh và các phần tử có xu hướng tràn hoặc hiển thị không mong muốn khi bị đẩy tới kích thước cực đoan.

### auto-fit và auto-fill

Hai giá trị này thực ra là một phần của đặc tả hàm `repeat()`, nhưng được để đến cuối bài vì sự hữu ích của chúng chưa rõ cho đến khi bạn hiểu hàm `minmax()`. Trường hợp sử dụng là: bạn muốn số cột của grid linh hoạt dựa trên kích thước grid. Ví dụ, nếu grid chỉ rộng `200px`, có thể bạn chỉ muốn một cột. Nếu rộng `400px`, bạn có thể muốn hai cột, v.v. May mắn là `auto-fit` và `auto-fill` sẽ giải quyết việc này!

Theo [đặc tả W3 về auto-fill và auto-fit](https://www.w3.org/TR/css-grid-1/#auto-repeat), cả hai hàm sẽ trả về “số nguyên dương lớn nhất” mà không làm grid item tràn khỏi container. Đây là một ví dụ:

```css
.example {
  display: grid;
  width: 1000px;
  grid-template-columns: repeat(auto-fit, 200px);
}
```

Grid này có chiều rộng cố định `1000px`, và chúng ta yêu cầu nó lấp đầy các cột bằng các track `200px`. Miễn là có ít nhất năm grid item, kết quả luôn là bố cục 5 cột. Trong trường hợp này, `auto-fill` thực ra cũng cho kết quả tương tự. Chúng ta sẽ nói về sự khác nhau sau.

Phép màu thực sự của `auto-fit` và `auto-fill` xuất hiện khi kết hợp `minmax()` vào công thức. Với `minmax()`, chúng ta có thể nói với grid rằng muốn có nhiều cột nhất có thể, dùng các ràng buộc của hàm `minmax()` để xác định kích thước từng cột mà không làm grid tràn. Hãy xem grid trông tuyệt thế nào khi thay đổi kích thước!

```css
.grid-container {
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
}
```

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="abLzzgR" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/abLzzgR">
  autofit 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn có thấy khi thay đổi kích thước, các cột tự biết có thể vừa bao nhiêu cột theo chiều ngang không? Nếu bạn không thấy *điều đó* thú vị, hãy kiểm tra mạch đập của mình!

Vậy cụ thể điều gì xảy ra với `repeat(auto-fit, minmax(150px, 1fr));`? Hãy nhớ rằng `auto-fit` trả về **số nguyên dương cao nhất** mà không làm grid tràn. Trước tiên, trình duyệt phải biết chiều rộng content box của grid (tức bỏ qua margin, border và padding).

Với ví dụ này, giả sử content box của grid hiện rộng `500px`. Thứ hai, trình duyệt cần biết có thể đặt bao nhiêu column track trong chiều rộng đó. Để làm vậy, nó dùng giá trị tối thiểu trong hàm `minmax()`, vì giá trị đó cho số item lớn nhất, ở đây là `150px`. Nếu cửa sổ rộng `500px`, grid sẽ hiển thị 3 cột. Nhưng chưa hết! Sau khi xác định số cột có thể vừa, trình duyệt sẽ phóng to các cột đến giá trị tối đa mà hàm `minmax()` cho phép. Trong trường hợp này, kích thước tối đa là `1fr`, nên cả ba cột nhận phần không gian khả dụng bằng nhau. Khi thay đổi kích thước cửa sổ, các phép tính này diễn ra theo thời gian thực và kết quả là thứ bạn thấy trong ví dụ trên!

#### Còn auto-fill thì sao?

Trong hầu hết trường hợp, `auto-fill` hoạt động giống hệt `auto-fit`. Sự khác biệt chỉ thấy rõ khi số item ít hơn số item có thể lấp đầy toàn bộ một hàng grid. Khi grid được mở rộng đến kích thước có thể chứa thêm một grid item nhưng không còn item nào, `auto-fit` sẽ giữ các grid item ở kích thước `max`. Khi dùng `auto-fill`, các grid item sẽ thu nhỏ trở lại kích thước `min` ngay khi có đủ chỗ để thêm một grid item, dù không có item nào để hiển thị. Chúng tiếp tục lớn dần đến `max` rồi bật về `min` khi grid mở rộng và có thêm không gian cho các grid track mới.

Để thấy điều này, hãy xem 2 ví dụ sau, ví dụ đầu dùng `auto-fit`, ví dụ thứ hai dùng `auto-fill`, rồi thay đổi kích thước grid theo chiều ngang:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="mdByJyJ" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/mdByJyJ">
  autofit 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="KKXwpwX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/KKXwpwX">
  autofill | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Vậy là xong! Xin chúc mừng, nếu đã đi đến đây thì bạn đang trên đường trở thành Grid Master!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc các phần “CSS Grid Properties”, “Special Units, Values, & Functions” và “Subgrid” trong [CSS-Tricks Grid Layout Guide](https://css-tricks.com/css-grid-layout-guide).
1. Chơi các level 18 - 28 của [CSS Grid Garden](https://cssgridgarden.com/) để luyện định vị item.
1. Làm các bài tập trong [thư mục `intermediate-html-css/advanced-grid` của repository bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/intermediate-html-css/advanced-grid) (nhớ rằng hướng dẫn nằm trong README), theo thứ tự:
   - `01-responsive-holy-grail`
   - `02-holy-grail-mockup`

   Lưu ý: Khi làm các bài tập này, hãy sử dụng mọi tài liệu và tài nguyên cần thiết để hoàn thành. Bạn *không* được kỳ vọng phải ghi nhớ tất cả vào lúc này. Hãy xem tài liệu, dùng Google, làm bất cứ điều gì cần thiết (ngoài việc xem lời giải) để hoàn thành.

</div>