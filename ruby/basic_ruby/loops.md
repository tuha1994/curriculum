### Giới thiệu

Đừng nhầm với Froot Loops, loại ngũ cốc gây nghiện có thể khiến trẻ em xuất hiện những triệu chứng tương tự ADHD, các vòng lặp trong Ruby là những khối mã được lặp đi lặp lại liên tục cho đến khi một điều kiện nhất định được thỏa mãn.

Có lẽ bạn cũng từng trải qua những vòng lặp ngoài đời giống tôi: bị phạt ở lại trường và phải chép đi chép lại cùng một câu về việc không vẽ những hình thù tục tĩu nhỏ trên bàn. Viết đi viết lại một thứ không chỉ nhàm chán mà còn dễ phát sinh lỗi. Bạn có thể đã viết sai chính tả ở một dòng và quên chấm lên chữ "i" ở dòng khác. Lập trình cũng vậy: bạn càng phải viết ít mã thì khả năng đưa vào những lỗi khiến chương trình gặp sự cố càng thấp.

Nếu thấy mình cần lặp lại một hành động nhiều hơn một lần trong mã, có lẽ bạn cần đến các vòng lặp trong cuộc sống lập trình của mình.

Với tất cả các ví dụ trong bài học này, bạn nên tự gõ và chạy theo trong irb hoặc bất kỳ môi trường REPL trực tuyến phù hợp nào để hiểu rõ hơn cách chúng hoạt động.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích vòng lặp là gì và vì sao nó hữu ích.
- Giải thích vòng lặp `loop` là gì và cách sử dụng nó.
- Giải thích vòng lặp `while` là gì và cách sử dụng nó.
- Giải thích vòng lặp `for` là gì và cách sử dụng nó.
- Giải thích vòng lặp `#times` là gì và cách sử dụng nó.
- Giải thích vòng lặp `until` là gì và cách sử dụng nó.
- Giải thích vòng lặp `#upto` và `#downto` là gì và cách sử dụng chúng.

### Vòng lặp Loop

Vòng lặp `loop` (gọi là gì nhỉ????) là vòng lặp của Ruby cứ chạy mãi không chịu dừng. Đây là một vòng lặp vô hạn, sẽ tiếp tục chạy trừ khi bạn chủ động yêu cầu nó dừng bằng lệnh `break`. Thông thường, `break` được dùng cùng một điều kiện, như trong ví dụ dưới đây.

```ruby
i = 0
loop do
  puts "i is #{i}"
  i += 1
  break if i == 10
end
```

Bạn sẽ không thường xuyên thấy vòng lặp này được dùng trong Ruby. Nếu thấy mình đang sử dụng `loop`, hãy biết rằng có lẽ tồn tại một vòng lặp phù hợp hơn, chẳng hạn như một trong những vòng lặp chuyên biệt bên dưới.

### Vòng lặp While

Vòng lặp `while` tương tự vòng lặp `loop`, ngoại trừ việc bạn khai báo trước điều kiện sẽ khiến vòng lặp dừng lại.

```ruby
i = 0
while i < 10 do
 puts "i is #{i}"
 i += 1
end
```

Đây là ví dụ sử dụng vòng lặp `while` với một bộ đếm. Vì bạn khai báo trước điều kiện kết thúc vòng lặp nên mục đích của mã trở nên rõ ràng hơn nhiều, khiến mã này dễ đọc hơn vòng lặp `loop` ở trên.

Bạn cũng có thể dùng vòng lặp `while` để liên tục đặt câu hỏi cho người dùng cho đến khi họ đưa ra câu trả lời mong muốn:

```ruby
while gets.chomp != "yes" do
  puts "Are we there yet?"
end
```

Ví dụ này cho thấy ưu điểm về tính linh hoạt của vòng lặp `while`: nó sẽ chạy cho đến khi điều kiện dừng được thỏa mãn, có thể là một số lần lặp thay đổi hoặc một số lần lặp ban đầu chưa biết. Ai mà biết được bạn sẽ đến nơi sau lần hỏi thứ nhất, thứ tư hay thứ bảy mươi chín?

### Vòng lặp Until

Vòng lặp `until` là đối lập của vòng lặp `while`. Vòng lặp `while` tiếp tục chạy khi điều kiện còn đúng, còn vòng lặp `until` tiếp tục chạy khi điều kiện còn sai. Vì vậy, về cơ bản bạn có thể dùng thay thế lẫn nhau hai vòng lặp này. Sau cùng, điều kiện dừng của bạn sẽ quyết định cách nào dễ đọc hơn.

Trong khả năng có thể, bạn nên tránh phủ định các biểu thức logic bằng `!` (không). Thứ nhất, dấu chấm than trong mã có thể thực sự khó nhận ra. Thứ hai, việc phủ định khiến logic khó suy luận hơn, từ đó làm mã khó hiểu hơn. Đây là những tình huống mà `until` phát huy tác dụng.

Ta có thể viết lại các ví dụ về vòng lặp `while` bằng `until`.

```ruby
i = 0
until i >= 10 do
 puts "i is #{i}"
 i += 1
end
```

Bạn có thể thấy ở đây rằng việc dùng `until` khiến vòng lặp tiếp tục chạy cho đến khi điều kiện i >= 10 trở thành đúng.

Ví dụ tiếp theo cho thấy cách dùng `until` để tránh phép phủ định `!` mà vòng lặp `while` ở trên phải sử dụng.

```ruby
until gets.chomp == "yes" do
  puts "Do you like Pizza?"
end
```

Dễ đọc hơn nhiều! Mà ai lại không thích Pizza chứ.

### Phạm vi

Nếu biết chính xác muốn vòng lặp chạy bao nhiêu lần thì sao? Ruby cho phép ta dùng một thứ gọi là [phạm vi](https://docs.ruby-lang.org/en/3.4/Range.html) để định nghĩa một khoảng. Tất cả những gì cần làm là cung cấp cho Ruby giá trị bắt đầu, giá trị kết thúc và cho biết muốn phạm vi bao gồm hay không bao gồm giá trị kết thúc.

```ruby
(1..5)      # inclusive range: 1, 2, 3, 4, 5
(1...5)     # exclusive range: 1, 2, 3, 4

# We can make ranges of letters, too!
('a'..'d')  # a, b, c, d
```

### Vòng lặp for

Vòng lặp `for` được dùng để lặp qua một tập hợp thông tin, chẳng hạn như một mảng hoặc phạm vi. Những vòng lặp này hữu ích khi bạn cần thực hiện một việc trong một số lần nhất định đồng thời sử dụng một bộ lặp.

```ruby
for i in 0..5
  puts "#{i} zombies incoming!"
end
```

Thực sự chỉ có vậy thôi.

### Vòng lặp Times

Nếu cần chạy một vòng lặp trong một số lần xác định, hãy tìm đến vòng lặp `#times` đáng tin cậy. Nó lặp qua một vòng lặp số lần đã chỉ định, thậm chí còn cho phép bạn truy cập vào số thứ tự mà nó đang lặp qua.

```ruby
5.times do
  puts "Hello, world!"
end
```

Chắc bạn cũng đoán được đoạn mã đó làm gì. Ruby vốn dễ đọc như vậy!

```ruby
5.times do |number|
  puts "Alternative fact number #{number}"
end
```

Hãy nhớ rằng các vòng lặp sẽ bắt đầu đếm từ chỉ mục không nếu không được chỉ định khác đi, vì vậy lần lặp đầu tiên sẽ xuất ra `Alternative fact number 0`.

### Vòng lặp Upto và Downto

Các phương thức Ruby `#upto` và `#downto` thực hiện đúng như bạn có thể đoán từ tên của chúng. Bạn có thể dùng các phương thức này để lặp từ một số bắt đầu tăng lên hoặc giảm xuống một số khác, tương ứng.

```ruby
5.upto(10) { |num| print "#{num} " }     #=> 5 6 7 8 9 10

10.downto(5) { |num| print "#{num} " }   #=> 10 9 8 7 6 5
```

Nếu cần lần lượt đi qua một dãy số (hoặc thậm chí các chữ cái) trong một phạm vi cụ thể, thì đây là những vòng lặp dành cho bạn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương Vòng lặp & Bộ lặp](https://launchschool.com/books/ruby/read/loops_iterators) trong *Introduction to Programming With Ruby* của LaunchSchool. Chúng tôi khuyên bạn bỏ qua phần về đệ quy vì đệ quy sẽ được học ở phần sau.
1. Thử viết lại các ví dụ trên bằng những phương thức vòng lặp khác để đạt được cùng kết quả.

</div>
