### Giới thiệu

Trình soạn thảo văn bản là công cụ được các nhà phát triển sử dụng nhiều nhất, bất kể bạn là kiểu nhà phát triển nào. Một trình soạn thảo văn bản tốt có thể giúp bạn viết mã tốt hơn nhờ kiểm tra mã theo thời gian thực, tô sáng cú pháp và tự động định dạng.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Trình soạn thảo mã là gì.
- Vì sao trình soạn thảo mã hữu ích đến vậy cho việc phát triển phần mềm.
- Cài đặt Visual Studio Code (VSCode).

### Tại sao tôi không thể dùng Microsoft Word?

Các trình soạn thảo văn bản có định dạng phong phú, chẳng hạn như Microsoft Word và LibreOffice Writer, rất phù hợp để viết tài liệu, nhưng những tính năng giúp chúng tạo ra các tài liệu được định dạng đẹp lại khiến chúng không thích hợp để viết mã. Một tài liệu được tạo bằng các trình soạn thảo văn bản có định dạng phong phú này không chỉ chứa văn bản mà còn có thêm dữ liệu được nhúng trong tệp. Các tệp này cũng chứa thông tin về cách hiển thị văn bản trên màn hình và dữ liệu về cách hiển thị hình ảnh được nhúng vào tài liệu. Ngược lại, các trình soạn thảo văn bản thuần túy như VSCode và Sublime không lưu thêm thông tin nào. Chỉ lưu văn bản cho phép các chương trình khác, chẳng hạn như trình thông dịch của Ruby, đọc và thực thi tệp dưới dạng mã.

### Trình soạn thảo mã

Bạn có thể hình dung trình soạn thảo mã là những công cụ phát triển web chuyên dụng. Chúng có khả năng tùy biến cao và cung cấp nhiều tính năng giúp công việc của bạn trở nên dễ dàng hơn. Không gì tệ hơn việc mất hai giờ cố gắng tìm hiểu vì sao chương trình không hoạt động, rồi nhận ra rằng bạn đã bỏ sót một dấu ngoặc đóng. Plugin, tô sáng cú pháp, tự động đóng ngoặc đơn và ngoặc nhọn, cùng tính năng lint là một vài trong số những lợi ích của việc sử dụng trình soạn thảo mã. Có rất nhiều trình soạn thảo văn bản để bạn lựa chọn, nhưng chúng tôi khuyên bạn nên bắt đầu với Visual Studio Code.

**Visual Studio Code**, thường được gọi ngắn gọn là VSCode, là một trình soạn thảo mã miễn phí xuất sắc. Công cụ này có khả năng hỗ trợ tiện ích mở rộng nổi bật và tích hợp Git rất tốt. VSCode là trình soạn thảo mã phổ biến nhất trong số các học viên và điều phối viên của Odin, vì vậy bạn có thể dễ dàng tìm được sự hỗ trợ trong cộng đồng.

Việc bạn sử dụng trình soạn thảo nào nhìn chung là vấn đề sở thích cá nhân, nhưng trong phạm vi khóa học này, chúng tôi sẽ giả định rằng bạn đang sử dụng VSCode vì nó miễn phí, dễ dùng và hoạt động gần như giống nhau trên mọi hệ điều hành. Hãy nhớ rằng điều này có nghĩa là bạn sẽ không thể nhận được hỗ trợ nếu sử dụng một trình soạn thảo văn bản khác VSCode trong quá trình học theo chương trình này.

Xin nhắc lại, nếu bạn đang sử dụng **máy ảo**, bạn nên cài đặt VSCode **trên máy ảo của mình**. Bạn cũng có thể cài đặt nó trên máy chủ (tức hệ điều hành Windows chính của bạn), nhưng hãy bảo đảm rằng công cụ quan trọng này được cài đặt bên trong máy ảo.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Tùy thuộc vào hệ điều hành bạn đang sử dụng, hãy làm theo hướng dẫn cài đặt VSCode phù hợp bên dưới:

   - [Linux](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/text_editors/linux.md "VSCode installation for Linux")
   - [macOS](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/text_editors/macos.md "VSCode installation for macOS")
   - [WSL2](https://github.com/TheOdinProject/curriculum/blob/main/foundations/installations/installation_guides/text_editors/wsl2.md "VSCode installation for WSL2")

   Về sau, nếu gặp bất kỳ vấn đề nào với VSCode hoặc muốn tìm hiểu thêm về một số tính năng, hãy xem [tài liệu VSCode](https://code.visualstudio.com/docs). Bạn cũng sẽ có thể sử dụng nhiều phím tắt trong VSCode; một số phím tắt có thể dành riêng cho hệ điều hành của bạn và bạn có thể tra cứu chúng khi cần.
1. Làm quen với VSCode sẽ giúp bạn tiết kiệm thời gian và làm việc hiệu quả hơn. Bằng cách xem [video hướng dẫn VSCode cho người mới bắt đầu](https://youtu.be/ORrELERGIHs?t=103), bạn sẽ hình dung được tất cả những tính năng mà VSCode cung cấp. Đừng lo về việc phải lập trình theo video; chỉ cần chú ý cách VSCode được sử dụng trong suốt video.
1. Tắt tính năng hoàn thành mã bằng AI Copilot, vốn được bật mặc định trong Visual Studio Code. Trong VSCode, hãy mở phần cài đặt (`File > Preferences > Settings`), tìm kiếm "Disable AI Features", sau đó đánh dấu vào ô bên dưới tiêu đề tương ứng. Để hiểu vì sao The Odin Project cho rằng bạn nên tắt tính năng này, hãy xem lại phần [Lưu ý về việc tạo mã bằng AI](https://www.theodinproject.com/lessons/foundations-motivation-and-mindset#a-note-on-ai-code-generation) trong bài học Động lực và tư duy.

</div>
