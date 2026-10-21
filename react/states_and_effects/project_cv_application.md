### Giới thiệu

Xin chúc mừng, bạn đã lọt vào dự án React đầu tiên. Trong dự án này, bạn sẽ tạo một ứng dụng nhỏ nơi người dùng có thể nhập thông tin của họ và tạo CV/sơ yếu lý lịch. Đây là một dự án tuyệt vời để giúp bạn bắt đầu vì nó bao gồm nhiều khái niệm cơ bản mà bạn đã học cho đến nay mà không cần quá phức tạp.

### Phân công

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Kết xuất kép

Trong khi làm việc với dự án này, bạn có thể nhận thấy rằng một số phần trong mã của bạn thực thi hai lần (ví dụ, nhật ký bảng điều khiển xuất hiện gấp đôi). Điều này là do [React.StrictMode](https://react.dev/reference/react/StrictMode) và là hành vi dự kiến. Chúng ta sẽ tìm hiểu điều này nhiều hơn trong bài học tiếp theo, vì vậy bây giờ đừng lo lắng về điều đó.

</div>

1. Tạo một dự án React mới.
1. Hãy suy nghĩ về cách cấu trúc ứng dụng của bạn thành các thành phần. Ứng dụng của bạn nên bao gồm:
    1. Một phần để thêm thông tin chung như tên, email và số điện thoại.
    1. Một phần để thêm kinh nghiệm học tập của bạn (tên trường, tên ngành học và ngày học)
    1. Phần bổ sung kinh nghiệm thực tế (tên công ty, chức danh, trách nhiệm chính trong công việc của bạn, thời gian từ và cho đến khi bạn làm việc cho công ty đó)
1. Đảm bảo bao gồm nút chỉnh sửa và gửi cho từng phần hoặc cho toàn bộ CV. Nút gửi sẽ gửi biểu mẫu của bạn và hiển thị giá trị của các trường nhập của bạn trong phần tử HTML. Nút chỉnh sửa sẽ thêm lại (hiển thị) các trường đầu vào, với thông tin được hiển thị trước đó dưới dạng giá trị. Trong các trường nhập đó, bạn có thể chỉnh sửa và gửi lại nội dung. Bạn sẽ sử dụng nhiều trạng thái và đạo cụ, vì vậy hãy chắc chắn rằng bạn hiểu những khái niệm đó.
1. Tạo thư mục `components` trong thư mục `src` của bạn và thêm các thành phần của bạn.
1. Bao gồm thư mục `styles` trong thư mục `src` cho các tệp CSS của bạn. Bạn sẽ cần nhập những thứ này vào các tệp thành phần để sử dụng chúng.
1. Đẩy kết quả và triển khai chúng với bất kỳ tùy chọn nào được đề cập bên dưới. Tại thời điểm này của chương trình giảng dạy, việc bạn chọn nền tảng nào không quan trọng miễn là dự án của bạn được hiển thị trực tuyến!

</div>

#### Triển khai ứng dụng React

Mặc dù chúng tôi đang sử dụng Trang GitHub để triển khai **trang web tĩnh** cho đến bây giờ và chúng tôi chắc chắn có thể tiếp tục làm như vậy bằng những cách hacky, việc để PaaS (Nền tảng là dịch vụ) làm điều tương tự cho chúng tôi sẽ dễ dàng hơn nhiều! Có rất nhiều lựa chọn để lưu trữ những trang web như vậy, và [Vite lưu trữ tài liệu](https://vitejs.dev/guide/static-deploy.html) đi qua những cái phổ biến nhất trong số đó.

Một số trong số này không chỉ loại bỏ các bước bổ sung sau khi đẩy mã của chúng tôi tới GitHub vì các dịch vụ này còn có các công cụ tự động*triển khai khi đẩy*, nhưng bạn cũng không phải lo lắng về các vấn đề khác trong chương trình giảng dạy, như bộ định tuyến và các bước xây dựng. Bây giờ chúng ta sẽ sử dụng một số tùy chọn đã chọn. Khi bạn đã hoàn thành nhiệm vụ, hãy chọn một trong các tùy chọn sau và triển khai!

#### Netlify

Mặc dù có nhiều cách để triển khai [Netlify](https://www.netlify.com/)([Tài liệu Netlify](https://docs.netlify.com/)), chẳng hạn như tải trực tiếp `dist` của bạn lên hoặc sử dụng `netlify-cli`, cách thuận tiện nhất là nhập trực tiếp kho lưu trữ GitHub của bạn vào Netlify.

1. Đẩy ứng dụng React của bạn tới GitHub.
1. [Nhập dự án của bạn vào Netlify](https://app.netlify.com/start) bằng cách đăng nhập và chọn kho lưu trữ của bạn.
1. Chọn nhánh để triển khai (cài đặt mặc định, từ `main`, hoạt động) và nhấn "Triển khai trang web"!
1. Bạn có thể truy cập thêm cài đặt tại đây nếu cần!

#### Vercel

Tương tự với Netlify [Vercel](https://www.vercel.com/)([Tài liệu Vercel](https://www.vercel.com/docs)) cũng cung cấp một số cách để triển khai và chúng tôi cũng sẽ nhập kho lưu trữ GitHub của mình để nhận được các lợi ích như triển khai khi đẩy.

1. Một lần nữa, đẩy ứng dụng React của bạn lên GitHub.
1. [Nhập dự án của bạn vào Vercel](https://vercel.com/new).
1. Vercel sẽ tự động phát hiện bạn đang sử dụng Vite. Đặt tên của bạn theo ý muốn và nhấn "Triển khai"!

#### Trang đám mây

[Trang đám mây](https://pages.cloudflare.com/)([Tài liệu về trang Cloudflare](https://developers.cloudflare.com/pages)) có quy trình và lợi ích tương tự như hai tùy chọn còn lại: nhập kho lưu trữ GitHub của bạn và đặt thư mục đầu ra và lệnh xây dựng phù hợp.

1. Có, đẩy ứng dụng React của bạn lên GitHub.
1. [Nhập dự án của bạn vào Trang Cloudflare](https://developers.cloudflare.com/pages/framework-guides/deploy-a-vite3-project/#:~:text=To%20deploy%20your%20site%20to%20Pages). Đảm bảo khi tạo ứng dụng, bạn tạo bằng Trang thay vì tạo Công nhân.
1. Nhấn "Lưu và triển khai" và xem nó trở nên sống động!
