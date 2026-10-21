### Giới thiệu

Có lẽ bạn đã khá quen thuộc với JSX qua các bài học React trước đây. Trong bài học này, chúng ta sẽ giải thích JSX *là gì*, *tại sao* sử dụng nó và *cách* sử dụng nó trong các ứng dụng React.

### Tổng quan bài học

Phần này cung cấp tổng quan chung về những chủ đề bạn sẽ học trong bài học này.

- JSX là gì.
- Sự khác nhau giữa JSX và HTML.

### JSX là gì?

JSX là một phần mở rộng cú pháp của JavaScript, cho phép bạn viết mã đánh dấu tương tự HTML bên trong một tệp JavaScript. JSX không bắt buộc khi viết các component React, nhưng giúp việc viết chúng ngắn gọn hơn.

Về cơ bản, JSX là cú pháp đường cho hàm [createElement](https://react.dev/reference/react/createElement) của React. Hàm này tạo ra một phần tử React, vốn là một đối tượng thuần túy, nên JSX được biên dịch thành các đối tượng JavaScript thuần túy.

Dưới đây là giá trị được ghi lại của một phần tử JSX `div` có chứa văn bản:

![Giá trị được ghi lại của phần tử JSX](https://cdn.statically.io/gh/TheOdinProject/curriculum/1100fab7884b195379664e41b62dfa95588f3a15/react/getting_started_with_react/what_is_jsx/imgs/00.png)

### Tại sao chúng ta sử dụng JSX?

Trong ứng dụng, logic kết xuất và mã đánh dấu vốn gắn liền với nhau, nhưng thông thường chúng ta tách chúng ra bằng cách đặt logic và mã đánh dấu trong các tệp riêng biệt. JSX cho phép React phân tách các mối quan tâm bằng cách chứa cả logic kết xuất lẫn nội dung tại cùng một nơi (một component).

Ngoài ra, JSX là một cách trực quan và dễ hiểu hơn để làm việc với giao diện người dùng ngay trong mã của bạn, đồng thời cho phép React hiển thị các thông báo lỗi và cảnh báo hữu ích hơn.

### Các quy tắc của JSX

Nếu bạn lấy một đoạn HTML hợp lệ rồi chép thẳng vào component React, đoạn mã đó sẽ không hoạt động. Nguyên nhân là JSX áp dụng một số quy tắc không có trong HTML.

1. Trả về một phần tử gốc duy nhất.

   Nếu muốn trả về nhiều phần tử trong một component, bạn có thể bọc chúng trong một thẻ cha. Thẻ đó có thể là `<div>`, hoặc nếu không muốn các phần tử có một vùng chứa, bạn có thể dùng [React fragment](https://react.dev/reference/react/Fragment), như sau: `<>Children</>`

   Correct:

   ```jsx
   function App() {
     // Could replace <></> with <div></div>
     return (
       <>
         <h1>Example h1</h1>
         <h2>Example h2</h2>
       </>
     );
   }
    ```

   Incorrect:

   ```jsx
   function App() {
     return (
       <h1>Example h1</h1>
       <h2>Example h2</h2>
     );
   }
   ```

1. Đóng tất cả các thẻ.

   Trong HTML, nhiều thẻ tự đóng và tự bao quanh nội dung. Tuy nhiên, trong JSX, chúng ta phải đóng và bao quanh các thẻ này một cách tường minh.

   `<input>` sẽ trở thành `<input />`, còn `<li>` sẽ trở thành `<li></li>`

   Correct:

   ```jsx
   function App() {
     return (
       <>
         <input />
         <li></li>
       </>
     );
    }
   ```

   Incorrect:

   ```jsx
   function App() {
     return (
       <>
         <input>
         <li>
       </>
     );
   }
   ```

1. Viết camelCase cho **hầu hết** mọi thứ.

   JSX được chuyển thành JavaScript, còn các thuộc tính của phần tử trở thành khóa của đối tượng JavaScript. Vì vậy, bạn không thể dùng dấu gạch ngang hoặc các từ dành riêng như `class`. Do đó, nhiều thuộc tính HTML được viết theo kiểu camelCase. Thay vì `stroke-width`, bạn sẽ dùng `strokeWidth`, và thay vì `class`, bạn sẽ dùng `className`.

   Correct:

   ```jsx
   function App() {
     return (
     <div className="container">
       <svg>
         <circle cx="25" cy="75" r="20" stroke="green" strokeWidth="2" />
       </svg>
     </div>
     );
   }
   ```

   Incorrect:

   ```jsx
   function App() {
     return (
       <div class="container">
         <svg>
           <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
         </svg>
       </div>
     );
   }
   ```

### Chuyển đổi HTML sang JSX

Sau khi đã tìm hiểu các quy tắc của JSX, chúng ta sẽ chuyển một đoạn HTML sang JSX.

```jsx
<h1>Test title</h1>
<svg>
  <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
</svg>
<form>
  <input type="text">
</form>
```

Nếu thử trả về đoạn mã này từ một component React, bạn sẽ nhận được rất nhiều lỗi, vì vậy chúng ta sẽ sửa chúng!

 Hãy thực hành theo ví dụ này trong môi trường cục bộ của bạn. Ngoài ra, bạn có thể truy cập [react.new](https://react.new/) để nhanh chóng khởi chạy một môi trường React trong trình duyệt web.

Vấn đề đầu tiên là đoạn mã này không trả về một phần tử gốc duy nhất, vì vậy hãy thêm một vùng chứa cho nó.

```jsx
<div>
  <h1>Test title</h1>
  <svg>
    <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
  </svg>
  <form>
    <input type="text">
  </form>
</div>
```

Bạn sẽ thấy một lỗi khác xuất hiện sau khi đã sửa lỗi ban đầu. Điều đó không có nghĩa là chúng ta tạo ra lỗi này do những thay đổi trước đó; chỉ là React chưa hiển thị nó mà thôi.

Tiếp theo là vấn đề thứ hai: chúng ta chưa đóng thẻ `<input>`.

```jsx
<div>
  <h1>Test title</h1>
  <svg>
    <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
  </svg>
  <form>
    <input type="text" />
  </form>
</div>
```

Nếu đang thực hành theo, ở bước này bạn sẽ không còn thấy lỗi được hiển thị trên màn hình nữa; lần này lỗi sẽ xuất hiện trong console.

Vấn đề cuối cùng là chúng ta chưa chuyển các thuộc tính sang camelCase, nên đang sử dụng các thuộc tính DOM không hợp lệ đối với JSX, cụ thể là `stroke-width`.

```jsx
<div>
  <h1>Test title</h1>
  <svg>
    <circle cx="25" cy="75" r="20" stroke="green" strokeWidth="2" />
  </svg>
  <form>
    <input type="text" />
  </form>
</div>
```

Sau khi áp dụng tất cả các cách sửa cho những lỗi React đưa ra, đây đã là mã JSX hoàn chỉnh có thể được dùng trong một component React mà không gặp vấn đề nào.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Trong các bài học tiếp theo, bạn sẽ dành thời gian đọc tài liệu React. Hầu hết các bài sẽ có những bài tập nhỏ ở cuối để kiểm tra nội dung bạn đã đọc. Chúng tôi sẽ không       nhắc đến chúng một cách rõ ràng mỗi lần, nhưng hãy nhớ thực hiện. Có thực hành thì mới thành thạo!

1. Đọc trang tài liệu React [Viết mã đánh dấu bằng JSX](https://react.dev/learn/writing-markup-with-jsx) để ôn lại nội dung chúng ta đã đề cập ở đây.

1. Đọc trang tài liệu React [JavaScript trong JSX](https://react.dev/learn/javascript-in-jsx-with-curly-braces) để làm quen với việc viết logic JavaScript và tham chiếu các giá trị động bên trong mã đánh dấu.

</div>