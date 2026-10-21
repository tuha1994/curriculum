### Giới thiệu

Bây giờ hãy cùng tìm hiểu về hoạt ảnh CSS bằng keyframes. Phần này sẽ mở rộng những gì bạn đã biết về CSS transitions và đi sâu vào sự khác biệt giữa chúng.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Sự khác biệt giữa CSS transitions và CSS animations.
- Cách cấu hình các thuộc tính con của animation.
- Cách sắp xếp trình tự của một animation bằng keyframes.

### Animations và transitions

Animations cho phép bạn tạo hoạt ảnh cho các phần tử từ cấu hình kiểu dáng này sang cấu hình kiểu dáng khác. Nghe quen thuộc chứ? Bạn sẽ không sai khi nghĩ rằng: "Nếu chúng về cơ bản giống transitions thì học animations để làm gì?", nhưng animations mở rộng đáng kể một số khả năng mà transitions không có. Một vài khác biệt gồm:

- Transitions được thiết kế để tạo hoạt ảnh cho một phần tử từ trạng thái này sang trạng thái khác. Chúng *có thể* lặp lại, nhưng không được thiết kế cho mục đích đó. Ngược lại, animations *được* thiết kế với mục đích cho phép lặp lại một cách rõ ràng.

- Transitions cần một tác nhân kích hoạt, chẳng hạn sử dụng pseudo-class như `:hover` hoặc `:focus`, hay thêm/xóa một class bằng JavaScript. Ngược lại, animations không cần tác nhân như vậy. Khi các phần tử đã được đặt vào vị trí và CSS đã được định nghĩa, animation sẽ bắt đầu chạy ngay nếu đó là điều bạn yêu cầu.

- Transitions không linh hoạt bằng animations. Khi định nghĩa một transition, hãy hình dung bạn đang đưa phần tử đó đi theo một đường thẳng từ điểm A đến điểm B. Đúng là `transition-timing-function` có thể tạo thêm một số biến đổi về thời gian của thay đổi này, nhưng vẫn không thể sánh với mức độ linh hoạt mà animations mang lại.

Nhìn chung, cả animations và transitions đều có trường hợp sử dụng riêng. Vì vậy, ngoài việc cân nhắc những khác biệt trên, bạn cũng nên dùng phán đoán của mình. Ví dụ, nếu cần thay đổi độ mờ của một phần tử khi nó đang active thì dùng animation sẽ là quá mức cần thiết, nhưng nếu cần thực hiện điều gì đó phức tạp hơn, animations sẽ cung cấp những công cụ bạn cần.

### Các thuộc tính animation

Hãy xem một animation hoạt động để hiểu rõ hơn điều chúng ta đang nói đến.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="jOGENZz" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/jOGENZz">
  keyframes 1 longhand</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy chú ý rằng animation đã chạy sẵn và liên tục lặp lại. Chúng ta sẽ đề cập đến rule `@keyframes` ở cuối ví dụ sau, nên trước hết hãy tập trung vào các thuộc tính animation thực sự được sử dụng trong ví dụ trên:

```css
#ball {
  /* ... other CSS properties ... */
  animation-duration: 2s;
  animation-name: change-color;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

Đây được gọi là giai đoạn cấu hình, trong đó chúng ta định nghĩa các thuộc tính animation trên phần tử `#ball`; và đây mới chỉ là nửa đầu của việc định nghĩa một animation. Trong ví dụ này, chúng ta đã:

- Đặt `animation-duration` là hai giây. Điều này có nghĩa là phần tử `#ball` sẽ mất hai giây để hoàn thành một chu kỳ animation.

- Đặt `animation-name` là "change-color", tên này rất quan trọng đối với phần `@keyframes` sắp xuất hiện. Đây chỉ là một tên tùy chỉnh, không phải một giá trị CSS cụ thể. Nếu muốn, chúng ta có thể gọi nó là "pineapples", nhưng với mục đích của mình, "change-color" phù hợp hơn.

- Đặt `animation-iteration-count` là `infinite`, nghĩa là animation này sẽ chạy mãi mãi. Bạn có thể đặt giá trị này là 1, 2 hoặc bao nhiêu lần lặp tùy ý.

- Đặt `animation-direction` là `alternate`. Thuộc tính này quyết định animation sẽ đổi hướng khi hoàn thành một chu kỳ hay đặt lại về điểm bắt đầu và lặp lại. Ở đây, điều đó có nghĩa là `#ball` sẽ đổi mượt trở lại màu ban đầu thay vì "nhảy" thẳng về màu đỏ.

### Keyframes

Bây giờ là lúc xử lý nửa còn lại của định nghĩa animation bằng cách tìm hiểu at-rule `@keyframes`.

```css
@keyframes change-color {
  from {
    background-color: red;
  }

  to {
    background-color: green;
  }
}
```

At-rule `@keyframes` tham chiếu đến tên 'change-color' mà chúng ta đã định nghĩa trước đó. Sau đó, chúng ta dùng các thuộc tính `from` và `to` để thay đổi `background-color` của `#ball` từ đỏ sang xanh lá.

Điều quan trọng cần biết là keyframes dùng phần trăm để biểu thị thời điểm animation diễn ra, và các câu lệnh `from` và `to` thực chất lần lượt là bí danh của `0%` và `100%`. Bạn có thể hiểu `from/0%` là 'ở giây thứ 0' và `to/100%` là 'ở giây thứ 2', theo `animation-duration` trong ví dụ trên. Không có quy tắc cứng nhắc nào về việc nên dùng `from/to` hay `0%/100%`. Chỉ cần chọn một kiểu và dùng nhất quán.

At-rule `@keyframes` cũng định nghĩa một chu kỳ animation. Vì vậy, nếu đổi `animation-iteration-count` ở trên thành 2, quả bóng sẽ đổi `background-color` từ đỏ sang xanh lá, rồi từ xanh lá sang đỏ, sau đó animation sẽ dừng. Đừng nhầm một lần lặp với một vòng lặp hoàn chỉnh; một lần lặp là một chu kỳ đơn từ đầu đến cuối (hoặc từ cuối đến đầu khi đổi hướng).

Bây giờ là lúc giới thiệu cú pháp viết tắt cho các thuộc tính animation và xem qua một chút về tính linh hoạt được bổ sung của cú pháp keyframe. Hãy xem ví dụ trực tiếp bên dưới, rồi xem cách viết.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="css,result" data-slug-hash="zYExOLQ" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/zYExOLQ">
  keyframes 2 shorthand</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

```css
#ball {
  /* ... other CSS properties ... */
  background-color: red;
  animation: 2s change-color infinite alternate;
}

@keyframes change-color {
  from {
    background-color: red;
  }
  
  50% {
    transform: scale(2);
    background-color: blue;
  }

  to {
    background-color: green;
  }
}
```

Ở đây, chúng ta đã thêm một keyframe khác tại thời điểm `animation-duration` đạt 50%, tức là 1 giây. Điều này có nghĩa là ngoài việc `background-color` đổi sang một giá trị bổ sung, chúng ta còn chỉ định quả bóng tăng gấp đôi kích thước. Hãy nhớ rằng các keyframe bổ sung luôn được định nghĩa bằng phần trăm. Chỉ các giá trị `0%/100%` mới có thể dùng bí danh `from/to`.

Hy vọng phần này giúp bạn hình dung được sức mạnh mà cú pháp `@keyframes` mang lại khi điều khiển các thuộc tính animation của một phần tử. Bạn có thể thêm keyframe bất cứ khi nào muốn, điều khiển bất kỳ thuộc tính CSS nào có thể tạo animation, và thỏa sức thêm nét sáng tạo cho các phần tử trên website.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Viết code theo [bài viết MDN về cách sử dụng CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations).
1. Đọc [tài liệu tham khảo về @keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) để hiểu sâu hơn cách keyframes được triển khai.
1. Đọc [hướng dẫn tương tác về keyframes](https://www.joshwcomeau.com/animation/keyframe-animations/) này.
1. Bây giờ hãy tạo một số animation thú vị! Hoàn thành các bài tập trong [thư mục `advanced-html-css/animation` của repository bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/advanced-html-css/animation) (nhớ rằng hướng dẫn nằm trong README) theo thứ tự:
   - `01-button-hover`
   - `02-pop-up`
   - `03-dropdown-menu`

</div>