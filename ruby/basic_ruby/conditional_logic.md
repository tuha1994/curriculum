### Giới thiệu

Bài học này tập trung vào việc điều khiển luồng thực thi của mã. Khi có một đoạn mã chỉ muốn chạy trong những điều kiện cụ thể, bạn cần một cách để máy tính kiểm tra xem các điều kiện đó đã được đáp ứng hay chưa. Logic điều kiện xuất hiện ở khắp nơi trong đời sống hằng ngày. Bạn đã bao giờ phải dọn phòng trước khi được phép chơi trò chơi điện tử chưa? Đó là mẹ bạn đang thiết lập một câu lệnh điều kiện, có thể trông như thế này trong một chương trình máy tính...

```ruby
if room_tidy == true
  "I can play video games"
end
```

Hẳn phải có đâu đó một trò đùa về bo mạch chủ. Câu trả lời xin gửi trên bưu thiếp!

Mọi câu lệnh điều kiện luôn có một biểu thức cho kết quả là `true` hoặc `false`. Dựa trên kết quả này, máy tính sẽ quyết định có thực thi đoạn mã tiếp theo hay không. Nếu kết quả là true, đoạn mã sẽ được xử lý; nếu là false, đoạn mã sẽ bị bỏ qua, và bạn có thể cung cấp một đoạn mã khác để chạy thay thế. Thậm chí có thể có nhiều câu lệnh điều kiện trên cùng một dòng, nhưng hãy nhớ rằng quá nhiều câu lệnh có thể khiến mã trở nên rối mắt.

Bạn sẽ viết rất nhiều câu lệnh điều kiện trên con đường trở thành ngôi sao lập trình. Dù về cơ bản chúng khá đơn giản, chúng lại là nguồn gây lỗi lớn trong mã của bạn khi mọi thứ không hoạt động như mong đợi. Hãy chắc chắn rằng bạn hiểu logic đằng sau biểu thức được đánh giá, để có thể lần theo từng bước nếu cần.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Mô tả và liệt kê các giá trị falsy.
- Giải thích cách sử dụng `if`, `elsif` và `else`.
- Giải thích sự khác nhau giữa `if` và `unless`.
- Mô tả chức năng của `||`, `&&` và `!`.
- Giải thích đánh giá đoản mạch là gì.
- Mô tả toán tử ba ngôi và cách sử dụng nó.
- Giải thích câu lệnh `case` là gì và cách nó hoạt động.

### Truthy và falsy trong Ruby

Bạn đã biết câu lệnh điều kiện kiểm tra các biểu thức để nhận giá trị true hoặc false, vì vậy bạn cần hiểu Ruby xem điều gì là true hoặc false. Đúng với phong cách Ruby điển hình, việc này rất đơn giản. Các giá trị false duy nhất trong Ruby chính là `nil` và `false`. Chỉ vậy thôi! Mọi thứ khác đều được xem là true. Ngay cả chuỗi `"false"` cũng là true trong các biểu thức điều kiện! Nếu từng làm việc với những ngôn ngữ lập trình khác, bạn có thể quen với việc số 0 hoặc chuỗi rỗng ("") tương đương với false. Ruby không như vậy, nên hãy cẩn thận khi viết các biểu thức đó. Nếu không, màn hình của bạn có thể đầy lỗi hơn cả khi dùng chế độ sáng lúc nửa đêm.

### Câu lệnh điều kiện cơ bản

Cách đơn giản nhất để điều khiển luồng mã bằng điều kiện là dùng câu lệnh `if`.

Cú pháp tổng quát của câu lệnh `if` được minh họa ở đây:

```ruby
if statement_to_be_evaluated == true
  # do something awesome...
end

if 1 < 2
  puts "Hot diggity, 1 is less than 2!"
end
#=> Hot diggity, 1 is less than 2!
```

Nếu trong khối chỉ có một dòng mã cần đánh giá, bạn có thể viết lại mã ngắn gọn hơn và chỉ chiếm một dòng:

```ruby
puts "Hot diggity damn, 1 is less than 2" if 1 < 2
```

Bạn thậm chí không cần câu lệnh `end`. Thật gọn gàng và súc tích!

Câu lệnh cần được đánh giá có thể là bất cứ thứ gì trả về true hoặc false. Đó có thể là một biểu thức toán học, giá trị của một biến hoặc một lệnh gọi phương thức. Chỉ khi biểu thức cho kết quả true thì đoạn mã bên trong khối mới chạy.

### Thêm else và elsif

Chúng ta thường muốn kiểm tra một điều kiện và chạy một đoạn mã nếu điều kiện đúng, rồi chạy một đoạn mã khác nếu điều kiện sai. Việc này được thực hiện bằng câu lệnh `if...else`.

```ruby
if attack_by_land == true
  puts "release the goat"
else
  puts "release the shark"
end
```

Ồ đúng rồi! Bảo vệ cả trên cạn lẫn dưới biển.

Nhưng nếu chúng ta bị tấn công từ trên không thì sao? Ta cần thêm một lần kiểm tra điều kiện nữa. Hãy dùng câu lệnh `if...elsif...else`!

```ruby
if attack_by_land == true
  puts "release the goat"
elsif attack_by_sea == true
  puts "release the shark"
else
  puts "release Kevin the flying octopus "
end
```

Không ai có thể cướp bóc vùng đất của chúng ta!

Bạn có thể có bao nhiêu biểu thức `elsif` tùy thích. Mệnh đề `else` là tùy chọn, nhưng thông thường bạn nên cung cấp một giá trị mặc định phòng khi không biểu thức nào trước đó cho kết quả `true`.

### Logic Boolean

Để xác định một biểu thức cho kết quả `true` hay `false`, bạn sẽ cần một toán tử so sánh. Ruby cung cấp một số toán tử như sau:

`==` (bằng) trả về `true` nếu các giá trị được so sánh bằng nhau.

```ruby
5 == 5 #=> true
5 == 6 #=> false
```

`!=` (khác) trả về `true` nếu các giá trị được so sánh không bằng nhau.

```ruby
5 != 7 #=> true
5 != 5 #=> false
```

`>` (lớn hơn) trả về `true` nếu giá trị bên trái toán tử lớn hơn giá trị bên phải.

```ruby
7 > 5 #=> true
5 > 7 #=> false
```

`<` (nhỏ hơn) trả về `true` nếu giá trị bên trái toán tử nhỏ hơn giá trị bên phải.

```ruby
5 < 7 #=> true
7 < 5 #=> false
```

`>=` (lớn hơn hoặc bằng) trả về `true` nếu giá trị bên trái toán tử lớn hơn hoặc bằng giá trị bên phải.

```ruby
7 >= 7 #=> true
7 >= 5 #=> true
```

`<=` (nhỏ hơn hoặc bằng) trả về `true` nếu giá trị bên trái toán tử nhỏ hơn hoặc bằng giá trị bên phải.

```ruby
5 <= 5 #=> true
5 <= 7 #=> true
```

`#eql?` kiểm tra cả kiểu giá trị lẫn giá trị thực tế mà nó chứa.

```ruby
5.eql?(5.0) #=> false; although they are the same value, one is an integer and the other is a float
5.eql?(5)   #=> true
```

`#equal?` kiểm tra xem cả hai giá trị có phải chính xác là cùng một đối tượng trong bộ nhớ hay không. Điều này có thể hơi khó hiểu vì cách máy tính lưu trữ một số giá trị để tăng hiệu quả. Hai biến cùng trỏ đến một số thường sẽ trả về `true`.

```ruby
a = 5
b = 5
a.equal?(b) #=> true
```

Biểu thức này là true vì cách máy tính lưu số nguyên trong bộ nhớ. Dù hai biến khác nhau cùng chứa số 5, chúng trỏ đến cùng một đối tượng trong bộ nhớ. Tuy nhiên, hãy xem ví dụ mã tiếp theo:

```ruby
a = "hello"
b = "hello"
a.equal?(b) #=> false
```

Điều này xảy ra vì máy tính không thể lưu chuỗi theo cách hiệu quả giống như khi lưu số. Dù giá trị của các biến giống nhau, máy tính đã tạo ra hai đối tượng chuỗi riêng biệt trong bộ nhớ.

Ngoài các toán tử trên, Ruby còn có một toán tử đặc biệt thường được gọi thân mật là **toán tử phi thuyền**. Không giống các toán tử so sánh khác, vốn đều trả về `true` hoặc `false`, toán tử phi thuyền trả về một trong ba giá trị số.

`<=>` (toán tử phi thuyền) trả về:

- `-1` nếu giá trị bên trái nhỏ hơn giá trị bên phải;
- `0` nếu giá trị bên trái bằng giá trị bên phải; và
- `1` nếu giá trị bên trái lớn hơn giá trị bên phải.

```ruby
5 <=> 10    #=> -1
10 <=> 10   #=> 0
10 <=> 5    #=> 1
```

Toán tử phi thuyền thường được dùng nhất trong các hàm sắp xếp, nội dung này sẽ được trình bày kỹ hơn ở phần sau.

Tất cả các toán tử trên cũng hoạt động với những kiểu dữ liệu khác ngoài số, chẳng hạn như chuỗi. Sao không thử nghịch với chúng trong một REPL?

### Toán tử logic

Đôi khi bạn sẽ muốn viết một biểu thức chứa nhiều hơn một điều kiện. Trong Ruby, việc này được thực hiện bằng các toán tử logic: `&&` (và), `||` (hoặc) và `!` (phủ định).

Có một số khác biệt giữa các phiên bản dạng từ và tương đương dạng ký hiệu, đặc biệt là ở cách chúng đánh giá mã. Chúng tôi khuyên bạn xem video giải thích [sự khác nhau giữa toán tử logic dạng ký hiệu và dạng từ](https://avdi.codes/how-to-use-rubys-english-andor-operators-without-going-nuts/).

Toán tử `&&` trả về `true` nếu **cả biểu thức bên trái và bên phải đều trả về `true`**.

```ruby
if 1 < 2 && 5 < 6
  puts "Party at Kevin's!"
end

# This can also be written as
if 1 < 2 and 5 < 6
  puts "Party at Kevin's!"
end
```

Một điều cần nhớ với các toán tử `&&` và `||` là thứ tự logic. Các biểu thức luôn được đánh giá từ trái sang phải.

Với toán tử `&&`, cả hai biểu thức đều phải trả về `true`. Nếu biểu thức đầu tiên được gặp trả về `false`, biểu thức thứ hai sẽ không bao giờ được kiểm tra. Với trình thông dịch Ruby, việc đánh giá nửa sau là vô ích vì toàn bộ biểu thức không thể nào trả về `true`.

Với toán tử `||`, nếu biểu thức đầu tiên cho kết quả `true`, biểu thức thứ hai sẽ không được kiểm tra vì toàn bộ biểu thức đã là `true`, và mã trong khối sẽ được chạy.

Điều này được gọi là **đánh giá đoản mạch**.

```ruby
if 10 < 2 || 5 < 6 #=> although the left expression is false, there is a party at Kevin's because the right expression returns true
  puts "Party at Kevin's!"
end

# This can also be written as
if 10 < 2 or 5 < 6
  puts "Party at Kevin's!"
end
```

Toán tử `!` đảo ngược logic của biểu thức. Vì vậy, nếu bản thân biểu thức trả về `false`, việc dùng toán tử `!` sẽ khiến biểu thức trở thành `true`, và mã bên trong khối sẽ được thực thi.

```ruby
if !false     #=> true

if !(10 < 5)  #=> true
```

### Câu lệnh case

Câu lệnh case là một cách gọn gàng để viết nhiều biểu thức điều kiện mà nếu viết theo cách thông thường sẽ tạo ra một câu lệnh `if...elsif` lộn xộn. Bạn thậm chí có thể gán giá trị trả về từ câu lệnh case cho một biến để dùng sau này.

Câu lệnh case xử lý lần lượt từng điều kiện; nếu điều kiện trả về `false`, nó chuyển sang điều kiện tiếp theo cho đến khi tìm thấy kết quả khớp. Có thể cung cấp mệnh đề `else` làm giá trị mặc định nếu không tìm thấy kết quả khớp.

```ruby
grade = 'F'

did_i_pass = case grade #=> create a variable `did_i_pass` and assign the result of a call to case with the variable grade passed in
  when 'A' then "Hell yeah!"
  when 'D' then "Don't tell your mother."
  else "'YOU SHALL NOT PASS!' -Gandalf"
end
```

Ngay khi tìm thấy kết quả khớp, giá trị của kết quả đó được trả về và câu lệnh case dừng thực thi. Bạn có đoán được giá trị của biến `did_i_pass` sau câu lệnh case sẽ là gì không?

Nếu cần thao tác mã phức tạp hơn, bạn có thể bỏ từ khóa `then` và đặt mã cần thực thi ở dòng tiếp theo.

```ruby
grade = 'F'

case grade
when 'A'
  puts "You're a genius"
  future_bank_account_balance = 5_000_000
when 'D'
  puts "Better luck next time"
  can_i_retire_soon = false
else
  puts "'YOU SHALL NOT PASS!' -Gandalf"
  fml = true
end
```

### Câu lệnh unless

Câu lệnh `unless` hoạt động ngược với câu lệnh `if`: nó chỉ xử lý mã trong khối nếu biểu thức cho kết quả `false`. Không có gì phức tạp hơn thế.

```ruby
age = 19
unless age < 18
  puts "Get a job."
end
```

Cũng giống câu lệnh `if`, bạn có thể viết câu lệnh `unless` trên một dòng, đồng thời có thể thêm mệnh đề `else`.

```ruby
age = 19
puts "Welcome to a life of debt." unless age < 18

unless age < 18
  puts "Down with that sort of thing."
else
  puts "Careful now!"
end
```

Bạn nên dùng câu lệnh `unless` khi muốn **không làm điều gì đó nếu một điều kiện là `true`**, vì cách này có thể khiến mã dễ đọc hơn so với việc dùng `if !true`.

### Toán tử ba ngôi

Toán tử ba ngôi là một câu lệnh `if...else` trên một dòng, giúp mã của bạn ngắn gọn hơn nhiều.

Cú pháp của nó là `condition ? <execute if true> : <execute if false>`. Bạn có thể gán giá trị trả về của biểu thức cho một biến.

```ruby
age = 19
response = age < 18 ? "You still have your entire life ahead of you." : "You're all grown up."
puts response #=> "You're all grown up."
```

Ở đây, vì biểu thức cho kết quả `false`, mã sau dấu `:` được gán cho biến `response`.

Viết lại dưới dạng câu lệnh `if...else` sẽ dài dòng hơn nhiều:

```ruby
age = 19
if age < 18
  response = "You still have your entire life ahead of you."
else
  response = "You're all grown up."
end

puts response #=> "You're all grown up."
```

Tuy nhiên, nếu các câu lệnh điều kiện của bạn phức tạp, dùng câu lệnh `if...else` có thể giúp mã dễ đọc hơn. Hãy nhớ rằng, **trên hết, mã của bạn cần dễ đọc và dễ hiểu đối với người khác**, đặc biệt trong giai đoạn phát triển. Bạn luôn có thể tối ưu mã để tăng hiệu suất sau khi hoàn tất và chuyển sang môi trường production, nơi tốc độ quan trọng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Để tìm hiểu sâu hơn, hãy đọc chương [Điều khiển luồng (Flow Control)](https://launchschool.com/books/ruby/read/flow_control#conditionals) trong *Introduction to Programming With Ruby* của LaunchSchool.
1. Để có cái nhìn tổng quan về việc điều khiển luồng, hãy đọc bài viết [Ruby Explained: Câu lệnh điều kiện và điều khiển luồng](https://www.eriktrautman.com/posts/ruby-explained-conditionals-and-flow-control).

</div>
