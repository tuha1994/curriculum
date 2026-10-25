### Giới thiệu

Hãy tưởng tượng cơ sở dữ liệu của bạn đã hoạt động và bạn muốn bảo đảm mọi người gửi dữ liệu hợp lệ trong các request của họ. Chẳng hạn, để tạo tài khoản trên trang web, người dùng cần nhập cả tên người dùng lẫn địa chỉ email. Làm thế nào để bắt buộc họ nhập cả hai giá trị, hoặc bảo đảm mỗi giá trị chỉ chứa những ký tự phù hợp? Những hạn chế như vậy được thực hiện bằng *validations*.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Validations là gì.
- Cách chỉ định validations.
- Ba cấp độ validations.

### Validations là gì?

Validations là những ràng buộc được áp dụng ở ba cấp độ để bảo đảm dữ liệu đáp ứng các tiêu chí nhất định trước khi có thể được lưu vào cơ sở dữ liệu. Ba cấp độ đó là phía client, phía server -- tức cấp độ Rails -- và cơ sở dữ liệu. Validation ở mỗi cấp độ sau lại nghiêm ngặt và an toàn hơn cấp độ trước.

### Cách chỉ định validations

Giả sử có người thử tạo một user mới. Họ nhấp vào nút gửi trên một biểu mẫu nào đó, HTTP request được gửi đi, rồi các quy trình validation diễn ra. Ở cấp độ Rails, bạn có thể viết một validation cơ bản trong file model User như sau.

Trong `app/models/user.rb`:

```ruby
class User < ApplicationRecord
  validates :name, presence: true
end
```

Lưu ý rằng ví dụ trên đã được đơn giản hóa để minh họa, chỉ thể hiện một validation duy nhất. Trong thực tế, logic validation trong một class thường phức tạp hơn và có thể gồm nhiều bước kiểm tra validation.

### Validation phía client

Ở cấp độ cao nhất, bạn có thể viết code bằng JavaScript trong trình duyệt để phát hiện xem ai đó đã điền biểu mẫu đúng cách chưa và nhắc họ hoàn tất trước khi tiếp tục. Chúng ta sẽ học thêm về phần này trong khóa học JavaScript. Ưu điểm của cách này là phản hồi gần như ngay lập tức, nhờ đó tạo ra trải nghiệm người dùng tốt. Vấn đề là JavaScript rất dễ bị qua mặt, và người dùng hoàn toàn có thể gửi một request độc hại hoặc sai lệch.

Ngoài ra, kể từ HTML5, validations cũng có thể được thực hiện trong các biểu mẫu HTML bằng HTML thuần. Cũng như validation biểu mẫu dựa trên JavaScript, validation biểu mẫu HTML rất dễ bị qua mặt.

### Validation phía server

Lớp thực thi thứ hai đối với validations cho dữ liệu người dùng (loại dữ liệu mà bạn tuyệt đối không nên tin tưởng) nằm ở cấp độ server. Điều này có nghĩa là viết code trong ứng dụng Rails của bạn (cụ thể là trong model mà bạn đang cố lưu một instance, chẳng hạn User) để kiểm tra dữ liệu người dùng nhập, đối chiếu với các ràng buộc bạn đã thiết lập và trả về lỗi nếu có.

Cách này an toàn hơn validation phía client, nhưng bất lợi ở chỗ cần một vòng request HTTP đầy đủ đến ứng dụng của bạn để kiểm tra. Model validations nhìn chung khá hiệu quả, và đó là phần chúng ta sẽ tập trung vào ở đây.

Một vấn đề khác xuất hiện khi ứng dụng của bạn mở rộng đến mức chạy nhiều instance trên nhiều server, tất cả cùng giao tiếp với một cơ sở dữ liệu trung tâm. Giả sử bạn muốn bảo đảm một tên người dùng là duy nhất... điều gì xảy ra nếu hai người dùng gần như đồng thời gửi cùng một tên người dùng, và request của họ được hai instance đang chạy đồng thời khác nhau tiếp nhận? Khi mỗi instance của ứng dụng kiểm tra với cơ sở dữ liệu xem tên người dùng có duy nhất không, cả hai lần đều nhận thấy mọi thứ có vẻ ổn, nên cả hai cùng tiếp tục lưu model... ôi không! Điều này có thể nghe không thật khả thi, nhưng hãy nghĩ đến những giao dịch tự động diễn ra liên tục với tốc độ cao. Những "race conditions" này hoàn toàn có thật.

### Validation ở cấp độ cơ sở dữ liệu

Vì vậy, cách duy nhất để thực sự thực thi các ràng buộc là ở cấp độ cơ sở dữ liệu, bởi cơ sở dữ liệu duy nhất của bạn mới là nơi phán quyết cuối cùng về thứ gì là duy nhất và hợp lệ trong hệ thống này. Bạn có thể truyền thêm tham số cho một số phương thức migration quen thuộc như `add_index`, chẳng hạn `add_index :users, :username, unique: true`, để thực thi việc cột đó là duy nhất theo cách an toàn nhất. Tuy vậy, một lần nữa, phần lớn validations của bạn có thể được triển khai trong các model của ứng dụng Rails.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương Validations trong Rails Guides](http://guides.rubyonrails.org/active_record_validations.html)
   - Có thể đọc lướt phần 2 về helpers -- các helper này giúp bạn làm validations cụ thể hơn, và sau này bạn sẽ gặp chúng
   - Bạn có thể đọc lướt phần 7 về custom validators
   - Phần 9 có lẽ chỉ thú vị nếu trước đây bạn đã thấy ERB trong các Rails view... chúng ta sẽ học đến đó.

</div>