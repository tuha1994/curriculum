### Giới thiệu

Trong dự án này, bạn sẽ tạo một món đồ chơi khá thú vị để rèn luyện kỹ năng thao tác DOM. Bạn sẽ xây dựng phiên bản trên trình duyệt của một thứ nằm giữa bảng vẽ và Etch-A-Sketch.

Dự án này *không nên* dễ. Có lẽ bạn sẽ phải thường xuyên Google để tìm đúng các phương thức JavaScript và CSS cần dùng – thực ra, đó chính là mục đích! Bạn *có thể* xây dựng dự án bằng những công cụ đã học. Trên mạng có rất nhiều tài nguyên để học những thứ chúng ta chưa đề cập nếu bạn thấy cần. Chúng tôi sẽ hướng dẫn các bước cơ bản, nhưng bạn sẽ phải tự triển khai chúng.

Nếu hoàn toàn bế tắc, hãy ghé [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để hỏi trợ giúp. Sẽ có người hướng dẫn bạn đi đúng hướng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Đừng quên commit sớm và thường xuyên! Bạn có thể xem lại [bài học Thông điệp commit](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/commit-messages)!

1. Làm theo hướng dẫn của chúng tôi về [cách thiết lập kho GitHub cho dự án](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/recipes#setting-up-your-projects-github-repository).
1. Tạo một trang web có lưới 16x16 gồm các div hình vuông.
    - Tạo các div bằng JavaScript. Đừng tự tạo chúng bằng cách sao chép và dán thủ công vào tệp HTML!
    - Tốt nhất nên đặt các ô vuông của lưới bên trong một div "container". Bạn có thể viết div này trong tệp HTML.
    - Dùng Flexbox để khiến các div xuất hiện dưới dạng lưới \(thay vì mỗi div nằm trên một dòng\). Dù tên gọi có thể khiến bạn tò mò, đừng nghiên cứu hoặc dùng CSS Grid, vì nó sẽ được dạy ở một bài học sau khi hoàn thành Foundations path. Dự án này đặc biệt nhằm giúp bạn luyện tập Flexbox!
    - Hãy cẩn thận với đường viền và lề, vì chúng có thể điều chỉnh kích thước các ô vuông!
    - "OMG, tại sao lưới của mình không được tạo vậy???"
        - Bạn đã liên kết biểu định kiểu CSS chưa?
        - Mở công cụ dành cho nhà phát triển của trình duyệt.
        - Kiểm tra xem console JavaScript có lỗi nào không.
        - Kiểm tra bảng "elements" để xem các phần tử đã xuất hiện nhưng bị ẩn theo cách nào đó hay chưa.
        - Cứ thoải mái thêm các câu lệnh  `console.log` vào JavaScript để xem mã có thực sự được tải hay không.
1. Tạo hiệu ứng "hover" để các div của lưới đổi màu khi chuột đi qua, để lại một đường nét \(pixel\) qua lưới giống như bút vẽ.
    - **Gợi ý**: "Hover" xảy ra khi chuột đi vào một div và kết thúc khi chuột rời khỏi nó. Bạn có thể bắt đầu bằng cách thiết lập trình lắng nghe sự kiện cho một trong hai sự kiện đó.
    - Có nhiều cách để đổi màu các div, chẳng hạn:
        - Thêm một class mới vào div.
        - Dùng JavaScript thay đổi màu nền của div.
1. Thêm một nút ở phía trên màn hình. Nút này sẽ gửi cho người dùng một hộp thoại bật lên để hỏi số ô vuông trên mỗi cạnh của lưới mới. Sau khi nhập, lưới hiện tại phải được *xóa*, rồi một lưới mới được tạo *trong cùng tổng diện tích như trước* \(ví dụ rộng 960px\), để bạn có một bảng vẽ mới.
    - **Mẹo**: Giới hạn giá trị người dùng nhập ở mức tối đa 100. Số ô vuông lớn hơn sẽ sử dụng nhiều tài nguyên máy tính hơn, có thể gây chậm, treo hoặc làm trình duyệt bị lỗi – những điều chúng ta muốn tránh.
    - Hãy tìm hiểu về `prompt`.
    - Bạn phải có thể nhập `64` và thấy một lưới 64x64 hoàn toàn mới xuất hiện mà không thay đổi tổng số pixel được sử dụng.
1. Đẩy dự án của bạn lên GitHub!

#### Điểm cộng thêm

Thay đổi cách một ô vuông hoạt động khi tương tác với chuột bằng cách thực hiện một loạt cải tiến.

1. Thay vì các ô vuông có cùng màu trên toàn bộ lưới, hãy ngẫu nhiên hóa các giá trị RGB của chúng sau mỗi lần tương tác.
1. Ngoài ra, hãy triển khai hiệu ứng tối dần theo đó mỗi lần tương tác sẽ làm ô vuông tối hơn 10%. Mục tiêu là làm một ô vuông trở nên đen hoàn toàn (hoặc có màu hoàn toàn) chỉ sau mười lần tương tác.
    - **Gợi ý**: Thuộc tính CSS opacity hữu ích cho việc này. Để tìm hiểu cách sử dụng, hãy xem [bài viết về thuộc tính opacity trên tài liệu MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity).

Bạn có thể chọn làm một hoặc cả hai thử thách, tùy bạn quyết định.

</div>