### Giới thiệu

Ở phần đầu của mục này, bạn đã học cách tạo và thao tác với các số và chuỗi, cũng như cách gán chúng cho các biến. Trong quá trình phát triển thực tế, khi phải làm việc với hàng chục (thậm chí hàng trăm!) biến, việc xử lý từng số và chuỗi riêng lẻ sẽ rất tẻ nhạt, nếu không muốn nói là bất khả thi.

Một cách Ruby cho phép bạn biểu diễn một tập hợp dữ liệu là bằng **mảng**, mà bạn có thể hình dung như những danh sách có thứ tự. Thay vì làm việc với từng biến, số hoặc chuỗi riêng lẻ, mảng cho phép bạn tạo và thao tác với một tập hợp dữ liệu có thứ tự và được lập chỉ mục. Các biến, số hoặc chuỗi riêng lẻ bên trong một mảng được gọi là **phần tử**. Một mảng có thể chứa bất kỳ sự kết hợp nào của biến, số, chuỗi hoặc các đối tượng Ruby khác (bao gồm cả những mảng khác), dù bạn nên giữ các kiểu dữ liệu tương tự trong cùng một mảng.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Mô tả mảng là gì và giải thích vì sao mảng hữu ích.
- Mô tả hai cách khác nhau để tạo một mảng mới.
- Giải thích cách truy cập các phần tử mảng bằng những phương thức khác nhau.
- Giải thích ba cách khác nhau để thêm dữ liệu vào mảng.
- Giải thích cách xóa các phần tử khỏi mảng.

### Tạo mảng

Dưới đây là hai mảng cơ bản:

```ruby
num_array = [1, 2, 3, 4, 5]
str_array = ["This", "is", "a", "small", "array"]
```

Cả hai mảng đều có năm phần tử được ngăn cách bằng dấu phẩy. Mảng đầu tiên chứa các số nguyên, còn mảng thứ hai chứa các chuỗi.

Mảng thường được tạo bằng **cú pháp literal của mảng**, một cú pháp đặc biệt dùng để tạo các thể hiện của đối tượng mảng. Để tạo một mảng mới bằng cú pháp literal của mảng, hãy dùng dấu ngoặc vuông (`[]`).

Bạn cũng có thể tạo mảng bằng cách gọi phương thức `Array.new`. Khi gọi phương thức này, bạn có thể truyền thêm tối đa 2 đối số tùy chọn (kích thước ban đầu và giá trị mặc định):

```ruby
Array.new               #=> []
Array.new(3)            #=> [nil, nil, nil]
Array.new(3, 7)         #=> [7, 7, 7]
Array.new(3, true)      #=> [true, true, true]
```

### Truy cập các phần tử

Mỗi phần tử trong một mảng đều có một **chỉ mục**, là biểu diễn bằng số cho vị trí của phần tử trong mảng. Giống như hầu hết các ngôn ngữ lập trình khác, mảng Ruby sử dụng **cách đánh chỉ mục bắt đầu từ 0**, nghĩa là chỉ mục của phần tử đầu tiên là 0, của phần tử thứ hai là 1, và cứ thế tiếp tục. Để truy cập một phần tử cụ thể trong mảng, hãy gọi `my_array[x]`, trong đó `x` là chỉ mục của phần tử bạn muốn lấy. Gọi một vị trí không hợp lệ sẽ cho kết quả là `nil`. Ruby cũng cho phép dùng các chỉ mục âm; chúng trả về các phần tử tính từ *cuối* mảng, bắt đầu với [-1].

```ruby
str_array = ["This", "is", "a", "small", "array"]

str_array[0]            #=> "This"
str_array[1]            #=> "is"
str_array[4]            #=> "array"
str_array[-1]           #=> "array"
str_array[-2]           #=> "small"
```

Cuối cùng, Ruby cung cấp các phương thức mảng `#first` và `#last`, với chức năng khá dễ đoán. Ngoài ra, các phương thức này có thể nhận một đối số số nguyên, chẳng hạn `my_array.first(n)` hoặc `my_array.last(n)`, và sẽ trả về một mảng mới chứa lần lượt `n` phần tử đầu hoặc cuối của `my_array`.

```ruby
str_array = ["This", "is", "a", "small", "array"]

str_array.first         #=> "This"
str_array.first(2)      #=> ["This", "is"]
str_array.last(2)       #=> ["small", "array"]
```

### Thêm và xóa các phần tử

Để thêm một phần tử vào mảng hiện có, hãy dùng phương thức `#push` hoặc toán tử shovel `<<`. Cả hai cách đều thêm các phần tử vào cuối mảng và trả về mảng đó cùng với các phần tử mới. Phương thức `#pop` sẽ xóa phần tử ở cuối mảng và trả về phần tử đã bị xóa.

```ruby
num_array = [1, 2]

num_array.push(3, 4)      #=> [1, 2, 3, 4]
num_array << 5            #=> [1, 2, 3, 4, 5]
num_array.pop             #=> 5
num_array                 #=> [1, 2, 3, 4]
```

Các phương thức `#shift` và `#unshift` được dùng để thêm và xóa các phần tử ở đầu mảng. Phương thức `#unshift` thêm các phần tử vào đầu mảng và trả về mảng đó (tương tự như `#push`). Phương thức `#shift` xóa phần tử đầu tiên của mảng và trả về phần tử đó (tương tự như `#pop`).

```ruby
num_array = [2, 3, 4]

num_array.unshift(1)      #=> [1, 2, 3, 4]
num_array.shift           #=> 1
num_array                 #=> [2, 3, 4]
```

Bạn cũng nên biết rằng cả `#pop` và `#shift` đều có thể nhận các đối số số nguyên:

```ruby
num_array = [1, 2, 3, 4, 5, 6]

num_array.pop(3)          #=> [4, 5, 6]
num_array.shift(2)        #=> [1, 2]
num_array                 #=> [3]
```

### Cộng và trừ các mảng

Bạn nghĩ kết quả của `[1, 2, 3] + [3, 4, 5]` sẽ là gì?

Nếu bạn đoán là `[1, 2, 3, 3, 4, 5]` thì xin chúc mừng! Cộng hai mảng sẽ trả về một mảng mới được tạo bằng cách nối chúng lại, tương tự như phép nối chuỗi. Phương thức `concat` cũng hoạt động theo cách tương tự.

```ruby
a = [1, 2, 3]
b = [3, 4, 5]

a + b         #=> [1, 2, 3, 3, 4, 5]
a.concat(b)   #=> [1, 2, 3, 3, 4, 5]
```

Để tìm phần khác nhau giữa hai mảng, bạn có thể dùng phép trừ `-`. Phương thức này trả về một bản sao của mảng thứ nhất sau khi loại bỏ mọi phần tử xuất hiện trong mảng thứ hai.

```ruby
[1, 1, 1, 2, 2, 3, 4] - [1, 4]  #=> [2, 2, 3]
```

### Các phương thức cơ bản

Ruby cung cấp cho bạn rất nhiều phương thức để thao tác với mảng và nội dung của chúng (hơn 150 phương thức!), trong đó có nhiều phương thức nằm ngoài phạm vi của bài học này. Để tìm hiểu các phương thức khác, hãy vào tài liệu chính thức và xem trang [tài liệu lớp Array](https://docs.ruby-lang.org/en/3.4/Array.html), nơi các phương thức được liệt kê theo thứ tự bảng chữ cái (bằng cách cuộn thanh bên trái) hoặc được tóm tắt và nhóm theo mục đích (bằng cách đọc phần "What's Here").

Việc gọi phương thức `#methods` trên một mảng cũng sẽ cho ra một danh sách dài các phương thức hiện có.

```ruby
num_array.methods       #=> A very long list of methods
```

Dưới đây là sơ lược về một số phương thức mảng phổ biến khác mà bạn có thể gặp:

```ruby
[].empty?               #=> true
[[]].empty?             #=> false
[1, 2].empty?           #=> false

[1, 2, 3].length        #=> 3

[1, 2, 3].reverse       #=> [3, 2, 1]

[1, 2, 3].include?(3)   #=> true
[1, 2, 3].include?("3") #=> false

[1, 2, 3].join          #=> "123"
[1, 2, 3].join("-")     #=> "1-2-3"
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc chương về [Mảng](https://launchschool.com/books/ruby/read/arrays) của Launch School, đồng thời làm theo các bài tập bằng irb hoặc REPL phù hợp khác.
1. Đọc [Hướng dẫn đầy đủ về mảng Ruby](https://www.rubyguides.com/2015/05/ruby-arrays/) của Jesus Castello.
1. Hoàn thành các bài tập về mảng trong [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises) mà bạn đã sao chép trước đó.
1. Hãy cùng luyện tập đọc tài liệu. Bạn sẽ cảm thấy mình giống một lập trình viên thực thụ! Bạn nghĩ các phương thức `#clear`, `#insert`, `#sample`, `#shuffle` và `#uniq` làm gì? Hãy tra cứu các phương thức này trong [tài liệu lớp Array](https://docs.ruby-lang.org/en/3.4/Array.html). Bạn đoán đúng đến đâu?

</div>
