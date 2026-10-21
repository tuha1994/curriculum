### Giới thiệu

Bạn đã biết React *là gì* và có thể còn hình dung được *nên* sử dụng nó ở đâu. Bây giờ là lúc bắt đầu tìm hiểu *cách* sử dụng React.

Bài học này sẽ hướng dẫn cách bắt đầu một dự án React mới trên máy tính, cùng với một số công cụ hữu ích hỗ trợ bạn trong quá trình đó. Chúng ta cũng sẽ giải thích một số vấn đề có thể phát sinh (và cách tránh chúng).

### Tổng quan bài học

Phần này cung cấp tổng quan chung về những chủ đề bạn sẽ học trong bài học này.

- Cách tạo các dự án React.
- Cách sử dụng Vite để tạo các dự án React mới.
- Cách định dạng mã trong các dự án React.
- React Developer Tools là gì.

### Nhiều cách tiếp cận

Có nhiều cách để bắt đầu sử dụng React trong các dự án, từ việc gắn một loạt thẻ `<script>` để tải React từ một [CDN](https://en.wikipedia.org/wiki/Content_delivery_network), cho đến các [toolchain](https://en.wikipedia.org/wiki/Toolchain) và [framework](https://en.wikipedia.org/wiki/Web_framework) mạnh mẽ, có khả năng cấu hình cao và cho phép tăng khả năng mở rộng cũng như tối ưu hóa.

Một số ví dụ về các toolchain này gồm:

- [Cấu hình React của Vite](https://vitejs.dev/)
- [Gatsby](https://www.gatsbyjs.com/)
- [NextJS](https://nextjs.org/)
- [Create React App](https://create-react-app.dev/) (Đã ngừng sử dụng)

Tại sao chúng ta cần những toolchain này? Chẳng phải chúng ta có thể tự tạo công cụ theo ý mình sao?

Được, nhưng việc đó *khó*. React là một hệ sinh thái phức tạp với rất nhiều phần chuyển động. Trước khi có thể bắt đầu viết bất kỳ loại mã nào cung cấp chức năng, bạn sẽ cần cấu hình *ít nhất* những thứ sau:

- Quản lý gói ([NPM](https://www.npmjs.com/), [Yarn](https://yarnpkg.com/))
- Đóng gói mô-đun ([Webpack](https://webpack.js.org/), [Parcel](https://parceljs.org/))
- Biên dịch ([Babel](https://babeljs.io/))
- Bản thân React

Tất cả những thứ này, và đôi khi còn *nhiều hơn nữa*, là cần thiết để đưa một dự án React cùng môi trường phát triển vào trạng thái hoạt động.

<div class="lesson-note" markdown=1>

#### Lưu ý về Create React App

Create React App, hay CRA, từng là cách chính thức để tạo khung cho các dự án React mới kể từ khi được giới thiệu vào năm 2016. Đáng tiếc là vì nhiều lý do, [CRA đã bị ngừng phát triển vào đầu năm 2023](https://github.com/reactjs/react.dev/pull/5487#issuecomment-1409720741). Do CRA rất phổ biến, bạn sẽ thấy nó được nhắc đến trong nhiều hướng dẫn và tài liệu. Tuy nhiên, hiện không còn khuyến nghị sử dụng CRA cho các dự án mới.

</div>

### Đơn giản hóa quy trình

Giờ bạn đã hiểu những gì liên quan đến việc bắt đầu một dự án React từ đầu, bạn có thể thở phào nhẹ nhõm khi biết rằng chúng ta có thể bắt đầu chỉ với *một lệnh terminal*. Chúng ta sẽ dùng template React của Vite để tạo khung cho dự án, tương tự như khi tự tạo và sử dụng một repo template.

Vite xây dựng các công cụ frontend cho nhà phát triển và tận dụng những công nghệ mới nhất bên dưới để mang lại trải nghiệm phát triển tốt. May mắn là Vite cũng hỗ trợ hệ sinh thái React. Chúng ta sẽ dùng CLI của Vite để nhanh chóng tạo một dự án React từ template. Công cụ này yêu cầu cấu hình tối thiểu và cung cấp ngay nhiều công cụ cực kỳ hữu ích, cho phép chúng ta bắt đầu học ngay. Hãy bắt đầu!

### Tạo một ứng dụng React

Hãy đảm bảo bạn đang sử dụng phiên bản *LTS* mới nhất của Node, nếu không có thể xảy ra lỗi. Mở terminal và thư mục chứa các dự án của bạn. Sau đó nhập lệnh sau (bạn có thể thay `my-first-react-app` bằng bất kỳ tên nào mình muốn):

```bash
npm create vite@latest my-first-react-app -- --template react
```

Bạn có thể được hỏi có muốn cài đặt gói `create-vite` hay không; khi đó, hãy chấp nhận bằng cách nhập `y` rồi nhấn <kbd>Enter</kbd>. Nếu được hỏi về việc sử dụng các tính năng thử nghiệm, bạn có thể trả lời không. Cuối cùng, trả lời có cho `Install with npm and start now?`.

Sau khi lệnh thực thi xong, đầu ra sẽ giống như sau:

```bash
➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h + enter to show help
```

Nếu mọi thứ diễn ra đúng kế hoạch, hãy truy cập `localhost:5173`; bạn sẽ thấy trang sau:

![Trang chủ template React của Vite](https://cdn.statically.io/gh/TheOdinProject/curriculum/9200680864d044390aeb1baa838f213addd65cbe/react/introduction/setting_up_a_react_environment/imgs/vite_react_homepage.png)

Xin chúc mừng! Bạn đã tạo ứng dụng React đầu tiên của mình. Bây giờ bạn có thể thoát máy chủ phát triển (<kbd>Ctrl</kbd> + <kbd>C</kbd>) và dùng `cd` để chuyển vào thư mục dự án. Bạn có thể khởi động máy chủ phát triển bất cứ lúc nào trong thư mục dự án bằng cách chạy `npm run dev`.

Để liên kết thư mục dự án cục bộ với một repo GitHub, hãy tạo một repo **trống** mới trên GitHub, rồi làm theo hướng dẫn trên trang repo mới để kết nối repo đó với thư mục dự án cục bộ.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Sử dụng repo hiện có

Ngoài ra, nếu bạn đã tạo và clone một repo GitHub, bạn có thể dùng `cd` để chuyển vào repo đã clone, rồi chạy lệnh Vite ở trên với `.` làm tên dự án:

```bash
npm create vite@latest . -- --template react
```

Lệnh này yêu cầu Vite sử dụng thư mục hiện tại cho dự án thay vì tạo một thư mục mới theo tên dự án đã cho. Thư mục đã clone này vốn đã được khởi tạo dưới dạng một git repo và kết nối với remote phù hợp.

</div>

### Tìm hiểu sâu hơn

Hãy xem kỹ hơn dự án mới của chúng ta. Bên trong, bạn sẽ thấy một số thư mục cùng các tệp `package.json`, `package-lock.json`, `.gitignore` và `README.md`. Tệp `README.md` chứa một số thông tin hữu ích mà bạn nên dành thời gian đọc lướt qua ngay bây giờ.

Thư mục `public` là nơi chứa toàn bộ tài nguyên tĩnh liên quan đến ứng dụng. Những tài nguyên này có thể gồm hình ảnh, biểu tượng và các tệp thông tin dành cho trình duyệt.

Trong thư mục `src`, bạn sẽ tìm thấy mã chạy ứng dụng. Tệp `main.jsx` là điểm vào của ứng dụng. Hãy mở tệp `main.jsx` và xem chúng ta có thể hiểu điều gì đang xảy ra không:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

Ồ! Có khá nhiều thứ ở đây. Hiện tại, bạn không cần phải nhận ra nhiều (hoặc bất kỳ) phần nào trong số này. Dưới đây là tóm tắt ngắn gọn về những gì đang diễn ra:

1. Chúng ta lần lượt import `StrictMode` và `createRoot` từ các package `react` và `react-dom`.
1. Chúng ta import component `App` từ `App.jsx` để có thể đặt (render) nó vào DOM.
1. Chúng ta import một số kiểu CSS (bạn có thể nhận ra cú pháp này từ phần tài liệu về Webpack).
1. Chúng ta tạo một đối tượng `root` bằng cách gọi `createRoot` với một phần tử trong `index.html`.
1. Chúng ta gọi phương thức `render`, vốn được gắn vào đối tượng `root`, với một cú pháp trông khá thú vị bên trong cặp dấu ngoặc đơn.

Dễ hiểu là tất cả những điều này có thể trông không giống bất cứ thứ gì bạn từng thấy cho đến nay, nhưng đừng lo. Sau khi dành thời gian học khóa học này, bạn sẽ biết chính xác tất cả những thứ này làm gì, và còn *nhiều hơn nữa*.

### Công cụ dành cho nhà phát triển

Khi tiến bộ với React, các dự án của bạn chắc chắn sẽ ngày càng lớn hơn, chứa ngày càng nhiều component và có mức độ chức năng ngày càng cao.

Khi đó, việc theo dõi (và thực hiện thay đổi trực tiếp đối với) các phần đang hoạt động bên trong ứng dụng sẽ rất hữu ích cho việc hiểu và gỡ lỗi mã. Để làm điều này, chúng ta có thể sử dụng tiện ích mở rộng Chrome có tên [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en).

Chúng tôi khuyên bạn cài đặt và làm quen với công cụ này càng sớm càng tốt, vì đây là công cụ vô giá để phát triển React hiệu quả.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Ôn lại tài liệu này bằng cách đọc [Trang bắt đầu với Vite](https://vitejs.dev/guide/).
1. Xem [hướng dẫn về React Developer Tools](https://www.debugbear.com/blog/react-devtools) để bắt đầu học cách sử dụng công cụ này (đừng lo nếu hiện tại bạn chưa hiểu một số chi tiết).
1. Thử dọn dẹp dự án `my-first-react-app` để dự án không còn hiển thị trang mặc định. Hãy xem bạn có thể làm cho nó hiển thị thông báo "Hello, World!" thay thế hay không.

</div>