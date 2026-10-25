### Giới thiệu

Một trong những điểm tuyệt vời của triết lý convention over configuration của Rails là nó cung cấp các giá trị mặc định hợp lý ngay từ đầu, nhờ đó bạn có thể bắt đầu nhanh chóng. Tuy nhiên, SQLite thường không được sử dụng trong môi trường production. Một vấn đề lớn là nhiều nhà cung cấp dịch vụ hosting có hệ thống tệp tạm thời, đồng nghĩa với việc cơ sở dữ liệu của bạn có thể bị xóa.

Mặc dù Rails cho phép bạn sử dụng các cơ sở dữ liệu khác nhau cho từng môi trường, việc đảm bảo môi trường phát triển của ứng dụng gần với môi trường production nhất có thể sẽ an toàn hơn. Điều này giúp tránh các vấn đề phát sinh do sử dụng những công cụ khác nhau trong các môi trường khác nhau.

### Tổng quan bài học

Phần này cung cấp tổng quan khái quát về các chủ đề bạn sẽ học trong bài học này.

- Môi trường là gì và môi trường có thể ảnh hưởng đến ứng dụng của chúng ta như thế nào.
- Biến môi trường là gì và cách sử dụng chúng.
- Cách cài đặt và thiết lập PostgreSQL.
- Cách thiết lập một ứng dụng Rails cơ bản để sử dụng một cơ sở dữ liệu cụ thể.

### Các môi trường và pipeline

Trong bối cảnh phát triển ứng dụng, môi trường là tập hợp các công cụ, cấu hình và quy ước mà lập trình viên sử dụng để viết và chạy mã. Một môi trường thường bao gồm một hệ điều hành cùng các gói phần mềm khác được cài đặt trên hệ điều hành đó. Một số phần mềm, chẳng hạn như `code`, giúp viết phần mềm, trong khi những phần mềm khác, chẳng hạn như `rbenv`, cung cấp các công cụ và cấu hình để giúp chạy phần mềm.

Có bốn loại môi trường cơ bản thường được thiết lập cùng nhau. Sau đó, ứng dụng đang được phát triển sẽ lần lượt đi qua các môi trường này; chúng được nhóm lại thành một "production pipeline":

1. Môi trường development thường là máy cục bộ của lập trình viên, nơi họ có thể thay đổi, thêm tính năng và cải thiện mã một cách an toàn mà không ảnh hưởng đến người dùng thực tế.
1. Môi trường test được thiết lập để hỗ trợ việc kiểm thử ứng dụng. Môi trường này có thể sử dụng các công cụ và thiết lập cấu hình khác nhau (chẳng hạn như ghi log chi tiết hơn) để kiểm tra hành vi của chương trình và xác định lỗi bằng các bài kiểm thử tự động.
1. Môi trường staging thường mô phỏng môi trường production, nhưng các lần triển khai không nhằm mục đích công khai.
1. Môi trường production chạy ứng dụng để người dùng có thể truy cập.

Vì môi trường là một tập hợp các công cụ và cấu hình, chúng rất hữu ích để lưu trữ các thông tin bí mật hoặc những thông tin có thể thay đổi tùy theo mục đích sử dụng của ứng dụng.

Ví dụ: máy chủ cơ sở dữ liệu cục bộ có thể sử dụng thông tin xác thực khác với cơ sở dữ liệu production, hoặc việc triển khai production có thể yêu cầu nhiều thông tin phân tích và log hơn.

Khi ứng dụng lớn dần, việc thiết lập các môi trường khác nhau này có thể giúp lập trình viên tạo ra sản phẩm tốt nhất, đồng thời đảm bảo các thay đổi (hoặc lỗi) không gây ảnh hưởng tiêu cực đến người dùng.

### Biến môi trường

Nền tảng của mọi môi trường là hệ điều hành, hệ điều hành cung cấp các cách để lưu trữ các biến cục bộ. Không có gì ngạc nhiên khi chúng được gọi là biến môi trường.

Tại sao biến môi trường lại quan trọng? Suy cho cùng, thông tin này có thể được thêm vào kho git (và do đó cả Github). Mặc dù các công cụ như git và Github rất tuyệt vời cho việc lưu trữ, quản lý phiên bản và chia sẻ mã, những thông tin nhạy cảm như API key hoặc mật khẩu nên được giữ kín. Ngay cả khi bạn xóa dữ liệu riêng tư khỏi commit hiện tại, dữ liệu đó vẫn có thể còn trong lịch sử commit!

Biến môi trường cho phép chúng ta thiết lập thông tin này tại nơi ứng dụng sẽ chạy mà không chia sẻ nó trên git hoặc Github. Vì các biến này chỉ có thể được truy cập cục bộ, ứng dụng của chúng ta có thể truy cập các giá trị nhạy cảm mà không công khai chúng.

Rails sử dụng môi trường để thiết lập biến môi trường nội bộ của riêng mình. Framework sẽ tìm biến `RAILS_ENV` trong môi trường và thiết lập env của nó theo cách đó. Nếu không tìm thấy biến này, thì [Rails sẽ giả định ứng dụng đang ở môi trường development](https://github.com/rails/rails/blob/main/railties/lib/rails.rb#L69-L77). Thật thú vị, phải không?

Chúng ta sẽ sử dụng một biến môi trường để thiết lập thông tin xác thực PostgreSQL cho các ứng dụng Rails của mình.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Cài đặt PostgreSQL

1. Tùy thuộc vào hệ điều hành bạn đang sử dụng, hãy làm theo hướng dẫn cài đặt PostgreSQL phù hợp bên dưới:
   - [Linux](https://github.com/TheOdinProject/curriculum/tree/main/ruby_on_rails/introduction/installation_guides/postgresql/linux.md "PostgreSQL installation for Linux")
   - [macOS](https://github.com/TheOdinProject/curriculum/tree/main/ruby_on_rails/introduction/installation_guides/postgresql/macos.md "PostgreSQL installation for macOS")

#### Tạo một ứng dụng Rails sử dụng PostgreSQL

Bây giờ chúng ta đã cài đặt và cấu hình máy chủ PostgreSQL cục bộ, hãy tạo một ứng dụng Rails mới để đảm bảo mọi thứ hoạt động đúng.

1. Trong các phần trước của curriculum, Rails đã sử dụng gem `sqlite` để chạy và quản lý cơ sở dữ liệu. Vì chúng ta vừa thiết lập PostgreSQL, chúng ta cần cài đặt gem đi kèm để Rails biết cách tương tác với nhà cung cấp cơ sở dữ liệu này:

   ```bash
   gem install pg
   ```

   Chúng ta đã quen thuộc với `rails new <app_name>`, nhưng cũng có thể chỉ định cơ sở dữ liệu Rails nên sử dụng khi tạo ứng dụng mới. Vì vừa cài đặt PostgreSQL, hãy thử ngay!

   Di chuyển đến thư mục mà bạn lưu các dự án và chạy:

   ```bash
   rails new <app_name> --database=postgresql
   ```

1. Hãy chuyển vào thư mục ứng dụng và thiết lập thông tin xác thực cơ sở dữ liệu.

   ```bash
   cd <app_name>
   ```

   Khi đã ở đúng thư mục, chúng ta có thể thiết lập tên người dùng và mật khẩu cơ sở dữ liệu trong `config/database.yml` bằng cách thêm các mục chứa thông tin của mình:

   ```diff
   default: &default
     adapter: postgresql
     encoding: unicode
     # For details on connection pooling, see Rails configuration guide
     # http://guides.rubyonrails.org/configuring.html#database-pooling
     pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
   +  username: <role_name> # role previously added
   +  password: <%= ENV['DATABASE_PASSWORD'] %> # variable previously added
   # You must remove the '+' signs to be able to migrate your database
   ```

   Lưu tệp lại, rồi chúng ta có thể xem Rails bắt đầu hoạt động:

   ```bash
   rails db:create
   ```

   Rails sẽ tạo hai cơ sở dữ liệu, và để kiểm tra lần cuối, chúng ta có thể khởi động máy chủ Rails:

   ```bash
   rails s
   ```

   Mở trình duyệt và truy cập `localhost:3000`. Nếu thấy trang chào mừng của Rails, vậy là chúng ta đã hoàn tất! Nếu gặp lỗi, hãy đến [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để được trợ giúp!

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy bạn có thể xem nó như tài liệu bổ sung.

- RubyGuides có một [bài giới thiệu hay về biến môi trường](https://www.rubyguides.com/2019/01/ruby-environment-variables/) chứa nhiều thông tin, bao gồm một số tùy chọn gem để quản lý biến môi trường.
- DigitalOcean có một [hướng dẫn tuyệt vời về cách thiết lập PostgreSQL](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-22-04) mà bạn cũng có thể tham khảo.