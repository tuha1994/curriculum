### Giới thiệu

Bài học trước đã đề cập đến kiểm thử đơn vị, và giờ chúng ta sẽ chuyển sang kiểm thử model. Trong bài này, bạn sẽ hình dung được cách kiểm thử trong Rails đối với model, đồng thời tìm hiểu sâu hơn về cách cấu trúc các bài kiểm thử này.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Kiểm thử model là gì và khi nào nên sử dụng.
- Cách chạy một bài kiểm thử model.
- Cách cấu trúc việc kiểm thử model.

### Kiểm thử model là gì?

Kiểm thử model là một dạng kiểm thử đơn vị trong bối cảnh Ruby on Rails. Khi kiểm thử các model, chúng ta thường muốn tập trung vào việc kiểm thử logic nghiệp vụ — về cơ bản là những gì diễn ra phía sau mỗi model và giúp trải nghiệm người dùng hoạt động chính xác. Chẳng hạn, đó có thể là bảo đảm giá vé khuyến mãi chính xác, chỉ chấp nhận dữ liệu biểu mẫu hợp lệ, hoặc thực hiện quy trình xác minh người dùng tùy chỉnh. Điều quan trọng cần lưu ý là mỗi model nên có một file riêng trong `spec/models`. Cách này giúp các bài kiểm thử model được sắp xếp gọn gàng và dễ đọc.

### Thiết lập kiểm thử

Công cụ đầu tiên chúng ta sẽ tìm hiểu để thiết lập dữ liệu là [factory bot](https://github.com/thoughtbot/factory_bot). Factory bot là một gem có thể dùng để thiết lập dữ liệu kiểm thử và tạo dữ liệu cần thiết cho các bài kiểm thử. Việc thêm Factory bot tương tự như thêm các gem kiểm thử khác. Hãy thêm dòng sau vào phần tương ứng trong Gemfile.

```ruby
group :development, :test do

  gem "factory_bot_rails"  #Add this line.

end
```

Sau đó, nhập `bundle install` trong terminal để cài đặt gem. Bây giờ chúng ta cần chuyển sang thiết lập cấu hình kiểm thử để sử dụng Factory bot. Trước hết, hãy mở `spec/support/factory_bot.rb` và thêm cấu hình sau.

```ruby
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```

Hãy chắc chắn rằng file này được require trong `rails_helper.rb`. Bạn có thể require file bằng cách thêm `require support/factory_bot` vào đầu file. Rails cũng cung cấp cách này trong dòng đã được comment `Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }`, cho phép bạn tải tất cả các file hỗ trợ khi khởi động. Điều này hữu ích vì bạn không phải thêm thủ công từng file hỗ trợ, đồng thời tránh các sự cố nếu chẳng hạn bạn quên thêm một file.

Điều cuối cùng có thể khiến bạn thắc mắc là cơ sở dữ liệu kiểm thử tự xóa dữ liệu giữa các bài kiểm thử như thế nào. Rails tự động thực hiện việc này cho bạn. Tuy nhiên, nếu Rails không thực hiện, hãy bảo đảm bạn đang làm theo hướng dẫn trong file rails_helper.rb và trước tiên hãy hỏi trên [Discord](https://discord.com/channels/505093832157691914/690591236922409012)! Nếu quan tâm, bạn có thể tìm hiểu thêm về việc dọn dẹp cơ sở dữ liệu trong phần tài nguyên bổ sung.

### Tạo một factory

Dưới đây là ví dụ về một factory cho user. Lưu ý rằng factory sẽ là một lớp User dựa trên tên của factory. Bạn cũng có thể định nghĩa bao nhiêu thuộc tính tùy ý; thông thường, chỉ nên thêm những thuộc tính mà bạn đang kiểm thử và bổ sung chúng khi cần.

```ruby
# This will guess the User class
FactoryBot.define do
  factory :user do
    first_name { "John" }
    last_name  { "Doe" }
    admin { false }
  end
end
```

Khi tạo một bản ghi bằng factory bot, bạn có thể dùng `create(:user)` để nhận về một instance đã được lưu của đối tượng trong cơ sở dữ liệu, hoặc `build(:user)` để nhận về một instance chưa được lưu của đối tượng.

### Ví dụ về kiểm thử model

Trong ví dụ sau, một tình huống thường gặp khi người dùng đăng ký là website sẽ yêu cầu email để gửi thông báo cho người dùng. RSpec có thể giúp bạn kiểm thử điều này bằng cách sử dụng các bài kiểm thử model. Hãy xem việc yêu cầu email của user phải tồn tại trong model và các bài kiểm thử tương ứng sẽ như thế nào.

```ruby
class User < ApplicationRecord
  validates :email, presence: true
end
```

Trong đoạn code trên, validates sẽ kiểm tra sự hiện diện của email. Khi dùng validates, model sẽ kiểm tra điều kiện sau: trong trường hợp này, email phải tồn tại khi bản ghi được tạo. Từ đó, chúng ta có bài kiểm thử RSpec sau, trong đó sẽ viết bối cảnh và kết quả mà chúng ta mong đợi.

```ruby
RSpec.describe User, type: :model do
  describe '#valid?' do
    context "when the user does not have an email address" do
      user =  build(:user)

      it "will not be valid" do
        expect(user.valid?).to be false
      end
    end
    
    context "when the user has an email address" do
      user =  build(:user, email_address: "jdoe@example.com")

      it "will be valid" do
        expect(user.valid?).to be true 
      end
    end
  end
end
```

Một điều cần lưu ý là chúng ta có hai bài kiểm thử. Chúng ta muốn kiểm thử cả điều kiện thành công, đồng thời có một bài kiểm thử riêng cho trường hợp không hợp lệ bị từ chối. Một điểm khác cần lưu ý là bạn có thể thiết lập các thuộc tính khi tạo bản ghi factory thay vì phải quay lại file factory.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [Repository của Factory bot](https://github.com/thoughtbot/factory_bot/blob/main/GETTING_STARTED.md#setup) có thông tin rất hữu ích để tìm hiểu sâu hơn, và bạn nên đọc qua để hiểu rõ hơn về factory bot.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Để xem một ví dụ về kiểm thử model và một tài nguyên hữu ích, bạn có thể truy cập [code with Jason](https://www.codewithjason.com/write-model-tests-part/).
- Nếu database cleaner tích hợp sẵn của RSpec không hoạt động khi kiểm thử, bạn có thể tìm một giải pháp thay thế trong [phần Database Cleaner](https://thoughtbot.com/blog/how-we-test-rails-applications#database-cleaner) trên blog của thoughtbot.
- Đọc [bài viết trên betterspecs.org](https://www.betterspecs.org/#factories) để tìm hiểu một số phương pháp hay nhất khi viết kiểm thử.