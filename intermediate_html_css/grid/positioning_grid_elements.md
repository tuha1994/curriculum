### Giới thiệu

Trong bài học này, chúng ta sẽ xem xét các phần khác nhau của một grid và khám phá những thuộc tính phổ biến có thể dùng để định vị grid item.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Mô tả sự khác nhau giữa track, line và cell.
- Định vị phần tử bằng cách xác định line bắt đầu và kết thúc.
- Sử dụng ký hiệu viết tắt.

### Ôn lại về track

Trước khi đi thẳng vào việc định vị, hãy thống nhất một số thuật ngữ để hiểu rõ hơn các phần khác nhau của grid. Ở bài trước, bạn đã học rằng khi định nghĩa một grid bằng `grid-template`, bạn đang định nghĩa các *track* mà grid sẽ có. Bạn có thể nghĩ grid track là bất kỳ hàng hoặc cột đơn nào trên grid.

Ví dụ, nếu muốn tạo grid 3x3 với các hàng cao 100 pixel và các cột rộng 100 pixel, chúng ta cần định nghĩa 3 track ngang có chiều cao 100 pixel và 3 track dọc có chiều rộng 100 pixel:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="poWvJXQ" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/poWvJXQ">
  3x3 Layout | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bạn sẽ nhận thấy hai dòng CSS đã bị comment trong CodePen này. Hãy bỏ comment thuộc tính trong selector `.first-row` để thấy grid track giữa line grid hàng thứ nhất và thứ hai.

Tiếp theo, bỏ comment thuộc tính trong selector `.last-column` để thấy grid track giữa line grid cột thứ ba và thứ tư.

### Line

Mỗi khi tạo grid track, các grid line sẽ được tạo ra *ngầm định*. Đây là điều quan trọng. Grid line chỉ được tạo sau khi các grid track đã được định nghĩa. Chúng ta không thể tạo grid line một cách tường minh.

Mỗi track có một line bắt đầu và một line kết thúc. Các line được đánh số từ trái sang phải và từ trên xuống dưới, bắt đầu từ 1. Vì vậy, grid 3x3 ở trên có các line dọc cho cột từ 1 đến 4 và các line ngang cho hàng từ 1 đến 4.

Grid line là thứ chúng ta dùng để định vị grid item. Chúng ta sẽ làm điều đó ngay sau đây, nhưng trước tiên hãy xem kỹ hơn về grid line bằng developer tools.

Nếu mở developer tools trong Chrome, bạn có thể chuyển đến ngăn Layout và tìm phần cài đặt hiển thị Grid overlay. Hãy bảo đảm *show line numbers* được bật. Chọn đúng phần tử trong Grid overlays (ví dụ, đây có thể là `div.container` nếu bạn đang kiểm tra CodePen). Bây giờ bạn sẽ thấy lớp phủ của các grid line.

Bạn cũng sẽ nhận thấy developer tools hiển thị các line âm đối diện với các line dương. Hiện tại bạn chưa cần lo về line âm, nhưng hãy biết rằng đây là một lựa chọn khác khi định vị grid item.

### Cell

Khoảng không gian trong grid được dùng chung bởi một row track và một column track được gọi là grid *cell*. Bạn có thể hình dung grid cell giống như một ô trong bảng tính: một khoảng không được xác định bởi tọa độ *hàng, cột*.

Mặc định, mỗi phần tử con của grid container sẽ chiếm một cell. Trong ví dụ trên, grid có 9 cell (3 hàng x 3 cột), mỗi cell chứa một phần tử con được định vị tự động. Phần tử có chữ “A” chiếm cell nằm ở row track 1 (giữa grid line hàng 1 và 2) và column track 1 (giữa grid line cột 1 và 2). Item có chữ “H” nằm ở cell thuộc row track 3 (giữa grid line hàng 3 và 4) và column track 2 (giữa grid line cột 2 và 3).

Nhưng điều gì xảy ra nếu chúng ta muốn thay đổi thứ tự các grid item? Hoặc nếu muốn item chiếm nhiều hơn một cell?

### Định vị

Để cảm nhận cách định vị item, chúng ta sẽ tạo sơ đồ mặt bằng giả lập cho một căn hộ. Hãy bắt đầu với toàn bộ diện tích căn hộ (grid container) được chia thành grid 5x5. Để ví dụ rõ ràng hơn, chúng ta sẽ dùng màu nền để phân biệt phần không gian container. Lưu ý rằng ở đây chúng ta cũng dùng `display: inline-grid` để container không kéo giãn chiếm không gian như một hộp cấp block. Điều này chỉ giúp chúng ta hình dung không gian rõ hơn.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="rNGaOxB" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/rNGaOxB">
  Positioning 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hiện trạng đây là một căn hộ khá buồn tẻ. Để nó bớt giống một chiếc hộp trống và giống một ngôi nhà hơn, chúng ta sẽ thêm các item vào grid container để đại diện cho những căn phòng khác nhau.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="poWvjgY" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/poWvjgY">
  Positioning 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hầu hết các căn phòng của chúng ta đại diện cho một grid cell. Nhưng chúng ta đã dành cho phòng khách một diện tích lớn. (Hoan hô!) Chúng ta định vị item này bằng `grid-column-start` và `grid-column-end`. Giá trị của các thuộc tính này biểu thị các column grid line mà item bắt đầu và kết thúc tại đó.

Bạn cũng sẽ thấy các giá trị thuộc tính cho việc định vị hàng của item này đã bị comment. Hãy bỏ comment các thuộc tính `grid-row-start` và `grid-row-end` để xem phòng khách có thể lớn hơn nữa bằng cách chiếm grid track giữa grid line hàng thứ nhất và thứ ba.

Các thuộc tính này cho phép dùng grid line hiện có để cho item biết mỗi item phải trải rộng qua bao nhiêu hàng và cột. Hãy dành một chút thời gian thử thay đổi các giá trị thuộc tính ở đây. Nếu số line gây khó hiểu, hãy kiểm tra container bằng dev tools để hiển thị grid overlay.

Tiếp theo, chúng ta cần sử dụng không gian hiệu quả hơn. Chúng ta sẽ làm cho các căn phòng còn lại trải rộng qua nhiều grid cell và lấp đầy phần còn lại của căn hộ.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="jOGEbrX" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/jOGEbrX">
  Positioning 3 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Bây giờ chúng ta đã có bản thiết kế cho toàn bộ căn hộ. Nếu xem selector `#kitchen`, bạn sẽ thấy chúng ta dùng các thuộc tính viết tắt ở đây. `grid-column` chỉ là sự kết hợp của `grid-column-start` và `grid-column-end`, với dấu gạch chéo giữa hai giá trị. `grid-row` là phiên bản viết tắt để thiết lập vị trí hàng của một item.

Một vấn đề trong sơ đồ mặt bằng là phòng tắm và nhà bếp nằm ở hai đầu đối diện của căn hộ. Chúng ta có thể tiết kiệm chi phí đường ống bằng cách đặt hai phòng này cạnh nhau. Bây giờ hãy thử thay đổi vị trí bắt đầu và kết thúc của phòng tắm, phòng ngủ và tủ để phòng tắm nằm ngay cạnh nhà bếp. Bạn có thể dùng thuộc tính dạng đầy đủ hoặc dạng viết tắt.

### grid-area

Bạn đã biết cách định vị grid item bằng line hàng và cột. Nhưng còn những cách khác để định vị item, và đây là lúc mọi thứ có thể hơi khó hiểu.

Có một cách viết tắt thậm chí ngắn hơn để định vị grid item bằng line bắt đầu và kết thúc. Bạn có thể kết hợp `grid-row-start` / `grid-column-start` / `grid-row-end` / `grid-column-end` vào một dòng bằng `grid-area`.

Phòng khách ở trên có thể viết như sau:

```css
/* styles.css */

#living-room {
  grid-area: 1 / 1 / 3 / 6;
}
```

Nhưng `grid-area` cũng có thể chỉ một vài thứ khác.

Thay vì dùng grid line để định vị tất cả item trong grid, chúng ta có thể tạo bố cục trực quan của grid bằng từ ngữ. Để làm vậy, chúng ta đặt tên cho mỗi item trên grid bằng `grid-area`.

Vì vậy, phòng khách có thể viết như sau:

```css
/* styles.css */

#living-room {
  grid-area: living-room;
}
```

Chúng ta có thể làm vậy với mọi grid item và đặt giá trị `grid-area` làm tên cho từng phòng. Sau đó, chúng ta có thể lập bản đồ toàn bộ cấu trúc bằng grid container sử dụng `grid-template-areas`.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="dyVPYpv" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/dyVPYpv">
  grid-template-areas 1 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Ồ! Bạn có thể mở trình duyệt CodePen và phóng to đủ lớn để thực sự đọc dòng bố cục `grid-template-areas` từng dòng. Nhưng công cụ này mang lại một cách hoàn toàn khác để định vị item.

Chúng ta thậm chí có thể dùng `.` để biểu thị các cell trống. Giả sử căn hộ sắp có máy nước nóng và máy giặt/máy sấy. Có thể chúng ta chưa chắc về bố cục chính xác, nhưng dễ dàng hình dung một số khoảng trống bằng cách bỏ bớt diện tích trong phòng tắm và nhà bếp:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="ZEXYbpg" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/ZEXYbpg">
  grid-template-areas 2 | CSS Grid</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Vậy là bây giờ bạn đã biết hai cách rất khác nhau để dùng `grid-area` trên một grid item. Bạn cũng có thể thấy thuật ngữ “grid area” dùng để chỉ một nhóm cell. Ví dụ, tất cả grid cell của phòng khách khi gộp lại là một grid area. Phép so sánh với căn hộ sẽ giúp ích: một grid item có thể chiếm nhiều cell để tạo thành một area của grid, giống như một căn phòng có bốn bức tường trong căn hộ.

### Tổng kết

Khi làm các bài tập, bạn sẽ gặp thêm những thuật ngữ như `span` và `auto` khi định vị grid item qua các track. Ngoài ra còn có các thuộc tính để justify và align grid item tương tự Flexbox. Cách tốt nhất để học tất cả thuật ngữ này và cách định vị item là luyện tập thật nhiều!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết của MDN về [Line-based Placement with CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Line-based_Placement_with_CSS_Grid).
1. Chơi các level 1 - 17 của [CSS Grid Garden](https://cssgridgarden.com/) để luyện định vị item. Lưu ý rằng các level còn lại vượt quá phạm vi của bài học này.
1. Làm bài tập trong [thư mục `intermediate-html-css/positioning-grid` của repository bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/intermediate-html-css/positioning-grid) (đừng quên xem hướng dẫn trong README):
   - `01-basic-holy-grail`

   Lưu ý: Khi làm bài tập này, hãy sử dụng mọi tài liệu và tài nguyên cần thiết để hoàn thành. Bạn *không* được kỳ vọng phải ghi nhớ tất cả vào lúc này. Hãy xem tài liệu, dùng Google, làm bất cứ điều gì cần thiết (ngoài việc xem lời giải) để hoàn thành bài.

</div>