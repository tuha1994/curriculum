### Giới thiệu

Vì vậy, hiện tại bạn đã học được rất nhiều JavaScript hấp dẫn, cho phép bạn làm cho các thành phần khác nhau của trang HTML/CSS phản ứng với hành động và/hoặc thông tin đầu vào của người dùng. Điều đó thật tuyệt! Như bạn đã thấy, bạn có thể khiến ứng dụng của mình thực hiện được rất nhiều điều thú vị chỉ bằng cách sử dụng JavaScript phía máy khách. Tuy nhiên, bạn có thể nhận thấy rằng vẫn còn thiếu một mảnh ghép quan trọng: trừ khi bạn sử dụng [Bộ nhớ cục bộ](http://coding.smashingmagazine.com/2010/10/11/local-storage-and-how-to-use-it/), ứng dụng của bạn sẽ 'quên' tùy chọn của người dùng cũng như mọi thay đổi khác được thực hiện ngay khi trang được tải lại.

Mặc dù Bộ nhớ cục bộ rất tuyệt nhưng nó không lý tưởng: nó chỉ lưu trữ dữ liệu trên máy tính mà người dùng đang truy cập trang. Cách tiếp cận này không cho phép ứng dụng 'ghi nhớ' bất kỳ điều gì khi cùng một người dùng cố gắng truy cập lại ứng dụng từ một thiết bị khác. Để làm được điều đó, bạn sẽ cần một chương trình phụ trợ thực sự.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiểu các cách khác nhau để sử dụng React trong ứng dụng Rails của chúng tôi.
- Giải thích cách truyền dữ liệu từ ứng dụng Rails của bạn sang React.
- Tìm hiểu cách tích hợp React với Rails và xử lý mã thông báo CSRF.

### Xây dựng chương trình phụ trợ của riêng bạn với Ruby on Rails

Vì bạn đã học Ruby on Rails trong [đường dẫn Ruby on Rails đầy đủ] (https://www.theodinproject.com/paths/full-stack-ruby-on-rails) của chúng tôi nên bước tiếp theo rất rõ ràng: bạn có thể xây dựng chương trình phụ trợ của riêng mình với Rails! Để chuẩn bị, hãy đọc lại [Bài học Rails về xây dựng API](https://www.theodinproject.com/lessons/ruby-on-rails-apis-and-building-your-own) để làm mới cách thiết lập chương trình phụ trợ Rails có thể xử lý các yêu cầu JSON.

Bạn xong chưa? Tốt. Tiếp theo, đã đến lúc thực hành cho phép JavaScript giao diện người dùng của bạn giao tiếp với chương trình phụ trợ Rails bằng cách sử dụng AJAX (yêu cầu HTTP không đồng bộ tới máy chủ web sử dụng JavaScript mà không yêu cầu tải lại toàn bộ trang).  Chúng tôi sẽ đề cập đến một số phương pháp hay nhất về cách truyền dữ liệu từ nơi này sang nơi khác và các cách khác nhau để tích hợp React với Rails, nhưng mặt khác, điều đó tùy thuộc vào bạn: bạn đã có mọi thứ mình cần để ghép những mảnh ghép cuối cùng của câu đố phát triển web lại với nhau.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết của thinkbot về ["Cách tích hợp React với Rails 7"](https://thoughtbot.com/blog/how-to-integrate-react-rails) để có cái nhìn tổng quan về các khả năng khác nhau.

1. Làm theo hướng dẫn trong ["Cách xây dựng chương trình phụ trợ Rails 7 fullstack với React Frontend"](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-ruby-on-rails-v7-project-with-a-react-frontend-on-ubuntu-20-04). Nó trình bày cách tích hợp React với Rails thông qua esbuild, truy xuất dữ liệu từ lớp API thông qua yêu cầu AJAX bằng API tìm nạp (hãy nhớ những gì bạn đã học trong bài học JavaScript ["Làm việc với API"](https://www.theodinproject.com/lessons/javascript-working-with-apis)) và xử lý mã thông báo CSRF trong các yêu cầu không phải GET.

</div>
