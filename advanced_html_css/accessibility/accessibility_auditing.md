### Giới thiệu

Giờ bạn đã có kiến thức cần thiết để làm website dễ tiếp cận hơn với nhiều người dùng, câu hỏi là: làm sao kiểm tra việc triển khai các tính năng a11y có đúng không? Có lỗi nào cần sửa hoặc điểm nào có thể cải thiện không? Bài học này sẽ trả lời các câu hỏi đó và giúp nâng cao kỹ năng a11y của bạn.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Mở phần accessibility trong DevTools của trình duyệt.
- Audit một trang web bằng công cụ audit của bên thứ ba.

### DevTools cho accessibility

DevTools của trình duyệt rất hữu ích cho nhiều việc, từ kiểm tra style áp dụng cho trang tới debug code, điều này bạn đã biết! Nhưng có thể bạn chưa biết rằng DevTools còn cho phép xem nhiều tính năng accessibility, rất phù hợp để thực hiện một dạng "quick audit". Bạn có thể kiểm tra contrast ratio (như đã đề cập ở bài trước), xem các thuộc tính accessibility và xem accessibility tree, cùng nhiều tính năng khác.

### Audit accessibility

Có rất nhiều công cụ bên thứ ba để audit accessibility của trang web, mỗi công cụ đều có ưu và nhược điểm. Ở đây chúng ta chỉ nhắc đến ba công cụ. Tạo thói quen audit website sẽ giúp bạn tìm các vấn đề a11y còn sót mà mình bỏ lỡ. Dù chọn một trong các công cụ này hay công cụ khác, hiện tại bạn chỉ nên tập trung sửa các vấn đề liên quan đến những khái niệm đã được giới thiệu trong phần curriculum này.

- [axe DevTools for Chrome](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd?hl=en-US) là công cụ dạng extension, trả về danh sách vấn đề được xếp theo mức độ nghiêm trọng và đánh dấu các vấn đề cần bạn kiểm tra thủ công.

- [Lighthouse for Chrome](https://developers.google.com/web/tools/lighthouse) có sẵn mặc định trong Chrome DevTools (có thể cũng được liệt kê là tab Auditing), hoặc có thể chạy từ command line. Lighthouse không chỉ audit a11y mà còn kiểm tra performance, best practices, search engine optimization (SEO) và progressive web app (PWA) nếu phù hợp. Các vấn đề được chia theo danh mục; giống axe DevTools, có thể có danh sách vấn đề cần kiểm tra thủ công.

- [WebAIM's WAVE](https://wave.webaim.org/) là công cụ trên website, nơi bạn nhập URL của trang muốn audit. Công cụ cũng có extension trình duyệt và API. WAVE trả về bản xem trước trang với lớp phủ biểu tượng; các vấn đề được chia thành cảnh báo, warning và lỗi tương phản. Những biểu tượng đặt trên trang có thể làm hỏng layout, nhưng đây có thể là vấn đề nhỏ nếu bạn tập trung hơn vào các vấn đề a11y được tìm thấy.

Dĩ nhiên, một trong những cách tốt nhất để kiểm tra accessibility của website là nhận phản hồi từ người dùng dựa vào các tính năng accessibility đó. Không phải lúc nào cũng dễ làm, nhưng rất đáng lắng nghe những người có thể bị ảnh hưởng bởi accessibility (hoặc việc thiếu accessibility) của trang.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc các tài nguyên sau:
    - [Accessibility features reference](https://developer.chrome.com/docs/devtools/accessibility/reference/#tab), bắt đầu từ phần Accessibility tab, để có cái nhìn ngắn gọn về các tính năng accessibility của Chrome trong DevTools.
    - Phần [Emulate vision deficiencies](https://developer.chrome.com/blog/new-in-devtools-83/#vision-deficiencies) trong trang cập nhật Chrome 83.
    - Phần [Open the Issues tab](https://developer.chrome.com/docs/devtools/issues/#open). Bạn có thể bỏ qua các nội dung không liên quan đến accessibility; mục tiêu chỉ là biết cách mở tab này trong DevTools. Khi mở được, bạn sẽ thấy các vấn đề a11y cùng những vấn đề khác.
    - Dù các trình duyệt có khác biệt, chẳng hạn giá trị của thuộc tính role hoặc cách hiển thị thuộc tính a11y, hãy xem phần ["Features of the Accessibility panel" trong tài liệu MDN](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html#features-of-the-accessibility-panel). Thông tin này thiên về Firefox nhưng vẫn có thể hữu ích cho người dùng Chrome.

</div>