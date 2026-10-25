### Giới thiệu

Trong các bài học trước, bạn đã tìm hiểu về vòng lặp, cũng như mảng và hash. Chẳng bao lâu nữa, bạn sẽ nhận ra rằng với tư cách là một developer, mình phải lặp qua các collection (duyệt hoặc thực hiện một việc nhiều lần) thường xuyên đến mức phát chóng mặt. Hãy nhớ đến cách tiếp cận **DRY (Don't Repeat Yourself - Đừng lặp lại chính mình)** trong lập trình mà chúng ta đã nói đến ở bài học về Methods? Ruby duy trì việc lặp theo tinh thần DRY nhờ một thứ gọi là enumerable.

**Enumerables** là một tập hợp các phương thức dựng sẵn tiện lợi trong Ruby, được cung cấp cho cả mảng và hash. Có những mẫu lặp mà bạn sẽ thấy mình thực hiện đi thực hiện lại, chẳng hạn như biến đổi, tìm kiếm và chọn ra các tập con của phần tử trong collection. Enumerables được thiết kế để việc triển khai những mẫu lặp này (và vì thế, cuộc sống của bạn với tư cách developer) dễ dàng hơn rất, rất nhiều.

Chúng ta sẽ đi qua những phương thức enumerable bạn thường dùng nhất và thường bắt gặp trong thực tế. Đây chắc chắn không phải danh sách đầy đủ, vì vậy hãy xem [tài liệu Ruby](https://docs.ruby-lang.org/en/3.4/) để biết thêm [module Enumerable](https://docs.ruby-lang.org/en/3.4/Enumerable.html) còn cung cấp những gì.

Trong tất cả ví dụ của bài học này, bạn có thể thực hành theo trong irb hoặc bất kỳ môi trường REPL trực tuyến phù hợp nào để hiểu rõ hơn cách chúng hoạt động.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích cách hoạt động của phương thức `#each` và `#each_with_index`, cũng như sự khác nhau giữa chúng.
- Giải thích cách hoạt động của phương thức `#map`.
- Giải thích cách hoạt động của phương thức `#select`.
- Giải thích cách hoạt động của phương thức `#reduce`.
- Mô tả sự khác nhau giữa việc dùng `do...end` và `{...}`.
- Giải thích bang method là gì và vì sao việc dùng nó có hoặc không được xem là best practice.

### Cuộc sống trước khi có enumerable

Giả sử bạn muốn lập danh sách khách mời cho sinh nhật bằng mảng `friends`, nhưng không muốn mời người bạn Brian vì cậu ta hơi điên khi dự tiệc và lúc nào cũng uống quá nhiều.

Với những vòng lặp đã học, bạn có thể viết như sau:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
invited_list = []

for friend in friends do
  if friend != 'Brian'
  invited_list.push(friend)
  end
end

invited_list #=> ["Sharon", "Leo", "Leila", "Arun"]
```

**Cảnh báo:** `do` là tùy chọn trong vòng lặp for của Ruby và có thể gây ra vấn đề khi dùng trong IRB

Cách này không quá khó, nhưng hãy tưởng tượng phải làm vậy cho mọi bữa tiệc bạn tổ chức từ nay cho đến hết đời! Có lẽ cách dễ hơn là thôi không chơi với Brian nữa.

Dùng phương thức enumerable `#select` (chúng ta sẽ tìm hiểu kỹ hơn ở phần sau), bạn có thể đổi đoạn code trên thành:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.select { |friend| friend != 'Brian' }
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

hoặc thậm chí ngắn gọn và đúng trọng tâm hơn:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.reject { |friend| friend == 'Brian' }
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

Bạn vừa rút gọn một chương trình vốn có 8 dòng xuống còn 2 dòng. Tuyệt vời! Hãy tưởng tượng bạn sẽ tiết kiệm được bao nhiêu thời gian khi sắp xếp danh sách khách mời từ giờ.

### Phương thức each

`#each` là ông tổ của các phương thức enumerable. Nó hơi giống Chuck Norris: gần như việc gì cũng làm được. Tuy nhiên, như bạn sẽ thấy trong bài học này, việc có thể dùng `#each` cho hầu hết mọi thứ không có nghĩa đó luôn là công cụ tốt nhất hoặc hiệu quả nhất cho công việc.

Gọi `#each` trên một mảng sẽ duyệt qua mảng đó và truyền từng phần tử vào một block, nơi ta có thể thực hiện một tác vụ:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.each { |friend| puts "Hello, " + friend }

#=> Hello, Sharon
#=> Hello, Leo
#=> Hello, Leila
#=> Hello, Brian
#=> Hello, Arun

#=> ["Sharon", "Leo", "Leila", "Brian" "Arun"]
```

Hãy phân tích cú pháp này:

- `friends` là mảng chứa các chuỗi tên bạn bè.
- `.each` là phương thức enumerable bạn đang gọi trên mảng `friends`.
- `{ |friend| puts "Hello, " + friend }` là một **block**, và code bên trong block này được chạy cho từng phần tử trong mảng. Vì mảng có 5 người bạn nên block sẽ chạy 5 lần, mỗi lần với một trong 5 phần tử.
- Bên trong block, bạn sẽ thấy `|friend|`, được gọi là **biến block**. Đây là phần tử trong mảng mà block hiện đang duyệt qua. Bạn có thể dùng bất kỳ tên biến nào thấy hữu ích; trong ví dụ này, ta có thể dùng `|x|`, nhưng `|friend|` mô tả rõ hơn mỗi phần tử là gì. Ở lần lặp đầu tiên, giá trị của `|friend|` là `'Sharon'`; ở lần thứ hai là `'Leo'`; lần thứ ba là `'Leila'`; và cứ thế cho đến hết mảng.

Nếu block muốn truyền vào một phương thức cần nhiều logic hơn mức có thể đặt trên một dòng thì sao? Khi đó code bắt đầu kém dễ đọc và trông cồng kềnh. Với block nhiều dòng, best practice thường được chấp nhận là đổi cú pháp sang `do...end` thay vì `{...}`:

```ruby
my_array = [1, 2]

my_array.each do |num|
  num *= 2
  puts "The new number is #{num}."
end

#=> The new number is 2.
#=> The new number is 4.

#=> [1, 2]
```

`#each` cũng hoạt động với hash, kèm thêm một chút chức năng. Mặc định, mỗi lần lặp sẽ truyền cả key và value riêng rẽ hoặc cùng nhau (dưới dạng một mảng) vào block, tùy theo cách bạn định nghĩa biến block:

```ruby
my_hash = { "one" => 1, "two" => 2 }

my_hash.each { |key, value| puts "#{key} is #{value}" }

#=> one is 1
#=> two is 2
#=> { "one" => 1, "two" => 2}
```

```ruby
my_hash.each { |pair| puts "the pair is #{pair}" }

#=> the pair is ["one", 1]
#=> the pair is ["two", 2]
#=> { "one" => 1, "two" => 2}
```

Có thể bạn đã để ý trong các ví dụ code trên rằng `#each` trả về mảng hoặc hash ban đầu, bất kể điều gì xảy ra bên trong block code. Đây là điều quan trọng cần ghi nhớ khi debug code, vì nó có thể gây ra một chút nhầm lẫn.

Hãy xem đoạn code này:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.each { |friend| friend.upcase }

#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
```

Bạn có thể nghĩ rằng kết quả sẽ là `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`, nhưng bạn đã nhầm---nhầm hoàn toàn. Thực tế, nó trả về mảng ban đầu mà bạn đã gọi `#each` trên đó. Brian, bạn *vẫn* không được mời.

### Phương thức each_with_index

Phương thức này gần giống `#each`, nhưng cung cấp thêm chức năng bằng cách truyền vào hai **biến block** thay vì một khi duyệt qua mảng. Giá trị của biến thứ nhất là chính phần tử, còn giá trị của biến thứ hai là chỉ mục của phần tử đó trong mảng. Nhờ vậy, bạn có thể thực hiện những việc phức tạp hơn một chút.

Ví dụ, nếu chỉ muốn in cách một từ trong mảng chuỗi một từ, ta có thể làm như sau:

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]

fruits.each_with_index { |fruit, index| puts fruit if index.even? }

#=> apple
#=> strawberry
#=> ["apple", "banana", "strawberry", "pineapple"]
```

Cũng giống phương thức `#each`, `#each_with_index` trả về mảng ban đầu mà nó được gọi trên đó.

### Phương thức map

Bạn còn nhớ lúc ta thử dùng `#each` để viết tên tất cả bạn bè bằng chữ hoa không? Đây là đoạn code đã thử:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.each { |friend| friend.upcase }

#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
```

Như ta thấy, `#each` trả về mảng ban đầu, nhưng đó không phải điều ta muốn. TA MUỐN CHỮ HOA!

Hãy chỉnh sửa code `#each` để nó hoạt động:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
shouting_at_friends = []

friends.each { |friend| shouting_at_friends.push(friend.upcase) }
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

shouting_at_friends #=> ['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']
```

Nó hoạt động! Tuy vậy, ta phải làm thêm khá nhiều việc. Ta phải thêm một mảng khác để lưu các phần tử đã biến đổi. Code này bắt đầu trở nên rườm rà và đáng ngờ là giống ví dụ vòng lặp `for` ở phần đầu, thứ mà ta đang cố tránh.

May mắn thay, ta có phương thức enumerable `#map` để giải cứu!

Phương thức `#map` (còn gọi là `#collect`) biến đổi từng phần tử của một mảng theo block bạn truyền vào và trả về các phần tử đã biến đổi trong một mảng mới. Ban đầu `#map` có thể gây khó hiểu, nhưng nó cực kỳ hữu ích. Chúng ta sẽ đi qua một số ví dụ và trường hợp sử dụng; chúng sẽ giúp bạn hiểu cách và thời điểm dùng sức mạnh enumerable này cho mục đích tốt.

Trước hết, hãy dùng `#map` để cải thiện đoạn code biến đổi tên tất cả bạn bè thành chữ hoa:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.map { |friend| friend.upcase }
#=> `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`
```

Chúng ta lại quay về hai dòng code rồi, bạn thân mến! Thật đẹp mắt phải không?

Có lẽ sau khi học căng thẳng thế này bạn bắt đầu đói và muốn đổi suất McDonald's từ cỡ vừa sang cỡ siêu lớn. Với `#map` và [`#gsub`](https://docs.ruby-lang.org/en/3.4/String.html#method-i-gsub), việc đó dễ như ăn bánh:

```ruby
my_order = ['medium Big Mac', 'medium fries', 'medium milkshake']

my_order.map { |item| item.gsub('medium', 'extra large') }
#=> ["extra large Big Mac", "extra large fries", "extra large milkshake"]
```

Có thể bạn quyết định đã đến lúc sắp xếp lại tài chính và muốn trừ các khoản tiền thuê nhà khỏi tiền lương trong vài tháng qua để chắc rằng mình không tiêu hết số tiền còn lại cho những bữa Big Mac cỡ siêu lớn:

```ruby
salaries = [1200, 1500, 1100, 1800]

salaries.map { |salary| salary - 700 }
#=> [500, 800, 400, 1100]
```

Bất cứ khi nào muốn trả về một mảng mới chứa kết quả chạy block code, `#map` là phương thức dành cho bạn!

### Phương thức select

Bạn đã thấy phương thức `#select` hoạt động ở đầu bài học này, trong nỗ lực biến Brian thành kẻ bị cô lập.

Phương thức `#select` (còn gọi là `#filter`) truyền từng phần tử trong mảng vào một block và trả về một mảng mới chỉ chứa những phần tử mà điều kiện bạn đặt trong block cho kết quả `true`.

Trước hết, hãy xem ta sẽ làm cùng một việc bằng `#each` như thế nào:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
invited_list = []

friends.each do |friend|
  if friend != 'Brian'
    invited_list.push(friend)
  end
end

invited_list
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

Với phương thức `#select` mới tinh, ta có thể rút gọn code này xuống còn hai dòng:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.select { |friend| friend != 'Brian' }

 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

Giờ đã loại Brian ra, ta có thể gửi lời mời! Giả sử những người bạn được mời đã phản hồi, và câu trả lời của họ được ghi lại trong một hash. Hãy dùng `#select` để xem ai sẽ đến. Hãy nhớ rằng khi dùng phương thức enumerable với hash, bạn cần thiết lập biến block cho cả key và value:

```ruby
responses = { 'Sharon' => 'yes', 'Leo' => 'no', 'Leila' => 'no', 'Arun' => 'yes' }
responses.select { |person, response| response == 'yes'}
#=> {"Sharon"=>"yes", "Arun"=>"yes"}
```

Có vẻ chỉ Sharon và Arun đến được. Một bữa tiệc vui cần nhiều người hơn. Có lẽ đã đến lúc miễn cưỡng gọi cho Brian, người mà bạn biết sẽ mang theo một mẻ IPA tự nấu dở tệ. Biết đâu mẻ cuối của cậu ta đã khá hơn?

### Phương thức reduce

Phương thức `#reduce` (còn gọi là `#inject`) có lẽ là enumerable khó nắm bắt nhất đối với người mới học code. Ý tưởng chung là nó nhận một mảng hoặc hash rồi rút gọn nó thành một object duy nhất. Bạn nên dùng `#reduce` khi muốn nhận đầu ra là một giá trị duy nhất.

Một ví dụ kinh điển cho việc dùng `#reduce` là tính tổng một mảng số. Trước hết, hãy xem ta sẽ làm việc này bằng `#each` như thế nào:

```ruby
my_numbers = [5, 6, 7, 8]
sum = 0

my_numbers.each { |number| sum += number }

sum
#=> 26
```

Xét về số dòng code thì không tệ, nhưng ta phải thêm một biến cục bộ tạm thời (`sum`) bên ngoài enumerable. Sẽ tốt hơn nhiều nếu có thể làm tất cả việc này bên trong enumerable:

```ruby
my_numbers = [5, 6, 7, 8]

my_numbers.reduce { |sum, number| sum + number }
#=> 26
```

Ồ! Gì cơ?! Ở đây có khá nhiều điều xảy ra, nên hãy lần lượt xem nó làm gì.

Biến block đầu tiên trong enumerable `#reduce` (`sum` trong ví dụ này) được gọi là **bộ tích lũy**. Kết quả của mỗi lần lặp được lưu vào bộ tích lũy rồi truyền sang lần lặp tiếp theo. Bộ tích lũy cũng là giá trị mà phương thức `#reduce` trả về sau khi hoàn tất. Mặc định, giá trị ban đầu của bộ tích lũy là phần tử đầu tiên trong collection, nên ở mỗi bước lặp ta có:

 1. Lần lặp 0: sum = 5 + 6 = 11
 1. Lần lặp 1: sum = 11 + 7 = 18
 1. Lần lặp 2: sum = 18 + 8 = 26

Ta cũng có thể đặt một giá trị ban đầu khác cho bộ tích lũy bằng cách truyền trực tiếp một giá trị vào phương thức `#reduce`.

```ruby
my_numbers = [5, 6, 7, 8]

my_numbers.reduce(1000) { |sum, number| sum + number }
#=> 1026
```

Bây giờ hãy xem một ví dụ công phu hơn để thấy phương thức này mạnh đến mức nào. Ví dụ này phức tạp hơn nhiều, vì vậy đừng nản nếu hiện tại bạn chưa hiểu hết. Chỉ cần biết rằng trong một số tình huống, `#reduce` có thể giúp bạn tiết kiệm rất nhiều dòng code.

Giờ bạn đã biết ai sẽ đến dự tiệc, cần quyết định đi đâu. Bạn không thích quyết định lắm, nên tổ chức bỏ phiếu giữa những người bạn.

Một lựa chọn là St. Mark's Bistro, nơi sang trọng phù hợp với một người tinh tế như bạn. Lựa chọn còn lại là Bob's Dirty Burger Shack, nơi bạn biết là chỗ yêu thích của Brian. Vì giờ cậu ta cũng đến tiệc, tốt nhất nên đưa nơi này vào danh sách để tránh cãi nhau. Phiếu của bạn bè được tập hợp trong mảng `votes`.

```ruby
votes = ["Bob's Dirty Burger Shack", "St. Mark's Bistro", "Bob's Dirty Burger Shack"]

votes.reduce(Hash.new(0)) do |result, vote|
  result[vote] += 1
  result
end
#=> {"Bob's Dirty Burger Shack"=>2, "St. Mark's Bistro"=>1}
```

Được rồi, vậy ở đây đã xảy ra chuyện gì? Ngoài việc Brian lại phá hỏng bữa tiệc của bạn. Một lần nữa.

Đầu tiên, lần này ta truyền vào một giá trị ban đầu thú vị hơn cho bộ tích lũy. Khi truyền một đối số vào `Hash.new`, đối số đó trở thành giá trị mặc định khi truy cập những key không tồn tại trong hash. Ví dụ:

```ruby
hundreds = Hash.new(100)
hundreds["first"]         #=> 100
hundreds["mine"]          #=> 100
hundreds["yours"]         #=> 100
```

Sau khi đặt giá trị cho một key thành một thứ khác, giá trị mặc định sẽ bị ghi đè:

```ruby
hundreds = Hash.new(100)
hundreds["new"]           #=> 100
hundreds["new"] = 99
hundreds["new"]           #=> 99
```

Giờ ta biết hash mới có giá trị mặc định `0` này là bộ tích lũy (được gọi là `result` trong block code), hãy xem điều gì xảy ra ở mỗi lần lặp:

 1. Lần lặp 0:
    - result = {}
    - Hãy nhớ rằng hash này đã có giá trị mặc định là `0`, nên `result["Bob's Dirty Burger Shack"] == 0` và `result["St. Mark's Bistro"] == 0`
 1. Lần lặp 1:
    - Phương thức chạy `result["Bob's Dirty Burger Shack"] += 1`
    - result = {"Bob's Dirty Burger Shack" => 1}
 1. Lần lặp 2:
    - Phương thức chạy `result["St. Mark's Bistro"] += 1`
    - result = {"Bob's Dirty Burger Shack" => 1, "St. Mark's Bistro" => 1}
 1. Lần lặp 3:
    - Phương thức chạy `result["Bob's Dirty Burger Shack"] += 1`
    - result = {"Bob's Dirty Burger Shack" => 2, "St. Mark's Bistro" => 1}

Lưu ý rằng ví dụ này trả về một hash với nhiều cặp `key => value`. Vì vậy, dù kết quả phức tạp hơn, `#reduce` vẫn chỉ trả về một object duy nhất, ở đây là một hash.

### Bang method

Ở trên, ta đã đề cập rằng các enumerable như `#map` và `#select` trả về mảng mới nhưng không sửa đổi những mảng mà chúng được gọi trên đó. Đây là chủ ý thiết kế, vì ta thường không muốn sửa mảng hoặc hash ban đầu và cũng không muốn vô tình làm mất thông tin đó. Ví dụ, nếu enumerable thay đổi mảng ban đầu, việc dùng `#select` để loại Brian khỏi danh sách khách mời sẽ *vĩnh viễn* xóa cậu ta khỏi danh sách bạn bè. Ồ! Hơi mạnh tay rồi. Brian có thể là kẻ điên khi dự tiệc, nhưng vẫn là bạn của ta.

Để thấy nguyên tắc này hoạt động, hãy quay lại ví dụ trước, nơi ta viết tên tất cả bạn bè bằng chữ hoa:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.map { |friend| friend.upcase }
#=> `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`

friends
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']
```

Bạn có thể thấy khi gọi lại mảng `friends` ban đầu, nó vẫn không thay đổi.

Nếu muốn thay đổi chính mảng `friends`, bạn có thể dùng bang method `#map!`:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.map! { |friend| friend.upcase }
#=> `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`

friends
#=> `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`
```

Lúc này, khi gọi lại mảng `friends` ban đầu, nó trả về các giá trị đã thay đổi từ phương thức `#map!`. Thay vì trả về một mảng mới, `#map!` đã sửa đổi mảng ban đầu.

Như bạn còn nhớ từ bài học Methods, **bang method** có thể dễ dàng nhận biết nhờ dấu chấm than (`!`) ở cuối tên. Tất cả bang method đều mang tính **phá hủy** và sửa đổi object mà chúng được gọi trên đó. Nhiều phương thức enumerable trả về phiên bản mới của mảng hoặc hash mà chúng được gọi trên đó có một phiên bản bang method tương ứng, chẳng hạn như `#map!` và `#select!`.

Tuy nhiên, best practice là tránh dùng những phương thức này, vì bạn hoặc developer tương lai làm việc trên code có thể cần phiên bản ban đầu. Hãy nhớ kẻ tâm thần bạo lực mà bạn nên dự đoán sẽ là người bảo trì code của mình chứ? Hãy nghĩ đến điều đó khi quyết định dùng bang method.

### Giá trị trả về của enumerable

Vậy nếu không nên dùng bang method nhưng ta cần tái sử dụng kết quả của một phương thức enumerable trong toàn bộ chương trình thì sao?

Một lựa chọn là đưa kết quả của phương thức enumerable vào một biến cục bộ:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_friends = friends.select { |friend| friend != 'Brian' }

friends
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_friends
#=> ["Sharon", "Leo", "Leila", "Arun"]
```

Một lựa chọn còn tốt hơn là bọc phương thức enumerable trong một định nghĩa phương thức:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

def invited_friends(friends)
  friends.select { |friend| friend != 'Brian' }
end

friends
#=> ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_friends(friends)
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc bài viết Ruby Explained về [Map, Select và các phương thức Enumerable khác](https://www.eriktrautman.com/posts/ruby-explained-map-select-and-other-enumerable-methods).
  1. Làm theo hướng dẫn [Cách sử dụng phương thức Map của Ruby](https://www.rubyguides.com/2018/10/ruby-map-method/) từ Ruby Guides.
  1. Làm theo bài viết [Reducing Enumerable](https://medium.com/@baweaver/reducing-enumerable-part-one-the-journey-begins-ddc1d4108490) của Brandon Weaver.
  1. Đọc về cách `do...end` so với `{...}` có thể ảnh hưởng đến precedence tại bài [do..end so với curly braces cho block trong Ruby](https://stackoverflow.com/questions/5587264/do-end-vs-curly-braces-for-blocks-in-ruby).
  1. Hoàn thành các bài tập basic enumerable trong [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises) mà bạn đã clone trước đó.

</div>