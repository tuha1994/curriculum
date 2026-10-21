### Giới thiệu

Bước đầu tiên để xây dựng bất kỳ trang web nào là có đúng công cụ. Với chúng ta, điều đó có nghĩa là thiết lập một môi trường phát triển để viết mã tốt.

Nhiều khóa học phát triển trực tuyến sử dụng các trình soạn thảo mã chạy trên trình duyệt hoặc “sandbox”, cung cấp cho bạn những công cụ và chương trình cần thiết để hoàn thành công việc trước mắt, chứ không cung cấp thêm gì khác. Bạn sẽ sử dụng một số sandbox trong những giai đoạn đầu của The Odin Project vì chúng rất phù hợp để bắt đầu nhanh chóng. Tuy nhiên, cách tốt nhất để chuẩn bị cho thành công lâu dài là làm việc trong một môi trường phát triển thực thụ.

Chúng tôi không giấu bạn điều này: cài đặt các gói, trình soạn thảo và thậm chí cả hệ điều hành (OS) có thể vô cùng khó chịu. Tuy vậy, trải nghiệm thiết lập một môi trường phát triển để chạy mã bạn sẽ viết là một kỹ năng thực tế vô giá, sẽ theo bạn trong suốt sự nghiệp.

**Nếu bạn đang sử dụng macOS, Ubuntu hoặc [một phiên bản chính thức của Ubuntu](https://ubuntu.com/desktop/flavours) làm hệ điều hành và đã cài Google Chrome làm trình duyệt, bạn có thể bỏ qua bài học này. Nếu không, hãy làm theo hướng dẫn cài đặt bên dưới để thiết lập môi trường phát triển của mình.**

### Tổng quan bài học

Phần này trình bày khái quát những chủ đề bạn sẽ học trong bài học này.

- Cách thiết lập một môi trường phù hợp để học theo chương trình của The Odin Project.
- Cài đặt Google Chrome trong môi trường của bạn.

### Các hệ điều hành được hỗ trợ

#### macOS

Nếu bạn đang sử dụng máy Mac thì bạn đã có một khởi đầu rất thuận lợi. Hướng dẫn của The Odin Project giả định rằng bạn sử dụng một hệ thống dựa trên Unix. Chỉ cần cài đặt một vài chương trình, bạn sẽ có thể bắt đầu việc học ngay!

#### Linux (Ubuntu và các phiên bản chính thức)

[Linux](https://en.wikipedia.org/wiki/Linux) là một hệ điều hành tự do và mã nguồn mở, hoạt động tốt với mọi ngôn ngữ lập trình. Phần lớn công cụ phát triển được viết để hoạt động nguyên bản trên Linux. Các công cụ của bạn có khả năng sẽ được cập nhật thường xuyên hơn, có nhiều thông tin xử lý sự cố hơn và đơn giản là chạy tốt hơn trên Linux.

Để tăng khả năng nhận được trợ giúp trong cộng đồng, chúng tôi chỉ giới hạn phạm vi hỗ trợ các bản phân phối Linux ở Ubuntu và các phiên bản chính thức của Ubuntu. Điều này có nghĩa là chúng tôi không hỗ trợ bất kỳ bản phân phối nào khác, kể cả những bản “dựa trên Ubuntu hoặc Debian” (chẳng hạn như Mint, Pop!_OS, v.v.).

Chúng tôi hỗ trợ các phương thức chạy Linux sau đây (hướng dẫn cho các phương thức này nằm trong phần bài tập bên dưới):

- Máy ảo VirtualBox
- Khởi động kép
- Windows Subsystem for Linux phiên bản 2 (WSL2)

#### Chromebook

Nếu sử dụng Chromebook, bạn có thể chạy một thiết bị đầu cuối Linux trên thiết bị của mình. Hướng dẫn về Linux trên ChromeOS trong phần bài tập bên dưới sẽ cung cấp thêm chi tiết.

### Bạn lo ngại về việc cài đặt một hệ điều hành mới?

“Khoan, khoan, khoan! Các bạn không hỗ trợ Windows sao? Nhưng tôi hoàn toàn hài lòng với hệ điều hành hiện tại của mình mà!”

Nếu không có máy tính Apple, nhiều khả năng bạn đang sử dụng Windows. **Bản thân Windows không được The Odin Project hỗ trợ nguyên bản, cũng không được hỗ trợ trên máy chủ Discord của chúng tôi.** Vì nhiều công cụ bạn sẽ sử dụng được viết với giả định về một môi trường dựa trên Unix, bạn sẽ cần một môi trường như vậy ngay cả khi dự định sau này dùng Windows làm hệ điều hành phát triển.

Đừng lo! Các lựa chọn trên không có nghĩa là bạn phải loại bỏ Windows, cũng không có nghĩa chúng tôi yêu cầu bạn ngừng sử dụng Windows cho những việc nằm ngoài The Odin Project. Linux sẽ vui vẻ cùng tồn tại trên ổ cứng với Windows. Chúng tôi biết có lẽ bạn đã học được nhiều mẹo và thủ thuật cho hệ điều hành yêu thích của mình và không muốn mất mọi thứ trên máy tính. Tuy nhiên, phần lớn hệ điều hành được phát triển với người dùng không chuyên kỹ thuật làm đối tượng, nên chúng ẩn đi hoặc khiến việc sử dụng nhiều ngôn ngữ và framework chúng ta cần cài đặt trở nên khó khăn. Việc phải xoay xở với những khó khăn đó khiến nhiều nhà phát triển mới bỏ cuộc trước cả khi bắt đầu hành trình đến với thế giới full-stack.

Việc chỉnh sửa máy tính hoặc thiết lập khởi động kép để làm việc với các công cụ cần thiết sẽ giúp bạn bắt đầu lập trình dễ dàng hơn nhiều và tạo ra một môi trường ít gây xao nhãng. Người dùng Windows có thể sử dụng bất kỳ lựa chọn Linux nào được The Odin Project hỗ trợ.

Vẫn chưa bị thuyết phục? Dưới đây là một số lý do tuyệt vời để cài Linux:

- **Đã được kiểm thử** - Chúng tôi đã kiểm thử hướng dẫn của mình với macOS, Ubuntu (và các phiên bản Ubuntu chính thức) và WSL2. Chúng tôi đã nghiên cứu để bạn có thể cài đặt công cụ với ít vấn đề nhất có thể, từ đó sớm bắt tay vào viết mã hơn. Thời gian vật lộn với hệ điều hành là thời gian bạn không thể dùng để học lập trình.
- **Hỗ trợ từ cộng đồng** - Sử dụng các công cụ chúng tôi đề xuất sẽ giúp chúng tôi hỗ trợ bạn dễ dàng hơn khi bạn gặp sự cố.
- **Công cụ phát triển được xây dựng cho Linux** - Ruby (on Rails) và Node.js, những công nghệ backend phổ biến được The Odin Project giảng dạy và được sử dụng rộng rãi trong cộng đồng phát triển web nói chung, là các dự án mã nguồn mở *mong đợi* được chạy trên một nền tảng mã nguồn mở (dựa trên UNIX) như Linux.
- **Làm việc như các chuyên gia** - Nhiều nhà phát triển sử dụng hệ điều hành dựa trên Unix.
- **Hiệu năng** - Bạn lo lắng về việc cài Linux vì máy tính chậm hoặc cũ và có dung lượng hạn chế? Khi hiệu năng là ưu tiên, Linux là một lựa chọn tuyệt vời. Linux sử dụng ít tài nguyên hệ thống hơn Windows và chiếm ít dung lượng ổ cứng hơn.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Hỗ trợ từ cộng đồng

**Chúng tôi chỉ có thể hỗ trợ những gì được cung cấp trong phạm vi chương trình học. Chúng tôi không hỗ trợ Windows nguyên bản làm môi trường phát triển.** Việc sử dụng Windows đã được thảo luận nhiều lần và hiện tại điều đó không khả thi. Vui lòng không yêu cầu chúng tôi hỗ trợ Windows và **đừng đề cập đến vấn đề này trên Discord**. Chúng tôi liên tục đánh giá chương trình học để nội dung luôn mới mẻ và dễ tiếp cận nhất có thể, nhưng Windows chưa chứng tỏ là một con đường ít trở ngại. Để biết thêm thông tin về The Odin Project và Windows, hãy xem [danh sách lý do Windows không phải là hệ điều hành được The Odin Project hỗ trợ](https://github.com/TheOdinProject/blog/wiki/Why-We-Do-Not-Support-Windows).

Nhiều học viên đến kênh Discord của chúng tôi để hỏi liệu có cần làm theo hướng dẫn trên trang này hay không. Đội ngũ vận hành cả máy chủ Discord đã viết toàn bộ nội dung bạn vừa đọc về kế hoạch cài đặt. Những người hỗ trợ học viên trên máy chủ Discord đồng ý với hướng dẫn trên trang này và sẽ đưa ra các khuyến nghị giống như bạn đã đọc.

</div>

Sau khi giải quyết xong vấn đề đó, chúng ta cần thiết lập một môi trường phát triển phù hợp!

### Lựa chọn cấu hình

Nếu bạn chưa sử dụng macOS, Ubuntu hoặc một phiên bản chính thức của Ubuntu, bạn sẽ cần thiết lập một môi trường phát triển. Dưới đây là những gì mỗi lựa chọn mang lại:

#### Máy ảo VirtualBox (khuyến nghị)

Một [máy ảo](https://youtu.be/yIVXjl4SwVo) (VM) là sự mô phỏng một máy tính chạy bên trong hệ điều hành hiện tại của bạn. Nó cho phép bạn sử dụng một hệ điều hành khác bên trong một chương trình trên hệ điều hành hiện tại (ví dụ: chạy Linux bên trong Windows). Máy ảo cũng dễ cài đặt như bất kỳ chương trình nào khác và không gây rủi ro. Nếu không thích Linux, bạn có thể dễ dàng xóa máy ảo. Máy ảo là một cách tuyệt vời để các nhà phát triển mới bắt đầu nhanh chóng.

Vì lý do đó, chúng tôi khuyến nghị lựa chọn này cho người mới bắt đầu, đặc biệt nếu sau khi đọc toàn bộ nội dung này bạn vẫn chưa chắc nên chọn phương án nào. Chúng tôi có hướng dẫn từng bước rất rõ ràng để giúp bạn thiết lập.

#### Khởi động kép

Khởi động kép nghĩa là cài đặt hai hệ điều hành trên máy tính, cho phép bạn chọn khởi động Linux hoặc Windows khi máy tính bật lên. Ưu điểm của khởi động kép so với máy ảo là hệ điều hành có thể sử dụng toàn bộ tài nguyên máy tính, nhờ đó hoạt động nhanh hơn nhiều. Việc cài đặt hệ thống khởi động kép có thể tiềm ẩn một số rủi ro vì bạn phải thay đổi các phân vùng ổ cứng (trừ khi có một ổ cứng riêng chỉ dành cho Linux), nhưng bạn sẽ hoàn toàn ổn nếu dành thời gian và cẩn thận làm theo hướng dẫn. Đừng cố trộn lẫn hướng dẫn từ các nguồn khác nhau.

#### ChromeOS/ChromeOS Flex

Lựa chọn này dành cho người dùng có Chromebook hoặc muốn cài ChromeOS Flex trên phần cứng cũ. Nhiều Chromebook có sẵn hỗ trợ Linux và hoạt động tốt cho việc phát triển.

#### WSL2 (nâng cao)

WSL2 là một dạng VM đặc biệt cho phép bạn chạy Linux trực tiếp bên trong Windows. Dù mạnh mẽ, nó có thể khiến người mới học bối rối vì bạn làm việc đồng thời trên hai hệ điều hành mà không có ranh giới trực quan rõ ràng, nên dễ tương tác nhầm với hệ điều hành này thay vì hệ điều hành kia hơn so với một lựa chọn được tách biệt rõ như VM với VirtualBox hoặc khởi động kép. Vì vậy, chúng tôi đánh dấu đây là lựa chọn nâng cao và không khuyến nghị cho người mới chưa hiểu nhiều về hệ điều hành. Một trải nghiệm Linux được tách biệt rõ ràng hơn (chẳng hạn như VM VirtualBox hoặc khởi động kép) trực quan hơn nhiều và ít có khả năng khiến bạn tương tác nhầm với những thứ không mong muốn.

Lưu ý rằng WSL2 không giống WSL1. The Odin Project chỉ hỗ trợ WSL2.

### Hỗ trợ trình duyệt

The Odin Project chỉ hỗ trợ sử dụng Google Chrome. Các bài học của chúng tôi sử dụng Google Chrome và trình duyệt này được các nhà phát triển cũng như người dùng sử dụng rộng rãi; những khuyến nghị chúng tôi đưa ra đều có chủ đích rõ ràng. Hãy xem [thị phần sử dụng của các trình duyệt web](https://en.wikipedia.org/wiki/Usage_share_of_web_browsers#Summary_tables) này để biết những người khác đang sử dụng trình duyệt nào nhiều nhất.

Google Chrome có các công cụ dành cho nhà phát triển mạnh mẽ mà bạn sẽ sử dụng trong suốt chương trình học để kiểm tra, gỡ lỗi và thử nghiệm các ứng dụng web. Dù các trình duyệt khác cũng có những công cụ tương tự, hướng dẫn và ảnh chụp màn hình của chúng tôi sẽ giả định bạn đang sử dụng Chrome. Cũng như với việc hỗ trợ hệ điều hành, chúng tôi không thể hỗ trợ các vấn đề hoặc khác biệt phát sinh khi sử dụng trình duyệt khác.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Nếu bạn chưa chạy một môi trường được hỗ trợ, hãy làm theo một trong các <span id="os-installation">hướng dẫn cài đặt</span> dưới đây:
   - [Máy ảo (khuyến nghị)](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/linux/virtual_machine.md "Linux virtual machine installation instructions (recommended option)") - Cách dễ dàng và đáng tin cậy nhất để bắt đầu. VM chạy bên trong hệ điều hành hiện tại của bạn, chẳng hạn như Windows.
   - [Khởi động kép Ubuntu/Windows](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/linux/dual_boot.md "Ubuntu dual-boot instructions") - Cài Ubuntu như một hệ điều hành riêng song song với Windows trên máy tính. Rất phù hợp với các hệ thống cấu hình thấp vì không cần ảo hóa.
   - [ChromeOS/ChromeOS Flex](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/linux/chromeos.md "Linux installation instructions for ChromeOS") - Hướng dẫn dành cho người dùng Chromebook.
   - [WSL2 (nâng cao)](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/linux/wsl2.md "WSL2 installation instructions (advanced option)") - Chạy Linux bên trong Windows. Có thể khiến người mới học bối rối vì ranh giới giữa các hệ điều hành kém rõ ràng hơn.

   <div class="lesson-note lesson-note--warning" markdown="1">

   #### Phần cứng và phần mềm không được hỗ trợ

   Chương trình học này chỉ hỗ trợ sử dụng máy tính xách tay, máy tính để bàn hoặc Chromebook được hỗ trợ. Chúng tôi không thể giúp bạn thiết lập môi trường phát triển trên RaspberryPi hoặc bất kỳ thiết bị nào khác. Bạn chỉ cần làm theo một trong các bộ hướng dẫn này nếu **chưa** sử dụng **macOS**, **Ubuntu** hoặc [một phiên bản chính thức của Ubuntu](https://ubuntu.com/desktop/flavours) làm hệ điều hành.

   Nếu bạn đã sử dụng phần cứng và phần mềm được hỗ trợ, bạn có thể chuyển thẳng đến bước 2 bên dưới.

   </div>

1. Vì các bài học của chúng tôi sử dụng Google Chrome, và Chrome/Chromium được các nhà phát triển cũng như người dùng sử dụng rộng rãi, những khuyến nghị chúng tôi đưa ra đều có chủ đích rõ ràng. Hãy xem [thị phần sử dụng của các trình duyệt web](https://en.wikipedia.org/wiki/Usage_share_of_web_browsers#Summary_tables) này để biết những người khác đang sử dụng trình duyệt nào nhiều nhất.

   <span id="google-chrome-installation">Cài đặt Google Chrome</span> cho hệ điều hành của bạn:
   - [Linux](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/chrome/linux.md "Google Chrome installation for Linux") - Dành cho người dùng VM hoặc khởi động kép, **không dành cho người dùng WSL2**.
   - [macOS](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/chrome/macos.md "Google Chrome installation for macOS")
   - [WSL2](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/chrome/wsl2.md "Google Chrome installation for WSL2")

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Phím tắt Chrome

   Google Chrome là một trong những công cụ quan trọng nhất bạn sẽ sử dụng trong suốt chương trình học. Tài liệu tham khảo này có nhiều [phím tắt thông dụng có thể hữu ích khi bạn sử dụng Chrome](https://support.google.com/chrome/answer/157179?hl=en&co=GENIE.Platform%3DDesktop#zippy=%2Ctab-window-shortcuts).

   </div>

</div>
