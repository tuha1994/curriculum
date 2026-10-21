### Giới thiệu

Được rồi! Hãy thử vận dụng kỹ năng của bạn một chút! Phần này rất đồ sộ và bạn đã học được *rất nhiều* điều. Cách duy nhất để ghi nhớ là thực hành!

Đối với dự án này, bạn sẽ tạo một ứng dụng quản lý hàng tồn kho cho một cửa hàng tưởng tượng. Loại hình kinh doanh này là tùy thuộc vào bạn -- bạn có thể quản lý cửa hàng tạp hóa, phụ tùng ô tô, đồ chơi trẻ em, nhạc cụ, ngựa con hoặc bất cứ thứ gì!

Ứng dụng quản lý hàng tồn kho của bạn phải có các danh mục và mặt hàng để khi người dùng truy cập trang chủ, họ có thể chọn một danh mục để xem, sau đó nhận danh sách mọi mặt hàng trong danh mục đó. Bạn nên triển khai đầy đủ các thao tác CRUD cho cả mặt hàng và danh mục, để bất kỳ ai truy cập trang web cũng có thể tạo, đọc, cập nhật hoặc xóa bất kỳ mặt hàng hay danh mục nào.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Thiết lập dự án Express và cơ sở dữ liệu PostgreSQL mới.
1. Trước khi bắt đầu, hãy dành chút thời gian để viết ra tất cả các bảng cơ sở dữ liệu và các trường của nó mà bạn cần, cũng như mối quan hệ giữa chúng. Ví dụ:
    1. Trong ứng dụng quản lý trò chơi, có thể có trò chơi, thể loại và nhà phát triển. Một trò chơi có thể có một hoặc nhiều nhà phát triển và thể loại. Tương tự, một nhà phát triển có thể phát triển nhiều trò chơi.
    1. Trong ứng dụng quản lý pokemon, có thể có pokemon, người huấn luyện và thực thể loại. Mỗi pokemon phải được chứa trong một loại. Trong khi một huấn luyện viên có thể có nhiều pokemon.

    Bất kỳ ứng dụng quản lý hàng tồn kho phù hợp nào cũng sẽ có các mối quan hệ và ràng buộc giữa các thực thể. Hãy xác định những chi tiết cơ sở dữ liệu này cho ứng dụng của bạn.
1. Thiết lập các tuyến đường và bộ điều khiển bạn sẽ cần.
1. Tạo tất cả các chế độ xem 'ĐỌC' (tức là xem danh mục và xem mục).
1. Tạo tất cả các biểu mẫu và xây dựng bộ điều khiển bạn cần cho các hành động tạo và cập nhật.
1. Tìm ra chức năng xóa. Điều gì xảy ra nếu bạn cố xóa một danh mục có các mục trong đó? Có nên xóa tất cả các mục không? Có nên xóa danh mục khỏi các mục không? Hay cái gì khác? Hành vi cụ thể này sẽ phụ thuộc vào yêu cầu của ứng dụng của bạn.
1. Khi bạn đã tự tin với dự án của mình, hãy thêm dữ liệu giả thông qua tập lệnh vào cơ sở dữ liệu cục bộ của bạn. Làm điều này một lần nữa khi bạn triển khai.
1. Triển khai nó và thể hiện những gì bạn đã làm!

#### Tín dụng bổ sung

1. Hãy làm cho nó đẹp!
1. Chúng ta sẽ tìm hiểu về cách tạo người dùng bằng mật khẩu an toàn trong bài học sau, nhưng hiện tại, chúng ta không muốn *bất kỳ ai* cũng có thể xóa và chỉnh sửa các mục trong kho của mình! Tìm hiểu cách bảo vệ các hành động phá hoại (như xóa và cập nhật) bằng cách yêu cầu người dùng nhập mật khẩu quản trị viên bí mật để xác nhận hành động.

</div>
