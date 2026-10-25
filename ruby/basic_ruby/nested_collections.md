### Giới thiệu

Bài học này sẽ xây dựng dựa trên kiến thức của bạn về mảng và hash. Như bạn đã thấy, mảng và hash là những cách tuyệt vời để lưu trữ dữ liệu. Tuy nhiên, đôi khi dữ liệu phức tạp hơn và cần nhiều cấu trúc hơn những gì một mảng hoặc hash cơ bản có thể cung cấp.

Hãy cùng tìm hiểu cách dùng mảng lồng nhau và hash lồng nhau để lưu trữ dữ liệu phức tạp hơn.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Mô tả mảng và hash lồng nhau.
- Giải thích loại dữ liệu nào phù hợp để lưu trữ trong mảng và hash lồng nhau.
- Giải thích cách truy cập dữ liệu trong mảng và hash lồng nhau.
- Giải thích vì sao phương thức `#dig` hữu ích.
- Giải thích cách thêm dữ liệu vào mảng và hash lồng nhau.
- Giải thích cách xóa dữ liệu trong mảng và hash lồng nhau.
- Giải thích cách tạo một mảng lồng nhau mới không bị thay đổi.
- Giải thích cách lặp qua mảng và hash lồng nhau.

### Mảng lồng nhau

Mảng có thể chứa mọi loại dữ liệu, bao gồm cả những mảng khác. Một mảng chứa các mảng khác được gọi là mảng lồng nhau, hoặc mảng đa chiều.

Mảng lồng nhau có thể hữu ích khi lưu trữ các nhóm dữ liệu tương tự hoặc dữ liệu theo vị trí. Mảng lồng nhau về điểm kiểm tra dưới đây lưu trữ các nhóm dữ liệu tương tự, còn các hộp thư của giáo viên lưu trữ các nhóm dữ liệu theo vị trí.

```ruby
test_scores = [
  [97, 76, 79, 93],
  [79, 84, 76, 79],
  [88, 67, 64, 76],
  [94, 55, 67, 81]
]

teacher_mailboxes = [
  ["Adams", "Baker", "Clark", "Davis"],
  ["Jones", "Lewis", "Lopez", "Moore"],
  ["Perez", "Scott", "Smith", "Young"]
]
```

### Truy cập phần tử

Bạn đã biết mọi phần tử trong một mảng đều có chỉ mục. Để truy cập một phần tử cụ thể bên trong mảng lồng nhau, ta gọi `array[x][y]`, trong đó `x` là chỉ mục của phần tử lồng nhau và `y` là chỉ mục bên trong phần tử lồng nhau đó.

```ruby
teacher_mailboxes[0][0]
#=> "Adams"
teacher_mailboxes[1][0]
#=> "Jones"
teacher_mailboxes[2][0]
#=> "Perez"
```

Bạn cũng có thể dùng chỉ mục âm để trả về các phần tử tính từ cuối mảng, bắt đầu từ [-1].

```ruby
teacher_mailboxes[0][-1]
#=> "Davis"
teacher_mailboxes[-1][0]
#=> "Perez"
teacher_mailboxes[-1][-2]
#=> "Smith"
```

Nếu cố truy cập chỉ mục của một phần tử lồng nhau không tồn tại, Ruby sẽ phát sinh `NoMethodError`, vì lớp nil không có phương thức `[]`. Tuy nhiên, cũng giống như với mảng thông thường, nếu cố truy cập một chỉ mục không tồn tại bên trong phần tử lồng nhau đang tồn tại, Ruby sẽ trả về nil.

```ruby
teacher_mailboxes[3][0]
#=> NoMethodError
teacher_mailboxes[0][4]
#=> nil
```

Nếu muốn nhận về giá trị nil khi cố truy cập chỉ mục của một phần tử lồng nhau không tồn tại, bạn có thể dùng phương thức `#dig`. Phương thức này cũng có thể được dùng khi truy cập một chỉ mục không tồn tại bên trong phần tử lồng nhau đang tồn tại.

```ruby
teacher_mailboxes.dig(3, 0)
#=> nil
teacher_mailboxes.dig(0, 4)
#=> nil
```

### Tạo mảng lồng nhau mới

Giờ bạn đã biết cách truy cập các giá trị bên trong mảng lồng nhau, chúng ta cần lùi lại một bước để xem cách tạo mảng lồng nhau mới. Trong một bài học trước, bạn đã được học cách tạo mảng mới bằng cách gọi phương thức Array.new với tối đa 2 đối số tùy chọn (kích thước ban đầu và giá trị mặc định), chẳng hạn như `Array.new(3)` hoặc `Array.new(3, 7)`. Tuy nhiên, có một "cạm bẫy" lớn cần lưu ý. Theo [tài liệu lớp Array](https://docs.ruby-lang.org/en/3.4/Array.html), đối số tùy chọn thứ hai của `Array.new` (giá trị mặc định) chỉ nên được dùng với một đối tượng bất biến (không thể thay đổi), chẳng hạn như số, giá trị boolean hoặc symbol. Việc dùng chuỗi, mảng, hash hoặc một đối tượng khả biến khác có thể dẫn đến hành vi khó hiểu, vì mỗi giá trị mặc định trong mảng thực chất sẽ là một *tham chiếu đến* cùng một giá trị mặc định. Vì vậy, bất kỳ thay đổi nào đối với **một** phần tử cũng sẽ thay đổi **tất cả** các phần tử trong mảng.

Để tạo một mảng chứa các đối tượng khả biến (chuỗi, mảng, hash, v.v.) nhưng mỗi đối tượng là một thể hiện riêng, bạn cần truyền giá trị mặc định cho `Array.new` thông qua block, dùng dấu ngoặc nhọn, thay vì đối số tùy chọn thứ hai. Đoạn mã trong block được đánh giá cho từng vị trí trong mảng, tạo ra nhiều đối tượng để khởi tạo mảng thay vì các tham chiếu đến cùng một đối tượng.

Để tự mình thấy điều này, hãy xem hai ví dụ. Ví dụ đầu tiên dùng đối số tùy chọn thứ hai làm giá trị mặc định.

```ruby
mutable = Array.new(3, Array.new(2))
#=> [[nil, nil], [nil, nil], [nil, nil]]
mutable[0][0] = 1000
#=> 1000
mutable
#=> [[1000, nil], [1000, nil], [1000, nil]]
```

Việc thay đổi giá trị của phần tử đầu tiên trong mảng lồng nhau đầu tiên khiến phần tử đầu tiên thay đổi trong cả ba mảng lồng nhau! Hành vi tương tự cũng xảy ra với chuỗi, hash hoặc bất kỳ đối tượng khả biến nào khác.

Bây giờ hãy xem một ví dụ bỏ qua đối số tùy chọn thứ hai và thay vào đó truyền giá trị khả biến trong một block.

```ruby
nested_arrays = Array.new(3) { Array.new(2) }
#=> [[nil, nil], [nil, nil], [nil, nil]]
nested_arrays[0][0] = 1000
#=> 1000
nested_arrays
#=> [[1000, nil], [nil, nil], [nil, nil]]
```

Mỗi block `{ Array.new(2) }` tạo ra một mảng bên trong mới. Điều này có nghĩa là cả ba mảng bên trong đều là **các đối tượng độc lập**. Khi bạn thay đổi một mảng, những mảng còn lại vẫn giữ nguyên.

Việc thay đổi giá trị của phần tử đầu tiên trong mảng lồng nhau đầu tiên không làm giá trị thay đổi trong bất kỳ mảng lồng nhau nào khác.

### Thêm và xóa phần tử

Bạn có thể thêm một phần tử khác vào cuối mảng lồng nhau bằng phương thức `#push` hoặc toán tử shovel `<<`. Nếu muốn thêm một phần tử vào một mảng lồng nhau cụ thể, bạn cần chỉ rõ chỉ mục của mảng lồng nhau đó.

```ruby
test_scores << [100, 99, 98, 97]
#=> [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81], [100, 99, 98, 97]]
test_scores[0].push(100)
#=> [97, 76, 79, 93, 100]
test_scores
#=> [[97, 76, 79, 93, 100], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81], [100, 99, 98, 97]]
```

Với cú pháp tương tự, bạn có thể thêm hoặc xóa phần tử khỏi toàn bộ mảng lồng nhau hoặc khỏi một phần tử lồng nhau cụ thể.

```ruby
test_scores.pop
#=> [100, 99, 98, 97]
test_scores[0].pop
#=> 100
test_scores
#=> [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81]]
```

### Lặp qua mảng lồng nhau

Hãy phân tích cách lặp qua một mảng lồng nhau bằng phương thức `#each_with_index`. Bạn có thể thấy hữu ích nếu hình dung mảng lồng nhau gồm các hàng và cột. Mỗi hàng là một phần tử lồng nhau, còn mỗi cột là chỉ mục của phần tử lồng nhau đó. Khi lặp qua ví dụ teacher_mailboxes, mỗi phần tử sẽ là một hàng.

```ruby
teacher_mailboxes.each_with_index do |row, row_index|
  puts "Row:#{row_index} = #{row}"
end
#=> Row:0 = ["Adams", "Baker", "Clark", "Davis"]
#=> Row:1 = ["Jones", "Lewis", "Lopez", "Moore"]
#=> Row:2 = ["Perez", "Scott", "Smith", "Young"]
#=> [["Adams", "Baker", "Clark", "Davis"], ["Jones", "Lewis", "Lopez", "Moore"], ["Perez", "Scott", "Smith", "Young"]]
```

Để lặp qua từng phần tử bên trong mỗi hàng, bạn cần lồng thêm một phương thức enumerable khác vào bên trong.

```ruby
teacher_mailboxes.each_with_index do |row, row_index|
  row.each_with_index do |teacher, column_index|
    puts "Row:#{row_index} Column:#{column_index} = #{teacher}"
  end
end
#=> Row:0 Column:0 = Adams
#=> Row:0 Column:1 = Baker
#=> Row:0 Column:2 = Clark
#=> Row:0 Column:3 = Davis
#=> Row:1 Column:0 = Jones
#=> Row:1 Column:1 = Lewis
#=> Row:1 Column:2 = Lopez
#=> Row:1 Column:3 = Moore
#=> Row:2 Column:0 = Perez
#=> Row:2 Column:1 = Scott
#=> Row:2 Column:2 = Smith
#=> Row:2 Column:3 = Young
#=> [["Adams", "Baker", "Clark", "Davis"], ["Jones", "Lewis", "Lopez", "Moore"], ["Perez", "Scott", "Smith", "Young"]]
```

Mặc dù những ví dụ này hơi gượng ép, điều quan trọng cần lưu ý là nếu chỉ cần tên của từng giáo viên thì sẽ có lợi hơn khi dùng `#flatten` trước khi lặp.

```ruby
teacher_mailboxes.flatten.each do |teacher|
  puts "#{teacher} is amazing!"
end
#=> Adams is amazing!
#=> Baker is amazing!
#=> Clark is amazing!
#=> Davis is amazing!
#=> Jones is amazing!
#=> Lewis is amazing!
#=> Lopez is amazing!
#=> Moore is amazing!
#=> Perez is amazing!
#=> Scott is amazing!
#=> Smith is amazing!
#=> Young is amazing!
#=> ["Adams", "Baker", "Clark", "Davis", "Jones", "Lewis", "Lopez", "Moore", "Perez", "Scott", "Smith", "Young"]
```

Bây giờ hãy xem một ví dụ phức tạp hơn, trong đó lồng hai enumerable dạng vị từ vào nhau. Dùng mảng lồng nhau chứa điểm kiểm tra ở trên, hãy xác định xem có học sinh nào đạt trên 80 ở mọi bài hay không.

```ruby
test_scores = [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81]]
#=> [[97, 76, 79, 93], [79, 84, 76, 79], [88, 67, 64, 76], [94, 55, 67, 81]]

test_scores.any? do |scores|
  scores.all? { |score| score > 80 }
end
#=> false
```

Điều này khá dễ hiểu. Kết quả là false vì không có mảng lồng nhau nào có toàn bộ điểm đều trên 80. Theo bạn, điều gì sẽ xảy ra nếu đổi `#any?` và `#all?` cho nhau? Kết quả có giống trước không?

```ruby
test_scores.all? do |scores|
  scores.any? { |score| score > 80 }
end
#=> true
```

Kết quả khác nhau vì giờ đây biểu thức xác định xem **tất cả** các mảng lồng nhau có chứa **bất kỳ** số nào lớn hơn 80 hay không. Kết quả là true vì mỗi mảng lồng nhau đều có ít nhất một số lớn hơn 80.

### Hash lồng nhau

Những hash bạn đã thấy từ đầu đến giờ chỉ có các cặp khóa/giá trị đơn. Tuy nhiên, cũng giống như mảng, chúng có thể được lồng vào nhau hoặc có nhiều chiều. Hash lồng nhau là một cách rất phổ biến để lưu trữ dữ liệu liên kết phức tạp.

```ruby
vehicles = {
  alice: {year: 2019, make: "Toyota", model: "Corolla"},
  blake: {year: 2020, make: "Volkswagen", model: "Beetle"},
  caleb: {year: 2020, make: "Honda", model: "Accord"}
}
```

### Truy cập dữ liệu

Việc truy cập một phần tử cụ thể trong hash lồng nhau rất giống với mảng lồng nhau. Ta thực hiện bằng cách gọi `hash[:x][:y]`, trong đó `:x` là khóa của hash và `:y` là khóa của hash lồng nhau.

```ruby
vehicles[:alice][:year]
#=> 2019
vehicles[:blake][:make]
#=> "Volkswagen"
vehicles[:caleb][:model]
#=> "Accord"
```

Tương tự mảng lồng nhau, nếu cố truy cập một khóa trong hash lồng nhau không tồn tại, Ruby sẽ phát sinh `NoMethodError`, vì vậy bạn có thể muốn dùng phương thức `#dig`. Như dự đoán, nếu cố truy cập một khóa không tồn tại trong hash lồng nhau đang tồn tại, Ruby sẽ trả về nil.

```ruby
vehicles[:zoe][:year]
#=> NoMethodError
vehicles.dig(:zoe, :year)
#=> nil
vehicles[:alice][:color]
#=> nil
vehicles.dig(:alice, :color)
#=> nil
```

### Thêm và xóa dữ liệu

Bạn có thể thêm các hash lồng nhau, giống như với hash thông thường. Giả sử Dave vừa mua một chiếc xe mới và chúng ta muốn thêm nó vào danh sách.

```ruby
vehicles[:dave] = {year: 2021, make: "Ford", model: "Escape"}
#=> {:year=>2021, :make=>"Ford", :model=>"Escape"}
vehicles
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape"}}
```

Bạn cũng có thể thêm một phần tử vào một trong các hash lồng nhau. Giả sử Dave rất thích chiếc Escape mới của mình và chúng ta muốn theo dõi màu của các xe. Để thêm một cặp khóa/giá trị mới vào hash lồng nhau, hãy chỉ rõ khóa của hash lồng nhau ngay trước khi nêu khóa mới.

```ruby
vehicles[:dave][:color] = "red"
#=> "red"
vehicles
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :blake=>{:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape", :color=>"red"}}
```

Xóa một trong các hash lồng nhau cũng giống như xóa trong hash thông thường. Giả sử Blake quyết định bán chiếc Beetle của mình và đeo ba lô đi khắp châu Âu. Vậy hãy xóa xe của Blake.

```ruby
vehicles.delete(:blake)
#=> {:year=>2020, :make=>"Volkswagen", :model=>"Beetle"}
vehicles
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape", :color=>"red"}}
```

Để xóa một trong các cặp khóa/giá trị bên trong hash lồng nhau, trước tiên bạn chỉ rõ khóa của hash. Giả sử Dave quyết định không cần lưu màu xe nữa. Vì vậy, trước khi chỉ ra khóa của hash lồng nhau cần xóa, ta chỉ rõ `vehicles[:dave]`.

```ruby
vehicles[:dave].delete(:color)
#=> "red"
vehicles
#=> {:alice=>{:year=>2019, :make=>"Toyota", :model=>"Corolla"}, :caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape"}}
```

### Phương thức

Có rất nhiều phương thức hữu ích để dùng với hash lồng nhau. Khi đã biết mình cần dữ liệu nào từ một hash lồng nhau, bạn có thể thấy rằng việc xem tài liệu và thử nghiệm các phương thức đó trong IRB là cách tốt nhất để hiểu cách chúng hoạt động.

Hãy xem một ví dụ dùng hash lồng nhau vehicles. Giả sử chúng ta muốn biết ai sở hữu các xe có đời từ năm 2020 trở lên. Thoạt nhìn vào tài liệu, có vẻ `#select` là một phương thức rất phù hợp.

```ruby
vehicles.select { |name, data| data[:year] >= 2020 }
#=> {:caleb=>{:year=>2020, :make=>"Honda", :model=>"Accord"}, :dave=>{:year=>2021, :make=>"Ford", :model=>"Escape"}}
```

Đúng vậy, dùng `#select` cho ta thông tin cần thiết. Tuy nhiên, nếu chỉ muốn tên của chủ xe chứ không muốn một hash lồng nhau khác thì sao? Hãy tiếp tục xem tài liệu để tìm hiểu thêm. Phương thức `#collect` có vẻ rất hữu ích trong tình huống này. Hãy thu thập tên nếu đời xe từ năm 2020 trở lên.

```ruby
vehicles.collect { |name, data| name if data[:year] >= 2020 }
#=> [nil, :caleb, :dave]
```

Dùng `#collect` giúp chúng ta tiến gần hơn đến mục tiêu chỉ lấy tên của những chủ xe mới. Nếu xem phương thức này trong tài liệu, bạn sẽ thấy `#collect` và `#map` có cùng chức năng. Cả hai phương thức đều dùng giá trị trả về của mỗi lần lặp, vì vậy khi câu lệnh if cho kết quả false, nó sẽ trả về giá trị nil.

Các giá trị nil có thể gây ra vấn đề về sau, vì vậy hãy xem tài liệu để tìm một phương thức giúp giải quyết vấn đề này. Phương thức `#compact` trả về một mảng (hoặc hash) không có các giá trị nil, nên hãy nối nó vào cuối block.

```ruby
vehicles.collect { |name, data| name if data[:year] >= 2020 }.compact
#=> [:caleb, :dave]
```

Đúng vậy, dùng `#collect` và `#compact` trả về dữ liệu chúng ta cần. Như bạn thấy, việc nối chuỗi các phương thức có thể rất hữu ích. Tuy nhiên, nếu tiếp tục khám phá tài liệu, chúng ta sẽ tìm thấy một phương thức kết hợp chức năng của hai phương thức này. Ruby phiên bản 2.7 đã thêm một phương thức enumerable mới có tên `#filter_map`, tỏ ra rất phù hợp cho tình huống này.

```ruby
vehicles.filter_map { |name, data| name if data[:year] >= 2020 }
#=> [:caleb, :dave]
```

Tuyệt vời! Chúng ta đã tìm được một giải pháp hay để trả về một mảng chỉ chứa tên của những chủ xe có xe đời từ năm 2020 trở lên! Ngoài ra, chúng ta còn có thêm kinh nghiệm dùng những phương thức khác mà có lẽ bạn sẽ sử dụng trong tương lai. Khi có một trường hợp sử dụng cụ thể, việc khám phá tài liệu đã giúp chúng ta tìm ra một số phương thức thực sự hữu ích.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc thêm về [việc dùng hash với mảng lồng nhau](https://web.archive.org/web/20220525215038/https://learn.co/lessons/nested-hash-iteration).
1. Câu trả lời trên Stack Overflow này giải thích thêm về [việc dùng mảng với hash lồng nhau](https://stackoverflow.com/questions/50529389/manipulating-output-from-an-array-of-nested-hashes-in-ruby).
1. Hoàn thành các bài tập về collection lồng nhau trong [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises).

</div>
