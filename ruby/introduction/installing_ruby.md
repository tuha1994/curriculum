### Giới thiệu

Trước khi bắt đầu học, trước hết chúng ta cần cài đặt Ruby. Đây là phần bạn có thể gặp khá nhiều lỗi.

Trước khi tiếp tục, hãy cùng xem lại một vài cách làm tốt cần ghi nhớ:

- Sao chép và dán các lệnh để tránh gõ sai.
- Làm theo hướng dẫn thật cẩn thận và đừng bỏ qua bất kỳ phần nào.
- **KHÔNG sử dụng `sudo` trừ khi The Odin Project yêu cầu cụ thể.** Không làm theo điều này có thể gây ra rất nhiều rắc rối, và đừng bao giờ chạy với tư cách người dùng `root`. Trong một số trường hợp, bạn có thể thấy một thông báo trong terminal yêu cầu sử dụng `sudo` và cài đặt thứ gì đó bằng `apt`. Tạm thời hãy bỏ qua thông báo đó và làm theo hướng dẫn *của chúng ta*.

Bây giờ, hãy bắt đầu!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề mà bạn sẽ học trong bài này.

- Cài đặt và cập nhật Ruby.
- Thiết lập các phiên bản Ruby cục bộ và toàn cục.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Cài đặt Ruby

1. Tùy thuộc vào hệ điều hành bạn đang sử dụng, hãy làm theo hướng dẫn cài đặt Ruby phù hợp dưới đây:
   - [Linux](https://github.com/TheOdinProject/curriculum/tree/main/ruby/introduction/installation_guides/ruby/linux.md "Ruby installation for Linux")
   - [macOS](https://github.com/TheOdinProject/curriculum/tree/main/ruby/introduction/installation_guides/ruby/macos.md "Ruby installation for macOS")

#### Cập nhật Ruby

Nếu bạn vừa cài đặt Ruby, xin chúc mừng! Có lẽ phiên bản của bạn đã hoàn toàn được cập nhật, nên bạn có thể bỏ qua phần này một cách an toàn. Tuy nhiên, nếu bạn tiếp tục sử dụng đủ lâu, cuối cùng sẽ đến lúc bạn cần cập nhật phiên bản Ruby của mình. May mắn là việc đã cài đặt `rbenv` giúp quá trình này khá đơn giản. Dưới đây là hướng dẫn từng bước để cập nhật phiên bản ruby của bạn:

1. Đi đến thư mục `.rbenv/` trong thư mục chính của bạn.

   ```bash
     cd ~/.rbenv
   ```

1. Tải về các thay đổi mới nhất cho trình quản lý phiên bản ruby của bạn (`rbenv`).

   ```bash
     git pull
   ```

1. Chạy lệnh này.

   ```bash
     ~/.rbenv/bin/rbenv init
   ```

1. Đi đến thư mục plugin `ruby-build`. Đây là công cụ mà `rbenv` sử dụng để biên dịch ruby từ mã nguồn (được viết bằng C).

   ```bash
     cd ~/.rbenv/plugins/ruby-build
   ```

1. Tải về các thay đổi mới nhất cho `ruby-build`.

   ```bash
     git pull
   ```

1. Bây giờ bạn có thể xem danh sách tất cả các phiên bản mới nhất có sẵn để cài đặt.

   ```bash
     rbenv install --list-all
   ```

1. Bây giờ bạn có thể cài đặt phiên bản Ruby mình muốn bằng cách chạy:

   ```bash
     rbenv install x.y.z
   ```

   trong đó trong `x.y.z`, `x` là phiên bản chính, `y` là phiên bản phụ và `z` là bản vá. Cách đánh số này được gọi là [quản lý phiên bản theo ngữ nghĩa](https://semver.org/#summary)

   Ví dụ:

   ```bash
     rbenv install 3.4.6
   ```

#### Thiết lập phiên bản Ruby cục bộ và toàn cục

`rbenv` giúp bạn dễ dàng cài đặt nhiều phiên bản Ruby khác nhau và chuyển đổi giữa chúng. Bạn có thể có một dự án chạy bằng Ruby 3.4 và một dự án khác chạy bằng Ruby 3.3, còn `rbenv` sẽ điều phối việc này cho bạn. Phiên bản Ruby được thiết lập ở cấp dự án được gọi là phiên bản "local".

Để thiết lập phiên bản Ruby cục bộ, hãy chạy:

```bash
rbenv local x.y.z
```

Lệnh này tạo một tệp `.ruby-version` để lưu phiên bản cục bộ. Để kiểm tra phiên bản Ruby cục bộ của dự án/thư mục hiện tại, hãy chạy:

```bash
rbenv local
```

Bạn cũng có thể cấu hình phiên bản Ruby mặc định. Phiên bản này được gọi là phiên bản "global". Để thiết lập phiên bản Ruby mặc định toàn cục, hãy chạy:

```bash
rbenv global x.y.z
```

Để kiểm tra phiên bản ruby toàn cục hiện tại, hãy chạy:

```bash
rbenv global
```

#### Phần bổ sung

Nếu bạn sử dụng Visual Studio Code làm IDE, bạn có thể cài đặt tiện ích mở rộng "Ruby LSP", tiện ích này sẽ cung cấp khả năng tô sáng ngữ nghĩa và hỗ trợ định dạng.

Sử dụng tiện ích mở rộng này là tùy chọn, nhưng cài đặt rất nhanh; hãy đi đến thẻ "Extensions" trong VS Code (<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>), tìm kiếm "Ruby LSP" và nhấp vào nút cài đặt ở tiện ích đầu tiên. Xin chúc mừng, tiện ích mở rộng đã được cài đặt.

Các tính năng quan trọng nhất do Ruby LSP cung cấp sẽ hoạt động ngay mà không cần cấu hình thêm. Tuy nhiên, tiện ích này có thể làm phiền bạn về việc sử dụng thiết lập monorepo, thiếu lockfile hoặc rubocop - hiện tại bạn có thể chọn "Don't show again". Chúng ta sẽ giới thiệu các nội dung này sau.

</div>
