### Giới thiệu

Project này cho bạn cơ hội vận dụng những kiến thức mới đã học trong vài bài vừa qua. Lần này, bạn sẽ xây dựng form đăng ký cho một dịch vụ tưởng tượng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Bước 1: Thiết lập và lập kế hoạch

1. Thiết lập repository git (tham khảo các project trước nếu cần ôn lại).
1. Thiết lập các file HTML và CSS với một ít nội dung mẫu để đảm bảo mọi thứ đã được liên kết đúng.
1. Tải bản sao đầy đủ độ phân giải của [file thiết kế](https://cdn.statically.io/gh/TheOdinProject/curriculum/afdbabfab03fbc34783c6b6f3920aba4a4d3b935/intermediate_html_css/forms/project_sign_up_form/imgs/sign-up-form.png), và hình dung tổng quát cách bạn cần bố trí các thành phần trong tài liệu HTML.

#### Bước 2: Thu thập asset

1. Thiết kế có một background-image lớn, vì vậy hãy tìm và tải một hình ảnh bạn muốn dùng cho phần đó. Hình ảnh trong thiết kế có thể tìm thấy trên [unsplash.com](https://unsplash.com/photos/25xggax4bSA), nhưng bạn hoàn toàn có thể chọn hình ảnh riêng. Hãy nhớ ghi công tác giả của hình ảnh!
1. Chọn một font bên ngoài cho phần 'logo'. Chúng tôi đã dùng [Norse Bold](https://www.joelcarrouche.com/fonts/norse), nhưng bạn có thể dùng bất kỳ font nào mình thích.
1. Ở image-sidebar, chúng tôi đã dùng [logo Odin](https://cdn.statically.io/gh/TheOdinProject/curriculum/5f37d43908ef92499e95a9b90fc3cc291a95014c/html_css/project-sign-up-form/odin-lined.png) này.

#### Bước 3: Một số mẹo!

1. Cách bạn thực hiện project này chủ yếu tùy thuộc vào bạn, nhưng nên bắt đầu bằng việc dựng khung cấu trúc trang, rồi xử lý từng phần một.
1. Khu vực phía sau logo "ODIN" là một div có màu nền tối nhưng bán trong suốt. Điều này giúp văn bản dễ đọc hơn trên hình nền nhiều chi tiết.
1. Màu chúng tôi chọn cho nút 'Create Account' tương tự các tông màu trong hình nền. Cụ thể là `#596D48`.
1. Theo mặc định, các input có viền rất nhạt (`#E5E7EB`), nhưng chúng tôi đưa vào 2 biến thể. Trước hết, input mật khẩu nên có viền đỏ nếu chứa mật khẩu không hợp lệ. Bạn có thể xử lý việc này bằng pseudo-class `:user-invalid` đã học ở bài trước.
1. Biến thể còn lại là input được chọn, input này nên có viền xanh và box-shadow nhẹ. Có thể thực hiện bằng pseudo-class `:focus` đã học trong một bài trước.
1. Đừng lo làm cho project trông đẹp trên thiết bị di động. Responsive design sẽ được học ở phần sau của curriculum.
1. Việc xác thực các trường mật khẩu khớp nhau cần JavaScript. Cách dùng JavaScript để xác thực form sẽ được học trong một bài sau. Hiện tại, chỉ cần xác thực từng trường riêng lẻ.

</div>
