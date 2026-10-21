### Giới thiệu

Trước khi tìm hiểu sâu hơn về code, chúng ta sẽ dành một chút thời gian để cải thiện thiết lập editor và năng suất tổng thể của bạn. Làm việc này ngay bây giờ sẽ giúp mọi thứ về sau dễ dàng hơn nhiều. Bài học này sẽ cung cấp một số thông tin về code style, sau đó giới thiệu cho bạn các công cụ giúp duy trì code-style nhất quán trong suốt các project. Trong một số trường hợp, các công cụ này thậm chí có thể giúp điều chỉnh những việc như thụt lề cho bạn!

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Tìm hiểu về style guide và lý do chúng quan trọng.
- Thiết lập một linter và prettier để cải thiện code.

### Style guide

Code style rất quan trọng! Có một tập hợp quy tắc nhất quán về những việc như thụt lề, kiểu dấu nháy ưu tiên hoặc các cách thực hành về cấu trúc code chung sẽ giúp code dễ bảo trì và dễ đọc hơn. Có một số style guide JavaScript phổ biến trên mạng đặt ra tiêu chuẩn cho những việc này, và bạn sẽ thấy chúng thường khác nhau về những gì chúng bắt buộc. Không có style guide nào "đúng" hay "sai"; điều quan trọng là chúng áp dụng *một điều gì đó* để thúc đẩy tính nhất quán trong toàn bộ codebase.

Dưới đây là một vài ví dụ về style guide, trong đó có những style guide được các công ty cụ thể sử dụng:

1. [Airbnb Style Guide](https://github.com/airbnb/javascript) là một trong những style guide phổ biến nhất.
1. Ngoài ra còn có [JavaScript style guide được sử dụng tại Google](https://google.github.io/styleguide/jsguide.html).
1. [JavaScript Standard Style](https://standardjs.com/rules.html).

### Linting

Các style guide chúng ta vừa nhắc đến có rất nhiều lời khuyên hữu ích về cách định dạng, tổ chức và cấu thành code. Nhưng có *rất nhiều* quy tắc - việc ghi nhớ tất cả có thể khó khăn. **Linters** là các công cụ quét code bằng một tập hợp quy tắc style và báo cáo mọi lỗi mà chúng tìm thấy. Trong một số trường hợp, chúng thậm chí có thể tự động sửa lỗi! Có nhiều linter tồn tại cho JavaScript, nhưng phổ biến nhất là [ESLint](https://eslint.org/).

ESLint được cài đặt dưới dạng dev dependency trong project, cho phép bạn chạy các phép kiểm tra trên bất kỳ file nào bằng command line. [Trang "Getting Started" chính thức của ESLint](https://eslint.org/docs/user-guide/getting-started) là một nơi tốt để bắt đầu, trong đó có hướng dẫn cài đặt và cấu hình cơ bản. Bộ quy tắc mặc định bao quát nhiều tình huống phổ biến nhất với các thiết lập mặc định hợp lý.

Bạn cũng nên xem [tài liệu về cấu hình ESLint](https://eslint.org/docs/latest/use/configure/) để biết danh sách các tùy chọn có thể thay đổi, chẳng hạn như bao gồm hoặc loại trừ một số folder hoặc file, cùng thông tin chi tiết về từng rule.

### Formatter

Formatter thật *tuyệt vời*. Chúng tương tự linter nhưng có chức năng hơi khác. Formatter nhận code JavaScript của bạn rồi tự động định dạng theo một tập hợp quy tắc. Không giống linter, formatter không tìm lỗi style mà tập trung vào layout của code, đưa ra các quyết định thông minh về những việc như khoảng trắng, mức thụt lề và ngắt dòng.

Như thường lệ, có nhiều formatter khác nhau. [Prettier](https://prettier.io/) là một lựa chọn rất phổ biến và có quan điểm khá cứng rắn. Ngoài một vài tùy chọn, phần lớn quyết định định dạng của nó không thể tùy chỉnh. Vì nhiều quyết định đã được đưa ra thay bạn, bạn sẽ dành ít thời gian hơn để quyết định những việc như kích thước thụt lề hoặc khoảng cách, và có thêm thời gian cho những vấn đề thực sự quan trọng.

Tương tự ESLint, Prettier được cài đặt dưới dạng dev dependency trong project, vì vậy hãy đọc [hướng dẫn cài đặt Prettier](https://prettier.io/docs/en/install.html) để biết cách thực hiện. Mặc dù nó thường chạy với các rule mặc định, bạn cũng có thể thay đổi mọi thiết lập trong [file cấu hình Prettier](https://prettier.io/docs/configuration). Nếu bạn đang dùng bộ rule mặc định được ESLint khuyến nghị, sẽ không có rule lint/format nào xung đột, nghĩa là bạn có thể cài đặt Prettier nguyên trạng mà không cần cấu hình `eslint-config-prettier`.

Sử dụng Prettier giúp việc code nhanh và dễ hơn! Bạn không cần lo lắng về việc thụt lề thật chuẩn hoặc nhớ từng dấu chấm phẩy, vì nó sẽ xử lý những chi tiết đó cho bạn.

### Extension IDE cho linting và formatting

Linters và formatter thường là các package bạn cài đặt trong project và sử dụng qua command line. Tuy nhiên, nhiều công cụ phổ biến cũng có extension IDE, và cả ESLint lẫn Prettier đều có extension cho Visual Studio Code, giúp việc linting và formatting trên máy của bạn thuận tiện hơn nhiều.

Ví dụ, sau khi cài đặt, [extension ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) có thể cung cấp cảnh báo và lỗi linter dưới dạng các đường gợn sóng được mã hóa bằng màu ngay trong file đang mở, thậm chí còn cho bạn biết chi tiết về rule cụ thể nào bị vi phạm, tất cả mà không cần chạy ESLint trong command line. [Extension Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) cho phép bạn format file bằng một command trong IDE hoặc shortcut bàn phím tùy chỉnh, cũng không cần chạy command trong terminal.

Điều quan trọng là bạn vẫn phải cài đặt các package dưới dạng dependency trong project cùng với mọi file cấu hình. Các extension có thể đặt sẵn rule dự phòng, nhưng nếu phát hiện package và file cấu hình tương ứng trong project, chúng sẽ sử dụng các rule đó và phiên bản package đã cài. Nhờ vậy, project luôn chứa nguồn sự thật cho các rule linting và formatting cần áp dụng, và nếu làm việc trên các project khác, bạn sẽ ít có khả năng vô tình đưa vào những thay đổi style không mong muốn từ thiết lập cục bộ.

Tóm lại, các extension là công cụ tiện lợi tuyệt vời, nhưng không nên được dùng làm nguồn sự thật cho thiết lập linting hoặc formatting của project.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Thêm thiết lập vào template repository

Hãy nhớ [template repository](https://www.theodinproject.com/lessons/node-path-javascript-revisiting-webpack#template-repositories) chứ? Bạn có thể đưa thiết lập linter và formatter vào bất kỳ template nào để mọi việc nhanh và dễ hơn trong tương lai!

</div>

### Assignment

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết này, trong đó trình bày sâu hơn về [giá trị của linter và cách chúng hoạt động](https://blog.codacy.com/what-is-a-linter).
1. Xem [video giới thiệu ngắn về Prettier](https://www.youtube.com/watch?v=hkfBvpEfWdA) của chính tác giả công cụ.
1. Truy cập [online playground của Prettier](https://prettier.io/playground) và dùng thử. Hãy copy/paste một phần code JavaScript cũ của bạn vào editor đó để xem điều gì xảy ra.

</div>