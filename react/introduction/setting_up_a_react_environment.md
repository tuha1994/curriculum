### Giới thiệu

Bạn đã biết *React là gì* và có thể cũng hình dung được *nên dùng React ở đâu*. Giờ là lúc tìm hiểu *cách sử dụng* React.

Bài học này sẽ đề cập đến việc bắt đầu một dự án React mới trên máy của bạn, cũng như một số công cụ hữu ích để trợ giúp bạn trong quá trình thực hiện. Chúng tôi cũng sẽ giải thích một số vấn đề có thể phát sinh (và cách tránh chúng).

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Cách tạo dự án React.
- Cách sử dụng Vite để tạo dự án React mới.
- Cách định dạng mã trong dự án React.
- React Công cụ dành cho nhà phát triển là gì.

### Nhiều con đường

Có nhiều cách để đưa React vào dự án, từ việc thêm các thẻ `<script>` tải React từ một [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) cho đến những [toolchain](https://en.wikipedia.org/wiki/Toolchain) và [framework](https://en.wikipedia.org/wiki/Web_framework) mạnh mẽ, có khả năng cấu hình cao, mở rộng và tối ưu hóa.

Một số ví dụ về các chuỗi công cụ này bao gồm:

- [Cấu hình Vite của React](https://vitejs.dev/)
- [Gatsby](https://www.gatsbyjs.com/)
- [Tiếp theoJS](https://nextjs.org/)
- [Tạo ứng dụng React](https://create-react-app.dev/)(Không dùng nữa)

Tại sao chúng ta cần những chuỗi công cụ này? Chúng ta không thể tự làm theo cách chúng ta thấy phù hợp sao?

Được, nhưng việc đó *khó*. React là một hệ sinh thái phức tạp với nhiều phần cần phối hợp. Trước khi viết được đoạn mã có chức năng, bạn sẽ phải cấu hình *ít nhất* những component sau:

- Quản lý gói ([NPM](https://www.npmjs.com/),[Yarn](https://yarnpkg.com/))
- Gói mô-đun ([Webpack](https://webpack.js.org/),[Bưu kiện](https://parceljs.org/))
- Biên soạn ([Babel](https://babeljs.io/))
- React chính nó

Tất cả những thứ đó, và đôi khi còn *nhiều hơn nữa*, đều cần thiết để khởi động một dự án React cùng môi trường phát triển.

<div class="lesson-note" markdown=1>

#### Lưu ý về Tạo ứng dụng React

Tạo ứng dụng React, hay CRA, là cách chính thức để xây dựng các dự án React mới kể từ khi được giới thiệu vào năm 2016. Thật không may, vì nhiều lý do, [CRA không còn được dùng nữa vào đầu năm 2023](https://github.com/reactjs/react.dev/pull/5487#issuecomment-1409720741). Do mức độ phổ biến của CRA, bạn sẽ thấy nó được đề cập trong nhiều hướng dẫn và tài liệu. Tuy nhiên, CRA không còn được khuyến khích cho các dự án mới.

</div>

### Đơn giản hóa quy trình

Giờ bạn đã biết việc khởi tạo một dự án React từ đầu gồm những gì, hẳn bạn sẽ nhẹ nhõm khi biết rằng có thể bắt đầu chỉ với *một lệnh trong terminal duy nhất*. Chúng tôi sẽ sử dụng mẫu React của chính Vite để xây dựng dự án của mình, giống như khi chúng tôi tạo và sử dụng kho lưu trữ mẫu của riêng mình.

Vite cung cấp các công cụ frontend cho lập trình viên và tận dụng những công nghệ mới để mang lại trải nghiệm phát triển tốt. Vite cũng hỗ trợ rất tốt hệ sinh thái React. Chúng ta sẽ dùng CLI của Vite để nhanh chóng tạo một dự án React mẫu. Công cụ này gần như không cần cấu hình nhưng đã có sẵn nhiều tính năng hữu ích, giúp ta bắt tay vào học ngay. Bắt đầu thôi!

### Tạo ứng dụng React

Hãy đảm bảo bạn đang dùng phiên bản *LTS* mới nhất của Node; nếu không, có thể phát sinh lỗi. Mở terminal và thư mục chứa các dự án của bạn. Sau đó nhập lệnh sau (bạn có thể thay thế `my-first-react-app` bằng bất kỳ tên nào bạn muốn):

```bash
npm create vite@latest my-first-react-app -- --template react
```

Bạn có thể được hỏi xem bạn có muốn cài đặt gói `create-vite` hay không, trong trường hợp đó bạn nên chấp nhận bằng cách nhập `y` rồi nhấn <kbd>Enter</kbd>. Nếu nó hỏi bạn bất kỳ câu hỏi nào về việc sử dụng các tính năng thử nghiệm, bạn có thể trả lời không. Cuối cùng, hãy trả lời có cho `Install with npm and start now?`.

Khi lệnh đã được thực thi, nó sẽ xuất ra như sau:

```bash
➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h + enter to show help
```

Nếu mọi thứ diễn ra đúng như dự kiến, hãy mở `localhost:5173`. Bạn sẽ thấy trang sau:

![Vite React trang chủ mẫu](https://cdn.statically.io/gh/TheOdinProject/curriculum/9200680864d044390aeb1baa838f213addd65cbe/react/introduction/setting_up_a_react_environment/imgs/vite_react_homepage.png)

Chúc mừng! Bạn đã tạo ứng dụng React đầu tiên của mình. Bây giờ bạn có thể thoát khỏi máy chủ dev (<kbd>Ctrl</kbd> + <kbd>C</kbd>) và `cd` vào thư mục dự án của bạn. Bạn có thể khởi động máy chủ dev bất kỳ lúc nào trong thư mục dự án bằng cách chạy `npm run dev`.

Để liên kết thư mục dự án trên máy với một repo GitHub, hãy tạo một repo **trống** trên GitHub, rồi làm theo hướng dẫn trên trang repo mới để kết nối repo đó với thư mục dự án.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Sử dụng repo hiện có

Ngoài ra, nếu bạn đã tạo một repo GitHub và sao chép nó, bạn có thể `cd` vào repo nhân bản của mình, sau đó chạy lệnh Vite ở trên, sử dụng `.` làm tên dự án:

```bash
npm create vite@latest . -- --template react
```

Lệnh này yêu cầu Vite dùng thư mục hiện tại làm thư mục dự án thay vì tạo thư mục mới theo tên dự án. Thư mục đã clone vốn đã là một git repo và đã kết nối với remote thích hợp.

</div>

### Tìm hiểu sâu hơn

Chúng ta hãy xem xét kỹ hơn về dự án mới của chúng tôi. Bên trong, bạn sẽ tìm thấy một số thư mục, cũng như các tệp `package.json`, `package-lock.json`, `.gitignore` và `README.md`. `README.md` chứa một số thông tin hữu ích mà bạn nên dành thời gian để đọc lướt qua ngay bây giờ.

Thư mục `public` là nơi chứa tất cả nội dung tĩnh liên quan đến ứng dụng của bạn. Điều này có thể bao gồm hình ảnh, biểu tượng và tệp thông tin cho trình duyệt.

Bên trong thư mục `src` là nơi bạn sẽ tìm thấy mã chạy ứng dụng của mình. Tệp `main.jsx` ở đây đóng vai trò là điểm vào của ứng dụng. Hãy mở tệp `main.jsx` và xem liệu chúng ta có thể hiểu chuyện gì đang xảy ra không:

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

Ối! Có khá nhiều ở đây. Bạn sẽ không nhận ra nhiều điều này (nếu có) ngay bây giờ. Dưới đây là tóm tắt ngắn gọn về những gì đang xảy ra:

1. Chúng tôi nhập `StrictMode` và `createRoot` từ các gói `react` và `react-dom` tương ứng.
1. Chúng tôi nhập component `App` từ `App.jsx` để có thể đặt (kết xuất) nó trong DOM.
1. Chúng tôi nhập một số kiểu CSS (bạn có thể nhận ra cú pháp này từ tài liệu Webpack).
1. Chúng ta tạo một đối tượng `root` bằng cách gọi `createRoot` với một phần tử từ `index.html` của chúng ta.
1. Chúng ta gọi phương thức `render`, được gắn vào đối tượng `root` của chúng ta, với một số cú pháp trông rất thú vị bên trong dấu ngoặc đơn.

Những thứ này có thể trông hoàn toàn xa lạ, nhưng đừng lo. Sau khi học hết khóa học, bạn sẽ hiểu chính xác chúng làm gì — và còn *nhiều điều hơn thế nữa*.

### Công cụ dành cho nhà phát triển

Khi bạn phát triển với React, các dự án của bạn chắc chắn sẽ ngày càng lớn hơn và bao gồm ngày càng nhiều component, với mức độ chức năng ngày càng tăng.

Khi điều này xảy ra, sẽ rất hữu ích nếu bạn có thể theo dõi (và thực hiện các thay đổi trực tiếp) các bộ phận chuyển động bên trong ứng dụng để hiểu và gỡ lỗi mã của mình. Để đạt được mục đích này, chúng ta có thể sử dụng tiện ích mở rộng của Chrome có tên [React Công cụ dành cho nhà phát triển](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en).

Chúng tôi khuyên bạn nên cài đặt phần mềm này và cảm thấy thoải mái khi sử dụng nó càng sớm càng tốt vì đây là một công cụ vô giá để phát triển React hiệu quả.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem lại tài liệu này bằng cách đọc qua [Trang Bắt đầu của Vite](https://vitejs.dev/guide/).
1. Kiểm tra cái này [hướng dẫn dành cho Công cụ dành cho nhà phát triển React](https://www.debugbear.com/blog/react-devtools) để bắt đầu học cách sử dụng nó (đừng lo lắng nếu bạn chưa hiểu một số chi tiết).
1. Cố gắng dọn dẹp dự án `my-first-react-app` của bạn để nó không còn render trang mặc định nữa. Xem liệu bạn có thể làm cho nó render thông báo "Xin chào, Thế giới!" thay vào đó hãy nhắn tin.

</div>
