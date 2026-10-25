### Giới thiệu

Hy vọng rằng cho đến nay bạn vẫn thấy vui khi lập trình bằng Ruby. Tuy nhiên, có lẽ một điều bạn **không** thích thú lắm là phải chạy tập lệnh trên dòng lệnh, tự mình thực hiện từng bước, rồi cuối cùng nhận ra nó không trả về kết quả như mong đợi. Và phần còn *vui* hơn nữa là đây: vò đầu bứt tóc đôi chút, thay đổi vài biến, rồi chạy lại tập lệnh từ đầu, **hy vọng** rằng kết quả là chính xác.

Các bạn, chúng tôi ở đây để nói rằng *CÓ* một cách tốt hơn. Hy vọng là dành cho bài kiểm tra mà bạn chưa học và phải làm trong tình thế bất đắc dĩ. **Biết chắc** mới là phát triển hướng kiểm thử.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Hiểu phát triển hướng kiểm thử (TDD) là gì và vì sao nó quan trọng.
- Hiểu ba giai đoạn của một chu kỳ TDD.

### Phát triển hướng kiểm thử là gì?

[Phát triển hướng kiểm thử (TDD)](https://en.wikipedia.org/wiki/Test-driven_development) là một quy trình và kỹ thuật phát triển phần mềm dựa trên việc lặp lại một chu kỳ phát triển rất ngắn và cụ thể. Trong mỗi chu kỳ, các yêu cầu (tức là điều bạn muốn mã của mình thực hiện) trước hết được chuyển thành các trường hợp kiểm thử cụ thể. Những yêu cầu này có thể là bất cứ thứ gì, từ một tính năng hoàn chỉnh cần kiểm thử đầu-cuối (E2E), chẳng hạn các bài kiểm thử bao quát việc người dùng đăng nhập thành công VÀ không thành công vào trang web của bạn, cho đến một lớp Ruby mới do bạn xây dựng, với lớp này thì kiểm thử đơn vị có thể là đủ. Dù là trường hợp nào, ban đầu bộ kiểm thử cho các yêu cầu này đều thất bại, vì mã thực tế vẫn chưa được viết.

Sau khi mã được viết và vượt qua bộ kiểm thử, bạn có thể chuyển sang tái cấu trúc. Như bạn có thể đoán, việc chạy bộ kiểm thử đối với những thay đổi chúng ta thực hiện cho các tính năng hoặc đơn vị cho phép chúng ta tái cấu trúc *đồng thời* vẫn đảm bảo mọi thứ hoạt động bình thường.

Theo cách nói thông thường, quy trình này thường được gọi là chu kỳ "**đỏ-xanh-tái cấu trúc**". Nói ngắn gọn, chỉ có vậy: các bài kiểm thử tự động **dẫn dắt** việc thiết kế phần mềm. Bạn không cần biết toàn bộ kiến trúc của ứng dụng mới mẻ, tuyệt vời và đột phá ngành của mình sẽ hoạt động ra sao. Ứng dụng chỉ cần được chia nhỏ từng bước một, cho đến khi xác định được các đơn vị nhỏ và bao phủ chúng bằng các bài kiểm thử.

### Vì sao điều này quan trọng?

Ngay cả khi TDD có vẻ là một thực hành tốt theo trực giác, tính hữu ích của nó vẫn được tranh luận gay gắt cho đến tận ngày nay.

Không ai nói rằng các bài kiểm thử không hữu ích. Kiểm thử ở mọi cấp độ đều *cực kỳ quan trọng*. Bài viết của Joël Quenneville về [Kim tự tháp kiểm thử](https://thoughtbot.com/blog/rails-test-types-and-the-testing-pyramid) trong Rails trình bày tổng quan về những loại và cấp độ kiểm thử khác nhau mà các nhà phát triển Rails thường xuyên sử dụng.

Kiểm thử đảm bảo ứng dụng của bạn hoạt động đúng như dự định. Quan trọng hơn, và có lẽ ít rõ ràng hơn, kiểm thử giúp nhà phát triển tự tin khi cần thay đổi mã hiện có. Việc thêm tính năng và dependency mới làm tăng sự thiếu ổn định tổng thể của ứng dụng, và chỉ một bước đi sai cũng có thể gây ra hiệu ứng dây chuyền làm sập ứng dụng ở những vị trí xa xôi, ngoài dự kiến. Kiểm thử xác nhận rằng những gì bạn thêm vào hoạt động cùng với những gì bạn đã có. Theo một nghĩa nào đó, nó tạo ra một mạng lưới an toàn, giúp ứng dụng cởi mở hơn với thay đổi và có không gian để phát triển, tiến hóa một cách ít đau đớn nhất có thể.

Câu hỏi thực sự là liệu các bài kiểm thử có nên *dẫn dắt* quá trình phát triển hay không, hay liệu chúng có nhất thiết phải xuất hiện *trước* hay không. Nhiều nhà phát triển viết kiểm thử sau khi đã viết mã. Chắc chắn danh sách ưu điểm của việc có kiểm thử nói chung dài hơn danh sách nhược điểm, không ai phủ nhận điều đó. Vậy chúng ta có nên kiểm thử trước không?

Như thường lệ, câu trả lời là tùy trường hợp. Dù vậy, sau đây là một số lý do khiến chúng tôi cho rằng việc triển khai TDD có thể quan trọng đối với bạn:

1. **Thực hành**. Nhiều nhà tuyển dụng tìm kiếm những nhà phát triển có kinh nghiệm hoặc hứng thú với kiểm thử. Đúng, bạn có thể viết kiểm thử sau, nhưng liệu bạn có thực sự làm vậy không? Hãy kiểm thử trước để đảm bảo bạn được tiếp xúc với việc này sớm nhất có thể.
1. **Hiệu quả**. Viết kiểm thử trước mã cũng giống như khởi động trước khi tập luyện. Việc đó không bắt buộc, nhưng nếu bỏ qua, buổi tập của bạn có thể bị ảnh hưởng. Viết kiểm thử trước đảm bảo mã được viết sau đó có thiết kế, mục đích và chất lượng rõ ràng hơn.
1. **Năng suất**. Về lâu dài, bạn sẽ tiết kiệm thời gian vì không phải chạy lại toàn bộ ứng dụng chỉ để đảm bảo những phần nhỏ vẫn hoạt động.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc về [9 lợi ích của phát triển hướng kiểm thử](https://www.madetech.com/blog/9-benefits-of-test-driven-development) để có thêm góc nhìn về thực hành này. Lưu ý rằng nhiều điểm trong số đó không chỉ dành riêng cho TDD mà còn áp dụng cho việc kiểm thử nói chung.
1. Để có một góc nhìn toàn diện hơn, hãy xem xét bài viết ["TDD đã chết. Kiểm thử muôn năm"](https://dhh.dk/2014/tdd-is-dead-long-live-testing.html) của DHH, bài viết đã khơi mào những cuộc thảo luận gây tranh cãi về TDD là gì, nó đã trở thành gì và nó nên là gì.
1. Bài viết của James Sinclair giải thích thêm về [lý do/cách thức và giá trị đằng sau TDD](https://jrsinclair.com/articles/2016/one-weird-trick-that-will-change-the-way-you-code-forever-javascript-tdd/), đồng thời đưa ra một số ví dụ rất hay về cách áp dụng nó. Dù các ví dụ được viết bằng JavaScript, trọng tâm nằm ở giá trị đằng sau phương pháp TDD.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu bổ sung.

- Video này cung cấp [các lập luận phản đối bài viết “TDD đã chết. Kiểm thử muôn năm” của DHH](https://www.youtube.com/watch?v=PCEHRFHKZSk) trong phần bài tập.
- Để có một góc nhìn toàn diện hơn về TDD, hãy xem hoặc nghe [loạt bài nói chuyện về ưu và nhược điểm của TDD](https://martinfowler.com/articles/is-tdd-dead/).