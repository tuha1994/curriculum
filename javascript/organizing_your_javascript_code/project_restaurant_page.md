### Giới thiệu

Hãy sử dụng những gì chúng tôi đã học và tận dụng cơ hội tiếp theo để tiếp tục thực hiện thao tác DOM bằng cách hiển thị động trang chủ nhà hàng! Cuối cùng, chúng tôi sẽ chỉ sử dụng JavaScript để tạo toàn bộ nội dung của trang web!

** Lưu ý: Phần tử DOM phải được tạo bằng JavaScript nhưng có thể tạo kiểu trong một tệp CSS riêng biệt.**

<div class="lesson-note lesson-note--tip" markdown="1">

#### .gitignore

Khi làm việc với các gói được cài đặt bằng npm, bạn không cần theo dõi nội dung của `node_modules` bằng git cũng như không cần cung cấp tệp đó lên GitHub. Vì chúng tôi đã học trong [bài học npm](https://www.theodinproject.com/lessons/node-path-javascript-npm), tệp `package.json` chứa tất cả thông tin phụ thuộc vào bất kỳ ai cũng có thể sao chép dự án của bạn và cài đặt chúng trên máy của họ bằng `npm install`.

Bạn có thể tạo tệp `.gitignore` trong thư mục gốc của dự án và bằng cách viết tên tệp hoặc thư mục trong đó, bạn có thể muốn biết những thứ bạn không theo dõi. Theo thông lệ, người ta thường thêm `node_modules` vào `.gitignore` vì nó có thể trở nên rất lớn. Tương tự, `dist` thường bị bỏ qua vì nó có thể được tạo khi ai đó thực hiện lệnh đóng gói/xây dựng ứng dụng.

Khi tạo mới repo trên GitHub, có một tùy chọn để định nghĩa chỉ `.gitignore` mẫu.  Rất nhiều mẫu bao gồm các tệp và phổ biến thư mục thường không được theo dõi dựa trên loại dự án hoặc ngôn ngữ được sử dụng.  Đối với các dự án JavaScript, có bao mẫu `node` bao gồm `node_modules` và `dist`.

</div>

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bắt đầu dự án giống như cách bạn bắt đầu gói webpack hướng dẫn dự án, bằng cách tạo tệp `package.json` và thiết lập Webpack.
    - Hãy nhớ rằng bạn chỉ cần cài đặt và cấu hình những thứ bạn cần cho dự án của mình. Ví dụ: nếu bạn không định nghĩa các tập tin hình ảnh cục bộ trong mẫu HTML của mình, bạn sẽ không cần phải cài đặt và định cấu hình `html-loader`.
1. Tạo tệp `.gitignore` trong thư mục gốc của dự án của bạn. Nó phải chứa bản văn `node_modules` và `dist` trên các dòng riêng biệt.

   ```text
   node_modules
   dist
   ```

1. Thiết lập bộ xương HTML bên trong `src/template.html`. Bên trong phần thân, thêm phần tử `<header>` chứa `<nav>` với các nút (không phải liên kết!) cho các "tab" khác (ví dụ: các cho "Trang chủ", "Menu" hoặc "Giới thiệu", v.v.). Bên dưới `<header>`, thêm một `<div id="content">`.
1. Bên trong `src/index.js` viết lệnh `console.log` hoặc `alert` rồi chạy `npx webpack serve`. Mở [http://localhost:8080](http://localhost:8080) trong trình duyệt của bạn và kiểm tra xem JavaScript có chạy không.
1. Bên trong `div#content`, tạo trang chủ cho nhà hàng của bạn. Bạn có thể muốn bao gồm một hình ảnh, dòng tiêu đề và một số văn bản về mức độ tuyệt vời của nhà hàng; bạn không cần phải làm việc đó ở bất kỳ thời điểm nào. Bạn có thể mã hóa phần cứng thứ này vào HTML ngay bây giờ chỉ để xem chúng như thế nào trên trang.
1. Bây giờ hãy xóa mọi thứ trong `div#content` khỏi HTML (để bạn vẫn còn `<header>` và `<nav>` với `<div id="content">` trống bên dưới) và thay vào đó hãy tạo chúng bằng cách chỉ sử dụng JavaScript, ví dụ: bằng cách thêm từng phần tử mới vào `div#content` sau trang khi được tải lần đầu tiên. Vì tất cả chúng ta đều đã thiết lập để viết mã của thành nhiều tệp của mình, hãy viết hàm tải trang ban đầu này bên trong mô-đun của chính nó, sau đó nhập và gọi nó bên trong `index.js`.
1. Tiếp theo, bạn hãy thiết lập trang web nhà hàng của mình để sử dụng tính năng duyệt theo thẻ để truy cập Menu và Liên hệ trang web. Hãy xem hoạt động của [trang web xem trước trực tiếp của học sinh](https://web.archive.org/web/20221024060550/https://eckben.github.io/bearysBreakfastBar/) này để tìm nguồn cảm hứng trực quan.
    1. Đặt nội dung của mỗi "tab" bên trong mô-đun riêng của nó. Mỗi mô-đun sẽ xuất ra một div phần tử tạo chức năng, bổ sung nội dung và kiểu thích hợp cho phần tử sau đó thêm nó vào DOM.
    1. Viết tab chuyển logic bên trong `index.js`. Bạn nên xử lý sự kiện cho từng nút trong thanh điều hướng tiêu đề để xóa nội dung hiện tại của `div#content`, sau đó chạy 'tab mô-đun' chính xác để điền lại nội dung mới vào đó.

#### Triển khai

Hãy phát triển dự án của bạn lên các trang GitHub! Công việc này phức tạp hơn một chút so với các dự án trước đó, vì GitHub Pages cố gắng tìm kiếm `index.html` * trong thư mục gốc của dự án của bạn*, nhưng dự án của bạn lại nằm trong `dist`! Chúng tôi sẽ thực hiện một số bước để cung cấp nội dung *the* trong thư mục `dist` của bạn đã chia sẻ riêng trên GitHub, sau đó nhánh này sẽ có `index.html` cấp gốc để phân phát các trang GitHub.

Bạn không cần phải biết chính xác tất cả các lệnh làm gì - miễn là bạn làm theo hướng dẫn bên dưới một cách cẩn thận thì bạn sẽ ổn thôi. Bạn có thể sử dụng hướng dẫn này để phát triển dự án của mình và cũng có thể phát triển lại dự án nếu sau này bạn thực hiện nhiều thay đổi hơn cho dự án của mình.

1. Tạo một nhánh mới để phát triển khai báo bằng cách chạy `git branch gh-pages`. Bạn chỉ cần thực hiện công việc này trong lần phát triển đầu tiên. Các bước còn lại sẽ được thực hiện mỗi khi bạn phát triển hoặc phát triển lại dự án của mình.
1. Hãy chắc chắn rằng bạn đã cam kết tất cả công việc của mình. Bạn có thể sử dụng `git status` để xem những gì cần cam kết.
1. Chạy `git checkout gh-pages && git merge main --no-edit` để thay đổi nhánh và đồng bộ hóa các thay đổi của bạn từ `main` để bạn sẵn sàng phát triển khai báo.
1. Bây giờ, hãy sử dụng gói ứng dụng của chúng tôi vào `dist` bằng cách xây dựng lệnh của bạn. Hiện tại, đó là `npx webpack`.
1. Bây giờ có thêm một số lệnh. Chạy từng cái này theo thứ tự:

   ```bash
   git add dist -f && git commit -m "Deployment commit"
   git subtree push --prefix dist origin gh-pages
   git checkout main
   ```

1. Hãy nhớ lại rằng nhánh **source** cho GitHub Pages được cài đặt trong kho lưu trữ cài đặt của bạn. Hãy thay đổi cái này thành nhánh `gh-pages`. Đó phải là tất cả!

</div>
