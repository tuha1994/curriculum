### Hướng dẫn: Cài đặt máy ảo

Cài đặt Máy ảo (VM) là cách dễ dàng và đáng tin cậy nhất để bắt đầu tạo môi trường phát triển web. VM là một máy tính được mô phỏng hoàn chỉnh, chạy bên trong Hệ điều hành (OS) hiện tại của bạn, chẳng hạn như Windows. Nhược điểm chính của VM là có thể chạy chậm vì về cơ bản bạn đang chạy hai máy tính cùng lúc. Chúng ta sẽ thực hiện một vài điều để cải thiện hiệu năng.

### Bước 1: Tải VirtualBox và Xubuntu

Cài đặt VM là một quy trình đơn giản. Hướng dẫn này sử dụng chương trình VirtualBox của Oracle để tạo và chạy VM. Đây là chương trình mã nguồn mở, miễn phí và dễ sử dụng. Còn mong gì hơn nữa? Bây giờ hãy đảm bảo chúng ta đã tải xuống mọi thứ và sẵn sàng cài đặt.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Chỉ sử dụng VM cho The Odin Project

Sau khi hoàn tất các hướng dẫn này, **bạn được yêu cầu làm việc hoàn toàn trong VM.** Phóng to cửa sổ, thêm các màn hình ảo nếu có, mở Internet Browser trong **Whisker Menu** ![Biểu tượng Whisker Menu hình chuột xanh-trắng](../../installations/imgs/00_whisker_icon.png) ở góc trên bên trái desktop. Mọi nhiệm vụ bạn thực hiện cho The Odin Project đều phải diễn ra trong VM. Nếu cảm thấy đã hiểu rõ sau một thời gian sử dụng VM và/hoặc muốn cải thiện trải nghiệm, chúng tôi khuyến nghị khởi động kép Ubuntu; hướng dẫn nằm ở bên dưới.

</div>

#### Bước 1.1: Tải VirtualBox

[Tải VirtualBox cho máy chủ Windows phiên bản 7.2.4 (phiên bản cuối cùng được biết là hoạt động)](https://download.virtualbox.org/virtualbox/7.2.4/VirtualBox-7.2.4-170995-Win.exe).

#### Bước 1.2: Tải Xubuntu

Có hàng nghìn bản phân phối Linux, nhưng Ubuntu chắc chắn là một trong những bản phổ biến và thân thiện với người dùng nhất. Khi cài Linux trên VM, chúng tôi khuyến nghị tải Xubuntu, một biến thể Ubuntu nhẹ. Xubuntu sử dụng cùng phần mềm nền tảng với Ubuntu nhưng có môi trường desktop cần ít tài nguyên máy tính hơn, vì vậy rất phù hợp với máy ảo. Nhấp vào nút `Direct Downloads` trên [trang phát hành Xubuntu 24.04](https://xubuntu.org/release/24.04/), tìm vị trí máy chủ gần bạn nhất rồi nhấp vào đó. Có một vài tệp được liệt kê; hãy tải tệp kết thúc bằng `.iso`, cũng là tệp lớn nhất (khoảng 4 gigabyte). Nếu tốc độ tải chậm, bạn có thể tải [trực tiếp Xubuntu `.iso` từ máy chủ image Ubuntu chính thức](https://cdimage.ubuntu.com/xubuntu/releases/noble/release/), vì liên kết trước trỏ đến một máy chủ đặt tại Hoa Kỳ.

Nếu không chắc nên chọn phiên bản nào trên trang tải xuống, bạn nên chọn phiên bản Hỗ trợ dài hạn (LTS) 24.04 (Noble Numbat). Bạn có thể muốn chọn bản phát hành mới hơn, nhưng cộng đồng Odin Project đã thử nghiệm kỹ phiên bản này, nên đây là lựa chọn đáng tin cậy nhất cho chương trình học này.

### Bước 2: Cài đặt VirtualBox và thiết lập Xubuntu

#### Bước 2.1: Cài đặt VirtualBox

Cài đặt VirtualBox rất đơn giản. Bạn không cần nhiều kiến thức kỹ thuật; quy trình cũng giống như cài đặt bất kỳ chương trình nào khác trên máy tính Windows. Nhấp đúp vào tệp VirtualBox đã tải xuống sẽ bắt đầu quá trình cài đặt.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Microsoft Visual C++ 2019

Nếu nhận được lỗi yêu cầu Microsoft Visual C++ 2019 Redistributable Package, bạn có thể tìm gói này trên [trang Microsoft Learn chính thức](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022). Nhiều khả năng bạn cần phiên bản có kiến trúc `X64` (nghĩa là 64-bit) - hãy tải xuống, cài đặt rồi thử cài VirtualBox lại.

</div>

Trong quá trình cài đặt, bạn sẽ thấy nhiều tùy chọn. Chúng tôi khuyên bạn bỏ Python Support vì không cần đến nó, bằng cách nhấp vào biểu tượng ổ đĩa có mũi tên và chọn **Entire feature will be unavailable**:

![Tùy chọn Python nằm ở cuối danh sách](../../installations/imgs/01_turn_off_python.png)

Sau khi tắt, cửa sổ cài đặt của bạn sẽ trông như sau:

![Tùy chọn Python có dấu "X" màu đỏ tươi](../../installations/imgs/02_c_install.png)

Bạn không bắt buộc phải loại trừ Python Support. Đây là gói tùy chọn cho phép tự động hóa máy ảo bằng VirtualBox, nằm ngoài phạm vi chương trình học này; chúng tôi chỉ không muốn bạn cài đặt nhiều hơn mức cần thiết.

Hãy đảm bảo cài ứng dụng vào ổ `C:`, vì cài ở nơi khác có thể gây lỗi. Bản thân máy ảo có thể được cài ở bất kỳ đâu, chúng ta sẽ nói về việc đó ngay sau đây.
Khi phần mềm đang cài đặt, thanh tiến trình có thể trông như bị kẹt; chỉ cần chờ cho đến khi hoàn tất.

#### Bước 2.2: Tạo máy ảo mới

Bây giờ VirtualBox đã được cài đặt, hãy khởi chạy chương trình. Khi mở lên, bạn sẽ thấy màn hình bắt đầu. Nhấp vào nút **New** để tạo một hệ điều hành ảo.

![Màn hình bắt đầu của VirtualBox](../../installations/imgs/03_start_screen.png)

Đặt tên là **Xubuntu-TOP**. Nếu muốn cài VM ở nơi khác thay vì vị trí mặc định trên ổ `C:`, hãy thay đổi tương ứng trong tùy chọn **Folder**. Đây là nơi lưu đĩa ảo, vì vậy hãy đảm bảo còn ít nhất 30GB. Trong **ISO Image**, chọn **Other** - một cửa sổ sẽ mở ra để bạn tìm tệp `.iso` trên máy tính. Tệp này thường nằm trong thư mục `Downloads`.

**Bỏ chọn "Proceed with Unattended Installation".** Tính năng này không hoạt động và bạn sẽ không thể kiểm soát quá trình cài đặt. Nếu vô tình bật trước khi cài, hãy xóa VM rồi thử lại.

Các trường sẽ tự điền sau khi bạn nhập VM Name.

![Màn hình máy ảo mới](../../installations/imgs/04_install_start.png)

**Chưa** nhấp vào Finish! Chúng ta chưa xong. Tiếp tục thiết lập bằng cách nhấn vào mục **Specify virtual hardware** rồi làm theo các bước tiếp theo.

#### Bước 2.3: Chỉ định phần cứng ảo

Bây giờ bạn sẽ thấy một cửa sổ tương tự như sau:

![Màn hình chỉ định phần cứng ảo](../../installations/imgs/05_specify_virtual_hardware.png)

Trong phần **Hardware** của quá trình cài đặt, hãy đặt **Base Memory** ít nhất là 2048 MB hoặc cao hơn nếu có thể - giới hạn trên là một nửa tổng RAM, nhưng 4096 MB với các cài đặt chúng tôi đề xuất sẽ mang lại trải nghiệm mượt mà.

> Ví dụ, nếu có 8 GB (tương ứng 8192 MB) RAM, bạn có thể cấp tối đa 4096 MB (1024 MB đến 1 GB) cho hệ điều hành của VM. Bạn có thể Google cách kiểm tra lượng RAM còn trống nếu chưa biết. Nếu VM hơi chậm, hãy thử cấp thêm bộ nhớ!

<div class="lesson-note lesson-note--tip" markdown="1">

#### Chuyển đổi gigabyte sang megabyte

Khó chuyển đổi Gigabyte (GB) sang Megabyte (MB)? 1 GB RAM bằng 1024 MB. Vì vậy, có thể nói **8 GB = 8 x 1024 = 8192 MB.**

</div>

Đối với **Processors**, hãy đặt ở mức 2 và không cao hơn. Bỏ chọn **Use EFI**.

Chúng ta vẫn chưa sẵn sàng nhấn nút Finish. Nhấp vào mục **Specify virtual hard disk accordion** để tiếp tục.

#### Bước 2.4: Chỉ định ổ cứng ảo

Bây giờ, hãy giữ nguyên mọi cài đặt ngoại trừ **Disk Size**; chúng tôi khuyến nghị cấp cho VM **ít nhất 30GB** dung lượng. Lưu ý rằng đĩa này sẽ được tạo trong thư mục bạn đã chỉ định ở bước đầu tiên của quá trình tạo VM, nhưng về sau bạn vẫn có thể di chuyển và tăng dung lượng đĩa nếu cần.

![Màn hình chỉ định ổ cứng ảo](../../installations/imgs/06_specify_virtual_hard_disk.png)

Bây giờ chúng ta đã sẵn sàng nhấn **Finish**. Hãy nhấn nút đó.

#### Bước 2.5: RAM video ảo

Cần thực hiện thêm một bước cuối trước khi khởi động VM và bắt đầu cài đặt. Hãy nhấp vào nút **Settings**.

![Màn hình bắt đầu với phần settings được đánh dấu](../../installations/imgs/07_start_screen_settings.png)

Trang Settings của VM sẽ mở ra và trông như hình bên dưới.

![Màn hình Settings](../../installations/imgs/08_settings_screen.png)

Hãy nhấp vào nút **Display**. Bạn sẽ chuyển đến màn hình cài đặt Display. Trước tiên, hãy bật **3D Acceleration**, sau đó tăng **Video Memory** lên mức tối đa. Bạn cũng có thể tăng số lượng Virtual Monitors nếu có nhiều màn hình vật lý. Chúng tôi khuyên nên làm việc này sau, khi bạn đã quen với VM.

![Màn hình cài đặt Display](../../installations/imgs/09_display_settings_screen.png)

Nhấp **OK**. Bây giờ chúng ta đã sẵn sàng bắt đầu cài đặt.

### Bước 3: Bắt đầu cài đặt Xubuntu

Bạn sẽ quay lại màn hình bắt đầu. Khi VM được chọn, hãy nhấp vào nút **Start**. Nếu vô tình mở menu thả xuống của nút Start, bạn có thể nhấp **Start with GUI**, hoặc nhấp lại nút Start. Một cách khác để khởi động VM là nhấp đúp vào chính VM đó!

![Màn hình bắt đầu với nút start được đánh dấu](../../installations/imgs/10_start_screen_start_button.png)

Một cửa sổ mới sẽ xuất hiện với một số hình ảnh cho biết bạn đang tải Oracle Virtual Machine. Sau đó, cửa sổ chuyển sang màn hình bắt đầu Xubuntu có tên GRUB Boot Menu. Theo mặc định, bạn có 30 giây để dùng phím mũi tên chọn một tùy chọn rồi xác nhận bằng Enter. Thông thường tùy chọn mặc định là tùy chọn cần dùng; trong trường hợp này là **Try or Install Xubuntu**. Nếu chọn nhầm, hãy kết thúc VM bằng cách nhấp File rồi Close, hoặc nhấn nút X, sau đó chọn **Power off the machine**.

Đúng vậy, lúc này màn hình sẽ nhỏ. Chúng ta sẽ sửa điều đó sau.

![Màn hình GRUB Boot](../../installations/imgs/11_grub.png)

Sau đó, một số dòng chữ trong console sẽ xuất hiện rồi biến mất, và cuối cùng bạn sẽ đến màn hình desktop Xubuntu.

Đây là một môi trường tạm thời có tên Live Desktop. Mọi thay đổi bạn thực hiện ở đây (ngoại trừ việc cài hệ điều hành lên VM) đều không vĩnh viễn.

Để tiếp tục, hãy nhấp đúp vào biểu tượng desktop **Install Xubuntu**.

![Desktop Xubuntu Live](../../installations/imgs/12_xubuntu_live_desktop.png)

Một cửa sổ khác có tiêu đề *Welcome to Xubuntu* sẽ mở ra. Chọn ngôn ngữ rồi nhấp **Next**.

![Welcome to Xubuntu](../../installations/imgs/13_choose_your_language.png)

Màn hình tiếp theo yêu cầu bạn *Select your keyboard layout*. Bạn có thể nhập vào ô nhập liệu để kiểm tra và đảm bảo lựa chọn chính xác. Hãy làm vậy rồi nhấp **Next**.

![Select your keyboard layout](../../installations/imgs/14_select_keyboard_layout.png)

Màn hình tiếp theo yêu cầu bạn cấu hình cài đặt để *Connect to the internet*. Nếu chưa được chọn, hãy chọn **Use wired connection**. VirtualBox được cấu hình để sử dụng thiết bị mạng của máy chủ, vì vậy VM nhận biết bạn đang dùng mạng có dây hay không dây **không quan trọng**; đối với VM, về cơ bản bạn luôn được kết nối có dây với internet.

Nếu không thể chọn **Use wired connection**, hãy chạy lại trình cài đặt VirtualBox và chọn **repair**.

> **Lưu ý cho người dùng VPN**: Nếu không thể tiếp tục cài đặt hoặc chạy cập nhật vì VM dường như không kết nối được internet **và** bạn đang dùng VPN, hãy cân nhắc đưa VM vào danh sách cho phép, dùng reverse split tunnel hoặc tạm thời tắt VPN hoàn toàn. Nếu split tunnel không hoạt động, hãy thử đặt VM Network Adapter thành *Bridged Adapter* rồi thử lại.

![Connect to the internet](../../installations/imgs/15_connect_to_the_internet.png)

Màn hình tiếp theo hỏi bạn *How would you like to install Xubuntu?* Một lần nữa, chúng ta giữ tùy chọn mặc định **Interactive installation** vì chưa chuẩn bị tệp cấu hình và cũng không muốn dùng tệp này trong trường hợp hiện tại. Tính năng này phù hợp hơn với các chuyên gia CNTT đang thiết lập không gian làm việc cho nhân viên, hoặc người muốn dựng homelab và có một mẫu cài đặt OS hoàn chỉnh. Tính năng này nằm ngoài phạm vi chương trình học, nên hãy nhấp **Next** để tiếp tục.

![Type of installation](../../installations/imgs/16_type_of_installation.png)

Màn hình tiếp theo hỏi *What apps would you like to install to start with?*; một lần nữa, chúng ta muốn giữ tùy chọn mặc định gồm tất cả ứng dụng. Nếu dùng VM làm công cụ học tập, bạn nên có đầy đủ tính năng. Nhấp **Next** để tiếp tục.

![Applications](../../installations/imgs/17_applications.png)

Màn hình tiếp theo hỏi bạn có muốn *Install recommended proprietary software?* hay không; câu trả lời là có. Chọn cả hai hộp kiểm rồi nhấp **Next** để tiếp tục.

![Optimize your computer](../../installations/imgs/18_optimize_your_computer.png)

Màn hình tiếp theo hỏi *How do you want to install Xubuntu?* Trong trường hợp này, chúng ta lại chọn tùy chọn mặc định **Erase disk and install Xubuntu**. Hành động này an toàn và không tác động đến ổ cứng/đĩa của máy chủ vì nó được ảo hóa. Cài đặt thủ công nằm ngoài phạm vi chương trình học, nhưng nếu muốn tìm hiểu sâu hơn về sau thì đây là lựa chọn đáng cân nhắc. Khi sẵn sàng, hãy nhấp **Next**.

![Disk setup](../../installations/imgs/19_disk_setup.png)

Màn hình tiếp theo yêu cầu bạn *Create your account* bằng cách nhập tên và mật khẩu. Tên máy tính và tên người dùng được tự động tạo từ tên bạn nhập, nhưng bạn có thể tùy chỉnh theo ý muốn bằng các ký tự hợp lệ. Bạn nhất định nên đặt mật khẩu, chọn một mật khẩu có thể nhớ và sẽ thường xuyên sử dụng, rồi nhấp **Next**.

![Create your account](../../installations/imgs/20_create_your_account.png)

Màn hình tiếp theo yêu cầu bạn *Select your timezone*. Nhập vị trí hoặc múi giờ vào các ô tương ứng rồi nhấp **Next**.

![Select your timezone](../../installations/imgs/21_select_your_timezone.png)

Màn hình tiếp theo yêu cầu bạn *Review your choices* và hoàn tất cài đặt. Thông tin hiển thị trong cửa sổ có thể hơi khó hiểu lúc này, nhưng sẽ phản ánh những lựa chọn chúng ta đã thực hiện trong quá trình này. Khi sẵn sàng chờ một lúc, hãy nhấp nút **Install** rồi đi lấy thêm cà phê.

![Ready to Install](../../installations/imgs/22_ready_to_install.png)

Một vài màn hình tiếp theo sẽ tự động xuất hiện trong quá trình cài đặt. Bạn không thể làm gì nhiều ngoài việc chờ.

![Installer progress](../../installations/imgs/23_installation_in_progress.png)

Khi đến màn hình này, điều đó cho biết quá trình cài đặt đã hoàn tất thành công. Bây giờ bạn được yêu cầu thoát khỏi môi trường live desktop để khởi động hệ điều hành Xubuntu mới cài trên VM. Nhấp **Restart now** để tiếp tục. Nếu đã nhấp Continue testing nhưng muốn tiếp tục, hãy nhấp ![Biểu tượng Whisker](../../installations/imgs/00_whisker_icon.png), nhấp Log Out rồi nhấp Restart. Sau khi khởi động lại, nếu màn hình đen xuất hiện trong thời gian dài, hãy kiên nhẫn. Nếu quá 10 phút, hãy đóng VM và khởi động lại thủ công.

![Installation complete](../../installations/imgs/24_installation_complete.png)

Khi VM khởi động lại, bạn sẽ thấy màn hình đăng nhập. Nhập mật khẩu rồi nhấp **Log in**.

![Login screen](../../installations/imgs/25_login_screen.png)

### Bước 4: Cập nhật và Guest Additions

Trước khi bắt đầu sử dụng VM, bạn có thể nhận thấy máy hơi giật, hơi trễ và thậm chí hơi nhỏ. Lý do là chúng ta chưa cài đủ trình điều khiển cần thiết để Xubuntu tận dụng "phần cứng" của VM. **Guest Additions** sẽ giải quyết việc này. VirtualBox cung cấp một image cho Guest Additions nên chúng ta sẽ sử dụng nó. Nhưng trước đó, chúng ta nên chạy các bản cập nhật.

Bây giờ chúng ta sẽ học một phím tắt hữu ích để mở một trong những công cụ quan trọng nhất trên hệ điều hành dựa trên Linux: Virtual Terminal, hay gọi ngắn là Terminal. Terminal thực ra luôn là ảo, dù bạn chạy trong VM hay không, nhưng điều đó nằm ngoài phạm vi chương trình học. Nếu muốn tìm hiểu thêm về Linux, hãy xem cuốn sách miễn phí [The Linux Command Line](https://linuxcommand.org/tlcl.php).

Khi nhấn <kbd>win</kbd> + <kbd>T</kbd>, một cửa sổ mới sẽ mở ra, cung cấp thông tin và yêu cầu bạn nhập dữ liệu. Đây là terminal của bạn. Bạn có thể mở nhiều terminal cùng lúc, nhưng hiện tại hãy chỉ dùng một terminal.

![Terminal](../../installations/imgs/26_new_terminal.png)

Bây giờ terminal đã mở, chúng ta có thể chạy cập nhật theo cách *ngầu* hơn. Có quy trình giao diện người dùng đồ họa (GUI) để chạy cập nhật, nhưng việc chạy vài lệnh trong giao diện dòng lệnh (CLI), để hàng loạt chữ chạy trên màn hình cùng thanh tiến trình, trông ấn tượng hơn nhiều.

Bạn sẽ nhập chính xác các lệnh sau và nhấn enter sau mỗi dòng mới. Hiện tại bạn chưa thể sao chép và dán.

```bash
sudo apt update
```

Khi nhập lệnh đầu tiên, terminal yêu cầu mật khẩu. Khi nhập mật khẩu, terminal sẽ không phản hồi, như thể bạn không nhập gì cả. Đây không phải lỗi; đây là tính năng. Một tính năng bảo mật. Nhiều ứng dụng biểu diễn mật khẩu bằng một số dấu chấm. Linux tiến thêm một bước và không cho người đứng cạnh cơ hội đếm số dấu chấm trên màn hình.

Nhập mật khẩu rồi nhấn enter để tải các bản cập nhật mới nhất.

![sudo apt update](../../installations/imgs/27_sudo_apt_update.png)

Bạn biết quá trình hoàn tất khi terminal lại sẵn sàng nhận dữ liệu. Terminal sẽ in ra số lượng gói có thể nâng cấp.

Sau khi chạy lệnh update, chúng ta vẫn cần chạy lệnh để *upgrade* các gói. Với các hệ điều hành biến thể Ubuntu, quy trình gồm hai bước. Trên các hệ thống khác, kết quả có thể khác.

Lệnh tiếp theo là lệnh này; hãy nhập rồi nhấn enter:

```bash
sudo apt upgrade
```

Lệnh đó sẽ hiển thị rất nhiều chữ, sau đó hỏi bạn có muốn tiếp tục bằng cách nhập `Y` để Yes hay `n` để No. Nhập `Y` rồi nhấn enter.

Cũng lưu ý rằng nếu chạy hai lệnh này liên tiếp, bạn sẽ không bị hỏi lại mật khẩu. Khi chạy thành công một lệnh `sudo`, một bộ đếm thời gian bắt đầu; terminal chỉ chờ đến khi bộ đếm kết thúc rồi mới hỏi lại. Thêm một tính năng bảo mật nữa!

Quá trình này có thể mất một lúc.

![sudo apt upgrade](../../installations/imgs/28_sudo_apt_upgrade.png)

Sau khi nâng cấp xong, hãy cài một số gói hữu ích để đảm bảo chúng ta cài Guest Additions thành công.

```bash
sudo apt install build-essential dkms
```

Nhớ nhập `Y` để cho phép cài các gói này. Đây là hai gói riêng biệt. Giải thích về chúng nằm ngoài phạm vi, nhưng bạn có thể tự tìm hiểu khi rảnh.

![sudo apt install helper packages](../../installations/imgs/29_sudo_apt_install_helper_packages.png)

Bước tiếp theo khó chụp ảnh màn hình, vì vậy phần được đánh dấu sẽ là menu bạn cần nhấp để tìm tùy chọn. Trên thanh công cụ VM ở phía trên, nhấp **Devices** rồi đi xuống cuối menu thả xuống, nơi có **Insert Guest Additions CD image...**, và nhấp vào đó. Có thể mất vài giây, sau đó một thông báo cho biết CD đã được gắn kết sẽ xuất hiện và trình quản lý tệp sẽ mở, hiển thị nội dung của image.

![Devices](../../installations/imgs/30_devices.png)

Cách dễ nhất để cài Guest Additions từ nội dung image là kéo thả biểu tượng vào terminal. Bạn cần tìm chính xác `VBoxLinuxAdditions.run`, sau đó nhấp giữ và kéo biểu tượng vào terminal đang mở.

Trước hết, hãy xóa phần xuất ra trong terminal để gọn hơn bằng cách nhập `clear` rồi nhấn enter. Sau khi xóa, nhập `sudo` kèm một dấu cách vì chúng ta cần quyền quản trị. *Chưa* nhấn enter. Sau đó kéo thả biểu tượng vào terminal. Khi terminal đã có đường dẫn đến tệp, hãy nhấp trở lại terminal, rời khỏi trình quản lý tệp.

Hãy chiêm ngưỡng bộ ảnh hướng dẫn thần bí gồm ba phần.

Nhấn enter trong terminal và nhập mật khẩu nếu được yêu cầu để tiếp tục.

![Phần 1 kéo thả Guest Additions](../../installations/imgs/31_drag_and_drop_part_1.png)

![Phần 2 kéo thả Guest Additions](../../installations/imgs/32_drag_and_drop_part_2.png)

![Phần 3 kéo thả Guest Additions](../../installations/imgs/33_drag_and_drop_part_3.png)

Cửa sổ của bạn có thể thay đổi kích thước trong quá trình này. Để hoàn tất cài đặt Guest Additions, bạn cần khởi động lại VM. Mở khóa phím tắt mới: `sudo reboot`

Sau khi đăng nhập, bạn có thể không nhận thấy điều gì khác biệt, nhưng hãy thử phóng to VM ngay bây giờ. Hãy nhìn xem, thật nhiều không gian cho các hoạt động!

![VM được phóng to](../../installations/imgs/34_room_for_activities.png)

Nhìn chung, bạn cũng sẽ gặp ít vấn đề về hiệu năng hơn và sẵn sàng tiếp tục chương trình học. Đừng quên tháo image Guest Additions. Bạn có thể nhập `sudo eject /dev/sr0` trong Terminal hoặc nhấp nút tháo hình tam giác cạnh image đã gắn kết (như hình bên dưới) trong trình quản lý tệp. Để image vẫn được gắn kết cũng không sao, nhưng tháo ra sẽ gọn gàng hơn vì chúng ta không còn cần đến nó.

![Nút tháo ISO](../../installations/imgs/35_iso_eject_button.png)

### Bước 4: Tìm hiểu VM mới của bạn

#### Bước 4.1: Đóng TOP trong trình duyệt Windows và mở website trong VM

Từ bây giờ, hãy chỉ sử dụng website The Odin Project (TOP) trong VM và làm theo các hướng dẫn Linux. Chương trình học thường yêu cầu bạn sao chép và dán code giữa bài học, không gian viết code và terminal. Việc này sẽ không hoạt động trơn tru nếu chuyển đổi giữa VM và Windows vì chúng là các môi trường hoàn toàn riêng biệt.

**Nhớ rằng:** Trong phần còn lại của chương trình học TOP, chỉ tham khảo các hướng dẫn Linux.

Dưới đây là một số mẹo giúp bạn bắt đầu trong môi trường ảo:

- Nếu không thấy thanh công cụ: Hãy bật thanh công cụ trong cài đặt VM - ở đó có những tùy chọn hữu ích để bạn thử, đặc biệt là các tùy chọn liên quan đến toàn màn hình hoặc nhiều màn hình. Để làm vậy, nhấp **Settings**, đi đến **User Interface**, rồi đánh dấu **Show at Top of Screen**.
  ![Bạn nên xem qua toàn bộ phần cài đặt để hiểu những gì có thể làm.](../../installations/imgs/16_toolbar.png)
- Mọi công việc của bạn phải diễn ra trong VM. Bạn sẽ cài mọi thứ cần thiết cho việc lập trình, bao gồm trình soạn thảo văn bản, môi trường ngôn ngữ và nhiều công cụ khác, bên trong VM. Bản cài đặt Xubuntu trong VM cũng có sẵn trình duyệt web, nhưng chúng ta sẽ sớm cài Chrome.
- Bật Shared Clipboard - Trên thanh công cụ nói trên, trong phần devices, bạn có thể bật Bidirectional dưới **Shared Clipboard**. Khi đó bạn có thể sao chép và dán giữa Host và Guest theo cả hai chiều. Tuy nhiên, hãy nhớ rằng nếu có thể thì bạn nên làm việc hoàn toàn trong VM. Mẹo thêm: `ctrl+shift+v` để dán trong terminal.
- Để cài phần mềm trên VM, hãy làm theo hướng dẫn cài đặt Linux (Ubuntu) từ bên trong VM Xubuntu.
- Bạn có thể cần chụp ảnh màn hình khi yêu cầu trợ giúp trên Discord; cách thực hiện tùy nơi bạn dùng Discord:
  - **Trong VM:** bạn có thể dùng <kbd>Shift</kbd> + <kbd>PrtSrc</kbd> để chụp một phần màn hình. Ngoài ra, có thể nhấp **Whisker Menu**, nhập **Screenshot**, rồi chọn chụp toàn màn hình, cửa sổ hiện tại hoặc một vùng cụ thể.
  - **Trên máy chủ (Windows):** bạn có thể dùng phím tắt Host Key (<kbd>Right Ctrl</kbd> + <kbd>E</kbd>) hoặc nhấp **View -> Take Screenshot** để chụp toàn màn hình. Cách khác là bỏ focus khỏi cửa sổ VM bằng cách nhấp ra ngoài, rồi dùng phím tắt Windows thông thường <kbd>Windows key</kbd> + <kbd>Shift</kbd> + <kbd>S</kbd> để chụp một phần màn hình.
- **Nhớ rằng:** mọi hoạt động phát triển liên quan đến TOP phải được thực hiện trong VM.
- Chúng tôi khuyên bạn dùng chế độ toàn màn hình (**View -> Full-screen Mode**) và quên hệ điều hành máy chủ (Windows). Để có hiệu năng tốt nhất, hãy đóng mọi chương trình trên hệ điều hành máy chủ khi chạy VM.
- Để thêm màn hình, hãy tắt VM, vào tab **Display** trong cài đặt VirtualBox của VM và tăng số lượng màn hình. Khi VM đang chạy, nhấp **View -> Virtual Screen 2 -> Enable**. Bạn có thể chạy toàn màn hình với nhiều màn hình, nhưng có thể cần thêm **Video Memory**, vốn đáng lẽ đã được tăng khi thêm màn hình. **Hãy bật Virtual Screens ở chế độ cửa sổ trước khi chuyển sang toàn màn hình, nếu không chúng sẽ không hoạt động.** Khi thoát chế độ toàn màn hình, màn hình phụ có thể đóng. Bạn có thể mở lại bằng các hướng dẫn này.

#### Các vấn đề/câu hỏi thường gặp

- Nếu khi cố khởi động VM bạn chỉ thấy màn hình đen, hãy đóng và `power off` VM, nhấp **Settings -> Display** trong VirtualBox, đảm bảo **Enable 3D Acceleration** được bỏ chọn và bộ nhớ Video được đặt **AT LEAST 128MB**.
- Hết dung lượng? Xem [hướng dẫn tăng dung lượng đĩa VM từ máy chủ Discord TOP](https://discord.com/channels/505093832157691914/690588860085960734/1015965403572351047).
- Bạn đang dùng màn hình cảm ứng? [Xem video hướng dẫn bật điều khiển màn hình cảm ứng cho VirtualBox](https://www.youtube.com/watch?v=hW-iyHHoDy4).

- **Mẹo về hiệu năng VM**:
  - Khi chạy VM, hãy giảm hoạt động trên Windows. Nếu dùng laptop, có lẽ bạn cũng nên cắm sạc.
  - Đảm bảo processors chỉ được đặt ở mức 2 và Memory cấp cho VM không quá một nửa tổng RAM nhưng ít nhất là 2GB. Nếu không thể dành 2GB, hãy khởi động kép.
  - Nếu video bị giật trong VM, hãy tăng Video Memory lên mức tối đa có thể hoặc phát video trên Windows nếu máy đủ khả năng. Tắt 3D Acceleration nếu đang bật.
  - Hãy chú ý biểu tượng ![Biểu tượng con rùa xanh lá và chữ V](../../installations/imgs/17_turtle.png) ở góc dưới bên phải cửa sổ VM. Biểu tượng này cho biết Hyper-V đang bật. Một chủ đề trên diễn đàn VirtualBox mô tả [cách tắt hoàn toàn Hyper-V](https://forums.virtualbox.org/viewtopic.php?f=25&t=99390). Bạn muốn thấy biểu tượng con chip có chữ V ![Biểu tượng con chip có chữ V](../../installations/imgs/18_vboxV.png) thay vào đó. Nếu không thấy biểu tượng nào trong hai biểu tượng ở góc dưới bên phải, bạn phải thoát chế độ toàn màn hình để nhìn thấy chúng. *Đúng vậy, con rùa xuất hiện trong tất cả ảnh chụp màn hình hữu ích ở trên.*
  - Bản cài WSL2 trước đó trên máy tính sẽ phụ thuộc vào Hyper-V, nên chỉ tắt Hyper-V sẽ không ảnh hưởng đến hiệu năng VM. Chúng tôi khuyên bạn tắt hoàn toàn WSL2 (nếu đã cài) trước khi chạy VM, đồng thời đảm bảo Hyper-V đã tắt. [Bài viết của ninjaOne về cách bật và tắt WSL2 trong Windows 10](https://www.ninjaone.com/blog/enable-or-disable-windows-subsystem-for-linux-wsl/) sẽ giúp bạn tìm nơi thực hiện thao tác này.
  - Nếu hiệu năng vẫn chưa đủ, hãy chọn khởi động kép để đảm bảo chỉ một hệ điều hành sử dụng toàn bộ thông số máy, nhờ đó trải nghiệm được cải thiện đáng kể.
- Nếu con lăn chuột hoạt động bất thường trong Google Chrome và/hoặc các ứng dụng khác, đồng thời bạn đã xem qua các mẹo về hiệu năng VM để đảm bảo VM hoạt động đúng, hãy xem [phiên bản Guest Additions của bạn có chính xác không](https://discord.com/channels/505093832157691914/690588860085960734/1195697147123867668).

- **Tắt VM an toàn**

  - Bạn không rút điện chiếc máy tính dùng hằng ngày của mình, đúng không? Vậy tại sao lại làm thế với máy tính ảo? Khi nhấp nút X và đóng VM, bạn có thể đang nói lời tạm biệt với các tệp của mình. Trong phần này, bạn sẽ hiểu ba cách tắt VM.
    - Cách 1 - Tắt từ bên trong VM bằng giao diện

      Nhấp **Whisker Menu** ![Biểu tượng Whisker Menu hình chuột xanh-trắng](../../installations/imgs/00_whisker_icon.png) rồi nhấp biểu tượng nguồn sẽ hiển thị một số tùy chọn để thay đổi phiên làm việc, trong đó có **Shutting Down**. Đúng vậy, đây cũng là nơi bạn có thể đã dùng **Restart** trước đó!

    - Cách 2 - Tắt từ bên trong VM bằng terminal

      Nhập `poweroff` trong terminal là đủ. Hệ thống sẽ lập tức tắt.

    - Cách 3 - Tắt từ bên ngoài VM

      Cách cuối cùng để tắt an toàn là dùng giao diện VM. Nhấp tab File rồi nhấn nút đóng (cũng có biểu tượng nguồn) sẽ mở một cửa sổ bật lên có tiêu đề **Close Virtual Machine**. Cửa sổ này hỏi bạn muốn **Save the machine state**, **Send the shutdown signal** hay **Power off the machine**.

      ![Menu File của VM](../../installations/imgs/19_vbox_close.png)

      ![Menu Close Virtual Machine](../../installations/imgs/20_send_shutdown.png)

      Để an toàn, chọn radio **Send the shutdown signal** rồi nhấn OK. VM sẽ tắt an toàn và các tệp của bạn không bị hỏng.

[Quay lại bài học Cài đặt](https://www.theodinproject.com/lessons/foundations-installations#os-installation)