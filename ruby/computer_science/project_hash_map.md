### Giới thiệu

Bạn đã biết điều kỳ diệu đằng sau các hash map. Bây giờ là lúc tự viết cách triển khai của riêng mình!

#### Giới hạn

Trước khi bắt đầu, chúng ta cần đặt ra một số quy tắc. Tính chất động của mảng trong Ruby cho phép ta chèn và truy xuất các chỉ mục nằm ngoài phạm vi kích thước của mảng. Ví dụ, nếu tạo một mảng có kích thước `16` để biểu diễn các bucket, không có gì ngăn ta lưu phần tử tại chỉ mục `500`. Điều này đi ngược mục đích giới hạn dung lượng lưu trữ của hash map, vì vậy ta cần áp đặt một số hạn chế.

Hãy sử dụng đoạn mã sau mỗi khi truy cập một bucket thông qua chỉ mục. Ta muốn phát sinh lỗi nếu cố truy cập một chỉ mục nằm ngoài phạm vi:

```ruby
raise IndexError if index.negative? || index >= @buckets.length
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

Bắt đầu bằng cách tạo lớp `HashMap`. Lớp này cần có ít nhất hai biến cho `load factor` và `capacity`. Với `load factor` bằng `0.75`, `capacity` ban đầu phải có kích thước `16`. Sau đó, hãy tạo các phương thức sau:

1. `#hash(key)` nhận một khóa dạng chuỗi và tạo ra mã băm từ khóa đó (trong thực tế, hash map có thể chấp nhận nhiều kiểu dữ liệu khác nhau làm khóa, chẳng hạn như số nguyên và mảng, nhưng hiện tại chúng ta sẽ giữ mọi thứ đơn giản). Ở bài học trước, chúng ta đã triển khai một hàm `hash` khá tốt. Nhắc lại:

   ```ruby
   def hash(key)
     hash_code = 0
     prime_number = 31

     key.each_char { |char| hash_code = prime_number * hash_code + char.ord }

     hash_code
   end
   ```

   Bạn có thể tự do sử dụng hàm này hoặc tự nghiên cứu về các thuật toán băm. Hãy cẩn thận, đây là một chủ đề rất sâu, rất dễ khiến bạn đi quá xa. Hãy nhớ lấy mã băm modulo (`%`) với capacity hiện tại trước khi trả về, để đảm bảo chỉ mục nằm trong phạm vi các bucket (bất kể hash map có tăng trưởng hay không).

   Sau này, khi truy cập các cặp khóa-giá trị, bạn có thể nhầm lẫn giữa khóa và mã băm. Khóa là các chuỗi do người dùng cung cấp và được truyền vào hàm băm; hàm này sau đó sẽ trả về một mã băm. Bạn sẽ không bao giờ truy cập trực tiếp các bucket bằng khóa, mà chỉ bằng mã băm.

1. `#set(key, value)` nhận hai đối số: đối số đầu tiên là khóa, đối số thứ hai là giá trị gắn với khóa đó. Nếu khóa đã tồn tại, giá trị cũ gắn với khóa phải được ghi đè bằng giá trị mới.

   Lưu ý rằng khi hai khóa *khác nhau* tạo ra cùng một mã băm và được gán vào cùng một bucket, đó là va chạm chứ không phải cập nhật. Ví dụ, giả sử `Rama` và `Sita` đều được băm thành `3`. Chúng sẽ nằm trong cùng một bucket mà không ghi đè lên nhau; ta biết đây là va chạm chứ không phải cập nhật vì các khóa khác nhau. Nếu cần, hãy xem lại [bài học về va chạm trong hash map](https://www.theodinproject.com/lessons/ruby-hashmap-data-structure#collisions).

   Hãy nhớ tăng gấp đôi capacity của các bucket khi hash map vượt quá `load factor`. Các phương thức được đề cập ở phần sau của bài tập có thể giúp bạn xử lý logic tăng trưởng, vì vậy bạn có thể để việc triển khai hành vi này đến sau.

1. `#get(key)` nhận một đối số là khóa và trả về giá trị gắn với khóa đó. Nếu không tìm thấy khóa, hãy trả về `nil`.

1. `#has?(key)` nhận một khóa làm đối số và trả về một giá trị Boolean dựa trên việc khóa đó có nằm trong hash map hay không.

1. `#remove(key)` nhận một khóa làm đối số. Nếu khóa được cung cấp có trong hash map, phương thức phải xóa mục nhập có khóa đó và trả về giá trị của mục nhập đã xóa. Nếu khóa không có trong hash map, phương thức phải trả về `nil`.

1. `#length` trả về số lượng khóa được lưu trong hash map.

1. `#clear` xóa tất cả mục nhập trong hash map.

1. `#keys` trả về một mảng chứa tất cả khóa (không phải giá trị) bên trong hash map.

1. `#values` trả về một mảng chứa tất cả giá trị (không phải khóa) bên trong hash map.

1. `#entries` trả về một mảng chứa từng cặp khóa-giá trị trong một mảng riêng, ví dụ: `[[first_key, first_value], [second_key, second_value]]`.

Hãy nhớ rằng hash map của chúng ta không bảo toàn thứ tự chèn khi truy xuất dữ liệu. Việc các khóa và giá trị xuất hiện không theo thứ tự bạn đã chèn là bình thường và được dự đoán trước.

#### Kiểm thử hash map

1. Tạo một tệp Ruby mới.

1. Tạo một thể hiện mới của hash map và đặt load factor thành `0.75`.

   ```ruby
   test = HashMap.new
   ```

1. Điền dữ liệu vào hash map bằng phương thức `#set(key, value)` theo đoạn mã sau:

   ```ruby
   test.set('apple', 'red')
   test.set('banana', 'yellow')
   test.set('carrot', 'orange')
   test.set('dog', 'brown')
   test.set('elephant', 'gray')
   test.set('frog', 'green')
   test.set('grape', 'purple')
   test.set('hat', 'black')
   test.set('ice cream', 'white')
   test.set('jacket', 'blue')
   test.set('kite', 'pink')
   test.set('lion', 'golden')
   ```

1. Sau khi điền hash map bằng dữ liệu trên, mức tải hiện tại của hash map phải là `0.75` (đầy dung lượng).

1. Khi hash map đã đầy, hãy thử ghi đè một vài nút bằng `#set(key, value)`. Thao tác này chỉ được ghi đè các `values` hiện có của các nút, không thêm nút mới, vì vậy `#length` vẫn phải trả về cùng một giá trị và `capacity` phải giữ nguyên.

1. Sau đó, điền vào hash map phần tử cuối cùng bên dưới. Việc này sẽ làm mức tải vượt quá `load factor`, kích hoạt chức năng tăng trưởng của hash map và tăng gấp đôi `capacity`:

   ```ruby
   test.set('moon', 'silver')
   ```

1. Nếu triển khai hash map đúng, mức tải của hash map sau khi mở rộng sẽ giảm xuống thấp hơn nhiều so với load factor, đồng thời các mục nhập sẽ được phân bổ đều giữa các bucket đã mở rộng.

1. Với hash map mới, hãy thử ghi đè một vài nút bằng `#set(key, value)`. Một lần nữa, thao tác này chỉ được ghi đè các `values` hiện có của các nút.

1. Kiểm thử các phương thức còn lại của hash map, chẳng hạn như `#get(key)`, `#has?(key)`, `#remove(key)`, `#length`, `#clear`, `#keys`, `#values` và `#entries`, để kiểm tra xem chúng có tiếp tục hoạt động như mong đợi sau khi hash map được mở rộng hay không.

#### Điểm cộng

- Tạo lớp `HashSet` hoạt động giống như `HashMap` nhưng chỉ chứa `keys`, không có `values`.

</div>