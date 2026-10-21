### Giới thiệu

Vui lòng mở rộng ví dụ về 'Sách' từ bài học trước và biến nó thành một Thư viện nhỏ ứng dụng.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Nếu chưa có, hãy thiết lập kho lưu trữ Git cho dự án của bạn với các tệp HTML/CSS và JS. Từ đây trở đi, chúng tôi sẽ chọn rằng bạn đã thực hiện công việc này.
1. Tất cả các đối tượng của bạn sẽ được lưu trữ trong một mảng, vì vậy bạn cần có một hàm tạo cho danh sách. Sau đó, việc thêm một hàm riêng vào lệnh (không phải trong hàm tạo) có thể nhận một số đối số, tạo một cuốn sách từ các số đó và lưu trữ cuốn sách đối tượng mới vào một mảng. Ngoài ra, tất cả các đối tượng của bạn phải có `id` duy nhất, có thể được tạo bằng `crypto.randomUUID()`. Điều này đảm bảo mỗi cuốn sách có một mã nhận dạng duy nhất và ổn định, vô ích khi sách bị xóa hoặc sắp xếp lại. Mã của bạn sẽ trông giống như thế này (chúng tôi chỉ hiển thị khung cơ sở không có tham số hàm):

   ```javascript
   const myLibrary = [];

   function Book() {
     // the constructor...
   }

   function addBookToLibrary() {
     // take params, create a book then store it in the array
   }
   ```

1. Viết hàm lặp qua mảng và hiển thị từng cuốn sách trên trang. Bạn có thể hiển thị chúng trong một số loại bảng hoặc mỗi loại trên "tên" riêng của chúng. Hiện tại, việc thêm một vài cuốn sách vào mảng của bạn theo cách thủ công để bạn có thể xem màn hình hữu ích.
   - Mặc dù việc làm hoạt động trực tiếp việc hiển thị danh sách có vẻ dễ dàng hơn thay vì lưu trữ dữ liệu trong một mảng trước đó, nhưng từ đây trở đi, bạn nên nghĩ đến những trách nhiệm này một cách riêng biệt. Chúng tôi sẽ tìm hiểu sâu hơn về khái niệm này sau, nhưng khi phát triển ứng dụng, chúng tôi mong muốn có được hoạt động linh hoạt trong việc tạo lại các thành phần (như thư viện của chúng tôi và các danh sách trong đó) theo nhiều cách khác nhau bằng cách sử dụng cùng một cơ sở dữ liệu. Do đó, vui lòng xem xét logic hiển thị danh sách cho người dùng và cấu trúc cấu hình chứa tất cả thông tin bên dưới các loại thực tế riêng biệt. Điều này đặc biệt sẽ nâng cao khả năng bảo trì và khả năng mở rộng mã hóa của bạn.
1. Thêm nút "Sách mới" để hiển thị một biểu mẫu cho phép người dùng nhập thông tin chi tiết về cuốn sách mới và thêm nó vào thư viện: tác giả, tên sách, số trang, cuốn sách chưa được đọc và bất kỳ điều gì khác mà bạn muốn. Cách bạn quyết định hiển thị biểu tượng tùy chọn này thuộc về bạn. Ví dụ: bạn có thể muốn hiển thị mẫu trong thanh bên hoặc bạn có thể muốn khám phá [hộp thoại và phương thức](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog) bằng cách sử dụng thẻ `<dialog>`. Dù bạn làm điều này như thế nào, rất có thể bạn sẽ gặp phải sự cố trong đó việc gửi biểu mẫu của bạn không mang lại kết quả như bạn mong đợi. Đó là đầu vào `submit` cố gắng gửi dữ liệu đến máy chủ theo mặc định. Đây là lúc `event.preventDefault();` sẽ phát huy tác dụng. Vui lòng xem [tài liệu về sự kiện.preventDefault](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault) và xem bạn có thể giải quyết vấn đề này như thế nào!
1. Thêm nút trên màn hình của mỗi cuốn sách để xóa danh sách khỏi thư viện.
   - Bạn sẽ cần liên kết các phần tử DOM của mình với các đối tượng thực tế theo bất kỳ cách nào. Một giải pháp dễ dàng được cung cấp cho chúng một [thuộc tính dữ liệu](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Solve_HTML_problems/Use_data_attributes) tương ứng với `id` duy nhất của đối tượng tương ứng.
1. Thêm một nút trên màn hình của mỗi cuốn sách để thay đổi trạng thái `read`.
   - Để tạo điều kiện thuận lợi cho công việc này, bạn sẽ muốn tạo mẫu hàm `Book` để chuyển đổi trạng thái `read` của phiên bản sách.

<div class="lesson-note" markdown="1">

#### Không cần thiết phải lưu trữ liên tục

Bạn không cần phải thêm bất kỳ loại bộ nhớ nào để lưu thông tin giữa các lần tải lại trang.

</div>

</div>
