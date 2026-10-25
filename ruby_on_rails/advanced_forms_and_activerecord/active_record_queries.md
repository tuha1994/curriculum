### Giới thiệu

Cho đến nay, chúng ta đã tìm hiểu những kiến thức cốt lõi cần có để xây dựng các truy vấn cơ bản bằng Active Record. Sau khi hoàn thành một vài dự án và làm theo phần hướng dẫn, bạn sẽ đủ tự tin để viết những truy vấn này.

Active Record mạnh hơn nhiều so với việc chỉ thực hiện các thao tác CRUD trên từng bản ghi. Nó cung cấp một giao diện mang phong cách Ruby để thực hiện gần như mọi việc bạn có thể làm bằng các câu lệnh SQL thuần. Bạn có thể chọn ra từng nhóm bản ghi dựa trên tiêu chí cụ thể và sắp xếp chúng theo ý muốn. Bạn có thể tự nối các bảng hoặc truy vấn thông qua các association do Rails thiết lập. Bạn cũng có thể trả về danh sách bản ghi hoặc thực hiện những phép tính cơ bản như đếm và tính trung bình.

Tất cả việc này được thực hiện ở cấp cơ sở dữ liệu, nhanh hơn nhiều so với việc tải toàn bộ một bảng dữ liệu vào các đối tượng Ruby rồi mới phân tích, tách lọc và tính toán.

Trong bài học này, chúng ta sẽ đi sâu vào những khía cạnh thú vị và hữu ích hơn của các truy vấn Active Record. Bạn sẽ tìm hiểu Active Record thực sự trả về gì và cách tùy ý thao tác với các giá trị được trả về. Trên đường đi, bạn cũng sẽ học cách làm cho các truy vấn hiệu quả hơn.

Có khá nhiều tài liệu cần đọc và nội dung cần bao quát, nhưng nhìn chung bài học dựa trên ý tưởng: "bạn làm được gì trong SQL thì cũng có thể làm điều đó trong Active Record". Hai bên phần lớn cũng sử dụng cùng một thuật ngữ. Active Record mở rộng chức năng đó bằng cách cung cấp một bộ phương thức linh hoạt (cùng những khái niệm như Relation), giúp mọi việc thân thiện hơn với người dùng.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Relation của Active Record là gì.
- Đánh giá trì hoãn là gì.
- Những phương thức truy vấn Rails thường dùng.
- Truy vấn N+1 và lý do chúng đáng lo ngại.
- Scope là gì.
- Enum là gì.

### Relation và đánh giá trì hoãn

Sử dụng `User.find(1)` sẽ trả về một đối tượng rõ ràng -- nó sẽ tìm người dùng có ID = 1 và trả đối tượng đó cho bạn dưới dạng một đối tượng Ruby. Tuy nhiên, hành vi này thực ra khá đặc biệt. Phần lớn truy vấn không thực sự trả về một đối tượng Ruby mà chỉ giả lập như vậy. Ví dụ:

```ruby
  User.where(id: 1)
```

có vẻ sẽ trả về một mảng chứa đối tượng User đã được tuần tự hóa, chẳng hạn:

```ruby
  [#<User id: 1, email: "foo@bar.com">]
```

Nhưng hãy thử chạy `User.where(id: 1).class`, bạn sẽ thấy nó không phải là một `Array` mà thực ra là một instance của `ActiveRecord::Relation`. Relation chỉ rất giỏi trong việc trông giống mảng, còn bên trong nó làm được nhiều việc hơn thế.

Các truy vấn Active Record trả về relation để trì hoãn việc thực thi. Về cơ bản, không có lý do gì phải yêu cầu cơ sở dữ liệu thực thi truy vấn trước thời điểm cuối cùng có thể. Nếu bạn hoàn toàn không cần dùng truy vấn đó thì sao? Nếu bạn muốn làm cho nó phức tạp hơn trước khi thực thi thì sao? Relation cho bạn sự linh hoạt ấy và giúp sử dụng hiệu quả hơn khoảng thời gian quý giá của cơ sở dữ liệu.

Relation chỉ được thực thi khi việc biết bên trong nó có gì trở nên thực sự cần thiết. Vì vậy, nếu controller lấy 5 bài viết blog bằng `@posts = Post.limit(5)`, thì thực chất nó đang truyền cho view một relation. Chỉ khi code trong view thực sự gọi một phương thức trên `@posts` (chẳng hạn `@posts.first.title`) thì truy vấn mới chạy và relation mới được lưu trong bộ nhớ như một đối tượng Ruby thật sự.

Hành vi này có thể hơi khó quan sát nếu bạn dùng thứ gì đó như Rails Console (`$ rails console`) để thử nghiệm, vì truy vấn sẽ được chạy ngay trong console: console ngầm gọi một phương thức tương tự `.inspect` trên relation, mà phương thức đó yêu cầu truy vấn phải được thực thi. Tuy vậy, hãy thử xây dựng một truy vấn như ví dụ trên và kiểm tra `#class` của nó... thông thường bạn sẽ nhận được `ActiveRecord::Relation`.

#### Nối chuỗi truy vấn

Relation không chỉ được xây dựng để tăng tốc... chúng còn được xây dựng để linh hoạt. Giả sử bạn muốn lấy 5 bài viết đầu tiên theo thứ tự giảm dần (`Post.limit(5).order(created_at: :desc)`). Vì `#limit` trả về một Relation, `#order` nhận relation đó rồi bổ sung tiêu chí riêng của nó. Bạn có thể nối hàng chục phương thức theo cách này; đến lúc cần thực thi, ActiveRecord và SQL (nếu đó là hệ quản trị cơ sở dữ liệu bạn dùng) sẽ tìm ra cách tối ưu để cấu trúc truy vấn nhằm đạt kết quả mong muốn.

Đây là kiểu hành vi mà bạn thường mặc nhiên mong đợi sẽ hoạt động, và Relation chính là thứ cho phép điều đó xảy ra.

#### Vì sao cần quan tâm?

Bạn nên quan tâm đến việc các truy vấn ActiveRecord thường trả về Relation vì khi viết code và gỡ lỗi, bạn sẽ thường xuyên gặp chúng. Kiến thức này sẽ giúp bạn tự tin nối các phương thức truy vấn để tạo ra những truy vấn phức tạp.

Nếu gặp một Relation nhưng thực sự muốn nó hoạt động như một Array, đôi khi bạn có thể chạy `#to_a` để buộc nó đánh giá truy vấn.

Các phương thức được triển khai trong `ActiveRecord::FinderMethods` KHÔNG trả về các đối tượng `ActiveRecord::Relation`. Các phương thức `#find`, `#find_by`, `#first` và `#last` trả về một bản ghi duy nhất (một instance của model). `#take` trả về một instance model duy nhất hoặc một mảng các instance nếu được truyền tham số. Không giống các phương thức trả về đối tượng `Relation`, những phương thức này sẽ chạy truy vấn SQL ngay lập tức khi được gọi.

### Vượt ra ngoài truy vấn cơ bản

Đến đây, bạn hẳn đã khá quen với những truy vấn dùng để tìm đối tượng. Phần tài liệu tiếp theo sẽ bao quát nền tảng rồi đi sâu hơn một chút so với trước. Có vài khái niệm mới đáng được nhắc đến.

#### Kiểm tra sự tồn tại

Khái niệm mới là cách kiểm tra một đối tượng có thực sự tồn tại hay chưa. Bạn có thể muốn làm điều này trước khi chạy một phương thức phụ thuộc vào việc đối tượng đó đã được lưu hay chưa.

`#exists?` sẽ trả về true/false. `#any?` sẽ trả về true nếu có bản ghi nào khớp tiêu chí đã chỉ định, còn `#many?` sẽ trả về true nếu có nhiều bản ghi khớp tiêu chí đó. Bạn có thể chạy từng phương thức trên model, một Relation, một association hoặc một scope (chúng ta sẽ tìm hiểu sau). Về cơ bản, ở bất kỳ nơi nào bạn nghĩ đến việc sử dụng chúng, rất có thể chúng đều hoạt động:

```ruby
  # From the Guide:
  # via a model
  Post.any?
  Post.many?

  # via a relation
  Post.where(published: true).any?
  Post.where(published: true).many?

  # via an association
  Post.first.categories.any?
  Post.first.categories.many?
```

#### Đối số

Có nhiều cách truyền đối số cho hầu hết các phương thức truy vấn Rails. Thông thường, bạn có thể dùng symbol, string hoặc kết hợp cả hai. Khi có thể, chúng tôi khuyến nghị dùng symbol và hash. Bạn cũng có thể dùng tham số `?` như trong SQL thông thường. Khi không gây mơ hồ (ví dụ bạn không làm việc với nhiều bảng), bạn có thể chọn chỉ rõ tên bảng hoặc không (xem mục số 5 bên dưới). Tất cả các cách sau đều tương đương:

1. `User.where(email: "foo@bar.com")`
1. `User.where("email" => "foo@bar.com")`
1. `User.where("email = 'foo@bar.com'")`
1. `User.where("email = ?", "foo@bar.com")`
1. `User.where("users.email" => "foo@bar.com")`

#### Một số kiến thức truy vấn khác

Các truy vấn rất lớn thực ra có thể được chia thành nhiều truy vấn con để không tiêu tốn quá nhiều tài nguyên hiệu năng. `#find_each` đảm nhiệm việc này. Nguyên tắc cơ bản là nó chia truy vấn thành từng phần, tải và đánh giá phần đầu tiên trước khi chuyển sang phần tiếp theo. Điều này sẽ hữu ích khi tối ưu truy vấn, nhưng ban đầu bạn chưa cần quá lo lắng về nó.

Các truy vấn `#where` khá linh hoạt -- chúng cho phép bạn chỉ định một giá trị chính xác, một khoảng giá trị hoặc nhiều giá trị cần tìm. Nếu biết loại truy vấn mình muốn, bạn gần như có thể đoán được cú pháp phù hợp để thực hiện nó.

Điểm quan trọng cần nhớ là `#find` trả về bản ghi thực, còn `#where` trả về một `ActiveRecord::Relation` về cơ bản hoạt động như một mảng. Vì vậy, nếu dùng `#where` để tìm một bản ghi duy nhất, bạn vẫn phải nhớ truy cập vào "mảng" đó và lấy bản ghi đầu tiên, chẳng hạn `User.where(email: "foo@bar.com")[0]` hoặc `User.where(email: "foo@bar.com").first`.

`#find_by` là một phương thức rất tiện, về cơ bản cho phép bạn tự xây dựng phương thức tìm kiếm của riêng mình. Đây là lựa chọn thay thế cho `#where` (bạn phải thêm một phương thức khác như `#take` hoặc `#first` để lấy kết quả ra khỏi mảng được trả về). Nếu muốn tìm theo email của người dùng, hãy viết `User.find_by(email: 'foo@bar.com')`.

`#select` hẳn khá dễ hiểu với một SQL ninja như bạn -- nó cho phép chọn những cột cần lấy từ các bảng, giống như trong SQL. Để chỉ chọn cột ID của tất cả người dùng, cú pháp rất đơn giản: `User.select(:id)`. Bạn cũng có thể dùng alias như trong SQL, nhưng nên dùng chuỗi thay vì symbol; chẳng hạn `@users = User.select("users.id AS user_id")` sẽ tạo một thuộc tính mới tên là `user_id`, cho phép bạn truy cập `@users.first.user_id`.

### Phép tổng hợp

Cũng như trong SQL, bạn thường muốn nhóm các trường lại với nhau (hoặc "cuộn" các giá trị dưới một tiêu đề). Ví dụ, nhóm các bài viết blog được viết vào một ngày nhất định. Việc này hữu ích nhất khi kết hợp với các phép toán như `#count` hoặc `#max`. Một ví dụ (phức tạp hơn một chút vì liên quan đến việc nối hai bảng) là khi muốn đếm tất cả bài viết blog được phân loại theo từng thẻ. Ta có thể viết như sau:

```bash
  Post.joins(:tags).group("tags.name").count
  # => {"tag1" => 4, "tag2" => 2, "tag3" => 5}
```

### Nối bảng

Khi làm việc với nhiều bảng, bạn thường muốn nối chúng lại với nhau. Các association của Rails thường gánh phần lớn việc thiết lập các phép nối nếu bạn làm việc với instance của một đối tượng, nên có thể ban đầu bạn chưa cần dùng `#join` một cách tường minh.

Nhưng nếu đang chạy những truy vấn như ví dụ nhóm số lượng Bài viết-Thẻ ở trên, bạn sẽ cần dùng phép nối để kết hợp các bảng phù hợp. Bạn cần cẩn thận hơn khi chọn dữ liệu bằng phép nối -- nếu tìm cột `:id`, ta đang hỏi ID của bảng nào? Khi nối bảng, bạn sẽ thấy mình dùng các chuỗi tường minh hơn, chẳng hạn trong ví dụ trên (được chép lại bên dưới), nơi chúng ta chỉ rõ thuộc tính `name` của bảng `tags`:

```bash
  Post.joins(:tags).group("tags.name").count
  # => {"tag1" => 4, "tag2" => 2, "tag3" => 5}
```

### Truy vấn N+1 và eager loading

Nếu muốn ứng dụng đạt hiệu năng ở mức chấp nhận được, bạn nên cố gắng giảm số truy vấn chạy trên cơ sở dữ liệu nhiều nhất có thể. Điều đó có nghĩa là phải xác định trước chính xác mình cần gì rồi xây dựng đúng truy vấn để lấy thứ đó chỉ một lần.

Việc lấy cùng một thông tin nhiều lần cũng không sao... Rails vốn đã lưu kết quả đầu tiên vào bộ nhớ đệm nên không gây ảnh hưởng hiệu năng. Tuy nhiên, có những tình huống bạn buộc `ActiveRecord::Relation` do một truy vấn trả về phải tự thực thi ngay, rồi lại chạy truy vấn trên từng phần tử trong tập hợp. Đó là một lượng truy vấn rất lớn và có thể nhanh chóng khiến ứng dụng chậm như sên.

Vấn đề truy vấn N+1 là trường hợp kinh điển của tình huống này -- bạn lấy tất cả bản ghi người dùng (`User.all`), sau đó lặp qua từng người dùng và gọi một association của họ, chẳng hạn thành phố nơi người dùng sống (`user.city`). Trong ví dụ này, chúng ta giả định có association giữa User và City, trong đó User `belongs_to` một City. Đoạn code có thể như sau:

```ruby
  User.all.each do |user|
    puts user.city
  end
```

Kết quả sẽ là một truy vấn để lấy tất cả người dùng, sau đó thêm một truy vấn cho mỗi người dùng để tìm thành phố thông qua association... tức N truy vấn bổ sung, trong đó N là tổng số người dùng. Vì vậy nó được gọi là vấn đề "N+1". Lưu ý rằng việc chỉ lấy một thuộc tính thông thường của User như `user.name` hoàn toàn ổn... vấn đề xuất hiện vì bạn truy cập xuyên qua association đến City, khiến phải chạy thêm một truy vấn đầy đủ.

Nếu cách tốt nhất để làm ứng dụng nhanh hơn là giảm số lần gọi cơ sở dữ liệu, thì chúng ta vừa làm hỏng mọi thứ khi tạo ra một số lượng truy vấn có thể cực kỳ lớn.

Rails hiểu rõ khó khăn này và cung cấp một giải pháp -- "eager loading". Khi lần đầu lấy danh sách tất cả người dùng, bạn có thể yêu cầu Rails lấy luôn các thành phố (chỉ với một truy vấn bổ sung) và lưu chúng trong bộ nhớ cho đến khi cần dùng. Khi đó `user.city` được xử lý giống như `user.name`... nó không chạy thêm truy vấn. Bí quyết nằm ở phương thức `#includes`.

```ruby
  User.all.includes(:city).each do |user|
    puts user.city
  end
```

Về cơ bản, `#includes` nhận tên của một hoặc nhiều association bạn muốn tải cùng lúc với đối tượng ban đầu rồi đưa chúng vào bộ nhớ. Bạn có thể nối nó với các phương thức khác như mệnh đề `#where` hoặc `#order`.

<div class="lesson-note" markdown="1">

#### Kết quả của các trường eager loading

Xét trên phương diện phát triển, một điều hơi khó chịu là chúng ta chưa tìm ra cách dễ dàng để "nhìn thấy" các trường eager loading khi xem output từ máy chủ Rails. Vì vậy, đừng lo nếu chúng không xuất hiện trong output của máy chủ.

</div>

Phương thức `#pluck`, được đề cập trong Rails Guide, cũng hữu ích gần như vậy. `#pluck` cho phép bỏ qua vài bước trong quy trình lấy nhiều bản ghi, lưu chúng vào bộ nhớ, sau đó lấy một cột cụ thể và đưa cột đó vào một mảng. `#pluck` trả về ngay mảng kết quả:

```ruby
  User.pluck(:name)
  # => ["Foo", "Bar", "Baz", "Jimmy-Bob"]
```

Đây là một cách khác để tăng tốc ứng dụng nếu bạn đã tìm thấy những điểm gây chậm. Tuy vậy, trước tiên hãy loại bỏ các truy vấn N+1.

### Scope

Scope thường bị đánh giá thấp, nhưng cực kỳ hữu ích. Về cơ bản, scope là một chuỗi tùy chỉnh các phương thức ActiveRecord mà bạn có thể gắn vào một Relation hiện có bằng cách gọi tên nó như một phương thức thông thường. Xem ví dụ sẽ dễ hiểu nhất.

Giả sử bạn cho phép người dùng lọc các bài viết blog chỉ lấy những bài được đánh dấu là "important":

```ruby
  # app/models/post.rb
  ...
  scope :important, -> { where(is_important: true) }
  ...

  # app/controllers/posts_controller.rb
  ...
  def index
    if params[:important] == true
      @posts = Post.important.all
    else
      @posts = Post.all
    end
  end
```

Thay vì luôn phải viết lại chuỗi phương thức ActiveRecord đó mỗi khi cần, bạn có thể tạo những scope có tên rõ ràng để chứa toàn bộ logic thành phần. Cách này giảm lặp code và giúp code dễ đọc hơn. Điểm hay nhất là scope trả về Relation... vì vậy bạn có thể nối bao nhiêu scope tùy thích.

Bạn có thể nghĩ: Tại sao dùng scope khi có thể viết một class method để làm điều tương tự? Bạn có thể làm vậy, miễn là class method trả về một Relation (điều này có thể cần suy nghĩ thêm cho các trường hợp biên). Thực tế, class method thường là lựa chọn tốt hơn nếu chuỗi logic của bạn khá phức tạp. Ví dụ trên cũng có thể được giải quyết bằng class method sau:

```ruby
  # app/models/post.rb
  ...
  def self.important
    self.where(is_important: true)
  end
  ...
```

Xem [phần Tài nguyên bổ sung](#additional-resources) để tìm các liên kết đến một số bài viết đào sâu hơn về trường hợp sử dụng của hai cách này.

Bạn cần hiểu hoặc quan tâm đến scope ở mức nào? Trong giai đoạn đầu, có lẽ bạn chưa gặp chúng hoặc chưa thấy lý do nên dùng. Hãy ghi nhớ chúng để dùng khi bắt đầu làm những dự án phức tạp hơn một chút và có thể cần đến chúng.

### Enum

Enum (viết tắt của "enumerations") ánh xạ các giá trị của cột cơ sở dữ liệu, thường được lưu dưới dạng số nguyên, sang một tập hợp tên tượng trưng. Chúng giúp code dễ đọc và bảo trì hơn, đồng thời tăng hiệu năng vì các truy vấn liên quan đến số nguyên nhanh hơn các truy vấn liên quan đến chuỗi.

Enum phù hợp để biểu diễn trạng thái của một thuộc tính có giá trị rời rạc. Ví dụ, một bài viết trên blog có thể ở trạng thái `draft` hoặc `published`. Nó không bao giờ ở giữa hai trạng thái này và cũng không bao giờ ở nhiều hơn một trạng thái tại cùng thời điểm.

#### Cách sử dụng enum

Để triển khai `enum`, chúng ta cần khai báo chúng trong model và thêm một cột để lưu trữ chúng trong bảng cơ sở dữ liệu chứa các instance của model.

Vì enum được lưu dưới dạng số nguyên trong cơ sở dữ liệu và hầu hết schema cơ sở dữ liệu không có kiểu `enum` gốc, chúng ta đặt kiểu dữ liệu của chúng là `integer`:

```bash
# If building a new model, pass `column_name:integer` to the model generator
bin/rails g model Article status:integer

# If adding a column to an existing table:
bin/rails g migration AddStatusToLightSwitch status:integer
```

Enum được khai báo trong class của model dưới dạng một mảng hoặc một hash. Cách dùng hash có một ưu điểm nhỏ: các giá trị số nguyên được ánh xạ tới `enum` không phụ thuộc vào vị trí, và các giá trị lưu trong cơ sở dữ liệu được nêu rõ.

```ruby
# Hash declaration (recommended)
class Article < ApplicationRecord
  belongs_to :author
  enum :status, { draft: 0, published: 1 }
end
```

Bây giờ, khi tạo Article, bạn có thể truyền vào một trạng thái:

```ruby
article = Article.create(status: :draft)
```

Việc định nghĩa enum trên một model cung cấp một số class method trả về các collection:

```ruby
Article.draft
Article.not_draft
Article.published
Article.not_published
```

Về chức năng, chúng tương đương với các [scope](#scopes) sau:

```ruby
scope :draft, -> { where(status: "draft") }
scope :not_draft, -> { where.not(status: "draft")}
scope :published, -> { where(status: "published") }
scope :not_published, -> { where.not(status: "published") }
```

Ngoài ra còn có một số instance method tiện dụng:

```ruby
# Predicates
article.draft?               #=> true
article.published?           #=> false

# Getters
article.status               #=> "draft"

# Setters
article.status = "published" #=> "published"
article.draft!               #=> "draft"
```

### SQL thuần

Đôi khi bạn không thể khiến ActiveRecord làm đúng điều mình muốn. Khi đó, nó cung cấp một giao diện đến SQL thuần để bạn tự nhập truy vấn theo ý muốn. Đây thực sự nên là phương án cuối cùng -- về cơ bản, bạn đang hard-code vào code ứng dụng. Hãy dùng phương thức `#find_by_sql` cho việc này.

### Kết luận

Đây là một lượng kiến thức khá lớn, nhưng bạn hẳn đã có cái nhìn đầy đủ hơn về phạm vi những việc có thể làm với Active Record. Tuy nhiên, ở mức cơ bản nhất, bạn có thể làm gần như mọi việc trong SQL bằng các phương thức truy vấn Active Record. Trong những dự án sau, bạn sẽ có cơ hội sử dụng một số phương thức truy vấn mới này; những phương thức khác sẽ xuất hiện khi bạn tự xây dựng ứng dụng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Kiến thức truy vấn cơ bản

1. Đọc 6 phần đầu của [Rails Guide về truy vấn Active Record](http://guides.rubyonrails.org/active_record_querying.html) để có cái nhìn cơ bản hơn về các hàm truy vấn. Bạn chưa cần quá quan tâm đến batching và `#find_each`.
1. Đọc phần 20 của cùng Rails guide để xem nhanh cách [sử dụng `exists?`, `any?` và `many?`](https://guides.rubyonrails.org/active_record_querying.html#existence-of-objects).
1. Đọc các phần 7 và 21 để tìm hiểu về [hàm nhóm và hàm tổng hợp](https://guides.rubyonrails.org/active_record_querying.html#grouping).
1. Đọc lướt phần 8 để tìm hiểu về [ghi đè điều kiện](https://guides.rubyonrails.org/active_record_querying.html#overriding-conditions).
1. Đọc phần 12 của cùng Rails guide để xem Rails cho phép bạn [nối các bảng với nhau](https://guides.rubyonrails.org/active_record_querying.html#joining-tables) như thế nào.
1. Đọc phần 18 của cùng Rails guide để xem nhanh [các phương thức `find_or_create_by` hữu ích](https://guides.rubyonrails.org/active_record_querying.html#find-or-build-a-new-object).

#### Truy vấn nâng cao

1. Đọc phần 14 trong [Rails Guide về truy vấn](https://guides.rubyonrails.org/active_record_querying.html#scopes) để tìm hiểu về scope. Một lần nữa, bạn không nhất thiết phải ghi nhớ mọi chi tiết về scope, nhưng nên hiểu khái niệm và lúc nào nó hữu ích.
1. Đọc bài [Cách sử dụng Enum trong Rails](https://blog.saeloun.com/2022/01/05/how-to-use-enums-in-rails/).
1. Đọc phần 19 “Tìm bằng SQL” của cùng Rails guide để tìm hiểu cách [dùng SQL trực tiếp để truy vấn](http://guides.rubyonrails.org/active_record_querying.html#finding-by-sql).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu bổ sung.

- [Bài viết SO về việc dùng Scope hay Class Method](http://stackoverflow.com/questions/5899765/activerecord-rails-3-scope-vs-class-method)
- [Platformatec đào sâu hơn về trường hợp sử dụng scope và class method](http://blog.plataformatec.com.br/2013/02/active-record-scopes-vs-class-methods/)
- [RailsCasts #22 về Eager Loading](http://railscasts.com/episodes/22-eager-loading-revised)
- [Giới thiệu về vấn đề N+1](https://www.youtube.com/watch?v=Xr3hZdIwuSw)
- [Vấn đề N+1: Eager Loading với Active Record](https://www.youtube.com/watch?v=wLMRzdOztUY)
- [Vấn đề N+1: Tối ưu số lượng bằng Join và Select tùy chỉnh](https://www.youtube.com/watch?v=rJg3I-leoo4)
- [Tăng tốc ActiveRecord bằng một chút tinh chỉnh](https://blog.codeship.com/speed-up-activerecord/)
- [Một gem hữu ích để phát hiện truy vấn N+1](https://github.com/flyerhzm/bullet)
- [Tài liệu API của ActiveRecord::Enum](https://api.rubyonrails.org/classes/ActiveRecord/Enum.html)
