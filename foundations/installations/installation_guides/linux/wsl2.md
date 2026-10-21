### Hướng dẫn: Thiết lập WSL2

Sử dụng WSL2 là một cách nhanh chóng và dễ dàng để bắt đầu làm quen với Linux, cho phép bạn chạy một bản phân phối Linux ngay trong Windows. WSL2 có trên Windows 10 phiên bản 2004 trở lên (Build 19041 trở lên) và Windows 11.

Nói rõ hơn: bạn sẽ sử dụng một hệ điều hành khác; đây không phải là cách để tránh dùng Linux. Do cách WSL2 tích hợp với Windows, công cụ này thường khiến người mới học khá bối rối. Nếu muốn tách biệt rõ ràng giữa Windows và Linux để dễ theo dõi chương trình học hơn, hãy dùng Máy ảo.

<div class="lesson-note" markdown="1">

#### Hướng dẫn WSL2 và Linux

Vì WSL2 là một bản phân phối Linux đầy đủ, gần như mọi điều chương trình học hướng dẫn về Linux cũng áp dụng được cho WSL2. Trong các bài học sau, khi có hướng dẫn khác nhau tùy hệ điều hành, bạn nên làm theo hướng dẫn dành cho Linux, trừ khi bài học có hướng dẫn riêng cho WSL2.

</div>

### Bước 1: Cài đặt

#### Bước 1.1: Cài đặt WSL2

1. Mở PowerShell ở chế độ quản trị viên bằng cách tìm ứng dụng này trong danh sách chương trình, nhấp chuột phải vào kết quả đầu tiên rồi chọn chạy với tư cách quản trị viên. Có thể bạn sẽ thấy lời nhắc hỏi có cho phép Windows Powershell thực hiện thay đổi trên thiết bị hay không: hãy chọn có.
1. Nhập lệnh sau

   ```powershell
   wsl --install
   ```

1. Sau vài phút, máy sẽ yêu cầu bạn khởi động lại; hãy thực hiện việc đó.
1. Bạn sẽ thấy một cửa sổ Powershell mở ra, yêu cầu nhập tên người dùng và mật khẩu. Tên người dùng nên viết thường, nhưng bạn có thể chọn tên phù hợp với mình. Bạn cũng cần nhập một mật khẩu mới.
1. Khi nhập mật khẩu, bạn có thể nhận thấy không có phản hồi trực quan nào. Đây là tính năng bảo mật tiêu chuẩn trong Linux và cũng sẽ xảy ra trong mọi trường hợp sau này khi bạn cần nhập mật khẩu. Chỉ cần gõ mật khẩu rồi nhấn <kbd>Enter</kbd>.

#### Bước 1.2.1: Cài đặt Windows Terminal (chỉ Windows 10)

Windows Terminal là một ứng dụng terminal giúp bạn dễ dàng tùy chỉnh và chạy các terminal hơn, đồng thời hỗ trợ nhiều tab, mỗi tab có thể chạy một terminal khác nhau. [Cài đặt Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/install) bằng tùy chọn cài đặt trực tiếp.

#### Bước 1.2.2: Đặt WSL2 làm mặc định (Tùy chọn)

Trừ khi thường xuyên sử dụng các terminal khác trên máy tính, chúng tôi khuyên bạn đặt WSL2 làm chương trình terminal mặc định khi mở Windows Terminal.

1. Mở Windows Terminal bằng cách tìm terminal trong danh sách chương trình.
1. Nhấp vào mũi tên thả xuống bên cạnh nút tab mới (ở đầu cửa sổ), rồi chọn Settings.
1. Bạn sẽ thấy tùy chọn Default Profile cùng một menu thả xuống bên cạnh.
1. Trong menu thả xuống, chọn Ubuntu.
1. Nhấp save ở cuối trang.

### Bước 2: Mở WSL2

Trên Windows có ba cách chính để mở WSL2.

- Nếu đã đặt Windows Terminal mở terminal Ubuntu theo mặc định, bạn có thể bắt đầu một phiên WSL2 mới bằng cách mở ứng dụng terminal.
- Bạn có thể mở Windows Terminal, nhấp vào mũi tên thả xuống bên cạnh nút tab mới (ở đầu cửa sổ), rồi chọn Ubuntu.
- Nếu tìm Ubuntu trong thanh tìm kiếm ứng dụng, bạn sẽ thấy một ứng dụng có tên Ubuntu; hãy mở ứng dụng đó để bắt đầu một phiên terminal mới.

Tất cả các lựa chọn dưới đây đều hợp lệ và sẽ mở cùng một chương trình:

![Kết quả tìm kiếm Ubuntu trong Start Menu của Windows](../../installations/imgs/ubuntu.png)

![Kết quả tìm kiếm WSL trong Start Menu của Windows](../../installations/imgs/wsl.png)

<div class="lesson-note lesson-note--tip" markdown="1">

#### Bảng màu terminal

Khi mở WSL2 bằng Windows Terminal, bạn có thể nhận thấy cửa sổ có bảng màu và biểu tượng khác so với khi mở terminal thông qua Ubuntu trong danh sách chương trình. Đó là vì Windows Terminal có bảng màu mặc định cho Ubuntu nhằm mô phỏng giao diện của một terminal Ubuntu thực. Khác biệt này chỉ mang tính thẩm mỹ, không có khác biệt thực tế nào giữa hai cách mở.

</div>

<div class="lesson-note lesson-note--warning" markdown="1">

#### /mnt/c không phải Linux!

Khi mở terminal WSL2, hãy bảo đảm bạn không thấy `/mnt/c` ở đầu dòng. `/mnt/c` là nơi bản cài đặt Windows của bạn nằm trong WSL2, và việc thao tác bất cẩn ở đó có thể gây ra hậu quả ngoài ý muốn.

</div>

[Quay lại bài học Cài đặt](https://www.theodinproject.com/lessons/foundations-installations#os-installation)