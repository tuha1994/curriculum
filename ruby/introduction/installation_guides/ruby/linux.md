### Hướng dẫn: Cài đặt Ruby trên Linux

#### Bước 1: Cài đặt các bản cập nhật, gói và thư viện

Trước khi có thể cài đặt Ruby, chúng ta cần cài đặt một số gói cơ bản.

1. Chúng ta sẽ dùng terminal để cài đặt tất cả các chương trình.

   Nếu bạn đang dùng Ubuntu hoặc Xubuntu, hãy nhấn <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>T</kbd> để mở terminal (cách này có thể hoạt động hoặc không hoạt động trên các biến thể Ubuntu khác; bạn sẽ phải thử).

   **Mẹo nhanh:** Trên Linux, bạn có thể sao chép từ terminal bằng <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> và dán bằng <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>V</kbd>.

1. Phần còn lại của quá trình cài đặt sẽ diễn ra trong cửa sổ terminal.

   Trước hết, chúng ta cần đảm bảo bản phân phối Linux của bạn đã được cập nhật. Hãy chạy lần lượt từng lệnh sau. Vì các lệnh này sử dụng `sudo`, bạn sẽ phải nhập mật khẩu để chúng có thể chạy. Khi nhập mật khẩu, có thể bạn sẽ không thấy phản hồi nào trên màn hình, nhưng hãy yên tâm rằng mật khẩu vẫn đang được nhập. Sau khi nhập xong mật khẩu, hãy nhấn <kbd>Enter</kbd>.

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

   Khi được nhắc, hãy nhấn <kbd>Y</kbd> rồi nhấn <kbd>Enter</kbd>.

1. Tiếp theo, bạn cần cài đặt một số gói bắt buộc chưa được cài sẵn. Hãy nhớ sao chép và dán lệnh này.

   ```bash
   sudo apt install gcc make libssl-dev libreadline-dev zlib1g-dev libsqlite3-dev libyaml-dev
   ```

   Khi được nhắc, hãy nhấn <kbd>Y</kbd> rồi nhấn <kbd>Enter</kbd>. Có thể bạn sẽ phải nhập mật khẩu sau khi nhấn <kbd>Enter</kbd>, nhưng cũng có thể không.

#### Bước 2: Cài đặt rbenv

Để cài đặt Ruby, chúng ta sẽ sử dụng một công cụ có tên `rbenv`, giúp việc cài đặt và quản lý các phiên bản Ruby trở nên dễ dàng.

1. Trước tiên, bạn cần sao chép kho lưu trữ rbenv:

   ```bash
   git clone https://github.com/rbenv/rbenv.git ~/.rbenv
   ```

1. Chạy lệnh tiếp theo để thiết lập rbenv:

   ```bash
   ~/.rbenv/bin/rbenv init
   ```

   Bây giờ hãy đóng cửa sổ terminal và mở lại một cửa sổ mới để làm mới môi trường.

1. Tiếp theo, bạn cần cài đặt `ruby-build` để hỗ trợ biên dịch các tệp nhị phân Ruby. Hãy chạy các lệnh sau trong terminal để tạo thư mục cho plugin ruby-build, sau đó tải plugin này vào đúng thư mục.

   ```bash
   mkdir -p "$(rbenv root)"/plugins
   git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
   ```

1. Cuối cùng, hãy chạy lệnh sau trong terminal để xác minh rằng `rbenv` đã được cài đặt chính xác:

   ```bash
   rbenv -v
   ```

   Bạn sẽ nhận được kết quả có số phiên bản **tương tự** như sau:

   ```bash
   rbenv 1.2.0-14-gc6cc0a1
   ```

   Nếu hoàn toàn không nhận được số phiên bản nào (bất kỳ kết quả nào không bắt đầu bằng `rbenv 1...`), hãy nhờ trợ giúp trong [máy chủ Discord của chúng tôi](https://discordapp.com/channels/505093832157691914/505093832157691916).

#### Bước 3: Cài đặt Ruby

Cuối cùng cũng đến lúc cài đặt Ruby bằng `rbenv`!

1. Trong terminal, hãy chạy lệnh này:

   ```bash
   rbenv install 3.4.6 --verbose
   ```

   Lệnh này sẽ mất từ 10–15 phút để hoàn tất. Cờ `--verbose` sẽ hiển thị những gì đang diễn ra để bạn biết chắc quá trình không bị treo. Trong lúc chờ cài đặt, bạn có thể xem [video những chú dê nhảy vui nhộn](https://youtu.be/X2CYWg9-2N0) hoặc đi lấy một cốc nước.

   Bạn có thể gặp thông báo lỗi sau:

   ```bash
   ruby-build: definition not found: x.x.x

   See all available versions with `rbenv install --list`.

   If the version you need is missing, try upgrading ruby-build:

     git -C /home/itorja/.rbenv/plugins/ruby-build pull
   ```

   Nếu gặp lỗi này, hãy chạy lệnh được đề xuất:

   ```bash
   git -C "$(rbenv root)"/plugins/ruby-build pull
   ```

1. Sau khi cài đặt Ruby, bạn cần cho rbenv biết phiên bản nào sẽ được sử dụng theo mặc định. Trong terminal, hãy chạy:

   ```bash
   rbenv global 3.4.6
   ```

1. Bây giờ, hãy kiểm tra phiên bản Ruby mà hệ thống đang sử dụng bằng cách chạy:

   ```bash
   ruby -v
   ```

   Lệnh trên sẽ trả về kết quả tương tự như dưới đây, trong đó x đại diện cho phiên bản có sẵn tại thời điểm bạn cài đặt Ruby:

   ```bash
   ruby 3.4.6pxx (20xx-xx-xx revision xxxxx) [x86_64-linux]
   ```

Làm tốt lắm! Hãy tự vỗ vai mình một cái! Phần khó nhất đã hoàn thành, và đã đến lúc chuyển sang bài học tiếp theo!

[Quay lại bài học Cài đặt Ruby](https://www.theodinproject.com/lessons/ruby-installing-ruby#assignment)
