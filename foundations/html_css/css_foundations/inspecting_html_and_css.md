### Giới thiệu

Biết cách kiểm tra và gỡ lỗi HTML, CSS là kỹ năng thiết yếu trong phát triển frontend. Bài học này sẽ hướng dẫn bạn qua Chrome Dev Tools, công cụ cho phép xem thông tin chi tiết về các phần tử và quy tắc CSS, đồng thời hỗ trợ tìm và sửa lỗi trong mã.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Bạn sẽ biết cách mở trình kiểm tra phần tử.
- Bạn sẽ biết cách chọn và kiểm tra từng phần tử cụ thể.
- Bạn sẽ biết cách thử nghiệm HTML và CSS trong trình kiểm tra.

### Trình kiểm tra

Để mở trình kiểm tra, bạn có thể nhấp chuột phải vào bất kỳ phần tử nào trên trang web rồi chọn "Inspect", hoặc nhấn F12. Hãy thử làm ngay bây giờ để xem HTML và CSS được sử dụng trên trang này.

Đừng để tất cả công cụ đang hiện ra làm bạn choáng ngợp! Trong bài này, chúng ta chỉ tập trung vào các bảng Elements và Styles.

### Kiểm tra phần tử

Trong bảng Elements, bạn có thể xem toàn bộ cấu trúc HTML của trang. Bạn có thể nhấp vào bất kỳ phần tử nào trong bảng để chọn đúng phần tử đó. Ngoài ra, bạn có thể nhấp vào biểu tượng "element select" ở góc trên bên trái (được hiển thị trong hình bên dưới), rồi di chuột qua bất kỳ phần tử nào trên trang.

![Biểu tượng trình kiểm tra](https://cdn.statically.io/gh/TheOdinProject/curriculum/e7ab90e044fba9f8ef9b5915c62221e2822b102b/foundations/html_css/css-foundations/inspecting-html-and-css/imgs/00.jpg)

Khi một phần tử được chọn, bảng Styles sẽ hiển thị mọi kiểu đang được áp dụng, cũng như những kiểu đang bị ghi đè (được biểu thị bằng cách gạch ngang văn bản). Ví dụ, nếu dùng trình kiểm tra để nhấp vào từ "Your" trong tiêu đề "Your Career in Web Development Starts Here" trên [trang chủ TOP](https://www.theodinproject.com/home), ở phía bên phải bạn sẽ thấy tất cả kiểu hiện đang tác động lên phần tử đó, như hình bên dưới (đừng lo về cú pháp `var()`, vì nó không liên quan đến điểm này):

![Kiểu bị ghi đè](https://cdn.statically.io/gh/TheOdinProject/curriculum/e7ab90e044fba9f8ef9b5915c62221e2822b102b/foundations/html_css/css-foundations/inspecting-html-and-css/imgs/01.jpg)

### Thử nghiệm kiểu trong trình kiểm tra

Bảng Styles cũng cho phép bạn chỉnh sửa kiểu trực tiếp trong trình duyệt. Bạn có thể nhấp vào bên trong một bộ chọn bất kỳ để thêm quy tắc mới, hoặc nhấp vào thuộc tính hay giá trị hiện có để thay đổi nó. Khi làm vậy, trang web sẽ phản hồi với các thay đổi theo thời gian thực. Việc này không ảnh hưởng đến mã nguồn trong trình soạn thảo, nhưng cực kỳ hữu ích để nhanh chóng thử nhiều thuộc tính và giá trị khác nhau mà không phải liên tục tải lại trang.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy xem qua các phần sau trong [tài liệu Chrome DevTools chính thức](https://developers.google.com/web/tools/chrome-devtools):

   - [Tổng quan về DevTools](https://developer.chrome.com/docs/devtools/overview/): đừng chuyển sang các trang khác được liên kết ở đây; hiện tại chỉ cần làm quen với *những* công cụ có trong DevTools, thay vì học cách sử dụng tất cả chúng.
   - [Mở Chrome DevTools](https://developer.chrome.com/docs/devtools/open/): tương tự những gì chúng ta vừa trình bày ở trên, nhưng có thêm một số nội dung hữu ích.
   - [Bắt đầu xem và thay đổi DOM](https://developer.chrome.com/docs/devtools/dom/): bỏ qua mọi phần sử dụng bảng điều khiển JavaScript.
   - [Xem và thay đổi CSS](https://developer.chrome.com/docs/devtools/css): nhớ làm theo các hướng dẫn tương tác!

</div>