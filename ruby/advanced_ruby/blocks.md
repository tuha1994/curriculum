### Giới thiệu

Bạn đã quen thuộc với các block trong Ruby qua bài học [Các phương thức Enumerable cơ bản](https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/ruby-programming/lessons/basic-enumerable-methods) trước đây. Nhưng block còn nhiều chiều sâu hơn những gì chúng ta đã tìm hiểu, vì vậy bài học này sẽ đi sâu hơn. Block là một phần nền tảng đến mức bạn nên hiểu rõ chúng từ trong ra ngoài.

Hãy cùng ôn lại thật nhanh những điều cơ bản mà có lẽ bạn đã quen thuộc...

Một block có thể được khai báo trên một dòng hoặc nhiều dòng. Theo quy ước của Ruby, dùng `{}` cho block một dòng và `do..end` cho block nhiều dòng. Bạn có thể truyền tham số cho block bằng cách định nghĩa chúng giữa các dấu gạch đứng, chẳng hạn `|arg1, arg2|`. Bạn hẳn đã biết cả hai dạng này khi làm việc với các phương thức enumerable.

```ruby
# Single-line block
[1,2,3].each { |num| puts num }

# Multi-line block
[1,2,3].each do |num|
  puts num
end
```

Mặc dù bạn đã quen với cách viết block trong ngữ cảnh của các phương thức enumerable, Ruby còn có những cách sử dụng block thú vị khác. Chẳng hạn, bạn có thể tự viết các phương thức nhận block, thậm chí lưu block vào biến. Đến cuối bài học này, bạn sẽ có một sự trân trọng mới dành cho những block đầy sức mạnh.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích block là gì.
- Mô tả những điểm tương đồng giữa block và phương thức.
- Mô tả hai cách khai báo block.
- Giải thích vì sao bạn có thể chọn dùng block thay vì phương thức.
- Giải thích `yield`.
- Giải thích cách truyền đối số cho block từ bên trong một phương thức.
- Giải thích cách kiểm tra xem một block có được truyền vào phương thức hay không.
- Mô tả proc.
- Mô tả lambda.
- Giải thích những điểm khác biệt và tương đồng giữa block, proc và lambda.

### Yield

`yield` là một từ khóa có thể được gọi bên trong phương thức để nhường quyền thực thi cho block đi kèm. Hãy tưởng tượng bạn đang viết một phương thức đơn giản cho thư viện của mình, cho phép người dùng thư viện ghi một số thông tin ra terminal. Yêu cầu then chốt là người dùng phải có thể quyết định thông tin đó được trình bày như thế nào. Đôi khi họ chỉ muốn `puts` một thứ gì đó, lúc khác họ có thể muốn kiểm tra một thứ bằng `p`. Nếu chỉ dùng một phương thức để viết việc này thì thực ra sẽ khá khó. Bạn phải tính đến mọi lựa chọn mà người dùng thư viện có thể muốn gọi, rồi viết tài liệu giải thích chúng. Với block, ta chỉ cần dùng `yield` để nhường quyền điều khiển phương thức cho block và cho phép người dùng quyết định cách in thông tin.

```ruby
def logger
  yield
end

logger { puts 'hello from the block' }
#=> hello from the block

logger do
  p [1,2,3]
end
#=> [1,2,3]
```

Ví dụ này chưa có gì thú vị, nhưng hy vọng bạn đã thấy block cho phép các phương thức xử lý dữ liệu linh hoạt đến mức nào.

Hãy thử làm ví dụ thú vị hơn. Bây giờ người dùng muốn một phương thức cho phép họ viết bất cứ nội dung nào, và nội dung đó được in ra terminal hai lần. Bạn sẽ xử lý yêu cầu này thế nào? Với `yield`, việc đó rất dễ! Bạn có thể gọi `yield` trong một phương thức bao nhiêu lần tùy ý, và mỗi lần nó sẽ hoạt động giống nhau.

```ruby
def double_vision
  yield
  yield
end

double_vision { puts "How many fingers am I holding up?" }
#=> How many fingers am I holding up?
#=> How many fingers am I holding up?
```

Bơ Millennial, Batman. Hay đấy.

Trước đó chúng ta đã nói rằng block có thể nhận đối số. Nhưng truyền chúng cho block bằng cách nào? Không vấn đề gì! Chỉ cần truyền chúng làm đối số cho `yield`, và chúng sẽ được chuyển vào block dưới dạng tham số. Nếu gọi `yield` nhiều hơn một lần, bạn có thể truyền một đối số khác nhau ở mỗi lần gọi nếu muốn.

```ruby
def love_language
  yield('Ruby')
  yield('Rails')
end

love_language { |lang| puts "I love #{lang}" }
#=> I love Ruby.
#=> I love Rails.
```

Điều này có thể hữu ích như thế nào? Hãy giả sử bạn đang tạo một ứng dụng ngân hàng cho phép ngân hàng in danh sách giao dịch vào một bản sao kê. Trong ví dụ này, ta sẽ dùng terminal, nhưng cũng có thể là bảng tính hay bất cứ thứ gì khác. Tuy nhiên, bạn gặp một vấn đề. Bạn muốn ứng dụng hoạt động cho mọi ngân hàng, nhưng mỗi ngân hàng lại muốn in giao dịch theo một định dạng khác nhau. Làm sao thiết kế để mỗi ngân hàng có thể chọn một định dạng riêng, trong khi bạn chỉ phải viết code một lần?

Ta có thể kết hợp sức mạnh của `yield` với phương thức enumerable `#each`. Trong ví dụ dưới đây, ta viết một phương thức lặp qua danh sách giao dịch và yield từng giao dịch cho một block. Bên gọi phương thức (ngân hàng) có thể gọi nó với bất kỳ block nào họ muốn. Nhờ vậy, họ có thể quyết định cách các giao dịch được in vào bản sao kê, còn bạn có thể tập trung cung cấp các giao dịch ngân hàng không lỗi.

```ruby
@transactions = [10, -15, 25, 30, -24, -70, 999]

def transaction_statement
  @transactions.each do |transaction|
    yield transaction # You just yield the transaction amount.
  end
end

transaction_statement do |transaction|
  p "%0.2f" % transaction # The bank that calls the method can define how it is handled.
end
#=> ["10.00", "-15.00", "25.00", "30.00", "-24.00", "-70.00", "999.00"]
```

Nếu một ngân hàng khác muốn in giao dịch theo cách khác thì cũng không sao, họ có thể cung cấp block riêng của mình.

Nếu thay vì để bên gọi quyết định cách in giao dịch, bạn chỉ muốn họ quyết định định dạng thì sao? Giống như mọi lời gọi phương thức trong Ruby, block có giá trị trả về. Vì vậy khi gọi `yield`, nó trả về giá trị được thực thi sau cùng từ block. Dựa trên ví dụ trên, nếu bạn chuyển `p` từ block vào bên trong lời gọi `#each`, phương thức của bạn sẽ kiểm soát cách in các giao dịch. Chẳng hạn, có thể bạn chỉ cho phép xuất sang .csv.

```ruby
@transactions = [10, -15, 25, 30, -24, -70, 999]

def transaction_statement
  @transactions.each do |transaction|
    p yield transaction # `p` is called within our method now instead of within the block
  end
end

transaction_statement do |transaction|
  "%0.2f" % transaction
end
#=> ["10.00", "-15.00", "25.00", "30.00", "-24.00", "-70.00", "999.00"]
```

Nếu muốn thu thập giá trị mà block trả về, bạn chỉ cần gán nó cho một biến hoặc gom nó vào một cấu trúc dữ liệu.

```ruby
@transactions = [10, -15, 25, 30, -24, -70, 999]

def transaction_statement
  formatted_transactions = []
  @transactions.each do |transaction|
    formatted_transactions << yield(transaction)
  end

  p formatted_transactions
end

transaction_statement do |transaction|
  "%0.2f" % transaction
end
#=> ["10.00", "-15.00", "25.00", "30.00", "-24.00", "-70.00", "999.00"]
```

Bạn cũng có thể viết câu lệnh return tường minh trong block. Cách này hoạt động giống như return tường minh trong một phương thức. Điều này có thể hữu ích nếu bạn cần một dạng [mệnh đề bảo vệ](https://blog.techatpower.com/never-let-your-guard-down-533605891528) nào đó.

Trong các ví dụ trên, giá trị ta yield cho block trong `transaction_statement` được block nhận và gán cho tham số đã đặt tên (biến nằm giữa các dấu gạch đứng, ở đây là `|transaction|`). Cách này hoạt động giống như trong các phương thức enumerable như `#each` và `#map`.

Nếu bạn không truyền giá trị cùng `yield` mà block lại mong đợi một giá trị, đối số đó sẽ được gán là `nil`.

```ruby
def say_something
  yield # No arguments are passed to yield
end

say_something do |word| # But the block expects one argument to be passed in
  puts "And then I said: #{word}"
end
#=> And then I said:
```

Nếu bạn có hai tham số nhưng truyền 3 đối số, đối số cuối cùng sẽ không được gán cho tham số nào và bạn không thể tham chiếu đến nó trong block.

```ruby
def mad_libs
  yield('cool', 'beans', 'burrito') # 3 arguments are passed to yield
end

mad_libs do |adjective, noun| # But the block only takes 2 parameters
  puts "I said #{adjective} #{noun}!"
end
#=> I said cool beans!
```

Nếu làm việc với hash, bạn có thể cần yield cả key và value; chỉ cần bảo đảm block của bạn đặt tên cho hai tham số.

```ruby
def awesome_method
  hash = {a: 'apple', b: 'banana', c: 'cookie'}

  hash.each do |key, value|
    yield key, value
  end
end

awesome_method { |key, value| puts "#{key}: #{value}" }
#=> a: apple
#=> b: banana
#=> c: cookie
```

Ta sẽ kết thúc phần tìm hiểu về `yield` bằng một câu hỏi. Điều gì xảy ra nếu bạn gọi `yield` nhưng phương thức gọi nó không đi kèm block? Ta sẽ tìm hiểu trong phần tiếp theo.

### Điều khiển block

Thông thường, người viết một phương thức và người gọi phương thức đó là hai người khác nhau. Nếu bạn viết một phương thức sử dụng `yield`, làm sao chắc chắn rằng bên gọi sẽ truyền vào một block? Điều gì xảy ra nếu họ không truyền?

```ruby
def simple_method
  yield
end

simple_method
# => `simple_method': no block given (yield) (LocalJumpError)
```

Đúng vậy, một lỗi đã xảy ra. Vậy làm thế nào để viết một phương thức hoạt động dù bên gọi có truyền block hay không?

Hãy dùng `block_given?`

Bạn có thể dùng phương thức này như một phép kiểm tra điều kiện bên trong phương thức của mình để xem bên gọi có truyền block hay không. Nếu có, `block_given?` trả về `true`; nếu không, nó trả về `false`. Nhờ đó, bạn có thể viết phương thức hoạt động khác nhau tùy thuộc vào việc nó có nhận được block hay không.

```ruby
def maybe_block
  if block_given?
    puts "block party"
  end
  puts "executed regardless"
end

maybe_block
# => executed regardless

maybe_block {} # {} is just an empty block
# => block party
# => executed regardless
```

Có thể bạn đã gặp điều này trong một số enumerable. [#count](https://docs.ruby-lang.org/en/3.4/Enumerable.html#method-i-count) là một phương thức có thể được gọi có hoặc không có block. Nếu được gọi mà không có đối số, nó chỉ trả về kích thước của đối tượng được gọi. Khi được gọi với một đối số, nó đếm số lần đối số đó xuất hiện trong đối tượng. Còn khi có block, nó yield cho block và cung cấp số lần block trả về một phản hồi đúng về mặt logic. Nếu chuyển sang xem mã nguồn của `#count` trên trang Tài liệu Ruby, bạn sẽ thấy nó được viết bằng C; nhưng ngay cả khi chỉ lướt qua cú pháp C xa lạ, bạn vẫn có thể nhận ra nó kiểm tra xem block có được truyền vào hay không. Với Ruby, ta chỉ có cú pháp tao nhã hơn.

### Lambda

Lambda là một cách viết block rồi lưu nó vào một biến. Điều này hữu ích nếu bạn gọi các phương thức khác nhau nhưng truyền cùng một block cho từng phương thức. Nó giúp bạn không phải gõ đi gõ lại cùng một đoạn code.

Có hai cách tạo lambda. Một cách là dùng từ khóa `lambda`, chẳng hạn `lambda { "inside the lambda" }`. Cách còn lại để khai báo lambda là dùng cú pháp "lambda mũi tên", `-> {}`. Trông nó hơi kỳ lạ nhưng bạn sẽ quen. Cú pháp lambda mũi tên được dùng phổ biến hơn, nên đó là cách chúng ta sẽ dùng trong các ví dụ còn lại.

```ruby
my_lambda = lambda { puts "my lambda" }

my_other_lambda = -> { puts "hello from the other side" }
```

Để gọi một lambda, bạn chỉ cần gọi phương thức `#call`.

```ruby
my_lambda = -> { puts "high five" }
my_lambda.call
# => high five
```

Nếu muốn lambda nhận đối số, bạn có hai lựa chọn; lựa chọn nào được dùng phụ thuộc vào cách bạn khai báo lambda. Nếu dùng cú pháp lambda mũi tên `->`, bạn dùng `()` để đặt tên các tham số. Nếu dùng từ khóa `lambda`, bạn dùng các dấu gạch đứng `||` bên trong block.

```ruby
my_name = ->(name) { puts "hello #{name}" }

my_age = lambda { |age| puts "I am #{age} years old" }


my_name.call("tim")
#=> hello tim
my_age.call(78)
#=> I am 78 years old
```

Một điều hơi lạ cần lưu ý về lambda là có rất nhiều cách gọi chúng.

```ruby
my_name = ->(name) { puts "hello #{name}" }

my_name.call("tim")
my_name.("tim")
my_name["tim"]
my_name.=== "tim"
```

Tốt nhất có lẽ là cứ dùng `#call`.

### Proc

Proc chỉ là một đối tượng mà bạn có thể dùng để lưu block và truyền qua lại như biến. Nếu bạn thấy nó hơi giống lambda thì đúng vậy. Thực tế không có lớp lambda đặc biệt nào. Lambda thật ra chỉ là một loại đối tượng proc nhưng có một số hành vi riêng biệt mà chúng ta sẽ tìm hiểu ngay sau đây.

Bạn khai báo một proc mới giống như khởi tạo bất kỳ đối tượng nào trong Ruby, bằng cách dùng `new`.

```ruby
a_proc = Proc.new { puts "this is a proc" }

a_proc.call
#=> this is a proc
```

hoặc bạn có thể chỉ dùng 'proc'

```ruby
a_proc = proc { puts "this is a proc" }

a_proc.call
#=> this is a proc
```

Các đối số được khai báo bên trong các dấu gạch đứng `||`

```ruby
a_proc = Proc.new { |name, age| puts "name: #{name} --- age: #{age}" }

a_proc.call("tim", 80)
#=> name: tim --- age: 80
```

### Proc và lambda

Không còn nhiều điều mới về việc tạo proc mà bạn chưa thấy ở lambda. Vậy tại sao chọn cái này thay vì cái kia? Có một số khác biệt then chốt giữa proc và lambda, khiến việc chọn một loại phù hợp hơn loại còn lại trong từng trường hợp.

#### Đối số

Về đối số, proc hoạt động khá giống block. Proc không quan tâm bạn truyền ít hay nhiều đối số hơn số lượng đã chỉ định. Thực ra, nó cũng không quan tâm nếu bạn không truyền đối số nào. Nó sẽ gán `nil` cho mọi tham số bạn đặt tên nhưng không truyền vào dưới dạng đối số.

```ruby
a_proc = Proc.new { |a, b| puts "a: #{a} --- b: #{b}" }

a_proc.call("apple")
# => a: apple --- b:
```

đó cũng là lý do điều này có thể thực hiện được:

```ruby
nested_array = [[1, 2], [3, 4], [5, 6]]
nested_array.select {|a, b| a + b > 10 }

# => [5, 6]
```

Như bạn thấy, `#select` chỉ định hai đối số `|a, b|`; trong mỗi lần lặp, ta truyền một phần tử duy nhất của nested_array vào block. Ở lần lặp đầu tiên, đó là `[1, 2]`; mảng này sẽ tự động được phân rã (thành a = 1, b = 2), rồi các giá trị của nó được so sánh theo chỉ định. Tiếp tục như vậy ở các lần lặp sau, khi ta lần lượt truyền `[3, 4]` và `[5, 6]`.
Điều này xảy ra vì block `{|a, b| if a + b > 10 }` được xử lý như một proc không phải lambda.
Tính chất này không chỉ giới hạn ở `#select` mà còn áp dụng cho các phương thức `enum` khác như `#map`, `#each`, v.v.
Bạn có thể đọc thêm về điều này trong [tài liệu lớp Proc](https://docs.ruby-lang.org/en/3.4/Proc.html).

Ngược lại, lambda CÓ quan tâm và sẽ phát sinh lỗi nếu bạn không truyền đúng số lượng tham số mà nó yêu cầu.

```ruby
a_lambda = lambda { |a, b| puts "a: #{a} --- b: #{b}" }

a_lambda.call("apple")
# => wrong number of Arguments (given 1, expected 2) (ArgumentError)

a_lambda.call("apple", "banana", "cake")
# => wrong number of Arguments (given 3, expected 2) (ArgumentError)
```

#### Giá trị trả về

Khi viết một return tường minh bên trong lambda, nó sẽ trả về từ block lambda về cho bên gọi.

```ruby
a_lambda = -> { return 1 }

a_lambda.call
# => 1
```

Tuy nhiên, một đối tượng proc sẽ trả về từ "phương thức bao ngoài" (phương thức *định nghĩa* proc).

```ruby
def my_method
  a_proc = Proc.new { return }
  puts "this line will be printed"
  a_proc.call
  puts "this line is never reached"
end

my_method
#=> this line will be printed
```

Trong ví dụ này, proc được gọi bên trong phương thức định nghĩa nó. Kết quả là chính phương thức đó được trả về.

Hãy xem một ví dụ phức tạp hơn:

```ruby
def outer_method
  a_proc = Proc.new { return }
  puts "this line will be printed"

  inner_method(a_proc)
  puts "this line is never reached"
end

def inner_method(proc)
  proc.call
  puts "this line is also never reached"
end

outer_method
#=> this line will be printed
```

Ở đây proc được truyền cho một phương thức khác và phương thức đó gọi nó. Khi `inner_method` gọi proc, nó sẽ trả về từ `outer_method`. Điều này có thể tạo ra hành vi khó hiểu, trong đó luồng thực thi đột nhiên nhảy qua các ngữ cảnh khác nhau. Vì vậy, nếu muốn định nghĩa một block sử dụng `return` rồi truyền nó cho các phương thức khác, lambda có lẽ là lựa chọn tốt hơn.

Lưu ý rằng việc định nghĩa một proc sử dụng `return` ở cấp cao nhất sẽ thoát hẳn khỏi chương trình:

```ruby
a_proc = Proc.new { return }

a_proc.call

puts "This line won't run."
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Tránh LocalJumpError trong IRB

Nếu chạy một proc có return ở cấp cao nhất trong IRB, nó sẽ phát sinh `LocalJumpError`. Tuy nhiên, hành vi này đặc thù do cách tiến trình IRB hoạt động; nếu chạy từ một file, nó sẽ hoạt động như mô tả ở trên.

</div>

### Điểm tương đồng

Giờ chúng ta đã tìm hiểu một số khác biệt lớn giữa proc và lambda, tiếp theo là những điểm tương đồng cần biết.

#### Đối số mặc định

Cả proc và lambda đều hỗ trợ đối số mặc định giống như các phương thức Ruby:

```ruby
my_proc = Proc.new { |name="bob"| puts name }

my_proc.call
# => bob

my_lambda = ->(name="r2d2") { puts name }

my_lambda.call
# => r2d2
```

#### Tham số phương thức

Cả proc và lambda đều có thể được dùng làm đối số cho một phương thức.

```ruby
def my_method(useful_arg)
  useful_arg.call
end

my_lambda = -> { puts "lambda" }
my_proc = Proc.new { puts "proc" }

my_method(my_lambda)
# => lambda

my_method(my_proc)
# => proc
```

### Bắt block

Giờ đã biết proc và lambda hoạt động ra sao, ta có thể áp dụng điều này cho block như thế nào? Như đã học, block giống những phương thức ẩn danh nhỏ. Nhưng nếu muốn giữ một tham chiếu đến block đó để làm gì đó với nó thì sao? Có thể ta cần nhận block trong phương thức ngay lúc này và lưu nó vào một biến instance để gọi sau.

Ruby cho phép ta bắt block trong định nghĩa phương thức dưới dạng một đối số đặc biệt bằng `&`.

```ruby
def cool_method(&my_block)
  my_block.call
end

cool_method { puts "cool" }
```

Nếu phương thức của bạn có các tham số khác, tham số `&` dùng để bắt block luôn phải đứng cuối.

Việc bắt block bằng `&` được gọi là block tường minh. Khi bạn không đặt tên block trong danh sách tham số, nó được gọi là block ngầm định.

Một điều khác cần lưu ý là ngay cả khi đã bắt block bằng `&`, bạn vẫn có thể dùng `yield` thay vì `#call` để nhường quyền thực thi cho block. Tuy nhiên, cách này khiến phương thức kém rõ ràng hơn, nên không được khuyến nghị.

Vậy cơ chế này hoạt động thế nào? Thực ra cú pháp bắt bằng `&` không chỉ dùng được với block. Khi bạn dùng `&`, Ruby sẽ gọi một phương thức tên là `#to_proc` trên bất cứ thứ gì được gán cho biến đó. Trong ví dụ trên, dùng `&` gán block cho `my_block`, sau đó gọi `#to_proc` trên nó để tạo ra một đối tượng proc. Vì vậy bạn có thể dùng `#call` trên đối tượng đó.

Có thể bạn từng thấy hoặc sử dụng đoạn code như sau

```ruby
arr = ["1", "2", "3"]

arr.map(&:to_i)
# => [1, 2, 3]
```

Bên dưới, `#to_proc` được gọi trên symbol `:to_i`. Bạn có thể xem [#to_proc hoạt động thế nào](https://docs.ruby-lang.org/en/3.4/Symbol.html#method-i-to_proc) trong tài liệu Ruby. Nó trả về một đối tượng proc phản hồi với phương thức được symbol chỉ định. Vì vậy ở đây, `#map` truyền từng giá trị trong mảng cho đối tượng proc, rồi đối tượng đó gọi `#to_i` trên giá trị.

(Đúng vậy, tên các phương thức như `#to_i` có thể được truyền qua lại bằng symbol. Nội dung này nằm ngoài phạm vi bài học, nhưng nếu tò mò, hãy xem [tài liệu về #send](https://docs.ruby-lang.org/en/3.4/Object.html#method-i-send) và bài viết Stack Overflow này về [cách #send và #to_i được dùng cùng nhau](https://stackoverflow.com/questions/14881125/what-does-to-proc-method-mean) để `arr.map(&:to_i)` hoạt động.)

`&` cũng hoạt động theo chiều ngược lại. Bạn có thể đặt nó trước một đối tượng proc, nó sẽ chuyển đối tượng đó thành block rồi truyền block cho phương thức đang được gọi.

```ruby
def cool_method
  yield
end

my_proc = Proc.new { puts "proc party" }

cool_method(&my_proc)
# => proc party
```

Nếu thử gọi `cool_method(my_proc)` mà không có `&`, bạn sẽ nhận lỗi vì phương thức không mong đợi đối số nào nhưng bạn lại truyền vào một đối số. Tương tự, nếu một phương thức có tham số được đặt tên và bạn gọi nó bằng một proc đã chuyển thành block, bạn cũng sẽ nhận lỗi.

```ruby
def cool_method(an_arg)
  an_arg.call
end

a_proc = Proc.new { puts "procodile hunter" }

cool_method(&a_proc) # Converting the proc object to a block

# => ArgumentError (wrong number of arguments (given 0, expected 1))
```

Như đã nói trước đó, khi gọi một phương thức, việc dùng `&` với proc sẽ chuyển proc thành block trước khi truyền cho phương thức. Nó không còn là một đối số được đặt tên của phương thức, đó là lý do ta nhận được `ArgumentError` trong ví dụ trên.

### Tổng kết

Block xuất hiện ở khắp nơi trong Ruby. Bạn sẽ gặp rất nhiều trường hợp sử dụng chúng, và thấy chúng được dùng rộng rãi trong các thư viện code. Block cho phép bên gọi phương thức truyền vào một số thông tin theo ngữ cảnh — việc vốn có thể khó thực hiện nếu không phải viết nhiều phương thức để xử lý các kịch bản khác nhau.

Sau khi nắm được nội dung bài học này, bạn sẽ thành bậc thầy về block, proc và lambda, có thể dễ dàng tạo ra những đoạn code Ruby đẹp.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bài viết này cung cấp một [bản tóm tắt khá hay về block, proc và lambda](https://www.rubyguides.com/2016/02/ruby-procs-and-lambdas/).
1. Hãy đọc [bài viết về lambda trong Ruby](https://www.honeybadger.io/blog/using-lambdas-in-ruby/); bài viết này cũng đề cập nhiều nội dung chúng ta đã sử dụng ở đây. Chúng tôi đặc biệt thích phần ngắn nói về việc dùng lambda làm hash và mảng được tính toán. Một trường hợp sử dụng rất thú vị.

</div>