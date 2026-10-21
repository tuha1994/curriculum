### Giới thiệu

"Responsive Design" là một khái niệm cực kỳ quan trọng khi phát triển cho web. Theo nhiều cách, bạn đã có đủ công cụ để tạo ra các website responsive đẹp mắt. Trong phần này, chúng ta sẽ bổ sung những mảnh ghép còn thiếu.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học Responsive Design là gì và vì sao nó quan trọng.

### Thuật ngữ

"Responsive Design" là thuật ngữ dùng để mô tả việc tạo ra các website *phản hồi* trước những thay đổi về kích thước trình duyệt, nhằm tạo ra thứ hoạt động trên mọi thiết bị. Đáng tiếc là cụm từ này hơi gây hiểu lầm vì có từ 'design'. Các bài học này sẽ không tập trung nhiều vào *thiết kế* thực tế của website responsive, mà tập trung vào những kỹ thuật bạn có thể sử dụng để triển khai tính responsive trên website.

Từ 'design' nói đến việc đưa ra quyết định về những thứ trông đẹp mắt, xây dựng trải nghiệm người dùng và quyết định chính xác một dự án nên trông như thế nào. Có thể một vài mẩu kiến thức về điều đó sẽ được rải rác trong các bài học này, nhưng đó không phải trọng tâm chính. *Tuy nhiên*, vì cụm từ này gần như đã được mọi người sử dụng thống nhất, chúng ta sẽ dùng nó ở đây.

### Vậy nó là gì?

Kể từ khi chiếc iPhone đầu tiên ra mắt vào năm 2007, việc các dự án web của bạn hoạt động đúng trên mọi thứ, từ màn hình desktop thông thường đến màn hình điện thoại nhỏ xíu, đã trở thành một *yêu cầu bắt buộc*.

Như bạn có lẽ đã nhận ra, kiểu responsive này thường không tự động xảy ra, đặc biệt khi dự án trở nên phức tạp hơn việc đặt văn bản đơn giản trên một trang (dự án công thức nấu ăn chỉ dùng HTML đầu tiên của bạn có lẽ hoạt động khá ổn trên điện thoại, ngoại trừ việc hình ảnh bị cắt). Vì vậy, nói cho cùng, "Responsive Web Design" là một tập hợp các kỹ thuật bạn có thể dùng để khiến website hoạt động trên màn hình có kích thước bất kỳ.

Có những cách làm cho thiết kế đủ linh hoạt để hoạt động tốt trên hầu hết màn hình, và cũng có những cách thay đổi đáng kể bố cục trang ở các kích thước nhất định. Trong phần lớn dự án, có lẽ bạn sẽ dựa vào cả hai.

### Bạn nên hỗ trợ những kích thước màn hình nào?

Những điện thoại nhỏ nhất đang được sử dụng phổ biến hiếm khi nhỏ hơn `320px`, vì vậy đây nên là mốc giới hạn dưới đáng tin cậy. Nếu website hoạt động ở chiều rộng `320px`, nó sẽ hoạt động trên mọi thiết bị nhỏ. Giới hạn trên khó xác định hơn một chút. Ngày nay, màn hình siêu rộng không còn hiếm, vì vậy bạn nên tính đến khả năng website được xem trên một màn hình rộng đến mức khó tưởng tượng. Thông thường, điều này được thực hiện bằng cách đặt max-width cho *toàn bộ* nội dung rồi căn giữa nội dung trên trang. Với cách thiết lập như vậy, nội dung sẽ trông ổn ngay cả ở độ phân giải siêu rộng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc hướng dẫn của Google Chrome DevTools về [cách mô phỏng màn hình thiết bị di động](https://developer.chrome.com/docs/devtools/device-mode/).

</div>