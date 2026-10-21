### Giới thiệu

Chúng tôi đang tạo một trò chơi Tic Tac Toe mà bạn có thể chơi trên trình duyệt của mình!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Thiết lập dự án của bạn với các tệp HTML, CSS và JavaScript và thiết lập tất cả repo Git.
1. Bạn sẽ lưu trữ bảng trò chơi dưới dạng một mảng trong Gameboard đối tượng, vì vậy hãy bắt đầu từ đó! Người chơi của bạn cũng sẽ được lưu trữ trong các đối tượng và bạn có thể muốn có một đối tượng để tự động kiểm soát biến thể trò chơi của trò chơi.
   1. Mục tiêu chính của bạn ở đây là càng ít mã toàn cầu càng tốt. Hãy thử tăng càng nhiều đồ càng tốt vào nhà máy. Nếu bạn chỉ cần một phiên bản duy nhất của một thứ gì đó (ví dụ: gameboard, displayController, v.v.), thì hãy sử dụng phiên bản bọc máy bên trong IIFE (mẫu mô-đun) để không thể sử dụng lại để tạo các bản bổ sung.
   1. Trong dự án này, hãy suy nghĩ cẩn thận về vị trí logic từng chút một. Mỗi phần chức năng nhỏ phải có thể phù hợp với các đối tượng trong trò chơi, người chơi hoặc bảng trò chơi. Hãy cẩn thận để đặt chúng ở những nơi “hợp lý”. Dành một chút thời gian động không ở đây có thể giúp cuộc sống của bạn sau này dễ dàng hơn nhiều!
   1. Nếu bạn gặp khó khăn, [Xây nhà từ trong ra ngoài](https://www.ayweb.dev/blog/building-a-house-from-the-inside-out) là một bài viết tuyệt vời đưa ra một ví dụ có tính ứng dụng cao về cách bạn có thể tiếp cận việc giải quyết dự án này cũng như cách bạn có thể tổ chức và cấu hình mã hóa cấu trúc của mình.
1. Trước tiên hãy sử dụng tập tin để đưa một trò chơi hoạt động vào bảng điều khiển. Hãy đảm bảo bạn bao gồm logic để kiểm tra xem trò chơi nào kết thúc! Bạn nên kiểm tra tất cả các trận thắng 3 liên tiếp và các trận hòa. Cố tránh nghĩ về DOM và HTML/CSS của bạn khi lựa chọn khi trò chơi của bạn đang hoạt động. Đừng lo lắng về việc lấy đầu vào thông tin của người dùng vào thời điểm này. Bạn có thể gọi các hàm của mình và chuyển đối số cho chúng để tự mình chơi trò chơi và kiểm tra xem mọi hoạt động thứ yếu như dự kiến ​​​​hay không.
1. Sau khi bạn đang chơi trò chơi trên bảng điều khiển, hãy tạo một đối tượng sẽ xử lý logic display/DOM. Viết một hàm sẽ hiển thị nội dung của mảng trò chơi lên trang web (hiện tại, bạn luôn có thể điền vào mảng trò chơi bằng `"X"` và `"O"` để xem điều gì đang diễn ra).
1. Viết các chức năng cho phép người chơi thêm điểm vào một vị trí công cụ trên bảng bằng cách tương tác với các phần tử thích hợp DOM (ví dụ: cho phép người chơi nhấp vào ô vuông trên bảng để đặt dấu đánh dấu của họ). Đừng quên logic ngăn chặn người chơi ở những vị trí đã có sẵn!
1. Giao diện thu gọn hơn để cho phép người chơi nhập tên, bao gồm nút bắt đầu/khởi động lại trò chơi và bổ sung các thành phần hiển thị kết quả khi kết thúc trò chơi!

</div>
