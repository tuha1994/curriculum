### Giới thiệu

Validation cho phép đặt các ràng buộc hoặc quy tắc cụ thể để quyết định dữ liệu người dùng có thể nhập vào input. Khi người dùng nhập dữ liệu vi phạm quy tắc, một thông báo sẽ xuất hiện, cho biết dữ liệu sai ở đâu và cách sửa.

Validation là thành phần thiết yếu của biểu mẫu được thiết kế tốt. Chúng giúp bảo vệ hệ thống backend khỏi dữ liệu không chính xác và giúp trải nghiệm tương tác với biểu mẫu đơn giản nhất có thể đối với người dùng.

Bài học này sẽ tìm hiểu một số validation tích hợp có thể dùng với form HTML. Chúng ta cũng sẽ đi sâu vào việc tạo kiểu cho validation bằng CSS.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài này.

- Giải thích validation biểu mẫu là gì.
- Biết cách dùng một số validation HTML tích hợp cơ bản.
- Biết cách xây dựng validation tùy chỉnh.

### Validation bắt buộc

Chúng ta thường muốn đảm bảo một số trường đã được điền trước khi gửi biểu mẫu, chẳng hạn email và mật khẩu trong form đăng nhập.

Để bắt buộc một trường, ta thêm thuộc tính `required` vào trường đó:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="vYeZGzB" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

<span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/vYeZGzB">
forms-required-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Để đảm bảo trải nghiệm người dùng tốt và đáp ứng hướng dẫn về khả năng tiếp cận, chúng ta luôn nên chỉ ra trường nào là bắt buộc. Thường có thể thêm dấu sao(\*) vào nhãn của trường bắt buộc như trong ví dụ. Bạn cũng có thể thêm thông báo giải thích rõ ý nghĩa của dấu sao(\*) cho người dùng.

### Validation độ dài văn bản

Đôi khi chúng ta muốn người dùng nhập tối thiểu hoặc tối đa một lượng văn bản vào trường. Ví dụ thực tế là giới hạn 140 ký tự mà X (trước đây là Twitter) từng áp dụng cho trường trạng thái, hoặc ràng buộc độ dài tối thiểu và tối đa cho trường tên người dùng.

#### Validation độ dài tối thiểu

Để thêm validation độ dài tối thiểu, ta thêm cho bộ điều khiển biểu mẫu thuộc tính `minlength` với giá trị số nguyên biểu thị số ký tự tối thiểu muốn cho phép:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="WNZOwgp" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/WNZOwgp">
  forms-min-length-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy thử nhập ít hơn ba ký tự vào vùng văn bản rồi nhấp nút post để xem validation hoạt động.

<div class="lesson-note lesson-note--tip" markdown="1">

#### minlength không đồng nghĩa với required

Bạn có thể tự thử trong ví dụ trên bằng cách không nhập gì vào input; biểu mẫu vẫn sẽ được gửi. Đó là vì như [tài liệu của MDN về minlength](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/minlength) đề cập, "Constraint validation is only applied when the value is changed by the user."

</div>

#### Validation độ dài tối đa

Để thêm validation độ dài tối đa, ta thêm cho bộ điều khiển biểu mẫu thuộc tính `maxlength` với giá trị số nguyên biểu thị số ký tự tối đa muốn cho phép:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="zYEzqJJ" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/zYEzqJJ">
  forms-maximum-length-validations</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Với validation độ dài tối đa, trình duyệt sẽ ngăn người dùng nhập nhiều ký tự hơn giá trị của thuộc tính max length. Hãy tự thử trong ví dụ trên.

#### Kết hợp các validation

HTML cho phép áp dụng bao nhiêu validation tùy ý lên một bộ điều khiển biểu mẫu. Ví dụ, ta có thể thêm cho `<textarea>` đăng bài cả validation `minlength` và `maxlength`:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="vYeZGVY" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/vYeZGVY">
  forms-combining-validations</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Điều này cho chúng ta nhiều khả năng hơn để kiểm soát dữ liệu người dùng nhập.

### Validation khoảng giá trị số

Cũng như thường cần kiểm soát độ dài của bộ điều khiển biểu mẫu dạng văn bản, có nhiều trường hợp chúng ta muốn kiểm soát khoảng giá trị người dùng có thể nhập vào bộ điều khiển biểu mẫu dạng số.

Ta có thể làm việc này bằng thuộc tính `min` và `max`, cho phép đặt giới hạn dưới và giới hạn trên của giá trị nhập vào bộ điều khiển biểu mẫu. Thuộc tính `min` và `max` chỉ hoạt động với các bộ điều khiển biểu mẫu dạng số như input number, date và time. Bạn có thể xem danh sách đầy đủ các phần tử được hỗ trợ trong [tài liệu MDN về thuộc tính `max`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/max#syntax).

Một số trường hợp thực tế là giới hạn số lượng trong form đặt hàng sản phẩm hoặc chọn số hành khách trong form đặt chuyến bay.

#### Validation min

Để thêm validation giá trị tối thiểu, ta thêm cho bộ điều khiển biểu mẫu thuộc tính `min` với giá trị số nguyên biểu thị số nhỏ nhất muốn chấp nhận:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="poWwyxd" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/poWwyxd">
  forms-min-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy thử gửi biểu mẫu với số lượng bằng 0 để xem validation hoạt động.

#### Validation max

Để thêm validation giá trị tối đa, ta thêm cho bộ điều khiển biểu mẫu thuộc tính `max` với giá trị số nguyên biểu thị số lớn nhất muốn chấp nhận:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="XWegdxB" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/XWegdxB">
  forms-max-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy thử gửi biểu mẫu với bảy hành khách để xem validation hoạt động.

### Validation pattern

Để đảm bảo nhận được thông tin chính xác từ người dùng, chúng ta thường muốn dữ liệu khớp với một pattern cụ thể. Ví dụ thực tế gồm kiểm tra mật khẩu, số thẻ tín dụng hoặc mã bưu chính có đúng định dạng hay không.

Để thêm validation pattern, ta thêm cho bộ điều khiển biểu mẫu thuộc tính `pattern`, với giá trị là một [biểu thức chính quy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) (regex). Trong ví dụ này, ta dùng validation pattern để đảm bảo mã ZIP của Hoa Kỳ đúng định dạng (5 chữ số, theo sau tùy chọn là dấu gạch ngang và 4 chữ số nữa):

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="YzrQqRK" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/YzrQqRK">
  forms-pattern-basic-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<div class="lesson-note lesson-note--tip" markdown="1">

#### Regex cho validation pattern

Bạn không thực sự cần đi sâu vào regex cho việc này. Regex có thể rất phức tạp, đặc biệt vì cú pháp chính xác có thể khác nhau khi viết cho thuộc tính HTML, chuỗi JavaScript, regex literal của JavaScript, v.v. Trong thực tế, tốt hơn hết là tìm một regex đã được kiểm chứng phù hợp với nhu cầu thay vì tự xây dựng.

</div>

Nhập mã ZIP không đúng rồi gửi biểu mẫu sẽ hiển thị lỗi validation sau trong trình duyệt: "Please match the requested format". Thông báo này không hữu ích lắm vì không cho biết cách sửa vấn đề.

Một thực hành tốt là dùng thuộc tính `placeholder` để cho người dùng thấy ví dụ về pattern mong đợi mà họ cần nhập:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="LYzLNXv" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/LYzLNXv">
  forms-pattern-with-placeholder-validation</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Thuộc tính pattern chỉ có thể dùng trên các phần tử `<input>`. Một số input vốn đã xác thực dữ liệu theo một pattern nhất định. Ví dụ, trường input email đảm bảo email hợp lệ được nhập, còn phần tử input url kiểm tra URL bắt đầu bằng http hoặc https:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="eYGRZbK" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/eYGRZbK">
  forms-built-in-pattern-validations</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Tạo kiểu cho validation

Các pseudo-class `:user-valid` và `:user-invalid` cho phép chọn các bộ điều khiển biểu mẫu đã vượt qua hoặc không vượt qua validation, đảm bảo trường input giữ trạng thái trung tính cho đến khi người dùng tương tác.

Để xem điều này hoạt động, chúng ta sẽ dùng ví dụ email và website đã xem trước đó:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="dyVRMwx" data-preview="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/dyVRMwx">
  forms-styling-validations</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Ban đầu, các trường email và URL giữ kiểu mặc định của trình duyệt, thể hiện trạng thái trung tính vì người dùng chưa tương tác với chúng. Trước hết, ta chọn các input hợp lệ và đặt viền xanh cho trường input tương ứng.

Khi người dùng nhập giá trị không hợp lệ rồi nhấp hoặc chuyển tab ra khỏi input (tạo thành một lần tương tác hoàn chỉnh), viền sẽ chuyển sang đỏ. Từ đó, viền tự động đổi giữa xanh và đỏ tùy theo tính hợp lệ của nội dung. Hãy thử bằng cách nhập email hoặc URL sai định dạng, nhấp ra ngoài để kích hoạt validation, rồi sửa giá trị qua lại giữa đúng và sai định dạng.

### Kết luận

Các validation tích hợp giúp ích rất nhiều trong việc đảm bảo người dùng nhập dữ liệu chính xác. Chúng nhanh và dễ thêm, nhưng vẫn có giới hạn.

Đôi khi bạn cần các validation mà validation tích hợp không thể thực hiện, chẳng hạn kiểm tra input mật khẩu và input xác nhận mật khẩu có cùng giá trị, hoặc kiểm tra tên người dùng đã được sử dụng hay chưa. Khả năng tạo kiểu cho thông báo validation và nội dung bên trong chúng cũng bị giới hạn.

Trong trường hợp này, chúng ta sẽ cần sáng tạo và tạo validation tùy chỉnh bằng JavaScript và CSS. Bài học sau sẽ đi sâu vào cách thực hiện validation bằng JavaScript.

Cũng cần lưu ý rằng validation phía client không phải giải pháp vạn năng để đảm bảo người dùng nhập dữ liệu chính xác. Để đảm bảo tính toàn vẹn của dữ liệu người dùng đi vào hệ thống, chúng ta cũng nên có validation phía server. Phần này sẽ được đề cập sau trong curriculum.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc và làm theo [Hướng dẫn validation form phía client của MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation). Bỏ qua phần "Validating forms using JavaScript". Phần này sẽ được học trong một bài sau.
1. Đọc [Complete Guide to HTML Forms and Constraint Validation Guide của SitePoint](https://www.sitepoint.com/html-forms-constraint-validation-complete-guide/). Bạn có thể bỏ qua phần "JavaScript and the Constraint Validation API" và "Creating a Custom Form Validator".
1. Xem thread X về [những điều nên và không nên làm trong UX của validation form](https://threadreaderapp.com/thread/1400388896136040454.html).

</div>
