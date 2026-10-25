### Giới thiệu

Bạn đã đi được một chặng đường dài. Giờ là lúc chứng minh mình đã học được bao nhiêu.

Cờ vua là một trò chơi kinh điển, thoạt nhìn có vẻ rất phức tạp nhưng có thể được chia thành các bước logic, khiến nó trở thành một dự án tổng kết Ruby tuyệt vời. Nếu chưa từng chơi, trước hết hãy tìm hiểu về luật chơi (xem [Trang Wikipedia về cờ vua](http://en.wikipedia.org/wiki/Chess)).

Đặc tả bài toán cho dự án Ruby cuối cùng này được cố ý viết khá sơ lược -- bạn phải tự mình bắt tay giải quyết với rất ít thông tin hoặc cấu trúc có sẵn. Đây là cách luyện tập tốt cho những thử thách lập trình trong thực tế. Bạn đã có mọi công cụ cần thiết. Bạn cũng đã thực hiện phần lớn quá trình suy nghĩ khó khăn trong [dự án Hành trình của quân mã](https://www.theodinproject.com/lessons/ruby-knights-travails).

Điểm khác biệt chính là bài toán này có phạm vi rộng nhất trong tất cả những gì bạn từng làm. Điều quan trọng ở đây là suy nghĩ thấu đáo về bài toán theo cách logic từ trước và duy trì một quy trình làm việc có kỷ luật. Mọi việc sẽ dễ dàng hơn nhiều nếu bạn có thể giữ cho mình ngăn nắp và chia bài toán thành các thành phần để xử lý lần lượt.

Đây là một dự án rất đáng đưa vào danh mục sản phẩm cá nhân về sau, vì nó cho thấy bạn có thể đảm nhận một bài toán gồm nhiều thành phần khác nhau.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. **Xây dựng một trò chơi cờ vua chạy trên dòng lệnh, trong đó hai người chơi có thể đấu với nhau.**
  1. Trò chơi phải được ràng buộc đúng cách -- phải ngăn người chơi thực hiện các nước đi không hợp lệ và thông báo đúng lúc khi bị chiếu hoặc chiếu hết.
  1. Cho phép lưu bàn cờ bất kỳ lúc nào (nhớ cách tuần tự hóa chứ?)
  1. Viết các bài kiểm thử cho những phần quan trọng. Bạn không nhất thiết phải phát triển theo TDD (trừ khi muốn), nhưng hãy nhớ dùng các bài kiểm thử RSpec cho bất cứ điều gì mà bạn thấy mình phải liên tục nhập vào dòng lệnh.

     <div class="lesson-note lesson-note--tip" markdown="1">

     #### Công cụ chỉnh sửa bàn cờ trực tuyến hỗ trợ kiểm thử

     Nhiều dịch vụ cờ vua trực tuyến cung cấp các công cụ giúp bạn dễ dàng thiết lập và trực quan hóa các thế cờ. Chúng cũng cho phép nhập và xuất thế cờ thuận tiện bằng [FEN](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation) cùng nhiều cách khác. Ví dụ, Lichess cung cấp [Trình chỉnh sửa bàn cờ](https://lichess.org/editor) và [Bàn phân tích](https://lichess.org/analysis). Những công cụ này có thể rất hữu ích khi bạn chuẩn bị một số trường hợp kiểm thử ở các giai đoạn nâng cao của dự án, đặc biệt nếu bạn quyết định thiết kế dự án để tận dụng FEN.

     </div>  

  1. Hãy cố gắng giữ cho các lớp của bạn có tính mô-đun và rõ ràng, đồng thời mỗi phương thức chỉ làm một việc. Đây là chương trình lớn nhất bạn từng viết, vì vậy khi bắt đầu gặp lỗi, bạn chắc chắn sẽ thấy lợi ích của việc tổ chức tốt (và kiểm thử).
  1. Chưa quen với cờ vua? Hãy xem thêm một số tài nguyên để giúp bạn làm quen.
  1. Hãy vui vẻ! Xem các [ký tự cờ vua Unicode](http://en.wikipedia.org/wiki/Chess_symbols_in_Unicode) trên Wikipedia để tạo thêm chút thú vị cho bàn cờ của bạn.

#### Điểm cộng

1. Xây dựng một người chơi máy AI rất cơ bản (chẳng hạn có thể thực hiện một nước đi hợp lệ ngẫu nhiên).

</div>

### Tài nguyên bổ sung

Phần này có các liên kết hữu ích đến những nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu tham khảo thêm.

- [Luật cờ vua minh họa](http://www.chessvariants.org/d.chess/chess.html)
- [Ký hiệu cờ vua cho người và máy tính](https://en.wikipedia.org/wiki/Chess_notation)
- Nếu cảm thấy bế tắc, bạn nên ghé qua Discord của cộng đồng!
- Sau khi hoàn thành dự án, hãy xem [bài thảo luận về việc viết cờ vua bằng Ruby](http://rubyquiz.com/quiz35.html) từ RubyQuiz. **Cảnh báo**: tài liệu này chứa bản thiết kế hướng dẫn cách hoàn thành dự án. Điều đó có nghĩa là nếu quyết định xem qua, bạn sẽ không còn cơ hội tự mình đào sâu vào bài toán này.