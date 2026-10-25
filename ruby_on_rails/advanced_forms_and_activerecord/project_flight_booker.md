### Giới thiệu

Trong dự án này, bạn sẽ có cơ hội xử lý một số form nâng cao hơn. Đây là dạng công việc bạn sẽ phải làm khi xử lý đơn hàng của người dùng cho bất kỳ thứ gì phức tạp hơn một cuốn e-book. Trong trường hợp này, chúng ta sẽ xây dựng ba bước đầu tiên của một quy trình thanh toán điển hình để đặt một chuyến bay một chiều:

Quy trình đặt vé máy bay điển hình:

1. Nhập ngày tháng / sân bay mong muốn và nhấp "Tìm kiếm"
1. Chọn từ danh sách các chuyến bay hiện có
1. Nhập thông tin hành khách cho tất cả hành khách
1. Nhập thông tin thanh toán

Bước 4 sẽ được thực hiện bằng cách tích hợp với một dịch vụ như [API Paypal](http://coding.smashingmagazine.com/2011/09/05/getting-started-with-the-paypal-api/), thông qua [gem paypal-express](https://github.com/nov/paypal-express), [SDK paypal-sdk-merchant](https://www.tommyblue.it/2013/07/02/paypal-express-checkout-with-ruby-on-rails-and-paypal-sdk-merchant/), hoặc [Stripe](https://docs.stripe.com/checkout/quickstart).

### Bài tập

Chúng ta sẽ tạo một ứng dụng đặt vé máy bay một chiều. Bạn sẽ được thực hành rất nhiều việc điền dữ liệu và xây dựng menu dropdown, nút radio và các lần gửi form lồng nhau. Hãy bắt đầu!

Dự án này yêu cầu bạn seed cơ sở dữ liệu, vì vậy hãy dùng file `db/seeds.rb` để viết code cần thiết nhằm tạo dữ liệu cho Airports và Flights. Một mẹo dành cho các ứng dụng thử nghiệm như thế này (đừng làm vậy trong môi trường production!) là dùng task `db:seed:replant` để đồng thời xóa cơ sở dữ liệu và chạy file seed. Nhờ đó, khi tạo model mới hoặc thay đổi cách thiết lập dữ liệu mẫu, bạn chỉ cần cập nhật file `seeds.rb` rồi chạy `$ rake db:seed:replant` để có một bộ dữ liệu sạch.

<div class="lesson-content__panel" markdown="1">

#### Thiết lập

1. Suy nghĩ về kiến trúc dữ liệu cần thiết để biến ý tưởng này thành hiện thực.
1. Tạo một ứng dụng Rails mới, ví dụ `odin-flight-booker`, và một Git repo mới
1. Chỉnh sửa README để giải thích bạn đang làm gì và liên kết ngược về dự án này.

#### Màn hình 1: tìm kiếm

Ở màn hình đầu tiên, bạn sẽ tạo 4 menu dropdown cho phép người dùng tìm các chuyến bay hiện có — sân bay khởi hành, sân bay đến, ngày và số hành khách. Đây là những form tương đối cơ bản, chỉ khác là chúng được điền sẵn bằng các tập hợp dữ liệu. Làm việc với ngày tháng sẽ giúp bạn tiếp cận thêm một chút kiến thức mới.

1. Tạo model Airport (về cơ bản chỉ cần một mã sân bay như "SFO" hoặc "NYC") và dùng file `db/seeds.rb` để tạo một số sân bay.
1. Tạo model Flight (với `id` của sân bay khởi hành và sân bay đến, datetime bắt đầu và thời lượng chuyến bay).
1. Thiết lập association để bạn có thể gọi `Flight.first.departure_airport`, `Flight.first.arrival_airport` và nhận lại các đối tượng Airport. Tương tự với `Airport.first.departing_flights` và `Airport.first.arriving_flights`, các method này phải trả về một danh sách các đối tượng Flight.
1. Seed cơ sở dữ liệu với các chuyến bay.
1. Bạn sẽ tìm kiếm và xem kết quả trên cùng một trang. Thiết lập FlightsController và routes để trang Index (`/flights`) là root route.
1. Tạo form tìm kiếm trên trang index `/flights`, cho phép gửi bằng request GET (không phải POST) về cùng URL.
1. Thêm bốn menu dropdown — danh sách sân bay khởi hành, sân bay đến, số hành khách (1-4) và dropdown ngày cho ngày bay. Dropdown ngày chỉ được bao gồm những ngày có chuyến bay hiện có. Đừng lo về việc hạn chế nội dung của các dropdown sân bay — thông thường bạn sẽ dùng JavaScript — nên hiện tại chỉ cần cho phép người dùng chọn bất kỳ sân bay nào. Xem [bài đăng Stack Overflow này về định dạng ngày trong danh sách dropdown](http://stackoverflow.com/questions/15720940/rails-format-date-in-drop-down-list-in-view), [bài đăng Stack Overflow ngắn này về chọn tháng/năm](http://stackoverflow.com/questions/13001904/rails-drop-down-select-month-year) và [tài liệu Rails DateHelper API](http://api.rubyonrails.org/classes/ActionView/Helpers/DateHelper.html) để được trợ giúp tạo các select ngày.

#### Màn hình 2: chọn chuyến bay

Sau khi kết quả tìm kiếm được trả về, người dùng chỉ cần chọn một trong các chuyến bay hiện có.

1. Sau khi người dùng gửi form, controller của bạn phải phát hiện các tham số truy vấn bổ sung (ví dụ từ `/flights?departure_code=SFO&arrival_code=NYC&date=20131215&num_tickets=2`) và truy vấn thông minh các chuyến bay phù hợp với tiêu chí đó từ cơ sở dữ liệu thông qua các method của model.
1. Gửi các chuyến bay đó về view `app/views/flights/index.html.erb`. Hãy để view hiển thị kết quả bên dưới (nếu có).
1. Đừng xóa các dropdown tìm kiếm — giữ form tìm kiếm hoạt động ở đầu trang để người dùng có thể thử một tìm kiếm mới.
1. Kết quả tìm kiếm cần nằm trong form riêng — lần này để chọn chuyến bay mà người dùng muốn chọn. Mỗi chuyến bay trả về phải được hiển thị kèm một nút radio. Người dùng sẽ chọn chuyến bay rồi gửi để chuyển sang bước tiếp theo. Form phải gửi đến action `#new` của BookingsController mà bạn sắp tạo, sử dụng một method GET khác. Bạn sẽ cần thêm một hidden field chứa số hành khách.

#### Màn hình 3: thông tin hành khách

Sau khi người dùng gửi chuyến bay đã chọn, đã đến lúc lấy thông tin đặt chỗ. Bạn sẽ cần một form để gửi Booking và đồng thời tạo một đối tượng Passenger cho mỗi hành khách cần vé. Đừng lo về việc tạo một đối tượng "Ticket" riêng; chúng ta sẽ giả định hãng hàng không sẽ cấp các vé đó sau khi booking được xác minh.

1. Tạo và migrate model Booking.
1. Tạo và migrate model Passenger (chỉ cần giữ thông tin đơn giản — chỉ tên và email).
1. Thiết lập association giữa Bookings, Passengers và Flights.
1. Tạo BookingsController và các routes phù hợp.
1. Thiết lập action `#new`, action này sẽ nhận các tham số ID chuyến bay và số hành khách, rồi dùng chúng để hỗ trợ render form cho một booking mới; form hiển thị ngày, các sân bay, ID chuyến bay đã chọn và một nhóm trường để nhập thông tin cá nhân cho từng hành khách. Bạn sẽ muốn tạo một đối tượng Passenger trống mới trong controller cho mỗi hành khách, sau đó dùng `#fields_for` trong view để thiết lập các form con.
1. Thử gửi form và xem các tham số trong server.
1. Action `#create` của bạn cần tạo một Booking mới (liên kết nó với Flight phù hợp), nhưng cũng phải chấp nhận các nested attributes cho từng đối tượng Passenger và tạo Passenger mới từ chúng. Hãy chú ý cho phép các nested parameter.
1. Mở model Booking và triển khai method `#accepts_nested_attributes_for`. Xem [Rails API](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html) để xem các ví dụ và [bài đăng Stack Overflow này về nested attributes và mass assignment](http://stackoverflow.com/questions/18540679/rails-4-accepts-nested-attributes-for-and-mass-assignment) để tham khảo thêm.
1. Sau khi form được gửi thành công, hãy render trang `#show` của booking, trang này hiển thị thông tin booking (thông tin chuyến bay và hành khách).
1. Bảo đảm quy trình cấp vé hoạt động đúng. Làm tốt lắm!

</div>