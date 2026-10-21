### Giới thiệu

Màu sắc có thể khiến trang hấp dẫn hơn về mặt thị giác, nhưng phối màu sai hoặc chỉ dựa vào màu để truyền đạt thông tin có thể khiến một số người khó nhận biết và hiểu nội dung hơn. Điều đó không có nghĩa bạn phải hạn chế khi chọn bảng màu; bạn chỉ cần cẩn thận hơn khi thực sự *sử dụng* chúng.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Hiểu contrast ratio là gì.
- Biết cách kiểm tra contrast ratio.
- Hiểu vì sao không nên chỉ dùng màu để truyền đạt thông tin.

### Độ tương phản màu

![Contrast ratio issues](https://user-images.githubusercontent.com/70952936/125673709-dd42bdf0-a4bc-4103-9a1b-e73e6c3bc85b.jpg)

Bạn có dễ đọc chữ trong cả ba (đúng vậy, ba) ví dụ trong hình trên không? Nếu không, đó là vì contrast ratio quá thấp. Nếu có, xin chúc mừng thị lực của bạn, nhưng bạn vẫn cần cung cấp website dễ nhận biết cho người khiếm thị.

Contrast ratio là chênh lệch độ sáng giữa hai màu, được biểu diễn dưới dạng một tỷ lệ. Chữ trắng trên nền trắng có tỷ lệ thấp nhất (1:1), còn chữ đen trên nền trắng có tỷ lệ cao nhất (21:1). Contrast ratio áp dụng cho cả văn bản thông thường lẫn hình ảnh chứa văn bản.

Có hai mức độ tuân thủ contrast ratio, mỗi mức đều có quy tắc cho văn bản thông thường và văn bản lớn. **Văn bản thông thường** là chữ có cỡ nhỏ hơn 18 points/24px (hoặc nhỏ hơn 14 points/18.66px nếu in đậm), còn **văn bản lớn** có cỡ ít nhất 18 points/24px (hoặc ít nhất 14 points/18.66px nếu in đậm).

1. **Level AA** (tối thiểu) yêu cầu tỷ lệ ít nhất 4.5:1 cho văn bản thông thường và 3:1 cho văn bản lớn.
1. **Level AAA** (nâng cao) yêu cầu tỷ lệ ít nhất 7:1 cho văn bản thông thường và 4.5:1 cho văn bản lớn.

Cả hai mức đều có ngoại lệ không cần tuân theo các quy tắc về tỷ lệ:

- Văn bản tình cờ xuất hiện, chẳng hạn chữ *vô tình* nằm trong một hình ảnh có nội dung thị giác quan trọng khác, hoặc văn bản chỉ mang tính trang trí.
- Văn bản thuộc một thành phần giao diện không hoạt động hoặc bị vô hiệu hóa, chẳng hạn nút bị vô hiệu hóa và có opacity thấp.
- Văn bản thuộc logo hoặc tên thương hiệu.

Có thể bạn đang nghĩ: "18.66 pixel? 4.5:1? Làm sao nhớ được các con số này? Mà tính tỷ lệ ban đầu thế nào?!" May mắn là bạn không cần phải làm vậy!

[WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) là công cụ tuyệt vời để kiểm tra contrast ratio. Chỉ cần nhập mã HEX của màu tiền cảnh và màu nền, công cụ sẽ tính xem tỷ lệ đạt mức tuân thủ nào, nếu có. Trang này cũng có liên kết tới công cụ kiểm tra tương phản của liên kết, giải thích tỷ lệ cần có khi liên kết văn bản không được gạch chân.

Bạn cũng có thể kiểm tra contrast ratio của văn bản trong một phần tử bằng dev tools của trình duyệt. Trong **Chrome**, nhấp công cụ "element picker" ở tab Elements rồi di chuột lên phần tử trên trang. Tooltip sẽ hiển thị contrast ratio trong phần Accessibility. Bạn cũng có thể chọn phần tử có văn bản trong tab Elements rồi nhấp công cụ chọn màu cho thuộc tính "color" trong pane Styles để xem tỷ lệ.

### Truyền đạt thông tin

Giờ bạn biết cần tính đến contrast ratio khi thêm màu cho văn bản và nền; hãy thử xem bạn có nhận ra nút nào trong hình dưới đây màu đỏ không:

![Color blindness simulation](https://user-images.githubusercontent.com/70952936/125673910-66b43803-3228-4920-98f8-80ac063ef344.jpg)

Đáp án là nút 4. Hình trên mô phỏng achromatopsia, còn gọi là mù màu toàn phần. Đây là điểm quan trọng khi dùng màu: **không nên chỉ dùng màu để truyền đạt thông tin**. Có thể có ngoại lệ cần chỉ dùng màu, nhưng nhìn chung bạn nên tuân theo quy tắc này.

Hãy xem ví dụ khác. Giả sử bạn tạo biểu mẫu với hướng dẫn rằng các trường bắt buộc được biểu thị bằng chữ màu đỏ. Nếu người dùng bị mù màu hoặc khó phân biệt một số màu, việc chỉ dựa vào màu của chữ có thể khiến họ khó, thậm chí không thể, nhận biết hoặc sử dụng biểu mẫu. Thay vì chỉ dùng màu, biểu mẫu có thể đánh dấu mọi trường bắt buộc bằng chữ đỏ và dấu sao:

![Conveying information with color](https://user-images.githubusercontent.com/70952936/125674026-9baafc58-2339-48f4-8b12-892375b87ad7.jpg)