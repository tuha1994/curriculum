### Giới thiệu

Đây sẽ là phần thiên về xây dựng nhiều nhất của chương trình từ trước đến nay. Tất nhiên, bạn vẫn sẽ được yêu cầu đọc tài liệu, xem các bài viết trên blog và xem video trước khi bắt tay xây dựng, nhưng các dự án sẽ là trọng tâm chính.

Sau mỗi một hoặc hai bài học, bạn sẽ được yêu cầu xây dựng một hoặc nhiều dự án độc lập bằng cách vận dụng những khái niệm vừa học (đúng như cách chúng ta đã làm từ đầu đến giờ).


### Lộ trình

Sau khi chuẩn bị cho việc triển khai và cài đặt Rails, chúng ta sẽ bắt đầu bằng phần tổng quan về những chủ đề quan trọng như HTTP, MVC, REST, API, cookie và xác thực để xây dựng nền tảng, trước khi chuyển sang những nội dung thực sự thú vị.

Chúng ta sẽ đi từ phần đầu đến phần cuối của ứng dụng, bắt đầu với lớp định tuyến rồi chuyển sang controller và view để bạn có thể xây dựng một giao diện hoạt động được (dù chưa nhất thiết phải đẹp) cho dữ liệu của mình. Tiếp theo, bạn sẽ học cách lưu trữ và tìm kiếm dữ liệu trong cơ sở dữ liệu bằng SQL, rồi chuyển các câu lệnh SQL đó thành những truy vấn Active Record. Sau đó, chúng ta sẽ tìm hiểu về biểu mẫu web — một lĩnh vực có nhiều xử lý phía sau hơn bạn có thể tưởng — và xác thực, yếu tố thiết yếu để bảo mật ứng dụng. Chúng ta cũng sẽ đề cập đến một số chủ đề trung cấp khác như trạng thái và asset pipeline để hoàn thiện hiểu biết ban đầu của bạn về Rails.

Nhưng chúng ta chưa thể dừng ở đó, nên sẽ quay lại với ActiveRecord để cung cấp cho bạn những công cụ thực sự giúp dữ liệu của mình vận hành linh hoạt, cùng kiến thức về biểu mẫu cần thiết để đưa các chức năng nâng cao đó đến với người dùng. Đây là phần rất quan trọng của Rails, vì vậy chúng ta sẽ dành khá nhiều thời gian để làm việc với nó.

Cuối cùng, chúng ta sẽ tìm hiểu thêm những chủ đề hữu ích như cách gửi email từ ứng dụng, xây dựng và kết nối với API, các mẫu thiết kế, metaprogramming và định tuyến nâng cao, trước khi yêu cầu bạn xây dựng dự án cuối khóa.

### Công cụ và tài liệu

Tài nguyên quan trọng nhất mà chúng ta sẽ sử dụng là [tài liệu Rails Guides](http://guides.rubyonrails.org/). Chúng ta sẽ dùng các tài liệu này cùng với [API Rails](https://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html), nơi cho phép bạn tìm kiếm các phương thức Rails cụ thể.

Rails Guides toàn diện đến mức về cơ bản chúng chính là một giáo trình và tài liệu tham khảo hoàn toàn mã nguồn mở về Rails. Đôi khi, nội dung trong đó sẽ kỹ thuật và đi sâu hơn mức bạn mong muốn; khi ấy, bạn có thể đọc lướt. Khi tìm kiếm trợ giúp trong lúc xây dựng dự án, nếu không tìm được giải pháp trực tiếp trên Stack Overflow, lựa chọn tốt nhất của bạn sẽ là tìm đến các liên kết Rails Guides phù hợp.


### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến những nội dung liên quan. Đây không phải phần bắt buộc, vì vậy bạn có thể xem như tài liệu bổ sung.

- [StackOverflow: Tóm tắt các khái niệm nền tảng của Ruby on Rails](http://stackoverflow.com/questions/5205002/summary-of-ruby-on-rails-fundamental-concepts)
- [Cách học Rails Guides](http://www.sihui.io/how-to-study-the-rails-guides/)