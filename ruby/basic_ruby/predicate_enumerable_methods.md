### Giới thiệu

Trong bài học trước, bạn đã học một số phương thức enumerable rất tiện dụng như `#each`, `#map` và `#select`. Trong bài này, chúng ta sẽ học về *nhiều* phương thức enumerable hơn nữa! Tuyệt vời! Bữa tiệc ở The Odin Project không bao giờ kết thúc!

Lần này, chúng ta tập trung vào một nhóm con cụ thể của các phương thức enumerable: các **phương thức enumerable dạng vị từ**. Bạn hẳn còn nhớ từ bài học về Methods rằng một phương thức vị từ được nhận biết bởi dấu hỏi (`?`) ở cuối tên phương thức và trả về `true` hoặc `false`. Một lần nữa, chúng ta sẽ không đi qua tất cả các phương thức enumerable dạng vị từ, vì vậy hãy nhớ xem qua [tài liệu Ruby](https://docs.ruby-lang.org/en/3.4/) để biết [Enumerable](https://docs.ruby-lang.org/en/3.4/Enumerable.html) còn cung cấp những gì khác.

Với tất cả các ví dụ trong bài học này, bạn cứ tự gõ và chạy theo trong irb hoặc bất kỳ môi trường REPL trực tuyến phù hợp nào để hiểu rõ hơn cách chúng hoạt động.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Mô tả phương thức enumerable dạng vị từ là gì.
- Giải thích cách phương thức `#include?` hoạt động.
- Giải thích cách phương thức `#any?` hoạt động.
- Giải thích cách phương thức `#all?` hoạt động.
- Giải thích cách phương thức `#none?` hoạt động.

### Phương thức include?

Phương thức `#include?` hoạt động đúng như bạn nghĩ. Nếu muốn biết một phần tử cụ thể có tồn tại trong một mảng hay không, ta có thể dùng phương thức `#include?`. Phương thức này trả về `true` nếu phần tử bạn truyền vào `#include?` dưới dạng đối số tồn tại trong mảng hoặc hash; nếu không, nó trả về `false`.

Trước tiên, hãy cùng tìm hiểu cách thực hiện việc này bằng phương thức `#each`:

```ruby
numbers = [5, 6, 7, 8]
element = 6
result = false

numbers.each do |number|
  if number == element
    result = true
    break
  end
end

result
# => true

element = 3
result = false

numbers.each do |number|
  if number == element
    result = true
    break
  end
end

result
#=> false
```

<div class="lesson-note" markdown="1">

#### Thoát khỏi vòng lặp

Nếu để ý, bạn sẽ thấy có câu lệnh `break` trong điều kiện `if`, để vòng lặp dừng thực thi ngay khi tìm thấy số đó. Nếu không, vòng lặp vẫn sẽ duyệt qua toàn bộ các số trong mảng, dù điều đó không cần thiết vì ta đã tìm thấy phần tử rồi.

</div>

Dùng `#include?`, ta có thể rút gọn đáng kể đoạn mã này:

```ruby
numbers = [5, 6, 7, 8]

numbers.include?(6)
#=> true

numbers.include?(3)
#=> false
```

Một ví dụ khác là quay lại các mảng `friends` và `invited_friends` từ bài học trước:

```ruby
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

invited_list = friends.select { |friend| friend != 'Brian' }

invited_list.include?('Brian')
#=> false
```

### Phương thức any?

Có lẽ bạn cũng đoán được phương thức `#any?` làm gì. Nó trả về `true` nếu *bất kỳ* phần tử nào trong mảng hoặc hash khớp với điều kiện bên trong block; nếu không, nó trả về `false`.

Giả sử ta muốn xem trong một mảng số có số nào lớn hơn 500 hoặc nhỏ hơn 20 hay không. Trước tiên, hãy xem ta có thể thực hiện việc đó bằng `#each` như thế nào.

```ruby
numbers = [21, 42, 303, 499, 550, 811]
result = false

numbers.each do |number|
  if number > 500
    result = true
    break
  end
end

result
#=> true

numbers = [21, 42, 303, 499, 550, 811]
result = false

numbers.each do |number|
  if number < 20
    result = true
    break
  end
end

result
#=> false
```

Dùng `#any?`, ta có thể rút gọn đáng kể đoạn mã này:

```ruby
numbers = [21, 42, 303, 499, 550, 811]

numbers.any? { |number| number > 500 }
#=> true

numbers.any? { |number| number < 20 }
#=> false
```

### Phương thức all?

Phương thức `all?` cũng khá dễ hiểu. Nó chỉ trả về `true` nếu *tất cả* các phần tử trong mảng hoặc hash khớp với điều kiện bạn đặt ra bên trong block; nếu không, nó trả về `false`.

Giả sử ta muốn kiểm tra xem tất cả các từ trong danh sách có dài hơn 3 ký tự hoặc 6 ký tự hay không. Trước tiên, hãy xem ta có thể thực hiện việc đó bằng `#each` như thế nào:

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
matches = []
result = false

fruits.each do |fruit|
  if fruit.length > 3
    matches.push(fruit)
  end
end

result = fruits.length == matches.length
result
#=> true

fruits = ["apple", "banana", "strawberry", "pineapple"]
matches = []
result = false

fruits.each do |fruit|
  if fruit.length > 6
    matches.push(fruit)
  end
end

result = fruits.length == matches.length
result
#=> false
```

Dùng `#all?`, ta có thể rút gọn đáng kể đoạn mã này:

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]

fruits.all? { |fruit| fruit.length > 3 }
#=> true

fruits.all? { |fruit| fruit.length > 6 }
#=> false
```

Một lưu ý đặc biệt khi gỡ lỗi: `#all?` mặc định sẽ trả về `true`, trừ khi block trả về `false` hoặc `nil`. Vì vậy, nếu gọi `#all?` trên một mảng hoặc hash rỗng (tức là không có phần tử nào để block đánh giá), nó sẽ trả về `true`.

### Phương thức none?

Đúng như bạn có thể mong đợi, `#none?` chỉ trả về `true` nếu điều kiện trong block không khớp với *bất kỳ* phần tử nào trong mảng hoặc hash; nếu không, nó trả về `false`.

Trước tiên, hãy xem có thể thực hiện việc này bằng `#each` như thế nào. Bạn sẽ nhận thấy cách tiếp cận này rất giống với những gì ta đã làm cho `#all?`.

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]
result = false

fruits.each do |fruit|
  if fruit.length > 10
    result = false
    break
  end

  result = true
end

result
#=> true

fruits = ["apple", "banana", "strawberry", "pineapple"]
result = false

fruits.each do |fruit|
  if fruit.length > 6
    result = false
    break
  end

  result = true
end

result
#=> false
```

Dùng `#none?`, ta có thể rút gọn đáng kể:

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]

fruits.none? { |fruit| fruit.length > 10 }
#=> true

fruits.none? { |fruit| fruit.length > 6 }
#=> false
```

### Phương thức one?

Ruby có các phương thức để kiểm tra xem tất cả, bất kỳ hay không phần tử nào trong một mảng khớp với một điều kiện. Nhưng nếu bạn cần chính xác một phần tử khớp—không hơn, không kém—thì sao? Ruby đã có sẵn phương thức `#one?`, phương thức này trả về true nếu block cho kết quả true với chính xác một phần tử.

```ruby
fruits = ["apple", "banana", "strawberry", "pineapple"]

fruits.one? { |fruit| fruit.length > 9 }
#=> true

fruits.one? { |fruit| fruit.length > 2 }
#=> false
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc [Cách sử dụng Any, All, None và One trong Ruby](https://www.rubyguides.com/2018/10/any-all-none-one/) để tham khảo cách giải thích khác về các enumerable dạng vị từ.
  1. Hoàn thành các bài tập về enumerable dạng vị từ trong [repo ruby-exercises](https://github.com/TheOdinProject/ruby-exercises) mà bạn đã clone trước đó.

</div>
