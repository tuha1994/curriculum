### Giới thiệu

Trong Ruby, methods là nơi mọi thứ thực sự diễn ra!

Bạn sẽ thường xuyên thấy mình viết những đoạn code thực hiện cùng một việc ở nhiều nơi khác nhau trong chương trình. Sẽ thật tuyệt nếu có cách tái sử dụng cùng một đoạn code hết lần này đến lần khác mà không phải viết lại toàn bộ mỗi lần, đúng không? Đó chính là mục đích của methods! Methods cho phép bạn đặt tên cho các phần code, rồi chạy phần code đó ở bất kỳ đâu trong chương trình, bao nhiêu lần tùy ý, chỉ bằng cách gọi tên ấy.

Khái niệm này tạo nên cách tiếp cận **DRY** trong lập trình: **Don't Repeat Yourself (Đừng lặp lại chính mình)**. Tái sử dụng các phần code thay vì lặp lại chúng trong chương trình có thể khiến chương trình dễ đọc và dễ quản lý hơn.

Khi đọc thêm về methods trong Ruby, bạn sẽ thấy các thuật ngữ "methods" và "functions" thường được dùng thay thế cho nhau. Vậy chúng khác nhau thế nào? Theo cuốn sách [The Ruby Programming Language](https://www.amazon.com/dp/0596516177/?tag=stackoverfl08-20):

> Nhiều ngôn ngữ phân biệt functions, vốn không gắn với object nào, với methods, vốn được gọi trên một receiver object. Vì Ruby là ngôn ngữ hướng đối tượng thuần túy, mọi method đều thực sự là method và được gắn với ít nhất một object.

Nói đơn giản, vì mọi thứ trong Ruby đều là object nên Ruby chỉ có methods, không có functions. Khi đã xác lập điều đó, ta biết rằng khi nói về Ruby, "methods" và "functions" chỉ cùng một thứ.

Trong bài học này, chúng ta sẽ phân tích methods là gì, xem xét hành vi của chúng và học cách sử dụng chúng.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích cách tạo và gọi một method mới.
- Giải thích cách truyền biến vào method và cách định nghĩa tham số mặc định.
- Mô tả sự khác nhau giữa implicit return và explicit return.
- Giải thích việc xâu chuỗi methods có nghĩa là gì và cách thực hiện.
- Giải thích predicate method là gì và cách sử dụng một predicate method.
- Giải thích bang method là gì và cách sử dụng một bang method.

### Các method dựng sẵn của Ruby

Một trong những ưu điểm lớn của Ruby đối với người mới lập trình là số lượng method dựng sẵn rất lớn. Bạn đã sử dụng nhiều method trong số đó, có lẽ mà không hề nhận ra. Trong quá trình học đến nay, bạn đã sửa đổi string và các object khác theo nhiều cách. Ví dụ, [vòng lặp #times](https://docs.ruby-lang.org/en/3.4/Integer.html#method-i-times) và [vòng lặp #upto](https://docs.ruby-lang.org/en/3.4/Integer.html#method-i-upto) mà bạn học trong bài Loops đều là các method thuộc [class Integer](https://docs.ruby-lang.org/en/3.4/Integer.html) của Ruby.

Nếu bạn thắc mắc về các dấu thăng (`#`), chúng chỉ là [quy ước viết instance method của Ruby](https://stackoverflow.com/questions/736120/why-are-methods-in-ruby-documentation-preceded-by-a-hash-sign). Ta có thể dùng chúng để viết tên đầy đủ của một instance method, chẳng hạn `Integer#upto`, hoặc chỉ viết tên method, chẳng hạn `#upto`, tùy ngữ cảnh. Lưu ý rằng trong giới phát triển, bạn không nên gọi chúng là hashtag. Tuy nhiên, nếu muốn thật ngầu, bạn có thể gọi chúng là ["octothorpe"](https://en.wiktionary.org/wiki/octothorpe). Từ đó đang rất thịnh hành.

Methods thường được gọi bằng cách thêm `.method_name` sau một instance của object chứa method đó.

```ruby
"anything".reverse
```

Trong trường hợp này, [`#reverse`](https://docs.ruby-lang.org/en/3.4/String.html#method-i-reverse) là method dựng sẵn dành cho các object [String](https://docs.ruby-lang.org/en/3.4/String.html).

Tuy nhiên, cũng có một số method dựng sẵn được Ruby cung cấp ở phạm vi toàn cục, chẳng hạn `print` và `puts`. Những method này được gọi chỉ bằng tên cùng các đối số. (Nếu quá tò mò, bạn nên biết các method này được [module Kernel](https://docs.ruby-lang.org/en/3.4/Kernel.html) cung cấp trên toàn cục thông qua [class Object](https://docs.ruby-lang.org/en/3.4/Object.html), nhưng hiện giờ bạn chưa cần biết nhiều đến thế.)

```ruby
puts "anything" #=> anything
```

Đáng lưu ý rằng trong hầu hết ngôn ngữ, đối số được truyền vào method bằng cách đặt chúng trong dấu ngoặc `()`. Tuy nhiên, trong Ruby, dấu ngoặc thường là *tùy chọn*. Ta có thể viết lại code trên thành `puts("anything")`, và Ruby sẽ hiểu theo cùng một cách.

### Tạo một method

Bạn có thể tạo method tùy chỉnh của riêng mình trong Ruby bằng cú pháp sau:

```ruby
def my_name
  "Joe Smith"
end

puts my_name    #=> "Joe Smith"
```

Hãy phân tích ví dụ này:

- `def` là một keyword dựng sẵn cho Ruby biết đây là phần bắt đầu của định nghĩa method.
- `my_name` là tên method mới. Bạn có thể đặt tên method gần như tùy ý, nhưng có một số giới hạn và quy ước được mô tả trong phần tiếp theo.
- `"Joe Smith"` là code bên trong thân method. Toàn bộ logic của method nằm trong các dòng được thụt lề thuộc thân method. Method cụ thể này trả về một string khi được gọi.
- `end` là keyword dựng sẵn cho Ruby biết đây là phần kết thúc định nghĩa method.
- Để gọi method, bạn cần dùng tên của nó, như thể hiện ở dòng cuối của ví dụ.

### Tên method

Như đã nói ở trên, bạn có thể đặt tên method gần như tùy ý, nhưng không nên chọn tên một cách tùy tiện. Có một số quy ước được khuyến nghị nhằm cải thiện khả năng đọc và bảo trì code.

Tên method có thể dùng chữ số, chữ hoa, chữ thường và các ký tự đặc biệt `_`, `?`, `!` và `=`. Cũng giống tên biến trong Ruby, quy ước đặt tên method gồm nhiều từ là dùng **snake_case**, ngăn cách các từ bằng dấu gạch dưới.

Nên bắt đầu tên method bằng chữ thường, vì [những tên bắt đầu bằng chữ hoa là hằng số trong Ruby](https://stackoverflow.com/questions/10542354/what-are-the-restrictions-for-method-names-in-ruby).

Sau đây là những việc bạn không được làm với tên method:

- Bạn không thể đặt tên method trùng với một trong [khoảng 40 từ dành riêng của Ruby](https://docs.ruby-lang.org/en/3.4/syntax/keywords_rdoc.html), chẳng hạn `end`, `while` hoặc `for`.
- Bạn không thể dùng ký hiệu nào khác ngoài `_`, `?`, `!` và `=`.
- Bạn không thể dùng `?`, `!` hoặc `=` ở vị trí nào khác ngoài cuối tên.
- Bạn không thể bắt đầu tên method bằng một chữ số.

Dưới đây là một số ví dụ về tên method hợp lệ và không hợp lệ:

```ruby
method_name      # valid
_name_of_method  # valid
1_method_name    # invalid
method_27        # valid
method?_name     # invalid
method_name!     # valid
begin            # invalid (Ruby reserved word)
begin_count      # valid
```

Bạn có thể nói vì sao một số tên này không hợp lệ không?

Nhìn chung, ngắn gọn nhưng có tính mô tả là nguyên tắc khi đặt tên. Bạn muốn có thể biết một method dự kiến làm gì chỉ dựa vào tên của nó, vì vậy đừng đặt tên method là `do_stuff`.

Nếu method làm quá nhiều việc đến mức bạn cảm thấy cần một cái tên rất dài, có lẽ nên chia method đó thành vài method nhỏ và đơn giản hơn. Lý tưởng nhất là mỗi method chỉ nên làm một việc. Thói quen này sẽ đem lại lợi ích lâu dài về khả năng đọc, khả năng mở rộng và khả năng bảo trì. (Nó cũng khiến việc kiểm thử code dễ hơn nhiều, nội dung sẽ được đề cập trong một bài học sau.)

### Parameters và arguments

Tất nhiên, không phải method nào cũng cơ bản như method ví dụ `my_name` ở trên. Suy cho cùng, methods có ích gì nếu bạn không thể tương tác với chúng? Khi muốn trả về thứ gì đó ngoài một kết quả cố định, bạn cần cung cấp parameters cho method. **Parameters** là các biến mà method sẽ nhận khi được gọi. Dùng parameters để khiến method linh hoạt hơn sẽ giúp bạn tương tác với chúng theo những cách có ý nghĩa và hữu ích hơn.

```ruby
def greet(name)
  "Hello, " + name + "!"
end

puts greet("John") #=> Hello, John!
```

Trong ví dụ này, `name` là parameter mà method `greet` dùng để trả về lời chào cụ thể hơn. Method được gọi với argument `"John"`, và trả về string "Hello John!"

Nếu bạn thắc mắc argument và parameter khác nhau thế nào, **parameters** đóng vai trò là biến giữ chỗ trong khuôn mẫu method, còn **arguments** là các giá trị thực tế được truyền vào method khi nó được gọi. Vì vậy, trong ví dụ trên, `name` là parameter và `"John"` là argument. Tuy nhiên, hai thuật ngữ này thường được dùng thay thế cho nhau, nên bạn không cần quá lo về sự phân biệt này.

#### Parameters mặc định

Nếu không phải lúc nào bạn cũng muốn cung cấp argument cho từng parameter mà method chấp nhận thì sao? Đó là lúc parameters mặc định trở nên hữu ích. Quay lại ví dụ trên, điều gì xảy ra nếu ta không biết tên người đó? Ta có thể đổi method `greet` để dùng `name` mặc định là "stranger":

```ruby
def greet(name = "stranger")
  "Hello, " + name + "!"
end

puts greet("Jane") #=> Hello, Jane!
puts greet #=> Hello, stranger!
```

### Methods trả về gì

Một chi tiết quan trọng mà lập trình viên phải học là hiểu methods của mình **trả về** gì. Nắm rõ method đang trả về điều gì là một phần quan trọng trong việc debug code khi mọi thứ không hoạt động như mong đợi.

Làm thế nào để cho method biết cần trả về gì? Hãy xem lại method ví dụ `my_name`:

```ruby
def my_name
  "Joe Smith"
end

puts my_name #=> "Joe Smith"
```

Method `my_name` trả về "Joe Smith". Điều này có vẻ hiển nhiên vì đó là thứ duy nhất bên trong method. Tuy nhiên, trong hầu hết ngôn ngữ, một method như vậy sẽ không trả về gì vì nó không có câu lệnh **explicit return**, tức câu lệnh bắt đầu bằng keyword `return`. Ví dụ trên cũng có thể dễ dàng viết bằng explicit return:

```ruby
def my_name
  return "Joe Smith"
end

puts my_name #=> "Joe Smith"
```

Ruby là một trong số ít ngôn ngữ cung cấp **implicit return** cho methods, nghĩa là method Ruby sẽ trả về biểu thức cuối cùng được đánh giá ngay cả khi không có keyword `return`. Biểu thức cuối cùng được đánh giá có thể là hoặc không phải dòng cuối cùng trong code, như bạn thấy ở ví dụ sau:

```ruby
def even_odd(number)
  if number % 2 == 0
    "That is an even number."
  else
    "That is an odd number."
  end
end

puts even_odd(16) #=>  That is an even number.
puts even_odd(17) #=>  That is an odd number.
```

Ở đây, giá trị trả về của method phụ thuộc vào kết quả của điều kiện `if`. Nếu argument là số chẵn, biểu thức bên trong câu lệnh `else` không bao giờ được đánh giá, nên method `even_odd` trả về `"That is an even number."`

Dù Ruby giúp việc dùng implicit return trở nên dễ dàng, explicit return vẫn có chỗ đứng trong code Ruby. Explicit return (dùng keyword `return`) về cơ bản nói với Ruby: "Đây là biểu thức cuối cùng bạn cần đánh giá." Ví dụ này cho thấy dùng `return` sẽ dừng method:

```ruby
def my_name
  return "Joe Smith"
  "Jane Doe"
end

puts my_name #=> "Joe Smith"
```

Ví dụ, explicit return hữu ích khi bạn muốn viết một method kiểm tra lỗi đầu vào trước khi tiếp tục.

```ruby
def even_odd(number)
  unless number.is_a? Numeric
    return "A number was not entered."
  end

  if number % 2 == 0
    "That is an even number."
  else
    "That is an odd number."
  end
end

puts even_odd(20) #=>  That is an even number.
puts even_odd("Ruby") #=>  A number was not entered.
```

Bây giờ hãy thử xóa explicit `return` khỏi method trên. Method có trả về điều bạn mong đợi không?

#### Sự khác nhau giữa puts và return

Một nguồn gây nhầm lẫn phổ biến với người mới lập trình là sự khác nhau giữa `puts` và `return`.

- `puts` là một method in mọi argument bạn truyền vào ra console.
- `return` là đầu ra cuối cùng của một method, có thể được dùng ở những nơi khác trong code.

Ví dụ, ta có thể viết một method tính bình phương của một số rồi `puts` kết quả ra console.

```ruby
def puts_squared(number)
  puts number * number
end
```

Method này chỉ in giá trị đã tính ra console, chứ không trả về giá trị đó. Nếu sau đó ta viết `x = puts_squared(20)`, method sẽ in `400` ra console, nhưng biến `x` sẽ được gán giá trị `nil`. (Nếu cần ôn lại, hãy quay lại xem bài học Ruby Input and Output.)

Bây giờ hãy viết cùng method đó nhưng dùng implicit return thay cho `puts`. (Dùng explicit return cũng cho kết quả hoàn toàn giống trong ví dụ này.)

```ruby
def return_squared(number)
  number * number
end
```

Khi chạy method `return_squared`, nó sẽ không in gì ra console. Thay vào đó, nó sẽ `return` kết quả theo cách cho phép ta sử dụng kết quả trong phần còn lại của code. Ta có thể lưu đầu ra khi chạy method vào một biến (`x` trong code dưới đây) và dùng biến đó theo nhiều cách. Nếu vẫn muốn thấy kết quả của method trên console, ta có thể `puts` biến đó ra console bằng phép nội suy string.

```ruby
x = return_squared(20) #=> 400
y = 100
sum = x + y #=> 500

puts "The sum of #{x} and #{y} is #{sum}."
#=> The sum of 400 and 100 is 500.
```

### Xâu chuỗi methods

Một trong những tính chất kỳ diệu của methods, giúp bạn viết code cực kỳ ngắn gọn, là khả năng **xâu chuỗi methods**. Bạn có thể làm điều này với methods dựng sẵn của Ruby hoặc với những method tự tạo.

```ruby
phrase = ["be", "to", "not", "or", "be", "to"]

puts phrase.reverse.join(" ").capitalize
#=> "To be or not to be"
```

Khi xâu chuỗi methods như vậy, mỗi lần gọi method thực chất xây dựng dựa trên kết quả của method trước đó trong chuỗi. Quá trình này về cơ bản tạo ra các bước sau:

```ruby
["be", "to", "not", "or", "be", "to"].reverse
= ["to", "be", "or", "not", "to", "be"].join(" ")
= "to be or not to be".capitalize
= "To be or not to be"
```

### Các method predicate

Đôi khi bạn sẽ gặp các method dựng sẵn của Ruby có dấu hỏi (`?`) ở cuối tên, chẳng hạn `even?`, `odd?` hoặc `between?`. Tất cả đều là **predicate method**, một quy ước đặt tên Ruby dùng cho các method trả về Boolean, tức là trả về `true` hoặc `false`.

```ruby
puts 5.even?  #=> false
puts 6.even?  #=> true
puts 17.odd?  #=> true

puts 12.between?(10, 15)  #=> true
```

Bạn cũng có thể tạo method của riêng mình với dấu `?` ở cuối tên để cho biết nó trả về Boolean. Ruby không bắt buộc quy ước đặt tên này, nhưng sau này bạn sẽ cảm ơn bản thân vì đã tuân theo hướng dẫn đó.

### Các bang method

Hãy xem ví dụ dưới đây:

```ruby
whisper = "HELLO EVERYBODY"

puts whisper.downcase #=> "hello everybody"
puts whisper #=> "HELLO EVERYBODY"
```

Chuyện gì vậy? Ta tưởng đã chuyển thứ đó thành chữ thường rồi! Vậy tại sao khi gọi lại, nó vẫn toàn chữ hoa?

Khi gọi một method trên object, chẳng hạn string `whisper` ở trên, method đó không sửa đổi giá trị ban đầu của object. Một quy tắc chung trong lập trình là không muốn methods ghi đè lên những object mà chúng được gọi trên đó. Điều này bảo vệ bạn khỏi vô tình ghi đè dữ liệu theo cách không thể đảo ngược. Bạn *có thể* ghi đè dữ liệu bằng cách gán lại một biến một cách rõ ràng (chẳng hạn `whisper = whisper.downcase`). Một cách khác để thực hiện kiểu gán lại này là dùng **bang methods**, được đánh dấu bằng dấu chấm than (`!`) ở cuối tên method.

Bằng cách thêm `!` vào cuối method, bạn cho biết method đó thực hiện tác vụ của mình đồng thời ghi đè giá trị của object ban đầu bằng kết quả.

```ruby
puts whisper.downcase! #=> "hello everybody"
puts whisper #=> "hello everybody"
```

Viết `whisper.downcase!` tương đương với viết `whisper = whisper.downcase`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Để có phần giới thiệu tốt về tất cả khái niệm khác nhau liên quan đến methods, hãy đọc [chương Methods](https://launchschool.com/books/ruby/read/methods) trong *Introduction to Programming with Ruby* của Launch School. Nhớ làm cả các bài tập ở cuối chương!
1. Để tìm hiểu về các kiểu method argument nâng cao, hãy đọc bài viết [Ruby's Method Arguments](https://www.rubyguides.com/2018/06/rubys-method-arguments/) từ *Ruby Guides*.
1. Để hiểu sâu hơn về cách tự viết methods, hãy đọc phần [Writing Methods](http://ruby-for-beginners.rubymonstas.org/writing_methods.html) trong *Ruby for Beginners* của Ruby Monsta.
1. Hoàn thành các bài tập về methods trong [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises) mà bạn đã clone trước đó.

</div>