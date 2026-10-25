### Giới thiệu

Bạn hẳn đã khá quen thuộc với module Enumerable được trộn vào các lớp Array và Hash (cùng nhiều lớp khác), cung cấp cho bạn rất nhiều phương thức lặp tiện dụng. Để chứng minh rằng đằng sau nó không có phép màu nào, bạn sẽ tự xây dựng lại những phương thức đó.

Chúng ta sẽ giải thích một ví dụ về cách một enumerable hoạt động để giúp bạn hình dung cách bắt đầu xử lý những phương thức này. Cụ thể, chúng ta sẽ phân tích phương thức enumerable `#find`. Phương thức `#find` tìm phần tử đầu tiên khớp với điều kiện của block được truyền vào rồi trả về phần tử đó. Nếu không tìm thấy phần tử nào, nó sẽ trả về `nil`.

```ruby
a = [1, 2, 3, 4]
a.find { |n| n == 2 }
# results in `2`

a.find { |n| n == 10 }
# results in `nil`
```

Vậy câu hỏi đặt ra là: làm thế nào chúng ta có thể xây dựng lại phương thức này bằng kiến thức về yield và block? Hãy cùng xem qua từng dòng của ví dụ bên dưới.

```ruby
module Enumerable
  def my_find
    self.each do |elem|
      return elem if yield(elem)
    end

    nil
  end
end

a = [1, 2, 3]
a.my_find { |n| n == 2 }
#=> 2
```

Trước hết, ở đây chúng ta đang làm một việc mà có thể bạn chưa từng thấy: thao tác với một class/module đã tồn tại trong ngôn ngữ Ruby. Ruby cho phép bạn làm điều đó. Chúng ta có thể mở lại module `Enumerable` rồi thêm các phương thức tùy chỉnh của mình vào đó.

Với `self.each`, chúng ta gọi phương thức `#each` trên instance của đối tượng đang gọi phương thức này. Trong ví dụ trên, đối tượng đó sẽ là một mảng. Vì vậy, `self` sẽ tham chiếu đến mảng đang gọi `#my_find`. Sau đó, chúng ta có thể dùng phương thức `#each` để lặp qua các phần tử của mảng. Đây chính là lúc `yield` trở nên cực kỳ hữu ích. Khi được gọi bên trong phương thức `#my_find`, `yield` sẽ chuyển quyền điều khiển cho block được cung cấp cho `#my_find`. Trong ví dụ sử dụng ngay bên dưới phần định nghĩa, ta thấy block `{ |n| n == 2 }` được truyền vào phương thức `#my_find`. Bên trong `#my_find`, mỗi phần tử trong mảng sẽ được yield cho block đó dưới dạng một đối số.

Nếu block trả về `true` hoặc một giá trị truthy cho một phần tử, chúng ta lập tức `return` phần tử đó. Nếu không tìm thấy gì, chúng ta sẽ lặp qua toàn bộ mảng rồi thực thi việc trả về `nil` ở cuối cùng. Khá thú vị phải không?

Một điều khác mà có thể bạn chưa quen thuộc là [lớp `Enumerator`](https://docs.ruby-lang.org/en/3.4/Enumerator.html). Nhiều phương thức Enumerable sẽ trả về một instance `Enumerator` nếu không được truyền block. Theo tài liệu, [`Enumerable#find`](https://docs.ruby-lang.org/en/3.4/Enumerable.html#method-i-find) cũng hoạt động như vậy. Nhờ đó, các phương thức Enumerable có thể được nối chuỗi một cách gọn gàng và hiệu quả. May mắn là việc tạo một `Enumerator` khá đơn giản. Bạn chỉ cần sử dụng phương thức `to_enum`.

```ruby
module Enumerable
  def my_find
    # returns an `Enumerator` bound to this method if a block isn't given
    return to_enum(:my_find) unless block_given?

    self.each do |elem|
      return elem if yield(elem)
    end

    nil
  end
end

a = [2, 3, 4, 5, 6, 7, 9]

# find the first element that's odd at an even index
a.my_find.with_index { |num, idx| num.odd? && idx.even? }
#=> 9
```

Đó chính là sức mạnh của Enumerator. Nếu `#find` chỉ có thể luôn trả về một kết quả, chúng ta sẽ không có cách nào kết hợp nó với các phương thức khác bằng cách nối chuỗi như trên.

Bây giờ là lúc bạn thực hành:

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Fork và clone [repo về các enumerable tùy chỉnh](https://github.com/TheOdinProject/custom_enumerable_project) của chúng ta.
1. Làm theo [hướng dẫn cài đặt trong README](https://github.com/TheOdinProject/custom_enumerable_project#installation) để thiết lập repo trên máy của bạn.
1. Xây dựng lại từng [phương thức trong bảng ở cuối README](https://github.com/TheOdinProject/custom_enumerable_project#methods) và đảm bảo tất cả đều vượt qua các bài kiểm thử tương ứng.

</div>

#### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy bạn có thể xem nó như tài liệu bổ sung.

- Jason Mattingly có một bài viết hữu ích về [Tự viết các Enumerable của riêng bạn trong Ruby](https://jsonmattingly.medium.com/writing-your-own-enumerables-in-ruby-18db4fa0e5fe).