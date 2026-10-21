### Giới thiệu

JavaScript *không* có class theo cùng nghĩa với các ngôn ngữ hướng đối tượng khác như Java hay Ruby. Tuy nhiên, ES6 *đã* giới thiệu cú pháp tạo đối tượng sử dụng từ khóa `class`. Về cơ bản, đây là một cú pháp mới *phần lớn* giống với object constructor và prototype mà chúng ta đã học trong bài về constructor.

Trong lịch sử, đặc biệt là khi ES6 mới được phát hành, cú pháp class từng gây tranh cãi vì nó trông giống class của những ngôn ngữ như Java. Mặc dù cú pháp class của JavaScript có vẻ chỉ là syntactic sugar, nó có một số đặc điểm riêng giúp phân biệt với object constructor và prototype. Dù vậy, các cơ chế nền tảng không thay đổi dù cú pháp khác đi (không có kế thừa cổ điển), nhưng điều này thường khiến mọi người bối rối vì cú pháp không nói rõ ràng *thực sự* đang diễn ra với các đối tượng này.

Tuy nhiên, đã khá lâu trôi qua và cú pháp class hiện đã xuất hiện trong nhiều codebase. Ở đây không có nhiều cơ chế mới cần học, chủ yếu chỉ là cú pháp mới cho những khái niệm phần lớn đã quen thuộc.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Giải thích sự khác nhau giữa object constructor và class.
- Giải thích getter và setter là gì.
- Mô tả cú pháp class cơ bản.
- Sử dụng kế thừa với class.
- Giải thích cách triển khai field và method private của class.
- Giải thích thuộc tính và method static là gì.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc về [getter và setter](https://javascript.info/property-accessors) (không cần lo về phần "accessor descriptors" vì chúng ta chưa học chúng). Mặc dù bài viết chưa trình bày class, class vẫn có thể sử dụng getter và setter qua cùng cú pháp (không cần `Object.defineProperty`).
1. Đọc [phần nhập môn về cú pháp class](https://javascript.info/class) của JavaScript.info để có cái nhìn tổng quan về cú pháp class.
1. [Tài liệu về class của MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), như thường lệ, là một nguồn rất tốt để tìm hiểu sâu hơn. Có rất nhiều tính năng cú pháp riêng lẻ, nhưng bạn có thể khám phá chúng dần theo thời gian. Như thường lệ, bạn không cần ghi nhớ mọi thứ chỉ bằng việc đọc tài liệu ngay bây giờ. Một số tính năng đáng xem qua gồm:
   - [Mở rộng class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends) (chẳng hạn có một class Player mở rộng class Person, thêm `Person.prototype` vào chuỗi prototype).
   - [Thuộc tính và method static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static), là các thuộc tính và method được truy cập trên chính class chứ không phải trên một instance của class. Điều này tương tự cách một số method chuỗi được truy cập trên instance của chính chuỗi, ví dụ `someString.slice(0, 5)`, trong khi một số method được gọi trực tiếp trên constructor String, ví dụ `String.fromCharCode(79, 100, 105, 110)`.
   - [Thuộc tính private](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields), cho phép bạn có các thuộc tính hoặc method không thể truy cập từ bên ngoài class, tương tự biến private trong factory function.

#### Thực hành

Quay lại [dự án Library](https://www.theodinproject.com/lessons/node-path-javascript-library) và refactor nó để sử dụng `class` thay cho constructor thông thường. Đừng quên sử dụng quy trình làm việc với nhánh Git mà bạn đã học trong [Revisiting Rock Paper Scissors](https://www.theodinproject.com/lessons/foundations-revisiting-rock-paper-scissors) để làm việc trên một tính năng mới. Bạn nên tập quen với cách làm việc này!

</div>