### Giới thiệu

Callback là một cách phổ biến để thực thi mã tại những thời điểm cụ thể trong vòng đời của một đối tượng Active Record, chẳng hạn ngay trước khi đối tượng được tạo, sau khi được lưu hoặc sau khi bị hủy. Chúng có thể rất hữu ích nếu bạn cần thực hiện một thao tác mỗi khi đối tượng chạm tới một trong những mốc vòng đời đó, chẳng hạn chuyển email của người dùng thành chữ thường khi tạo tài khoản. Callback là cách nói kiểu như: "Này Active Record, khi bạn tạo xong một đối tượng User mới, hãy gọi cho tôi để tôi chạy phương thức này trước khi bất kỳ việc gì khác diễn ra."

Đây là một phần ngắn về một chủ đề hữu ích. Phần đọc trong Rails Guide đã trình bày khá đầy đủ, vì vậy phần tóm tắt của chúng ta đương nhiên sẽ ngắn gọn.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Callback được dùng để làm gì.
- Các giai đoạn chính trong vòng đời của một đối tượng Active Record.
- Cách xây dựng một callback "around".
- Cách chỉ định một hành động cụ thể để chạy callback.

### Vòng đời của một đối tượng active record

Callback cung cấp các hook tại những thời điểm cụ thể (trước, sau hoặc đôi khi là "xung quanh") trong vòng đời của một đối tượng. Những thời điểm đó là:

- **Khởi tạo** -- Khi đối tượng được dựng lần đầu HOẶC bất cứ khi nào nó được tải lại từ cơ sở dữ liệu vào bộ nhớ (tức là mỗi khi bạn tìm thấy nó trong một truy vấn).
- **Xác thực** -- Mỗi khi Rails kiểm tra đối tượng có hợp lệ hay không. Việc này có thể xảy ra khi bạn đang cố lưu đối tượng hoặc khi bạn tự chạy phương thức `#valid?`.
- **Lưu** -- Hành động thực sự lưu một đối tượng đã được dựng vào cơ sở dữ liệu. Việc này được kích hoạt bất cứ khi nào đối tượng được lưu, không chỉ trong lần đầu nó được tạo.
- **Tạo** -- Việc tạo và lưu một đối tượng mới.
- **Cập nhật** -- Việc cập nhật một đối tượng hiện có.
- **Tìm** -- Truy vấn các đối tượng hiện có (ví dụ: `#all`, `#first`, `#find`). Việc này thường được kích hoạt khi Rails thao tác với các đối tượng ở phía sau.

Bạn thường có ba lựa chọn cho callback. Không phải mọi bước trong vòng đời đối tượng đều hỗ trợ mọi callback, nhưng ba lựa chọn cơ bản là (lấy `create` làm ví dụ):

1. `before_create` -- Chạy phương thức trước hành động được nêu
1. `after_create` -- Chạy phương thức sau hành động được nêu
1. `around_create` -- Hơi phức tạp hơn. Với lựa chọn này, bạn sẽ viết một phương thức thực sự gọi `yield` tại một thời điểm nào đó tới hành động gốc. Nhờ vậy, bạn có thể đặt mã ở trước và sau nó, đồng thời BẠN quyết định hành động gốc được thực hiện ở thời điểm nào. Không quá phổ biến.

### Sử dụng callback

Để sử dụng callback, bạn cần "đăng ký" nó ở đầu Model bằng phương thức thích hợp (ví dụ: `before_create`). Bạn truyền cho phương thức đó một symbol tương ứng với tên phương thức, hoặc có thể viết callback dưới dạng một block ngay tại đó. Rails sẽ giữ phương thức này lại và gọi nó vào đúng thời điểm. Ví dụ:

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base

    before_create do |user|
      puts "about to create #{user.name}"
    end

    after_create :just_created

    private

    def just_created
      puts "just created a user"
    end
  end
```

### Chỉ định đặc điểm của callback

Callback cung cấp cho bạn một số tùy chọn để thu hẹp hoặc lựa chọn chính xác thời điểm bạn muốn chúng chạy. Nếu chỉ muốn chạy callback khi một action cụ thể của controller gọi nó, hãy dùng tùy chọn `:on`, tùy chọn này nhận một symbol đơn hoặc cả một mảng, ví dụ `before_create :run_code, :on => [:create, :update]`.

Bạn cũng có thể dùng các tùy chọn logic điều kiện `:if` và `:unless` để kiểm tra một phương thức trước khi chạy callback, chẳng hạn:

```ruby
  before_create :run_code, :unless => :method_is_true

  private

  def method_is_true
    true
  end
```

### Callback giao dịch

Đôi khi ứng dụng Rails của bạn cần tương tác với một ứng dụng bên ngoài (vốn không hoàn hảo) trong quá trình lưu. Những lúc khác, thao tác lưu của bạn sẽ phải xử lý nhiều việc cùng lúc và nếu một việc thất bại, tất cả đều cần được rollback. Thông thường, những trường hợp này sẽ bao gồm việc bọc thao tác lưu cơ sở dữ liệu trong một "transaction", nghĩa là hoặc tất cả các bước đều thành công, hoặc tất cả đều thất bại và được rollback.

Việc `commit` một transaction và khả năng `rollback` nếu nó thất bại đều là các sự kiện trong vòng đời mà bạn có thể móc vào bằng callback, ví dụ `after_commit` và `after_rollback`. Điều này không phổ biến, vì vậy hãy xem nó như một trong những điều kiểu "chỉ cần nhớ rằng đây là một tùy chọn".

### Kết luận

Callback hữu ích và nhiều callback, như `:after_create` và `:before_destroy`, khá phổ biến. Không có gì quá cao siêu ở đây, chỉ là một khái niệm hữu ích.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc [Rails Guide về Callback](http://guides.rubyonrails.org/active_record_callbacks.html)
  1. Đọc bài viết này của Samuel Mullen về [các hướng dẫn sử dụng callback](https://samuelmullen.com/articles/guidelines-for-using-activerecord-callbacks).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải nội dung bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [Tài liệu tham khảo WikiBooks về Callback](http://en.wikibooks.org/wiki/Ruby_on_Rails/ActiveRecord/Callbacks)
