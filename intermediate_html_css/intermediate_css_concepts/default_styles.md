### Giới thiệu

Trình duyệt áp dụng một tập hợp style mặc định cho mọi trang web. Có thể bạn chưa từng trực tiếp nghĩ về điều này, nhưng chắc chắn đã gặp chúng.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Bạn sẽ tìm hiểu về style mặc định của trình duyệt.
- Bạn sẽ học cách dùng CSS reset để loại bỏ hoặc thay đổi những style mặc định đó.

### Style mặc định là gì và chúng đến từ đâu?

Khi làm các dự án, có thể bạn đã quan sát thấy style mặc định được áp dụng cho một số phần tử, chẳng hạn tiêu đề trên phần tử `h1` lớn hơn và đậm hơn, cũng như liên kết trên phần tử `a` có màu xanh và được gạch chân. Rất có thể bạn cũng từng gặp khó khăn với những thứ như margin và padding mặc định. Những style này thuộc về user-agent stylesheet, giúp bảo đảm các trang web có kiểu dáng cơ bản ngay cả khi không có CSS. Mỗi trình duyệt có tập user-agent stylesheet riêng, nên style mặc định có thể hơi khác nhau giữa các trình duyệt.

### Nếu không thích các giá trị mặc định thì sao?

Ngoại trừ rất ít trường hợp, bạn chỉ cần viết các rule CSS của riêng mình. Các rule bạn viết trong stylesheet có độ ưu tiên cao hơn rule của user agent, nên sẽ ghi đè các giá trị mặc định. Tuy nhiên, còn một lựa chọn khác.

Để xử lý sự không nhất quán giữa các trình duyệt và thiết lập điểm khởi đầu nhất quán cho việc tạo kiểu, một số nhà phát triển bắt đầu dùng CSS reset. Đây là các stylesheet chứa những rule CSS nhằm thay đổi hoặc loại bỏ các giá trị mặc định do user-agent stylesheet thiết lập. Dùng chúng có thể giúp tạo sự nhất quán và cung cấp một nền tảng sạch để nhà phát triển áp dụng style mà không bị can thiệp.

CSS reset vẫn được sử dụng phổ biến, nhưng không bắt buộc. Một số nhà phát triển chọn không dùng, trong khi những người khác tự tạo reset hoặc dùng reset có sẵn. Điều quan trọng là hiểu rằng reset mang tính chủ quan và có quan điểm riêng, phản ánh sở thích của nhà phát triển tạo ra nó. Bạn có thể tự quyết định cách mình muốn làm.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Reboot, Resets, and Reasoning](https://css-tricks.com/reboot-resets-reasoning/) để tìm hiểu lịch sử của reset và ý nghĩa của việc một reset mang quan điểm riêng.
1. Đọc [Making the case for CSS normalize and reset stylesheets in 2023](https://mattbrictson.com/blog/css-normalize-and-reset). Bài viết trình bày rất tốt sự khác nhau giữa các reset và lý do bạn có thể chọn dùng chúng.
1. [Custom CSS reset của Josh Comeau](https://www.joshwcomeau.com/css/custom-css-reset/) cũng giải thích kỹ quá trình suy nghĩ phía sau từng rule được sử dụng, giúp bạn hình dung cách lập luận về những ý tưởng này.

</div>