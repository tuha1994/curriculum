### Giới thiệu

Bây giờ bạn đã biết CSS Grid Layout là gì, bạn sẽ học cách tự tạo một grid. Bài học này sẽ đề cập đến việc tạo grid container, thêm cột và hàng, khái niệm grid tường minh và ngầm định, cũng như cách tạo khoảng cách giữa các ô trong grid.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Tạo một grid container.
- Xác định các grid track.
- Giải thích sự khác nhau giữa grid ngầm định và grid tường minh.
- Thiết lập khoảng cách giữa các ô trong grid.

### Thiết lập một grid

Bài học này sẽ chỉ cho bạn việc tạo bố cục grid dễ dàng đến mức nào. Trong các bài học tiếp theo, bạn sẽ tìm hiểu thêm về định vị và cách tạo các grid phức tạp, nhưng hiện tại chúng ta sẽ bắt đầu với một ví dụ cơ bản.

#### Grid container

Chúng ta có thể nghĩ về CSS Grid theo terms container và item. Khi biến một phần tử thành grid container, nó sẽ “chứa” toàn bộ grid. Trong CSS, một phần tử được chuyển thành grid container bằng thuộc tính `display: grid` hoặc `display: inline-grid`.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="ZEXYGGx" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/ZEXYGGx">
  My First Grid | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Trong ví dụ này, phần tử cha có `class="container"` trở thành grid container, còn mỗi phần tử con trực tiếp bên dưới nó tự động trở thành grid item. Điểm dễ ở CSS Grid là bạn không phải gán thuộc tính cho từng phần tử con.

Lưu ý rằng ở đây chỉ các phần tử con trực tiếp mới trở thành grid item. Nếu chúng ta có thêm một phần tử là con của *một trong những* phần tử con này, nó sẽ không phải là grid item. Trong ví dụ dưới đây, phần tử paragraph không phải grid item:

```html
<!-- index.html -->

<div class="container">
  <div>Item 1</div>
  <div>Item 2
    <p>I am not a grid item!</p>
  </div>
  <div>Item 3</div>
  <div>Item 4</div>
</div>
```

Nhưng cũng như bạn đã học trong các bài về flexbox, grid item *cũng có thể* là grid container. Vì vậy, nếu muốn, bạn có thể tạo grid bên trong grid.

#### Các line và track trong grid, ôi trời!

Vì bạn đang làm theo ví dụ của chúng ta (đúng không?) nên bạn sẽ nhận thấy nó chưa trông giống grid lắm. Nhiều tài liệu về CSS Grid thích hiển thị các hộp và bảng grid có đường viền ngay từ đầu. Nhưng nếu grid container và grid item của bạn không có border thì bạn sẽ không thực sự nhìn thấy các line này trên trang. Vì vậy đừng lo, chúng vẫn ở đó!

Nếu kiểm tra các phần tử này trên một trang web bằng developer tools, bạn sẽ thấy huy hiệu grid trên các phần tử grid trong code. Các tùy chọn Layout của developer tools cho phép bạn chọn một lớp phủ có thể hiển thị các line, track và area vô hình của grid. Bạn sẽ đọc về cách dùng developer tools của trình duyệt trong bài tập bên dưới, đồng thời tìm hiểu thêm về line, track và area ở bài tiếp theo.

#### Cột và hàng

Bây giờ grid container cùng một số grid item đã sẵn sàng, đã đến lúc xác định các cột và hàng. Việc này sẽ định nghĩa grid track (khoảng không gian giữa các line trên grid). Chúng ta có thể thiết lập một column track để tạo khoảng không giữa các cột và một row track để tạo khoảng không giữa các hàng. Bài tiếp theo sẽ đi sâu vào track và line, còn hiện tại hãy chỉ tạo một số cột và hàng.

Các thuộc tính `grid-template-columns` và `grid-template-rows` giúp việc định nghĩa column track và row track trở nên dễ dàng. Trong bài này, chúng ta sẽ dùng pixel để định nghĩa cột và hàng. Ở các bài học sau, bạn sẽ tìm hiểu thêm cách định nghĩa bằng phần trăm và đơn vị phân số.

Quay lại grid container ở trên, hãy định nghĩa hai cột và hai hàng để đặt bốn grid item của chúng ta:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="yLzyNYp" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/yLzyNYp">
  Columns and Rows 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Nếu muốn thêm cột hoặc hàng vào grid, chúng ta có thể định nghĩa các giá trị này để tạo thêm một track. Giả sử muốn thêm cột thứ ba vào ví dụ:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="NWaPqxj" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/NWaPqxj">
  Columns and Rows 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

CSS Grid cũng có một thuộc tính viết tắt để định nghĩa hàng và cột. Trong ví dụ trước, chúng ta có thể thay các thuộc tính `grid-template-rows` và `grid-template-columns` bằng thuộc tính viết tắt `grid-template`. Ở đây, chúng ta có thể định nghĩa hàng và cột cùng lúc. Với thuộc tính này, hàng được định nghĩa trước dấu gạch chéo còn cột được định nghĩa sau dấu gạch chéo. Hãy giữ nguyên các giá trị cột và hàng, nhưng dùng thuộc tính viết tắt:

```css
/* styles.css */

.container {
  display: grid;
  grid-template: 50px 50px / 50px 50px 50px;
}
```

Cột và hàng cũng không cần phải dùng cùng các giá trị. Hãy thay đổi giá trị thuộc tính của các cột để cột đầu tiên rộng gấp năm lần những cột còn lại:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="LYzEVGo" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/LYzEVGo">
  Columns and Rows 3 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Grid tường minh và ngầm định

Hãy quay lại ví dụ ban đầu về bố cục 2x2 cho bốn grid item. Điều gì xảy ra nếu thêm item thứ năm vào container mà không thay đổi các thuộc tính `grid-template-columns` hoặc `grid-template-rows`?

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="qBPEdZw" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/qBPEdZw">
  Implicit Grid | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn sẽ nhận thấy item thứ năm được đặt vào grid và nằm trong hàng thứ ba mà chúng ta chưa định nghĩa. Đây là do khái niệm grid ngầm định; nhờ đó CSS Grid có thể tự động đặt grid item khi chúng ta chưa định nghĩa tường minh bố cục cho chúng.

Khi dùng các thuộc tính `grid-template-columns` và `grid-template-rows`, chúng ta đang định nghĩa tường minh các grid track để bố trí grid item. Nhưng khi grid cần thêm track cho nội dung bổ sung, nó sẽ ngầm định tạo các grid track mới. Ngoài ra, các giá trị kích thước được thiết lập từ `grid-template-columns` hoặc `grid-template-rows` không được áp dụng cho các grid track ngầm định này. Tuy vậy, chúng ta có thể định nghĩa giá trị cho chúng.

Chúng ta có thể thiết lập kích thước của grid track ngầm định bằng các thuộc tính `grid-auto-rows` và `grid-auto-columns`. Nhờ vậy, mọi track mới mà grid ngầm định tạo ra cho nội dung bổ sung sẽ dùng các giá trị do chúng ta định nghĩa.

Giả sử chúng ta muốn mọi hàng mới giữ cùng giá trị với kích thước row track tường minh:

```css
/* styles.css */

.container {
  display: grid;
  grid-template-columns: 50px 50px;
  grid-template-rows: 50px 50px;
  grid-auto-rows: 50px;
}
```

Mặc định, CSS Grid sẽ thêm nội dung bổ sung bằng các hàng ngầm định. Điều này có nghĩa là các phần tử thừa sẽ tiếp tục được thêm xuống dưới grid theo chiều dọc. Việc muốn nội dung bổ sung được thêm theo chiều ngang trong grid ít phổ biến hơn, *nhưng* có thể thiết lập bằng thuộc tính `grid-auto-flow: column`, còn kích thước các track ngầm định đó có thể định nghĩa bằng thuộc tính `grid-auto-columns`.

### Gap

Khoảng cách giữa các hàng và cột trong grid được gọi là gutter hoặc alley. Kích thước gap có thể điều chỉnh riêng cho hàng và cột bằng các thuộc tính `column-gap` và `row-gap`. Ngoài ra, chúng ta có thể dùng thuộc tính viết tắt `gap` để thiết lập cả `row-gap` và `column-gap`.

Trước khi thêm các thuộc tính gap cho grid, hãy làm cho mọi thứ dễ nhìn hơn mà không cần dựa vào developer tools. Chúng ta sẽ thêm border quanh các grid item để dễ hình dung vị trí của chúng so với nhau:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="eYGmNzj" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/eYGmNzj">
  Gap 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Tiếp theo, chúng ta sẽ dùng một khoảng cách cột nhỏ để tách hai cột ra một chút:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="wvrBazJ" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/wvrBazJ">
  Gap 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Cuối cùng, chúng ta sẽ thêm một khoảng cách lớn giữa các hàng để làm nổi bật sự khác biệt:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="abLzOmX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/abLzOmX">
  Gap 3 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn cũng có thể thử dùng `gap` viết tắt để thiết lập cả `row-gap` và `column-gap` trong CodePen ở trên.

### Tổng kết grid đầu tiên

Bây giờ đã tạo được một grid, bạn có thể thấy việc kiểm soát bố cục các phần tử bằng CSS Grid dễ dàng đến mức nào. Bạn cũng có thể nhận ra CSS Grid giải quyết được nhiều vấn đề bố cục phổ biến. Trong hai bài học tiếp theo, chúng ta sẽ đề cập đến việc định vị phần tử và các thuộc tính grid nâng cao. Trước hết, hãy xem các liên kết bên dưới để tìm hiểu kỹ hơn về những kiến thức cơ bản khi tạo grid.

### Bài tập

<div class="lesson-content__panel" markdown="1">

- Đọc các phần “Introduction” và “Key Terms” trong [CSS-Tricks Grid Layout Guide.](https://css-tricks.com/css-grid-layout-guide/)
- Xem [video ngắn về track ngầm định và tường minh](https://www.youtube.com/watch?v=8_153Zz4YI8&ab_channel=WesBos) từ khóa học CSS Grid của Wes Bos.
- Xem tài liệu developer tools về [kiểm tra CSS Grid trong Chrome DevTools](https://developer.chrome.com/docs/devtools/css/grid/).

</div>