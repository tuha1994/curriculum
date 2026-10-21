### Giới thiệu

Tại thời điểm này, bạn đã thực hiện nhiều hành động khác bằng cách sử dụng các kỹ thuật khác nhau mà chúng tôi đã chỉ cho bạn. Nhưng chúng tôi đã cung cấp một lượng thông tin *lot* theo cách của bạn, vì vậy trước khi tiếp tục, chúng tôi sẽ dành một phút để làm chậm và thực hiện một dự án tuyệt vời khác để gắn kết các kỹ thuật này với nhau. Bạn sẽ có cơ hội hiện hầu hết (nếu không phải tất cả) các kỹ năng mới học được của mình!

### Danh sách công việc cần làm

Danh sách công việc cần làm là yếu tố chính trong các hướng dẫn dành cho nhà phát triển web mới bắt đầu, việc phát triển khai có thể rất cơ bản. Tuy nhiên, vẫn còn rất nhiều chỗ cần cải tiến và nhiều tính năng có thể được bổ sung vào.

Trước khi đi sâu vào mã hóa, hãy dành một phút để suy nghĩ về cách bạn muốn tổ chức dự án của mình.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. 'Những công việc cần làm' của bạn sẽ là đối tượng mà bạn muốn tạo động, có nghĩa là sử dụng nhà hoặc chức năng tạo/lớp để tạo ra chúng.
1. Hãy suy nghĩ về loại thuộc tính mà công việc bạn cần làm sẽ có. Tối thiểu phải có `title`, `description`, `dueDate` và `priority`.  Bạn cũng có thể muốn bao gồm `notes` hoặc thậm chí `checklist`.
1. Danh sách công việc bạn cần làm phải có `projects` hoặc danh sách `todos` riêng.  Khi người dùng mở ứng dụng lần đầu tiên, sẽ có một số loại dự án 'mặc định' mà tất cả công việc cần làm của họ sẽ được đưa vào.  Người dùng có thể tạo dự án mới và chọn dự án mà họ sẽ thực hiện.
1. Bạn nên tách ứng dụng logic của mình (tức là tạo công việc cần làm mới, đặt công việc cần làm là hoàn thành, thay đổi mức độ ưu tiên của công việc cần làm, v.v.) khỏi nội dung liên kết đến DOM, vì vậy hãy đặc biệt giữ tất cả những thứ đó trong các mô-đun riêng biệt.
1. Giao diện của người dùng giao diện tùy thuộc vào bạn nhưng nó không thể thực hiện những công việc sau:
   1. Xem tất cả các dự án.
   1. Xem tất cả công việc cần thực hiện trong mỗi dự án (có thể chỉ là tiêu đề và ngày hết hạn... có thể thay đổi màu sắc cho các mức độ ưu tiên khác nhau).
   1. Mở rộng một công việc cần làm để xem/chỉnh sửa chi tiết của nó.
   1. Xóa một công việc cần làm.
1. Để có cảm hứng, hãy xem các ứng dụng cần làm tuyệt vời sau đây. (xem ảnh chụp màn hình, xem video giới thiệu của họ, v.v.)
   1. [Todoist](https://en.todoist.com/)
   1. [Thành phần](https://culturedcode.com/things/)
   1. [any.do](https://www.any.do/)
1. Vì bạn đã sử dụng webpack nên việc bổ sung các thư viện bên ngoài từ npm là điều chắc chắn!  Bạn có thể cân nhắc việc sử dụng thư viện hữu ích sau trong mã của mình:
   1. [date-fns](https://github.com/date-fns/date-fns) cung cấp cho bạn một loạt tiện ích để định dạng và thao tác ngày và giờ.
1. Chúng tôi chưa học được bất kỳ kỹ thuật nào để thực hiện lưu trữ dữ liệu của mình ở bất kỳ đâu, vì vậy khi người dùng làm trang mới, tất cả công việc cần làm của họ sẽ biến mất! Bạn nên bổ sung thêm một số tính năng bền bỉ cho ứng dụng cần làm này bằng cách sử dụng Web Storage API.

[localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API) cho phép bạn lưu trữ dữ liệu trên máy tính của người dùng. Nhược điểm ở đây là CHỈ dữ liệu có thể truy cập được trên máy tính nhưng nó không được tạo trên đó. Dù vậy, nó vẫn tiện ích! Thiết lập chức năng lưu dự án (và công việc cần làm) vào localStorage mỗi khi dự án mới (hoặc công việc cần làm) được tạo và một chức năng tìm kiếm dữ liệu khác trong localStorage khi ứng dụng của bạn được tải xuống lần đầu tiên. Ngoài ra, đây là một số Mẹo nhanh chóng giúp bạn không gặp rắc rối:
   - Đảm bảo ứng dụng của bạn không gặp sự cố nếu dữ liệu bạn muốn truy cập từ localStorage không có ở đó!
   - Bạn có thể kiểm tra dữ liệu bạn đã lưu trong localStorage bằng DevTools! Để thực hiện công việc này, hãy mở tab `Application` trong DevTools và nhấp vào tab `Local Storage` trong `Storage`. Mỗi khi bạn bổ sung, cập nhật và xóa dữ liệu khỏi localStorage trong ứng dụng của mình, những thay đổi đó sẽ được phản ánh trong DevTools.
   - localStorage use [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) để gửi và lưu trữ dữ liệu và khi bạn truy cập dữ liệu, dữ liệu đó cũng sẽ ở định dạng JSON. Hãy nhớ rằng *không thể lưu trữ các hàm trong JSON*, vì vậy bạn sẽ phải tìm cách bổ sung các phương thức trả lại thuộc tính đối tượng thuộc tính của mình sau khi tải chúng. Chúc may mắn!

</div>
