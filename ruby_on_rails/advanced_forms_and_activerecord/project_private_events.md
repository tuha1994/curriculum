### Giới thiệu

Trong các dự án này, cuối cùng bạn sẽ có dịp tìm hiểu sâu về các association của ActiveRecord, một trong những tính năng tuyệt vời nhất mà nó cung cấp.

### Khởi động: ưu tiên suy nghĩ về dữ liệu

Hãy phác thảo kiến trúc dữ liệu mà bạn cần triển khai để xây dựng các kịch bản sau:

1. Một trang web trông giữ thú cưng (chăm sóc thú cưng của người khác khi họ vắng nhà). Mỗi người có thể trông nhiều thú cưng và mỗi thú cưng có thể có nhiều người trông.
1. Bạn thích mời mọi người đến ăn tối, nên muốn xây dựng một trang web mời dự tiệc tối. Một người dùng có thể tạo các bữa tiệc, mời mọi người đến một bữa tiệc và chấp nhận lời mời đến bữa tiệc của người khác.
1. Bài tập thêm (khó): Bạn và bạn bè rất thích đăng bài và theo dõi nhau. Bạn sẽ thiết lập các model thế nào để một người dùng có thể theo dõi một người dùng khác?

### Dự án: Sự kiện riêng tư

Bạn muốn xây dựng một trang web tương tự như [Eventbrite](http://www.eventbrite.com) phiên bản riêng tư, cho phép người dùng tạo sự kiện rồi quản lý việc đăng ký tham dự.

Một người dùng có thể tạo các sự kiện. Một người dùng có thể tham dự nhiều sự kiện. Một sự kiện có thể có nhiều người dùng tham dự. Các sự kiện diễn ra vào một ngày cụ thể và tại một địa điểm (bạn chỉ cần lưu địa điểm dưới dạng chuỗi, chẳng hạn như "Andy's House").

Dự án này sẽ yêu cầu bạn mô hình hóa các mối quan hệ nhiều-nhiều, đồng thời phải đặc biệt chú ý đến các khóa ngoại và tên class (gợi ý: bạn sẽ không thể chỉ dựa vào các giá trị mặc định của Rails như trước đây).

### Bài tập

Đến đây chúng ta đã đi khá xa, vì vậy các nhiệm vụ này chỉ trình bày tổng quan ở mức cao về những gì bạn cần xây dựng. Bạn sẽ phải tự triển khai các chi tiết. Như thường lệ, sản phẩm không cần trông đẹp mắt, chỉ cần hoạt động. Phần thiết kế là nội dung để làm thêm.

<div class="lesson-content__panel" markdown="1">

#### Thiết lập

1. Suy nghĩ và đặc tả cách thiết lập các model dữ liệu cho ứng dụng này.
1. Tạo một ứng dụng Rails `private-events` mới và một repo GitHub.
1. Cập nhật README để mô tả rõ ràng và liên kết đến dự án này.

#### Sự kiện và người dùng

1. Xây dựng và migrate model Event mà chưa thêm khóa ngoại hay validation nào. Bao gồm ngày của sự kiện trong model, nhưng chưa cần làm gì đặc biệt với trường này.
1. Tạo EventsController và thêm action `#index` để hiển thị tất cả sự kiện. Tạo view tương ứng và thêm một tiêu đề với nội dung tùy bạn chọn.
1. Thiết lập [devise](https://github.com/heartcombo/devise) để xử lý việc xác thực và tạo model User. Đặt `root_path` trỏ đến trang Index của Event.
1. Thêm association giữa người tạo sự kiện (một User) và sự kiện. Gọi user này là "creator". Thêm khóa ngoại vào model Event khi cần. Bạn sẽ cần chỉ định cẩn thận các thuộc tính của association (ví dụ: `:foreign_key`, `:class_name`).
1. Hiển thị trong trang Show của User tất cả sự kiện mà một user đã tạo.
1. Cập nhật EventsController và các route tương ứng để cho phép bạn tạo một sự kiện mới. Action `#create` phải sử dụng phương thức tham chiếu association `#build` để tạo sự kiện mới với ID của user được điền sẵn. Hãy tìm đúng [phương thức tham chiếu association](https://guides.rubyonrails.org/v7.1/association_basics.html#detailed-association-reference) `#build` cho loại association bạn đã thiết lập giữa các model. Bạn có thể sử dụng phương thức `::new` của Event rồi nhập ID thủ công, nhưng... đừng làm vậy.
1. Tạo form để tạo một sự kiện.
1. Hiển thị thông tin chi tiết của sự kiện trong trang Show của Event.

#### Việc tham dự sự kiện

1. Thêm association giữa người tham dự sự kiện (cũng là một User) và sự kiện. Gọi user này là "attendee". Gọi sự kiện là "attended_event". Một lần nữa, bạn sẽ phải xử lý các khóa ngoại, class và source được đặt tên riêng.
1. Tạo và migrate tất cả bảng cùng khóa ngoại cần thiết. Việc này sẽ yêu cầu một bảng "through", vì một Event có thể có nhiều Attendee và một User (Attendee) có thể tham dự nhiều Event... quan hệ nhiều-nhiều.
1. Tạo một Controller và các route tương ứng cho bảng "through", cho phép một user trở thành "attendee" của một sự kiện. Bạn cũng sẽ cần tạo một dạng giao diện nào đó trong các view để user có thể cho biết họ muốn tham dự một sự kiện.
1. Cập nhật trang Show của Event để hiển thị danh sách người tham dự.
1. Thêm danh sách các "attended_events" của user vào trang Show của User.
1. Chia danh sách "attended_events" này thành các sự kiện đã diễn ra trong quá khứ và các sự kiện sẽ diễn ra trong tương lai. Bạn sẽ có cơ hội thực hành tốt việc [xây dựng các truy vấn](https://guides.rubyonrails.org/active_record_querying.html#array-conditions) và làm việc với ngày tháng. Giữ logic này trong view và không đặt các lời gọi phương thức riêng biệt trong controller.

#### Hoàn thiện

1. Chia các sự kiện đã qua và sắp tới trên trang Index của Event bằng cách tạo hai phương thức class trong model Event (ví dụ: `Event.past`).
1. Refactor các phương thức class dành cho sự kiện đã qua và sắp tới thành các [scope](https://guides.rubyonrails.org/active_record_querying.html#scopes).
1. Đặt các liên kết điều hướng ở phía trên để giúp bạn chuyển qua lại giữa các trang.
1. Đặt mỗi sự kiện ở chế độ riêng tư và thêm chức năng cho phép người tạo sự kiện mời những user cụ thể tham dự.
1. Bài tập thêm: Cho phép user chỉnh sửa và xóa những sự kiện mà họ đã tạo.
1. Bài tập thêm: Cho phép user tự hủy tư cách người tham dự khỏi các "attended_events" của mình.
1. Bài tập thêm: Cho phép người tạo chuyển đổi chế độ hiển thị của sự kiện giữa công khai và riêng tư.

</div>
