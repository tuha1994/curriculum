### Giới thiệu

Khi tiếp tục hành trình phát triển web, điều quan trọng cần nhớ là người dùng sản phẩm của bạn có thể sử dụng nhiều trình duyệt khác nhau: Chrome, Microsoft Edge, Firefox và Safari là một vài ví dụ. Đồng thời, số người sử dụng hệ điều hành di động đang tăng nhanh, vì vậy bạn cũng nên cân nhắc các phiên bản di động của những trình duyệt khác nhau.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Hiểu về khả năng tương thích của trình duyệt và lịch sử của nó.
- Biết các tính năng CSS mới được đưa vào trình duyệt như thế nào.
- Biết cách kiểm tra khả năng tương thích.

### Lịch sử trình duyệt

Lịch sử duyệt web hiện đại bắt đầu vào tháng 12 năm 1990 với sự ra mắt của trình duyệt WorldWideWeb. Trình duyệt này được Tim Berners-Lee viết khi ông làm việc cho cơ quan nghiên cứu hạt nhân châu Âu có tên CERN. Sau đó, nó được đổi tên thành Nexus để tránh nhầm lẫn với World Wide Web.

Nexus là trình duyệt đầu tiên thuộc loại này, cho phép người dùng xem các style sheet cơ bản, đọc nhóm tin và thậm chí có cả chức năng kiểm tra chính tả! Ngày nay điều đó có thể không có vẻ nhiều, nhưng vào thời điểm ấy, đây thực sự là một bước đột phá.

Tuy nhiên, sự ra mắt của Nexus mới chỉ là khởi đầu, vì trong thập kỷ tiếp theo, người dùng chứng kiến những phiên bản đầu tiên của các trình duyệt như Mosaic Browser. Trình duyệt này nhanh chóng trở nên phổ biến và trở thành trình duyệt được sử dụng nhiều nhất trên thế giới. Từ đó, World Wide Web phát triển bùng nổ cùng sự ra mắt của các trình duyệt Opera và Netscape Navigator.

Năm 1995, thế giới được giới thiệu phiên bản đầu tiên của Internet Explorer, trình duyệt sau đó trở thành thế lực thống trị thị trường. Có thời điểm Internet Explorer được hơn 90% người dùng sử dụng. Để đối phó với sự thống trị này, Netscape đã khởi xướng dự án về sau trở thành Mozilla Foundation, tổ chức phát triển và duy trì Firefox. Không lâu sau, năm 2003, Apple ra mắt Safari, còn năm 2008, Google ra mắt Chrome.

Có lẽ bạn đã quen thuộc với hầu hết, nếu không phải tất cả, những cái tên này. Cho đến ngày nay, sự [cạnh tranh giữa các trình duyệt](https://www.youtube.com/watch?v=W4wWdmfOibY) vẫn rất mạnh, dù Chrome (và [Chromium](https://en.wikipedia.org/wiki/Chromium_(web_browser))) đang là thế lực thống trị thị trường.

### Khả năng tương thích của trình duyệt là gì?

Ngày nay, thật khó tưởng tượng Web mà không có trình duyệt. Chúng ta đã chứng kiến sự chuyển dịch từ các ứng dụng độc lập sang HTML5 và Progressive Web Apps, cho phép ứng dụng hoạt động đầy đủ trong trình duyệt. Ví dụ, trong thời gian dài Microsoft Word và Excel chỉ có thể chạy thông qua ứng dụng độc lập. Giờ đây, bạn có thể sử dụng các ứng dụng đó bằng bất kỳ trình duyệt nào mà không cần cài đặt tệp nào.

Khi các công ty cạnh tranh để giành thị phần, những trình duyệt khác nhau sử dụng các engine khác nhau để hiển thị thông tin trên trang web. Ví dụ, Chrome và Chromium sử dụng Blink, còn Safari sử dụng WebKit.

Do những khác biệt này, ứng dụng của bạn có thể hoạt động khác nhau trên từng trình duyệt. Vì Chrome đang chiếm ưu thế, phần lớn ứng dụng được thiết kế để hoạt động trơn tru với Chromium, còn việc mang lại hiệu năng tốt tương đương trên các trình duyệt khác thường được xem là thứ yếu.

Để các dự án web của bạn tiếp cận được nhiều người hơn, bạn phải bảo đảm kiểm thử ứng dụng trên những trình duyệt mà người dùng có khả năng sử dụng nhất. Chrome, Safari, Firefox và các trình duyệt dựa trên Chromium khác (Microsoft Edge, Brave, v.v.) phổ biến hơn với người dùng thông thường. Tuy nhiên, tùy vào nhóm người dùng hoặc công ty bạn làm việc, bạn có thể cần hỗ trợ cả những trình duyệt ít phổ biến hơn. Với các trình duyệt Chromium, nếu ứng dụng hoạt động trên Chrome thì về cơ bản nó cũng sẽ hoạt động trên các trình duyệt liên quan khác.

### Các bản phát hành trình duyệt và tính năng CSS mới

W3C [World Wide Web Consortium](https://www.w3.org/) là tổ chức đứng sau việc phát triển các tiêu chuẩn web nhằm tối đa hóa khả năng tiếp cận và tính nhất quán của trải nghiệm web. W3C cũng là tổ chức có thẩm quyền phát triển các tính năng mới trong CSS. Đây là một cách tiếp cận hợp tác chặt chẽ giữa cộng đồng Web và các công ty phát triển trình duyệt.

Khi các trình duyệt như Nexus và Netscape được phát hành, chưa có tổ chức W3C để giúp tạo ra khả năng tương thích tốt hơn. Ứng dụng của bạn có thể trông và hoạt động khác nhau trên từng trình duyệt. Tệ hơn nữa, ứng dụng có thể hoàn toàn không sử dụng được. Các nhà phát triển web phải điều chỉnh riêng cho từng trình duyệt, và không phải nhà phát triển nào cũng có đủ nguồn lực để làm việc đó cho tất cả mọi người.

Ngày nay, khi các tiêu chuẩn web tiếp tục tiến hóa và thay đổi, đồng thời các nhà phát triển web bắt đầu triển khai tính năng mới trong codebase, trình duyệt phải cung cấp hỗ trợ cho những tính năng đó. Nếu trải nghiệm người dùng bị ảnh hưởng vì trình duyệt thiếu hỗ trợ, người dùng có thể chuyển sang đối thủ cạnh tranh.

### Khi nào có thể an toàn sử dụng tính năng mới?

Dù việc triển khai tính năng mới rất hào hứng, việc quá vội vàng cũng có rủi ro. Người dùng sẽ có trải nghiệm không tốt nếu, chẳng hạn, ứng dụng trước đây hoạt động tốt trên Firefox nhưng do thay đổi codebase lại không thể sử dụng trên Firefox mà chỉ hoạt động tốt trên Safari. May mắn là có một công cụ giúp bạn ngăn tình huống này.

["Can I Use"](https://caniuse.com/) là một tài nguyên tuyệt vời giúp bạn xác nhận trình duyệt có hỗ trợ tính năng mới hay không. Công cụ này cung cấp số liệu về những trình duyệt và nền tảng đang hỗ trợ công nghệ mới, thậm chí cho biết phiên bản nào của các trình duyệt đó hỗ trợ từng tính năng.

Nhìn chung, bạn nên triển khai tính năng mới khi chúng được hầu hết các trình duyệt phổ biến hỗ trợ. Nhờ vậy, bạn sẽ ít có khả năng gặp phải vấn đề mà nhiều người dùng cùng đối mặt.

### Trình duyệt di động

Theo truyền thống, Web được xây dựng ưu tiên cho máy tính để bàn. Một ứng dụng được xem là thành công nếu nó hoạt động tốt trên các trình duyệt máy tính để bàn. Nhưng khi điện thoại thông minh ngày càng phổ biến, mỗi năm càng có nhiều người dùng thiết bị di động làm thiết bị chính để truy cập Web. Ở một số khu vực trên thế giới, người dùng di động chiếm đa số áp đảo.

Thiết bị di động chủ yếu gồm điện thoại thông minh và máy tính bảng. Các hệ điều hành di động phổ biến nhất là Android và iOS của Apple.

Khi phát triển ứng dụng, bạn cũng phải cân nhắc liệu ứng dụng có cần tương thích đầy đủ với thiết bị di động hay không. Có một vài điểm riêng của trình duyệt di động mà bạn cần ghi nhớ.

1. Trên iOS và iPadOS, Safari là trình duyệt duy nhất được hỗ trợ ở hầu hết các khu vực. Đúng là bạn có thể cài Chrome hoặc Firefox, thậm chí đặt chúng làm mặc định, nhưng chúng không phải là các trình duyệt hoàn chỉnh. Chúng vẫn sử dụng engine kết xuất của Safari (WebKit). Vì vậy, để ứng dụng web hoạt động với người dùng Apple, bạn phải bảo đảm hỗ trợ WebKit và các công nghệ khác được Safari sử dụng.
Điều quan trọng cần nhớ là trình duyệt di động không hoàn toàn giống phiên bản máy tính để bàn tương ứng. Một dự án hoạt động trên phiên bản Safari máy tính để bàn vẫn có thể cần điều chỉnh để hoạt động đúng trên phiên bản di động của cùng trình duyệt.
1. Một điểm khác cần cân nhắc ở trình duyệt di động là có rất nhiều kích thước màn hình khác nhau. Gần như không thể có sẵn mọi thiết bị thực tế để kiểm thử, và may mắn là trình duyệt cung cấp cách mô phỏng các thiết bị khác. Điều quan trọng cần nhớ là khi bạn, chẳng hạn, mô phỏng iPhone trong Chrome, thứ duy nhất được mô phỏng là kích thước màn hình. Hãy nhớ rằng các đặc điểm riêng của hệ điều hành sẽ không được tái hiện. Nghĩa là dù mọi thứ hoạt động tốt trong Chrome khi mô phỏng thiết bị, nó vẫn có thể hoạt động khác trên điện thoại hoặc máy tính bảng thực tế.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem lại [Can I Use](https://caniuse.com/). Tất cả công nghệ bạn đã gặp cho đến nay có được các trình duyệt phổ biến hỗ trợ không?
1. Đọc bài viết này [về trình duyệt trên iOS](https://adactio.com/journal/17428).

</div>