### Giới thiệu

Bạn có thể còn nhớ phần kiểm thử trong mục Ruby và tầm quan trọng của việc kiểm thử. Tương tự như phần đó, kiểm thử trong Ruby on Rails rất quan trọng để bảo đảm ứng dụng của bạn ổn định và phát hiện lỗi trước khi bạn mở rộng ứng dụng hơn nữa! Loại kiểm thử đầu tiên chúng ta sẽ tìm hiểu là kiểm thử đơn vị.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Kiểm thử đơn vị là gì và chúng thực hiện nhiệm vụ gì.
- Vì sao kiểm thử đơn vị lại quan trọng đến vậy.
- Kiểm thử đơn vị trong bối cảnh Ruby on Rails.

### Kiểm thử đơn vị là gì?

Kiểm thử đơn vị là việc kiểm thử một đơn vị code duy nhất để bảo đảm method hoặc đơn vị đó hoạt động đúng như mong đợi! Trong Ruby on Rails, model là một trong những khu vực nên được bao phủ bằng kiểm thử đơn vị. Cụ thể, các bài kiểm thử model này được dùng để kiểm thử logic phía sau các đối tượng model. Logic đó có thể bao gồm việc xác thực dữ liệu người dùng nhập từ biểu mẫu và các logic khác giữa các model, chẳng hạn như khi bạn cần bảo đảm method tạo mức giảm giá cho đối tượng hoạt động chính xác. Khi dự án phát triển, điều quan trọng là phải bảo đảm logic phía sau các model vẫn hoạt động đúng trong quá trình dự án liên tục mở rộng. Điều này giúp dự án dễ bảo trì hơn, vì các bài kiểm thử tạo ra một mạng lưới an toàn để bảo đảm chức năng hiện có tiếp tục hoạt động như mong đợi khi model cần được mở rộng hoặc chỉnh sửa.

Trước đó, bạn đã sử dụng RSpec để kiểm thử các dự án trong phần Ruby. Mặc dù Ruby on Rails đi kèm framework kiểm thử riêng, RSpec vẫn là một framework rất phổ biến để kiểm thử, kể cả trong Ruby on Rails. Vì vậy, chúng ta sẽ sử dụng RSpec cho phần kiểm thử Ruby on Rails.

### Thiết lập RSpec

Cài đặt RSpec cho Rails là một việc tương đối đơn giản.

Trước tiên, chúng ta thêm nó vào nhóm development và test của gemfile. Hãy mở gemfile và thêm đoạn này vào phần development/test.

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

Tiếp theo, hãy xem [hướng dẫn cài đặt rspec-rails](https://github.com/rspec/rspec-rails#installation) để hoàn tất các bước còn lại. Sau đó, bạn sẽ thấy một thư mục spec bổ sung ở cấp cao nhất của dự án.

Trong đó, bạn sẽ tìm thấy các thư mục khác nhau cho từng khu vực của ứng dụng, chẳng hạn như controller, model, system, v.v. Khi dùng lệnh để tạo model, controller, v.v., file spec sẽ được tạo trong thư mục tương ứng với đối tượng bạn đã tạo. Nếu tự tạo file spec, hãy bảo đảm bạn tạo file trong đúng thư mục, chẳng hạn file spec cho controller phải nằm trong thư mục controller. Bạn cũng sẽ thấy các file rails_helper và spec_helper. Đây là những file RSpec dùng để cấu hình môi trường kiểm thử.

### Ví dụ kiểm thử

Mỗi model nên có file spec riêng trong thư mục spec/models. File này sẽ được tạo khi bạn sử dụng lệnh sau.

```bash
rails generate model <model_name>
```

Dưới đây là ví dụ về một bài kiểm thử.

```ruby
RSpec.describe 'Post' do           
  context 'before publication' do  # Explains the context.
    it 'cannot have comments' do   # This is the outcome we expect.
      expect { Post.create.comments.create! }.to raise_error(ActiveRecord::RecordInvalid)  
      # Now we should expect RSpec to notify us that the test passed.
    end
  end
end
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Để xem thêm một ví dụ về kiểm thử đơn vị cùng các thông tin hữu ích khác, hãy đọc [repository của gem rspec-rails](https://github.com/rspec/rspec-rails/tree/6-0-maintenance#rspec-dsl-basics-or-how-do-i-write-a-spec).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Để xem một ví dụ về kiểm thử model và một tài nguyên hữu ích, bạn có thể truy cập [code with Jason](https://www.codewithjason.com/a-rails-model-test-hello-world/)