### Giới thiệu

[Git](https://git-scm.com/) là một hệ thống kiểm soát phiên bản rất phổ biến. Bạn sẽ làm quen với phần mềm này rất nhiều trong suốt quá trình học TOP, vì vậy hiện tại đừng quá lo lắng nếu chưa hiểu hết về nó. Về sau, chương trình có nhiều bài học tập trung vào Git.

[GitHub](https://github.com/) là một dịch vụ cho phép bạn tải lên, lưu trữ và quản lý mã nguồn bằng Git thông qua một giao diện web thân thiện.

Mặc dù GitHub và Git có tên nghe khá giống nhau, chúng không phải là cùng một thứ, cũng không được tạo ra bởi cùng một công ty.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Git là gì.
- GitHub là gì.
- Cài đặt và cấu hình Git.
- Tạo tài khoản GitHub.
- Kết nối Git với tài khoản GitHub.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Cài đặt Git

1. Tùy thuộc vào hệ điều hành bạn đang sử dụng, hãy làm theo hướng dẫn cài đặt Git tương ứng dưới đây:
   - [Linux](https://github.com/TheOdinProject/curriculum/tree/main/foundations/installations/installation_guides/git/linux.md "Linux Git installation")
   - [macOS](https://github.com/TheOdinProject/curriculum/tree/main/foundations/installations/installation_guides/git/macos.md "macOS Git installation")
   - [ChromeOS](https://github.com/TheOdinProject/curriculum/tree/main/foundations/installations/installation_guides/git/chromeos.md "ChromeOS Git installation")

#### Cấu hình Git và GitHub

1. Truy cập [GitHub.com](https://github.com/) và tạo một tài khoản! Trong quá trình thiết lập tài khoản, bạn sẽ được yêu cầu cung cấp địa chỉ email. Đây phải là email có thật và theo mặc định sẽ được dùng để xác định các đóng góp của bạn. Nếu bạn quan tâm đến quyền riêng tư hoặc đơn giản là không muốn địa chỉ email của mình được công khai, hãy nhớ chọn hai ô sau trên trang [Cài đặt email](https://github.com/settings/emails) sau khi đăng nhập:

   ![Cài đặt email riêng tư trên GitHub](https://cdn.statically.io/gh/TheOdinProject/curriculum/725b80d126105d2f717697f97e7eaefb5f886c7b/foundations/installations/setting_up_git/imgs/01.png)

   Bật hai tùy chọn này sẽ giúp ngăn việc vô tình để lộ địa chỉ email cá nhân của bạn khi làm việc với Git và GitHub.

   Bạn cũng có thể thấy một địa chỉ email bên dưới tùy chọn **Giữ địa chỉ email của tôi ở chế độ riêng tư**. Đây là địa chỉ email GitHub riêng tư của bạn. **Nếu định sử dụng địa chỉ này, hãy ghi lại ngay vì bạn sẽ cần nó khi thiết lập Git ở bước tiếp theo.**

   1. (Tùy chọn) Bật xác thực hai yếu tố (2FA) của GitHub. Đây là một lớp bảo mật bổ sung được sử dụng khi đăng nhập vào các trang web hoặc ứng dụng. Với 2FA, bạn phải đăng nhập bằng tên người dùng và mật khẩu, đồng thời cung cấp thêm một hình thức xác thực mà chỉ bạn biết hoặc có quyền truy cập.

      Truy cập [Tài liệu GitHub về 2FA](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-app), sau đó làm theo hướng dẫn cấu hình. Ở bước 1, chúng tôi khuyên bạn dùng Google Authenticator, một ứng dụng mật khẩu dùng một lần theo thời gian (TOTP). Truy cập [Trợ giúp Tài khoản Google](https://support.google.com/accounts/answer/1066447), nhấp vào `Android` hoặc `iPhone & iPad`, rồi làm theo hướng dẫn tải xuống và thiết lập.

      <div class="lesson-note lesson-note--warning" markdown=1>

      #### Làm mất thông tin xác thực 2FA hoặc quyền truy cập vào mã khôi phục tài khoản

      Vì lý do bảo mật, bộ phận Hỗ trợ GitHub sẽ không thể khôi phục quyền truy cập vào các tài khoản đã bật xác thực hai yếu tố nếu bạn làm mất thông tin xác thực hai yếu tố hoặc mất quyền truy cập vào các phương thức khôi phục tài khoản.

      </div>

1. Để Git hoạt động đúng cách, chúng ta cần cho Git biết bạn là ai để Git có thể liên kết người dùng Git cục bộ (chính là bạn) với GitHub. Khi làm việc trong một nhóm, điều này cho phép mọi người biết bạn đã commit những gì và ai là người commit từng dòng mã.

   Các lệnh dưới đây sẽ cấu hình Git. Hãy nhớ nhập thông tin của chính bạn bên trong dấu ngoặc kép (nhưng vẫn phải giữ các dấu ngoặc kép!) — nếu bạn chọn giữ email riêng tư trên GitHub, hãy dùng địa chỉ email GitHub riêng tư đặc biệt của bạn từ bước 1.

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email yourname@example.com
   ```

   Ví dụ, nếu bạn đặt email ở chế độ riêng tư trên GitHub, lệnh thứ hai sẽ trông tương tự như sau:

   ```bash
   git config --global user.email 123456789+odin@users.noreply.github.com # Remember to use your own private GitHub email here.
   ```

   Gần đây GitHub đã thay đổi nhánh mặc định trên các kho lưu trữ mới từ `master` thành `main`. Hãy dùng lệnh sau để thay đổi nhánh mặc định của Git:

   ```bash
   git config --global init.defaultBranch main
   ```

   Để xác minh mọi thứ đang hoạt động đúng cách, hãy nhập các lệnh sau và kiểm tra xem kết quả có khớp với tên và địa chỉ email của bạn hay không.

   ```bash
   git config --get user.name
   git config --get user.email
   ```

   <div class="lesson-note" markdown="1">

   #### Dành cho người dùng macOS

   Chạy hai lệnh sau để yêu cầu Git bỏ qua các tệp .DS_Store, vốn được tự động tạo khi bạn dùng Finder để mở một thư mục. Tệp .DS_Store bị ẩn với người dùng và chứa các thuộc tính hoặc siêu dữ liệu tùy chỉnh (chẳng hạn như hình thu nhỏ) của thư mục. Nếu không cấu hình Git để bỏ qua chúng, những tệp .DS_Store phiền phức sẽ xuất hiện trong các commit của bạn. Hãy nhớ sao chép và dán từng lệnh này vào terminal.

   ```bash
   echo .DS_Store >> ~/.gitignore_global
   git config --global core.excludesfile ~/.gitignore_global
   ```

   </div>

1. Tạo một khóa SSH, đây là một mã định danh được bảo mật bằng mật mã. Nó giống như một mật khẩu rất dài dùng để xác định máy tính của bạn. GitHub sử dụng khóa SSH để cho phép bạn tải mã lên kho lưu trữ mà không phải nhập tên người dùng và mật khẩu mỗi lần.

   <div class="lesson-note" markdown="1">

   #### Nhiều khóa SSH

   Nếu trước khi bắt đầu The Odin Project, bạn đã thiết lập một cặp khóa SSH với GitHub trên một máy tính khác, GitHub cho phép bạn liên kết nhiều cặp khóa với tài khoản của mình. Bạn chỉ cần làm lại các hướng dẫn này để thiết lập thêm một cặp khóa và đăng ký cặp khóa đó với GitHub.

   </div>

   Trước hết, chúng ta cần kiểm tra xem bạn đã cài đặt khóa SSH theo thuật toán Ed25519 hay chưa. Nhập lệnh sau vào terminal và đối chiếu kết quả với thông tin bên dưới:

   ```bash
   ls ~/.ssh/id_ed25519.pub
   ```

   Nếu trong bảng điều khiển xuất hiện thông báo chứa dòng chữ "No such file or directory", nghĩa là bạn chưa có khóa SSH Ed25519 và cần tạo một khóa. Nếu không xuất hiện thông báo như vậy trong kết quả của bảng điều khiển, bạn có thể chuyển thẳng sang bước tiếp theo.

   Để tạo khóa SSH mới, hãy chạy lệnh sau trong terminal.

   ```bash
   ssh-keygen -t ed25519
   ```

   Khi chương trình hỏi vị trí lưu khóa được tạo, chỉ cần nhấn <kbd>Enter</kbd>.

   Tiếp theo, chương trình sẽ hỏi bạn một cụm mật khẩu. Cụm mật khẩu này được dùng để mã hóa khóa SSH riêng tư được lưu trên máy tính của bạn, và bạn sẽ phải nhập cụm mật khẩu đó mỗi lần sử dụng SSH với các khóa này. Nếu không dùng cụm mật khẩu, khóa riêng tư sẽ có thể được đọc bởi bất kỳ ai có quyền truy cập vào máy tính của bạn, và người đó sẽ có thể chỉnh sửa toàn bộ kho lưu trữ GitHub của bạn. Bạn có thể nhập cụm mật khẩu nếu muốn, nhưng không bắt buộc. Nếu chọn không dùng cụm mật khẩu, chỉ cần nhấn <kbd>Enter</kbd> mà không nhập gì.

1. Bây giờ, bạn cần cho GitHub biết khóa SSH của mình là gì để có thể đẩy mã lên mà không phải nhập mật khẩu mỗi lần.

   Trước hết, bạn sẽ đi đến nơi GitHub tiếp nhận khóa SSH của chúng ta. Đăng nhập GitHub và nhấp vào ảnh hồ sơ ở góc trên bên phải. Sau đó, nhấp vào `Settings` trong trình đơn thả xuống.

   Tiếp theo, ở phía bên trái, nhấp vào `SSH and GPG keys`. Sau đó, nhấp vào nút màu xanh lá ở góc trên bên phải có chữ `New SSH Key`. Đặt tên khóa đủ mô tả để bạn nhớ khóa SSH này đến từ thiết bị nào, chẳng hạn như `linux-ubuntu`. Để nguyên cửa sổ này trong khi thực hiện các bước tiếp theo.

   Bây giờ bạn cần sao chép khóa SSH công khai của mình. Để làm việc này, chúng ta sẽ dùng một lệnh có tên là [`cat`](http://www.linfo.org/cat.html) để đọc tệp ra bảng điều khiển. (Lưu ý rằng phần mở rộng tệp `.pub` rất quan trọng trong trường hợp này.)

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

   Bôi đen và sao chép toàn bộ kết quả của lệnh. Nếu bạn đã làm theo các hướng dẫn trên, kết quả có thể sẽ bắt đầu bằng `ssh-ed25519` và kết thúc bằng `username@hostname`.

   Bây giờ, quay lại GitHub trong cửa sổ trình duyệt và dán khóa đã sao chép vào trường khóa. Giữ nguyên loại khóa là `Authentication Key`, rồi nhấp vào `Add SSH key`. Xong rồi! Bạn đã thêm khóa SSH thành công!

1. Kiểm tra khóa bằng cách làm theo [hướng dẫn của GitHub để kiểm tra kết nối SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection?platform=linux). Hãy đảm bảo kết quả dấu vân tay trong terminal khớp với [một trong bốn dấu vân tay công khai của GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints).

   Bạn sẽ thấy phản hồi sau trong terminal: `Hi username! You've successfully authenticated, but GitHub does not provide shell access.` Đừng lo lắng vì GitHub không cung cấp quyền truy cập shell. Nếu thấy thông báo này, nghĩa là bạn đã thêm khóa SSH thành công và có thể tiếp tục. Nếu kết quả không khớp chính xác, hãy thử thực hiện lại các bước này hoặc vào [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB) để xin trợ giúp.

#### Hoàn tất!

Hãy cho chúng tôi biết mọi việc diễn ra thế nào! Bạn đã hoàn thành phần cài đặt cơ bản, làm tốt lắm! Khi tiếp tục chương trình, sẽ còn có những công cụ khác cần cài đặt, vì vậy hãy chú ý theo dõi!

Có lẽ bạn cảm thấy mình đang phải tiếp nhận quá nhiều thứ và chưa hiểu rõ phần lớn những gì đã làm. Điều đó hoàn toàn bình thường. Hãy kiên trì. Bạn có thể làm được! Và chúng tôi luôn ủng hộ bạn.

</div>
