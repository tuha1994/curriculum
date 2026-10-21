### Giới thiệu

Dự án này cuối cùng sẽ mang đến cho bạn cơ hội kết nối mọi thứ bạn đã học được cho đến nay. Đây là một dự án có nhiều sự phức tạp nên hãy thực hiện từng bước một. Làm việc với phần phụ trợ khá đơn giản nhưng bạn cũng sẽ phải sử dụng một số chức năng của phần giao diện người dùng. Điều này hoàn toàn nằm trong phạm vi những thứ mà bạn có thể được yêu cầu xây dựng trong công việc (mặc dù tất nhiên là không chính xác). Bạn đã sẵn sàng chưa?

Bạn đã từng chơi [Where's Waldo](http://en.wikipedia.org/wiki/Where's_Wally%3F) chưa? Nếu không thì không sao cả! Hãy để chúng tôi chia nhỏ nó ra: bạn sẽ thấy một hình minh họa bận rộn và đông đúc chứa nhiều người, đồ vật và địa điểm khác nhau. Nhiệm vụ của bạn là tìm một nhân vật cụ thể tên Waldo, người ẩn đâu đó trong hình minh họa.

### Nhiệm vụ của bạn

Xây dựng một ứng dụng mà khi hoàn thành sẽ có cảm giác rất giống với ứng dụng gắn thẻ ảnh. Bạn sẽ bắt đầu với một bức ảnh lớn chứa một số yếu tố mà người dùng muốn tìm, ví dụ: Waldo, The Wizard, Wilma, v.v. Bạn thậm chí có thể đặt tên cho riêng mình nếu muốn sử dụng ảnh tùy chỉnh. Người dùng sẽ đưa ra lựa chọn cho từng ký tự và họ sẽ nhận được phản hồi về việc liệu chúng có đúng hay không.

Để bắt đầu, bạn cần chọn một ảnh và xác định chính xác vị trí mỗi người đang sử dụng vị trí pixel của mình và lưu ảnh đó vào cơ sở dữ liệu của bạn. Khi người dùng nhấp vào ảnh, nó sẽ đặt một hộp nhắm mục tiêu xung quanh phần ảnh mà người dùng đã nhấp vào. Hộp đó phải chứa danh sách các ký tự có thể có.

Khi người dùng chọn một trong các ký tự này, bạn nên kiểm tra với chương trình phụ trợ của mình để xem liệu ký tự đó có thực sự nằm trong hộp nhắm mục tiêu hay không. Cung cấp cho người dùng phản hồi thích hợp (ví dụ: nếu sai, sẽ có thông báo lỗi). Nếu đúng, hãy đặt điểm đánh dấu lên ảnh ở vị trí của nhân vật. Trong cả hai trường hợp, hãy xóa hộp nhắm mục tiêu cho đến khi người dùng nhấp lại.

Theo dõi mất bao lâu kể từ khi ảnh được tải lần đầu tiên cho đến khi người dùng cuối cùng xác định được tất cả các ký tự. Bạn nên thực hiện việc này ở phía máy chủ, nếu không người dùng có thể hack điểm của họ (nhưng bây giờ bạn nên biết điều này). Sau khi hoàn thành một vòng, hãy hỏi tên người dùng và ghi lại thời gian đó. Việc này sẽ hơi khó khăn vì bạn sẽ có những người dùng ẩn danh mà bạn cần theo dõi!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Hãy nghĩ xem bạn cần phải làm gì để tất cả những điều này có thể hoạt động hiệu quả. Đây là lúc việc suy nghĩ kỹ càng trên giấy hoặc bảng trắng trước khi thực hiện sẽ thực sự hữu ích! Một vài phút suy nghĩ có thể giúp bạn tránh lãng phí một giờ vào việc viết mã.
1. Xây dựng chức năng giao diện người dùng mà chưa thực sự sử dụng bất kỳ lệnh gọi nào tới mặt sau. Cụ thể, tạo chức năng bật hộp nhắm mục tiêu và menu thả xuống trên màn hình khi người dùng nhấp vào ảnh và xóa ảnh đó khi người dùng nhấp đi.
1. Bây giờ hãy kết nối chức năng xác thực với phần phụ trợ của bạn xem người dùng có nhấp vào đúng vị trí cho ký tự họ đã chọn từ danh sách thả xuống hay không.
** Lưu ý:**Tùy thuộc vào cách bạn lấy tọa độ nhấp chuột của người dùng, các kích thước màn hình khác nhau có thể tạo ra tọa độ khác nhau. Điều này có thể khiến ứng dụng của bạn ghi lại tọa độ chính xác trên màn hình lớn chứ không phải màn hình nhỏ hơn. Biết được điều này, bạn có thể cần phải triển khai các phương pháp cho logic nhấp chuột của mình để chuẩn hóa tọa độ trên các kích thước màn hình khác nhau.
1. Gắn nó vào giao diện người dùng của bạn để bạn có thể chọn các ký tự, xác thực chúng một cách liền mạch và đặt các điểm đánh dấu thích hợp trên bản đồ nếu lựa chọn đúng.
1. Thêm khả năng tính thời gian cho người dùng từ khi họ tải trang lần đầu tiên và sau đó hiển thị "điểm" (thời gian) của họ khi họ xác định thành công tất cả các ký tự. Tạo một cửa sổ bật lên yêu cầu họ nhập tên vào bảng điểm cao nếu họ đã đạt được.
1. Chơi với nó!
1. Đẩy giải pháp của bạn lên GitHub và triển khai nó trên bất kỳ tùy chọn lưu trữ nào. Ngoài ra, hãy gửi giải pháp của bạn bên dưới. Đây là một dự án nghiêm túc, xin chúc mừng!

#### Tín dụng bổ sung

- Tải nhiều hình ảnh vào cơ sở dữ liệu của bạn và cho phép người dùng lựa chọn trong số đó trước khi bắt đầu trò chơi.

</div>
