### Giới thiệu

Trong Computer Science, một trong những cấu trúc dữ liệu cơ bản và nền tảng nhất là linked list, hoạt động tương tự array. Lợi ích chính của linked list so với array thông thường là các phần tử trong list có thể dễ dàng được chèn hoặc xóa mà không cần cấp phát lại những phần tử khác.

Trong một số ngôn ngữ lập trình, kích thước của array là vấn đề, và một cách để khắc phục vấn đề đó cũng như cho phép dữ liệu được cấp phát động là dùng linked list.

May mắn là trong **JavaScript**, array không bị giới hạn ở một kích thước nhất định, và việc chèn hay xóa có thể thực hiện đơn giản tại bất kỳ index nào bằng method array tích hợp phù hợp, nên bạn không phải suy nghĩ về việc vượt qua các giới hạn đó.

Vậy nếu kích thước array, chèn array và xóa array không phải giới hạn trong JavaScript, linked list có thực sự cần thiết không?
Câu trả lời ngắn là *tùy trường hợp*; tuy nhiên, đây là cấu trúc dữ liệu động đơn giản nhất và sẽ cho bạn nền tảng vững chắc để hiểu các cấu trúc dữ liệu phức tạp hơn như graph và binary tree dễ dàng hơn.

### Cấu trúc của linked list

Một *linked list* là một tập hợp tuyến tính các phần tử dữ liệu gọi là node, trong đó mỗi node "trỏ" tới node tiếp theo bằng một pointer.

Mỗi node chứa một phần tử dữ liệu duy nhất và một link hoặc pointer tới node tiếp theo trong list.

Node head là node đầu tiên trong list, node tail là node cuối cùng trong list. Dưới đây là biểu diễn cơ bản của linked list:

`[ NODE(head) ] -> [ NODE ] -> [ NODE(tail) ] -> null`

Để có giải thích kỹ hơn, dùng các tài nguyên sau:

1. [Linked Lists in Plain English](https://www.youtube.com/watch?v=oiW79L8VYXk)
1. [What's a Linked List, Anyway?](https://dev.to/vaidehijoshi/whats-a-linked-list-anyway)
1. [A more verbose explanation with plenty of diagrams](https://web.archive.org/web/20200217010131/http://www.cs.cmu.edu/~adamchik/15-121/lectures/Linked%20Lists/linked%20lists.html)
1. [Are Linked Lists necessary?](https://dev.to/karimdevelops/are-linked-lists-necessary-2ckl)

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Chạy ES6 module trong Node

Node v22 (đã trở thành LTS vào October 2024) hiện có thể tự động phát hiện ES6 module và chạy chúng mà không cần cấu hình thêm. Nếu bạn dùng ES6 module và gặp lỗi vì Node không nhận ra syntax, hãy bảo đảm [update Node to the latest LTS version](https://www.theodinproject.com/lessons/foundations-installing-node-js#installing-node).

</div>

Bạn sẽ cần hai class hoặc factory:

1. Class/factory `LinkedList`, đại diện cho toàn bộ list.
1. Class/factory `Node`, chứa property `value` và property `nextNode`, mặc định cả hai đều là `null`.

Xây dựng các function sau trong class/factory linked list:

1. `append(value)` thêm node mới chứa `value` vào cuối list.
1. `prepend(value)` thêm node mới chứa `value` vào đầu list.
1. `size()` trả về tổng số node trong list.
1. `head()` trả về value của node đầu tiên trong list. Nếu list rỗng, trả về `undefined`.
1. `tail()` trả về value của node cuối cùng trong list. Nếu list rỗng, trả về `undefined`.
1. `at(index)` trả về value của node tại index đã cho. Nếu không có node ở index đó, trả về `undefined`.
1. `pop()` xóa node head khỏi list và trả về value của nó. Nếu dùng trên list rỗng, chỉ cần trả về `undefined`.
1. `contains(value)` trả về true nếu value truyền vào nằm trong list, nếu không trả về false.
1. `findIndex(value)` trả về index của node chứa value đã cho. Nếu không tìm thấy value trong list, trả về `-1`. Nếu có nhiều node có value khớp với `value`, trả về index của node đầu tiên khớp.
1. `toString()` biểu diễn object LinkedList dưới dạng string để bạn có thể in và xem trước chúng trong console. Nếu list rỗng, trả về string rỗng. Format phải là: `( value ) -> ( value ) -> ( value ) -> null`.

#### Điểm cộng

1. `insertAt(index, ...values)` chèn các node mới với các value đã cho tại `index`. Ví dụ:

   ```javascript
   // list that's already set up
   console.log(list.toString());
   //=> "( 1 ) -> ( 2 ) -> ( 3 ) -> null"

   list.insertAt(1, 10, 11)
   console.log(list.toString());
   //=> "( 1 ) -> ( 10 ) -> ( 11 ) -> ( 2 ) -> ( 3 ) -> null"
   ```

   Nếu gọi method với index nằm ngoài phạm vi (nhỏ hơn 0 hoặc lớn hơn size của list), throw một `RangeError`.

1. `removeAt(index)` xóa node tại `index` đã cho. Nếu index nằm ngoài phạm vi (nhỏ hơn 0 hoặc lớn hơn hoặc bằng size của list), throw một `RangeError`.

**Mẹo điểm cộng:** Khi chèn hoặc xóa một node, hãy xem xét ảnh hưởng của nó đến các node hiện có. Một số node sẽ cần cập nhật link `nextNode`.

#### Thử nghiệm

Hãy kiểm thử Linked List bạn đã tạo!

1. Tạo file `main.js` và bảo đảm nó import class hoặc factory `LinkedList`. Đây là nơi chúng ta sẽ kiểm thử list.
1. Tạo một instance của `LinkedList` và nạp node vào đó:

   ```javascript
   // example uses class syntax - adjust as necessary
   const list = new LinkedList();

   list.append("dog");
   list.append("cat");
   list.append("parrot");
   list.append("hamster");
   list.append("snake");
   list.append("turtle");
   ```

1. Thêm `console.log(list.toString());` vào cuối file và chạy nó.
1. Nếu mọi thứ hoạt động, output phải là:

   ```text
   ( dog ) -> ( cat ) -> ( parrot ) -> ( hamster ) -> ( snake ) -> ( turtle ) -> null
   ```

   Bạn có thể dùng các value khác để kiểm thử nếu muốn.

</div>