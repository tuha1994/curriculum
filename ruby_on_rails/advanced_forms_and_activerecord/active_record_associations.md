### Giới thiệu

Bạn hẳn đã làm quen phần nào với association, đặc biệt là các dạng cơ bản `has_one`, `has_many` và `belongs_to`. Cho đến lúc này, có lẽ bạn chủ yếu dùng các association này để lấy ra những tập hợp đối tượng như các bài đăng của một người dùng (`user.posts`). Rails còn cho phép bạn làm rất nhiều việc tiện dụng khác với association. Phần ngắn này sẽ giới thiệu một số phương thức hữu ích hơn đi kèm với association.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Cách Rails thường biết nên sử dụng bảng và foreign key nào khi bạn có một association (ví dụ: `User.first.posts`).
- Chỉ định tùy chọn `:class_name` trong một association.
- Chỉ định tùy chọn `:foreign_key`.
- Chỉ định tùy chọn `:source`.
- Polymorphic association và khi nào bạn nên dùng chúng.
- Hai cách dùng association để tạo một đối tượng mới thay vì chỉ gọi `YourObject.new`.
- Khi nào và vì sao việc dùng association để tạo đối tượng mới có thể hữu ích hoặc được ưu tiên.
- Cách tự động hủy các đối tượng liên kết khi một đối tượng bị xóa.
- Cách thiết lập self-association, chẳng hạn Users theo dõi Users.

### Kiến thức cơ bản

Nếu bạn vẫn chưa nắm chắc association cơ bản, trước hết hãy quay lại xem phần Associations trong [bài học Basic Active Record](https://www.theodinproject.com/lessons/ruby-on-rails-active-record-basics). Phần này chỉ nhằm nhắc lại một số kiến thức cơ bản mà có thể bạn chưa từng được tiếp cận.

#### Foreign key và tên class

Khi bạn tạo một association, Rails đưa ra hai giả định chính -- thứ nhất, class của model mà association trỏ tới được suy ra trực tiếp từ tên association; thứ hai, foreign key trong bất kỳ quan hệ `belongs_to` nào sẽ có tên là `yourassociationname_id`. Bất cứ khi nào bạn đi lệch khỏi các mặc định này, bạn chỉ cần cho Rails biết nên tìm loại class nào và sử dụng foreign key nào.

Một trường hợp rất đơn giản là một User có thể tạo nhiều Post cho một blog:

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base
    has_many :posts
  end
```

```ruby
  # app/models/post.rb
  class Post < ActiveRecord::Base
    belongs_to :user
  end
```

Vì vậy, bạn có thể yêu cầu người dùng đầu tiên trả về tất cả bài đăng của cô ấy bằng `User.first.posts`, hoặc yêu cầu bài đăng đầu tiên trả về user tác giả của nó bằng `Post.first.user`. Rails biết phải tìm foreign key tên là `user_id` trong bảng Posts. Nếu bạn yêu cầu `Post.first.user`, Rails sẽ tìm trong bảng Users User có ID tương ứng với cột `user_id` trong bảng Posts. Mọi việc đều ổn khi tên association tương ứng trực tiếp với tên model và bảng.

Nhưng nếu bạn muốn bài đăng thuộc về hai loại user -- "author" và "editor" thì sao? Trong trường hợp này, bạn sẽ cần hai foreign key riêng trong bảng Posts, có lẽ một cột tên `author_id` và một cột khác tên `editor_id`. Làm thế nào để nói cho Rails biết mỗi foreign key này thực sự trỏ tới một User (để Rails biết phải tìm chúng trong bảng Users)? Chỉ cần chỉ định class mà association của bạn sẽ trỏ tới bằng tùy chọn có tên rất phù hợp là `:class_name`:

```ruby
  # app/models/post.rb
  class Post < ActiveRecord::Base
    belongs_to :author, class_name: "User"
    belongs_to :editor, class_name: "User"
  end
```

Trong trường hợp này, Rails sẽ tự động tìm foreign key được đặt tên theo association, ví dụ `author_id` hoặc `editor_id`, trong bảng Posts.

Nếu bạn đặt tên association khác hoàn toàn với tên foreign key trong bảng, bạn cũng cần nói cho Rails biết điều đó. Điều này sẽ trở nên rõ ràng nếu bạn suy nghĩ về mối quan hệ này từ phía User. User sẽ có một số bài đăng mà cô ấy là author và những bài khác mà cô ấy là editor. Bạn cũng sẽ cần đổi tên association ở phía User để mọi thứ thật rõ ràng, chẳng hạn tách `authored_posts` khỏi `edited_posts`.

Nhưng lúc này Rails hoàn toàn không biết phải tìm ở đâu và tìm gì. Theo mặc định, nếu bạn yêu cầu `User.first.authored_posts`, nó sẽ tìm trong bảng `authored_posts` một foreign key tên `user_id` (cả hai đều không tồn tại). Để trỏ tới đúng bảng, một lần nữa chúng ta cần chỉ định `:class_name`; và để dùng đúng foreign key, chúng ta cần chỉ định `:foreign_key` phù hợp. Ví dụ:

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base
    has_many :authored_posts, foreign_key: "author_id", class_name: "Post"
    has_many :edited_posts, foreign_key: "editor_id", class_name: "Post"
  end
```

Ý chính là giả định rằng Rails đang tìm foreign key được đặt tên theo association trong bảng cũng được đặt tên theo association. Nếu bất kỳ tên nào trong số này không đúng vì association hoặc foreign key được đặt tên sáng tạo, bạn cần chỉ định chúng. Đây là cách khá phổ biến để làm cho association dễ đọc hơn.

#### Source (nguồn)

Giờ đã rõ rằng bạn cần cho Rails biết khi mình đặt tên sáng tạo cho association hoặc foreign key, chúng ta nên chỉ ra rằng sẽ có thêm một bước nếu bạn dùng association `has_many :through` được đặt tên sáng tạo. Hãy nhớ rằng association has-many-through là nơi bạn tạo một "bảng trung gian" đóng vai trò kết nối giữa hai model có quan hệ nhiều-nhiều.

Ví dụ, có thể chúng ta thay đổi ví dụ trên để một Post thực sự có nhiều Author (nhưng vẫn chỉ có một editor). Chúng ta sẽ cần tạo một bảng mới, gọi là `post_authorings`. `post_authorings` nối hai model này với nhau và chứa các cột `authored_post_id` và `post_author_id`. Có lẽ bạn đã hình dung được vấn đề -- chúng ta đã đặt tên cho foreign key mô tả và hữu ích hơn chỉ `post_id` và `user_id`, nhưng sẽ phải thông báo cho Rails về thay đổi này. Các model của chúng ta trông như sau:

```ruby
  # app/models/post.rb
  class Post < ActiveRecord::Base
    has_many :post_authorings, foreign_key: :authored_post_id
    has_many :authors, through: :post_authorings, source: :post_author
    belongs_to :editor, class_name: "User"
  end
```

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base
    has_many :post_authorings, foreign_key: :post_author_id
    has_many :authored_posts, through: :post_authorings
    has_many :edited_posts, foreign_key: :editor_id, class_name: "Post"
  end
```

```ruby
  # app/models/post_authoring.rb
  class PostAuthoring < ActiveRecord::Base
    belongs_to :post_author, class_name: "User"
    belongs_to :authored_post, class_name: "Post"
  end
```

Và mô hình dữ liệu của chúng ta trông như sau:

| **users**  |            |
| ---------- | ---------- |
| name       | *string*   |
| created_at | *datetime* |
| updated_at | *datetime* |

| **posts**  |            |
| -----------| ---------- |
| content    | *text*     |
| editor_id  | *integer*  |
| created_at | *datetime* |
| updated_at | *datetime* |

| **post_authorings**  |            |
| -------------------- | ---------- |
| authored_post_id     | *integer*  |
| post_author_id       | *integer*  |
| created_at           | *datetime* |
| updated_at           | *datetime* |

Điều quan trọng cần lưu ý là với association has-many-through, Rails sử dụng *tên của association trong bảng trung gian* để xác định foreign key và tên bảng cần truy cập. Nếu tên đó không theo quy tắc thông thường, bạn sẽ dùng tùy chọn `:source` để chỉ định association nào thực sự trỏ tới nơi chúng ta muốn đi. Bạn có thể nghĩ về `:source` như `:class_name`, nhưng dành cho các association trong "bảng trung gian".

Minh họa cách Rails đang làm việc có thể sẽ hữu ích. Trong ví dụ trên, nếu bạn yêu cầu `Post.first.authors`, Rails sẽ "suy nghĩ" đại khái như sau:

1. Bạn vừa gọi association "authors" trên model Post
1. Để lấy "authors", trước tiên chúng ta cần đi qua association `post_authorings` để tới đó.
1. Khi đã ở model `PostAuthoring`, để đi tới `author`, chúng ta cần dùng association `:belongs_to` và nó sẽ có tên là `post_author`. Chúng ta biết điều này vì đã dùng tùy chọn `:source`. Nếu không dùng tùy chọn `:source` trong association has-many-through ban đầu, chúng ta sẽ tìm `belongs_to :author` thay thế.
1. Bây giờ chúng ta đã có mọi thông tin cần thiết để tạo các phép nối SQL và lấy danh sách author của bài đăng.

Nghe có vẻ hơi kỳ quặc nhưng thực ra chỉ là cùng logic như trước -- nếu Rails không thể biết dựa trên các giả định của nó rằng bạn định dùng association, tên class hoặc foreign key nào, bạn cần tự chỉ định chúng bằng `:source`, `:foreign_key` hoặc `:class_name`. Cần luyện tập một chút nhưng bạn sẽ nắm được. Thông thường, bạn biết có vấn đề khi gặp các thông báo lỗi dạng `ActiveRecord::StatementInvalid: SQLite3::SQLException: no such column`.

Khi đã hiểu ra mọi thứ, bạn vẫn nên xem output máy chủ Rails để biết những phép nối nào đang được thực hiện nhằm xây dựng truy vấn SQL. Đây là một cửa sổ tuyệt vời cho thấy association đang làm gì ở phía sau (vì rốt cuộc, mọi thứ đều xoay quanh việc tìm ra truy vấn SQL đúng để chạy).

### Association polymorphic

Chúng ta sẽ trình bày polymorphism ở đây nhưng nếu đầu bạn thực sự quay cuồng vì các khái niệm khác, cứ lướt qua phần này. Hãy xem đây là một "chủ đề bổ sung" thay vì chủ đề cốt lõi như foreign key, tên class và source.

Polymorphic association ban đầu có thể hơi khó hiểu và không quá phổ biến, nhưng rất phù hợp với trường hợp sử dụng của nó. Chúng dùng một từ lớn để mô tả một khái niệm khá đơn giản -- nếu bạn có một model duy nhất có thể thuộc về nhiều loại model khác nhau thì sao? Ví dụ, hãy tưởng tượng bạn đang xây dựng một dịch vụ như Facebook, nơi user có thể bình luận vào bất kỳ loại nội dung nào do user khác đăng (như văn bản, ảnh, hình). Làm thế nào để cho phép bình luận lên tất cả các loại đối tượng khác nhau này chỉ bằng một model Comment duy nhất?

Trong tình huống thông thường, comment sẽ `belongs_to` một Post, Picture hoặc Video (hay bất kỳ thứ gì bạn đang bình luận). Bạn sẽ có một foreign key kiểu như `post_id` trong bảng Comments. Nhưng nếu muốn có thể bình luận lên nhiều loại nội dung, chúng ta cần tìm một cách khác để xử lý foreign key, bởi vì một foreign key duy nhất có thể trỏ tới post, image, video, v.v. và chúng ta không biết nó trỏ tới cái nào... điều đó thật mơ hồ. Bạn có thể tạo một cột khác cho từng loại, ví dụ `post_id`, `image_id`, `video_id`, nhưng cách này cực kỳ thiếu thanh lịch và bị hardcode (hãy tưởng tượng nếu có 100 loại bài đăng khác nhau mà chúng ta muốn cho phép bình luận!). Chúng ta cần giữ lại một cột foreign key duy nhất.

Chúng ta giải quyết việc này bằng cách lưu không chỉ foreign key **id**, mà còn một tham chiếu cho biết nó tương ứng với **loại** model nào. Nhờ vậy, mỗi khi muốn lấy một comment, chỉ cần chỉ định nó thuộc về loại đối tượng nào thì yêu cầu của bạn không còn mơ hồ. Lưu ý rằng Rails thực hiện việc này giúp bạn ở phía sau, miễn là nó biết bạn đang làm việc với một polymorphic association.

Chúng ta phải đặt tên foreign key hơi khác so với trường hợp thông thường vì không rõ nó đang tham chiếu tới model nào và bạn không thể chỉ dùng `post_id` hay `picture_id`. Một quy ước là nghĩ ra một thuật ngữ trừu tượng cho loại hành động bạn đang thực hiện và dùng thuật ngữ đó để đặt tên association. Trong trường hợp này, chúng ta đang bình luận lên các đối tượng nên có thể gọi foreign key là `"commentable"`. Bạn sẽ thấy quy ước `*able` được dùng khá nhiều. Vì vậy, migration cho model đó có thể trông như sau:

```ruby
  class CreateComments < ActiveRecord::Migration
    def change
      create_table :comments do |t|
        t.string :title
        t.text :content
        t.integer :commentable_id
        t.string :commentable_type
        t.timestamps
      end
    end
  end
```

"Commentable" cũng sẽ được dùng để chỉ các association. Bạn cần cho model Comment biết rằng nó thực sự là polymorphic để Rails biết phải kiểm tra thêm cột `commentable_type` khi sử dụng nó. Việc này được thực hiện như sau:

```ruby
  # app/models/comment.rb
  class Comment < ActiveRecord::Base
    belongs_to :commentable, polymorphic: true
  end
```

Ở phía còn lại của association, bạn chỉ cần xử lý comment như bất kỳ association nào khác (vốn tình cờ có một tên không theo chuẩn). Bạn chỉ cần chỉ định association trên mọi model `has_many` comment. Điểm cần lưu ý duy nhất là vì đang dùng tên "commentable", bạn cần chỉ định nó trong một alias giống như khi bất kỳ association nào khác có tên không theo chuẩn:

```ruby
  # app/models/post.rb
  class Post < ActiveRecord::Base
    has_many :comments, as: :commentable
  end

  # app/models/picture.rb
  class Picture < ActiveRecord::Base
    has_many :comments, as: :commentable
  end
```

Rails sẽ lo phần còn lại. Mỗi khi bạn yêu cầu một Picture trả về tất cả comment của nó (`Picture.first.comments`), Rails sẽ chỉ trả về những comment thuộc về picture đó mà bạn không cần lo lắng thêm điều gì.

### Self join (liên kết với chính nó)

Nhiều khi bạn có các mối quan hệ giữa những model cùng loại, chẳng hạn user có thể theo dõi user khác. Trong trường hợp này, bạn cần chỉ định cả hai association trong model User nhưng đặt tên chúng khác nhau. Bạn sẽ cần chỉ định trong association `has_many` tên của `foreign_key` là gì:

```ruby
  class Employee < ActiveRecord::Base
    has_many :subordinates, class_name: "Employee",
                            foreign_key: "manager_id"

    belongs_to :manager, class_name: "Employee"
  end
```

### Các phương thức tiện dụng

Như đã đề cập trong phần giới thiệu, association cho bạn quyền truy cập vào một số thủ thuật hữu ích mà có thể bạn chưa nghĩ tới.

#### Tạo các đối tượng association

Có một vài lối tắt để tạo các đối tượng association mới. Cách đầu tiên là gọi `#new` hoặc `#create` trên association để tự động điền foreign key. Ví dụ, nếu một User `has_many` Post và Post `belongs_to` một User:

```bash
  # Long version:
  > user = User.first
  > post = Post.create(title: "sample", user_id: user.id)

  # Shorter version:
  > user = User.first
  > user.posts.create(title: "sample")
```

Một thủ thuật tiện dụng khác là tạo một đối tượng mới rồi dùng toán tử shovel để thêm nó vào association. Đây chỉ là một trong những cách **association đôi khi hoạt động như mảng**:

```bash
  > user = User.create(name: "foobar")
  > post = Post.new(title: "sample")
  > user.posts << post
```

Thao tác này sẽ lưu post vào cơ sở dữ liệu với foreign key của User đó. Về cơ bản, nó tương đương với việc gọi:

```bash
  > post.user_id = user.id
  > post.save
```

Nếu thực sự muốn, bạn có thể thay thế toàn bộ association bằng một tập hợp mới bằng cách gán nó bằng tập hợp mới đó:

```bash
  > user = User.first
  > post1 = Post.find(1)
  > post2 = Post.find(2)
  > user.posts = [post1, post2]  # posts added to that user's collection
```

#### Hủy các đối tượng phụ thuộc

Nếu user của bạn đã tạo rất nhiều post rồi quyết định xóa tài khoản, làm thế nào để xóa tất cả post liên kết? Hãy chỉ định tùy chọn `dependent: :destroy` khi khai báo association lần đầu:

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base
    has_many :posts, dependent: :destroy
  end
```

Đây chỉ là tùy chọn phổ biến nhất trong số nhiều tùy chọn bạn có thể chỉ định cho `:dependent`. Nó sẽ chạy phương thức `destroy` trên tất cả đối tượng thuộc về user đó khi user bị hủy.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Rails Guide về Associations](https://guides.rubyonrails.org/association_basics.html). Bắt đầu bằng cách đọc lướt chương 1 và 2 (những chương bạn hẳn đã đọc rồi).
1. Đọc toàn bộ nội dung trong chương 3, 4 và 7.

</div>

#### Kết luận

Trong bài học này, chúng ta đã tìm hiểu một số nội dung nâng cao hơn về association. Association xuất hiện ở khắp nơi trong Rails và cực kỳ hữu ích vì chúng cung cấp cho bạn rất nhiều phương thức mới. Miễn là bạn dừng lại và suy nghĩ về những gì Rails đang giả định khi thiết lập chúng, bạn sẽ có thể tùy chỉnh chúng theo ý muốn mà không gặp quá nhiều khó khăn. Tuy nhiên, luyện tập sẽ giúp bạn hoàn thiện, vì vậy hãy tiếp tục xây dựng các dự án có association và cuối cùng bạn sẽ ghi nhớ được.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải nội dung bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [Ôn lại kiến thức về Rails Associations](https://www.sitepoint.com/brush-up-your-knowledge-of-rails-associations/)
- [Polymorphic Association trong Rails](https://dev.to/adjoa/rails-polymorphic-associations-511n)
- [Tìm hiểu về Polymorphic Association trong Rails](https://web.archive.org/web/20210507031450/https://launchschool.com/blog/understanding-polymorphic-associations-in-rails)
- [RailsCasts #154 về Polymorphic Association](http://railscasts.com/episodes/154-polymorphic-association-revised)
- [Comment với Polymorphic Association](https://gorails.com/episodes/comments-with-polymorphic-associations)
- [RailsCasts Pro #394 STI và Polymorphic Association](http://railscasts.com/episodes/394-sti-and-polymorphic-associations)
