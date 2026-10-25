### Giới thiệu

Việc truy tìm và sửa cả lỗi lẫn những hành vi bất ngờ trong mã của bạn là một phần không thể tránh khỏi khi làm nhà phát triển. Nghệ thuật tìm ra nguyên nhân của vấn đề rồi sửa nó trong mã được gọi là **gỡ lỗi**. [Nguồn gốc của thuật ngữ "gỡ lỗi"](https://en.wikipedia.org/wiki/Debugging#Etymology) là một câu chuyện kinh điển của khoa học máy tính, rất đáng đọc nếu bạn chưa đọc.

Trong bài học này, chúng ta sẽ tìm hiểu tất cả những kỹ thuật chính có thể dùng để gỡ lỗi mã khi gặp vấn đề.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học.

- Stack trace là gì.
- Dùng stack trace để gỡ lỗi mã.
- Dùng `puts`, `p`, Pry và tích hợp VSCode của gem debug để gỡ lỗi mã.
- Cách quyết định nên bắt đầu gỡ lỗi từ đâu.

### Đọc stack trace

Khi chương trình Ruby bị dừng sau khi gặp lỗi thời gian chạy hoặc ngoại lệ, nó sẽ tạo ra một khối văn bản dài gọi là **stack trace**, sau đó in khối văn bản này ra terminal. Stack trace trông gần như sau:

![Ví dụ về stack trace](https://cdn.statically.io/gh/TheOdinProject/curriculum/a2cfa47e944fa8127ccf5faa6e1c7c328de42428/ruby/basic_ruby/debugging/imgs/00.png)

Đừng để stack trace làm bạn e ngại. Bạn không cần đọc hết, thậm chí cũng không cần biết phần lớn nội dung của nó có nghĩa gì.

Stack trace in ra từng dòng mã trong chương trình đã được thực thi trước khi chương trình bị dừng. Dòng đầu tiên của stack trace thường cung cấp thông tin hữu ích nhất về lỗi mà chương trình gặp phải:

![Dòng đầu tiên của stack trace](https://cdn.statically.io/gh/TheOdinProject/curriculum/a2cfa47e944fa8127ccf5faa6e1c7c328de42428/ruby/basic_ruby/debugging/imgs/01.png)

Trước hết, dòng này của stack trace cho biết chính xác dòng nào gây ra lỗi thời gian chạy. Trong ví dụ trên, lỗi xảy ra ở dòng 31 của tệp `bottles.rb`. Dòng này cũng cung cấp phần giải thích ngắn gọn về lỗi và tên của lỗi. (Trong trường hợp này là [`NameError`](https://docs.ruby-lang.org/en/3.4/NameError.html)). Và đúng vậy, trong Ruby, [lỗi (Exception) cũng là các đối tượng](https://docs.ruby-lang.org/en/3.4/Exception.html).

Vậy là xong. Đến đây, bạn biết ngoại lệ được phát sinh ở đâu trong mã, đồng thời biết loại lỗi mình đang xử lý. Thậm chí bạn có thể đã biết cần triển khai những cách sửa nào trong mã.

Nhưng nếu bạn không biết cách sửa mã thì sao? Khi đó đã đến lúc đi sâu vào việc gỡ lỗi!

### Gỡ lỗi bằng puts

Quá trình gỡ lỗi xoay quanh việc xác nhận các giả định về mã cho đến khi bạn tìm thấy điều gì đó trái với giả định của mình. Chẳng hạn, một biến hoặc phương thức có trả về giá trị bạn mong đợi không? Một phép tính hoặc vòng lặp qua mảng hay hash có cho ra kết quả bạn mong đợi không?

Cách dễ nhất và nhanh nhất để xác nhận các giả định khi gỡ lỗi là dùng các câu lệnh `puts` để in giá trị trả về của biến, phương thức, phép tính, vòng lặp, hoặc thậm chí cả dòng mã ra terminal.

Giả sử vì *một lý do nào đó*, chúng ta cần viết một phương thức nhận vào một chuỗi rồi kiểm tra xem chuỗi đó có phải là **isogram** (một từ không có chữ cái lặp lại) hay không. Có lẽ đây là một tình huống bắt cóc cần đến một chuyên gia gỡ lỗi? Hãy xem bản nháp đầu tiên:

```ruby
def isogram?(string)
  original_length = string.length
  string_array = string.downcase.split
  unique_length = string_array.uniq.length
  original_length == unique_length
end

isogram?("Odin")

#=> false
```

Được rồi, cách đó không hoạt động. Chúng ta không ngờ kết quả lại như vậy. Tại sao? Vì chuỗi *Odin* là một isogram nhưng chúng ta lại nhận được `false`. Phương thức không ném ra ngoại lệ, nên thậm chí chúng ta cũng không có dòng nào để bắt đầu gỡ lỗi. Vậy giờ phải làm gì?

Chúng ta biết `original_length == unique_length` trả về `false` vì đó là câu lệnh cuối cùng, vậy tại sao không đặt một `puts` ở dòng ngay trước nó để xem `unique_length` là gì. Thay cho `puts`, `p` cũng thường được dùng để gỡ lỗi; `p` là sự kết hợp giữa `puts` và `inspect` (sẽ tìm hiểu thêm bên dưới). Để thể hiện rõ hơn sự khác nhau giữa việc Ruby in ra terminal và giá trị Ruby trả về, các ví dụ trong phần này dùng cú pháp IRB đầy đủ, chính xác như những gì bạn sẽ thấy nếu nhập các lệnh này vào phiên IRB trong terminal của mình.

```ruby
irb(main):001:1* def isogram?(string)
irb(main):002:1*   original_length = string.length
irb(main):003:1*   string_array = string.downcase.split
irb(main):004:1*   unique_length = string_array.uniq.length
irb(main):005:1*
irb(main):006:1*   p unique_length
irb(main):007:1*
irb(main):008:1*   original_length == unique_length
irb(main):009:0> end
=> :isogram?
irb(main):010:0> isogram?("Odin")
1
=> false
```

*THÚ VỊ*. Dùng `p` với `unique_length` sẽ in giá trị đó ra console và cho thấy có điều gì đó không ổn trong cách chúng ta gọi `#uniq` trên `string_array`, vì chúng ta biết đầu vào có `4` ký tự khác nhau nhưng kết quả lại là `1`. Để kiểm chứng, hãy đặt thêm một câu lệnh `p` trước câu lệnh `unique_length`:

```ruby
irb(main):001:1* def isogram?(string)
irb(main):002:1*   original_length = string.length
irb(main):003:1*   string_array = string.downcase.split
irb(main):004:1*
irb(main):005:1*   p string_array
irb(main):006:1*
irb(main):007:1*   unique_length = string_array.uniq.length
irb(main):008:1*
irb(main):009:1*   p unique_length
irb(main):010:1*
irb(main):011:1*   original_length == unique_length
irb(main):012:0> end
=> :isogram?
irb(main):013:0> isogram?("Odin")
["odin"]
1
=> false
```

Quả nhiên, chúng ta đã không dùng `#split` đúng cách, vì nó tạo ra một mảng chứa chuỗi đã cho thay vì một mảng gồm các ký tự của chuỗi. Tại sao? Theo mặc định, nếu không cung cấp đối số, [phương thức #split](https://docs.ruby-lang.org/en/3.4/String.html#method-i-split) sẽ chia chuỗi bằng `whitespace` làm dấu phân cách. Hãy thử chạy đoạn mã trên trong IRB với `#split('')` hoặc một REPL trực tuyến phù hợp, bạn sẽ thấy sự khác biệt.

Tình huống bắt cóc đã được giải quyết! Cũng không quá khó, đúng không?

#### Gỡ lỗi bằng puts và nil

Dùng `puts` là một cách tuyệt vời để gỡ lỗi, nhưng có một điểm cần **ĐẶC BIỆT LƯU Ý**: gọi `puts` trên bất kỳ thứ gì là `nil`, chuỗi rỗng hoặc collection rỗng sẽ chỉ in ra một dòng trống trong terminal.

Trong trường hợp này, dùng `p` sẽ cho bạn nhiều thông tin hơn. Như đã đề cập ở trên, `p` là sự kết hợp giữa puts và [phương thức #inspect](https://docs.ruby-lang.org/en/3.4/Object.html#method-i-inspect); về cơ bản, phương thức sau sẽ in ra biểu diễn dạng chuỗi của đối tượng mà nó được gọi trên đó. Để minh họa, hãy thử đoạn mã sau trong một REPL:

```ruby
puts "Using puts:"
puts []
p "Using p:"
p []
```

### Gỡ lỗi bằng Pry-byebug

[Pry](https://github.com/pry/pry) là một gem Ruby cung cấp cho bạn một REPL tương tác trong khi chương trình đang chạy. REPL do Pry cung cấp rất giống IRB nhưng có thêm chức năng. Gem Ruby được khuyến nghị để gỡ lỗi là [Pry-byebug](https://github.com/deivid-rodriguez/pry-byebug), và gem này bao gồm Pry như một dependency. Pry-byebug bổ sung khả năng gỡ lỗi từng bước và điều hướng stack.

Để dùng Pry-byebug, trước tiên bạn cần cài đặt nó trong terminal bằng cách chạy `gem install pry-byebug`. Sau đó, bạn có thể cung cấp gem này cho chương trình bằng cách thêm `require 'pry-byebug'` ở đầu tệp. Cuối cùng, để dùng Pry-byebug, bạn chỉ cần gọi `binding.pry` tại bất kỳ vị trí nào trong chương trình.

Để làm theo các ví dụ này, hãy lưu mã vào một tệp Ruby (ví dụ: `script.rb`), rồi chạy tệp trong terminal (ví dụ: `ruby script.rb`)

```ruby
require 'pry-byebug'

def isogram?(string)
  original_length = string.length
  string_array = string.downcase.split

  binding.pry

  unique_length = string_array.uniq.length
  original_length == unique_length
end

isogram?("Odin")
```

Khi mã chạy đến `binding.pry`, nó sẽ mở một phiên giống IRB trong terminal. Sau đó, bạn có thể dùng phiên này để kiểm tra giá trị của bất kỳ thứ gì nằm trong phạm vi nơi bạn chèn `binding.pry`. Tuy nhiên, hãy nhớ rằng mọi mã được viết *sau* câu lệnh `binding.pry` đều chưa được đánh giá trong phiên Pry.

Ví dụ, ở đây `original_length` và `string_array` nằm trong phạm vi. Tuy nhiên, `unique_length` thì không, vì nó được viết sau `binding.pry` và chưa được đánh giá.

Vì vậy, việc thêm một dòng `binding.pry` vào mã tương tự như tạo một breakpoint trong JavaScript.

Để thấy điều này hoạt động, hãy thử chạy đoạn mã sau:

```ruby
require 'pry-byebug'

def yell_greeting(string)
  name = string

  binding.pry

  name = name.upcase
  greeting = "WASSAP, #{name}!"
  puts greeting
end

yell_greeting("bob")
```

Trong phiên này, nếu kiểm tra giá trị của `name`, bạn sẽ thấy kết quả là `bob` thay vì `BOB`. Bạn nghĩ `greeting` sẽ trả về giá trị nào? Đúng vậy, nó sẽ là `nil`. Đó là vì `name = name.upcase` và `greeting = "WASSAP, #{name}!"` xuất hiện sau lời gọi `binding.pry` và chưa bao giờ được đánh giá.

Vẫn dùng ví dụ trên, bạn có thể dùng một trong các lệnh của pry-byebug để tìm hiểu `name = name.upcase` sẽ trả về gì. Bạn không cần thoát phiên hay thêm một `binding.pry` khác bên dưới nó. Hãy nhập `next` để tiến đến dòng tiếp theo.

```ruby
[1] pry(main)> name
=> "bob"
[2] pry(main)> greeting
=> nil
[3] pry(main)> next

     5: def yell_greeting(string)
     6:   name = string
     7:
     8:   binding.pry
     9:
    10:   name = name.upcase
 => 11:   greeting = "WASSAP, #{name}!"
    12:   puts greeting
    13: end

[4] pry(main)> name
=> "BOB"

```

Nó dừng lại sau khi đánh giá dòng tiếp theo. Giờ `name` trả về `BOB`. Gọi `next` lần nữa sẽ đánh giá dòng kế tiếp. Hãy thử để biết `greeting` sẽ trả về gì. [Pry-byebug còn có một số lệnh khác](https://github.com/deivid-rodriguez/pry-byebug). Hãy thử dùng chúng để hiểu rõ tác dụng của từng lệnh.

Như bạn thấy, dùng Pry-byebug để gỡ lỗi cũng đạt được kết quả giống như gỡ lỗi bằng `puts`: nó cho phép bạn xác nhận các giả định về những phần cụ thể trong mã. Nếu mã phức tạp, Pry-byebug có thể giúp bạn gỡ lỗi nhanh hơn nhờ môi trường runtime tương tác. Trong những tình huống như vậy, Pry-byebug dễ thao tác hơn so với việc thêm các câu lệnh `puts` ở khắp nơi rồi chạy lại mã mỗi lần.

Bạn còn có thể làm nhiều việc hơn nữa với Pry-byebug, nhưng những nội dung đó nằm ngoài phạm vi bài học này. Hãy xem phần Bài tập và Tài nguyên bổ sung để biết nơi tìm hiểu thêm về gem hữu ích này.

### Bắt đầu gỡ lỗi như thế nào

Chương trình thường gặp vấn đề vì hai lý do chính:

1. Chương trình gặp lỗi và không thể chạy. Ví dụ, một lỗi đánh máy có thể khiến một lỗi cú pháp được phát sinh, làm chương trình bị dừng. Trong trường hợp này, Ruby cung cấp một stack trace.

1. Chương trình chạy nhưng không hoạt động theo cách bạn mong đợi. Ví dụ, bạn mong một phương thức trả về `2`, nhưng khi chạy lại nhận được `6`. Trong trường hợp này, không có stack trace.

Rõ ràng, nếu có stack trace thì đó là nơi đầu tiên bạn nên xem khi gỡ lỗi. Nếu không có stack trace, `puts` và Pry là những công cụ dễ dùng và nhanh nhất để giúp bạn đưa chương trình hoạt động trở lại.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy học qua bài hướng dẫn [Gỡ lỗi Ruby](https://www.rubyguides.com/2015/07/ruby-debugging/) của Ruby Guides; bài này bao quát các chủ đề chúng ta đã học nhưng đi sâu hơn.
1. Đọc phần [Ngoại lệ và Stack Trace](https://launchschool.com/books/ruby/read/more_stuff#readingstacktraces) trong cuốn sách trực tuyến *Nhập môn lập trình với Ruby* của Launch School.
1. Kiểm tra [Phần mở rộng VSCode](https://code.visualstudio.com/docs/editor/extension-marketplace) và đảm bảo đã cài đặt các phần mở rộng [Ruby LSP](https://marketplace.visualstudio.com/items?itemName=Shopify.ruby-lsp) và [Trình gỡ lỗi Ruby rdbg cho VSCode](https://marketplace.visualstudio.com/items?itemName=KoichiSasada.vscode-rdbg).
1. Giờ bạn đã quen với những kiến thức cơ bản, chúng ta sẽ vui chơi một chút với VSCode! Hãy xem [tài liệu Trình gỡ lỗi Ruby rdbg cho VSCode](https://github.com/ruby/vscode-rdbg) và tạo cấu hình trong VSCode bằng cách vào `Run and Debug`, nhấp vào `create a launch.json file`, rồi chọn `Ruby (rdbg)`. Cấu hình bạn muốn dùng là `Debug current file with rdbg`, nằm ở đầu thanh bên Debug. Bạn cũng có thể gọi cấu hình gỡ lỗi được dùng gần đây nhất bằng `F5`. Các tệp `launch.json` cần được tạo riêng cho từng dự án. Nếu gặp khó khăn trong việc tìm đến tệp `launch.json` để thay đổi cấu hình, hãy xem phần [Gỡ lỗi bằng cấu hình khởi chạy VScode](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations). Tuy vậy, chúng tôi khuyến khích bạn đọc toàn bộ bài viết!
1. Giờ mọi thứ đã được cài đặt và cấu hình, hãy tạo một tệp mới có thể đặt tên là `script.rb`. Tiếp theo, sao chép và dán ví dụ đầu tiên trong phần [Gỡ lỗi bằng pry-byebug](#debugging-with-pry-byebug). Xóa dòng `require pry-byebug` và đổi `binding.pry` thành `debugger`. Lưu tệp.
1. Nhấp vào nút `Run and Debug`, mở thư mục chứa tập lệnh, đặt một breakpoint của VSCode ở đâu đó bên trong hàm rồi chạy trình gỡ lỗi! Tất cả hẳn trông rất quen thuộc với bạn; khi chạm đến breakpoint của VSCode, giao diện sẽ tương tự các breakpoint bạn đã dùng trong [bài học Công cụ dành cho nhà phát triển JavaScript](https://www.theodinproject.com/lessons/foundations-javascript-developer-tools). Nhưng *chờ đã*, khi chạm đến breakpoint `debugger`, chúng ta lại có một REPL tương tác để khám phá! Đúng là kết hợp được những điều tốt nhất của cả hai! Hãy thử dùng nó, và nếu bị mắc kẹt thì có thể tham khảo [tài liệu Gỡ lỗi bằng VScode](https://code.visualstudio.com/docs/editor/debugging).
1. Mặc dù trình gỡ lỗi của VSCode là một công cụ hữu ích giúp việc gỡ lỗi đơn giản hơn, nhiều công ty sẽ không dùng nó - và sẽ muốn bạn quen với việc gỡ lỗi bằng các khái niệm mà bài học này tập trung vào: stack trace, `puts`,`debug`, `pry-byebug`. Hãy luyện tập bằng cách hoàn thành các bài tập gỡ lỗi trong [kho ruby-exercises](https://github.com/TheOdinProject/ruby-exercises/tree/main/ruby_basics#readme) mà bạn đã clone trước đây.

</div>