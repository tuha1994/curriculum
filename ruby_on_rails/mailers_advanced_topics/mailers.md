### Giới thiệu

Email có mức độ phức tạp ở nhiều phương diện nhưng thường không được đánh giá đúng. Chắc chắn bạn sẽ không nghĩ nhiều về nó cho đến khi thật sự cần tích hợp vào ứng dụng. Nhưng những gì diễn ra phía sau, đặc biệt sau khi nhấn "send", khá ấn tượng... email phải đi qua rất nhiều tầng trong kiến trúc Internet, vượt qua tường lửa và xử lý các bộ lọc SPAM. May mắn là một số nhà cung cấp email bên thứ ba chuyên dụng có thể đảm nhiệm toàn bộ việc này.

Ở đây chúng ta sẽ tập trung vào việc tạo email thực tế... chẳng hạn, làm thế nào để tạo email đặc biệt kiểu "cảm ơn bạn đã đăng ký, userX". Xét về mặt khái niệm, việc tạo và gửi email khá giống render view rồi gửi view đó đến trình duyệt web, ít nhất là từ góc nhìn của một lập trình viên Rails. Trong bài học này, chúng ta sẽ tìm hiểu quy trình đó và bạn sẽ có cơ hội tự gửi email trong các dự án.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Mailer là gì.
- Mailer tương tự controller hoặc model như thế nào.
- Vì sao mailer cần cả phiên bản text lẫn HTML.
- Nhà cung cấp email được dùng để làm gì.
- Gem `letter_opener` được dùng để làm gì.

### Tổng quan ngắn

Để gửi email, Rails sử dụng gem `ActionMailer` (một trong 7 gem cốt lõi cấu thành Rails). Mailer hoạt động tương tự như sự kết hợp giữa model và controller. Bạn sẽ tạo một mailer mới giống như tạo một controller mới (`$ rails generate mailer SomeName`). Sau đó, bạn thiết lập các method cho từng email mà mình muốn gửi (ví dụ: email chào mừng, email nhắc lại, email cảm ơn sau khi mua hàng...). Nghe khá giống các action của controller, đúng không?

Trong mỗi method, bạn cần xác định những trường chính và mọi biến sẽ được dùng trong phần nội dung email (chẳng hạn tên người dùng). Bản thân email thực chất chỉ là một view. Các view này nằm trong thư mục `app/views` cùng với những view còn lại, nhưng ở một thư mục con được đặt tên theo mailer.

Một điểm khác biệt giữa view của mailer và view "bình thường" là email thường có hai định dạng riêng -- HTML và text. Phần lớn mọi người sẽ xem phiên bản HTML, nhưng một số người tắt HTML nên họ cần phiên bản text để đọc email. Có cả hai phiên bản cũng giúp email vượt qua các bộ lọc SPAM. Bạn sẽ nhanh chóng nhận ra rằng nhiều việc liên quan đến email đều nhằm vượt qua bộ lọc SPAM đáng sợ.

Vì vậy, bạn thiết lập một view, chẳng hạn `app/views/user_mailer/welcome_email.html.erb`, và một view khác là `app/views/user_mailer/welcome_email.text.erb`. Việc phải quản lý hai phiên bản khác nhau của cùng một email hơi phiền, nhưng đó là cái giá của khả năng tiếp cận và việc chống SPAM. Bạn cũng sẽ nhận thấy có thể dùng tiền xử lý ERB (hoặc HAML, v.v.) cho email. Giống mọi view khác, cách này cho phép bạn tạo động nội dung có những thành phần như tên người dùng hoặc thông tin tài khoản không nhạy cảm. Tất cả phụ thuộc vào các biến bạn truyền cho view từ mailer, giống như việc truyền các biến instance của action controller cho một view thông thường.

Mailer thực chất là quy trình gồm hai bước -- trước tiên bạn tạo bản thân email, sau đó gọi method `#deliver_now` (hoặc `#deliver_now!`) để thực sự gửi nó. Một điểm khiến mailer khác controller là bạn gọi mailer trực tiếp thay vì đi qua router. Khi muốn gọi email chào mừng, bạn sẽ viết `UserMailer.welcome_email(@user)`, lệnh này trả về đối tượng email. Gửi nó bằng cách gọi method `#deliver_now` trên đối tượng đó, tạo thành chuỗi đầy đủ: `UserMailer.welcome_email(@user).deliver_now`.

Miễn là các tệp view của bạn (HTML và text) được đặt tên giống với các method của mailer, Rails sẽ tìm thấy mọi thứ và gửi email (gồm cả phần HTML lẫn text) bằng nhà cung cấp email mà bạn đã chọn. Phần hướng dẫn Rails được giao đọc trình bày khá tốt quy trình này, nên bạn sẽ có cơ hội thấy nó hoạt động thực tế.

### Callback

Mailer cho phép bạn dùng callback giống như controller thông thường, chẳng hạn callback `after_action`. Các callback sẽ chạy sau khi email được *tạo*, chứ không phải sau khi email được *gửi*, nên bạn có thể sửa các phần của email nếu cần. Bạn có quyền truy cập vào các biến instance của đối tượng mail, vì vậy có thể dùng biến `@user` trong logic của mình.

### Nhà cung cấp email

Trong tài liệu đọc thêm, bạn sẽ thấy cách gửi thư bằng tài khoản Gmail, nhưng nếu xây dựng một ứng dụng thực tế thì rõ ràng bạn sẽ muốn một giải pháp mạnh mẽ hơn. Có một số nhà cung cấp hàng đầu chuyên gửi email thay bạn. Công việc của họ là đảm bảo thư được gửi đến nơi và được mở, để bạn có thể tập trung xây dựng ứng dụng.

[SendGrid](https://addons.heroku.com/sendgrid#1500000) là nhà cung cấp được website này dùng để gửi email chào mừng và các email tương tự; việc sử dụng nó trên Heroku khá đơn giản.

Hãy xem [tài liệu của Heroku về Twilio SendGrid](https://devcenter.heroku.com/articles/sendgrid), cũng như [tài liệu riêng của SendGrid](https://docs.sendgrid.com/for-developers/sending-email/rubyonrails), trong đó có hướng dẫn thiết lập tệp `config/environment.rb` để ActionMailer giao tiếp với họ. Bạn sẽ lại cần dùng biến môi trường (hoặc gem `figaro`) để tránh hard-code mật khẩu và tên người dùng.

Giống như hầu hết mọi thứ, dịch vụ này miễn phí cho đến một mức sử dụng nhất định. Trong lúc xây dựng các ứng dụng thử nghiệm, nó hoàn toàn đáp ứng được nhu cầu. Ngoài ra còn có các lựa chọn như [MailGun](https://www.mailgun.com/), [Mailchimp](https://mailchimp.com/) và [Postmark](https://postmarkapp.com/).

Bạn sẽ có cơ hội thực hành với mailer trong các dự án.

### Letter Opener

Một điều quan trọng cần lưu ý là bạn không muốn gửi hàng loạt email khi đang kiểm thử ứng dụng trong môi trường development. Đó không chỉ là cách làm không tốt; nó còn có thể khiến người dùng khó chịu và làm địa chỉ của bạn bị đưa vào danh sách SPAM. Không ổn chút nào. Nhưng bạn vẫn cần chắc chắn chức năng email hoạt động đúng. May mắn là có một giải pháp khá hữu ích.

[Gem Letter Opener](https://github.com/ryanb/letter_opener), được thêm vào group `development` trong Gemfile, sẽ lấy các email và hiển thị chúng trong trình duyệt web bất cứ khi nào lẽ ra chúng được gửi đi. Bạn chỉ cần đổi một thiết lập cấu hình trong tệp `config/environments/development.rb` là xong. Rất tiện.

### Kinh nghiệm về email

- Email CHẬM! Mỗi email có thể mất 1–2 giây để gửi, vì vậy đừng để ứng dụng chính của bạn xử lý việc đó khi phải gửi rất nhiều email; nếu không, bất kỳ ai cố truy cập ứng dụng cũng có thể bị chặn.
- Hãy chắc chắn dùng URL đầy đủ trong mọi liên kết của mailer (dùng các helper `_url`, không dùng `_path`), vì người dùng sẽ mở email và nhấp vào liên kết từ một nguồn bên ngoài. Trong tệp `config/environments/production.rb`, bạn cần chỉ định tên host của website bằng cách như `config.action_mailer.default_url_options = { :host => 'yourapp.com' }`. Nếu không thiết lập, bạn có thể gặp thông báo lỗi về host hoặc các liên kết sẽ hiển thị kỳ lạ.
- Method `#deliver!` sẽ ném lỗi khi gửi thất bại, trong khi `#deliver` sẽ trả về false (thất bại im lặng).
- Khi tạo kiểu cho HTML email, bạn sẽ không có stylesheet để dùng, nên phải đặt toàn bộ CSS trực tiếp hoặc dùng các tag `<style>`.
- Đính kèm hình ảnh vào email (chẳng hạn dùng logo trong phiên bản HTML) có thể khá phiền. Hãy xem tài liệu đọc thêm.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc các phần 1 và 2 của [Rails Guide về ActionMailer](https://guides.rubyonrails.org/action_mailer_basics.html).
  1. Đọc các phần 6 và 7 để [tìm hiểu về callback và helper](https://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-callbacks).
  1. Đọc phần 8.2, trình bày về [gửi thư bằng Gmail của bạn](https://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-configuration-for-gmail).
  1. Đọc lướt các phần 9 và 10 về [kiểm thử và chặn email](https://guides.rubyonrails.org/action_mailer_basics.html#previewing-and-testing-mailers).

</div>

#### Kết luận

Gửi email chỉ là một cách hơi khác để sử dụng những mẫu mà bạn đã quen dùng cho controller và view, nên việc tiếp cận sẽ khá dễ dàng. Khi thử những điều mới, bạn sẽ phải xử lý loạt lỗi quen thuộc (thường liên quan đến cấu hình hoặc việc đặt tên tệp cho đúng), nhưng bạn sẽ nhanh chóng nắm được.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- [Cách tạo, xem trước và gửi email từ ứng dụng Rails](https://www.youtube.com/watch?v=9eFXEzOPRNs)
