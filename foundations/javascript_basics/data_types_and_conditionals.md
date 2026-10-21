### Giới thiệu

Trong JavaScript, bạn sẽ gặp một số kiểu dữ liệu cực kỳ phổ biến; các bài học nền tảng này sẽ giúp bạn xây dựng nền tảng thật vững về tất cả chúng. Tuy nhiên, trước khi đi sâu hơn, hãy đọc [tổng quan về những kiểu dữ liệu phổ biến nhất trong JavaScript](http://javascript.info/types) này.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Kể tên tám kiểu dữ liệu trong JavaScript.
- Hiểu sự khác nhau giữa dấu nháy đơn, dấu nháy kép và dấu nháy ngược.
- Nhúng một biến/biểu thức vào chuỗi.
- Hiểu method là gì.
- Kể tên ba toán tử logic.
- Hiểu các toán tử so sánh là gì.
- Hiểu conditional là gì.
- Hiểu nesting là gì.
- Hiểu các giá trị truthy và falsy là gì.

### Chuỗi

Tùy vào loại công việc đang làm, có thể bạn sẽ làm việc với các đoạn văn bản nhiều hơn là với những con số. **Chuỗi** là một đoạn văn bản... và là một khối xây dựng nền tảng của ngôn ngữ này.

1. Đọc và viết code theo [bài hướng dẫn của MDN về chuỗi trong JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Strings). Bỏ qua phần `Concatenation in context`, vì phần này đề cập đến những khái niệm sẽ được học trong một bài sau về DOM Manipulation.
1. Học qua [bài của W3Schools về các method của chuỗi](https://www.w3schools.com/js/js_string_methods.asp) để biết thêm một chút về những gì bạn có thể làm với chuỗi. **Method** là một chức năng được tích hợp sẵn trong ngôn ngữ hoặc trong những kiểu dữ liệu cụ thể.
1. Có nhiều method tích hợp sẵn cho chuỗi hơn những gì tài liệu trước đã giới thiệu. Bạn không cần đọc hết, càng không cần ghi nhớ chúng, nhưng nên biết đến [tài liệu MDN về chuỗi](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String). Tại đây bạn sẽ tìm thấy nhiều thông tin hơn về chuỗi cũng như tài liệu tham khảo đầy đủ về các method tích hợp sẵn, nên hãy lưu lại trang này!

### Conditional

Giờ là lúc đến với phần thú vị... Cho đến nay, những gì chúng ta làm với chương trình vẫn chưa vượt quá những gì bạn có thể làm bằng các kỹ năng toán cơ bản. Đúng là chúng ta đã chỉ cho máy tính cách thực hiện phép tính để nó làm nhanh hơn, nhưng cốt lõi của lập trình là dạy máy tính cách đưa ra quyết định để thực hiện những việc phức tạp hơn. Conditional là cách chúng ta làm điều đó. Hãy đọc các tài liệu sau:

1. Bước đầu tiên để học về conditional là chắc chắn rằng bạn nắm vững [phép so sánh](http://javascript.info/comparison).
1. [Bài viết của MDN về conditional](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/conditionals) là phần giới thiệu rất hay về nhiều cách viết code có điều kiện.
1. [Bài học của JavaScript.info về `if/else`](http://javascript.info/ifelse) trình bày cùng khái niệm cơ bản (hãy đọc để ôn lại!) và, quan trọng hơn, có các bài tập quen thuộc ở cuối trang!
1. Học qua [bài học của JavaScript.info về các toán tử logic](http://javascript.info/logical-operators). Lưu ý nhỏ về các bài tập trong bài đọc này: bạn sẽ gặp những câu hỏi có `alert()` với một số hoặc chuỗi bên trong dấu ngoặc đơn. Nội dung đang diễn ra ở đây sẽ được giải thích trong phần sau của chương trình học. Một số đáp án có thể chưa có ý nghĩa với bạn lúc này, nhưng chúng chính xác, và bạn sẽ hiểu chúng khi tiến bộ trong chương trình học. Hiện tại, đừng quá lo lắng về điều đó!
1. [Tìm hiểu về câu lệnh `switch`](https://javascript.info/switch), vốn rất tiện khi bạn có nhiều điều kiện.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Để bạn có cơ hội thực hành nhiều hơn, chúng tôi đã tạo các bài tập JavaScript để bạn tự khám phá. Các bài tập có kèm những test dùng để kiểm tra xem code của bạn có hoạt động đúng như mong đợi hay không. Bất cứ khi nào thấy `return`, điều đó chỉ có nghĩa là khi function chạy xong, nó sẽ trả về phần đứng sau `return`. Trong các bài học sau, chúng ta sẽ tìm hiểu những khái niệm này chi tiết hơn nhiều, nên hiện tại hãy cứ kiên nhẫn nhé!

Hãy làm theo đúng thứ tự được trình bày ở đây. Đọc tất cả hướng dẫn, theo dõi terminal và đọc toàn bộ lỗi.

1. Làm theo [hướng dẫn trong README của repository `javascript-exercises`](https://github.com/TheOdinProject/javascript-exercises#how-to-use-these-exercises) để thiết lập môi trường cục bộ.
1. Sau khi fork repository, clone repository và cài đặt Jest, hãy chuyển đến thư mục [`foundations/data_types_and_conditionals` của repository bài tập JavaScript](https://github.com/TheOdinProject/javascript-exercises/tree/main/foundations/data_types_and_conditionals). Đọc từng file README trước khi lần lượt hoàn thành các bài tập sau:
    - `01_helloWorld` (Bài tập này cố ý rất thân thiện với người mới bắt đầu, nhằm bảo đảm rằng bạn đã thiết lập mọi thứ đúng cách!)
    - `02_addNumbers`
    - `03_numberChecker`
    - `04_mathEquations`
    - `05_joinStrings`

Lưu ý: Có thể tìm lời giải cho các bài tập này trong thư mục `solution` của từng bài.

</div>