### Giới thiệu

Hãy nghĩ về ứng dụng nhắn tin yêu thích của bạn - Discord, Telegram, WhatsApp, Facebook Messenger, AOL Instant Messenger, v.v. - và tưởng tượng sẽ như thế nào nếu bạn có thể tự xây dựng ứng dụng này. Dự án này sẽ cho bạn cơ hội để làm điều đó. Bạn sẽ xây dựng một ứng dụng web cho phép người dùng gửi tin nhắn cho nhau.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Hãy nhớ rằng các ứng dụng mà chúng tôi đã đề cập trước đó có toàn bộ nhóm nhà phát triển làm việc trên chúng, vì vậy bạn không cần phải sử dụng quá nhiều tính năng trong ứng dụng của mình. Tuy nhiên, ở mức tối thiểu, ứng dụng của bạn phải bao gồm các chức năng cốt lõi sau:
   1. Ủy quyền
   1. Gửi tin nhắn cho người dùng khác
   1. Tùy chỉnh hồ sơ người dùng
1. Dành thời gian lên kế hoạch cho ứng dụng của bạn. Giao diện người dùng sẽ trông như thế nào? Mô hình dữ liệu sẽ trông như thế nào? Bạn sẽ cần sử dụng những thư viện nào?
1. Bắt đầu xây dựng ứng dụng của bạn bằng cách triển khai chức năng cốt lõi trong backend và giao diện người dùng của bạn.
   1. Bạn có thể đã nhận ra rằng REST API backend không thể xử lý các cập nhật theo thời gian thực. Đó là "phản hồi yêu cầu", vì vậy máy chủ chỉ có thể phản hồi yêu cầu. Nếu người dùng gửi tin nhắn cho người dùng khác, REST API không thể tự động thông báo cho người nhận vì người nhận sẽ không yêu cầu dữ liệu đó. Các phương pháp cập nhật máy chủ-máy khách theo thời gian thực (đặc biệt là khi xử lý phần tách front/backend) chưa được dạy nên bạn không cần phải triển khai bất kỳ phương pháp nào trong ứng dụng này.
1. Triển khai ứng dụng của bạn lên web và giới thiệu ứng dụng đó trong các nội dung gửi bên dưới cũng như trên Discord của chúng tôi. Chúng tôi rất muốn xem những gì bạn đã xây dựng!

#### Tín dụng bổ sung

- Cho phép gửi hình ảnh trong trò chuyện
- Thêm danh sách bạn bè mà người dùng có thể thêm người dùng khác vào và xem khi nào ai đó trực tuyến (thay thế: "Thêm danh sách người dùng để hiển thị những người dùng nào hiện đang trực tuyến"; điều tương tự ngoại trừ có thể ít hơn một hoặc hai bước vì nó không yêu cầu thêm bạn bè)
- Cho phép người dùng tạo và gửi tin nhắn trong cuộc trò chuyện nhóm

</div>
