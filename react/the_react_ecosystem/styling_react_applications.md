### Giới thiệu

Trong các khóa học trước, bạn sẽ học được rất nhiều CSS và tất cả những kỹ năng đó vẫn có thể áp dụng cho React. Tuy nhiên, có một số điều chúng tôi muốn nhấn mạnh. Như bạn có thể đã nhận thấy, tất cả các kiểu chúng tôi viết đều có chung phạm vi toàn cầu, điều đó có nghĩa là khi ứng dụng của chúng tôi phát triển, việc quản lý CSS của chúng tôi sẽ ngày càng khó khăn hơn. Một số công cụ được đề cập dưới đây là những thứ mọi người sử dụng để giúp giải quyết vấn đề này.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Biết về các cách tạo kiểu cho ứng dụng React.

### mô-đun CSS

CSS thông thường là cách tạo kiểu đơn giản nhất. CSS Các mô-đun cho phép bạn viết các khai báo kiểu CSS nằm trong phạm vi cục bộ, điều đó có nghĩa là cuối cùng chúng ta không còn phải lo lắng về tên lớp của mình có khả năng xung đột với các lớp khác trong phạm vi toàn cục.

### CSS trong JS

Tại sao lại viết CSS bằng CSS khi bạn có thể viết nó bằng JavaScript? Tất nhiên là chỉ đùa thôi!

CSS-in-JS là một mô hình để tạo kiểu cho các dự án front-end. Nó cho phép bạn kiểm soát hoàn toàn CSS bằng JavaScript và mở rộng nó bằng nhiều tính năng khác nhau. Ngoài ra, nó cũng giúp áp dụng kiểu dáng một cách hợp lý, ví dụ dựa trên trạng thái và cũng hỗ trợ mô-đun CSS giống như cách mà các mô-đun CSS thực hiện. Có nhiều giải pháp CSS-in-JS khác nhau. Một trong những cái phổ biến nhất trong hệ sinh thái React là [thành phần theo kiểu](https://styled-components.com/).

### CSS Khung tiện ích

CSS Các khung tiện ích là một lựa chọn phổ biến để tạo kiểu cho các ứng dụng React. Chúng cung cấp một tập hợp các lớp được xác định trước mà bạn có thể sử dụng trực tiếp trong HTML hoặc JSX trong trường hợp của chúng tôi.[Cơn gió ngược CSS](https://tailwindcss.com) là sự lựa chọn phổ biến nhất hiện nay.

### Thư viện thành phần

Điều gì sẽ xảy ra nếu mọi thứ đã được thực hiện cho bạn? Kiểu dáng, hành vi và khả năng truy cập được chăm sóc cho bạn trong các thư viện thành phần. Đúng như tên gọi, các thư viện này cung cấp các thành phần có thể thích ứng và tái sử dụng mà bạn có thể sử dụng trực tiếp trong dự án của mình. Các thành phần này bao gồm nhưng không giới hạn ở danh sách thả xuống, ngăn kéo, lịch, nút chuyển đổi, tab và tất cả các thành phần khác mà bạn có thể nghĩ tới.

[Chất liệu UI](https://mui.com/),[Cơ số](https://www.radix-ui.com/), Và [Luân xa UI](https://chakra-ui.com/) đáng được đề cập khi nói về các thư viện thành phần.

Ngoài ra còn có các thư viện thành phần biểu tượng như [sáng tỏ phản ứng](https://lucide.dev/guide/packages/lucide-react), cho phép bạn đưa các biểu tượng vào dự án của mình dưới dạng thành phần.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Phương pháp tạo kiểu được đề xuất

Vì mục đích tìm hiểu trong suốt khóa học này, chúng tôi thực sự khuyên bạn nên tránh sử dụng khung CSS hoặc thư viện thành phần (sử dụng thư viện thành phần biểu tượng là được) và thay vào đó hãy triển khai kiểu dáng của thành phần từ đầu bằng cách sử dụng Mô-đun CSS.

</div>

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc [CSS Tài liệu mô-đun](https://github.com/css-modules/css-modules) Và [Cách tạo kiểu cho các thành phần React bằng cách sử dụng Mô-đun CSS](https://www.makeuseof.com/react-components-css-modules-style/).

1. Đọc [CSS so với CSS-in-JS](https://blog.logrocket.com/css-vs-css-in-js/) Và [phân tích kỹ lưỡng về CSS-in-JS](https://css-tricks.com/a-thorough-analysis-of-css-in-js/).

1. Lướt qua [tài liệu về các thành phần theo kiểu](https://styled-components.com/).

</div>
