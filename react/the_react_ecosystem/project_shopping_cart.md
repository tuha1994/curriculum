### Giới thiệu

Bây giờ bạn đã vượt xa những ngày còn bé React- của mình. Bạn đã có sẵn các công cụ như bộ định tuyến và khung thử nghiệm nhưng bạn vẫn còn một chặng đường dài phía trước. Bây giờ là thời điểm tuyệt vời để áp dụng những khái niệm này vào một dự án cổ điển - một chiếc xe đẩy hàng mô phỏng.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Tạo một dự án React mới.
1. Hãy suy nghĩ về thành phần và cấu trúc thư mục. Bạn có thể thiết lập ứng dụng của mình bằng cách nào? Bạn cần những thành phần hoặc chức năng nào? Bạn nên ghi lại điều này ở nơi nào đó mà bạn có thể dễ dàng tìm đến và tham khảo lại cũng như bổ sung vào đó để theo dõi.
1. Bạn nên có ba trang: trang chủ, trang cửa hàng và trang giỏ hàng. Cho phép người dùng điều hướng giữa các trang bằng thanh điều hướng, thanh này sẽ được hiển thị trên tất cả các trang.
1. Vào trang chủ của bạn, bạn có thể thêm bất cứ điều gì bạn muốn! Một vài hình ảnh hoặc thông tin sẽ hoàn toàn ổn; nó không nhất thiết phải là thứ gì đó cầu kỳ - đó là để kiểm tra các khái niệm đã được dạy cho đến nay.
1. Trên trang cửa hàng, hãy xây dựng các thành phần thẻ riêng lẻ cho từng sản phẩm của bạn. Hiển thị trường nhập trên đó, cho phép người dùng nhập thủ công số lượng mặt hàng họ muốn mua. Ngoài ra, thêm nút tăng giảm bên cạnh để tinh chỉnh. Bạn cũng có thể hiển thị tiêu đề cho từng sản phẩm cũng như nút "Thêm vào giỏ hàng".
1. Khi có mặt hàng trong giỏ hàng của bạn, liên kết trang giỏ hàng trong thanh điều hướng sẽ cho biết có bao nhiêu mặt hàng trong giỏ hàng của bạn. Điều này sẽ cập nhật theo thời gian thực khi người dùng thêm hoặc xóa các mặt hàng khỏi giỏ hàng của họ.
1. Trên trang giỏ hàng, bạn nên hiển thị các mặt hàng cũng như số lượng của chúng, đồng thời cho phép người dùng increase/decrease số lượng mặt hàng trong giỏ hàng của họ (bao gồm cả việc loại bỏ nếu thích hợp). Không cần triển khai bất kỳ hệ thống checkout/payment nào.
1. Lấy các mặt hàng trong cửa hàng của bạn từ [FakeStore API](https://fakestoreapi.com) hoặc một cái gì đó tương tự.
1. Đảm bảo kiểm tra kỹ ứng dụng của bạn bằng Thư viện thử nghiệm React. Hãy cẩn thận không kiểm tra trực tiếp `react-router` vì đây là thư viện bên ngoài và các nhà phát triển làm việc trên đó chắc chắn đã kiểm tra thư viện đó rồi.
1. Như thường lệ, hãy tạo kiểu cho ứng dụng của bạn để bạn có thể thể hiện nó! Bạn đã có một loạt các tùy chọn được cung cấp rồi.
1. Cuối cùng, đã đến lúc triển khai nó! Tùy thuộc vào giải pháp lưu trữ bạn đang sử dụng, bạn có thể cần một số cấu hình bổ sung để việc định tuyến của bạn được xử lý chính xác dưới dạng một ứng dụng một trang (SPA).

   - **Netlify**: Bạn cần thêm tệp `_redirects` vào thư mục `public/` của dự án. Sao chép phần sau để chuyển hướng tất cả các tuyến đến trang chỉ mục và để `react-router` xử lý phần còn lại. Bạn có thể đọc thêm về điều này tại [Tài liệu Netlify về chuyển hướng](https://docs.netlify.com/routing/redirects/).

     ```text
     /* /index.html 200
     ```

   - **Vercel**: Bạn cần thêm tệp `vercel.json` vào thư mục gốc của dự án và sao chép cấu hình sau. Tương tự như Netlify, điều này chuyển hướng tất cả các tuyến đến trang chỉ mục và cho phép `react-router` xử lý phần còn lại. Thông tin thêm có thể được tìm thấy ở đây tại [Tài liệu Vercel dành cho SPA và Vite](https://vercel.com/docs/frameworks/vite#using-vite-to-make-spas).

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

   - **Trang đám mây**: Tính đến thời điểm viết bài, không giống như Netlify và Vercel, không yêu cầu bước bổ sung nào vì hành vi mặc định sẽ cho phép `react-router` xử lý chính xác các chuyển hướng cho SPA. Bạn có thể tìm hiểu thêm về điều này tại [Tài liệu về Cloudflare trên các trang phục vụ](https://developers.cloudflare.com/pages/platform/serving-pages/).

</div>
