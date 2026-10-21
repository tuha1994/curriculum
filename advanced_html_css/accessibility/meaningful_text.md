### Giới thiệu

Meaningful text khá dễ hiểu: khi người dùng đọc văn bản hoặc nghe văn bản được đọc, họ phải lập tức hiểu ý nghĩa ngay cả khi không có ngữ cảnh xung quanh. Thiếu meaningful text có thể ảnh hưởng mọi người dùng, đặc biệt người dựa vào assistive technologies. Bài học này trình bày một số trường hợp bạn cần cung cấp meaningful text.

### Tổng quan bài học

- Biết cách cung cấp link có ý nghĩa.
- Biết cách cung cấp văn bản có ý nghĩa trong form.
- Biết cách cung cấp thuộc tính `alt` có ý nghĩa cho hình ảnh.

### Link

Hãy xem hai ví dụ link:

```html
<!-- Example 1: Where's "here"? -->
<a href='...'>Click here</a> to start your career in web development!

<!-- Example 2: I love that place! -->
Visit <a href='...'>The Odin Project</a> to start your career in web development!
```

Với người nhìn thấy, link ở Ví dụ 1 có vẻ hoàn toàn rõ. Tuy nhiên, ngoài khả năng điều hướng bằng landmark và heading (như trong bài Semantic HTML), screen reader có thể điều hướng giữa các phần tử cùng loại, chẳng hạn link. Nếu người dùng điều hướng qua tất cả link trên trang, thứ duy nhất được đọc ở Ví dụ 1 là "Click here, link." Chính xác "here" là đâu? Không có ngữ cảnh xung quanh, link không có ý nghĩa. Nếu nhiều link trên trang có cùng nội dung, người dùng còn được bảo phải "click here" nhiều lần.

Ngược lại, link trong Ví dụ 2 có ý nghĩa với mọi người trong ngữ cảnh, và cũng có ý nghĩa *ngoài ngữ cảnh* với người dùng screen reader: "The Odin Project, link."

Khi thêm link vào trang, hãy tuân theo vài quy tắc:

1. Nội dung văn bản của phần tử `<a>` phải cho biết link dẫn đến đâu và nên ngắn (khoảng 100 ký tự). Tránh các cụm như "click here" hoặc "this page".
1. Nếu link mở hoặc tải file, hãy cho biết loại file và kích thước.
1. Nếu link tự động mở tab hoặc cửa sổ mới bằng thuộc tính `target="_blank"`, hãy báo cho người dùng biết.

```html
<!-- Example 1: Now the user is aware that this link will open or download a PDF file. -->
<a href='...'>2021 Sign Up Statistics (PDF, 1MB)</a>

<!-- Example 2: And now the user knows this link opens in a new tab! -->
<a href='...'>GitHub (opens in new tab)</a>
```

Lần tới khi dùng link, hãy tự đọc thành tiếng nội dung phần tử. Nó có cho biết hợp lý link sẽ đưa bạn tới đâu, chẳng hạn tiêu đề trang, bài viết hoặc video không? Bạn có biết link tự mở tab mới hay mở hộp thoại tải xuống không? Nếu đã thử screen reader, cách tốt hơn nữa là dùng chính screen reader để kiểm tra meaningful text của link.

### Form

Cung cấp lỗi có ý nghĩa khi người dùng điền hoặc gửi form có thể biến trải nghiệm từ khó chịu thành... có lẽ chưa vui, nhưng ít nhất bớt khó chịu. Hãy xem các lỗi từ không hữu ích đến hữu ích:

```html
<!-- Example 1: Huh? -->
<div class='input-error'>Error: Invalid input.</div>

<!-- Example 2: That makes more sense. -->
<div class='input-error'>Error: Email is invalid.</div>

<!-- Example 3: Even better! -->
<div class='input-error'>Error: 'JohnSmith@@test.com' is not valid. Example of a valid email: example@yourdomain.com.</div>
```

Ngay cả khi biết input nào gây lỗi ở Ví dụ 1, điều không phải lúc nào cũng đúng, thông báo vẫn không có meaningful text. Input nào không hợp lệ? Vì sao? Sửa thế nào? Không câu hỏi nào được trả lời. Hãy tưởng tượng lỗi này vô nghĩa thế nào với người dùng assistive technologies, khi họ có thể không thấy lỗi hiển thị ở đâu và chỉ nghe "invalid input".

Lỗi ở Ví dụ 2 cho biết rõ input nào không hợp lệ, nên bạn biết phải đi đâu. Dù có thể đó là tất cả điều người dùng cần, lỗi vẫn khá mơ hồ: ta không biết email không hợp lệ *vì sao*.

Lỗi ở Ví dụ 3 có ý nghĩa hơn nữa. Nó cho biết input sai và cách sửa. Nhìn chung, khi báo lỗi form, hãy cho biết input gây lỗi và khi có thể, cách sửa hoặc lý do lỗi xảy ra.

Một cách khác để cung cấp meaningful text trong form là dùng instruction, chẳng hạn password input liệt kê các ký tự bắt buộc ("Must include at least one uppercase letter and one number..."). Instruction riêng cho một input nên đặt cạnh input. Instruction áp dụng chung cho form, như cho biết input nào bắt buộc, nên đặt ở đầu form ("* indicates a required field") hoặc cạnh input/label ("Name (required)").

### Văn bản thay thế

Đến đây bạn hẳn đã quen với thuộc tính `alt` trên phần tử `img`. Dù quen hay chưa, hãy xem ví dụ nào hợp lệ:

```html
<!-- Example 1 -->
<img src='...' alt='' />

<!-- Example 2 -->
<img src='...' alt='Odin' />
```

Cả hai ví dụ đều hợp lệ! Ví dụ 1 không có meaningful text (có thể là một *sự thiếu vắng* có chủ ý), nhưng bạn cần hiểu tầm quan trọng của nó. Khi hình ảnh chỉ để trang trí hoặc không quan trọng với người dùng, nhìn chung bạn không muốn assistive technologies thông báo về nó. Khi đó, **luôn** dùng chuỗi rỗng cho giá trị `alt` như Ví dụ 1 (còn gọi là null value, không phải data type JavaScript). Nếu bỏ thuộc tính `alt`, sự hiện diện của ảnh vẫn có thể được thông báo và gây khó hiểu, nhất là khi tên file là chuỗi chữ số ngẫu nhiên.

Ở Ví dụ 2, screen reader sẽ đọc "Odin, graphic", cho người dùng biết có hình ảnh và hình ảnh là gì. Tuy nhiên, alternative text phù hợp cuối cùng vẫn phụ thuộc nhiều yếu tố.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc [Alternative Text - WebAIM](https://webaim.org/techniques/alttext) để học khi nào và cách thêm alternative text cho hình ảnh dựa trên chức năng của ảnh cùng ngữ cảnh xung quanh.
  1. Đọc [Usable and Accessible Form Validation and Error Recovery](https://webaim.org/techniques/formvalidation/) để học các cách cung cấp lỗi cho người dùng và ưu nhược điểm của từng cách.

</div>