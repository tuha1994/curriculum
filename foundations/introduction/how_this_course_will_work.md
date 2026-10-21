### Giới thiệu

The Odin Project là một cộng đồng mã nguồn mở chuyên cung cấp những nguồn thông tin tốt nhất để đưa bạn từ con số không đến trở thành một nhà phát triển full-stack.

Trong phần này, chúng ta sẽ tìm hiểu web hoạt động như thế nào và bắt đầu suy nghĩ về những kiến thức cơ bản của máy tính và lập trình web.

Mỗi phần và bài học sau đây đều bao quát những kiến thức nền tảng thiết yếu.  Ngay cả khi bạn không có ý định trở thành nhà phát triển web, tài liệu này vẫn giúp bạn hiểu hữu ích về những thành phần vận hành đằng sau việc tạo và cung cấp nội dung trên web.

Trước hết, chúng ta sẽ làm quen với internet và chính chiếc máy tính của bạn.  Tiếp theo, chúng ta sẽ thiết lập môi trường phát triển và tìm hiểu về Git và GitHub. Sau đó, chúng ta sẽ đi qua những kiến thức cơ bản về các công nghệ front-end như HTML, CSS và JavaScript, trước khi bước sang back end với phần giới thiệu ngắn về các khái niệm cơ bản của công nghệ back-end.

Đến cuối phần này, bạn không chỉ hiểu web hoạt động như thế nào mà còn có thể nhận diện và phân biệt tất cả công nghệ mình sẽ sử dụng để xây dựng các ứng dụng web riêng. Bạn sẽ có thể tạo một webpage cơ bản, định kiểu cho nó và thêm một vài tương tác nhỏ, đồng thời làm việc thoải mái từ dòng lệnh.

Phần này cố ý bao quát một phạm vi chủ đề rất rộng. Sẽ thật vô lý nếu lao ngay vào lập trình phía máy chủ mà chưa có bối cảnh về nó, về lý do nó hữu ích và vì sao bạn nên học nó!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Hiểu chương trình học của The Odin Project hoạt động như thế nào.
- Hiểu khóa học Foundations hoạt động như thế nào.

### Cách chương trình hoạt động

Chương trình này tập hợp những nội dung tốt nhất trên internet để dạy một chủ đề cụ thể. Trong mỗi bài học, chúng tôi sẽ giới thiệu chủ đề và cố gắng cung cấp một số ngữ cảnh hữu ích trước khi dẫn bạn đến các tài nguyên bên ngoài do người khác tạo ra.

Một số bài học sẽ có bài tập để bạn hoàn thành. Ngoài ra, chúng tôi cung cấp nhiều dự án trong suốt chương trình để giúp bạn hiểu sâu hơn bằng cách thực sự xây dựng mọi thứ.

Đừng nghĩ về The Odin Project, hay việc lập trình, như một lớp học ở trường. Đây không phải là tài liệu bạn học một lượt để làm bài kiểm tra rồi đỗ hoặc trượt. Bạn có thể hình dung nó như một quả cầu tuyết. Chính bạn là quả cầu tuyết đó. Bạn lăn xuống một ngọn đồi phủ đầy tuyết; càng lăn xa, càng có nhiều tuyết bám vào bạn. Tất nhiên tuyết cũng sẽ rơi khỏi bạn và bạn sẽ thường xuyên quên mọi thứ, nhưng đó chỉ là một phần của quá trình. Đừng lo nếu đến một dự án mà bạn cảm thấy mình chẳng nhớ hay ghi nhớ được gì. Điều đó hoàn toàn tự nhiên và xảy ra với tất cả mọi người. **Các dự án không phải là bài kiểm tra những gì bạn đã ghi nhớ cho đến lúc này.** Chúng là những điểm thực hành, nơi kiến thức sẽ quay trở lại khi bạn bắt đầu giải quyết từng vấn đề một, dựa vào Google và Cộng đồng Odin để được hỗ trợ.

### Lưu ý về công cụ

Chúng tôi thường nhận được câu hỏi vì sao khóa học này không bao quát một ngôn ngữ hoặc công cụ nào đó.

Sự thật là sau khi hoàn thành chương trình này, bạn sẽ **không cần** một khóa học như thế này hay bất kỳ dạng "tutorial" nào nữa. Mục tiêu của The Odin Project không phải là dạy bạn những kỹ năng độc lập và đây cũng không phải tutorial. Ý tưởng chính là dạy bạn *lý do* và thay đổi cách bạn suy nghĩ.

Một trong những phần khó nhất của khóa học là hình thành tư duy đúng. Một thách thức còn lớn hơn là học những ý tưởng cốt lõi của lập trình. Lập trình không chỉ là viết mã; đó còn là giải quyết vấn đề, đặt đúng câu hỏi và nghiên cứu đủ nhiều. Khi đạt đến trạng thái thoải mái với các kỹ năng này, bạn cũng sẽ thấy tự tin hơn khi mở rộng bộ kỹ năng của mình. Khi đã có một *nền tảng* vững chắc, việc phát triển thêm sẽ đơn giản hơn. Đây chính xác là điều The Odin Project hướng tới: phát triển những *kỹ năng* này.

Sau khi hoàn thành khóa học, bạn sẽ có thể đi thẳng vào tài liệu của bất cứ thứ gì mình đang cố học.
Đọc tài liệu là cách tốt nhất để học bất kỳ công cụ nào. Nhưng để làm chủ việc đó cũng cần kỹ năng. Nhiều người cảm thấy e ngại trước các thuật ngữ kỹ thuật, điều này hoàn toàn dễ hiểu. Những người viết tài liệu thường giả định người đọc đã có nền tảng lập trình. Đó chính là lý do chúng tôi cung cấp cho bạn nhiều tài liệu để tự đọc.

Những kỹ năng bạn có được khi hoàn thành The Odin Project sẽ là nền tảng để bạn tiếp tục xây dựng trong nhiều năm và nhiều thập kỷ tới. Nếu học xong mà bạn vẫn nghĩ mình cần một khóa học tương tự để học một thứ như Python, thì hoặc là bạn chưa tin vào bản thân, hoặc là bạn chưa tiếp thu những ý tưởng quan trọng được trình bày trong khóa học này.

Những kỹ năng học được ở đây có thể chuyển giao sang nhiều bối cảnh khác nhau. Một khi hiểu được *ý tưởng* và biết rằng việc nào đó là *có thể thực hiện*, việc tìm ra cách thực hiện nó sẽ dễ dàng hơn đáng kể và thường chỉ cách một lần tìm kiếm trên Google.

### Lưu ý về ngôn ngữ tự nhiên

The Odin Project thu hút mọi người từ khắp nơi trên thế giới, những người mong muốn học cách trở thành nhà phát triển. Xin lưu ý rằng chương trình này được viết bằng tiếng Anh và do những người nói tiếng Anh duy trì; họ không thể và cũng không được kỳ vọng sẽ dịch chương trình cho bạn. Khi phát triển thành một lập trình viên, bạn sẽ thấy thế giới mình bước vào có nền tảng vững chắc là tiếng Anh. Điều đó có nghĩa là cú pháp của ngôn ngữ lập trình, tài liệu hướng dẫn cách sử dụng nó và phần lớn những người trong cộng đồng đều mong đợi giao tiếp với bạn bằng tiếng Anh.

Nếu bạn không nói tiếng Anh, hoặc tiếng Anh không phải ngôn ngữ chính của bạn, sự thật này không nhằm làm bạn nản chí mà để chuẩn bị cho bạn.

Là một phần của sự chuẩn bị đó, chúng tôi *rất* khuyến nghị bạn dành thêm thời gian cho những chủ đề và thuật ngữ chưa hiểu ngay. Chúng tôi cũng khuyến khích bạn tìm thêm các tài nguyên giải thích những chủ đề này bằng ngôn ngữ mẹ đẻ để hiểu chúng đầy đủ hơn.

Ngoài ra, bạn có thể cân nhắc sử dụng từ điển dịch bằng ngôn ngữ của mình song song với chương trình để dễ tra cứu khi học. Chúng tôi không có đề xuất cụ thể vì trên thế giới có rất nhiều ngôn ngữ; việc tìm một công cụ phù hợp có thể là bước đầu tốt để học cách tìm những công cụ hữu ích trên mạng bằng việc tìm kiếm. Đây là kỹ năng bạn sẽ sử dụng và cải thiện trong quá trình học để trở thành nhà phát triển.

### Điều gì sẽ đến tiếp theo

Sau khi hoàn thành khóa học này, bạn sẽ thấy thoải mái với những viên gạch nền tảng của lập trình web nhưng cũng nóng lòng đào sâu hơn.  Dù khóa học dành khá nhiều thời gian cho từng chủ đề lớn, đây thực sự mới chỉ là nếm thử những gì sắp tới (và tất cả những điều thú vị bạn có thể làm với chúng).

Bài học cuối cùng của khóa học sẽ cho bạn cơ hội lựa chọn giữa lộ trình Full Stack JavaScript và Full Stack Rails; cả hai đều được thiết kế để phát triển thêm nền tảng đã hình thành trong khóa học này. Mỗi lộ trình sẽ tập trung biến những viên gạch thô ban đầu thành một bộ kỹ năng có tính ứng dụng cao.

The Odin Project được duy trì bởi các chuyên gia. Chúng tôi đã chọn một số tài nguyên tốt nhất hiện có và biên soạn hướng dẫn về cách học chúng. Nếu không có tài nguyên tốt, chúng tôi tự viết. Vì vậy, hãy nhớ rằng **mọi thứ** trong chương trình đều được đưa vào có chủ đích và đều thiết yếu để bạn trở thành một lập trình viên thành công.

Khi tiến lên trong chương trình, mỗi phần đều được xây dựng trên mọi thứ đã học trước đó, nên bỏ qua nội dung sẽ tạo ra những khoảng trống kiến thức bắt đầu ảnh hưởng đến khả năng giải quyết vấn đề và hiểu nhiệm vụ trước mắt của bạn.

Tài nguyên bổ sung là thứ duy nhất được xem là tùy chọn, trừ khi có nói rõ khác đi. Những tài nguyên này dành cho lúc bạn cảm thấy cần hoặc muốn đào sâu hơn vào một chủ đề để hiểu rõ hơn.

**ĐỪNG BỎ QUA BẤT CỨ ĐIỀU GÌ!**

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Tìm hiểu thêm về The Odin Project bằng cách đọc [trang giới thiệu](https://www.theodinproject.com/about).
1. Xem phần [các câu hỏi thường gặp](https://www.theodinproject.com/faq).

</div>