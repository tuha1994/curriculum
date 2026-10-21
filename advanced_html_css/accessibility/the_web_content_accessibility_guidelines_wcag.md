### Giới thiệu

Sau bài trước, bạn đã hiểu hơn accessibility quan trọng thế nào với một số người dùng và có lợi cho *tất cả* người dùng ra sao. Nhưng làm thế nào biết điều gì nên thân thiện với a11y và triển khai thế nào? Có nhiều nguồn giải thích, trong đó người thực sự dựa vào accessibility là nguồn tham khảo tốt nhất.

Một nguồn khác trình bày nhiều cách làm website *dễ tiếp cận hơn* là Web Content Accessibility Guidelines (WCAG). Bài này chỉ lướt qua WCAG để bạn làm quen ở mức cơ bản.

### Tổng quan bài học

- Giải thích mục đích của Web Content Accessibility Guidelines.
- Kể tên 4 nguyên tắc của WCAG.

### Web Content Accessibility Guidelines

WCAG giúp tạo ra một tiêu chuẩn chung về web accessibility. Hãy xem accessibility là đích đến, còn WCAG là một trong những công cụ giúp ta tiến gần tới đích.

WCAG rất hữu ích để xây nền tảng a11y, nhưng không phải đích cuối của accessibility. Đúng như tên gọi, đây là các hướng dẫn; chúng chỉ giúp website *dễ tiếp cận hơn*.

### Bốn nguyên tắc

WCAG được tổ chức quanh bốn nguyên tắc cốt lõi (POUR) cần ghi nhớ khi triển khai tính năng accessibility:

1. **Perceivable:** Người dùng phải nhận biết được thông tin hoặc giao diện được trình bày. Ví dụ, chữ sáng trên nền sáng có thể khó nhận biết với người khiếm thị.
1. **Operable:** Người dùng phải thao tác được với giao diện và điều hướng; giao diện không được yêu cầu một tương tác người dùng không thể thực hiện. Thanh điều hướng chỉ mở menu con khi con trỏ chuột hover chẳng hạn sẽ không thao tác được bằng bàn phím.
1. **Understandable:** Người dùng phải hiểu thông tin hoặc giao diện được trình bày. Nếu gửi form và nhận lỗi "Error 113: Bad data", họ không biết lỗi nghĩa là gì hay sửa thế nào.
1. **Robust:** Nội dung phải truy cập được bằng assistive technologies và user agent hiện tại, đồng thời vẫn truy cập được khi các công nghệ ấy tiến bộ.

#### Mức độ tuân thủ

WCAG có ba mức tuân thủ, mỗi mức gồm các success criteria (quy tắc) cần đáp ứng. Bạn không cần đáp ứng hoàn toàn mức nào trong các bài này, chỉ cần biết chúng tồn tại:

- **Level A**, hay hỗ trợ thiết yếu, là mức tuân thủ tối thiểu của WCAG.
- **Level AA**, hay hỗ trợ lý tưởng, là mức nhiều tổ chức hướng tới; đạt mức này cũng phải đạt Level A.
- **Level AAA**, hay hỗ trợ chuyên biệt, không được khuyến nghị cho toàn bộ website vì một số nội dung có thể khiến việc đạt mức này bất khả thi; đạt mức này phải đạt cả Level A và Level AA.

### (Trước khi) triển khai accessibility

Các bài sau trình bày nhiều khái niệm giúp cải thiện accessibility nhưng không bao quát *mọi* khía cạnh. Mục tiêu chỉ là giới thiệu một số khái niệm phổ biến bạn nên hình thành thói quen kiểm tra.

Dù chỉ mới làm quen một số khái niệm a11y, bạn có thể lo website chưa đủ accessible. Điều đầu tiên cần nhớ là **không website nào đạt 100% accessibility**, vì vậy đừng nhắm tới mục tiêu bất khả thi. Đôi khi mục đích hoặc khái niệm của website còn yêu cầu một số phần không accessible theo một cách nào đó.

Điều thứ hai là những bước đầu tiên hướng tới website accessible cũng quan trọng như các bước sau này. Dù hiện tại chỉ thêm một tính năng a11y, điều bạn nghĩ là bổ sung nhỏ có thể cải thiện rất lớn cho nhiều người. Đừng nghĩ phải hoàn hảo hoặc thêm mọi thứ cùng lúc, nhất là khi mới bắt đầu.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc trang [WCAG Overview](https://www.w3.org/WAI/standards-guidelines/wcag/). Chưa cần lo các link khác; mục tiêu là hiểu phần tổng quan và làm quen với website để dùng khi cần.
1. Đọc lướt [WebAIM's WCAG 2 Checklist](https://webaim.org/standards/wcag/checklist), chú ý disclaimer quan trọng. Hiện chỉ cần hình dung các vấn đề accessibility phổ biến; hãy lưu tài nguyên này để dùng làm checklist khi bắt đầu triển khai accessibility.

</div>