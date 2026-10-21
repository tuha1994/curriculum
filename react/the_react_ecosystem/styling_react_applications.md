### Giới thiệu

Trong các khóa học trước, bạn đã học rất nhiều về CSS và tất cả kỹ năng đó vẫn áp dụng được cho React. Tuy nhiên, có một vài điều chúng tôi muốn nhấn mạnh. Như bạn có thể đã nhận thấy, mọi style chúng ta viết đều dùng chung global scope, nghĩa là khi application phát triển, việc quản lý CSS sẽ ngày càng khó. Một số công cụ dưới đây được mọi người dùng để giải quyết vấn đề này.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài này.

- Biết các cách style application React.

### CSS modules

CSS thông thường là cách style đơn giản nhất. CSS Modules cho phép viết các khai báo CSS có scope cục bộ, nghĩa là cuối cùng chúng ta không còn phải lo class name có thể xung đột với class khác trong global scope.

### CSS in JS

Tại sao phải viết CSS trong CSS khi có thể viết nó trong JavaScript? Tất nhiên chỉ đùa thôi!

CSS-in-JS là một paradigm để style project front-end. Nó cho phép bạn kiểm soát hoàn toàn CSS bằng JavaScript và mở rộng CSS với nhiều tính năng. Ngoài ra, nó còn giúp áp dụng style theo cách logic, ví dụ dựa trên state, đồng thời hỗ trợ CSS dạng module giống CSS Modules. Có nhiều giải pháp CSS-in-JS khác nhau. Một trong những giải pháp phổ biến nhất trong hệ sinh thái React là [styled-components](https://styled-components.com/).

### CSS Utility Frameworks

CSS Utility Frameworks là lựa chọn phổ biến để style application React. Chúng cung cấp một tập class được định nghĩa sẵn mà bạn có thể dùng trực tiếp trong HTML, hoặc JSX trong trường hợp của chúng ta. [Tailwind CSS](https://tailwindcss.com) là lựa chọn phổ biến nhất.

### Component libraries

Nếu mọi thứ đã được làm sẵn cho bạn thì sao? Component library lo phần style, behavior và accessibility. Đúng như tên gọi, các thư viện này cung cấp component có thể tùy chỉnh và tái sử dụng để bạn dùng trực tiếp trong project. Các component gồm, nhưng không chỉ gồm, dropdown, drawer, calendar, toggle, tab và mọi component khác bạn có thể nghĩ tới.

[Material UI](https://mui.com/), [Radix](https://www.radix-ui.com/), and [Chakra UI](https://chakra-ui.com/) are worth a mention when talking about component libraries.

Ngoài ra còn có các thư viện icon component như [lucide react](https://lucide.dev/guide/packages/lucide-react), cho phép đưa icon vào project dưới dạng component.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Cách tiếp cận style được khuyến nghị

Trong suốt khóa học, để phục vụ việc học, chúng tôi đặc biệt khuyên bạn tránh dùng CSS framework hoặc component library (dùng thư viện icon component thì được), thay vào đó hãy tự triển khai style cho component bằng CSS Modules.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [CSS Modules documentation](https://github.com/css-modules/css-modules) và [How to style React components using CSS Modules](https://www.makeuseof.com/react-components-css-modules-style/).

1. Đọc [CSS vs CSS-in-JS](https://blog.logrocket.com/css-vs-css-in-js/) và [a thorough analysis of CSS-in-JS](https://css-tricks.com/a-thorough-analysis-of-css-in-js/).

1. Đọc lướt qua [tài liệu styled components](https://styled-components.com/).

</div>
