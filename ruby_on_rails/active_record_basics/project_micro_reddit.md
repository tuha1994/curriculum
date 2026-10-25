### Giới thiệu

Trong dự án này, bạn sẽ có cơ hội dành thời gian tìm hiểu kỹ về các model. Chúng ta sẽ thực hiện một số công việc mô hình hóa dữ liệu và áp dụng kiến thức về Active Record bằng cách xây dựng phiên bản Reddit thu gọn của riêng bạn.

### Khởi động: Nghĩ về dữ liệu trước

Bước đầu tiên để xây dựng một mô hình dữ liệu tốt thường không diễn ra trên máy tính. Bạn nên dành một chút thời gian suy nghĩ về những gì mình sẽ cần. Khi mới bắt đầu, có thể bạn chưa biết hết mọi thứ, nhưng việc vạch ra hướng tiếp cận là yếu tố then chốt để tránh những sai lầm tốn kém về sau.

Trong phần khởi động này, bạn sẽ được đưa ra nhiều kịch bản khác nhau và được yêu cầu phác thảo các model dữ liệu cho từng kịch bản. Bạn có thể dùng giấy bút hoặc, nếu may mắn có sẵn, một chiếc bảng trắng. Có một vài kỹ thuật cụ thể thường được chấp nhận để phác thảo model và association, nhưng bạn cứ làm theo cách tự nhiên nhất với mình. Một cách là lập danh sách các model; cách khác là tạo một sơ đồ trực quan, trong đó mỗi model được biểu diễn bằng một ô và bạn nối chúng bằng những mũi tên phù hợp.

Với mỗi kịch bản, bạn sẽ được yêu cầu ghi ra dữ liệu, association và validation cần thiết để xây dựng nó. Điều đó có nghĩa là xác định những model (bảng dữ liệu) nào cần thiết để lưu trữ dữ liệu (và cần những cột nào), cũng như những trường nào của các bảng đó phải chịu validation (ví dụ: độ dài mật khẩu tối thiểu hoặc username phải là duy nhất). Đừng lo nếu bạn chưa chắc cách triển khai một phần nào đó; mục đích ở đây là bắt đầu suy nghĩ về cách dữ liệu sẽ được cấu trúc.

Ví dụ: Bạn đang xây dựng một blog cho startup của mình, blog có nhiều tác giả và mỗi tác giả có thể viết nhiều bài đăng.

Có thể mô hình sẽ trông như sau:

Lưu ý: Chúng ta sẽ bao gồm các cột `:id`, `:created_at` và `:updated_at`, nhưng bạn có thể yên tâm giả định rằng chúng luôn tồn tại vì Rails hoặc cơ sở dữ liệu sẽ tự động cung cấp chúng

- Authors

  ```text
  username:string [unique, 4-12 chars, present]
  email:string [unique, present]
  password:string [6-16 chars, present]
  id:integer
  created_at:datetime
  updated_at:datetime

  has_many posts
  ```

- Posts

  ```text
  title:string [unique, present]
  body:text [present]
  author_id:integer [present]
  id:integer
  created_at:datetime
  updated_at:datetime

  belongs_to author
  ```

Hãy dùng bất kỳ định dạng nào bạn thấy phù hợp nhất.

### Bài tập về mô hình dữ liệu

Với mỗi kịch bản sau đây, hãy ghi ra những model, cột, validation và association mà bạn có thể sử dụng để triển khai kịch bản đó. Một số kịch bản khó hơn những kịch bản khác, và bạn sẽ phải vận dụng một chút sáng tạo để suy ra những cột nào có thể cần thiết để kịch bản có ý nghĩa trong thế giới thực.

Điểm mấu chốt là xác định phần nào nên là một model riêng và các model đó sẽ liên hệ với nhau qua association như thế nào (tất cả các quan hệ bên dưới đều là `has_many`, `has_one` và/hoặc `belongs_to`). Nếu chưa hình dung được chính xác, hãy ghi nhớ kịch bản khi học qua vài bài tiếp theo.

Hãy nhớ rằng, nếu bạn cảm thấy mình sẽ phải hard-code cùng một dữ liệu nhiều lần, đó có lẽ là dấu hiệu cho thấy bạn nên tạo một bảng riêng. Một ví dụ phổ biến là thông tin địa chỉ — bạn có thể ghi rõ thành phố và bang cho từng user. Vậy tại sao không tạo các model City và State riêng rồi liên kết chúng với nhau?

<div class="lesson-content__panel" markdown="1">

  1. Bạn đang xây dựng một nền tảng học trực tuyến (khá giống nền tảng này!). Bạn có nhiều khóa học khác nhau, mỗi khóa có tiêu đề và mô tả, và mỗi khóa học có nhiều bài học. Nội dung bài học gồm một tiêu đề và phần văn bản nội dung.
  1. Bạn đang xây dựng tab hồ sơ cho một user mới trên trang web. Bạn đã lưu username và email của user, nhưng giờ muốn thu thập thông tin nhân khẩu học như thành phố, bang, quốc gia, tuổi và giới tính. Hãy suy nghĩ — mỗi user nên có bao nhiêu profile? Bạn sẽ liên kết profile này với model User như thế nào?
  1. Bạn muốn xây dựng một bảng ghim ảo, nơi các user trên nền tảng có thể tạo "pin". Mỗi pin chứa URL trỏ tới một hình ảnh trên web. User có thể bình luận trên pin (nhưng không thể bình luận trên bình luận).
  1. Bạn muốn xây dựng một bảng tin nhắn giống [Hacker News](http://news.ycombinator.com). User có thể đăng các liên kết. Những user khác có thể bình luận trên các bài gửi đó hoặc bình luận trên các bình luận. Bạn sẽ làm thế nào để bảo đảm một bình luận biết nó nằm ở đâu trong hệ thống phân cấp?

</div>

### Dự án: Micro-Reddit

Hãy xây dựng [Reddit](http://reddit.com). À, có lẽ là một phiên bản rất sơ khai của nó mang tên `micro-reddit`. Trong dự án này, bạn sẽ xây dựng các cấu trúc dữ liệu cần thiết để hỗ trợ việc gửi liên kết và bình luận. Chúng ta sẽ không xây dựng giao diện người dùng vì không cần thiết... bạn có thể dùng Rails console để thử nghiệm với các model mà không phải mất công tạo HTTP request và liên quan đến controller hay view.

Nhắc đến console, dưới đây là một vài lệnh tắt tiện lợi mà bạn có thể sử dụng:

- `rails c` (viết tắt của `rails console`)
- `rails s` (viết tắt của `rails server`)
- `rails g ...` (viết tắt của `rails generate ...`)

Bạn cứ dùng cách nào thấy thoải mái hơn. Trong các bài học sau, chúng tôi vẫn sẽ tham chiếu đến các lệnh đầy đủ để dễ theo dõi.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bắt đầu

   1. Cũng như trong phần khởi động, hãy lập kế hoạch về các model dữ liệu cần có để user có thể sử dụng trang web (hiện chưa cần lo về đăng nhập/đăng xuất hay bảo mật mật khẩu), gửi liên kết ("post") và bình luận trên liên kết. User KHÔNG cần bình luận được trên bình luận... mỗi bình luận đều tham chiếu đến một Post.
   1. Tạo một ứng dụng rails mới từ dòng lệnh (`$ rails new micro-reddit`) và mở ứng dụng đó. Chúng ta sẽ dùng cơ sở dữ liệu SQLite3 mặc định nên bạn không cần thay đổi gì ở phần này.
   1. Tạo model User và điền vào migration để có các cột bạn muốn.
   1. Chạy migration bằng `$ rails db:migrate`. Bạn có thể dùng `$ rails db:rollback` nếu nhận ra mình đã quên điều gì đó, hoặc chỉ cần tạo một migration mới để sửa (có thể sẽ dùng các lệnh `#add_column`, `#remove_column` hoặc `#change_column`). Xem [Tài liệu Rails API](http://api.rubyonrails.org/classes/ActiveRecord/Migration.html) để biết chi tiết về cú pháp và các method hiện có.

1. Thử nghiệm với validation

   1. Mở `$ rails console` trong một tab mới (có thể dùng `rails c` cho ngắn). Hãy thử yêu cầu tất cả user bằng `> User.all`. Bạn sẽ nhận lại một mảng rỗng (chưa có user nào!). Bây giờ tạo một user mới trống và lưu nó vào một biến bằng `> u = User.new`. User này đã được tạo trong vùng nhớ của Ruby nhưng chưa được lưu vào cơ sở dữ liệu. Hãy nhớ rằng nếu bạn dùng method `#create` thay vì method `#new`, nó sẽ ngay lập tức thử lưu user mới. Thay vào đó, giờ chúng ta có thể thử nghiệm với user này.
   1. Kiểm tra xem user mới có thực sự hợp lệ không (ví dụ: nếu thử thì có lưu được không?). `> u.valid?` sẽ chạy tất cả validation. Kết quả là `true`... bất ngờ nhỉ! Chúng ta chưa viết validation nào nên kết quả như vậy là đúng dự đoán. Nhưng đây cũng là một vấn đề, vì chúng ta không muốn có những user đi lại trong hệ thống với username trống.
   1. Triển khai các validation cho user mà bạn đã nghĩ đến ở bước đầu tiên trong file `app/models/user.rb`. Chúng có thể bao gồm giới hạn về độ dài username, yêu cầu username phải hiện diện (nếu không, bạn có thể có user không có username!) và username phải là duy nhất.
   1. Tải lại console bằng `> reload!`. Bạn sẽ cần làm vậy mỗi khi thay đổi ứng dụng để console có thể tải phiên bản hiện tại. Nếu vẫn có vẻ không hoạt động, hãy `> quit` để thoát rồi khởi động lại (đôi khi `#reload!` không thực sự có tác dụng). Tạo một user mới khác nhưng chưa lưu bằng `> u2 = User.new`. Chạy lại `> u2.valid?` để thực hiện validation và kết quả sẽ là false. Tốt.
   1. Làm thế nào để biết có lỗi gì? Rails rất hữu ích vì thực sự gắn các thông báo lỗi trực tiếp vào đối tượng user khi validation thất bại, để bạn có thể đọc chúng bằng method `#errors`. Hãy thử `> u2.errors` để xem lỗi hoặc tốt hơn, `> u2.errors.full_messages` để trả về một mảng thông báo dễ đọc. Nếu bạn viết thông báo tùy chỉnh trong validation, chúng cũng sẽ xuất hiện ở đây.
   1. Tạo một user thực sự có thể lưu bằng `> u3 = User.new(your_attributes_here)` và chạy validation. Kết quả phải là true. Hãy lưu user bằng method `#save` để bạn có user đầu tiên trong cơ sở dữ liệu.

1. Thử nghiệm với association

   1. Tạo model Post dựa trên kế hoạch dữ liệu ở bước đầu tiên, migrate cơ sở dữ liệu và thêm validation cho model.
   1. Kiểm tra validation từ console, nhớ reload hoặc khởi động lại console giữa các lần thay đổi.
   1. Bây giờ hãy thiết lập association giữa các model User và Post. Bạn có nhớ đưa cột khóa ngoại (`user_id`) vào bảng posts không? Nếu chưa, bạn chỉ cần thêm một migration mới (`$ rails generate migration yourmigrationname`) và dùng method `#add_column` đã đề cập ở trên.
   1. Nếu đã thiết lập association đúng cách, bạn sẽ có thể dùng thêm một số method trong console, bao gồm tìm Posts của một User và tìm User của một Post. Trước tiên hãy thử tìm Posts của User duy nhất — `> User.first.posts`. Kết quả phải là một mảng rỗng vì bạn chưa tạo post nào, nhưng không được phát sinh lỗi.
   1. Tạo (nhưng chưa lưu) một post mới từ console, đặt tên là `p1`, chẳng hạn như `> p1 = Post.new(your_attributes_here)`. Đừng quên đưa ID của user vào trường `user_id`!
   1. Bây giờ hãy tạo một post khác bằng association với user — thay `#new` bằng `#build` và đi qua association — `p2 = User.first.posts.build`. Chưa cần điền trường nào. Kiểm tra đối tượng đã tạo và bạn sẽ thấy trường ID đã được tự động điền, hay thật! Đây là một thủ thuật thú vị mà bạn sẽ học trong bài về association.
   1. Lưu post mới ban đầu `p1` để user của bạn chính thức viết được một bài. Kiểm tra phía còn lại của association bằng cách thử `> Post.first.user`, kết quả phải trả về chính đối tượng User ban đầu có ID mà bạn đã chỉ đến khi tạo post. Mọi thứ đã khép kín!

1. Thêm bình luận

   1. Bây giờ bạn đã có một User và một Post được liên kết với nhau. Model Comment sẽ khá giống model Post nhưng không chỉ liên quan đến post là "parent" của nó mà còn liên quan đến user đã viết bình luận. Hãy thiết lập migration và migrate cơ sở dữ liệu cho model Comment.
   1. Như trước, hãy thêm validation vào model và kiểm tra chúng trong console (refresh console!). Hãy bảo đảm bạn yêu cầu hai khóa ngoại (cho posts và users) phải được gửi lên, nếu không bạn có thể tạo ra một bình luận mồ côi. Bạn không được lưu một Comment không hợp lệ và phải có thể lưu một Comment hợp lệ.
   1. Tạo user thứ hai và tạo một bình luận mới thể hiện việc user này bình luận trên post của user đầu tiên.
   1. Như trước, hãy thêm các association cần thiết giữa users, posts và comments. Bạn cần thực hiện thành công các method sau trong console (giả sử user thứ hai có ID là 2):
      1. `> u2 = User.find(2)`
      1. `> c1 = u2.comments.first` phải trả về bình luận của user đó. `#comments` trả về một mảng các bình luận, vì vậy cần dùng `#first` để thực sự lấy được bình luận.
      1. `> c1.user` phải trả về User là tác giả của bình luận đó (`u2`).
      1. `> p1 = Post.first`
      1. `> p1.comments.first` phải trả về bình luận `c1`.
      1. `> c1.post` phải trả về post `p1`.

Nếu bất kỳ thao tác nào không hoạt động, hãy kiểm tra lại association. Đôi khi thông báo lỗi có thể gợi ý cách thiết lập các association đó.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [Rails API](http://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html) có tài liệu tốt về những nội dung này, bên cạnh Edge Guides mà bạn đã đọc. Thông thường, tìm kiếm trên Google trang API phù hợp sẽ dễ hơn là điều hướng trên trang web, ví dụ: "rails api has_many".
- [Trang JumpstartLab về Quan hệ giữa các Model](https://web.archive.org/web/20231205162005/http://tutorials.jumpstartlab.com/topics/models/relationships.html) có một số thông tin đặc thù hữu ích về cách tiếp cận việc tối ưu các association phổ biến.