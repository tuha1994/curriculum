### Giới thiệu

Có thể bạn đã quen với JSX từ các bài học React trước. Trong bài này, chúng ta sẽ giải thích JSX là *gì*, *tại sao* dùng nó và *cách* dùng nó trong ứng dụng React.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- JSX là gì.
- Sự khác biệt giữa JSX và HTML.

### JSX là gì?

JSX là phần mở rộng cú pháp của JavaScript, cho phép bạn viết đánh dấu giống HTML ngay trong tệp JavaScript. Bạn không bắt buộc phải dùng JSX khi viết component React, nhưng JSX giúp mã ngắn gọn hơn.

Về cơ bản, JSX là cú pháp rút gọn cho [hàm createElement](https://react.dev/reference/react/createElement) của React. Hàm này tạo ra một React element — thực chất là một object đơn giản — nên JSX cuối cùng sẽ được biên dịch thành các object JavaScript.

Tại đây, bạn có thể thấy giá trị được ghi của phần tử div JSX kèm theo một số văn bản:

![Giá trị được ghi của phần tử JSX](https://cdn.statically.io/gh/TheOdinProject/curriculum/1100fab7884b195379664e41b62dfa95588f3a15/react/getting_started_with_react/what_is_jsx/imgs/00.png)

### Tại sao chúng ta sử dụng JSX?

Hầu hết thời gian trong ứng dụng, logic kết xuất và đánh dấu vốn được kết hợp với nhau nhưng chúng tôi thường tách chúng ra bằng cách đặt logic và đánh dấu trong các tệp riêng biệt. JSX cho phép React phân tách các mối quan tâm bằng cách chứa cả logic render và nội dung ở cùng một nơi (một component).

Ngoài ra, JSX mang đến cách làm việc với UI trực quan hơn ngay trong mã, đồng thời giúp React đưa ra các thông báo lỗi và cảnh báo hữu ích hơn.

### Quy tắc của JSX

Nếu sao chép HTML hợp lệ nguyên xi vào một component React, mã có thể không chạy. JSX áp dụng một số quy tắc mà HTML không có.

1. Trả về một phần tử gốc duy nhất.

   Nếu bạn muốn trả về nhiều phần tử trong một component, bạn có thể làm như vậy bằng cách gói chúng trong thẻ cha. Đây có thể là `<div>` hoặc nếu bạn không muốn các phần tử có vùng chứa, bạn có thể sử dụng [đoạn React](https://react.dev/reference/react/Fragment), như vậy: `<>Children</>`

   Đúng:

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

   Trong HTML, nhiều thẻ có khả năng tự đóng và tự gói. Tuy nhiên, trong JSX, chúng ta phải đóng và bọc các thẻ này một cách rõ ràng.

   `<input>` sẽ trở thành `<input />` và `<li>` sẽ trở thành `<li></li>`

   Đúng:

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

   Sai:

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

1. camelCase**Hầu hết**đồ đạc.

   JSX biến thành JavaScript và thuộc tính của các phần tử trở thành khóa của đối tượng JavaScript, vì vậy bạn không thể sử dụng dấu gạch ngang hoặc các từ dành riêng như `class`. Vì điều này, nhiều thuộc tính HTML được viết bằng camelCase. Thay vì `stroke-width`, bạn sẽ sử dụng `strokeWidth` và thay vì `class`, bạn sẽ sử dụng `className`.

   Đúng:

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

   Sai:

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

### Chuyển đổi HTML thành JSX

Sau khi đã nắm các quy tắc của JSX, hãy cùng chuyển một đoạn HTML sang JSX.

```jsx
<h1>Test title</h1>
<svg>
  <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
</svg>
<form>
  <input type="text">
</form>
```

Nếu bạn cố gắng trả về cái này từ một component React, bạn sẽ gặp nhiều lỗi, vì vậy chúng tôi sẽ khắc phục điều đó!

 Hãy làm theo ví dụ này trong môi trường local của bạn. Ngoài ra, bạn có thể mở [react.new](https://react.new/) để nhanh chóng có một môi trường React ngay trên trình duyệt.

Vấn đề đầu tiên chúng tôi nhận được là điều này sẽ không trả về một phần tử gốc duy nhất, vì vậy hãy cung cấp cho nó một vùng chứa.

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

Bạn sẽ thấy một lỗi khác xuất hiện sau khi chúng tôi đã sửa lỗi ban đầu. Điều này không có nghĩa là chúng tôi đã tạo ra lỗi với những thay đổi trước đó, chỉ là React chưa render lỗi này.

Bây giờ, vấn đề thứ hai là chúng ta chưa đóng thẻ `<input>`.

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

Nếu bạn đang theo dõi, tại thời điểm này, bạn sẽ không còn thấy lỗi render trên màn hình nữa, lần này nó sẽ xuất hiện trong bảng điều khiển.

Vấn đề cuối cùng là chúng tôi chưa có thuộc tính camelCased và do đó đang sử dụng các thuộc tính DOM không hợp lệ cho JSX, cụ thể là `stroke-width`.

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

Bây giờ chúng tôi đã áp dụng tất cả các bản sửa lỗi cho các lỗi mà React gây ra, đây là mã JSX hoàn chỉnh có thể được sử dụng trong component React mà không gặp bất kỳ sự cố nào.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Trong các bài học tiếp theo, bạn sẽ dành chút thời gian để đọc tài liệu React. Hầu hết trong số họ sẽ có những nhiệm vụ nhỏ ở cuối để kiểm tra những gì bạn đã đọc. Chúng tôi sẽ không đề cập đến chúng một cách rõ ràng mỗi lần, nhưng hãy chắc chắn thực hiện chúng. Luyện tập tạo nên sự hoàn hảo!

1. Đọc qua tài liệu React[Viết đánh dấu với JSX](https://react.dev/learn/writing-markup-with-jsx)trang để xem lại nội dung chúng tôi đã đề cập ở đây.

1. Đọc qua tài liệu React[JavaScript ở JSX](https://react.dev/learn/javascript-in-jsx-with-curly-braces)trang để giới thiệu bản thân về cách viết logic JavaScript và tham chiếu các giá trị động bên trong đánh dấu của bạn.

</div>
