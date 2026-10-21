### Giới thiệu

Node.js (hay gọi ngắn gọn là "Node") là một môi trường runtime JavaScript cho phép bạn chạy JavaScript bên ngoài trình duyệt web. Chúng ta sẽ cần nó cho một số bài tập trong các bài học sắp tới. Để bắt đầu, trước khi cài đặt Node vào hệ thống, chúng ta cần một số công cụ bắt buộc.

Chúng ta sẽ cài đặt Node bằng nvm (Node Version Manager), vì công cụ này giúp việc chuyển đổi và nâng cấp phiên bản Node trở nên dễ dàng. Ngoài ra còn có một công cụ tên là npm, sau này chúng ta sẽ dùng để cài đặt nhiều thư viện và công cụ được sử dụng trong các môi trường JavaScript. Hai công cụ này rất dễ bị nhầm lẫn, nên hãy đọc kỹ!

Node cũng rất dễ cài đặt bằng nvm, vì vậy phần này sẽ diễn ra nhanh thôi :)

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Cài đặt và cấu hình nvm (Node Version Manager), Node và npm.
- Chạy Node REPL.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Cài đặt nvm

1. Trước tiên, chúng ta cần cài đặt nvm để có thể dễ dàng cài đặt và quản lý các bản Node. Hãy làm theo hướng dẫn tương ứng với hệ điều hành bạn đang sử dụng:
   - [Linux](https://github.com/TheOdinProject/curriculum/tree/main/foundations/javascript_basics/installation_guides/linux.md "Linux nvm installation")
   - [macOS](https://github.com/TheOdinProject/curriculum/tree/main/foundations/javascript_basics/installation_guides/macos.md "macOS nvm installation")

#### Cài đặt Node

Sau khi cài đặt nvm, chúng ta có thể dùng nó để cài đặt Node.

1. Trong terminal, chạy:

   ```bash
   nvm install --lts
   ```

   Lệnh này sẽ cài đặt phiên bản Node ổn định mới nhất thuộc nhánh "long-term support" (LTS), và bạn sẽ thấy rất nhiều output trong terminal. Nếu mọi thứ hoạt động đúng, trong phần output bạn sẽ thấy một dòng tương tự như sau (các ký tự X sẽ được thay bằng những con số thực tế):

   ```text
   Downloading and installing Node vXX.xx.x...
   ```

   Nếu không thấy, hãy đóng terminal, mở lại rồi chạy `nvm install --lts` lần nữa.

1. Chúng ta cần cho nvm biết sẽ dùng phiên bản Node nào khi chạy lệnh `node`. Việc này rất đơn giản, chỉ cần chạy lệnh sau:

   ```bash
   nvm use --lts
   ```

   Chúng ta đã yêu cầu nvm dùng phiên bản LTS mới nhất của Node đang được cài trên máy tính. Bạn **phải** dùng phiên bản LTS của Node để tránh tình trạng không tương thích với các package mà chúng ta sẽ cài trong những bài học sau.

   Bây giờ, khi chạy `node -v`, bạn sẽ thấy `vXX.xx.x` hoặc một giá trị tương tự (các ký tự X sẽ được thay bằng những con số thực tế).

   Cài đặt Node cũng đồng thời cài npm. Hãy chạy `npm -v`; tương tự như trên, bạn sẽ thấy `XX.xx.x` hoặc một giá trị tương tự.

   Nếu thấy những thông báo đó, bạn đã cài đặt Node và npm thành công!

1. Cuối cùng, chúng ta sẽ triển khai một thiết lập được giới thiệu trong npm v11.10.0, giúp trì hoãn việc cài đặt các dependency mới phát hành. Hãy chạy lệnh sau trong terminal:

   ```bash
   npm config set min-release-age=3
   ```

   Thiết lập này sẽ ngăn npm cài đặt các dependency được phát hành chưa đầy ba ngày, giúp tránh các vấn đề do package mới phát hành có thể đã bị xâm nhập, đồng thời góp phần bảo vệ chúng ta trước các cuộc tấn công chuỗi cung ứng.

#### Sử dụng Node REPL

Để thuận tiện, Node cung cấp một REPL, tức là một console tương tác cho phép bạn chạy và chỉnh sửa code JavaScript ngay trong terminal, tương tự console trong trình duyệt mà bạn đã sử dụng ở bài học trước. Công cụ này rất hữu ích để nhanh chóng debug hoặc kiểm thử các đoạn code nhỏ không liên quan đến trình duyệt mà không phải mở trình duyệt mỗi lần.

Để chạy console Node, hãy mở terminal và tự chạy `node` để mở REPL. Gõ `.exit` để thoát REPL.

</div>