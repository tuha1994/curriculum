### Hướng dẫn: Cài đặt Ruby trên macOS

#### Bước 1: Cài đặt các gói và thư viện

Trước khi có thể cài đặt Ruby, chúng ta cần cài đặt một số gói cơ bản. Chúng ta sẽ dùng terminal để cài đặt tất cả các chương trình.

1. Trong thư mục Applications, tìm "Utilities" rồi nhấp đúp vào "Terminal". Ngoài ra, bạn có thể dùng Spotlight (<kbd>Cmd</kbd> + <kbd>Space</kbd>) hoặc Launchpad rồi nhập "Terminal".

   Phần hướng dẫn còn lại sẽ được thực hiện trong cửa sổ terminal này; nếu bạn đã làm theo toàn bộ hướng dẫn Foundations, bạn hẳn đã hoàn thành bước 1.2 và 1.3.

1. Điều đầu tiên bạn cần cài đặt là Xcode, một chương trình do Apple cung cấp để lập trình. Xcode sẽ cài đặt nhiều chương trình cần thiết cho Ruby và Git, đồng thời quá trình này có thể mất từ 10–15 phút.

   Hãy chạy lệnh sau trong terminal (có thể bạn sẽ cần nhấp vào "Install" khi được nhắc):

   ```bash
   xcode-select --install
   ```

1. Chương trình tiếp theo bạn cần cài đặt là [Homebrew](https://brew.sh/), công cụ giúp bạn dễ dàng cài đặt những chương trình khác cần dùng. Trong terminal, hãy chạy lệnh sau:

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

   Bạn sẽ được yêu cầu nhập mật khẩu. Khi nhập mật khẩu, có thể bạn sẽ không thấy phản hồi nào trên màn hình, nhưng hãy yên tâm rằng mật khẩu vẫn đang được nhập. Sau khi nhập xong mật khẩu, hãy nhấn <kbd>Enter</kbd>.

1. Để kiểm tra việc cài đặt Homebrew, chúng ta có thể chạy:

   ```bash
   which brew
   ```

   Nếu lệnh xuất ra một đường dẫn nào đó, bạn có thể tiếp tục! Nhưng nếu terminal hiển thị ```brew not found```, hãy làm theo [hướng dẫn macOS trong bài học thiết lập git](https://www.theodinproject.com/lessons/foundations-setting-up-git) để cài đặt homebrew.

Chúc mừng! Bạn đã cài đặt xong các thành phần tiên quyết!

### Bước 2: Cài đặt rbenv

Bây giờ bạn đã sẵn sàng cài đặt Ruby. Chúng ta sẽ sử dụng một công cụ có tên `rbenv`, giúp việc quản lý các phiên bản Ruby trở nên dễ dàng.

1. Trước tiên, hãy cài đặt `ruby-build`:

   ```bash
   brew install ruby-build
   ```

   `ruby-build` sẽ cho phép bạn cài đặt phiên bản Ruby mà mình muốn.

1. Để cài đặt `rbenv`, hãy chạy lệnh sau trong terminal:

   ```bash
   brew install rbenv
   ```

   Sau đó, chạy lệnh này:

   ```bash
   rbenv init
   ```

1. Để xác nhận mọi thứ đã diễn ra đúng như mong đợi, hãy đóng terminal và mở lại một cửa sổ mới, sau đó chạy:

   ```bash
   rbenv -v
   ```

   Bạn sẽ nhận được kết quả có số phiên bản **tương tự** như sau:

   ```bash
   rbenv 1.2.0-14-gc6cc0a1
   ```

   Nếu hoàn toàn không nhận được số phiên bản nào (bất kỳ kết quả nào không bắt đầu bằng `rbenv 1...`), hãy nhờ trợ giúp trong [máy chủ Discord của chúng tôi](https://discordapp.com/channels/505093832157691914/505093832157691916).

#### Bước 3: Cài đặt Ruby

Bây giờ (cuối cùng) chúng ta có thể cài đặt Ruby! Hiện tại, chương trình học của chúng ta sử dụng phiên bản 3.4.6, phiên bản này sẽ cho phép bạn hoàn thành các tài liệu và nội dung của lộ trình mà không gặp lỗi. Khi cần thiết, chúng tôi sẽ cập nhật tài liệu để phù hợp với các phiên bản mới hơn. Không chần chừ thêm nữa, hãy bắt đầu thôi!

1. Trước tiên, hãy nâng cấp `ruby-build`:

   ```bash
   brew upgrade ruby-build
   ```

1. Bây giờ chúng ta đã sẵn sàng cài đặt phiên bản Ruby mong muốn:

   ```bash
   rbenv install 3.4.6 --verbose
   ```

   Lệnh này sẽ mất từ 10–15 phút để hoàn tất. Cờ `--verbose` sẽ hiển thị những gì đang diễn ra để bạn biết chắc quá trình không bị treo. Trong lúc chờ cài đặt, bạn có thể xem [video những chú dê nhảy vui nhộn](https://www.youtube.com/watch?v=X2CYWg9-2N0) hoặc đi lấy một cốc nước.

1. Sau khi cài đặt Ruby, bạn cần cho rbenv biết phiên bản nào sẽ được sử dụng theo mặc định. Trong terminal, hãy chạy:

   ```bash
   rbenv global 3.4.6
   ```

1. Bây giờ, hãy kiểm tra lại để chắc chắn mọi thứ đã hoạt động bằng cách xác nhận phiên bản Ruby hiện đang được sử dụng là 3.4.6:

   ```bash
   ruby -v
   ```

   Bạn sẽ nhận được kết quả có số phiên bản **tương tự** như sau:

   ```bash
   ruby 3.4.6pxx (20xx-xx-xx revision xxxxx) [x86_64-darwin18]
   ```

   Nếu không thấy kết quả như trên, hãy đóng rồi mở lại cửa sổ terminal và chạy lại lệnh.

Làm tốt lắm! Hãy tự vỗ vai mình một cái! Phần khó nhất đã hoàn thành, và đã đến lúc chuyển sang bài học tiếp theo!

[Quay lại bài học Cài đặt Ruby](https://www.theodinproject.com/lessons/ruby-installing-ruby#assignment)
