### Giới thiệu

Đến thời điểm này, bạn đã viết khá nhiều HTML và CSS thuần, đồng thời học được nhiều kỹ thuật thiết kế mà bạn sẽ tiếp tục sử dụng khi phát triển thành nhà phát triển. Trong quá trình học, có thể bạn đã gặp thông tin về framework và preprocessor CSS (hay precompiler). Cả hai loại công cụ này đều có thể giúp việc viết CSS trở nên gọn gàng hơn và bớt tẻ nhạt.

Một lý do hữu ích để biết về framework và preprocessor CSS là chúng thường xuất hiện ở nơi làm việc. Dù người phỏng vấn các vị trí đầu vào có khả năng tập trung nhiều hơn vào nền tảng CSS (ngay cả khi công việc sử dụng một framework hoặc preprocessor cụ thể), việc biết những công cụ này là gì và tìm chúng ở đâu sau khi xác định cần học là điều có ích.

Bạn nên lưu ý rằng ở giai đoạn học hiện tại, bạn được khuyên tiếp tục sử dụng CSS thuần trong các dự án. Tất cả framework và preprocessor này đều được xây dựng quanh CSS, vì vậy việc phát triển nền tảng vững chắc sẽ giúp bạn học và chuyển đổi giữa các framework hoặc preprocessor dễ dàng hơn đáng kể trong tương lai. Cố học một công cụ trong suốt khóa học này sẽ không hiệu quả hoặc có giá trị lâu dài bằng việc củng cố kỹ năng CSS nền tảng.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Biết framework CSS là gì.
- Biết một số framework hiện có.
- Biết preprocessor là gì.
- Biết một số preprocessor hiện có.

### Tổng quan về framework

Các framework khác nhau có mục tiêu khác nhau. Những framework như [Bootstrap](https://getbootstrap.com/) đảm nhiệm nhiều phần việc nặng bằng cách đóng gói sẵn mã CSS thường dùng, thậm chí cả icon và tương tác (như menu dropdown). Chúng được thiết kế để trừu tượng hóa quá trình viết các phần tử trực quan, có thể tái sử dụng và responsive. Những công cụ như [Tailwind](https://tailwindcss.com/) hướng đến việc thay đổi cách chúng ta áp dụng CSS thông qua một cú pháp khác, bằng cách cung cấp các class được đặt tên sẵn, thường chỉ áp dụng một dòng CSS. Về bản chất, framework CSS chỉ là một gói CSS mà bạn có thể sử dụng và truy cập thông qua các class do framework định nghĩa. Ví dụ, nhiều framework cung cấp một class tên `.btn`, class này thêm mọi style cần thiết cho button mà bạn không phải tự viết CSS. Nhìn chung, để dùng framework, bạn cần hiểu nó yêu cầu bạn cấu trúc website như thế nào và dùng những class nào để áp dụng bộ style riêng của nó.

Bạn nên biết rằng có khá nhiều framework hiện có. Hai framework khác bạn có thể gặp là [Bulma](https://bulma.io/) và [Foundation](https://get.foundation), nhưng còn rất nhiều framework khác.

### Nhược điểm của framework

Framework rất hữu ích để nhanh chóng tạo ra các website có giao diện mà người dùng cuối có thể tương tác dễ dàng. Tuy nhiên, sau khi xem qua một số framework phổ biến, bạn sẽ bắt đầu nhận thấy rất nhiều điểm tương đồng giữa các website, vì chúng sử dụng các framework tương tự nhau. Ngoài vấn đề đó, **quá nhiều nhà phát triển mới cũng lao vào học framework quá sớm trong quá trình học**; triển vọng không phải luyện viết CSS thuần rất hấp dẫn. Kết quả là nhiều nhà phát triển không có đủ thời gian luyện CSS để củng cố nền tảng của ngôn ngữ rất quan trọng này.

Ngoài ra, việc ghi đè style của framework hoặc gỡ lỗi các vấn đề về style trên trang sẽ trở nên rất khó nếu nền tảng CSS của bạn yếu. Bạn cần hiểu framework đang làm gì "bên dưới lớp vỏ" để có thể xử lý những vấn đề này về sau (và tin chúng tôi đi, bạn sẽ phải xử lý).

Cuối cùng, framework có thể giúp bạn bắt đầu nhanh, nhưng về lâu dài chúng có thể giới hạn bạn. Khi đã bắt đầu một dự án bằng framework, việc loại bỏ nó có thể khó khăn. Trong tương lai, bạn (hoặc người thuê bạn!) có thể phải quyết định có dùng framework cho một dự án hay không, và nếu có thì dùng framework nào.

### Tổng quan về preprocessor

Preprocessor (hay precompiler) là các ngôn ngữ giúp bạn viết CSS dễ dàng hơn. Chúng có thể giảm việc lặp mã và cung cấp nhiều tính năng tiết kiệm thời gian, tiết kiệm mã, chẳng hạn cho phép bạn viết vòng lặp và điều kiện, cũng như nối nhiều stylesheet.

Preprocessor CSS về cơ bản là phần mở rộng của CSS thuần, cung cấp thêm một số chức năng. Khi chạy preprocessor, nó lấy code của bạn và chuyển thành CSS thuần để bạn import vào dự án.

Preprocessor có những công cụ riêng hữu ích, nhưng nhiều tính năng hữu ích nhất của chúng đã được triển khai trong CSS thuần, nên có thể không đáng phải học một preprocessor trừ khi bạn thực sự cần các tính năng đó. Ví dụ, bạn đã học về custom properties, thứ trước đây chỉ có thể thực hiện bằng preprocessor. [CSS nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting/Using_CSS_nesting) cũng từng là một ưu điểm phổ biến của một số preprocessor, nhưng nay đã có mặt trong CSS thuần và gần đây bắt đầu được nhiều trình duyệt hỗ trợ hơn.

Một số preprocessor tiêu chuẩn đang được sử dụng là [SASS](https://sass-lang.com/), [LESS](https://lesscss.org/) và [Stylus](https://stylus-lang.com/).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [phần tổng quan ngắn về framework CSS](https://medium.com/html-all-the-things/what-is-a-css-framework-f758ef0b1a11).
1. Đọc lướt [phần tổng quan về SASS, LESS và Stylus](https://www.lambdatest.com/blog/css-preprocessors-sass-vs-less-vs-stylus-with-examples/).
1. Đọc về [một số nhược điểm của việc dùng preprocessor CSS](https://adamsilver.io/blog/the-disadvantages-of-css-preprocessors/). Lưu ý rằng từ khi bài viết được viết, CSS đã có nesting và variable thông qua custom properties.

</div>