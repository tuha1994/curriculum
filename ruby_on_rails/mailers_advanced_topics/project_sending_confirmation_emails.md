### Giới thiệu

Thiết lập một mailer là công việc tương đối đơn giản. Nó rất giống với việc xây dựng một controller và các view mới. Một khi đã làm vài lần, bạn sẽ thấy việc này khá tự nhiên.

<div class="lesson-note" markdown="1">

#### Thiết lập WSL cho Letter Opener

   Nếu gặp vấn đề khi sử dụng gem `letter_opener` trên WSL, bạn có thể khắc phục bằng cách định nghĩa lược đồ URI tệp cho WSL trong `config/development.rb`:

   ```ruby
   LetterOpener.configure do |config|
     config.file_uri_scheme = 'file://wsl.localhost/Ubuntu'
   end
   ```

   Bạn có thể kiểm tra lại lược đồ URI tệp bằng lệnh sau trong terminal WSL:

   ```bash
   wslpath -m /
   ```

   Bạn cũng sẽ phải tạo một biến môi trường có tên BROWSER, chứa đường dẫn đến trình duyệt bạn chọn, rồi thêm biến đó vào ~/.bashrc. Ví dụ:

   ```bash
   export BROWSER=/mnt/c/Program\ Files/Google/Chrome/Application/chrome.exe
   ```

   Biến này phải trỏ đến trình duyệt của bạn, tức là phải chứa đường dẫn đến trình duyệt.

</div>

### Bài tập

Hãy lấy lại dự án [Flight Booker](/lessons/ruby-on-rails-flight-booker) và làm cho dự án gửi một email xác nhận với nội dung "Bạn đã đặt vé" đến tất cả Passenger khi chúng được tạo trong quá trình đặt vé. Hãy đảm bảo gửi một email cho từng Passenger trong Booking, chứ không chỉ gửi một email cho cả Booking. (Ngoài ra, bạn có thể dùng một dự án khác của mình, miễn là dự án đó có chức năng đăng ký người dùng.)

<div class="lesson-content__panel" markdown="1">

1. Tìm và mở tệp dự án.
1. Hãy tập chống đẩy và jumping jack. Gần đây bạn đã ngồi trước máy tính hơi nhiều rồi đấy.
1. Tạo mailer mới bằng `$ rails generate mailer PassengerMailer`.
1. Cài đặt [gem Letter Opener](https://github.com/ryanb/letter_opener) để mở email trong trình duyệt thay vì gửi email trong môi trường phát triển.
1. Làm theo các bước trong [Rails Guide](http://guides.rubyonrails.org/action_mailer_basics.html) để tạo action gửi email xác nhận.
1. Tạo cả phiên bản `.html.erb` và `.text.erb` cho email xác nhận vé.
1. Kiểm tra việc gửi email bằng cách tạo một lượt đặt chuyến bay mới (`letter_opener` sẽ mở email trong trình duyệt nếu bạn đã thiết lập đúng).
1. Hãy thử thêm một thủ thuật khác -- gọi trực tiếp mailer từ Rails Console bằng cách tương tự như sau:

   ```ruby
   > PassengerMailer.confirmation_email(Passenger.first).deliver_now!
   ```

1. Điểm cộng: Triển khai dự án lên một nhà cung cấp dịch vụ lưu trữ và thử nghiệm. Bạn sẽ cần thêm một chút thiết lập để một nhà cung cấp email như [SendGrid](https://sendgrid.com/) hoạt động và gửi email trong môi trường production.

(Một lưu ý: để sử dụng nhà cung cấp email, có lẽ bạn sẽ phải cung cấp thông tin thẻ tín dụng. Họ làm vậy để ngăn chặn spammer. Bạn không phải trả phí cho dịch vụ, nhưng vẫn phải cung cấp thông tin của mình.)

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Bạn không bắt buộc phải đọc, nên hãy xem đây là phần bổ trợ.

- [Thiết lập email: Rails, Heroku, SendGrid, Figaro](http://howilearnedrails.wordpress.com/2014/02/25/setting-up-email-in-a-rails-4-app-with-action-mailer-in-development-and-sendgrid-in-production-using-heroku/comment-page-1/#comment-79)