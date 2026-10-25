### Giới thiệu

Bạn còn nhớ phần khởi động trong project trước, khi bạn thử làm việc với một API không? Bây giờ là lúc thực sự sử dụng một API. Hoan hô! Lần này, chúng ta sẽ dùng một dịch vụ khác có tên là Pexels. Bạn sẽ tạo một widget ảnh cơ bản cho phép hiển thị ảnh từ thư viện Pexels của mình. Chúng ta sẽ dựa vào một bài hướng dẫn để thực hiện phần lớn công việc, nhưng các nguyên tắc bạn áp dụng ở đây có thể tiếp tục được sử dụng khi làm việc với những API khác.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Trước tiên, hãy tạo một tài khoản trên [Pexels](https://www.pexels.com/). Nhấp vào nút "Join".
1. Đăng nhập, nhấp vào biểu tượng ba dấu chấm ở góc trên bên phải rồi chọn ["Image & Video API"](https://www.pexels.com/api/). Tiếp theo, nhấp vào nút ["Your API Key"](https://www.pexels.com/api/key/).
1. Bạn sẽ phải cung cấp một số thông tin cơ bản về ứng dụng của mình. Điền vào biểu mẫu và cho họ biết ứng dụng thư viện ảnh của bạn sẽ tuyệt vời đến mức nào. Nhấp vào "Generate API Key" để tạo một key cho bạn. Sao chép key vào một nơi mà sau này bạn có thể truy cập. Khi đã đăng nhập, bạn vẫn có thể tìm lại key bằng cách truy cập lại ["Your API Key"](https://www.pexels.com/api/key/).
1. Duyệt website để tìm một số ảnh rồi thêm chúng vào một collection (hoặc tự tải ảnh của bạn lên!). Truy cập collection vừa tạo và chú ý rằng URL sẽ có dạng `https://www.pexels.com/collections/collectionTitle-collectionIDhere/`. Ví dụ có thể là `adkmjqz`. Bạn sẽ cần phần này cho một số phương thức API sau đó.
1. Tự tạo một client class tương tác với Pexels API. Bạn có thể sử dụng một vài gem HTTP client, chẳng hạn như [Faraday](https://github.com/lostisland/faraday) hoặc [HTTParty](https://github.com/jnunemaker/httparty). Có thể đây là đoạn code đầu tiên bạn viết trong một Rails app không vừa vặn với một ActiveRecord model hoặc ActionController controller, nên bạn có thể tự hỏi: nên đặt class này ở đâu? Rails không quy định một convention cho trường hợp này. Có nhiều lựa chọn, và mỗi team có thể có quan điểm khác nhau về nơi đặt loại code này. Bạn có thể đặt nó trong `app/models/` (và *không* cho nó kế thừa từ `ApplicationRecord`). Một số developer lại thích tạo một directory mới bên trong `app/` để chứa các object dùng cho việc tích hợp với những service bên ngoài. Một ví dụ rất phổ biến là tạo directory `app/services` rồi thêm vào đó các class dùng để gọi external API.

   <div class="lesson-note" markdown="1">

   #### Gem wrapper API

   Dĩ nhiên, có nhiều gem wrapper cho từng API cụ thể. Bạn có thể tìm thấy nhiều kết quả về [Pexels API wrapper gems](https://www.google.com/search?q=pexels+api+gem) trên Google hoặc trên [RubyGems](https://rubygems.org/) và sử dụng một trong số đó. Dù chúng có thể giúp việc phát triển dễ dàng hơn, nhiều wrapper không được duy trì tốt nếu không có nhiều người dùng. Vì vậy, chúng không an toàn để cài đặt một cách tùy tiện. Hãy đảm bảo mọi dependency bạn sử dụng đều được duy trì và hỗ trợ tốt.

   </div>

1. Lưu ý rằng hard code API key trực tiếp trong app không phải là một practice tốt, vì khi đó nó hầu như chẳng còn là bí mật, đặc biệt nếu bạn đẩy code lên GitHub. Một practice tốt hơn là lưu key trong một [environment variable](http://railsapps.github.io/rails-environment-variables.html) hoặc sử dụng một gem như [`figaro` (xem tài liệu)](https://github.com/laserlemon/figaro). Bạn cũng có thể đọc [bài viết của Web Crunch về Rails credentials](https://web-crunch.com/posts/the-complete-guide-to-ruby-on-rails-encrypted-credentials) (dù bài viết không đề cập, giá trị của mỗi key cũng có thể được truy cập bằng cách nối các key như các method, như trong những ví dụ của [Rails Guides](https://guides.rubyonrails.org/security.html#custom-credentials)). Environment variable cho phép bạn truyền key trực tiếp vào app từ command line khi app khởi động. Figaro cũng hoạt động theo nguyên tắc tương tự, nhưng giúp bạn lưu key trong một file thực tế mà không commit cùng phần code còn lại. Rails credentials mã hóa các key bằng master key. Hãy sử dụng một trong những kỹ thuật này, trừ khi bạn muốn hành động như một cao bồi.
1. Xây dựng một StaticPagesController cơ bản để hiển thị trang chủ cùng một form cơ bản. Form chỉ cần có một text field nhận ID của một Pexel collection mà bạn đã tạo. Khi submit form, trang sẽ refresh và hiển thị các ảnh từ collection đó.
1. Đọc [tài liệu Pexel API](https://www.pexels.com/api/documentation/) để biết thêm thông tin về cách API hoạt động.

</div>