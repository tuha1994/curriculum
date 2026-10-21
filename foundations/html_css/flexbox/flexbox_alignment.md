### Giới thiệu

Cho đến giờ, mọi thứ chúng ta làm với flexbox đều dùng quy tắc `flex: 1` cho tất cả flex item, khiến các item cùng tăng hoặc giảm để lấp đầy toàn bộ không gian khả dụng. Tuy nhiên, rất nhiều khi đó không phải là hiệu ứng mong muốn. Flex cũng rất hữu ích để sắp xếp các item có kích thước cụ thể.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học cách căn chỉnh các item bên trong flex container theo cả chiều dọc lẫn chiều ngang.

### Căn chỉnh

Hãy xem một ví dụ.

<p class="codepen" data-height="400" data-default-tab="html,result" data-slug-hash="MWoyBzR" data-editable="true" data-user="TheOdinProjectExamples" style="height: 400px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/MWoyBzR">
  ví dụ căn chỉnh flex</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Đến đây, hẳn bạn có thể đoán được điều gì xảy ra nếu đặt `flex: 1` cho `.item`. Hãy thử trước khi tiếp tục!

Thêm `flex: 1` vào `.item` khiến mỗi item nở ra để lấp đầy không gian khả dụng, nhưng nếu muốn chúng giữ nguyên chiều rộng mà phân bố theo cách khác trong container thì sao? Làm được chứ!

Xóa `flex: 1` khỏi `.item` rồi thêm `justify-content: space-between` vào `.container`. Kết quả sẽ trông gần như sau:

![space between](https://cdn.statically.io/gh/TheOdinProject/curriculum/495704c6eb6bf33bc927534f231533a82b27b2ac/html_css/v2/foundations/flexbox/imgs/07.png)

`justify-content` căn chỉnh các item dọc theo **trục chính**. Bạn có thể dùng một vài giá trị ở đây. Bạn sẽ học những giá trị còn lại trong phần bài đọc, nhưng trước mắt hãy thử đổi thành `center`; các hộp sẽ được căn giữa theo trục chính.

Để thay đổi vị trí của các item dọc theo **trục phụ**, hãy dùng `align-items`. Thử đưa các hộp vào giữa container bằng cách thêm `align-items: center` vào `.container`. Kết quả mong muốn như sau:

![centered](https://cdn.statically.io/gh/TheOdinProject/curriculum/495704c6eb6bf33bc927534f231533a82b27b2ac/html_css/v2/foundations/flexbox/imgs/08.png)

Vì `justify-content` và `align-items` dựa trên trục chính và trục phụ của container, hành vi của chúng sẽ thay đổi khi bạn đổi flex-direction của flex container. Ví dụ, khi đổi `flex-direction` thành `column`, `justify-content` sẽ căn chỉnh theo chiều dọc còn `align-items` sẽ căn chỉnh theo chiều ngang. Tuy vậy, hành vi phổ biến nhất là mặc định: `justify-content` căn các item theo chiều ngang (vì trục chính mặc định nằm ngang), còn `align-items` căn chúng theo chiều dọc. Đây là một trong những điểm khiến người mới học flexbox dễ bối rối nhất khi hành vi này thay đổi.

#### Gap

Một tính năng rất hữu ích của flex là thuộc tính `gap`. Đặt `gap` trên flex container sẽ thêm một khoảng cách xác định giữa các flex item, tương tự như thêm margin cho chính các item đó. `gap` là một thuộc tính *mới* nên chưa xuất hiện trong nhiều tài liệu, nhưng hoạt động ổn định trên mọi trình duyệt hiện đại, vì vậy bạn có thể yên tâm dùng và sẽ thấy nó rất tiện! Thêm `gap: 8px` vào ví dụ đã căn giữa ở trên sẽ cho ra kết quả dưới đây.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="qBjZyea" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/qBjZyea">
  ví dụ căn chỉnh flex</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Phần bài đọc bên dưới còn nhiều điều để bạn khám phá, nhưng đến đây chắc hẳn bạn đã thấy flexbox hữu ích đến mức nào. Chỉ với những thuộc tính đã học, bạn đã có thể tạo ra những bố cục khá ấn tượng!

Hãy dành thời gian đọc kỹ. Phần này sẽ ôn lại một số nội dung chúng ta đã học, đồng thời đi sâu hơn và đề cập đến vài điều chưa xuất hiện. Đừng quá áp lực vì phải ghi nhớ từng chi tiết ngay lúc này; hãy viết code theo các ví dụ và cố gắng hiểu những gì flexbox *có thể* làm. Khi đến các bài tập thực hành, bạn sẽ cần tra lại những tài liệu này, và điều đó hoàn toàn bình thường. Bạn càng dùng nhiều thì kiến thức càng ghi nhớ lâu... mà bạn sẽ dùng nó *liên tục*. Chúc bạn học vui!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [Hướng dẫn tương tác về Flexbox](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/) tuyệt vời này bao quát mọi điều bạn cần biết. Nó giúp củng cố các khái niệm đã học qua những ví dụ rất thú vị và sáng tạo. Hãy dành thời gian xem qua; một phần có thể là nội dung ôn tập, nhưng những nền tảng ở đây rất quan trọng!
1. ["Hướng dẫn về Flexbox" của CSS Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) là tài liệu kinh điển. Hình ảnh và ví dụ trong đó rất hữu ích. Bạn nên xem lại phần 1–3 và phần 5 (đừng lo về các phần media query, chúng ta sẽ học sau trong khóa học), rồi lưu lại làm tài liệu tra cứu nhanh cho tương lai (hãy để sẵn để dùng trong các bài tập thực hành).
1. Hoàn thành [Flexbox Froggy](https://flexboxfroggy.com/), một trò chơi nhỏ vui nhộn để luyện di chuyển các đối tượng bằng flexbox.
1. Làm các bài tập trong [thư mục `foundations/flex` của kho bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/foundations/flex) (nhớ rằng hướng dẫn nằm trong README) theo thứ tự:
    - `01-flex-center`
    - `02-flex-header`
    - `03-flex-header-2`
    - `04-flex-information`
    - `05-flex-modal`
    - `06-flex-layout`
    - `07-flex-layout-2`

    Lưu ý: Bạn có thể tìm lời giải cho các bài tập này trong thư mục `solution` của từng bài.

</div>