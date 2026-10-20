### Giới thiệu

Đến lúc này, bạn đã có khá nhiều thực hành di chuyển các phần tử trên màn hình bằng những thứ như margin, padding và flexbox. Các kỹ thuật này đều dựa trên "positioning-mode" mặc định của CSS. Chế độ định vị mặc định này khá trực quan, và bạn sẽ tiếp tục dùng nó cho gần như mọi nhu cầu bố cục. Tuy nhiên, bạn còn có những phương pháp khác có thể rất hữu ích trong một số tình huống.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài.

- Bạn sẽ học cách sử dụng định vị tuyệt đối.
- Bạn sẽ học cách sử dụng định vị cố định.
- Bạn sẽ học cách sử dụng định vị bám dính.
- Bạn sẽ biết sự khác nhau giữa từng thuộc tính và cách kết hợp chúng.

### Định vị tĩnh và tương đối

Chế độ định vị mặc định mà bạn đã quen thuộc là `position: static`. Sự khác nhau giữa static và relative khá đơn giản. Static là vị trí mặc định của mọi phần tử, và các thuộc tính `top`, `right`, `bottom` và `left` không ảnh hưởng đến vị trí của phần tử. Ngược lại, relative gần như giống static, nhưng các thuộc tính `top`, `right...(etc.)` sẽ dịch chuyển phần tử so với vị trí bình thường của nó trong luồng tài liệu.

### Định vị tuyệt đối

`position: absolute` cho phép bạn đặt một phần tử tại vị trí chính xác trên màn hình mà không làm xáo trộn các phần tử khác xung quanh. Cụ thể hơn, dùng định vị tuyệt đối cho một phần tử sẽ loại phần tử đó khỏi luồng tài liệu bình thường, đồng thời định vị nó tương đối với một phần tử tổ tiên. Nói cách khác: các phần tử bị loại khỏi luồng bình thường của tài liệu không ảnh hưởng đến các phần tử khác và cũng không bị các phần tử khác ảnh hưởng. Dùng định vị tuyệt đối cho phép bạn đặt phần tử ở bất cứ đâu trên màn hình bằng các thuộc tính `top`, `right`, `bottom` và `left`. Thuộc tính này thực sự hữu ích khi bạn muốn đặt một thứ ở vị trí chính xác trên màn hình mà không làm xáo trộn các phần tử khác. Một vài trường hợp sử dụng phù hợp cho định vị tuyệt đối là:

- modal
- hình ảnh có chú thích bên trên
- biểu tượng nằm trên các phần tử khác

Trong ví dụ sau, chúng ta dùng định vị tuyệt đối để hiển thị văn bản trên một hình ảnh.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="poWyWeJ" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/poWyWeJ">
Absolute Position | CSS Positioning</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Lưu ý: định vị tuyệt đối có những trường hợp sử dụng rất cụ thể; nếu có thể, nên ưu tiên flexbox hoặc grid. Không nên dùng định vị tuyệt đối để tạo toàn bộ bố cục trang.

### Định vị cố định

Các phần tử fixed cũng bị loại khỏi luồng bình thường của tài liệu và được định vị tương đối với `viewport`. Về cơ bản, bạn dùng các thuộc tính `top`, `right`, `bottom` và `left` để định vị phần tử, và phần tử đó sẽ giữ nguyên vị trí khi người dùng cuộn. Điều này đặc biệt hữu ích cho những thứ như thanh điều hướng và nút trò chuyện nổi.

### Định vị bám dính

Các phần tử sticky sẽ hoạt động như những phần tử bình thường cho đến khi bạn cuộn vượt qua chúng, sau đó chúng bắt đầu hoạt động như các phần tử fixed. Chúng cũng không bị đưa ra khỏi luồng bình thường của tài liệu. Điều này có thể nghe hơi khó hiểu, vì vậy hãy xem [ví dụ về định vị sticky](https://codepen.io/theanam/pen/MPLBYy), có thể nó sẽ giúp bạn hiểu rõ hơn. Định vị này hữu ích cho những thứ như tiêu đề các phần. Bạn còn nhớ việc vẫn có thể nhìn thấy mình đang xem danh mục nào khi cuộn qua một cửa hàng không? Đây là cách thực hiện điều đó!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Video [Learn CSS Position](https://www.youtube.com/watch?v=jx5jmI0UlXU) của Web Dev Simplified có nhịp độ nhanh nhưng biểu diễn trực quan khá tốt các hành vi định vị khác nhau. Hãy xem video.
1. [Tài liệu MDN về `position`](https://developer.mozilla.org/en-US/docs/Web/CSS/position) trình bày tất cả chi tiết khái niệm về định vị.
1. Trang [Absolute, Relative, Fixed Positioning](https://css-tricks.com/absolute-relative-fixed-positioining-how-do-they-differ/) của CSS trick sẽ cung cấp cho bạn một góc nhìn khác về chủ đề này. Bạn cũng nên đọc bài đó.
1. Cuối cùng, bài viết của Kevin Powell thảo luận về [sự khác nhau giữa định vị fixed và sticky](https://www.kevinpowell.co/article/positition-fixed-vs-sticky/). Đây là một bài đọc rất hay để hiểu rõ hơn sự khác biệt.

</div>