### Giới thiệu

Trong bài học này, chúng ta sẽ tìm hiểu các khái niệm cơ bản về các thành phần React - chúng hoạt động như thế nào và cách viết chúng. Đảm bảo sử dụng dự án bạn đã thiết lập trong bài học trước, nhưng cố gắng không sao chép và dán bất kỳ mã nào trong khi bạn đang viết mã.

### Tổng Quan Bài Học

Phần này bao gồm tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Tìm hiểu về các thành phần React.
- Hiểu cách tạo các thành phần.
- Mô tả nơi các thành phần cư trú trong một dự án React.

### Các thành phần là gì?

Ưu điểm của React là nó cho phép bạn chia nhỏ giao diện người dùng (Giao diện người dùng) thành các phần có thể tái sử dụng độc lập, mà chúng ta sẽ gọi là các thành phần. Hình ảnh sau đây sẽ cung cấp cho bạn ý tưởng về cách thực hiện điều đó khi xây dựng một ứng dụng rất cơ bản.

![Component Example](https://cdn.statically.io/gh/TheOdinProject/curriculum/91485eec76445d86b29d35894e83324e2967f2fb/react/imgs/00.png)

Ví dụ: trang web này có thể được chia thành các thành phần sau:

- `App`, đại diện cho ứng dụng chính của bạn và sẽ là thành phần chính của tất cả các thành phần khác.
- `Navbar`, sẽ là thanh điều hướng.
- `MainArticle`, sẽ là thành phần hiển thị nội dung chính của bạn.
- `NewsletterForm`, là một biểu mẫu cho phép người dùng nhập email của họ để nhận bản tin hàng tuần.

Hãy nghĩ về những khối có thể tái sử dụng này như các hàm JavaScript có thể lấy một số loại đầu vào và trả về một phần tử React.

### Cách tạo thành phần

Để có được cảm giác làm việc với các thành phần, chúng ta sẽ thực hành tạo ra các thành phần chức năng. Các thành phần chức năng là gì? Các hàm JavaScript! Chúng ta hãy xem.

```jsx
function Greeting() {
  return <h1>"I swear by my pretty floral bonnet, I will end you."</h1>;
}
```

Điều này có thể trông gần như quen thuộc với bạn - đó là một hàm JavaScript, trả về JSX. Mở dự án bạn đang thực hiện, tạo một tệp mới có tên `Greeting.jsx` và trong tệp đó viết thành phần chức năng thủ công của riêng bạn. Đặt tên cho nó bất cứ điều gì bạn muốn, và yêu cầu nó trả lại bất cứ JSX nào bạn muốn.

Bạn đã hoàn tất chưa? Hãy kiểm tra việc đặt tên cho chức năng của bạn! Nó có được viết hoa không? Hãy ghi nhớ sự khác biệt chính này. ** Các thành phần React phải được viết hoa** nếu không chúng sẽ không hoạt động như mong đợi, đó là lý do tại sao chúng tôi viết hoa `Greeting()`. Tìm hiểu thêm về điều đó sau.

#### HTML đang làm gì trong JavaScript của tôi?

Đó là JSX. Lúc đầu nó có vẻ chói tai, nhưng chẳng mấy chốc chúng ta sẽ nhận ra nó tuyệt vời như thế nào. Chúng ta sẽ tìm hiểu tất cả về nó trong các bài học sắp tới!

### Các thành phần sống ở đâu?

Vì vậy, hãy nhớ làm thế nào thành phần của chúng tôi chỉ là treo trong tập tin chuyên dụng của riêng mình? Điều này làm cho nó độc lập với phần còn lại của cơ sở mã! Điều đó nói rằng, trong khi tính độc lập là rất tốt, chúng tôi muốn thành phần sử dụng chức năng được tạo ở nơi khác và chia sẻ chính nó với các thành phần khác. Chúng ta có thể làm điều này như thế nào? `import`ing và `export`ing! Trong một thời gian rất dài phát triển React, `import` React trong các tệp JavaScript của bạn sử dụng các thành phần React, nhưng kể từ React v17.0, nó không còn cần thiết nữa. Hãy `export` thành phần mới được tạo của chúng tôi để các thành phần cha có thể sử dụng nó như một đứa trẻ trong suốt dự án của bạn.

```jsx
function Greeting() {
  return <h1>"I swear by my pretty floral bonnet, I will end you."</h1>;
}

export default Greeting;
```

Chúng ta đã xong chưa? Hãy suy nghĩ về điều này - chúng ta đã khai báo thành phần của mình và xuất nó, nhưng `main.jsx` có biết về nó chưa? Không! Hãy khắc phục điều đó. Hãy nhìn vào `main.jsx`, chúng ta có thể thấy rằng `render()` đang hiển thị thành phần `App`. Hãy thay thế thành phần `App` đó bằng lời chào mới được tạo của chúng tôi, chúng tôi sẽ phải đảm bảo lần đầu tiên được nhập đúng cách. Kết quả cuối cùng sẽ trông như thế này:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import Greeting from "./Greeting.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <Greeting />
  </StrictMode>,
);
```

Hãy nhớ rằng `<Greeting />` nên được viết hoa! Hãy thử sử dụng chữ thường cho việc nhập, tên hàm và thành phần và xem điều gì sẽ xảy ra! Khi JSX được phân tích cú pháp, React sử dụng cách viết hoa để cho biết sự khác biệt giữa thẻ HTML và phiên bản của thành phần React. `<greeting />` sẽ được hiểu là một phần tử HTML bình thường không có ý nghĩa đặc biệt, thay vì thành phần React mới sáng bóng của bạn.

Nếu không, chỉ cần như vậy, bạn đã nhập thành công và sử dụng thành phần tùy chỉnh đầu tiên của mình, xin chúc mừng!

### Nhiệm vụ

<div class="lesson-content__panel" markdown="1">

1. Đã đến lúc tạo ra một số thành phần mới! Sử dụng cùng một dự án, nhưng chơi xung quanh với nó, hãy thử trưng bày một cái gì đó giống như món ăn yêu thích của bạn.
    - While components normally get exported as defaults, try using some named exports instead of default exports. If you're unsure how to do this, consult your best friend: [the MDN documentation about export statements](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#description)

</div>
