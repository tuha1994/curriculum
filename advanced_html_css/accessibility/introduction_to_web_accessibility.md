### Giới thiệu

Đến thời điểm này, bạn đã học những khái niệm rất có giá trị trong lộ trình full stack đã chọn, tạo được các dự án đáng đưa vào hồ sơ, và hiểu rõ hơn về HTML cùng CSS. Thậm chí có thể nói bạn đã bắt đầu thích làm việc với CSS...?

Tuy vậy, có lẽ bạn chưa hiểu nhiều về accessibility (khả năng tiếp cận), thường được gọi là "a11y" (vì có 11 chữ cái nằm giữa chữ cái đầu và cuối). Đáng tiếc là nhiều người không biết nhiều về chủ đề này hoặc không tính đến nó khi phát triển website. Nếu thuộc một trong hai nhóm đó, bạn có thể đã hình thành những thói quen không thật sự thân thiện với a11y. Trước khi tìm hiểu cách thay đổi các thói quen ấy và triển khai những khái niệm thân thiện với a11y, điều quan trọng là phải nắm một số thông tin cơ bản về khả năng tiếp cận web.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích khả năng tiếp cận web là gì.

### Khả năng tiếp cận web là gì?

Khả năng tiếp cận web nghĩa là website, công cụ và công nghệ được thiết kế, phát triển để người khuyết tật cùng những người gặp các hạn chế do hoàn cảnh có thể sử dụng với ít rào cản nhất có thể.

Có nhiều dạng khuyết tật, gồm (nhưng không chỉ gồm) khiếm khuyết thính giác, thể chất/vận động, nhận thức hoặc thị giác. Khuyết tật có thể vĩnh viễn, chẳng hạn người hoàn toàn mù hoặc điếc, hoặc tạm thời, chẳng hạn người bị gãy tay. Một người có thể đồng thời có nhiều khuyết tật. Người lớn tuổi với năng lực thay đổi cũng có thể gặp những tình trạng tương tự người khuyết tật.

Các hạn chế do hoàn cảnh hơi khác. Dùng điện thoại ngoài trời vào ngày nắng, duyệt website bằng một tay khi tay kia đang bận, hoặc sống ở nơi Internet chậm hay băng thông đắt đều là những ví dụ. Không giống khuyết tật, các hạn chế này chỉ xuất hiện trong những tình huống cụ thể, nhưng vẫn rất quan trọng khi phát triển website.

### Vì sao khả năng tiếp cận web quan trọng?

Hãy xem một ví dụ không liên quan đến web để có thêm góc nhìn. Hãy tưởng tượng bạn ở trong một tòa nhà nhiều tầng không có thang máy. Với một số người, điều đó chỉ gây bất tiện: "Không có thang máy à? Vậy đi bộ lên vài tầng thôi." Nhưng với người cần xe lăn, việc đi lên trên tầng một là bất khả thi hoặc ít nhất khó khăn hơn rất nhiều. Ngay cả khi có người nâng xe lăn qua từng bậc, quá trình ấy vẫn khó hơn nhiều. Điểm chính là thang máy sẽ khiến tòa nhà dễ tiếp cận hơn.

Tòa nhà là website, còn thang máy là tập hợp nhiều tính năng và công cụ accessibility (...một chiếc thang máy khá lớn). Khi phát triển website, bạn đang phát triển cho người dùng, và website phải thực sự *usable* (có thể sử dụng được) đối với họ. Người khuyết tật, người lớn tuổi có năng lực thay đổi, người chưa thành thạo công nghệ và người gặp hạn chế do hoàn cảnh đều là người dùng; website nên có khả năng sử dụng công bằng nhất có thể đối với họ.

Một lý do lớn khác khiến accessibility quan trọng là tùy quốc gia, có thể có luật *yêu cầu* triển khai khả năng tiếp cận.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc các tài nguyên sau:
   - [Tài liệu tham khảo về tính năng accessibility](https://www.w3.org/WAI/people-use-web/abilities-barriers/) của W3C để hiểu rõ hơn một số người khuyết tật bị ảnh hưởng thế nào bởi website không dễ tiếp cận.
1. Xem [các video về những góc nhìn của Web Accessibility](https://www.w3.org/WAI/perspective-videos/) của W3C để thấy người dùng nào có thể hưởng lợi từ các tính năng accessibility và hưởng lợi ra sao. Mỗi video khá ngắn, có mô tả âm thanh và bản chép lời. Nếu muốn, trang này cũng liên kết tới bản tổng hợp toàn bộ video trên YouTube.

</div>