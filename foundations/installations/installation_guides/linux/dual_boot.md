### Hướng dẫn: Khởi động kép Ubuntu và Windows

Khởi động kép cung cấp hai hệ điều hành trên máy tính của bạn, giữa chúng bạn có thể chuyển đổi bằng cách khởi động lại đơn giản. Một hệ điều hành sẽ không sửa đổi hệ điều hành kia trừ khi bạn chủ động yêu cầu. Có rất nhiều biến thể Ubuntu, và dù chương trình học hỗ trợ các biến thể Ubuntu chính thức, hướng dẫn này tập trung vào việc cài đặt Ubuntu Desktop chính thức chạy trên GNOME. Nếu muốn cài đặt một biến thể khác, hướng dẫn này vẫn có thể áp dụng. Chỉ cần chú ý đến hướng dẫn của trình cài đặt dành cho biến thể đó.

Trước khi tiếp tục, hãy sao lưu mọi dữ liệu quan trọng và chuẩn bị cách thức để yêu cầu trợ giúp. Nếu bị lạc hướng, lo lắng hoặc mắc kẹt, chúng tôi sẵn sàng hỗ trợ trong [phòng chat hỗ trợ kỹ thuật Odin](https://discordapp.com/channels/505093832157691914/514204667245363200). Hãy vào chào "Hi"!

### Bước 0: BitLocker

Nếu bạn dùng Windows Pro (hoặc bất kỳ phiên bản nào được liệt kê trong [Tổng quan BitLocker chính thức của Microsoft](https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/#windows-edition-and-licensing-requirements)), ổ đĩa chính của bạn có thể đã bật [BitLocker](https://support.microsoft.com/en-us/windows/bitlocker-drive-encryption-76b92ac9-1040-48d6-9f5f-d14b3c5fa178). BitLocker là phần mềm mã hóa ổ đĩa của Microsoft. Nếu ổ đĩa bạn muốn cài Ubuntu đã bật BitLocker thì ban đầu bạn sẽ không thể cài đặt.

Có một vài cách giải quyết tình huống này, nhưng chúng ta sẽ chọn cách đơn giản nhất: tạm thời tắt BitLocker.

[Hướng dẫn chính thức của Ubuntu về cách tắt BitLocker trong Windows](https://ubuntu.com/desktop/docs/en/latest/how-to/turn-off-bitlocker-in-windows/#turn-off-bitlocker-in-windows) sẽ hướng dẫn bạn tắt BitLocker trên cả Windows 11 và các bản Windows cũ hơn.

Nếu cài Ubuntu trên một ổ đĩa/phân vùng riêng không bật BitLocker, bạn sẽ không gặp vấn đề khi khởi động kép và không cần tắt BitLocker.

### Bước 1: Tải Ubuntu

Trước tiên, bạn cần tải phiên bản Ubuntu muốn cài trên máy tính. Ubuntu có nhiều phiên bản ("biến thể"), nhưng chúng tôi đề xuất bản desktop tiêu chuẩn [Ubuntu](https://releases.ubuntu.com/24.04/). Hãy tải phiên bản 64-bit của Ubuntu. Nếu không chắc nên chọn phiên bản nào trên trang tải xuống, bạn nên chọn phiên bản Hỗ trợ dài hạn (LTS) 24.04 (Noble Numbat). Bạn có thể muốn chọn bản phát hành mới hơn nếu có, nhưng cộng đồng Odin Project đã thử nghiệm kỹ phiên bản này, nên đây là lựa chọn đáng tin cậy nhất cho chương trình học này.

### Bước 2: Tạo ổ flash có thể khởi động

Tiếp theo, làm theo [bước 3 trong hướng dẫn Cài đặt Ubuntu Desktop chính thức](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick) để tạo ổ flash có thể khởi động, nhờ đó bạn có thể cài Ubuntu lên ổ cứng. Bạn cần một USB còn trống ít nhất 12GB. Hãy sao lưu dữ liệu trên USB vì bước này sẽ xóa toàn bộ dữ liệu trên đó.

### Bước 3: Cài đặt Ubuntu

#### Bước 3.1: Khởi động từ ổ flash

Trước tiên, bạn cần khởi động Ubuntu từ ổ flash. Các bước chính xác có thể khác nhau, nhưng nhìn chung bạn cần làm như sau:

1. Cắm ổ flash vào máy tính.
1. Khởi động lại máy tính.
1. Chọn ổ flash làm thiết bị khởi động thay cho ổ cứng.

Ví dụ, trên máy Dell, bạn cần cắm ổ flash, khởi động lại máy tính và nhấn phím F12 trong lúc máy vừa bắt đầu khởi động để mở menu khởi động. Từ đó, bạn có thể chọn khởi động từ ổ flash. Máy tính của bạn có thể không giống hệt như vậy, nhưng Google có thể giúp bạn tìm hiểu.

#### Bước 3.2: Cài đặt Ubuntu

Sau khi vào được, trình cài đặt Ubuntu sẽ hiển thị để bạn chọn ngôn ngữ, cài đặt hỗ trợ tiếp cận, bố cục bàn phím và kết nối wifi.

Nếu muốn dùng thử Ubuntu trên ổ flash, hãy nhấp vào **Try Ubuntu**. Khi sẵn sàng cài đặt, hãy nhấp đúp vào biểu tượng **Install Ubuntu 24.04 LTS** trên màn hình chính.

Cài đặt Ubuntu là lúc những thay đổi thực sự bắt đầu diễn ra trên máy tính. Các cài đặt mặc định hầu hết đều phù hợp, nhưng hãy nhớ chọn **Install Ubuntu alongside Windows** và đổi dung lượng ổ đĩa phân bổ cho Ubuntu thành 30 GB (hoặc nhiều hơn nếu có thể).

Để xem hướng dẫn từng bước, hãy làm theo [bước 6 trong hướng dẫn Cài đặt Ubuntu Desktop chính thức](https://ubuntu.com/tutorials/install-ubuntu-desktop#6-type-of-installation).

### Intel RST (Rapid Storage Technology)

Nếu gặp lỗi yêu cầu tắt **Intel RST** khi đang cố cài Ubuntu, hãy làm theo [cách khắc phục để cài Ubuntu trên hệ thống Intel RST](https://askubuntu.com/questions/1233623/workaround-to-install-ubuntu-20-04-with-intel-rst-systems/1233644#1233644), cụ thể là **Choice #2**. Quy trình này buộc Windows khởi động vào chế độ an toàn sau khi bạn chuyển trình điều khiển lưu trữ của bo mạch chủ để tương thích với Ubuntu. Sau khi Windows khởi động, chế độ an toàn bị ép buộc sẽ được tắt và bạn có thể thử cài Ubuntu lại.

[Quay lại bài học Cài đặt](https://www.theodinproject.com/lessons/foundations-installations#os-installation)