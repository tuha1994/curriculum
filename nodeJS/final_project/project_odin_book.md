### Giới thiệu

Bạn đã đi được một chặng đường dài, xin chúc mừng! Đến thời điểm này, bạn hẳn đã cảm thấy thoải mái khi xây dựng các ứng dụng Express mới và sử dụng PostgreSQL để mô hình hóa cũng như lưu trữ dữ liệu. Dự án này sẽ yêu cầu bạn vận dụng toàn bộ kiến thức đó. Dự án sẽ không dễ, nhưng chắc chắn nằm trong khả năng của bạn và sẽ là một sản phẩm *tuyệt vời* cho portfolio.

Bạn sẽ xây dựng một bản sao của một trang mạng xã hội, chẳng hạn như Facebook, X, Myspace hoặc Threads. Cũng như các bài học trước, mức độ đầu tư vào phần styling và front-end tùy thuộc vào bạn. Phần quan trọng là dữ liệu và back-end. Bạn sẽ ghép lại các tính năng cốt lõi của nền tảng như người dùng, profile, bài đăng, theo dõi và “thích”.

Bạn cũng sẽ triển khai một hình thức xác thực nào đó. Lý tưởng nhất là dùng passport.js để hỗ trợ xác thực thông qua trang mạng xã hội mà bạn đang sao chép, nhưng một số trang (chẳng hạn Facebook) gần đây đã khiến quy trình này trở nên bất khả thi. Nếu trang bạn chọn rơi vào trường hợp đó, bạn có thể dùng passport.js để hỗ trợ xác thực bằng username và password với `passport-local`, hoặc qua Github với `passport-github2`.

Có thể sẽ có những tính năng bạn chưa từng tiếp xúc, chẳng hạn như chat, cập nhật theo thời gian thực và thông báo. Bạn không phải chịu trách nhiệm về những tính năng đó, trừ khi hiện tại bạn cảm thấy rất tự tin vào kỹ năng của mình (ví dụ: [socket.io](https://socket.io/) cho phép bạn dùng websocket để giao tiếp theo thời gian thực).

### Bài tập

<div class="lesson-content__panel" markdown="1">

Hãy xây dựng một trang mạng xã hội! Trong dự án này, bạn sẽ xây dựng phần lớn chức năng cốt lõi dành cho người dùng của trang bạn chọn. Bạn không cần lo lắng về một số phần front-end cầu kỳ hơn nếu không muốn, vì bạn không cần chúng để tạo ra trải nghiệm người dùng tốt.

Dự án này cho bạn cơ hội biến một tập hợp yêu cầu tương đối cấp cao thành một website hoạt động được. Bạn sẽ cần tự nghiên cứu và đọc tài liệu của một số module sẽ sử dụng trong dự án.

#### Bắt đầu

1. Hãy suy nghĩ về những việc cần làm. Viết kế hoạch trước ra giấy hoặc bảng trắng sẽ rất hữu ích! Vài giờ suy nghĩ lúc này có thể giúp bạn tiết kiệm nhiều ngày code. Hãy cố gắng liệt kê TẤT CẢ. Một phần quan trọng của việc lập kế hoạch là **scope**. Rõ ràng bạn không thể xây dựng toàn bộ website (mà có lẽ đã cần cả một đội ngũ kỹ sư làm việc trong nhiều năm), nên bạn sẽ phải xác định chức năng cốt lõi của trang và những phần “có thì tốt”. **Hãy đảm bảo hoàn thành chức năng cốt lõi TRƯỚC KHI làm phần còn lại.** Nếu cố làm mọi thứ cùng lúc, bạn sẽ bị lạc hướng và nản lòng. Hãy tin chúng tôi. Mọi thứ đều mất nhiều thời gian hơn bạn dự kiến.
1. Suy nghĩ về kiến trúc dữ liệu cần thiết để hệ thống hoạt động. Có rất nhiều model và quan hệ giữa chúng phức tạp hơn bất kỳ điều gì bạn từng làm trước đây. Hãy dành thời gian lập kế hoạch tiếp cận trước khi bắt tay vào làm.
1. Làm lần lượt theo danh sách bên dưới! Mỗi bước sẽ đem đến một thử thách mới, nhưng bạn đã có đủ công cụ.
1. Bạn có thể tạo dữ liệu giả như user và post bằng module [Faker](https://github.com/faker-js/faker) từ npm. Để làm vậy, hãy tạo một file JavaScript mới có tên `seeds.js`, import các Prisma model của bạn và dùng module faker để tạo rồi lưu một loạt user mới.

#### Yêu cầu

Các yêu cầu sau là danh sách rất tổng quát về những tính năng app nên có. Vì dự án này có tính mở, có thể một số yêu cầu không phù hợp với trang bạn chọn, đồng thời trang của bạn có thể có những tính năng cốt lõi chưa được đề cập ở đây.

1. Người dùng phải đăng nhập để xem bất kỳ nội dung nào ngoài trang đăng nhập.
1. Người dùng có thể đăng nhập bằng phương thức xác thực bạn chọn.
1. Người dùng có thể gửi yêu cầu theo dõi đến người dùng khác.
1. Người dùng có thể tạo bài đăng (ban đầu chỉ cần văn bản).
1. Người dùng có thể thích bài đăng.
1. Người dùng có thể bình luận trên bài đăng.
1. Bài đăng luôn phải hiển thị nội dung bài đăng, tác giả, bình luận và lượt thích.
1. Cần có một trang index cho bài đăng, hiển thị tất cả bài đăng gần đây của người dùng hiện tại và những người mà họ đang theo dõi.
1. Người dùng có thể tạo profile kèm ảnh profile. Tùy cách bạn xử lý xác thực, chẳng hạn qua `passport-github2`, bạn có thể dùng ảnh profile hiện có trong tài khoản của họ. Nếu không, bạn có thể dùng [Gravatar](https://www.gravatar.com/) để tạo ảnh.
1. Trang profile của người dùng phải chứa thông tin profile, ảnh profile và các bài đăng của họ.
1. Cần có một trang index cho user, hiển thị tất cả user và các nút gửi yêu cầu theo dõi đến những user mà người dùng hiện tại chưa theo dõi hoặc chưa có yêu cầu đang chờ.
1. Deploy app lên một nhà cung cấp hosting mà bạn chọn!

#### Điểm cộng

1. Cho phép bài đăng có cả ảnh (hoặc chỉ bằng URL, hoặc bằng cách tải ảnh lên). Trong [dự án File Uploader](https://www.theodinproject.com/lessons/nodejs-file-uploader), có thể bạn còn nhớ [Cloudinary](https://cloudinary.com/documentation/node_integration) hoặc [Supabase storage](https://supabase.com/docs/guides/storage) là những lựa chọn tốt để lưu trữ ảnh do người dùng tải lên. Sau đó, bạn có thể lưu các URL mà chúng cung cấp vào cơ sở dữ liệu thay vì dữ liệu nhị phân thô của ảnh.
1. Cho phép người dùng cập nhật ảnh profile.
1. Tạo chức năng đăng nhập khách cho phép khách truy cập bỏ qua màn hình đăng nhập mà không cần tạo tài khoản hoặc cung cấp thông tin xác thực. Tính năng này đặc biệt hữu ích nếu bạn dự định đưa dự án vào résumé - hầu hết recruiter, hiring manager, vân vân sẽ không dành thời gian tạo tài khoản. Tính năng này cho phép họ xem thành quả của bạn mà không phải trải qua quy trình đăng ký rườm rà.
1. Làm cho nó đẹp mắt!

</div>