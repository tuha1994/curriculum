### Giới thiệu

Cho đến nay, phần lớn bạn mới chỉ làm việc với các chương trình dòng lệnh độc lập. Đã đến lúc bắt đầu mở rộng phạm vi và tương tác với các tệp. Về cơ bản, tệp chỉ là tập hợp các bit và byte mà bạn sẽ cần mở, đọc vào chương trình, chỉnh sửa rồi lưu lại theo một cách nào đó. Dù nhiều tệp (chẳng hạn như hình ảnh) trông giống một mớ dữ liệu khổng lồ khi mở trong trình soạn thảo văn bản, bạn vẫn có thể hình dung mọi tệp như một chuỗi hoặc luồng byte rất dài. Tập lệnh của bạn sẽ đọc chúng từ đầu đến cuối, đồng thời thực hiện mọi thao tác mà bạn chỉ định.

May mắn là Ruby giúp bạn xử lý tệp khá dễ dàng. Ruby có những công cụ cần thiết để đọc các luồng byte dài đó vào chương trình, sau đó cho phép bạn làm việc với chúng bằng những đối tượng quen thuộc. Chỉ cần nhớ rằng tệp là một luồng dài gồm các từ/ký tự/byte được đọc từ đầu đến cuối thì mọi việc sẽ khá trực quan. Nếu muốn thực hiện những thao tác chi tiết hơn, chẳng hạn như ghi vào một vị trí cụ thể trong tệp, trước hết bạn cần xác định mình đang ở vị trí nào, vì có thể bạn đang ở giữa tệp.

Làm việc với tệp sẽ đưa bạn đến với khái niệm tuần tự hóa (serialization), về cơ bản là chuyển đổi dữ liệu sang một định dạng có thể lưu trữ, chẳng hạn như chuỗi. Điều này rất hữu ích, dù bạn đang nghĩ đến việc lưu các đối tượng và lớp của mình vào tệp (ví dụ khi đang ở giữa một ván game), hay truyền chính những loại đối tượng đó đến trình duyệt web (vì cách duy nhất để thông tin truyền qua HTTP là dưới dạng chuỗi).

Một lần nữa, Ruby lại giúp mọi việc trở nên khá dễ dàng. Có một số định dạng tuần tự hóa dữ liệu được chấp nhận rộng rãi, và Ruby cung cấp cho bạn các công cụ cần thiết để làm việc với tất cả chúng. Hai định dạng bạn sẽ thường xuyên gặp là YAML và JSON. YAML thường được dùng để lưu các tệp cấu hình trong Ruby on Rails vì nó rất nhẹ và dễ hiểu. Bạn có thể đọc YAML dễ dàng trong trình soạn thảo văn bản. JSON hiện diện ở khắp nơi trên web và là định dạng được ưu tiên để truyền dữ liệu phức tạp hoặc lồng nhau sâu (chẳng hạn như đối tượng) từ một website đến chương trình của bạn thông qua API (ví dụ khi bạn muốn tích hợp với Google Maps).

Cuối cùng, tệp và tuần tự hóa có nhiều điểm giao thoa với ý tưởng cũng như mục đích của cơ sở dữ liệu — chúng hỗ trợ việc duy trì trạng thái và tính lâu bền cho dữ liệu của bạn. Chúng ta cũng sẽ xem qua một số kết nối cơ sở dữ liệu cơ bản mà Ruby cung cấp.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Hai cách lưu một tệp từ ổ cứng vào chuỗi hoặc mảng trong tập lệnh Ruby.
- Ba việc trở nên khả thi hoặc dễ thực hiện hơn nhiều nhờ tuần tự hóa.
- JSON là gì.
- YAML là gì.
- Chuyển một đối tượng Ruby thành JSON.
- Chuyển JSON thành một đối tượng Ruby.
- Những lớp và phương thức nào có thể được tập lệnh Ruby sử dụng để đọc và ghi vào ổ cứng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc bài viết [I/O trong Ruby của Thoughtbot](https://thoughtbot.com/blog/io-in-ruby). Bạn có thể bỏ qua cả phần 'Kết hợp mọi thứ lại với nhau' và phần 'Làm việc với các API không đồng nhất'.
  1. Xem [phần giới thiệu về tuần tự hóa của icc0612](https://www.youtube.com/watch?v=uS37TujnLRw). Phần này sẽ giải thích khái niệm tuần tự hóa trước khi bạn triển khai nó trong Ruby.
  1. Đọc bài [Chọn định dạng tuần tự hóa phù hợp](https://www.sitepoint.com/choosing-right-serialization-format/) để tìm hiểu thêm về các tùy chọn tuần tự hóa khác nhau mà bạn có thể lựa chọn.
  1. Đọc lướt [bài viết của Alan Skorkin về tuần tự hóa hướng đối tượng trong Ruby](http://www.skorks.com/2010/04/serializing-and-deserializing-objects-with-ruby/). Bài viết được trình bày chưa tốt và một số ví dụ mã đã lỗi thời, nhưng vẫn chứa những thông tin hữu ích. Để xem ở định dạng dễ đọc hơn, bạn có thể sao chép nội dung từ [pastebin này](https://pastebin.com/ZixCHcWk) rồi dán vào [Công cụ xem trước Markdown của Odin](https://www.theodinproject.com/lessons/preview).
  1. Đọc các phần 'Tệp Ruby' và 'Thư mục Ruby' trong [chương Nhập/Xuất của Ruby Tutorial trên Zetcode](http://zetcode.com/lang/rubytutorial/io/) để xem ví dụ về cách thao tác với tệp và thư mục bằng các lớp `File` và `Dir`.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến những nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [Mọi điều bạn cần biết về tuần tự hóa trong Ruby on Rails](https://blog.kiprosh.com/serialization_in_ruby_on_rails_part_one/)
- [Phần về tuần tự hóa của Ruby Monk](https://web.archive.org/web/20160505174806/http://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/45-more-classes/lessons/104-serializing)
- [Ví dụ ngắn về tuần tự hóa](https://web.archive.org/web/20200627063721/http://rubylearning.com/satishtalim/object_serialization.html) từ Ruby Learning
- Cách [đọc](https://www.youtube.com/watch?v=92li0A8d4io) và [ghi](https://www.youtube.com/watch?v=FW9hDsMY1is) tệp của Giraffe Academy