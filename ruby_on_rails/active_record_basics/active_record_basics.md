### Giới thiệu

Có lẽ bạn đang ở đây để học phát triển web (nếu không thì... có thể bạn đã vào nhầm chỗ...). Dù mục tiêu của bạn là tự xây dựng một trang web hay bắt đầu sự nghiệp lập trình viên, kỹ năng quan trọng nhất cần rút ra từ tất cả những điều này là khả năng tư duy logic và chia nhỏ một vấn đề thành các phần cấu thành. Sau đó, bạn có thể giải quyết từng phần một. Đó chính là tinh thần cốt lõi của kỹ thuật.

Có lẽ cách quan trọng nhất mà tư duy logic được vận dụng khi xây dựng một trang web là thiết lập mô hình dữ liệu cho đúng. Dữ liệu là nền tảng của hầu hết ứng dụng web lớn, từ một blog cơ bản cho đến mạng lưới dữ liệu phức tạp khổng lồ của Facebook. Một mô hình dữ liệu khó hiểu hoặc quá phức tạp có thể khiến bạn không thể mở rộng ứng dụng, đồng thời làm cuộc sống của một lập trình viên trở nên vô cùng khổ sở. Nếu bạn đang dùng sai công cụ, một việc đơn giản như hiển thị tất cả bình luận mà một người dùng đã viết trên các bài đăng web của người dùng khác cũng có thể tiêu tốn quá nhiều công sức suy nghĩ và chu kỳ CPU để hoàn thành.

Nếu dữ liệu là phần quan trọng nhất của một ứng dụng web, thì cách Rails xử lý dữ liệu hẳn rất đáng để bạn quan tâm. May mắn là đây cũng là một trong những lý do lớn nhất giúp Rails hoạt động tốt hơn nhiều so với các lựa chọn từng có cách đây vài năm. Active Record là giao diện mà Rails cung cấp giữa cơ sở dữ liệu và ứng dụng của bạn. Nó cho phép bạn cấu trúc các mô hình dữ liệu cho người dùng, bài đăng blog, bình luận, người theo dõi, v.v. theo cách logic và gần như bằng ngôn ngữ đời thường. Nếu đôi lúc nó có vẻ phức tạp (và đúng là sẽ có lúc như vậy), hãy thử tưởng tượng cuộc sống trước khi có Active Record.

Nắm vững Active Record sẽ khiến phần còn lại của Rails có vẻ đơn giản hơn nhiều. Hãy nhớ lại từ vài bài học trước rằng Model trong MVC là phần đảm nhận hầu hết công việc nặng. Trong bài học này, chúng ta sẽ tìm hiểu toàn bộ kiến thức cơ bản khi làm việc với các model. Như thường lệ, phần giải thích này chỉ nhằm cung cấp một cái nhìn tổng quan, còn tài liệu đọc thêm sẽ giúp bạn đi sâu hơn. Những chủ đề nâng cao sẽ được trình bày trong một số bài học sắp tới.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- ORM là gì.
- Vì sao và bằng cách nào Active Record có thể hữu ích hơn việc chỉ dùng SQL.
- Hai bước cần thiết để tạo một hàng mới trong bảng cơ sở dữ liệu bằng Active Record.
- “Generators” trong Rails.

### ORM là gì?

Active Record rốt cuộc là gì? Hãy nhớ rằng Rails thực tế gồm bảy gem Ruby phối hợp hài hòa với nhau. Nói một cách không mấy hoa mỹ, Active Record là gem lo liệu toàn bộ phần cơ sở dữ liệu. Nó được gọi là một “ORM”.

ORM là viết tắt của Object-Relational-Mapping. Về cơ bản, điều đó có nghĩa là Active Record lấy dữ liệu được lưu trong bảng cơ sở dữ liệu dưới dạng hàng và cột — loại dữ liệu vốn cần được sửa đổi hoặc truy xuất bằng cách viết các câu lệnh SQL (nếu bạn dùng cơ sở dữ liệu SQL) — rồi cho phép bạn tương tác với dữ liệu đó như thể nó là một đối tượng Ruby thông thường.

Nếu muốn lấy một mảng chứa danh sách tất cả người dùng, thay vì viết mã để khởi tạo kết nối đến cơ sở dữ liệu, thực hiện một truy vấn kiểu `SELECT * FROM users`, rồi chuyển các kết quả đó thành một mảng, bạn chỉ cần gõ `User.all`, và Active Record sẽ cung cấp cho chúng ta một mảng chứa đầy các đối tượng User để tùy ý sử dụng. Thật ấn tượng!

Ấn tượng hơn nữa là loại cơ sở dữ liệu bạn đang dùng thực ra không quá quan trọng (miễn là bạn đã thiết lập tệp `config/database.yml` đúng cách). Active Record sẽ giúp bạn san bằng mọi khác biệt giữa các cơ sở dữ liệu đó, để bạn không phải bận tâm. Bạn tập trung viết mã cho ứng dụng, còn Active Record lo những chi tiết vụn vặt trong việc kết nối với cơ sở dữ liệu. Điều đó cũng có nghĩa là nếu chuyển từ cơ sở dữ liệu này sang cơ sở dữ liệu khác, bạn không thực sự phải thay đổi phần mã ứng dụng lớn nào, chỉ cần chỉnh một vài tệp cấu hình. Nghe hợp lý, đúng không?

### Các model trong Rails

Tuy nhiên, như vậy là chúng ta đã đi trước một bước, bởi trước hết cần hiểu mối quan hệ giữa Rails và cơ sở dữ liệu. Thực ra điều này khá đơn giản — bạn muốn lưu thông tin về người dùng, nên tạo một bảng cơ sở dữ liệu tên là `users`. Bạn muốn truy cập dữ liệu đó từ ứng dụng, nên tạo một model tên là `User`; model này thực chất chỉ là một tệp Ruby kế thừa từ Active Record, nhờ đó có thể dùng những phương thức tiện lợi mà chúng ta đã nhắc đến trước đó như `all`, `find` và `create`. Mỗi bảng tương ứng với một model kế thừa từ Active Record.

#### 30 giây về cách làm việc với model

Nói ngắn gọn, Active Record cho phép bạn tạo một đối tượng Ruby đại diện cho một hàng trong một trong các bảng cơ sở dữ liệu, chẳng hạn như `User`. Việc tạo một User mới gồm hai bước: trước tiên, bạn cần gọi `User.new` và có thể truyền vào một hash chứa các thuộc tính của nó, chẳng hạn như

```bash
u = User.new(name: "Sven", email: "sven@theodinproject.com")
```

Nếu không truyền hash, bạn sẽ phải tự thêm các thuộc tính bằng cách gán chúng như với bất kỳ đối tượng Ruby nào khác: `u.name = "Sven"`. Bước thứ hai là thực sự lưu instance của model đó vào cơ sở dữ liệu. Cho đến lúc này, nó chỉ nằm trong bộ nhớ và sẽ biến mất nếu bạn không làm gì thêm. Để lưu, hãy gọi `u.save`. Bạn có thể thực hiện cả hai bước cùng lúc bằng phương thức `#create`:

```bash
u = User.create(name: "Sven", email: "sven@theodinproject.com")
```

Cách này giúp bạn tiết kiệm thời gian, nhưng như bạn sẽ thấy sau này, đôi khi việc tách riêng hai bước trong ứng dụng sẽ hữu ích hơn.

### Bài tập

Phần trên thực sự chỉ là một lời giới thiệu ngắn về những gì Active Record có thể làm. Trong tài liệu đọc dưới đây, bạn sẽ học cách tương tác cụ thể với Active Record trong các model và đi sâu hơn vào những chủ đề khác đã đề cập.

<div class="lesson-content__panel" markdown="1">

1. Đọc phần [Kiến thức cơ bản về Active Record](http://guides.rubyonrails.org/active_record_basics.html) trong Rails Guides.
    - Chúng ta sẽ tìm hiểu sâu hơn về Migrations và Validations trong các bài học tiếp theo, cũng như trong bài học về Callbacks ở phần sau của khóa học.
    - Các tệp model trong Rails nằm trong thư mục `app/models` và chỉ là những tệp .rb thông thường. Điểm mấu chốt là tên tệp và tên lớp được đặt theo tên bảng trong cơ sở dữ liệu của bạn (nhưng ở dạng số ít), đồng thời lớp đó kế thừa từ ApplicationRecord để có được các siêu năng lực của nó.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [Schneems nói về các model được hỗ trợ bởi cơ sở dữ liệu với Active Record](https://www.youtube.com/watch?v=EU98yHB-_7A).