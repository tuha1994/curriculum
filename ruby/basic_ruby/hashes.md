### Giới thiệu

Giờ bạn đã học về mảng, đã đến lúc tìm hiểu người anh em họ được tăng cường sức mạnh của mảng: **hash**. Hash trong Ruby là những collection dữ liệu nâng cao hơn, tương tự object trong JavaScript và dictionary trong Python nếu bạn quen với các kiểu dữ liệu đó.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học.

- Giải thích hash là gì.
- Mô tả cách tạo một hash mới và cách thêm thông tin vào đó.
- Giải thích key và value là gì.
- Mô tả cách thay đổi và xóa dữ liệu bên trong hash.
- Mô tả cách gộp hai hash.

### Mảng và hash

Bạn đang ngồi trong buồng làm việc, chăm chỉ làm việc (vì bạn chẳng bao giờ mơ đến chuyện lười biếng ở chỗ làm), thì giờ ăn trưa đến. Bạn cần kiếm gì đó ăn, nhưng sẽ yêu cầu đồ ăn bằng cách nào? Trong câu chuyện ví dụ này, bạn có hai lựa chọn: máy bán hàng tự động hoặc một nhà hàng tử tế.

Nếu đến máy bán hàng tự động, bạn sẽ thấy những hàng đồ ăn ngay ngắn, mỗi lựa chọn được gắn một con số. Các nhãn này chính là các index của mảng máy bán hàng. Điều quan trọng cần lưu ý là các index không thể hoán đổi: "12" luôn đứng trước "13" và sau "11". Bạn yêu cầu đồ ăn bằng cách dùng một index để cho máy biết mình muốn gì. Máy hiểu index đó và trả về thứ nằm ở vị trí tương ứng. Mmmmm, còn gì bằng một bữa trưa gồm Flamin' Hot Cheetos và Diet Coke! Dù sao bạn cũng là một lập trình viên mà.

Lựa chọn còn lại là ngồi vào một chiếc bàn phủ khăn trắng đẹp đẽ, nơi một người phục vụ thân thiện sẽ đáp ứng mọi nhu cầu của bạn. Việc đầu tiên họ làm là mang cho bạn thực đơn; với những ai cả đời chỉ ăn đồ từ máy bán hàng tự động, thực đơn này liệt kê mọi lựa chọn món ăn, được gắn tên món như "sublimated artichoke frittata" hoặc "whole pork belly, market acorns, and activated shell bean". Trong hash thực đơn này, tên món được gọi là **key**: các nhãn dùng để xác định những lựa chọn ăn uống của bạn. Đồ ăn mà những tên món đó đại diện là các **value** mà key trỏ đến. Để gọi món, bạn đưa cho người phục vụ key (nói tên món mình muốn), rồi họ trả về value của key đó (món ăn khớp với mô tả trong thực đơn). Mmmmm, còn gì bằng một bữa trưa gồm bò rừng chăn thả tự nhiên với ngô, sốt đào và một ly IPA! Dù sao bạn cũng là một lập trình viên mà.

Có hai điểm khác biệt quan trọng giữa mảng máy bán hàng và hash thực đơn. Thứ nhất, dùng tên của sự vật để tìm thứ mình cần dễ hơn nhiều so với việc phải chuyển điều mình muốn thành các index dạng số. Đây là một lợi thế rất lớn của hash: không còn phải đếm từng phần tử mảng để yêu cầu thứ mình muốn! Thứ hai, các món trong thực đơn có thể xuất hiện theo bất kỳ thứ tự nào, và chúng ta vẫn nhận được đúng món mình muốn miễn là dùng đúng tên. Tính chất không phụ thuộc vào thứ tự này không đúng với mảng, vì mảng phụ thuộc rất nhiều vào thứ tự.

### Tạo hash

Hãy bắt tay vào tạo một hash!

```ruby
my_hash = {
  "a random word" => "ahoy",
  "Dorothy's math test score" => 94,
  "an array" => [1, 2, 3],
  "an empty hash within a hash" => {}
}
```

Ví dụ này cho thấy cách cơ bản nhất để tạo hash, đó là dùng hash literal với cặp ngoặc nhọn (`{}`).

Hash trên có bốn key trỏ đến bốn value khác nhau. Ví dụ, key đầu tiên, `"a random word"`, trỏ đến value `"ahoy"`. Như bạn thấy, value của hash có thể là một số, một chuỗi, một mảng, hoặc thậm chí một hash khác. Key và value được liên kết bằng một toán tử đặc biệt gọi là **hash rocket**: `=>`.

Cũng giống như với mảng, bạn có thể tạo một hash mới bằng cách gọi phương thức `::new` quen thuộc trên class `Hash`.

```ruby
my_hash = Hash.new
my_hash               #=> {}
```

Tất nhiên, hash không chỉ nhận chuỗi làm key và value. Ruby là một ngôn ngữ khá linh hoạt, nên bạn có thể cho bất kỳ thứ gì vào đó và nó vẫn hoạt động bình thường.

```ruby
hash = { 9 => "nine", :six => 6 }
```

### Truy cập value

Bạn có thể truy cập value trong hash giống như cách truy cập phần tử trong mảng. Khi gọi value của hash bằng key, key được đặt bên trong một cặp ngoặc vuông, giống như khi gọi một phần tử mảng bằng index.

```ruby
shoes = {
  "summer" => "sandals",
  "winter" => "boots"
}

shoes["summer"]   #=> "sandals"
```

Nếu cố truy cập một key không tồn tại trong hash, kết quả sẽ là `nil`:

```ruby
shoes["hiking"]   #=> nil
```

Đôi khi hành vi này có thể gây vấn đề nếu việc âm thầm trả về giá trị `nil` có khả năng phá hỏng chương trình của bạn. May mắn là hash có phương thức `fetch`, phương thức này sẽ phát sinh lỗi khi bạn cố truy cập một key không có trong hash.

```ruby
shoes.fetch("hiking")   #=> KeyError: key not found: "hiking"
```

Ngoài ra, phương thức này có thể trả về một giá trị mặc định thay vì phát sinh lỗi nếu không tìm thấy key đã cho.

```ruby
shoes.fetch("hiking", "hiking boots") #=> "hiking boots"
```

### Thêm và thay đổi dữ liệu

Bạn có thể thêm một cặp key-value vào hash bằng cách gọi key rồi gán value, giống như với bất kỳ biến nào khác.

```ruby
shoes["fall"] = "sneakers"

shoes     #=> {"summer"=>"sandals", "winter"=>"boots", "fall"=>"sneakers"}
```

Bạn cũng có thể dùng cách này để thay đổi value của một key đã tồn tại.

```ruby
shoes["summer"] = "flip-flops"
shoes     #=> {"summer"=>"flip-flops", "winter"=>"boots", "fall"=>"sneakers"}
```

### Xóa dữ liệu

Xóa dữ liệu khỏi hash được thực hiện bằng phương thức `#delete` của hash. Phương thức này còn có chức năng tiện lợi là trả về value của cặp key-value vừa bị xóa khỏi hash.

```ruby
shoes.delete("summer")    #=> "flip-flops"
shoes                     #=> {"winter"=>"boots", "fall"=>"sneakers"}
```

### Phương thức

Hash phản hồi với nhiều phương thức giống mảng vì cả hai đều sử dụng module **Enumerable** của Ruby. Trong bài học tiếp theo, chúng ta sẽ tìm hiểu sâu hơn nhiều về module Enumerable, bao gồm sự khác nhau trong cách các phương thức Enumerable hoạt động với mảng và hash.

Một vài phương thức hữu ích dành riêng cho hash là `#keys` và `#values`; đúng như tên gọi, chúng lần lượt trả về key và value của hash. Lưu ý rằng cả hai phương thức này đều trả về *mảng*.

```ruby
books = {
  "Infinite Jest" => "David Foster Wallace",
  "Into the Wild" => "Jon Krakauer"
}

books.keys      #=> ["Infinite Jest", "Into the Wild"]
books.values    #=> ["David Foster Wallace", "Jon Krakauer"]
```

### Gộp hai hash

Đôi khi bạn sẽ gặp tình huống hai hash muốn kết hợp thành một mối lương duyên thiêng liêng. May mắn thay, có một phương thức dành cho việc đó. (Không cần mục sư làm lễ đâu!)

```ruby
hash1 = { "a" => 100, "b" => 200 }
hash2 = { "b" => 254, "c" => 300 }
hash1.merge(hash2)      #=> { "a" => 100, "b" => 254, "c" => 300 }
```

Hãy chú ý rằng khi hai hash có cùng một key, các value từ hash được gộp vào (trong trường hợp này là các value trong `hash2`) sẽ ghi đè lên value của hash đang được... ừm, gộp *vào* (`hash1` ở đây).

Để xem danh sách đầy đủ các phương thức hoạt động trên hash, hãy tham khảo [tài liệu class Hash](https://docs.ruby-lang.org/en/3.4/Hash.html).

### Symbol làm key của hash

Trong bài học này, chúng ta chủ yếu dùng chuỗi làm key của hash, nhưng trong thực tế, bạn gần như luôn thấy symbol (chẳng hạn `:this_guy`) được dùng làm key. Lý do chủ yếu là symbol có hiệu năng tốt hơn chuỗi rất nhiều trong Ruby, đồng thời cho phép cú pháp gọn gàng hơn khi định nghĩa hash. Hãy chiêm ngưỡng vẻ đẹp này:

```ruby
# 'Rocket' syntax
american_cars = {
  :chevrolet => "Corvette",
  :ford => "Mustang",
  :dodge => "Ram"
}
# 'Symbols' syntax
japanese_cars = {
  honda: "Accord",
  toyota: "Corolla",
  nissan: "Altima"
}
```

Ví dụ cuối cùng khiến bạn rơi lệ vì quá đẹp, đúng không? Hãy chú ý rằng hash rocket và dấu hai chấm đại diện cho symbol đã được gộp làm một. Tuy nhiên, điều này đáng tiếc chỉ hoạt động với symbol, nên đừng thử `{ 9: "value" }`, nếu không bạn sẽ gặp lỗi cú pháp.

Khi dùng cú pháp 'symbols' gọn gàng hơn để tạo hash, bạn vẫn cần dùng cú pháp symbol tiêu chuẩn khi muốn truy cập một value. Nói cách khác, bất kể dùng một trong hai tùy chọn cú pháp ở trên để tạo hash, cả hai đều tạo ra các key dạng symbol và được truy cập theo cùng một cách.

```ruby
american_cars[:ford]    #=> "Mustang"
japanese_cars[:honda]   #=> "Accord"
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương về Hash](https://launchschool.com/books/ruby/read/hashes) của Launch School và làm các bài tập bằng irb hoặc bất kỳ REPL phù hợp nào khác.
1. Hoàn thành các bài tập về hash trong [kho ruby-exercises](https://github.com/TheOdinProject/ruby-exercises) mà bạn đã clone trước đây.

</div>