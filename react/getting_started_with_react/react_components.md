### Giới thiệu

Trong bài học này, chúng ta sẽ tìm hiểu các khái niệm cơ bản về các thành phần React - ​​chức năng của chúng và cách viết chúng. Đảm bảo sử dụng dự án bạn đã thiết lập trong bài học trước, nhưng cố gắng không sao chép và dán bất kỳ mã nào trong khi bạn đang viết mã.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Tìm hiểu về các thành phần React.
- Hiểu cách tạo các thành phần.
- Mô tả vị trí các thành phần trong dự án React.

### Thành phần là gì?

Cái hay của React là nó cho phép bạn chia UI (Giao diện người dùng) thành các phần độc lập có thể tái sử dụng mà chúng ta sẽ gọi là các thành phần. Hình ảnh sau đây sẽ cho bạn ý tưởng về cách thực hiện điều đó khi xây dựng một ứng dụng rất cơ bản.

![Ví dụ về thành phần](https://cdn.statically.io/gh/TheOdinProject/curriculum/91485eec76445d86b29d35894e83324e2967f2fb/react/imgs/00.png)

Ví dụ: trang web này có thể được chia thành các thành phần sau:

- `App`, đại diện cho ứng dụng chính của bạn và sẽ là cha mẹ của tất cả các thành phần khác.
- `Navbar`, đây sẽ là thanh điều hướng.
- `MainArticle`, đây sẽ là thành phần hiển thị nội dung chính của bạn.
- `NewsletterForm`, là biểu mẫu cho phép người dùng nhập email của họ để nhận bản tin hàng tuần.

Hãy coi các khối có thể tái sử dụng này như các hàm JavaScript có thể nhận một số loại dữ liệu đầu vào và trả về phần tử React.

### Cách tạo thành phần

Để có cảm giác làm việc với các thành phần, chúng ta sẽ thực hành tạo các thành phần chức năng. Các thành phần chức năng là gì? JavaScript hoạt động! Chúng ta hãy xem xét.

```jsx
function Greeting() {
  return <h1>"I swear by my pretty floral bonnet, I will end you."</h1>;
}
```

Điều này có thể trông quen thuộc với bạn - đó là hàm JavaScript, trả về JSX. Mở dự án bạn đang thực hiện, tạo một tệp mới có tên `Greeting.jsx` và trong tệp đó hãy viết thành phần chức năng thủ công của riêng bạn. Đặt tên cho nó bất cứ điều gì bạn muốn và yêu cầu nó trả về bất cứ điều gì JSX bạn muốn.

Bạn xong chưa? Kiểm tra cách đặt tên cho chức năng của bạn! Nó có được viết hoa không? Hãy ghi nhớ sự khác biệt chính này.**Các thành phần React phải được viết hoa** nếu không chúng sẽ không hoạt động như mong đợi, đó là lý do tại sao chúng tôi viết hoa `Greeting()`. Thông tin thêm về điều đó sau.

#### HTML đang làm gì trong JavaScript của tôi?

Đó là JSX. Thoạt nhìn có vẻ chói tai nhưng chúng ta sẽ sớm nhận ra nó tuyệt vời như thế nào. Chúng ta sẽ tìm hiểu tất cả về nó trong các bài học sắp tới!

### Các thành phần sống ở đâu?

Vì vậy, hãy nhớ cách thành phần của chúng ta chỉ xuất hiện trong tệp chuyên dụng của riêng nó? Điều này làm cho nó độc lập với phần còn lại của cơ sở mã! Điều đó có nghĩa là, mặc dù tính độc lập là rất tốt nhưng chúng tôi muốn thành phần này sử dụng chức năng được tạo ở nơi khác và chia sẻ chính nó với các thành phần khác. Làm thế nào chúng ta có thể làm điều này? `import`ing và `export`ing! Trong một thời gian rất dài trong quá trình phát triển React, cần phải có `import` React trong các tệp JavaScript của bạn sử dụng các thành phần React, nhưng kể từ React v17.0 thì điều này không còn cần thiết nữa. Hãy `export` thành phần mới được tạo của chúng ta để các thành phần gốc có thể sử dụng nó như thành phần con trong suốt dự án của bạn.

```jsx
function Greeting() {
  return <h1>"I swear by my pretty floral bonnet, I will end you."</h1>;
}

export default Greeting;
```

Chúng ta xong chưa? Hãy suy nghĩ về điều này - chúng ta đã khai báo thành phần của mình và xuất nó, nhưng `main.jsx` có biết về nó không? Không! Hãy khắc phục điều đó. Hãy nhìn vào `main.jsx`, chúng ta có thể thấy rằng `render()` đang hiển thị thành phần `App`. Hãy thay thế thành phần `App` đó bằng lời chào mới tạo của chúng ta, mà chúng ta sẽ phải đảm bảo rằng trước tiên được nhập đúng cách. Kết quả cuối cùng sẽ trông giống như thế này:

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

Hãy nhớ rằng `<Greeting />` phải được viết hoa! Hãy thử sử dụng chữ thường để nhập, tên hàm và thành phần và xem điều gì sẽ xảy ra! Khi JSX được phân tích cú pháp, React sử dụng cách viết hoa để cho biết sự khác biệt giữa thẻ HTML và phiên bản của thành phần React. `<greeting />` sẽ được hiểu là một phần tử HTML bình thường không có ý nghĩa đặc biệt, thay vì thành phần React mới sáng bóng của bạn.

Nếu không, cứ như vậy, bạn đã nhập và sử dụng thành công thành phần tùy chỉnh đầu tiên của mình, xin chúc mừng!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đã đến lúc tạo ra một số thành phần mới! Sử dụng cùng một dự án, nhưng thử nghiệm với nó, thử trưng bày thứ gì đó giống như món ăn yêu thích của bạn.
    - Mặc dù các thành phần thường được xuất dưới dạng mặc định, hãy thử sử dụng một số xuất được đặt tên thay vì xuất mặc định. Nếu bạn không chắc chắn về cách thực hiện việc này, hãy tham khảo ý kiến ​​người bạn thân nhất của bạn:[tài liệu MDN về báo cáo xuất khẩu](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#description)

</div>
