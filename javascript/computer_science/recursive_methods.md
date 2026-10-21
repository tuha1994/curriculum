### Giới thiệu

Recursion là ý tưởng một function gọi chính nó. Chỉ có vậy. Nó được dùng để lấy một bài toán lớn và bắt đầu chia nó thành những phần ngày càng nhỏ ("Divide and Conquer"), rồi tiếp tục đưa lời giải của chúng trở lại function ban đầu cho đến khi đạt được một dạng câu trả lời và toàn bộ chuỗi được tháo ngược.

Từ [Wikipedia entry on Divide and Conquer Algorithms](http://en.wikipedia.org/wiki/Divide_and_conquer_algorithm):

> Trong computer science, divide and conquer (D&C) là một paradigm thiết kế algorithm quan trọng dựa trên recursion nhiều nhánh. Algorithm divide and conquer hoạt động bằng cách đệ quy chia một bài toán thành hai hoặc nhiều bài toán con cùng loại (hoặc có liên quan), cho đến khi chúng đủ đơn giản để giải trực tiếp. Sau đó các lời giải của bài toán con được kết hợp để tạo ra lời giải cho bài toán ban đầu.

Ngoài ra còn có cách dùng recursion đúng và sai. Thực tế là bất kỳ bài toán nào bạn giải được bằng recursion cũng có thể giải bằng các iterator bạn đã quen thuộc và yêu thích. Nếu bạn thấy mình nói "sao lúc đó mình không dùng vòng `while`?" thì có lẽ bạn nên dùng nó. Bạn không thường xuyên dùng lời giải đệ quy cho một bài toán, nhưng nên có cảm nhận về khi nào nó là ý tưởng tốt. Một số bài toán cũng bị chia thành quá nhiều phần và hoàn toàn làm bộ nhớ máy tính quá tải. Cần có sự cân bằng.

Trong bài học ngắn này, bạn sẽ có cơ hội tìm hiểu thêm về khi nào và cách dùng recursion, rồi trong project tiếp theo bạn sẽ áp dụng một phần kiến thức đó (vì có lẽ nó sẽ chưa thực sự ghi nhớ cho đến khi bạn thử).

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài học này.

- Hiểu tại sao recursion là kỹ thuật hữu ích để giải quyết các bài toán lớn.
- Nhận biết các giới hạn của lời giải đệ quy.
- Xác định loại bài toán nào nên giải bằng loop thay vì recursion.
- Tìm hiểu khái niệm "recursive depth" nghĩa là gì.
- Hiểu "stack overflow" là gì (khái niệm, không phải website).
- Tìm hiểu tại sao stack overflow liên quan đến các bài toán đệ quy.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [intro to recursion](https://javascript.info/recursion) này. Bạn *không* cần hoàn thành các bài tập ở cuối bài viết.
1. Xem [explanation of recursion by Web Dev Simplified](https://www.youtube.com/watch?v=6oDQaB2one8).
1. Xem [5 Simple Steps for Solving Any Recursive Problem](https://www.youtube.com/watch?v=ngCos392W4w).
1. Xem [Video on Recursion](https://www.youtube.com/watch?v=mz6tAJMVmfM) từ CS50.
1. Đọc ["Implementation Issues" section of the wiki article](http://en.wikipedia.org/wiki/Divide_and_conquer_algorithm#Implementation_issues) để có cái nhìn tổng quan về một số giới hạn của recursion.
1. Đi tới [the `computer_science/recursion/` directory of The Odin Project's JavaScript exercises repo](https://github.com/TheOdinProject/javascript-exercises/tree/main/computer_science/recursion) và hoàn thành từng bài tập theo thứ tự. Hãy xem README của mỗi bài trước khi hoàn thành. Nếu cần, xem lại README của repo để biết hướng dẫn chung về fork, clone và sử dụng exercise.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Hãy dùng debugger!

   Việc theo dõi điều gì đang diễn ra trong một function đệ quy theo từng layer có thể khá khó, đừng tự làm khổ mình! Nếu bạn chưa dùng debugger khi giải một số bài toán (nếu chưa thì bạn đã bỏ lỡ rất nhiều!), đây là lúc thích hợp để làm quen với nó, vì debugger giúp việc điều hướng luồng code dễ dàng hơn nhiều.

   </div>

</div>