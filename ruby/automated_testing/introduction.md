### Giới thiệu

Hy vọng rằng đến giờ bạn vẫn đang thấy việc phát triển bằng Ruby khá thú vị. Tuy nhiên, có lẽ một điều bạn không thích lắm là quá trình kiểm thử ứng dụng theo cách thủ công để đảm bảo ứng dụng vẫn hoạt động sau mỗi thay đổi. Tệ hơn nữa là bạn chạy ứng dụng, đi được khá xa trong quá trình thực thi rồi phát hiện ra một lỗi, sau đó phải làm lại mọi thứ để xác minh rằng mình đã sửa được lỗi đó.

Kiểm thử tự động là giải pháp thay thế cho kiểm thử thủ công. Trước đây trong khóa học này, bạn đã thấy kiểm thử tự động hoạt động khi làm các [bài tập Ruby cơ bản](https://github.com/TheOdinProject/ruby-exercises/tree/main/ruby_basics). Các bài tập đó yêu cầu bạn viết code để làm cho một loạt test có sẵn đang thất bại chuyển sang thành công. Trong vài bài học tiếp theo, bạn sẽ học cách tự viết những test đó và đưa việc viết test vào quy trình làm việc của mình.

### Mục tiêu học tập

- Kiểm thử tự động là gì?
- Test suite là gì?
- Kiểm thử tự động có những lợi ích gì so với kiểm thử thủ công?
- Regression là gì?

#### Test tự động là gì?

Đúng như tên gọi, test tự động tự động hóa quá trình kiểm thử dự án của bạn. Khi viết một tính năng mới, bạn cũng viết các test cho tính năng đó để xác minh tính đúng đắn của nó. Nếu các test chạy qua, bạn có thể tin rằng code hoạt động như dự định. Nếu chúng thất bại, bạn biết rằng đã có sai sót cần được sửa.

Có nhiều loại test tự động khác nhau. Một số test ở cấp độ thấp và chỉ kiểm tra từng class, từng method riêng lẻ. Chúng được gọi là unit test.

Các test khác ở cấp độ cao và kiểm tra toàn bộ hệ thống bằng cách mô phỏng cách một người dùng thật tương tác với hệ thống. Chúng được gọi là end-to-end test. Tập hợp tất cả các test của bạn được gọi là test suite.

### Test tự động giải quyết những vấn đề gì?

#### Tiết kiệm thời gian

Lợi ích rõ ràng nhất của kiểm thử tự động so với kiểm thử thủ công là lượng thời gian mà nó có thể giúp bạn tiết kiệm.

Kiểm thử thủ công đòi hỏi rất nhiều thao tác lặp lại. Mỗi khi thêm một tính năng mới, bạn phải kiểm tra tính năng đó cũng như các tính năng hiện có để đảm bảo chúng không bị hỏng do những thay đổi bạn đã thực hiện.

Tùy thuộc vào quy mô dự án, việc kiểm tra thủ công toàn diện mọi thứ có thể mất từ vài phút đến vài tuần. Khi dự án tăng về quy mô và độ phức tạp, khoảng thời gian này cũng chỉ tăng lên.

Ngược lại, test tự động chỉ mất một phần nhỏ thời gian để chạy, thường chỉ vài giây. Bạn cũng chỉ cần viết một test tự động một lần; sau đó, test này sẽ kiểm tra hành vi đó mỗi khi test suite được chạy, thay vì phải lặp đi lặp lại việc kiểm tra thủ công cùng một thứ mỗi khi có thay đổi.

#### Mang lại sự tự tin

Các dự án phần mềm hiếm khi thực sự hoàn thành và chúng thường xuyên thay đổi. Những tính năng mới cần được thêm vào, còn các tính năng hiện có cần được chỉnh sửa. Mọi thay đổi mới đều có nguy cơ vô tình làm hỏng một thứ trước đó vẫn hoạt động. Đây được gọi là việc tạo ra một regression.

Thời gian cần thiết để kiểm tra thủ công toàn bộ chức năng trong một dự án có quy mô vừa phải khiến việc rút ngắn quy trình gần như không thể tránh khỏi, và các regression bị bỏ sót vì cuối cùng chỉ những khu vực xung quanh nơi thay đổi mới được kiểm tra. Điều này đẩy gánh nặng kiểm thử mọi thứ còn lại sang người dùng cuối của dự án.

Test tự động là một mạng lưới an toàn giúp ngăn việc tạo ra regression. Chúng cung cấp một vòng phản hồi tức thì, cho bạn biết khi mình mắc lỗi từ rất lâu trước khi những thay đổi đó đến tay người dùng.

Sự yên tâm và tự tin mà mạng lưới an toàn gồm các test tự động mang lại là điều không thể đánh giá quá cao. Nó khuyến khích bạn, với tư cách là developer, thực hiện những thay đổi và cải tiến mà có thể bạn đã không dám làm vì sợ phá hỏng một chức năng quan trọng nào đó. Đổi lại, điều này giúp bạn nâng cao chất lượng và độ tin cậy tổng thể của dự án, phát hành tính năng mới nhanh hơn và an toàn hơn, đồng thời giúp việc bảo trì dự án trở nên dễ dàng hơn về lâu dài.

#### Kết luận

Cũng như hầu hết mọi thứ, kiểm thử tự động có những đánh đổi. Bạn phải bỏ ra chi phí thời gian ban đầu để viết test, và nó tạo thêm code cần được bảo trì.

Nhưng lợi ích mà một dự án được kiểm thử tốt mang lại lớn hơn rất nhiều so với những chi phí này, đồng thời còn tích lũy trong suốt vòng đời của dự án.

Để có thêm động lực, test tự động đã trở thành một tiêu chuẩn của ngành. Khả năng viết chúng sẽ là yêu cầu đối với gần như mọi công việc mà bạn ứng tuyển. Vì vậy, học cách viết test cũng rất quan trọng để cải thiện cơ hội được tuyển dụng với tư cách developer.

Giờ bạn đã biết lý do cần kiểm thử, vài bài học tiếp theo sẽ tập trung vào cách thực hiện. Bạn sẽ học cách viết test bằng một testing framework có tên là RSpec, cũng như cách dùng RSpec để viết cả unit test lẫn end-to-end (E2E) test. Cuối cùng, chúng ta sẽ tìm hiểu về phát triển hướng kiểm thử, một quy trình trong đó bạn viết test trước và dùng chúng để định hướng thiết kế code.