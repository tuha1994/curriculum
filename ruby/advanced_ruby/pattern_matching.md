### Giới thiệu

Được giới thiệu trong Ruby 2.7, pattern matching sử dụng các mẫu được chỉ định để đối chiếu với một số dữ liệu. Nếu dữ liệu phù hợp với mẫu, việc đối chiếu thành công và dữ liệu được phân rã tương ứng. Nếu không có kết quả khớp, bạn có thể cung cấp một giá trị mặc định để trả về; nếu không, `NoMatchingPatternError` sẽ được ném ra.

Kể từ Ruby 3.1, hầu hết các phần trong cú pháp pattern matching không còn được xem là thử nghiệm, vì vậy đây là lúc bạn nên nắm chắc những điều cơ bản. Ban đầu, cú pháp này có thể hơi vụng về, nhưng trong một số trường hợp, nó thực sự giúp đơn giản hóa mã Ruby. Ruby 3 cũng bổ sung một vài mẫu mới mà chúng ta sẽ giới thiệu ở cuối bài.

Nếu muốn thử các ví dụ, hãy đảm bảo bạn đang dùng ít nhất Ruby 2.7 hoặc thử một REPL trực tuyến phù hợp.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích cú pháp để triển khai một câu lệnh case dùng pattern matching.
- Giải thích những kiểu dữ liệu có thể được sử dụng trong pattern matching.
- Giải thích các giá trị có thể được trả về từ một lần pattern matching.
- Giải thích cách triển khai điều kiện bảo vệ trong câu lệnh pattern matching.
- Giải thích cách pattern matching với một Array và các Array lồng trong Array.
- Giải thích cách pattern matching với một Hash.
- Giải thích những khác biệt chính giữa pattern matching với Hash và Array.
- Giải thích cách sử dụng toán tử ghim.
- Giải thích cách sử dụng mẫu thay thế.
- Giải thích cách liên kết các kết quả khớp với một biến.
- Giải thích cách đối chiếu với các lớp tùy chỉnh của riêng bạn.

### Cơ bản

Dạng cơ bản của pattern matching là một câu lệnh case. Nó không quá khác câu lệnh case quen thuộc dùng để đối chiếu điều kiện trong Ruby, chỉ khác là thay vì `when`, giờ ta dùng `in`. Nếu trường hợp sử dụng của bạn rất đơn giản, bạn sẽ thấy dùng `in` hay `when` cũng không khác nhau, như ví dụ dưới đây minh họa.

```ruby
grade = 'C'

case grade
when 'A' then puts 'Amazing effort'
when 'B' then puts 'Good work'
when 'C' then puts 'Well done'
when 'D' then puts 'Room for improvement'
else puts 'See me'
end

# => Well done
```

cú pháp pattern matching tương ứng

```ruby
grade = 'C'

case grade
in 'A' then puts 'Amazing effort'
in 'B' then puts 'Good work'
in 'C' then puts 'Well done'
in 'D' then puts 'Room for improvement'
else puts 'See me'
end

# => Well done
```

Dạng thứ hai là cú pháp một dòng, sử dụng hash rocket quen thuộc mà bạn đã biết từ các hash.

```ruby
login = { username: 'hornby', password: 'iliketrains' }

login => { username: username }

puts "Logged in with username #{username}"

#=> Logged in with username hornby
```

Chúng ta sẽ sớm tìm hiểu cách đối chiếu với hash, nên bạn chưa cần lo về cú pháp chính xác đang diễn ra ở đây. Chỉ cần lưu ý rằng ta có thể dùng hash rocket `=>` để đối chiếu với một dạng cấu trúc nào đó. Dạng thứ hai vẫn được xem là thử nghiệm, vì vậy các ví dụ tiếp theo sẽ dùng dạng câu lệnh case.

Dạng case/in phù hợp nhất khi có nhiều điều kiện có thể khớp và bạn cần kiểm tra lần lượt tất cả chúng. Cú pháp hash rocket phù hợp nhất khi đã biết cấu trúc dữ liệu cần đối chiếu, chẳng hạn dữ liệu đăng nhập trong ví dụ trên.

### Các mẫu

Có nhiều cách để đối chiếu một đầu vào. Các mẫu có thể là:

- Bất kỳ đối tượng Ruby nào được đối chiếu bằng `===`. Mẫu đối tượng.
- Bắt giữ biến / Mẫu biến
- Mẫu as
- Mẫu thay thế
- Điều kiện bảo vệ
- Mẫu Array
- Mẫu Hash

Bạn có thể sử dụng các mẫu trên cùng với những bổ sung thử nghiệm sau đây.

- Phép gán hướng phải
- Mẫu tìm kiếm

Các mẫu trên cũng có thể được kết hợp với nhau. Ví dụ, bạn có thể có một array bên trong một hash, khi đó có thể dùng cả mẫu hash lẫn mẫu array. Dưới đây chúng ta sẽ lần lượt tìm hiểu chi tiết từng loại mẫu.

Khi nói “mẫu”, chúng ta không nói đến các mẫu thiết kế mà bạn có thể từng gặp. Ở đây, chúng ta nói đến mẫu dùng để đối chiếu với một đầu vào.

### Giá trị trả về

Một câu lệnh pattern matching có hai kết quả có thể xảy ra — khớp hoặc không khớp. Nếu khớp, nó sẽ trả về giá trị được đánh giá sau cùng trong phần thân của nhánh khớp.
Nếu không có kết quả khớp, câu lệnh pattern matching sẽ trả về `NoMatchingPatternError`.
Hãy xem ví dụ sau, trong đó biến `result` được gán giá trị 3.

```ruby
grade = 'C'

result = case grade
  in 'A' then 1
  in 'B' then 2
  in 'C' then 3
  else 0
end

puts result
#=> 3
```

Khi `puts` một giá trị bên trong câu lệnh case, chúng ta dùng `#=>` để cho biết `puts` sẽ in gì.
Tuy nhiên, trong terminal, bạn sẽ thấy giá trị được in ra kèm theo `=> nil`, vì `puts` trả về `nil`.
Chúng ta sẽ bỏ qua phần đó vì nó không liên quan đến điều muốn minh họa.
Chỉ cần nhớ rằng `nil` bạn nhìn thấy chỉ là giá trị trả về của `puts`. Đây là hành vi tiêu chuẩn của Ruby.

Như bạn sẽ thấy, mục đích của pattern matching thường không chỉ là đối chiếu với một mẫu, mà còn liên kết toàn bộ hoặc một phần kết quả khớp với một hay nhiều biến để có thể sử dụng chúng bên ngoài biểu thức pattern matching.

### Đối chiếu theo mẫu đối tượng

Bất kỳ đối tượng nào cũng có thể được sử dụng trong pattern matching. Đối tượng được đối chiếu bằng `===` để so sánh hai đối tượng, và đây cũng là cơ sở của việc đối chiếu trong dạng case/when. Mẫu này thường được dùng bên trong các mẫu khác, chẳng hạn mẫu array.

Ví dụ về điểm số ở trên là một trường hợp sử dụng mẫu đối tượng của Ruby. Ở mỗi lần kiểm tra, Ruby so sánh điểm số với chữ cái được chỉ định. Khi khớp, nó thực thi mệnh đề then của lần đối chiếu đó. `'A' === 'A' #=> true`.

Nhờ vậy, chúng ta cũng có thể kiểm tra kiểu dữ liệu.

```ruby
input = 3

case input
in String then puts 'input was of type String'
in Integer then puts 'input was of type Integer'
end

#=> input was of type Integer
```

Điều quan trọng cần lưu ý là Ruby đặt mẫu cần đối chiếu ở bên trái phép so sánh trong `===`. Nếu không, việc đối chiếu sẽ không thành công. `3 === Integer` là false, trong khi `Integer === 3` là true. Điều này là do cách Ruby triển khai phương thức `===` trên Integer và trên các thể hiện của Integer. Với Integer, `===` kiểm tra toán hạng bên phải của phép so sánh có kiểu Integer hay không. Với một thể hiện của số nguyên, `===` kiểm tra hai bên có cùng giá trị hay không. `3 === 3` là true.

Với pattern matching của Ruby, bạn có thể đối chiếu các kiểu literal sau đây.

- Boolean
- nil
- Số
- Chuỗi
- Symbol
- Array
- Hash
- Khoảng
- Biểu thức chính quy
- Proc

Điều này không khác những gì ta có thể làm với câu lệnh case/when, nhưng sức mạnh xuất hiện khi bạn đối chiếu các kiểu này bên trong một số mẫu khác, như chúng ta sẽ thấy tiếp theo.

### Mẫu biến

Mẫu biến liên kết một hoặc nhiều biến với các giá trị khớp với mẫu.

Dạng cơ bản là:

```ruby
age = 15

case age
in a
  puts a
end

# => 15
```

Bản thân ví dụ này không hữu ích lắm, nhưng nó đặt nền tảng cho việc sử dụng trong các mẫu khác, như chúng ta sẽ thấy.

Điều cần lưu ý là mẫu biến luôn liên kết giá trị với biến. Hãy cẩn thận nếu trong phạm vi bên ngoài có một biến cùng tên mà bạn cần sử dụng.

```ruby
a = 5

case 1
in a
  a
end

puts a
#=> 1
```

Ở trên, có thể bạn nghĩ rằng mình đang so sánh giá trị ban đầu nằm trong biến `a` với giá trị `1` từ câu lệnh case. Thực tế, điều xảy ra là một phép pattern matching gán biến. Để tránh điều này, Ruby cung cấp toán tử ghim `^`, qua đó đối chiếu với biến có tên tương ứng.

```ruby
a = 5

case 1
in ^a
  :no_match
end

#=> NoMatchingPatternError
```

### Đối chiếu theo mẫu as

Mẫu as tương tự mẫu biến, nhưng có thể dùng để xử lý những phép gán phức tạp hơn. Mẫu này sẽ dễ hiểu nhất khi dùng với Array và Hash, vì vậy các ví dụ sẽ được trình bày ở phần nói về những mẫu đó; còn dạng tổng quát được viết như sau:

```ruby
case 3
in 3 => a
  puts a
end

# => 3
```

Mẫu này dùng hash rocket giống như pattern matching một dòng. Hãy chú ý các ví dụ ở những phần sau để hiểu rõ khi nào mẫu này hữu ích.

### Đối chiếu theo mẫu thay thế

Mẫu thay thế cho phép bạn kiểm tra xem đầu vào có khớp với một trong nhiều lựa chọn hay không.

```ruby
case 0
in 0 | 1 | 2
  puts :match
end

# => match
```

Mẫu này không có gì nhiều hơn thế. Các quy tắc giống với mọi mẫu khác. Vì vậy, nếu dùng các biến để so sánh, bạn có thể sử dụng toán tử `^`.

### Điều kiện bảo vệ

Đây không hẳn là một mẫu, mà là cách đảm bảo mẫu chỉ được khớp khi điều kiện bảo vệ có giá trị true.

```ruby
some_other_value = true

case 0
in 0 if some_other_value
  puts :match
end

# => match
```

Bạn có thể dùng bất kỳ biểu thức nào trong điều kiện, miễn là biểu thức đó có thể được đánh giá thành true hoặc false. Nếu phù hợp, bạn cũng có thể dùng `unless` thay cho `if`.

### Đối chiếu theo mẫu Array

Có vài cách khác nhau để đối chiếu với array. Ở mức cơ bản nhất, bạn có thể đối chiếu với các phần tử chính xác trong array.

```ruby
arr = [1 ,2]

case arr
in [1, 2] then puts :match
in [3, 4] then puts :no_match
end

# => match
```

Cách này hoạt động nhờ toán tử `===` của mẫu đối tượng, nên cũng sẽ hoạt động với case/when. Như vậy thì chưa thú vị lắm, hãy nâng mức độ lên một chút.

```ruby
arr = [1, 2]

case arr
in [Integer, Integer]
  puts :match
in [String, String]
  puts :no_match
end

# => match
```

Ở đây, chúng ta đối chiếu kiểu của các phần tử trong array. Đây là ví dụ Ruby đối chiếu với một mẫu thay vì các giá trị thực tế trong array.

Nếu array có nhiều phần tử hơn thì sao?

```ruby
arr = [1, 2, 3]

case arr
in [Integer, Integer]
  puts :no_match
end

# => [1, 2, 3] (NoMatchingPatternError)
```

Một lỗi! Có vẻ Ruby chỉ đối chiếu với các array có cùng số lượng phần tử. Nếu muốn đối chiếu chỉ một phần của array thì sao? Hãy dùng splat `*` quen thuộc.

```ruby
arr = [1, 2, 3]

case arr
in [Integer, Integer, *]
  puts :match
end

# => match
```

Bạn có thể đặt splat ở bất kỳ vị trí nào trong array để đối chiếu với nhiều phần tử.

```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

case arr
in [Integer, Integer, *, Integer, Integer]
  puts :match
end

# => match
```

Ở đây, chúng ta chỉ kiểm tra hai phần tử đầu và hai phần tử cuối có phải là Integer hay không. Bạn cũng có thể kết hợp việc kiểm tra giá trị thực tế với việc kiểm tra kiểu dữ liệu.

```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

case arr
in [Integer, Integer, *, 9, 10]
  puts :match
end

# => match
```

Nếu muốn gom các giá trị trong array được đối chiếu với splat, bạn có thể dùng mẫu biến.

```ruby
arr = [1, 2, 3, 4]

case arr
in [1, 2, *tail]
  p tail
end

# => [3, 4]
```

Nếu không quan tâm đến một số giá trị trong array và chấp nhận đối chiếu với bất kỳ giá trị nào tại vị trí đó, bạn có thể dùng `_`.

```ruby
arr = [1, 2, 3, 4]

case arr
in [_, _, 3, 4]
  puts :match
end

# => match
```

Giả sử bạn muốn đối chiếu với một array gồm hai số, nhưng chỉ khi hai số đó khác nhau. Bạn có thể dùng mệnh đề bảo vệ.

```ruby
arr = [1, 2]
case arr
in [a, b] unless a == b
  puts :match
end

# => match
```

Bạn thậm chí có thể đối chiếu với các array lồng nhau.

```ruby
arr = [1, 2, [3, 4]]

case arr
in [_, _, [3, 4]]
  puts :match
end

# => match
```

Đây là nơi sức mạnh thực sự của pattern matching tỏa sáng: duyệt qua các cấu trúc lồng nhau sâu để tìm kết quả khớp.

Bạn có thể kết hợp mẫu biến để liên kết các giá trị khớp với các biến và sử dụng chúng về sau.

```ruby
arr = [1, 2, 3, 4, 5]

case arr
in [1, 2, 3, a, b]
  puts a
  puts b
end

# => 4
# => 5
```

Giả sử bạn có một array lồng nhau và muốn đối chiếu cả array lồng lẫn một số phần tử riêng lẻ của nó. Đây là lúc có thể dùng mẫu as.

```ruby
case [1, 2, 3, [4, 5]]
in [1, 2, 3, [4, a] => arr]
  puts a
  p arr
end

# => 5
# => [4, 5]
```

Hãy cẩn thận với việc gán lại biến như đã thảo luận trước đó.

```ruby
arr = [1, 2, 3]

case [1, 2, 4]
in arr
  :match
end

p arr

# => [1, 2, 4]
```

Một điểm cuối cần lưu ý là khi đối chiếu với array, cặp `[]` bên ngoài là tùy chọn.

```ruby
arr = [1, 2, 3, 4]

case arr
in 1, 2, 3, 4
  puts :match
end

# => match
```

### Pattern matching với Hash

Pattern matching với Hash hoạt động tương tự array, nhưng có một vài khác biệt quan trọng. Thứ nhất, pattern matching chỉ hoạt động với các khóa symbol, không phải khóa string. Lý do liên quan đến cách Ruby đối chiếu với một hash. Điều này có thể thay đổi trong các phiên bản Ruby tương lai. Khác biệt quan trọng thứ hai là, không giống array, bạn có thể đối chiếu với một phần của Hash mà không cần xử lý khả năng còn có thêm các cặp khóa-giá trị.

Chúng ta có thể đối chiếu với các giá trị thực tế của một hash.

```ruby
case { a: 'apple', b: 'banana' }
in { a: 'aardvark', b: 'bat' }
  puts :no_match
in { a: 'apple', b: 'banana' }
  puts :match
end

# => match
```

Chúng ta có thể đối chiếu với một hash và gán các giá trị cho biến.

```ruby
case { a: 'apple', b: 'banana' }
in { a: a, b: b }
  puts a
  puts b
end

# => apple
# => banana
```

Nhờ cú pháp rút gọn của Ruby dành cho hash, chúng ta có thể viết lại đoạn trên như sau. Hãy chú ý rằng không cần cung cấp tên cho các biến. Đây không phải là hành vi riêng của pattern matching, mà là hành vi thông thường của hash trong Ruby.

```ruby
case { a: 'apple', b: 'banana' }
in { a:, b: }
  puts a
  puts b
end

# => apple
# => banana
```

Cũng như với array, khi dùng hash bạn có thể bỏ cặp ngoặc `{}`.

```ruby
case { a: 'apple', b: 'banana' }
in a:, b:
  puts a
  puts b
end

# => apple
# => banana
```

Bạn có thể dùng double splat `**` để gom nhiều cặp khóa-giá trị.

```ruby
case { a: 'ant', b: 'ball', c: 'cat' }
in { a: 'ant', **rest }
  p rest
end

# => { :b => "ball", :c => "cat" }
```

Một điều cần lưu ý với hash là vì hash có thể khớp chỉ với một tập con các khóa, bạn cần đề phòng những tình huống không mong muốn hành vi đó.

```ruby
case { a: 'ant', b: 'ball' }
in { a: 'ant' }
  'this will match'
in { a: 'ant', b: 'ball' }
  'this will never be reached'
end
```

Nếu muốn đảm bảo chỉ khớp chính xác, bạn có thể dùng `**nil`.

```ruby
case { a: 'ant', b: 'ball' }
in { a: 'ant', **nil }
  puts :no_match
in { a: 'ant', b: 'ball' }
  puts :match
end

# => match
```

Chúng ta có thể dùng mẫu as để gán toàn bộ hash đã khớp cho một biến.

```ruby
case { a: 'ant', b: 'ball' }
in { a: 'ant' } => hash
  p hash
end

#=> { :a => 'ant', :b => 'ball' }
```

### Các mẫu trong Ruby 3

Ruby 3 bổ sung một vài mẫu mới. Bạn có thể nhận được cảnh báo rằng các tính năng này vẫn đang thử nghiệm và có thể thay đổi, vì vậy chúng chưa được xem là sẵn sàng cho mã production.

### Phép gán hướng phải

Thông thường, bạn đặt các biến ở bên trái biểu thức liên kết một giá trị với biến đó. Với phép gán hướng phải, bạn phân rã một đối tượng rồi gán các giá trị cho các biến ở bên phải biểu thức. Thay vì dùng `=` để gán, bạn dùng hash rocket `=>`.

Tất cả các mẫu đã trình bày trước đó đều có thể được dùng với cú pháp này.

```ruby
login = { username: 'hornby', password: 'iliketrains' }

login => { username: username }

puts "Logged in with username #{username}"

#=> "Logged in with username hornby"
```

Dạng case/in phù hợp nhất khi có nhiều điều kiện có thể khớp và bạn cần kiểm tra lần lượt tất cả chúng. Cú pháp hash rocket phù hợp nhất khi đã biết cấu trúc dữ liệu cần đối chiếu, chẳng hạn dữ liệu đăng nhập trong ví dụ trên.

### Mẫu tìm kiếm

Mẫu array đối chiếu các phần tử theo vị trí, bắt đầu từ đầu array. Điều này khiến nó không phù hợp khi bạn muốn đối chiếu một giá trị có thể xuất hiện ở bất kỳ đâu trong array.

Mẫu as có thể bắt giữ toàn bộ array và mẫu biến có thể bắt giữ từng phần tử, nhưng cả hai vẫn dựa vào việc đối chiếu theo vị trí. Để cho phép đối chiếu một chuỗi con bất kể vị trí của nó, Ruby đã giới thiệu mẫu tìm kiếm.

Mẫu này hoạt động bằng cách đặt một `*` ở hai bên phần bạn muốn đối chiếu. Bạn thậm chí có thể dùng mẫu biến để đặt tên biến cho từng `*` nhằm tham chiếu về sau. Hãy xem một vài ví dụ.

```ruby
case [1, 2, 3]
in [*pre, 1, 2, 3, *post]
  p pre
  p post
end

# => []
# => []
```

Vì mọi thứ đều đã được khớp giữa mẫu, các biến `pre` và `post` của chúng ta được gán các array rỗng. Hãy xem điều gì xảy ra khi chúng không rỗng.

```ruby
case [1, 2, 3, 4, 5]
in [*pre, 2, 3, *post]
  p pre
  p post
end

# => [1]
# => [4, 5]
```

Đây là một ví dụ thú vị. Giả sử bạn có một array gồm cả string và số nguyên, và muốn tìm lần xuất hiện đầu tiên của hai string liên tiếp. Nếu không dùng pattern matching, việc triển khai điều này sẽ khá phiền; còn với mẫu tìm kiếm thì rất đơn giản. Ta thậm chí có thể thêm mẫu as để lấy các giá trị của hai string liên tiếp.

```ruby
case [1, 2, "a", 4, "b", "c", 7, 8, 9]
in [*pre, String => x, String => z, *post]
  p pre
  p x
  p z
  p post
end

# => [1, 2, "a", 4]
# => "b"
# => "c"
# => [7, 8, 9]
```

Ở ví dụ cuối, chúng ta sẽ xét một trường hợp sử dụng phổ biến, qua đó hy vọng cho thấy khi nào mẫu tìm kiếm phù hợp hơn giải pháp Ruby thông thường. Trong Ruby, việc gặp một array chứa các hash hoặc dữ liệu JSON là chuyện không hiếm, và bạn có thể cần tìm một bản ghi trong dữ liệu đó. Bạn cần đối chiếu dữ liệu dựa trên một vài khóa hash. Dữ liệu có thể trông như sau.

```ruby
data = [
  {name: 'James', age: 50, first_language: 'english', job_title: 'general manager'},
  {name: 'Jill', age: 32, first_language: 'italian', job_title: 'leet coder'},
  {name: 'Helen', age: 24, first_language: 'dutch', job_title: 'biscuit quality control'},
  {name: 'Bob', age: 64, first_language: 'english', job_title: 'table tennis king'},
  {name: 'Betty', age: 55, first_language: 'spanish', job_title: 'pie maker'},
]
```

Giả sử bạn nhận được tên, tuổi và chức danh công việc dưới dạng tham số. Nếu người đó tồn tại, bạn cần trả về ngôn ngữ đầu tiên của họ; nếu không, trả về `nil`. Trước khi có pattern matching, bạn có thể làm như sau.

```ruby
name = 'Jill'
age = 32
job_title = 'leet coder'

match = data.find do |person|
  person[:name] == name && person[:age] == age && person[:job_title] == job_title
end

match&.fetch(:first_language)
```

Có vài điểm cần lưu ý. Thứ nhất, vì `match` có thể là `nil` nếu không tìm được bản ghi, chúng ta phải dùng toán tử điều hướng an toàn `&` để kiểm tra xem có thể gọi fetch trên match hay không. Nếu không, chương trình sẽ bị lỗi do gọi phương thức không tồn tại. Thứ hai, dù đoạn mã này thực ra chưa quá tệ, hãy thử tưởng tượng ta có hơn 3 trường cần tìm. Nếu có 10 trường thì sao? Mã của chúng ta sẽ bắt đầu trở nên mất kiểm soát.

Bây giờ hãy xem có thể xử lý việc này bằng pattern matching như thế nào.

```ruby
name = 'Jill'
age = 32
job_title = 'leet coder'

case data
in [*, { name: ^name, age: ^age, first_language: first_language, job_title: ^job_title }, *]
else
  first_language = nil
end

puts first_language

# => italian
```

Với pattern matching, chúng ta cần xét cả trường hợp không có kết quả khớp. Nếu không có mệnh đề else, ta sẽ nhận lỗi không có mẫu khớp. Nhưng đó chính là mục đích của mệnh đề else trong câu lệnh case. Cách làm này đem lại một vài lợi ích. Thứ nhất, ta có thể liên kết giá trị `first_language` với một biến ngay trong mẫu. Thứ hai — và chúng tôi thừa nhận đây phần nào là ý kiến chủ quan — chúng tôi thấy câu lệnh case hữu ích ở chỗ cho thấy chính xác loại cấu trúc dữ liệu đang được đối chiếu. Điều này có thể hữu ích khi làm quen với mã sử dụng dữ liệu đến từ API bên thứ ba. Và hãy nhớ, đây vẫn là một ví dụ rất đơn giản. Trong thực tế, dữ liệu có thể lồng sâu nhiều tầng, dẫn đến một mớ mã spaghetti rối rắm khi cố đảm bảo có thể tìm được một giá trị ở nhiều tầng bên trong. Nếu phải đi sâu 6 tầng nhưng đồng thời lấy một hoặc hai giá trị trên đường đi từ vài tầng cao hơn thì sao? Pattern matching xử lý được việc đó.

### Tổng kết

Pattern Matching cung cấp một cách mạnh mẽ để lấy được dữ liệu cần thiết theo cách dễ hiểu. Nhưng cũng như mọi công cụ khác, bạn không cần dùng nó trong mọi trường hợp. Với các cấu trúc dữ liệu nông, hãy cân nhắc xem có cần đưa pattern matching vào không, khi bạn có thể lấy giá trị bằng các phương thức hash và array vốn đã có sẵn. Nếu bạn thấy mình đang xử lý một cấu trúc lồng sâu với nhiều lần kiểm tra `nil` và rất nhiều trường hợp biên, có lẽ đã đến lúc xem pattern matching có thể giúp ích gì không.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [tài liệu Ruby về pattern matching](https://docs.ruby-lang.org/en/3.4/syntax/pattern_matching_rdoc.html). Tài liệu này bao quát khá nhiều nội dung.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ trợ.

- [Giới thiệu về Pattern Matching trong Ruby](https://blog.appsignal.com/2021/07/28/introduction-to-pattern-matching-in-ruby.html) từ AppSignal có thể mang đến một góc nhìn hữu ích khác về nội dung này.
