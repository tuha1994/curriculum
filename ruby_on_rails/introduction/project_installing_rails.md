### Giới thiệu

Sẵn sàng bắt đầu với Rails chưa? Hãy bắt đầu bằng việc cài đặt Rails và tạo ứng dụng Rails đầu tiên của bạn!

Trước khi tiếp tục, hãy cùng xem lại một số cách làm tốt cần ghi nhớ:

- Làm theo hướng dẫn thật cẩn thận và đừng bỏ qua bất kỳ phần nào.
- **KHÔNG sử dụng `sudo` trừ khi The Odin Project yêu cầu cụ thể.** Không làm theo lời khuyên này có thể gây ra *rất nhiều* rắc rối. Trong một số trường hợp, bạn có thể thấy một thông báo trong terminal yêu cầu sử dụng `sudo` và/hoặc cài đặt thứ gì đó bằng `apt`. **Hãy bỏ qua những gì terminal nói** và làm theo hướng dẫn bên dưới.
- Sao chép và dán các lệnh để tránh gõ sai.
- Nếu bạn dừng làm dự án giữa chừng rồi quay lại sau, hãy nhớ dùng `cd` để di chuyển trở lại thư mục dự án, nếu không các lệnh sẽ không hoạt động.

Trong dự án này, chúng ta sẽ xây dựng một ứng dụng Rails đầy đủ chức năng. Mục đích chính của bài tập là đảm bảo mọi thứ đã được cài đặt và hoạt động chính xác trên máy tính của bạn. Đừng *lo lắng* nếu bạn chưa hiểu hoàn toàn mình đang làm gì. Sau này trong khóa học, bạn sẽ học chính xác tất cả các lệnh này thực hiện điều gì. Hiện tại, hãy làm thật chậm và nhớ làm theo **từng bước một** thật cẩn thận. Nếu gặp khó khăn, đừng quên rằng bạn luôn có thể tìm kiếm trợ giúp trên [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB). Bạn cũng có thể dùng [chức năng tìm kiếm của Discord](https://support.discordapp.com/hc/en-us/articles/115000468588-Using-Search) để kiểm tra xem có ai khác từng gặp vấn đề tương tự và cách họ giải quyết hay chưa.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Tạo ứng dụng web Ruby on Rails đầu tiên

Đừng lo nếu bạn chưa hoàn toàn hiểu mình đang làm gì trong các bước tiếp theo. Sau này, bạn sẽ học tất cả những điều này có tác dụng gì trong chương trình học của The Odin Project. Miễn là các lệnh hoàn tất thành công, hãy cứ tiếp tục. Lý do chính chúng ta thực hiện việc này là để đảm bảo mọi thứ đã được cài đặt và cấu hình đúng cách.

1. #### Cài đặt Rails

   Trước đó chúng ta đã cài đặt Ruby, và bây giờ là lúc cài đặt Ruby on Rails. Chỉ cần chạy một lệnh!

   ```bash
   gem install rails
   ```

   Sau khi quá trình cài đặt hoàn tất, bạn có thể kiểm tra xem mọi thứ có diễn ra tốt đẹp không bằng cách khởi động lại terminal và chạy lệnh sau:

   ```bash
   rails -v
   ```

   Lệnh này sẽ hiển thị phiên bản Rails đã cài đặt trên hệ thống của bạn, cho biết quá trình cài đặt đã diễn ra suôn sẻ.

1. #### Tạo ứng dụng

   Đây là lúc bạn có thể bắt đầu khó theo dõi chính xác điều gì đang xảy ra. Nếu chưa hiểu chuyện gì, chỉ cần kiểm tra lại xem bạn có nhập đúng các lệnh hay không rồi tiếp tục. Phần này nhằm giúp bạn làm quen với quy trình và xác minh rằng mọi thứ đang hoạt động. Một lần nữa, ở giai đoạn này bạn chưa hiểu chuyện gì đang xảy ra cũng không sao.

   Trước tiên, chúng ta sẽ di chuyển đến thư mục dùng để lưu các dự án, sau đó yêu cầu Rails khởi tạo ứng dụng giúp chúng ta.

   ```bash
   cd ~/your_odin_project_directory
   rails new my_first_rails_app
   ```

   Lệnh này sẽ thực hiện rất nhiều việc, và bạn sẽ thấy rất nhiều kết quả trong terminal. Nếu mọi thứ hoạt động, ở gần cuối phần kết quả trong terminal, bạn sẽ thấy nội dung tương tự như sau:

   ```bash

   Bundle complete! 16 Gemfile dependencies, 76 gems now installed.
   Use `bundle info [gemname]` to see where a bundled gem is installed.
   Switch development cable to use redis
      gsub  config/cable.yml
   Create controllers directory
      create  app/javascript/controllers
      create  app/javascript/controllers/index.js
      create  app/javascript/controllers/application.js
      create  app/javascript/controllers/hello_controller.js
   Import Stimulus controllers
      append  app/javascript/application.js
   Pin Stimulus
      append  config/importmap.rb

   ```

   Trong quá trình trên, Rails đã tạo một thư mục mới cho chúng ta. Bây giờ hãy `cd` vào đó:

   ```bash
   cd my_first_rails_app
   ```

   Chạy lệnh sau trong terminal:

   ```bash
   rails generate scaffold car make:string model:string year:integer
   ```

   Sau khi tạo scaffold, chúng ta cần migrate cơ sở dữ liệu.

   ```bash
   rails db:migrate
   ```

1. #### Khởi động ứng dụng

   Bây giờ bạn đã tạo một ứng dụng Rails, hãy khởi động ứng dụng để xem nó có hoạt động không!

   Trong terminal, nhập

   ```bash
   rails server
   ```

   Bây giờ, hãy mở trình duyệt và truy cập [http://localhost:3000/cars](http://localhost:3000/cars) để xem ứng dụng của bạn!

   <div class="lesson-note lesson-note--warning" markdown=1>

   #### Truy cập localhost từ máy ảo

   Nếu đang sử dụng máy ảo, bạn cần mở trình duyệt bên trong máy ảo để thao tác này hoạt động.

   </div>

   Hãy tạo một chiếc xe mới, sau đó tải lại trang để kiểm tra xem ứng dụng có hoạt động không. Bạn muốn thêm bao nhiêu chiếc xe cũng được! Khi đã hài lòng, hãy quay lại terminal nơi Rails server đang chạy và nhập <kbd>Ctrl</kbd> + <kbd>C</kbd> để đóng server.

   Trong cùng cửa sổ terminal, nhập `rails console --sandbox` để mở một phiên IRB đã được nạp mã của dự án. Lệnh này hữu ích khi bạn muốn khám phá dự án và thử nhanh các ý tưởng. Cờ `--sandbox` đảm bảo mọi thay đổi bạn thực hiện đối với cơ sở dữ liệu của ứng dụng sẽ không được lưu lại sau khi bạn rời phiên. Hãy thử nhập `Car.all` để xem danh sách các đối tượng xe bạn đã tạo trên trang web. Khi hoàn tất phiên, nhập `exit` để quay lại cửa sổ terminal.

#### Nền tảng Git

Giống như tất cả các dự án bạn đã làm cho đến nay, chúng ta cần sử dụng Git để quản lý phiên bản và đẩy ứng dụng lên các remote khác nhau.

1. #### Stage và commit các thay đổi cục bộ

   Rails đã tự khởi tạo Git cho bạn trong quá trình hoạt động, vì vậy hãy stage và commit toàn bộ phần việc đã thực hiện cho đến lúc này.

   ```bash
   git add .
   git commit -m "Initial commit"
   ```

1. #### Khởi tạo trên GitHub, thêm remote và push

   Hãy tạo một repo trên Github và nhớ **không** khởi tạo repo bằng README, vì Rails đã tạo sẵn một README rồi. Thêm repo này làm remote và push repo lên GitHub như bạn vẫn thường làm.

   NHẮC LẠI: Không nhập các ký hiệu `<` or `>` bên dưới. Hãy thay các ký hiệu đó cùng mọi nội dung nằm giữa chúng bằng URL bạn đã sao chép từ GitHub.

   ```bash
   git remote add origin <SSH URL from GitHub>
   git push -u origin main
   ```

1. #### Xác nhận Git hoạt động chính xác

   Xác nhận rằng việc push đã thành công và trên GitHub bạn thấy tất cả thư mục, tệp được Rails tạo ở máy cục bộ, bắt đầu bằng thư mục có tên "app".

   Đây là cột mốc khởi đầu cho hành trình Rails của bạn! Sau này, bạn sẽ có thể nhìn lại repo này và kinh ngạc trước chặng đường mình đã đi được bao xa!

</div>
