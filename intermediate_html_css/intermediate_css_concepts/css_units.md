### Giới thiệu

Có nhiều đơn vị khác nhau bạn có thể sử dụng để định nghĩa kích thước trong CSS. Bài này sẽ giới thiệu những đơn vị quan trọng nhất đối với bạn và chỉ cho bạn nơi tìm hiểu về các đơn vị còn lại.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Bạn sẽ học sự khác nhau giữa đơn vị tương đối và đơn vị tuyệt đối.
- Bạn sẽ học khi nào nên dùng từng loại đơn vị.

### Đơn vị tuyệt đối

Đơn vị tuyệt đối là những đơn vị luôn có cùng giá trị trong mọi ngữ cảnh. `px` là một đơn vị tuyệt đối vì kích thước của một pixel không thay đổi tương đối với bất kỳ thứ gì khác trên trang. Trên thực tế, `px` là đơn vị tuyệt đối duy nhất bạn nên dùng cho các dự án web. Những đơn vị còn lại phù hợp hơn với môi trường in ấn vì chúng liên quan đến các đơn vị vật lý như `in` (inch) và `cm` (centimeter).

### Đơn vị tương đối

Đơn vị tương đối là những đơn vị có thể thay đổi tùy theo ngữ cảnh. Có một số đơn vị bạn có thể sẽ gặp và muốn sử dụng.

#### em và rem

`em` và `rem` đều liên quan đến cỡ chữ, dù chúng thường được dùng để định nghĩa các kích thước khác trong CSS. Bạn sẽ thường xuyên gặp cả hai, vì vậy chúng ta sẽ giải thích cả hai, nhưng theo nguyên tắc chung, hãy ưu tiên `rem`.

`1em` là `font-size` của một phần tử (hoặc phần tử cha của nó nếu bạn dùng đơn vị này để đặt `font-size`). Ví dụ, nếu `font-size` của một phần tử là `16px`, việc đặt chiều rộng của nó thành `4em` sẽ khiến chiều rộng là `64px` (`16 * 4 == 64`).

`1rem` là `font-size` của phần tử gốc (có thể là `:root` hoặc `html`). Phép tính hoạt động giống với `rem` như với `em`, nhưng không có sự phức tạp do phải theo dõi cỡ chữ của phần tử cha. Việc phụ thuộc vào `em` có thể khiến một kích thước thay đổi khi ngữ cảnh thay đổi; rất có thể đó không phải hành vi bạn muốn.

Dùng kích thước tương đối như `rem` để định nghĩa cỡ chữ trên toàn website *là* điều được khuyến nghị. Nhiều trình duyệt cho phép người dùng thay đổi cỡ chữ cơ sở để tăng khả năng đọc. Nếu có thể, bạn nên tôn trọng mong muốn của người dùng về cỡ chữ. Bạn sẽ tìm hiểu thêm về điều này trong các bài đọc được giao.

#### Đơn vị viewport

Các đơn vị `vh` và `vw` liên quan đến kích thước viewport. Cụ thể, `1vh` bằng `1%` chiều cao viewport và `1vw` bằng `1%` chiều rộng viewport. Chúng hữu ích bất cứ khi nào bạn muốn một thứ được định kích thước tương đối với viewport, chẳng hạn hero có chiều cao toàn màn hình và giao diện giống ứng dụng chiếm toàn màn hình.

### Có quá nhiều đơn vị!

Nếu tra cứu tất cả đơn vị có trong CSS, ngay cả chỉ [các đơn vị độ dài](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/length), bạn sẽ thấy có *rất nhiều*. Làm sao biết nên dùng đơn vị nào và khi nào?

Đừng cố tìm các quy tắc cứng nhắc về việc dùng đơn vị nào trong tình huống nào. Mỗi đơn vị có tác dụng khác nhau, vì vậy cũng như với mọi đoạn mã khác, hãy nghĩ về cách bạn muốn một thứ hoạt động và chỉ dẫn bạn muốn viết, rồi tìm hiểu xem đơn vị nào có thể giúp bạn làm điều đó.

Có thể bạn muốn một kích thước margin dựa trên (và do đó thay đổi theo) cỡ chữ gốc, khi đó bạn sẽ dùng `rem`. Nhưng `rem` có thể không phù hợp cho một margin khác, và bạn thực sự muốn nó cố định bằng giá trị `px`. Có thể bạn muốn chiều rộng của div này bằng một nửa phần tử cha, khi đó chiều rộng `50%` hợp lý; nhưng với div kia, bạn muốn nó bằng một nửa chiều rộng viewport, nên `%` không phù hợp mà `vw` mới phù hợp. Bạn sẽ không ghi nhớ hết những điều này trong thời gian ngắn, và việc ghi nhớ cũng không giúp ích nhiều, nên đừng ngại tra cứu khi cần (điều này đúng với mọi thứ trong hành trình lập trình của bạn).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [CSS values and units](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units) trình bày tất cả các đơn vị hiện có.
1. Bài viết [CSS units](https://web.archive.org/web/20251130034321/https://codyloyd.com/2021/css-units/) đi sâu vào cách thức và thời điểm bạn có thể muốn dùng `em`, `rem` hoặc `px`.
1. [Fun with Viewport Units](https://css-tricks.com/fun-viewport-units/) trình bày một số điều thú vị bạn có thể làm với `vh` và `vw`.

</div>