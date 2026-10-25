### Giới thiệu

Một cách hay để làm quen và bắt đầu đóng góp cho một dự án mới là viết các bài kiểm thử cho dự án đó. Đây cũng là cách tốt nhất để làm quen với một cơ sở mã mới, điều mà bạn sẽ phải làm khi bắt đầu đi làm. Việc mã kiểm thử cuối cùng chiếm số dòng gấp đôi mã của dự án thực tế là khá phổ biến!

Có thể lúc này bạn vẫn chưa thật sự tự tin với RSpec (điều đó hoàn toàn bình thường), vì vậy hãy quay lại và viết các bài kiểm thử cho một số mã bạn đã hoàn thành để rèn thêm kỹ năng. Nếu bạn đã viết được một loạt bài kiểm thử tốt, hãy gửi chúng ở bên dưới và chúng tôi sẽ đưa chúng vào phần mô tả dự án gốc để giúp đỡ các học viên trong tương lai!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Sao chép [repo kiểm thử Ruby](https://github.com/TheOdinProject/ruby_testing) của chúng tôi và hoàn thành các bài học trong thư mục spec.
1. Xem [video hữu ích của Sandi Metz](https://www.youtube.com/watch?v=URSWYvyc42M) này để tìm hiểu thêm về việc kiểm thử Ruby.
1. Quay lại [Dự án Mật mã Caesar](/lessons/ruby-caesar-cipher) và viết các bài kiểm thử cho mã của bạn. Bạn không cần quá nửa tá bài kiểm thử để bao quát mọi trường hợp có thể xảy ra. Như mọi khi, hãy sử dụng quy trình làm việc với git mà bạn đã học trong [Ôn lại Oẳn tù tì](https://www.theodinproject.com/lessons/foundations-revisiting-rock-paper-scissors) để làm việc trên một tính năng mới, nhờ đó bạn có thể phát triển mã mới mà không phải lo lắng.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Sử dụng RuboCop RSpec

   Đến thời điểm này, bạn hẳn đã có kinh nghiệm vững vàng với RuboCop và hy vọng bạn đã nhận ra công cụ này có thể hướng dẫn bạn viết Ruby tốt hơn như thế nào. Có thể bạn vẫn nhớ khi chúng ta thảo luận về việc [mở rộng RuboCop](https://www.theodinproject.com/lessons/ruby-linting-and-rubocop#you-are-the-code-dictator) trong bài học Kiểm tra mã và RuboCop. Vậy thì bây giờ, khi quay lại viết các bài kiểm thử cho dự án Mật mã Caesar, chính là lúc thích hợp để bắt đầu sử dụng phần mở rộng [RuboCop RSpec](https://github.com/rubocop/rubocop-rspec) đầy mạnh mẽ này.

   Cũng như RuboCop cơ bản giúp bạn viết Ruby tốt hơn, RuboCop RSpec giúp bạn viết RSpec tốt hơn bằng cách chỉ ra những cạm bẫy thường gặp, lý do nhìn chung nên tránh chúng và cách khắc phục. Từ giờ trở đi, bạn được khuyến khích mạnh mẽ nên luyện tập cài đặt và sử dụng RuboCop RSpec trong mọi dự án có dùng RSpec.

   </div>

1. Viết các bài kiểm thử cho [dự án Tic Tac Toe](/lessons/ruby-tic-tac-toe) của bạn. Trong trường hợp này, việc chỉ nghĩ ra các đầu vào rồi kiểm tra xem phương thức có trả về kết quả đúng hay không sẽ không hoàn toàn đơn giản. Bạn sẽ cần đảm bảo rằng các bài kiểm thử xác định điều kiện thắng hoặc thua được đánh giá chính xác.
   1. Hãy bắt đầu bằng việc viết các bài kiểm thử để đảm bảo người chơi thắng khi họ phải thắng, chẳng hạn như khi bàn cờ có X X X trên hàng đầu tiên, phương thức `#game_over` của bạn (hoặc phương thức tương đương) phải được kích hoạt.
   1. Kiểm thử từng phương thức quan trọng để đảm bảo chúng hoạt động đúng và xử lý các trường hợp biên.
   1. Sử dụng mock/double để cô lập các phương thức và đảm bảo chúng trả về đúng kết quả.

</div>

### Dự án: TDD Connect Four

Hy vọng hầu hết mọi người đã từng chơi [Connect Four](http://en.wikipedia.org/wiki/Connect_Four). Đây là một trò chơi cơ bản, trong đó mỗi người chơi lần lượt thả các quân cờ vào khung. Người chơi thắng nếu đặt được 4 quân cờ của mình liên tiếp theo hàng ngang, hàng dọc hoặc đường chéo.

Luật chơi khá đơn giản và bạn sẽ xây dựng trò chơi này trên dòng lệnh, giống như đã làm với các trò chơi khác. Nếu muốn làm các quân cờ trở nên thú vị hơn, hãy tìm hiểu [các ký hiệu linh tinh trong Unicode](http://en.wikipedia.org/wiki/List_of_Unicode_characters#Miscellaneous_Symbols) trên Wikipedia. Phần Ruby của bài này hiện đã hoàn toàn nằm trong khả năng của bạn, vì vậy việc suy nghĩ về nó sẽ không quá khó khăn.

Điểm khác biệt lớn ở đây là bạn sẽ thực hiện bài này theo phong cách TDD. Vì vậy, hãy xác định điều gì cần xảy ra, viết một bài kiểm thử (đang thất bại) cho điều đó, sau đó viết mã để bài kiểm thử vượt qua, rồi xem có thể tái cấu trúc mã để làm cho nó tốt hơn hay không.

Chỉ viết vừa đủ mã để bài kiểm thử vượt qua. Thông thường, bạn sẽ phải viết hai bài kiểm thử để một phương thức thực hiện được điều gì đó hữu ích. Điều đó hoàn toàn ổn trong bài này. Có thể bạn sẽ cảm thấy hơi quá mức cần thiết, nhưng đó chính là mục đích của bài tập. Suy nghĩ của bạn có lẽ sẽ giống như: "Được rồi, mình cần khiến việc này xảy ra. Mình kiểm thử nó thế nào đây? Được rồi, đã viết bài kiểm thử, vậy mình chuyển nó thành mã Ruby ra sao? Được rồi, đã viết mã Ruby, giờ mình có thể cải thiện nó thế nào?" Bạn sẽ dành khá nhiều thời gian tìm kiếm trên Google và cố gắng tìm hiểu chính xác cách kiểm thử một chức năng cụ thể. Điều đó cũng hoàn toàn ổn... Thực ra bạn đang học RSpec ở đây, chứ không phải Ruby, và sẽ cần một thời gian để quen với nó.

### Bài tập tiếp theo

<div class="lesson-content__panel" markdown="1">

  1. Xây dựng Connect Four! Chỉ cần đảm bảo bạn thực hiện theo TDD.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy bạn có thể xem nó như tài liệu bổ sung.

- Stack Overflow có một [ví dụ về Mock Object trong RSpec](http://stackoverflow.com/questions/3622604/rspec-mock-object-example).
- Tutorials Point có một bài viết về [viết spec trong RSpec](https://www.tutorialspoint.com/rspec/rspec_writing_specs.htm).