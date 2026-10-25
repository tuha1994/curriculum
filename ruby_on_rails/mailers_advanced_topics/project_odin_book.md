### Giới thiệu

Bạn đã đi được một chặng đường dài, xin chúc mừng! Đến thời điểm này, bạn hẳn đã cảm thấy khá tự tin khi xây dựng các ứng dụng Rails mới, mô hình hóa dữ liệu và làm việc với các biểu mẫu. Dự án này sẽ yêu cầu bạn vận dụng toàn bộ kiến thức mới học để kiểm tra năng lực của mình. Đây cũng sẽ là một sản phẩm tuyệt vời trong hồ sơ năng lực, thể hiện những gì bạn có thể làm được. Dự án sẽ cần một chút suy nghĩ và thời gian, nhưng chắc chắn nằm trong khả năng của bạn.

Bạn sẽ xây dựng một bản sao của một trang mạng xã hội, chẳng hạn như Facebook, X, Myspace hoặc Threads. Cũng như các bài học trước, bạn muốn đầu tư bao nhiêu công sức cho phần định kiểu và giao diện người dùng là tùy bạn. Phần quan trọng là dữ liệu và phần máy chủ. Bạn sẽ xây dựng các tính năng cốt lõi của nền tảng như người dùng, hồ sơ, bài đăng, theo dõi và "thích".

Bạn cũng sẽ triển khai một hình thức xác thực nào đó. Tốt nhất là bạn nên dùng OmniAuth để hỗ trợ xác thực thông qua trang mạng xã hội mà bạn đang xây dựng bản sao, nhưng một số trang (chẳng hạn như Facebook) gần đây đã khiến quy trình này trở nên bất khả thi. Nếu trang của bạn rơi vào trường hợp đó, bạn có thể dùng Devise để hỗ trợ xác thực bằng tên người dùng và mật khẩu (sử dụng Devise), hoặc thông qua Github với `omniauth-github`.

Có thể sẽ có những tính năng bạn chưa từng được làm quen -- chẳng hạn như trò chuyện, cập nhật bảng tin theo thời gian thực và thông báo theo thời gian thực. Bạn không bắt buộc phải tạo những tính năng đó, trừ khi muốn đi trước chương trình và thử sức.

### Bài tập

Hãy xây dựng một trang mạng xã hội! Trong dự án này, bạn sẽ xây dựng phần lớn chức năng cốt lõi dành cho người dùng của trang bạn đã chọn. Bạn không cần lo lắng về một số phần giao diện cầu kỳ hơn trừ khi muốn làm, vì chúng không cần thiết để tạo ra trải nghiệm người dùng tốt.

Bạn nên viết ít nhất một bộ kiểm thử tích hợp cơ bản để kiểm tra mỗi trang có tải đúng hay không, cùng với các kiểm thử đơn vị để đảm bảo những association đã được thiết lập chính xác (ví dụ: kiểm tra `User.first.posts` hoạt động đúng). Hãy chạy chúng liên tục ở chế độ nền bằng [Guard](https://github.com/guard/guard) (xem [Chương 3.7.3 của Ruby on Rails Tutorial](https://www.learnenough.com/ruby-on-rails-4th-edition-tutorial/static_pages#sec-guard)).

Dự án này cho bạn cơ hội biến một bộ yêu cầu tương đối khái quát thành một website hoạt động được. Bạn sẽ cần đọc tài liệu trên GitHub về các gem mà mình sử dụng.

Hãy ghi nhớ các yêu cầu sau. Chúng ta sẽ trình bày những bước cụ thể để bắt đầu ở bên dưới danh sách này:

<div class="lesson-content__panel" markdown="1">

1. Ngay từ đầu, hãy sử dụng PostgreSQL cho cơ sở dữ liệu (không dùng SQLite3), để việc triển khai sau này diễn ra thuận lợi hơn nhiều.
1. Người dùng phải đăng nhập mới có thể xem bất kỳ nội dung nào ngoài trang đăng nhập.
1. Chức năng đăng nhập của người dùng phải sử dụng gem [Devise](https://github.com/plataformatec/devise). Devise cung cấp rất nhiều phương thức hữu ích, vì vậy bạn không còn phải tự viết mật khẩu người dùng, session và các phương thức `#current_user`. Hãy xem [Railscast về Devise](http://railscasts.com/episodes/209-introducing-devise?view=asciicast) (sử dụng Rails 3) để được hướng dẫn từng bước. Tài liệu chính thức sẽ được cập nhật đầy đủ.
1. Người dùng có thể gửi yêu cầu theo dõi đến những người dùng khác.
1. Người dùng có thể tạo bài đăng (ban đầu chỉ cần hỗ trợ văn bản).
1. Người dùng có thể thích bài đăng.
1. Người dùng có thể bình luận trên bài đăng.
1. Bài đăng luôn phải hiển thị nội dung bài đăng, tác giả, bình luận và lượt thích.
1. Cần có một trang danh sách bài đăng, hiển thị tất cả bài đăng gần đây của người dùng hiện tại và những người dùng mà họ đang theo dõi.
1. Người dùng có thể tạo hồ sơ kèm ảnh đại diện. Bạn có thể lấy ảnh đại diện khi người dùng đăng nhập bằng OmniAuth. Nếu không thể, bạn có thể dùng [Gravatar](https://en.gravatar.com/) để tạo ảnh.
1. Trang hồ sơ của người dùng phải chứa thông tin hồ sơ, ảnh hồ sơ và các bài đăng của họ.
1. Cần có một trang danh sách người dùng, hiển thị tất cả người dùng cùng các nút gửi yêu cầu theo dõi đến những người dùng mà người dùng hiện tại chưa theo dõi hoặc chưa có yêu cầu đang chờ.
1. Thiết lập một [mailer](http://guides.rubyonrails.org/action_mailer_basics.html) để gửi email chào mừng khi người dùng mới đăng ký. Sử dụng [gem Letter Opener](https://github.com/ryanb/letter_opener) để kiểm thử trong môi trường phát triển.
1. Triển khai ứng dụng lên một nhà cung cấp dịch vụ lưu trữ.
1. Thiết lập một nhà cung cấp email và bắt đầu gửi email thật.

#### Điểm cộng

1. Cho phép bài đăng chứa cả hình ảnh (chỉ thông qua URL hoặc, phức tạp hơn, bằng cách tải ảnh lên).
1. Sử dụng [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html) để cho phép người dùng tải ảnh lên hồ sơ của mình.
1. Cho phép bài đăng là văn bản HOẶC ảnh bằng cách sử dụng một polymorphic association (để người dùng vẫn có thể thích hoặc bình luận mà không cần biết bài đăng thuộc loại nào).
1. Hãy định kiểu cho website thật đẹp! Chúng ta sẽ tìm hiểu sâu hơn về HTML/CSS trong khóa học tiếp theo.

#### Bắt đầu

1. Hãy suy nghĩ về kiến trúc dữ liệu cần thiết để dự án hoạt động. Có rất nhiều model và association, vì vậy hãy dành thời gian lập kế hoạch cho cách tiếp cận của bạn.
1. Xây dựng ứng dụng Rails PostgreSQL mới bằng `$ rails new odinbook --database=postgresql`, khởi tạo kho Git và cập nhật README để liên kết trở lại trang này.
1. Lần lượt thực hiện danh sách ở trên! Mỗi bước sẽ đem đến một thử thách mới, nhưng bạn đã có đủ công cụ.
1. Bạn có thể tạo dữ liệu như người dùng và bài đăng bằng gem [Faker](https://github.com/stympy/faker), về cơ bản đây là một thư viện chứa tên và email mẫu. Tuy vậy, có lẽ sẽ dễ hơn nếu bạn tự viết seed trong tệp `db/seeds.rb`; tệp này sẽ được chạy khi bạn nhập `$ rake db:seed`.

#### Bước cuối cùng

1. Trước khi tiếp tục, chúng tôi rất mong bạn [gửi phản hồi về khóa học Ruby on Rails](https://docs.google.com/forms/d/e/1FAIpQLScrIWkh4Bm3Qv7FH7PuWjLRCryevoJGgpA4tTis4R7CpXp9oQ/viewform?usp=sf_link). Phản hồi của người dùng (chính là bạn) rất quan trọng để chúng tôi có thể tiếp tục cải thiện chương trình và hiểu hơn về trải nghiệm của bạn.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Bạn không bắt buộc phải đọc, nên hãy xem đây là phần bổ trợ.

- Để tìm một giải pháp thay thế cho việc dùng AWS S3 lưu trữ hình ảnh, hãy xem [cloudinary](https://cloudinary.com/documentation/rails_integration) này