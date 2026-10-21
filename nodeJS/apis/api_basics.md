### Giới thiệu

Trong những năm gần đây, một mô hình phát triển website mới ngày càng phổ biến. Thay vì tạo một app lưu trữ cả cơ sở dữ liệu và view template, nhiều developer tách các mối quan tâm này thành những project riêng, lưu trữ back-end và cơ sở dữ liệu trên một server (chẳng hạn trên [Heroku](https://www.heroku.com/) hoặc VPS như [Digital Ocean](https://www.digitalocean.com/)), rồi dùng dịch vụ như [GitHub Pages](https://pages.github.com/) hoặc [Netlify](https://www.netlify.com/) để lưu trữ front-end. Kỹ thuật này đôi khi được gọi là [Jamstack](https://jamstack.org/what-is-jamstack/).

Tổ chức project theo cách này có lợi vì project trở nên module hóa hơn thay vì trộn business logic với view logic. Cách này cũng cho phép bạn dùng một nguồn back-end duy nhất cho nhiều ứng dụng front-end như website, desktop app hoặc mobile app. Một số developer thích mô hình này vì họ muốn dùng các frontend framework như React hoặc Vue để tạo những single-page application chỉ ở phía front-end và có giao diện tốt.

Các ứng dụng front-end và back-end thường giao tiếp với nhau bằng JSON; có thể bạn đã gặp điều này nếu hoàn thành khóa học JavaScript front-end của chúng tôi. Vì vậy, điều duy nhất bạn thực sự cần học lúc này là cách khiến ứng dụng Express giao tiếp bằng JSON thay vì HTML. Bài tập cuối bài sẽ hướng dẫn bạn qua một tutorial, nhưng về cơ bản, tất cả những gì cần làm là truyền thông tin vào [`res.json()`](https://expressjs.com/en/5x/api/response/#resjson) thay vì [`res.send()`](https://expressjs.com/en/5x/api/response/#ressend) hoặc [`res.render()`](https://expressjs.com/en/5x/api/response/#resrender). Dễ đến mức nào, đúng không?

Nếu nhớ lại cách tổ chức route trong [bài Routes](https://www.theodinproject.com/lessons/nodejs-routes#routers), ta đã nhóm các route liên quan rồi tách từng nhóm vào một tệp riêng. Cách tiếp cận này giúp dễ dàng sửa các route cụ thể hơn mà không ảnh hưởng đến các route khác.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Biết `REST` là viết tắt của cụm từ nào.
- Giải thích mục đích dùng REST khi tổ chức API.
- Trình bày quy ước đặt tên REST cho endpoint API.
- Củng cố hiểu biết về HTTP Method/Verb.
- Mô tả Same Origin Policy.
- Giải thích mục đích của CORS.
- Dùng CORS làm middleware trong Express (toàn cục và trên một route riêng).
- Cấu hình CORS chỉ cho phép một số origin nhất định truy cập API.
- Giải thích các header CORS.

### REST

Cấu trúc API có thể có nhiều dạng. Ví dụ, bạn có thể có các route tên là `/api/getAllPostComments/:postid` hoặc `/api/posts/:postid/comments`.
*Gần như vậy*, theo quy ước, ta nên tuân theo REST (viết tắt của Representational State Transfer), một phương pháp tổ chức API phổ biến và thông dụng, tương ứng với [CRUD action](https://www.codecademy.com/article/what-is-crud). Tuân theo các pattern đã được thiết lập như REST giúp API dễ bảo trì hơn và giúp developer khác tích hợp với API dễ hơn. Phát triển phần mềm thường là quá trình giao tiếp rõ ràng, và việc tuân thủ các kỳ vọng chung sẽ hỗ trợ điều đó.

[Định nghĩa kỹ thuật của REST](https://en.wikipedia.org/wiki/Representational_state_transfer) khá phức tạp, nhưng trong phạm vi của chúng ta, phần lớn yếu tố (statelessness, cacheability, v.v.) mặc định đã được Express xử lý khi xuất JSON. Điều ta cần tập trung là cách **tổ chức URI của endpoint** (Uniform Resource Identifier).

REST API dựa trên resource. Về cơ bản, thay vì đặt tên như `/getPostComments` hoặc `/savePostInDatabase`, ta tham chiếu **trực tiếp đến resource** (ở đây là blog post) và dùng HTTP verb như GET, POST, PUT và DELETE để xác định action.
Thông thường, mỗi resource có 2 URI: một cho toàn bộ collection và một cho object riêng trong collection đó. Ví dụ, bạn có thể lấy danh sách blog post từ `/posts`, rồi lấy một post cụ thể từ `/posts/:postid`. Bạn cũng có thể lồng các collection theo cách này. Để lấy danh sách comment của một post, bạn truy cập `/posts/:postid/comments`, còn để lấy một comment cụ thể: `/posts/:postid/comments/:commentid`. Dưới đây là một số ví dụ cơ bản khác về endpoint.

#### Bảng HTTP Verb

| Verb   | Hành động | Ví dụ                                       |
| ------ | ------ | --------------------------------------------- |
| POST   | Tạo | `POST /posts` Tạo một blog post mới         |
| GET    | Đọc   | `GET /posts/:postid` Lấy một post cụ thể    |
| PUT    | Cập nhật | `PUT /posts/:postid` Cập nhật một post cụ thể    |
| DELETE | Xóa | `DELETE /posts/:postid` Xóa một post cụ thể |

Mỗi phần của URI API đều chỉ định resource. Ví dụ, `GET /posts` trả về toàn bộ danh sách blog post, còn `GET /posts/:postid` chỉ định chính xác blog post ta muốn. Ta có thể lồng sâu hơn bằng `GET /posts/:postid/comments` để trả về danh sách comment của blog post đó, hoặc thậm chí `GET /posts/:postid/comments/:commentid` cho một comment rất cụ thể của blog post.

### CORS

[Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) là một biện pháp bảo mật quan trọng của trình duyệt, hạn chế web page gửi request đến origin khác với origin đã phục vụ trang đó (hãy xem liên kết trên để biết một vài ví dụ về những gì được xem là 'same origin').

Trong project tiếp theo, ta sẽ triển khai REST API và front-end riêng biệt (trên các domain khác nhau), nghĩa là cần bật **Cross-Origin Resource Sharing (CORS)** trên server để front-end riêng có thể truy cập resource của nó. Express có [gói CORS middleware](https://expressjs.com/en/resources/middleware/cors.html) giúp thiết lập mọi thứ.

Hiện tại, cho phép truy cập từ mọi origin là chấp nhận được. Điều này khiến quá trình phát triển dễ hơn khá nhiều, nhưng với bất kỳ project *thực tế* nào, sau khi triển khai lên môi trường production, có lẽ bạn sẽ muốn chặn rõ ràng mọi origin *ngoại trừ* website front-end. Tài liệu trên giải thích cách thực hiện.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc về [thiết kế RESTful API](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design). Nếu muốn code theo bài viết đầu tiên, hãy lưu ý bài viết dùng body-parser middleware để phân tích dữ liệu JSON trong request body; tuy nhiên từ Express 4.16.0, chức năng phân tích này đã được tích hợp trực tiếp vào package Express.
1. Đọc và code theo tutorial về [thiết lập REST API trong Express](https://www.robinwieruch.de/node-express-server-rest-api/). Đây là một trong những tutorial Express hay nhất chúng tôi từng gặp; tutorial cũng nói về tổ chức code theo module, viết middleware và liên kết đến một số thông tin bổ sung hữu ích ở cuối.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ sung.

- [Định nghĩa REST dựa trên ví dụ đơn giản](https://simple.wikipedia.org/wiki/Representational_state_transfer).