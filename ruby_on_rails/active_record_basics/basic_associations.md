### Giới thiệu

Trong các phần về cơ sở dữ liệu, bạn đã học cách một cơ sở dữ liệu quan hệ như SQLite3 hoặc PostgreSQL cho phép liên kết hai bảng với nhau bằng khóa chính của chúng — khóa của một bảng tham chiếu đến bảng khác được gọi là khóa ngoại. Sức mạnh thực sự của cơ sở dữ liệu quan hệ nằm ở việc chúng cho phép bạn tận dụng những, ừm, mối quan hệ này. Bạn muốn lấy tất cả bài đăng blog của người dùng đầu tiên? Hãy thử `User.first.posts`. Đơn giản vậy thôi. Chức năng đó không tự nhiên có sẵn — bạn cần nói cho Rails biết rằng các bài đăng thực sự thuộc về một người dùng. Việc này được thực hiện bằng *các liên kết*.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Liên kết là gì.
- Cách chỉ định các liên kết.
- Tầm quan trọng của việc thiết lập mô hình dữ liệu và cách thực hiện việc đó.
- Quan hệ một-nhiều.
- Quan hệ nhiều-nhiều.

### Liên kết là gì?

Liên kết là cách Rails thiết lập mối quan hệ giữa các bảng. Về mặt khái niệm, việc một Post thuộc về một User là điều hợp lý. Việc khai báo liên kết cũng giúp đơn giản hóa một số thao tác, chẳng hạn như thêm hoặc xóa dữ liệu. Ví dụ, sau khi đã khai báo một liên kết, bạn có thể trực tiếp và dễ hiểu thêm một bài đăng mới bằng `@post = @user.posts.create(title: "New Post Title", content: "Content of the new post")`. Hoặc nếu quan tâm đến các bài đăng của một người dùng, chúng ta chỉ cần truy vấn bảng posts để lấy tất cả bài đăng liên kết ngược về ID của người dùng đó bằng `User.find(user_id).posts`. Ngoài ra, liên kết là cách Rails trừu tượng hóa các mối quan hệ khóa ngoại. Ở cấp độ bảng cơ sở dữ liệu, mỗi hàng trong bảng posts sẽ có một cột `user_id` cho biết người dùng nào “sở hữu” bài đăng đó. Bảng users không cần biết đến các bài đăng — suy cho cùng, một người dùng có thể có vô số bài đăng.

### Cách chỉ định một liên kết

Ví dụ về posts-user của chúng ta có một liên kết mang tên rất phù hợp là “has many / belongs to”. Một User `has_many` đối tượng Post được liên kết với nó, còn một Post `belongs_to` một User duy nhất. Rails giúp việc chỉ định các mối quan hệ này trở nên rất dễ dàng. Bạn có thể thiết lập liên kết giữa các model bằng cách định nghĩa liên kết trong các tệp model của mình. Các liên kết trong mã nguồn của bạn sẽ có dạng như sau:

Trong `app/models/user.rb`:

```ruby
class User < ApplicationRecord
  has_many :posts
end
```

Trong `app/models/post.rb`:

```ruby
class Post < ApplicationRecord
  belongs_to :user
end
```

### Thiết lập mô hình dữ liệu

Bước đầu tiên để hiểu nội dung này chỉ đơn giản là suy nghĩ về những loại mối quan hệ khác nhau có thể tồn tại. Hãy nhớ rằng một nửa công sức để thiết lập ứng dụng nằm ở việc hiểu các mối quan hệ dữ liệu của bạn sẽ trông như thế nào, vì vậy hãy suy nghĩ kỹ và tiếp tục cố gắng ngay cả khi mọi thứ trở nên khó hiểu. Nếu đầu óc bạn đang hơi quá tải, hãy bắt đầu từ thế giới thực và đừng nghĩ ở cấp độ cơ sở dữ liệu — hãy nhớ rằng tất cả những điều này là nỗ lực phản ánh các kiểu quan hệ có thể xảy ra trong thế giới thực.

### Quan hệ một-nhiều

Quan hệ `has_many` / `belongs_to`, hay quan hệ “một-nhiều”, khá phổ biến và thường dễ hình dung nhất khi nghĩ đến các đối tượng thực tế... một Child có thể có nhiều đối tượng Marble, mỗi đối tượng trong số đó thuộc về Child đó. Tuy nhiên, quan hệ này cũng áp dụng trong những trường hợp kém trực quan hơn một chút, chẳng hạn một đối tượng duy nhất `belongs_to` nhiều đối tượng khác. Ví dụ có thể là một FranchiseLocation của McDonalds, vừa `belongs_to` Corporation McDonalds vừa có thể `belongs_to` City San Francisco.

Rõ ràng FranchiseLocation nên thuộc về công ty mẹ, nhưng tại sao nó cũng thuộc về một City? Thường sẽ dễ nghĩ theo chiều ngược lại hơn — một City chắc chắn có thể có nhiều đối tượng FranchiseLocation. Miễn là một FranchiseLocation chỉ có thể nằm trong một thành phố duy nhất, về bản chất nó “belongs_to” thành phố đó theo cách Rails mô tả.

Một điểm phân biệt quan trọng là chúng ta không nói đến việc một User *hiện đang* có bao nhiêu đối tượng Post, hay một City *hiện đang* có bao nhiêu đối tượng FranchiseLocation; chúng ta đang cố mô hình hóa số lượng mà chúng *CÓ THỂ* có trong toàn bộ vòng đời của ứng dụng. Nếu User hiện chỉ có một Post, bạn có thể tham chiếu ID của bài đăng đó trong bảng user. Nhưng khi người dùng có thêm một Post khác, bạn sẽ phải tạo thêm một cột trong bảng để chứa ID đó. Rồi thêm một cột nữa. Và thêm nữa. Điều đó hoàn toàn vô lý... vì vậy chúng ta nói User `has_many :posts` và để bảng Posts tham chiếu đến ID của User, bởi một bài đăng sẽ chỉ luôn gắn với một User (giả sử bài đăng chỉ có một tác giả).

### Quan hệ nhiều-nhiều

Một quan hệ phổ biến khác là quan hệ nhiều-nhiều, trong thuật ngữ Rails cũng có thể gọi là `has_and_belongs_to_many`. Quan hệ này thường xuất hiện trong các mối quan hệ thực tế — một Human có thể có nhiều đối tượng Dog yêu thích, và mỗi Dog cũng có thể có nhiều đối tượng Human yêu thích. Trong trường hợp này, bạn sẽ chỉ định những Dog nào là các đối tượng yêu thích của mình như thế nào? Thực tế, bạn cần tạo thêm một bảng khác (bảng nối, hay bảng “through”) để chuyên theo dõi tất cả các mối quan hệ đó. Khi mới học, khái niệm này hơi khó hiểu, nhưng sau một thời gian làm quen, nó sẽ trở nên hoàn toàn tự nhiên.

### Kết luận

Chẳng bao lâu nữa, bạn sẽ bắt đầu nhìn thế giới xung quanh dưới góc độ những mối quan hệ này (nếu bạn không nghỉ giải lao đủ). Sức mạnh thực sự của chúng xuất hiện khi bạn cần sử dụng chúng — khi muốn truy xuất dữ liệu về tất cả các đối tượng được liên kết với một đối tượng khác. Bạn muốn xem danh sách tất cả người theo dõi X (trước đây có tên là Twitter) của mình? Bạn muốn đếm tất cả bạn học thời trung học hiện đang sống cùng thành phố với mình? Bạn muốn xem tất cả bình luận mà một người dùng đã để lại trên dòng thời gian của người dùng khác? Tất cả những việc này tương đối đơn giản và trực quan một khi bạn đã thiết lập các mối quan hệ dữ liệu phù hợp. Vì vậy, hãy tập trung hiểu những mối quan hệ này.

Active Record là phần mạnh mẽ nhất của Rails, đồng thời cũng là một trong những phần khó nắm bắt nhất. Bạn cần có khả năng chuyển thế giới thực thành các bảng cơ sở dữ liệu, và việc đó cần một khoảng thời gian để trở nên quen thuộc. Những khái niệm khó nhất đối với người mới bắt đầu thường là các liên kết, và bạn sẽ học về chúng trong những bài học sắp tới.

Cách dễ nhất là bắt đầu suy nghĩ về các mối quan hệ cụ thể trong thế giới thực, rồi chuyển chúng thành các liên kết Active Record. Khi đã thấy thoải mái với việc model nào `has_many` model nào khác và thực sự ai `belongs_to` ai, bạn có thể bắt đầu mô hình hóa những khái niệm trừu tượng hơn, chẳng hạn lời mời tham gia sự kiện và việc chấp nhận lời mời — những thứ không trực quan bằng Child và các viên bi của nó (ví dụ chúng ta đã dùng ở trên).

Tất cả đều cần luyện tập, nên từ đây trở đi, các dự án sẽ yêu cầu bạn suy nghĩ về cách tổ chức model trước khi bắt đầu. Dành vài phút suy nghĩ trước về các mối quan hệ là điều thiết yếu để khởi đầu đúng hướng khi bạn bắt tay viết mã.

### Bài tập

Nếu bạn là một người bình thường, có lẽ hiện giờ bạn đang ở đâu đó giữa “hả?” và “Tôi ghét bạn, đừng dạy tôi nữa”. Hãy kiên trì; mục tiêu ở đây là giúp bạn suy nghĩ về cách mô hình hóa các mối quan hệ và làm quen với chúng. Dự án sẽ cho bạn cơ hội thực sự xây dựng những gì đã học, và mọi thứ sẽ dễ hiểu hơn nhiều sau khi bạn có cơ hội đó.

<div class="lesson-content__panel" markdown="1">

1. Đọc phần đầu của [chương Associations trong Rails Guides](http://guides.rubyonrails.org/association_basics.html), cho đến hết mục 3.2. Mọi nội dung sau đó có thể để dành cho sau... điều quan trọng là bạn đã thấy các mối quan hệ và cách thiết lập chúng.

</div>