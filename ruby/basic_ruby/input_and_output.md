### Giới thiệu

Để tạo ra những chương trình thân thiện với người dùng và có tính tương tác, bạn cần biết cách **xuất** dữ liệu ra màn hình cũng như cách nhận **đầu vào** từ người dùng. Trong bài học này, chúng ta sẽ tìm hiểu những cách phổ biến nhất để thực hiện các công việc đó trong Ruby. Cũng như với các bài học khác, đặc biệt là bài này, việc làm theo các ví dụ trong irb hoặc một REPL trực tuyến phù hợp sẽ rất hữu ích.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Phân biệt lệnh `print` và `puts`.
- Mô tả phương thức dùng để nhận đầu vào từ người dùng.

### Các lệnh xuất dữ liệu

Để xuất thông tin, chẳng hạn ra môi trường irb hoặc REPL hay dòng lệnh, chúng ta có thể dùng lệnh `print`. Để thể hiện rõ hơn sự khác biệt giữa nội dung Ruby in ra thiết bị đầu cuối và giá trị trả về, các ví dụ trong bài này sử dụng cú pháp irb đầy đủ, đúng như những gì bạn sẽ thấy nếu gõ các lệnh này trong phiên irb trên thiết bị đầu cuối của mình.

```ruby
irb(main):001:0> print "Learning to code is FUN!"
Learning to code is FUN!=> nil

irb(main):002:0> print "1234"
1234=> nil
```

Chúng ta cũng có thể dùng lệnh `puts`:

```ruby
irb(main):001:0> puts "Learning to code is cool!!"
Learning to code is cool!!
=> nil

irb(main):002:0> puts "Hey, I want 2 key lime pies."
Hey, I want 2 key lime pies.
=> nil

irb(main):003:0> x = "My name is ALEX! :)"
=> "My name is ALEX! :)"

irb(main):004:0> puts x
My name is ALEX! :)
=> nil
```

Như bạn thấy, các lệnh `puts` và `print` khá giống nhau, nhưng có lẽ bạn đã nhận ra điểm khác biệt nhỏ giữa chúng: `puts` nối thêm một dòng mới vào đối số được truyền vào, còn `print` giữ mọi thứ trên cùng một dòng. Một điều rất quan trọng cần lưu ý với cả hai lệnh là sau khi in ra bất kỳ đối số nào được truyền vào, cả `puts` và `print` đều trả về `nil`.

Để làm nổi bật sự khác nhau giữa `puts` và `print`, chúng ta sẽ dùng một cú pháp Ruby hiếm khi được sử dụng: [dấu chấm phẩy](https://stackoverflow.com/questions/3953846/can-you-use-semicolons-in-ruby). Tóm lại, dấu chấm phẩy không bao giờ bắt buộc trong Ruby, nhưng cho phép bạn chạy nhiều lệnh Ruby trên cùng một dòng. Kiểu [code golf](https://en.wikipedia.org/wiki/Code_golf) này nhìn chung không được khuyến khích, nhưng hữu ích cho ví dụ này. Trong một REPL, hai câu lệnh có thể được viết trên các dòng riêng mà không cần dấu chấm phẩy và vẫn cho ra kết quả mong muốn. Tuy nhiên, irb chỉ có thể đọc một câu lệnh mỗi lần, nên dấu chấm phẩy là cần thiết để đoạn mã này hoạt động như dự định.

```ruby
irb(main):001:0> print "Hello World"; print "I love drinking coffee"
Hello WorldI love drinking coffee=> nil

irb(main):002:0> puts "Hello World"; puts "I love drinking coffee"
Hello World
I love drinking coffee
=> nil
```

### Các lệnh nhận đầu vào

Để nhận đầu vào từ người dùng, chúng ta có thể dùng lệnh `gets`. Khi dùng `gets`, chương trình sẽ tạm dừng và chờ người dùng nhập dữ liệu. Sau khi người dùng nhấn <kbd>Enter</kbd>, chương trình sẽ tiếp tục thực thi.

Hãy thử nhé. Mở irb, gõ `gets`, rồi gõ `The Odin Project`. Bạn sẽ thấy kết quả tương tự như sau:

```ruby
irb(main):001:0> gets
The Odin Project
=> "The Odin Project\n"
```

Thú vị chứ? Hãy cùng phân tích quá trình này một chút.

Bạn sẽ nhận thấy rằng, không giống `puts` và `print`, `gets` thực sự **trả về** dữ liệu người dùng nhập thay vì trả về `nil`. Điều này có nghĩa là đầu vào có thể được gán cho một biến để bạn tiếp tục sử dụng, thao tác, biến đổi và in ra theo cách mình muốn.

Có thể bạn cũng nhớ từ phần String trong bài học Kiểu dữ liệu cơ bản rằng `"\n"` là một ký tự escape biểu diễn một dòng mới. Lệnh `gets` luôn trả về một dòng mới ở cuối phần đầu vào. Lệnh này thường sử dụng một "dấu phân cách" để đọc các luồng dữ liệu và tệp nhiều dòng, nhưng nội dung đó nằm ngoài phạm vi của bài học nhập môn này. Hiện tại, bạn chỉ cần biết rằng `#chomp` là một phương thức thường được dùng để loại bỏ dấu phân cách.

```ruby
irb(main):001:0> new_string = gets.chomp
This is a sentence.
=> "This is a sentence."

irb(main):002:0> puts new_string
This is a sentence.
=> nil
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc và hoàn thành bài tập trong [bài học "Hello, World" của learnrubyonline.org](https://www.learnrubyonline.org/en/Hello_World).
1. Đọc bốn phần đầu tiên của [Ruby - File I/0](https://www.tutorialspoint.com/ruby/ruby_input_output.htm) trên TutorialsPoint về `puts`, `gets`, `putc` và `print`.
1. Đọc [bài viết trên Ruby guides mô tả sự khác nhau giữa `puts`, `print` và `p`](https://www.rubyguides.com/2018/10/puts-vs-print/).

</div>
