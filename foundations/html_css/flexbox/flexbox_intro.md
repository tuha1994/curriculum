### Giới thiệu

Như bạn sẽ học, có *rất nhiều* cách để di chuyển các phần tử trên một trang web. Qua nhiều năm, những phương pháp mới đã được phát triển còn các phương pháp cũ dần lỗi thời. Flexbox không phải lúc nào cũng có trong CSS — sự ra mắt của nó mang tính *cách mạng*.

Nhiều tài liệu đưa flexbox xuống gần cuối chương trình học vì đây là công nghệ tương đối mới. Nhưng đến nay, nó đã trở thành cách định vị phần tử mặc định của nhiều lập trình viên. Flexbox sẽ là một trong những công cụ được dùng nhiều nhất trong hộp công cụ của bạn, vậy tại sao không học nó trước?

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học cách định vị phần tử bằng flexbox.
- Bạn sẽ tìm hiểu về flex container và flex item.
- Bạn sẽ học cách tạo các component và bố cục hữu ích, không chỉ đơn giản là xếp chồng và căn giữa các item.

### Trước khi bắt đầu

Bố cục flexbox đôi khi có thể hơi phức tạp. Trong bài học trước, bạn đã học cách kiểm tra và gỡ lỗi bằng công cụ dành cho nhà phát triển của trình duyệt. Những công cụ đó sẽ *cực kỳ quan trọng* trong các bài học tiếp theo. Nếu có điều gì không hoạt động như mong đợi, bước đầu tiên của bạn *lúc nào cũng phải là* kiểm tra nó bằng công cụ dành cho nhà phát triển.

Flexbox không hẳn khó hơn các khái niệm khác mà chúng ta đã học, nhưng nó *có* thêm một số yếu tố cần theo dõi. Cho đến khi học xong và ghép mọi thứ lại, bạn sẽ hơi khó áp dụng những điều đang học trong các bài đầu tiên này. Trong quá trình học, hãy tự giúp mình bằng cách **thử nghiệm với tất cả ví dụ code.**

Gần như chắc chắn bạn sẽ cần quay lại tham khảo các bài học này (hoặc một vài tài liệu chúng tôi chia sẻ) khi đến phần bài tập cuối chương. Nhưng nếu kiên nhẫn thử nghiệm với mọi ví dụ code được cung cấp, bạn sẽ biết nên tìm ở đâu khi cần.

### Cùng flex nào!

Flexbox là cách sắp xếp các item thành hàng hoặc cột. Những item này sẽ co giãn (tức là tăng hoặc giảm kích thước) dựa trên một số quy tắc bạn định nghĩa. Để bắt đầu, hãy xem một minh họa.

<div class="lesson-note" markdown="1">

#### CodePen tương tác

Chúng tôi đã nhúng rất nhiều ví dụ tương tác trong các bài học này. Hãy dành thời gian thử nghiệm với chúng trong lúc học để ghi nhớ các khái niệm chắc hơn!

</div>

<p class="codepen" data-height="400" data-default-tab="html,result" data-slug-hash="QWgNxrp" data-editable="true" data-user="TheOdinProjectExamples" style="height: 400px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/QWgNxrp">
  ví dụ flex đầu tiên</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Chúng ta sẽ sớm giải thích chính xác điều gì đang xảy ra ở đây. Nhưng trước mắt, hãy bỏ chú thích hai khai báo CSS liên quan đến flex trong CodePen ở trên bằng cách xóa các thẻ `/*` và `*/` bao quanh chúng, rồi xem kết quả.

<div class="lesson-note" markdown="1">

#### Comment

Comment ngăn trình duyệt diễn giải các dòng như code và được bao quanh bởi những thẻ cụ thể. CSS dùng `/*` làm thẻ mở comment và `*/` làm thẻ đóng comment, còn HTML và JavaScript có cú pháp riêng. Bạn có thể 'bật lại' các dòng code đã comment bằng cách xóa những thẻ comment bao quanh code.

</div>

Lúc này cả 3 div sẽ được sắp xếp theo chiều ngang. Nếu thay đổi kích thước khung kết quả bằng các nút "1x", ".5x" và ".25x", bạn cũng sẽ thấy các div 'co giãn'. Chúng sẽ lấp đầy khu vực khả dụng và mỗi div có cùng chiều rộng.

Nếu thêm một div khác vào HTML, bên trong `.flex-container`, nó sẽ xuất hiện cạnh các div còn lại và mọi thứ sẽ co giãn để vừa với khu vực khả dụng.

<div class="lesson-note" markdown="1">

#### Mở CodePen bên ngoài

Nếu khó nhìn thấy điều gì đang xảy ra trong CodePen được nhúng với kích thước nhỏ, bạn có thể nhấp vào nút "Edit on CodePen" hoặc "Fork on CodePen". Thao tác này sẽ đưa ví dụ vào một môi trường có kích thước đầy đủ. Một số ví dụ về sau đặc biệt có thể hữu ích khi mở theo cách này.

</div>

#### Flex container và flex item

Như bạn đã thấy, flexbox không chỉ là một thuộc tính CSS đơn lẻ mà là cả một bộ công cụ gồm các thuộc tính giúp bạn đặt mọi thứ vào đúng vị trí. Một số thuộc tính được đặt trên *flex container*, còn một số thuộc tính khác được đặt trên *flex item*. Đây là một khái niệm quan trọng.

Flex container là bất kỳ phần tử nào có `display: flex`. Flex item là bất kỳ phần tử nào nằm trực tiếp bên trong một flex container.

![Phần tử được áp dụng 'display: flex' trở thành flex container và các phần tử con là flex item của nó](https://cdn.statically.io/gh/TheOdinProject/curriculum/b2a53579fcbec1cfde47646cc5a2b109cd7772cc/foundations/html_css/flexbox/imgs/03.png)

Hơi khó hiểu một chút, nhưng bất kỳ phần tử nào cũng có thể vừa là flex container *và* vừa là flex item. Nói cách khác, bạn cũng có thể đặt `display: flex` lên một flex item rồi dùng flexbox để sắp xếp các phần tử con *của nó*.

![Flex container có ba flex item con, trong đó một item cũng là flex container với các flex item con riêng](https://cdn.statically.io/gh/TheOdinProject/curriculum/495704c6eb6bf33bc927534f231533a82b27b2ac/html_css/v2/foundations/flexbox/imgs/04.png)

Tạo và lồng nhiều flex container cùng flex item là cách chủ yếu chúng ta sẽ dùng để xây dựng các bố cục phức tạp. Hình ảnh sau được tạo ra *chỉ* bằng flexbox để sắp xếp, định kích thước và đặt các phần tử khác nhau. Flexbox là một công cụ *rất* mạnh.

![Nhiều flex container riêng biệt và lồng nhau, với kích thước và khoảng cách flex item khác nhau, cùng các hướng flex khác nhau](https://cdn.statically.io/gh/TheOdinProject/curriculum/1c08f76bfc94871a3e01bcc2488c82519335b5cd/foundations/html_css/flexbox/imgs/05.png)

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bất ngờ chưa! Không có bài tập! Bạn sẽ học thêm rất nhiều trong các bài tiếp theo, nên hãy tiếp tục.

</div>