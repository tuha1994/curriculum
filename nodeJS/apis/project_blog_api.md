### Giới thiệu

Bạn biết mình cần gì không? Bạn cần một blog. Hoặc có thể không, hoặc bạn đã có blog rồi; dù thế nào, project này cũng là cách tuyệt vời để thực hành và thấy được lợi ích của việc tạo back-end chỉ dành cho API. Thực tế, chúng ta sẽ tạo back-end và *hai* front-end khác nhau để truy cập và chỉnh sửa bài viết trên blog. Một website front-end dành cho những người muốn đọc và bình luận bài viết, còn website kia chỉ dành cho bạn viết, chỉnh sửa và xuất bản bài viết.

Tại sao lại thiết lập như vậy? Vì ta có thể làm thế! Bài tập quan trọng ở đây là thiết lập API rồi truy cập nó từ bên ngoài. Tách riêng website đọc blog và chỉnh sửa blog có một số lợi ích về bảo mật, nhưng thực ra ta làm vậy chủ yếu để minh họa sức mạnh và tính linh hoạt của việc tách code back-end khỏi code front-end.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bạn tự quyết định cách cấu trúc project. Một số người thích tạo repository GitHub riêng cho mỗi app trong ba app sẽ làm, để tách biệt chúng và lịch sử commit. Một số khác thích monorepo, trong đó mỗi app nằm trong một thư mục riêng của cùng một repository.
1. Bắt đầu bằng việc thiết kế model và schema back-end. Cách thiết kế tùy bạn, nhưng có thể cân nhắc vài điều:
   - Blog phải có post và comment, vì vậy hãy nghĩ đến các field muốn đưa vào mỗi loại.
   - Bạn có yêu cầu người dùng để lại username hoặc email cùng comment không?
   - Bạn có hiển thị ngày hoặc timestamp cho post và comment không?
   - Post có lẽ nên có title, nhưng comment thì sao?
   - Một tính năng hữu ích của blog là cho phép có những post nằm trong database nhưng chưa được công khai. Bạn sẽ đánh dấu post đã published và chưa published trong DB như thế nào?
   - Bạn sẽ cần một user model chứa các tác giả blog và các tài khoản người dùng thông thường. Ngay cả khi quyết định chỉ có một tác giả và không có tài khoản người dùng thông thường, một user model tối thiểu vẫn hữu ích để dễ bảo vệ route bằng authentication.
1. Thiết lập app Express và định nghĩa model trong Prisma.
1. Thiết lập route và controller! Hãy nghĩ về cách tổ chức RESTful cho phần này. Hầu hết ví dụ trong bài trước tập trung vào post và comment nên việc này không quá khó.
   - Bạn có thể kiểm thử route theo cách tùy ý. Dùng `curl` trong terminal là một cách tiện lợi, nhưng dùng web browser cũng hiệu quả tương đương. Một số nền tảng cho phép gửi request `PUT` và `POST` mà không cần thiết lập và điền HTML form. [Postman](https://www.postman.com/downloads/) có lẽ là nền tảng phổ biến nhất.
1. Một số route cần được bảo vệ bằng authentication. Bạn chắc chắn không muốn người lạ bất kỳ trên Internet chỉnh sửa bài viết của mình! Nếu triển khai cả tài khoản người dùng thông thường, bạn cũng có thể muốn bảo vệ một số route, chỉ cho phép truy cập sau khi đăng nhập.
   - Dù có nhiều cách xử lý authentication, trong project này hãy dùng JWT.
   - Bạn có thể dùng [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) để tạo và xác minh JWT. Bạn có thể muốn dùng [JWT strategy của Passport](https://github.com/mikenicholson/passport-jwt) để xác minh JWT, đặc biệt nếu đã thiết lập Passport với local strategy để xử lý đăng nhập.
   - Đăng nhập thành công sẽ cấp cho người dùng một JWT. Người dùng có thể đính kèm JWT vào các request sau đó; API có thể xác minh JWT để cho phép hoặc từ chối quyền truy cập phần còn lại của route được bảo vệ. Khi người dùng đăng xuất, client có thể xóa JWT khỏi storage.
   - Có nhiều cách gửi và lưu JWT, chẳng hạn qua cookie, lưu trong localStorage, dùng access/refresh token, v.v. Một số cách phức tạp hơn (dù nếu triển khai đúng có thể bảo mật hơn), đặc biệt sau khi triển khai cả hai phía. Ví dụ, cross-site cookie có thể thực sự đau đầu nếu bạn không biết một số chi tiết bổ sung. Bạn có thể tìm hiểu các lựa chọn này trong tương lai. Hiện tại, hãy giữ mọi thứ đơn giản: gửi JWT qua header "Authorization" với schema "Bearer", và để client lưu JWT trong localStorage.
1. Khi API hoạt động, bạn có thể tập trung vào code front-end. Cách làm hoàn toàn tùy bạn. Nếu quen React thì cứ dùng! Nếu thích HTML, CSS và vanilla JavaScript thuần thì cũng không sao. Tất cả những gì cần làm để đưa post vào website là `fetch` đúng endpoint API rồi hiển thị kết quả. Làm việc với fetch và API từ góc nhìn front-end được đề cập trong [bài Working with APIs](https://www.theodinproject.com/lessons/javascript-working-with-apis).
1. Tạo website thứ hai để viết và chỉnh sửa post. Bạn có thể thiết lập theo cách tùy ý, nhưng các tính năng sau có thể hữu ích:
   - Danh sách tất cả post, cho biết chúng đã được published hay chưa.
   - Nút publish các post chưa published hoặc unpublish các post đã published!
   - Form 'NEW POST'. Nếu muốn nâng cao hơn, bạn có thể dùng trình soạn thảo rich text như [TinyMCE](https://www.tiny.cloud/docs/tinymce/6/cloud-quick-start/).
   - Khả năng quản lý comment (tức là xóa hoặc chỉnh sửa chúng).
1. Mức độ đầu tư vào code front-end của project này tùy bạn. Về kỹ thuật, đây là khóa học tập trung vào back-end, nên nếu muốn, bạn hoàn toàn có thể tập trung vào REST API.
1. Việc triển khai các app riêng biệt không có gì đặc biệt. Hãy triển khai API như các project trước bằng PaaS từ [bài Deployment](https://www.theodinproject.com/lessons/node-path-nodejs-deployment), và triển khai front-end như cách bạn đã từng triển khai front-end. Nếu dùng React, hãy nhớ lại một số lựa chọn hosting từ [project CV Application](https://www.theodinproject.com/lessons/node-path-react-new-cv-application).

</div>