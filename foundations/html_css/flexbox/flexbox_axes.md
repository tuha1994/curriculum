### Giới thiệu

Hãy xem cách điều khiển hướng của các item bên trong flex container bằng thuộc tính `flex-direction`.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ tìm hiểu về 2 "trục" của flex container.
- Bạn sẽ học cách thay đổi các trục đó để sắp xếp nội dung thành các cột thay vì các hàng.

### Các trục

Điều gây bối rối nhất về flexbox là nó có thể hoạt động theo chiều ngang hoặc chiều dọc, và một số quy tắc sẽ thay đổi đôi chút tùy theo hướng bạn đang dùng.
Hướng mặc định của flex container là chiều ngang, hay `row`, nhưng bạn có thể đổi sang chiều dọc, hay `column`. Có thể chỉ định hướng trong CSS như sau:

```css
.flex-container {
  flex-direction: column;
}
```

Dù đang dùng hướng nào, bạn cũng cần hình dung flex container có 2 trục: trục chính và trục phụ. Khi thay đổi `flex-direction`, hướng của các trục này cũng thay đổi. Trong *hầu hết* trường hợp, `flex-direction: row` đặt trục chính theo chiều ngang (từ trái sang phải), còn `column` đặt trục chính theo chiều dọc (từ trên xuống dưới).

Nói cách khác, trong ví dụ đầu tiên, chúng ta đặt `display: flex` cho một div và nó sắp xếp các phần tử con theo chiều ngang. Đây là minh họa cho `flex-direction: row`, thiết lập mặc định. Ví dụ sau cũng tương tự. Nếu bỏ chú thích dòng có `flex-direction: column`, các div đó sẽ xếp chồng theo chiều dọc.

<p class="codepen" data-height="400" data-default-tab="html,result" data-slug-hash="BaZKPdw" data-editable="true" data-user="TheOdinProjectExamples" style="height: 400px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen [ví dụ về flex-direction](https://codepen.io/TheOdinProjectExamples/pen/BaZKPdw) của TheOdinProject ([@TheOdinProjectExamples](https://codepen.io/TheOdinProjectExamples))
  trên [CodePen](https://codepen.io).</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Một điều cần lưu ý là trong ví dụ này, `flex-direction: column` sẽ không hoạt động như mong đợi nếu ta dùng dạng viết tắt `flex: 1`. Hãy thử ngay (tức là đổi giá trị flex trên dòng `flex: 1 1 auto;`). Bạn có tìm ra lý do `flex: 1` không hoạt động không? Các div bị co lại, dù *rõ ràng* chúng có `height` được định nghĩa.

Lý do là dạng viết tắt flex mở rộng `flex-basis` thành `0`, nghĩa là mọi phép tính `flex-grow` và `flex-shrink` đều bắt đầu từ `0`. Mặc định, div rỗng có chiều cao bằng 0, nên để flex item lấp đầy chiều cao của container, chúng thực ra không cần có height nào cả.

Ví dụ trên khắc phục điều này bằng cách chỉ định `flex: 1 1 auto`, cho flex item dùng `height` được chỉ định làm giá trị mặc định. Ta cũng có thể sửa bằng cách đặt chiều cao cho `.flex-container` cha, hoặc dùng `flex-grow: 1` thay cho dạng viết tắt.

Một chi tiết khác cần chú ý: khi đổi flex-direction thành `column`, `flex-basis` sẽ tham chiếu đến `height` thay vì `width`. Điều này có thể khá hiển nhiên trong ngữ cảnh này, nhưng vẫn cần ghi nhớ.

Chúng ta đã hơi đi chệch khỏi trọng tâm... Ta đang nói về flex-direction và các trục. Quay lại vấn đề chính, hành vi mặc định là `flex-direction: row`, sắp xếp mọi thứ theo chiều ngang. Điều này thường hoạt động tốt mà không cần thay đổi các chi tiết khác trong CSS vì phần tử dạng block mặc định có toàn bộ chiều rộng của phần tử cha. Chuyển sang chiều dọc bằng `flex-direction: column` phức tạp hơn vì phần tử dạng block mặc định có chiều cao bằng nội dung của nó, mà trong trường hợp này lại *không có* nội dung.

Có những tình huống hành vi của `flex-direction` có thể thay đổi nếu bạn dùng ngôn ngữ viết từ phải sang trái hoặc thậm chí viết theo chiều dọc, nhưng bạn chưa cần lo về điều đó cho đến khi sẵn sàng làm website bằng tiếng Ả Rập hoặc tiếng Mãn Châu.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Như mọi khi, tài liệu MDN sẽ là nguồn tham khảo chính, vì vậy hãy xem [tài liệu MDN về `flex-direction`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/flex-direction) và các ví dụ của họ.

</div>