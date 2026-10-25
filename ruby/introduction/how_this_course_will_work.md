### Giới thiệu

Khóa học này tập trung vào việc dạy bạn **Ruby**, ngôn ngữ được thiết kế đặc biệt để lập trình viên cảm thấy hạnh phúc. Còn gì có thể tuyệt hơn thế nữa?! Một tác vụ cần hàng chục dòng mã để hoàn thành trong Java hoặc cả trăm dòng trong C có thể chỉ cần vài dòng trong Ruby, nhờ Ruby đóng gói sẵn rất nhiều hàm hữu ích thành những phương thức tiện lợi và dễ sử dụng.

Ruby và Python khá giống nhau, và ở một vài khía cạnh còn gợi nhớ đến các ngôn ngữ La-tinh; một khi đã học một ngôn ngữ, bạn sẽ không quá khó để làm quen với ngôn ngữ còn lại vì chúng thường tuân theo nhiều quy ước giống nhau. Python thường được giảng dạy nhiều hơn ở các trường đại học và thường được dùng cho những ứng dụng cần xử lý nhiều dữ liệu hoặc đòi hỏi nhiều năng lực xử lý hơn.

Tuy nhiên, Ruby có một vũ khí bí mật khiến các nhà phát triển web thích lặp nhanh say mê: framework **Ruby on Rails**, mà chúng ta sẽ học trong một khóa sau. Rails được tối ưu để viết mã nhanh hơn và ít đau đầu hơn, cho phép bạn lặp lại thường xuyên hơn khi xây dựng một website. Lặp nhanh hơn đồng nghĩa với việc sản phẩm cuối cùng có nhiều khả năng đáp ứng nhu cầu của khách hàng hoặc người dùng hơn, từ đó giúp các dự án của bạn thành công hơn.

Bạn sẽ học được khá nhiều trong khóa học này, nhưng đến cuối khóa, bạn sẽ xây dựng được một số dự án rất thú vị, bao gồm **Tic Tac Toe**, **Hangman**, và thậm chí cả **Chess**. Bạn sẽ có thể lưu và mở tệp, viết bài kiểm thử cho mã của mình, tách phần mã spaghetti thành những lớp riêng biệt, gọn gàng, và thậm chí tự hiện thực một số thuật toán cùng cấu trúc dữ liệu cơ bản để giải quyết các bài toán phức tạp.

**Nói ngắn gọn, bạn sẽ bắt đầu cảm thấy mình giống một lập trình viên thực thụ hơn rất nhiều, và cảm giác đó hoàn toàn có cơ sở.**

### Học Ruby trước Ruby on Rails

Một số người cho rằng bạn có thể lao thẳng vào Rails và bắt đầu nhanh chóng tạo ra các website. Vậy tại sao chúng ta lại dành hẳn một khóa học chỉ cho Ruby trước khi tìm hiểu Rails? Rails là một framework được xây dựng hoàn toàn bằng Ruby, và từng phần mã trong đó đều được viết bằng Ruby. Khi (*không phải* là "nếu") có gì đó trong dự án của bạn bị hỏng, bạn phải có khả năng gỡ lỗi; điều đó đòi hỏi bạn hiểu vững Ruby, cách Ruby hoạt động, cũng như hình thức các thông báo lỗi của nó. Hơn nữa, khi bắt đầu mở rộng khả năng của mình và làm điều gì đó vượt ra ngoài những gì các bài hướng dẫn cơ bản chỉ rõ, bạn sẽ nhanh chóng thấy vô cùng nản lòng nếu không có nền tảng Ruby vững chắc. Khoảng thời gian bạn phải dành để Google các thông báo lỗi rồi ngồi nhìn chằm chằm vào tài liệu trợ giúp sẽ hữu ích hơn nhiều nếu được dùng để học Ruby ngay từ bây giờ.

### Chặng đường phía trước

Ruby là một ngôn ngữ lớn với rất nhiều cú pháp, vì vậy chúng ta đã chia các khái niệm thành những phần nhỏ hơn để bạn dễ tiếp thu. Một số nội dung ban đầu sẽ khá đơn giản và dựa vào các bài tập thân thiện với người mới để củng cố hiểu biết của bạn. Khi tiến bộ qua khóa học, mục tiêu là giúp bạn hiểu tài liệu sâu sắc và thực tế hơn nhiều. Khi chuyển sang những chủ đề nâng cao hơn, bạn sẽ đọc ít hơn và xây dựng nhiều hơn — đúng như cách nên làm!

- Trong phần đầu tiên, bạn sẽ học toàn bộ kiến thức cơ bản về Ruby cùng với một số nền tảng lập trình kinh điển.
- Trong phần thứ hai, bạn sẽ học về lập trình hướng đối tượng, qua đó biết cách tổ chức mã của mình thành các đối tượng có thể tái sử dụng một cách rõ ràng và ngăn nắp.
- Trong phần thứ ba, bạn sẽ tìm hiểu một số kiến thức khoa học máy tính cơ bản, trong đó có đệ quy và các cấu trúc dữ liệu phổ biến.
- Trong phần thứ tư, bạn sẽ học thêm một số tính năng nâng cao của Git và quy trình làm việc với Git được các nhà phát triển chuyên nghiệp sử dụng.
- Trong phần thứ năm, bạn sẽ học những kiến thức cơ bản về phát triển hướng kiểm thử.
- Trong phần thứ sáu và cũng là phần cuối, bạn sẽ kết nối mọi thứ lại bằng cách xây dựng một trò chơi cờ vua hoàn chỉnh.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note lesson-note--warning" markdown="1">

#### Chatbot AI của LaunchSchool

Khóa học này thường xuyên liên kết đến các tài nguyên của LaunchSchool, và gần đây họ đã tích hợp một chatbot hỗ trợ việc học vào nền tảng của mình. Chúng tôi không khuyến khích bạn sử dụng chatbot đó để học trong chương trình này vì những lý do đã được trình bày trong [bài học Động lực và Tư duy](https://www.theodinproject.com/lessons/foundations-motivation-and-mindset#a-note-on-ai-code-generation). Nếu bạn từng bị mắc kẹt ở một bài toán hoặc không hiểu một phần nội dung đang đọc, thay vào đó hãy liên hệ với chúng tôi trong [máy chủ Discord của chúng tôi](https://discord.com/invite/fbFCkYabZB).

</div>

1. Đọc phần Giới thiệu của cuốn sách [*Introduction to Programming with Ruby* của Launch School](https://launchschool.com/books/ruby/read/introduction) để tìm hiểu thêm một chút về lịch sử của Ruby.

1. Để hiểu cách bạn sẽ tương tác với Ruby trong suốt khóa học này, hãy đọc chương [Your Tools trong cuốn *Ruby for Beginners*](http://ruby-for-beginners.rubymonstas.org/your_tools.html) của Ruby Monstas. (Lưu ý rằng dù họ khuyến nghị dùng Sublime làm trình soạn thảo văn bản, bất kỳ trình soạn thảo mã nào cũng dùng được. Nói thêm là ở đây chúng tôi vẫn thường thích VS Code hơn!)
1. Bỏ qua phần hướng dẫn cài đặt và đọc chương ["Preparations" trong cuốn *Introduction to Programming with Ruby* của Launch School](https://launchschool.com/books/ruby/read/preparations#usingacodeeditor) để nhận được một số lời khuyên hữu ích về cách sử dụng và học Ruby hiệu quả. Ngoài ra, bạn sẽ chưa thể cài Pry cho đến khi cài Ruby ở bài học tiếp theo.
1. Giờ bạn đã biết một vài cách tương tác với mã Ruby của mình, hãy đọc về [cách sử dụng các tệp mã bên trong IRB](https://stackoverflow.com/questions/13112245/ruby-how-to-load-a-file-into-interactive-ruby-console-irb/38533339), điều này sẽ hữu ích khi bạn cần nhanh chóng khắc phục sự cố và thử nghiệm.

**Được rồi, nói đủ rồi. Hãy học Ruby thôi!**

</div>