### Giới thiệu

Biến là cách gán dữ liệu cho các tên trong chương trình của bạn. Bạn có thể hình dung một biến như một chiếc hộp có dán nhãn: nó lưu trữ một thứ gì đó và có một cái tên để bạn biết bên trong chứa gì. Đây là một phép ẩn dụ chưa hoàn hảo, như bạn sẽ thấy ở cuối bài học này, nhưng hiện tại nó sẽ giúp bạn hiểu về biến.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài học này.

- Mô tả biến là gì và cách gán cho biến một giá trị hoặc biểu thức.
- Giải thích các toán tử gán `+=`, `-=`, `*=`, và `/=` hoạt động như thế nào.
- Mô tả các quy ước đặt tên biến.

### Khai báo biến

Đây là cách tạo một biến trong Ruby:

```ruby
age = 18 #=> 18
```

Bạn cũng có thể gán kết quả của một biểu thức cho một biến.

```ruby
age = 18 + 5 #=> 23
```

Tên biến có thể được sử dụng lại, vì vậy bạn có thể gán một giá trị mới cho biến ở bất kỳ thời điểm nào trong chương trình. Đương nhiên, việc đó sẽ ghi đè giá trị ban đầu.

```ruby
age = 18
age #=> 18
age = 33
age #=> 33
```

Sẽ có nhiều tình huống bạn muốn thực hiện một phép toán trên giá trị ban đầu của biến, rồi gán lại kết quả của phép toán đó cho chính biến ấy.

```ruby
age = 18
age #=> 18
age = age + 4
age #=> 22
```

Vì đây là một tình huống phổ biến, Ruby cung cấp một toán tử gán viết tắt tiện lợi để thực hiện việc này: `+=`.

```ruby
age = 18
age += 4 #=> 22
```

Có các toán tử gán tương tự cho tất cả những toán tử toán học thông dụng:

```ruby
age = 18
age -= 2  #=> 16

cash = 10
cash *= 2 #=> 20

temperature = 40
temperature /= 10 #=> 4
```

#### Cách đặt tên biến

Ruby là một ngôn ngữ hướng đến việc đọc tự nhiên và viết dễ dàng. Hãy ghi nhớ điều này khi đặt tên biến. Tên biến nên mô tả rõ nhất có thể giá trị của biến đại diện cho điều gì.

Đặt tên biến rõ ràng sẽ mang lại lợi ích khi bạn xem lại mã nguồn vài tháng sau khi viết, lúc bạn không còn nhớ biến đó được thiết kế để lưu trữ gì. Từ giờ trở đi, khi đặt tên biến, hãy ghi nhớ câu nói sau của John Woods:

> Hãy luôn viết mã như thể người cuối cùng bảo trì mã của bạn là một kẻ tâm thần bạo lực biết nơi bạn sống.

Điều cơ bản nhất bạn có thể làm để viết mã sạch và dễ bảo trì là đặt tên biến đúng cách. Vì vậy, hãy hình thành thói quen này từ sớm để tránh những lập trình viên tâm thần tìm đến bạn.

Tên biến luôn phải viết thường, và các từ tạo thành tên biến phải được ngăn cách bằng dấu gạch dưới. Quy ước này được gọi là **snake_case**.

```ruby
# bad
a = 19
string = "John"

# good
age = 19
name = "John"
can_swim = false
```

#### Biến là các tham chiếu

Thông tin mà bạn đặt tên bằng một biến được lưu trong bộ nhớ máy tính, vì vậy về bản chất, một biến là một tham chiếu hoặc con trỏ đến địa chỉ đó trong bộ nhớ. Điều này rất quan trọng vì đôi khi nó có thể gây ra hành vi ngoài dự kiến trong mã của bạn.

Hãy xem một ví dụ về hành vi ngoài dự kiến này, với hai biến: `desired_location`, được gán chuỗi "Barcelona", và `johns_location`, được gán biến `desired_location`. Cả hai biến đều trỏ đến nơi "Barcelona" được lưu trong bộ nhớ.

```ruby
desired_location = "Barcelona"
johns_location = desired_location

desired_location  #=> "Barcelona"
johns_location    #=> "Barcelona"
```

Hành vi ngoài dự kiến xảy ra nếu chuỗi "Barcelona" được lưu trong bộ nhớ bị sửa đổi. Một cách sửa đổi chuỗi là sử dụng phương thức `upcase!`, thay vì phương thức an toàn `upcase`. Nếu chuỗi được sửa đổi bằng `johns_location.upcase!`, thì `desired_location` cũng sẽ phản ánh thay đổi đó:

```ruby
johns_location.upcase!  #=> "BARCELONA"

desired_location        #=> "BARCELONA"
johns_location          #=> "BARCELONA"
```

Ở thời điểm này của bài học, ví dụ này có thể khó hiểu trọn vẹn. Khái niệm quan trọng là việc gán biến này cho biến khác có thể gây ra những tác động phụ ngoài ý muốn. Việc bạn có thể làm điều đó không có nghĩa là bạn nên làm. Bạn sẽ có dịp xem lại ví dụ này trong một bài tập sắp tới.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc chương [Các biến](https://launchschool.com/books/ruby/read/variables) trong cuốn *Nhập môn lập trình với Ruby* xuất sắc của LaunchSchool. Như bài viết này chỉ ra, hãy nhớ rằng bạn không nên sử dụng `$global_variables`. Ngoài ra, `@@class_variables` hiếm khi cần thiết và rất dễ bị sử dụng sai.
1. Đọc các bài học ngắn gọn, súc tích về biến của Ruby Monstas:
      - [Tổng quan về biến](http://ruby-for-beginners.rubymonstas.org/variables.html)
      - [Sử dụng lại tên biến](http://ruby-for-beginners.rubymonstas.org/variables/reusing_names.html)
      - [Vế bên phải được thực hiện trước](http://ruby-for-beginners.rubymonstas.org/variables/right_goes_first.html)
1. Sử dụng IRB trên dòng lệnh hoặc một môi trường REPL trực tuyến phù hợp, rồi thử đặt tên cho một số biến và gán giá trị cho chúng. Ở giai đoạn này, đừng quá lo lắng về các quy ước đặt tên tốt. Thay vào đó, hãy thử nghiệm với nhiều tên biến khác nhau và xem tên nào hợp lệ. Hãy thử dùng ký hiệu hoặc chữ số trong tên biến. Hãy thử gán một biến cho một biến khác và quan sát hành vi khi sử dụng `upcase!`, như trong ví dụ trên. Nếu gặp điều gì kỳ lạ, hãy tra Google để tìm hiểu lý do.

</div>
