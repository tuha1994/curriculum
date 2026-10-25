### Giới thiệu

Trong các project này, bạn sẽ triển khai các hệ thống xác thực để user chỉ có thể truy cập những khu vực của một site mà họ được phép truy cập.

### Project: Chỉ dành cho thành viên!

Trong project này, bạn sẽ xây dựng một clubhouse độc quyền, nơi các thành viên có thể viết những bài đăng ẩn danh. Bên trong clubhouse, thành viên có thể thấy tác giả của một bài đăng là ai, nhưng ở bên ngoài, họ chỉ thấy câu chuyện và phải tự hỏi ai đã viết nó.

Nếu muốn thêm những nét riêng về mặt phong cách, hãy xem đó là phần điểm cộng.

### Bài tập

Các project sẽ ngày càng ít hướng dẫn cụ thể về cách đạt được mục tiêu, vì chúng tôi mong bạn vận dụng kiến thức đã học trước đó. Nếu không biết làm một việc gì, bạn có thể xem lại các bài học hoặc project trước, hoặc Google cách triển khai đúng (nhưng hãy cẩn thận, vì việc đó có thể đưa bạn đi sâu hơn mức chúng tôi dự định).

Nếu muốn thử thách bản thân, đừng làm theo các bước bên dưới; hãy tự bắt tay xây dựng app!

<div class="lesson-content__panel" markdown="1">

#### Thiết lập cơ bản

1. Suy nghĩ và phác thảo cách thiết lập các data model cho ứng dụng này. Bạn sẽ cần user với các thuộc tính nhận diện thông thường như name, email và password. Họ cũng cần tạo các post.
1. Tạo app Rails `members-only` mới và repo GitHub tương ứng. Cập nhật README.
1. Thêm devise vào Gemfile và cài đặt nó trong app bằng hướng dẫn thiết lập trên [README của Devise](https://github.com/heartcombo/devise).

#### Xác thực và post

Hãy xây dựng những bí mật đó! Chúng ta cần đảm bảo chỉ user đã đăng nhập mới có thể thấy tác giả của mỗi post. Chúng ta sẽ không cần quan tâm đến việc chỉnh sửa hay xóa post.

1. Tạo một model Post, một controller Posts và resource tương ứng trong tệp Routes, cho phép các method `[:new, :create, :index]`.
1. Ở đầu Posts Controller, dùng một `#before_action` để giới hạn quyền truy cập vào các method `#new` và `#create`, chỉ cho phép user đã đăng nhập.
1. Trong Posts Controller, chuẩn bị action `#new`.
1. Viết một form trong view `app/views/posts/new.html.erb` để tạo một Post mới.
1. Trong action `#create` tương ứng, hãy xây dựng một post sao cho foreign key của tác giả (ví dụ `user_id`) tự động được điền dựa trên user đang đăng nhập. Nếu thành công, redirect đến view Index.
1. Hoàn thiện action `#index` của PostsController và tạo view tương ứng. View này phải hiển thị danh sách mọi post.
1. Bây giờ thêm logic vào view Index để hiển thị tên tác giả, nhưng chỉ khi có user đang đăng nhập.
1. Đăng nhập và tạo một vài post bí mật.
1. Kiểm tra thử -- đăng xuất rồi truy cập trang index. Bạn sẽ thấy danh sách các post nhưng không có tên tác giả. Đăng nhập, tên tác giả sẽ xuất hiện. Bí mật của bạn đã an toàn!

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [Railscasts #250 Xác thực từ đầu](http://railscasts.com/episodes/250-authentication-from-scratch-revised)
- [Railscasts #274 Ghi nhớ tôi và đặt lại mật khẩu](http://railscasts.com/episodes/274-remember-me-reset-password)
- Xem [Xây dựng một bản sao Twitter bằng Ruby on Rails, Phần 1](https://www.youtube.com/watch?v=loYfUpwoLlM&t=3s)
và [Xây dựng một bản sao Twitter bằng Ruby on Rails, Phần 2](https://www.youtube.com/watch?v=fyj70TbS75c&t=474s). Nếu bạn vẫn cần thêm cảm hứng cho project này hoặc muốn hiểu sâu hơn, phần còn lại của loạt hướng dẫn này sẽ là một tài nguyên hữu ích.
