### Giới thiệu

Forms là một phần quan trọng của hầu hết website. Gần như mọi site lớn đều có form đăng ký, form liên hệ, form tìm kiếm và nhiều loại khác! May mắn là HTML5 và JavaScript có một số method tích hợp sẵn hữu ích. Bạn đã học về validation bằng HTML và tạo kiểu cho validation bằng CSS trong bài học [Form Validations](https://www.theodinproject.com/paths/full-stack-javascript/courses/intermediate-html-and-css/lessons/form-validation) thuộc khóa Intermediate HTML and CSS.

Trong bài học này, chúng ta sẽ tìm hiểu Constraint Validation API: một cách để validate forms ở frontend bằng JavaScript.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Tầm quan trọng của validation trong HTML forms.
- Constraint Validation API.
- Validation chỉ bằng JavaScript.

### Assignment

<div class="lesson-content__panel" markdown="1">

1. Làm theo [tutorial về Form Validation](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#validating_forms_using_javascript), trong đó trình bày cách dùng JavaScript để validate forms, bao gồm Constraint Validation API.

1. Bạn cũng sẽ có lợi khi đọc qua [tài liệu Constraint Validation](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation).

#### Luyện tập thêm một chút

1. Xây dựng một browser form thu thập các trường Email, Country, Postal Code, Password và Password Confirmation. Form này nên dùng live inline validation để cho user biết một field đã được điền đúng hay chưa. Nghĩa là phải validate khi bạn nhập, không chỉ khi submit form. Khi một field không hợp lệ, field đó nên được highlight màu đỏ và hiển thị thông báo lỗi để hướng dẫn user.

   Form không cần thực sự submit đi đâu, nhưng bạn nên hiển thị thông báo lỗi tùy chỉnh nếu user cố submit form khi còn lỗi đang hoạt động hoặc các required field chưa được điền. Trong phạm vi bài học này, hãy bảo đảm element `<form>` có [thuộc tính `novalidate`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form#novalidate), thuộc tính này cho phép bạn thực hiện **toàn bộ** validation trong các file JavaScript. Bạn vẫn có thể dùng các loại `<input>` khác nhau, nhưng sẽ cần dùng JavaScript để kiểm tra và báo cáo tính hợp lệ của chúng. Nếu mọi thứ đều ổn và form được "submit", hãy tặng user một high five.

   1. Thiết lập một tài liệu HTML trống.
   1. Hãy suy nghĩ xem bạn sẽ thiết lập các form element khác nhau và các validator đi kèm như thế nào. Bạn sẽ cần những object và function nào? Suy nghĩ vài phút có thể giúp bạn tránh lãng phí cả giờ code. Điều tốt nhất bạn có thể làm là phác thảo toàn bộ solution trên bảng trước khi chạm vào máy tính.
   1. Viết các form element.
   1. Thêm code JavaScript kiểm tra validation khi user đi qua form. Khi user rời khỏi một form field, field đó phải tự động được validate.
   1. Test mọi trường hợp có thể xảy ra.
   1. Đừng quên tạo kiểu cho validation bằng CSS thông qua các pseudo-class `:user-valid` và `:user-invalid`!

1. Quay lại project Library và thêm validation tùy chỉnh cho form đó! Hiển thị thông báo lỗi tùy chỉnh khi user cố submit một form field trống (ví dụ: "The author name must be filled!"). Đừng quên sử dụng workflow git branch mà bạn đã học trong [Revisiting Rock Paper Scissors](https://www.theodinproject.com/lessons/foundations-revisiting-rock-paper-scissors) từ Foundations để làm việc trên một feature mới.

</div>