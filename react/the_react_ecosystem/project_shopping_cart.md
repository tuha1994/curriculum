### Giới thiệu

Đến lúc này, bạn đã đi rất xa so với những ngày đầu làm quen với React. Bạn đã nắm các công cụ như router và framework kiểm thử, nhưng vẫn còn một chặng đường dài. Đây là thời điểm tuyệt vời để áp dụng các khái niệm này vào một project kinh điển — một giỏ hàng mô phỏng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Tạo một project React mới.
1. Hãy suy nghĩ về cấu trúc component và thư mục. Bạn có thể thiết lập application như thế nào? Bạn cần những component hoặc chức năng nào? Bạn nên ghi lại ở nơi dễ truy cập để có thể xem lại và bổ sung, nhờ đó theo dõi tiến độ.
1. Bạn nên có ba page: trang chủ, trang shop và trang giỏ hàng. Cho phép người dùng điều hướng giữa các page bằng thanh navigation hiển thị trên mọi page.
1. Bạn có thể thêm bất cứ thứ gì mình thích vào homepage! Một vài hình ảnh hoặc thông tin là hoàn toàn ổn; không cần phải cầu kỳ — mục đích là kiểm tra các khái niệm đã học.
1. Trên shop page, xây dựng một card riêng cho mỗi sản phẩm. Hiển thị một input để người dùng nhập số lượng muốn mua. Đồng thời thêm button tăng và giảm bên cạnh để tinh chỉnh. Bạn cũng có thể hiển thị tiêu đề cho mỗi sản phẩm cùng button "Add To Cart".
1. Khi giỏ hàng có sản phẩm, link tới cart page trên navbar phải cho biết có bao nhiêu sản phẩm trong giỏ. Con số này phải cập nhật theo thời gian thực khi người dùng thêm hoặc xóa sản phẩm.
1. Trên cart page, hiển thị các sản phẩm và số lượng, đồng thời cho phép người dùng tăng/giảm số lượng (bao gồm xóa nếu phù hợp). Không cần triển khai hệ thống checkout/payment.
1. Lấy các sản phẩm shop từ [FakeStore API](https://fakestoreapi.com) hoặc API tương tự.
1. Hãy kiểm thử app thật kỹ bằng React Testing Library. Cẩn thận không kiểm thử trực tiếp `react-router`, vì đây là thư viện bên ngoài và các developer của thư viện hẳn đã kiểm thử nó.
1. Như thường lệ, hãy style application để có thể giới thiệu sản phẩm của mình! Bạn đã có nhiều lựa chọn được cung cấp từ trước.
1. Cuối cùng, đã đến lúc deploy! Tùy giải pháp hosting đang dùng, bạn có thể cần cấu hình thêm để routing được xử lý đúng như một single-page application (SPA).

   - **Netlify**: Bạn cần thêm file `_redirects` vào thư mục `public/` của project. Chép nội dung sau để redirect mọi route về index page và để `react-router` xử lý phần còn lại. Bạn có thể đọc thêm tại [tài liệu Netlify về redirects](https://docs.netlify.com/routing/redirects/).

     ```text
     /* /index.html 200
     ```

   - **Vercel**: Bạn cần thêm file `vercel.json` ở thư mục gốc của project và chép cấu hình sau. Tương tự Netlify, cấu hình này redirect mọi route về index page và để `react-router` xử lý phần còn lại. Xem thêm tại [tài liệu Vercel về SPA và Vite](https://vercel.com/docs/frameworks/vite#using-vite-to-make-spas).

     ```json
     {
       "rewrites": [
         {
           "source": "/(.*)",
           "destination": "/index.html"
         }
       ]
     }
     ```

   - **Cloudflare Pages**: Tại thời điểm viết bài, khác với Netlify và Vercel, không cần thêm bước nào vì hành vi mặc định cho phép `react-router` xử lý đúng redirect cho SPA. Bạn có thể tìm hiểu thêm tại [tài liệu Cloudflare về serving pages](https://developers.cloudflare.com/pages/platform/serving-pages/).

</div>
