### Giới thiệu

Trong Khoa học máy tính, một trong những cấu trúc dữ liệu cơ bản và nền tảng nhất là danh sách liên kết,
hoạt động tương tự như một mảng. Ưu điểm chính của danh sách liên kết so với mảng thông thường là các phần tử
của danh sách có thể dễ dàng được thêm vào hoặc xóa đi mà không cần phân bổ lại bất kỳ phần tử nào khác.

Trong một số ngôn ngữ lập trình, kích thước của mảng là một vấn đề, và một trong những cách khắc phục vấn đề đó
để cho phép dữ liệu được cấp phát động là sử dụng danh sách liên kết.

May mắn là trong **Ruby**, mảng không bị giới hạn ở một kích thước nhất định, đồng thời việc thêm và xóa phần tử có thể được thực hiện dễ dàng tại bất kỳ chỉ mục nào bằng phương thức mảng dựng sẵn phù hợp, vì vậy bạn không cần phải suy nghĩ về cách khắc phục những hạn chế đó.

Vậy nếu kích thước mảng, việc thêm phần tử vào mảng và việc xóa phần tử khỏi mảng không phải là những hạn chế trong Ruby, liệu danh sách liên kết có thực sự cần thiết không?
Câu trả lời ngắn gọn là *tùy trường hợp*; tuy nhiên, đây là cấu trúc dữ liệu động đơn giản nhất và sẽ cho bạn một nền tảng vững chắc để có thể hiểu các cấu trúc dữ liệu phức tạp hơn như đồ thị và cây nhị phân một cách dễ dàng hơn.

### Cấu trúc của danh sách liên kết

Một *danh sách liên kết* là một tập hợp tuyến tính gồm các phần tử dữ liệu gọi là các nút, chúng “trỏ”
đến nút tiếp theo bằng một con trỏ.

Mỗi nút chứa một phần tử dữ liệu duy nhất cùng một liên kết hoặc con trỏ đến nút tiếp theo trong danh sách.

Nút đầu là nút đầu tiên trong danh sách, còn nút cuối là nút cuối cùng trong danh sách. Dưới đây là cách biểu diễn cơ bản của một danh sách liên kết:

`[ NODE(head) ] -> [ NODE ] -> [ NODE(tail) ] -> nil`

Để xem phần giải thích kỹ hơn, hãy tham khảo các tài liệu sau:

1. [Danh sách liên kết bằng tiếng Anh dễ hiểu](https://www.youtube.com/watch?v=oiW79L8VYXk)
1. [Danh sách liên kết là gì vậy?](https://dev.to/vaidehijoshi/whats-a-linked-list-anyway)
1. [Danh sách liên kết, cấu trúc dữ liệu còn thiếu của Ruby](https://www.sitepoint.com/rubys-missing-data-structure/)
1. [Phần giải thích chi tiết hơn với nhiều sơ đồ](https://web.archive.org/web/20200217010131/http://www.cs.cmu.edu/~adamchik/15-121/lectures/Linked%20Lists/linked%20lists.html)
1. [Danh sách liên kết có cần thiết không?](https://dev.to/karimdevelops/are-linked-lists-necessary-2ckl)

### Bài tập

<div class="lesson-content__panel" markdown="1">

Bạn sẽ cần hai lớp:

1. Lớp `LinkedList`, dùng để biểu diễn toàn bộ danh sách.
1. Lớp `Node`, chứa hai biến instance là `#value` và `#next_node`, mặc định đều được đặt là `nil`.

Hãy xây dựng các phương thức sau trong lớp danh sách liên kết của bạn:

1. `#append(value)` thêm một nút mới chứa `value` vào cuối danh sách.
1. `#prepend(value)` thêm một nút mới chứa `value` vào đầu danh sách.
1. `#size` trả về tổng số nút trong danh sách.
1. `#head` trả về giá trị của nút đầu tiên trong danh sách. Nếu danh sách rỗng, phương thức phải trả về nil.
1. `#tail` trả về giá trị của nút cuối cùng trong danh sách. Nếu danh sách rỗng, phương thức phải trả về nil.
1. `#at(index)` trả về giá trị của nút tại chỉ mục đã cho. Nếu không có nút nào tại chỉ mục đó, phương thức phải trả về nil.
1. `#pop` xóa nút đầu khỏi danh sách và trả về giá trị của nút đó. Nếu được gọi trên một danh sách rỗng, phương thức chỉ cần trả về nil.
1. `#contains?(value)` trả về true nếu value được truyền vào có trong danh sách, ngược lại trả về false.
1. `#index(value)` trả về chỉ mục của nút chứa value. Nếu không tìm thấy giá trị, phương thức phải trả về nil. Nếu có nhiều hơn một nút chứa giá trị trùng với giá trị đã cho, phương thức phải trả về chỉ mục của nút đầu tiên có giá trị phù hợp.
1. `#to_s` biểu diễn các đối tượng LinkedList của bạn dưới dạng chuỗi, để bạn có thể in chúng ra và xem trước trong bảng điều khiển. Nếu danh sách rỗng, phương thức phải trả về một chuỗi rỗng. Định dạng phải là: `( value ) -> ( value ) -> ( value ) -> nil`.

#### Bài tập thêm điểm

1. `#insert_at(index, *values)` phải chèn các nút mới với những giá trị đã cho tại chỉ mục đã cho. Ví dụ:

    ```ruby
    # list that's already set up
    puts list
    # => "( 1 ) -> ( 2 ) -> ( 3 ) -> nil"

    list.insert_at(1, 10, 11)
    puts list
    # => "( 1 ) -> ( 10 ) -> ( 11 ) -> ( 2 ) -> ( 3 ) -> nil"
    ```

    Nếu phương thức được gọi với một chỉ mục nằm ngoài phạm vi (nhỏ hơn 0 hoặc lớn hơn kích thước của danh sách), hãy phát sinh một `IndexError`.

1. `#remove_at(index)` xóa nút tại `index` đã cho. Nếu chỉ mục đã cho nằm ngoài phạm vi (nhỏ hơn 0 hoặc lớn hơn hoặc bằng kích thước của danh sách), hãy phát sinh một `IndexError`.

**Mẹo cho bài tập thêm điểm:** Khi chèn hoặc xóa một nút, hãy cân nhắc việc đó sẽ ảnh hưởng đến các nút hiện có như thế nào. Một số nút sẽ cần cập nhật liên kết `#next_node` của chúng.

#### Tự kiểm tra

Hãy thử nghiệm Danh sách liên kết mà bạn đã tạo!

1. Tạo một tệp `main.rb` và bảo đảm tệp đó yêu cầu tệp chứa `LinkedList`. Đây là nơi chúng ta sẽ kiểm tra danh sách.

1. Tạo một đối tượng `LinkedList` và thêm các nút vào đó:

   ```ruby
   list = LinkedList.new

   list.append('dog')
   list.append('cat')
   list.append('parrot')
   list.append('hamster')
   list.append('snake')
   list.append('turtle')
   ```

1. Thêm `puts list` vào cuối tệp rồi chạy tệp. Vì cách đặt tên, câu lệnh này sẽ sử dụng phương thức `#to_s` mà bạn đã viết trước đó.

1. Nếu mọi thứ hoạt động đúng, kết quả xuất ra phải là `( dog ) -> ( cat ) -> ( parrot ) -> ( hamster ) -> ( snake ) -> ( turtle ) -> nil`. Bạn có thể dùng các giá trị khác để kiểm tra nếu muốn.

</div>
