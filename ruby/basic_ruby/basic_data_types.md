### Giới thiệu

Ruby mang tính hướng đối tượng rất cao, nghĩa là tuyệt đối mọi thứ trong Ruby đều là một đối tượng, kể cả những kiểu dữ liệu cơ bản nhất. Chúng ta sẽ bắt đầu với bốn kiểu dữ liệu cơ bản của Ruby: số (số nguyên và số thực), chuỗi, symbol và Boolean (`true`, `false` và `nil`).

Trong tất cả các ví dụ của bài học này, bạn có thể thực hành theo trong irb hoặc bất kỳ [REPL](https://www.rubyguides.com/2018/12/what-is-a-repl-in-ruby/) trực tuyến phù hợp nào để hiểu rõ hơn cách chúng hoạt động.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Liệt kê các toán tử số học cơ bản và chức năng của chúng.
- Mô tả sự khác nhau giữa số nguyên và số thực, cũng như cách chuyển đổi giữa hai kiểu này.
- Giải thích nội suy chuỗi và phép nối chuỗi.
- Mô tả ký tự escape là gì và liệt kê một số ví dụ.
- Định nghĩa symbol là gì và nó khác chuỗi như thế nào.
- Giải thích các Boolean `true`, `false` và `nil` biểu thị điều gì.

### Số

Có lẽ bạn đã biết số là gì, nên không cần dùng những phép ẩn dụ cầu kỳ ở đây. Ruby có tất cả các toán tử toán học quen thuộc mà bạn mong đợi:

```ruby
# Addition
1 + 1 #=> 2

# Subtraction
2 - 1 #=> 1

# Multiplication
2 * 2 #=> 4

# Division
10 / 5 #=> 2

# Exponent
2 ** 2 #=> 4
3 ** 4 #=> 81

# Modulus (find the remainder of division)
8 % 2 #=> 0  (8 / 2 = 4; no remainder)
10 % 4 #=> 2  (10 / 4 = 2 with a remainder of 2)
```

#### Số nguyên và số thực

Ruby có hai kiểu số chính. **Số nguyên** là các số không có phần thập phân, chẳng hạn 10. **Số thực** là các số có dấu thập phân, chẳng hạn 10.5, 10.0 hoặc 0.25.

Điều quan trọng cần nhớ là khi thực hiện phép tính với hai số nguyên trong Ruby, *kết quả luôn là một số nguyên*.

```ruby
17 / 5 #=> 3, not 3.4
```

Để nhận được kết quả chính xác, chỉ cần thay một trong các số nguyên trong biểu thức bằng một số thực.

```ruby
17 / 5.0 #=> 3.4
```

#### Chuyển đổi kiểu số

Ruby giúp việc chuyển đổi số thực sang số nguyên và ngược lại trở nên rất dễ dàng.

```ruby
# To convert an integer to a float:
13.to_f #=> 13.0

# To convert a float to an integer:
13.0.to_i #=> 13
13.9.to_i #=> 13
```

Như ví dụ cuối cho thấy, khi Ruby chuyển một số thực thành số nguyên, phần thập phân sẽ bị cắt bỏ. Ruby không làm tròn trong quá trình chuyển đổi này.

#### Một số phương thức hữu ích cho số

Ruby tích hợp sẵn rất nhiều phương thức hữu ích cho số. Ví dụ:

- `#even?`

  ```ruby
  6.even? #=> true
  7.even? #=> false
  ```

- `#odd?`

  ```ruby
  6.odd? #=> false
  7.odd? #=> true
  ```

### Chuỗi

Chuỗi, chuỗi, những thứ tuyệt vời, hãy dùng chúng thật tốt và... ứng dụng của bạn sẽ... mọc cánh? Đại khái là vậy.

Thoạt nhìn, bạn có thể nghĩ chuỗi chỉ là một đống ký tự, chẳng hữu ích mấy ngoài việc nhận dữ liệu người dùng và xuất thông tin ra màn hình; nhưng giống như Burt Reynolds đã bỏ qua cơ hội đóng Han Solo, bạn đã nhầm. Nhầm to. Burt, lúc đó bạn đang nghĩ gì vậy?

#### Dấu ngoặc kép đôi và đơn

Chuỗi có thể được tạo bằng dấu ngoặc kép đôi `""` hoặc dấu ngoặc kép đơn`''`, còn gọi là *literal chuỗi*. Chúng khá giống nhau, nhưng có một số khác biệt. Cụ thể, nội suy chuỗi và các ký tự escape mà chúng ta sắp thảo luận chỉ hoạt động bên trong dấu ngoặc kép đôi, không hoạt động bên trong dấu ngoặc kép đơn.

#### Phép nối chuỗi

Đúng với phong cách Ruby, có rất nhiều cách để nối chuỗi.

```ruby
# With the plus operator:
"Welcome " + "to " + "Odin!" #=> "Welcome to Odin!"

# With the shovel operator:
"Welcome " << "to " << "Odin!" #=> "Welcome to Odin!"

# With the concat method:
"Welcome ".concat("to ").concat("Odin!") #=> "Welcome to Odin!"
```

Ruby đúng là kinh điển!

#### Chuỗi con

Bạn có thể truy cập chuỗi nằm bên trong chuỗi. Stringception! Cách này cũng cực kỳ dễ.

```ruby
"hello"[0] #=> "h"

"hello"[0..1] #=> "he"

"hello"[0, 4] #=> "hell"

"hello"[-1] #=> "o"
```

Trong ví dụ trên, ta có thể truy cập từng ký tự của một chuỗi bằng cách tham chiếu đến chỉ mục của ký tự đó trong chuỗi thông qua `[]`. Để biết thêm thông tin, bạn có thể đọc [tài liệu về phương thức #slice](https://docs.ruby-lang.org/en/3.4/String.html#class-String-label-String+Slices).

#### Ký tự escape

Ký tự escape cho phép bạn nhập biểu diễn của các ký tự khoảng trắng và đưa dấu ngoặc kép vào chuỗi mà không vô tình kết thúc chuỗi. Xin nhắc lại, ký tự escape chỉ hoạt động bên trong dấu ngoặc kép đôi.

```ruby
\\  #=> Need a backslash in your string?
\b  #=> Backspace
\r  #=> Carriage return, for those of you that love typewriters
\n  #=> Newline. You'll likely use this one the most.
\s  #=> Space
\t  #=> Tab
\"  #=> Double quotation mark
\'  #=> Single quotation mark
```

Cách tốt nhất là thử nghịch với chúng trong irb hoặc một REPL.

```ruby
irb(main):001:0> puts "Hello \n\nHello"
Hello

Hello
=> nil
```

#### Nội suy

Nội suy chuỗi cho phép bạn đánh giá một chuỗi chứa các biến giữ chỗ. Đây là kỹ thuật rất hữu ích và phổ biến, nên bạn sẽ thường xuyên dùng đến nó. Hãy nhớ dùng dấu ngoặc kép đôi để nội suy chuỗi hoạt động!

```ruby
name = "Odin"

puts "Hello, #{name}" #=> "Hello, Odin"
puts 'Hello, #{name}' #=> "Hello, #{name}"
```

#### Các phương thức chuỗi thường dùng

Ruby tích hợp sẵn rất nhiều phương thức chuỗi hữu ích. Bạn cần viết hoa một từ ư? Không vấn đề! Đảo ngược một chuỗi ư? Dễ như ăn bánh. Trích xuất thuật toán hạ nguyên tử nhị phân từ bất kỳ grep regex nào ư? Chúng tôi không biết, nhưng vì đây là Ruby, cứ chọn *CÓ* vậy.

Chỉ cần nhớ rằng chuỗi có vô số phương thức được cung cấp sẵn, và bạn có thể tìm thấy tất cả chúng trong [tài liệu Ruby về lớp String](https://docs.ruby-lang.org/en/3.4/String.html). Nếu đang làm việc với chuỗi và cần thực hiện một thao tác nào đó, trước hết hãy xem tài liệu Ruby để kiểm tra xem đã có phương thức làm việc đó cho bạn chưa.

Dưới đây là phần ôn nhanh về những phương thức chuỗi phổ biến mà bạn có thể sẽ dùng:

- `#capitalize`

  ```ruby
  "hello".capitalize #=> "Hello"
  ```

- `#include?`

  ```ruby
  "hello".include?("lo") #=> true

  "hello".include?("z") #=> false
  ```

- `#upcase`

  ```ruby
  "hello".upcase #=> "HELLO"
  ```

- `#downcase`

  ```ruby
  "Hello".downcase #=> "hello"
  ```

- `#empty?`

  ```ruby
  "hello".empty? #=> false

  "".empty? #=> true
  ```

- `#length`

  ```ruby
  "hello".length #=> 5
  ```

- `#reverse`

  ```ruby
  "hello".reverse #=> "olleh"
  ```

- `#split`

  ```ruby
  "hello world".split #=> ["hello", "world"]

  "hello".split("") #=> ["h", "e", "l", "l", "o"]
  ```

- `#strip`

  ```ruby
  " hello, world   ".strip #=> "hello, world"
  ```

Bạn sẽ đọc thêm về những phương thức này và các phương thức khác trong bài tập. Các ví dụ dưới đây chỉ nhằm khơi nguồn sáng tạo bằng một vài cách tuyệt vời để bạn biến đổi chuỗi.

```ruby
"he77o".sub("7", "l") #=> "hel7o"

"he77o".gsub("7", "l") #=> "hello"

"hello".insert(-1, " dude") #=> "hello dude"

"hello world".delete("l") #=> "heo word"

"!".prepend("hello, ", "world") #=> "hello, world!"
```

Các bài tập sẽ đi sâu hơn nhiều, vì vậy hãy làm thật kỹ và nhớ thử nghiệm trong một REPL khi đọc.

#### Chuyển các đối tượng khác thành chuỗi

Bằng phương thức `to_s`, bạn có thể chuyển gần như bất cứ thứ gì thành chuỗi. Dưới đây là một vài ví dụ:

```ruby
5.to_s #=> "5"

nil.to_s #=> ""

:symbol.to_s #=> "symbol"
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Thao tác làm biến đổi literal chuỗi

Hãy cẩn thận khi thực hiện các thao tác làm biến đổi literal chuỗi, vì Ruby sẽ sớm không còn cho phép điều này nữa.

</div>

### Symbol

Symbol là một biến thể thú vị của ý tưởng chuỗi. Phần giải thích đầy đủ có thể hơi dài, nhưng đây là phiên bản ngắn gọn:

Chuỗi có thể thay đổi, vì vậy mỗi lần một chuỗi được sử dụng, Ruby phải lưu nó trong bộ nhớ, ngay cả khi đã có một chuỗi khác có cùng giá trị tồn tại. Ngược lại, symbol chỉ được lưu trong bộ nhớ một lần, khiến chúng nhanh hơn trong một số tình huống.

Một trường hợp phổ biến mà symbol được ưu tiên hơn chuỗi là làm khóa trong hash. Chúng ta sẽ tìm hiểu chi tiết điều này trong bài học về hash ở phần sau của khóa học.

Ban đầu bạn sẽ không cần dùng symbol nhiều, nhưng làm quen với chúng là điều hữu ích để có thể nhận ra chúng và hình dung chúng trông như thế nào.

#### Tạo một symbol

Để tạo symbol, hãy đặt dấu hai chấm ở đầu một đoạn văn bản:

```ruby
:my_symbol
```

#### Symbol và chuỗi

Để hiểu rõ hơn cách symbol được lưu trong bộ nhớ, hãy thử chạy đoạn này trong irb hoặc một REPL. [Phương thức `#object_id`](https://docs.ruby-lang.org/en/3.4/Object.html#method-i-object_id) trả về một mã định danh số nguyên cho một đối tượng. (Và hãy nhớ: trong Ruby, *mọi thứ* đều là một đối tượng!)

```ruby
"string" == "string" #=> true

"string".object_id == "string".object_id #=> false

:symbol.object_id == :symbol.object_id #=> true
```

### Boolean

Bạn sẽ học chi tiết hơn về các kiểu dữ liệu này trong bài Logic điều kiện (Conditional Logic) ở phần sau của khóa học. Mục tiêu của bài này là giúp bạn có hiểu biết cơ bản về Boolean.

#### True và false

Các giá trị Boolean `true` và `false` biểu thị đúng như tên gọi: `true` biểu thị điều đúng, còn `false` biểu thị điều sai.

#### Nil

Trong Ruby, `nil` biểu thị “không có gì”. Mọi thứ trong Ruby đều có một giá trị trả về. Khi một đoạn mã không có gì để trả về, nó sẽ trả về `nil`. Điều này khá trừu tượng, nhưng sẽ dần dễ hiểu hơn khi bạn học và sử dụng Ruby.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Nếu gặp khó khăn, hãy đặt câu hỏi trong kênh #ruby-help của máy chủ Discord của chúng ta!

1. Đọc [chương Basics của *Introduction to Programming With Ruby* của LaunchSchool](https://launchschool.com/books/ruby/read/basics) để xem một cách giải thích khác về các kiểu dữ liệu của Ruby. Nhớ hoàn thành cả các bài tập.
1. Đọc [bài viết ngắn của Alex Chaffee về Objects](https://codelikethis.com/lessons/learn-to-code/objects) để hiểu cơ bản về đối tượng trong lập trình. Chúng ta sẽ tìm hiểu chủ đề này sâu hơn nhiều ở phần sau của khóa học.
1. Bây giờ là lúc hoàn thành một số bài tập theo hướng kiểm thử trước để thực hành những gì bạn đã học trong bài.
   - Làm theo [hướng dẫn sao chép và cài đặt trong README chính của kho ruby-exercise](https://github.com/TheOdinProject/ruby-exercises?tab=readme-ov-file#cloning-and-installation) để thiết lập bài tập.
   - Sau đó, làm theo [hướng dẫn sử dụng trong README của ruby-basics](https://github.com/TheOdinProject/ruby-exercises/tree/main/ruby_basics#usage) để hoàn thành các bài tập trong thư mục `1_data_types`.
   - Sau khi hoàn thành bài tập, hãy xem nhánh solutions trên GitHub để đối chiếu lời giải.

</div>
