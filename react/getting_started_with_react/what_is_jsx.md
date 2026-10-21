### Giới thiệu

Có thể bạn đã quen với JSX trong các bài học React trước, trong bài học này chúng tôi sẽ giải thích*Gì*đó là,*Tại sao* chúng tôi sử dụng nó và*Làm sao*để sử dụng nó trong các ứng dụng React của bạn.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- JSX là gì.
- Sự khác biệt giữa JSX và HTML.

### JSX là gì?

JSX là phần mở rộng cú pháp cho JavaScript cho phép bạn viết đánh dấu giống HTML bên trong tệp JavaScript. Không bắt buộc phải sử dụng JSX khi viết các thành phần React nhưng nó giúp việc viết chúng ngắn gọn hơn.

Về cơ bản, JSX là cú pháp của React [chức năng createElement](https://react.dev/reference/react/createElement). Hàm này tạo ra một phần tử React, là một đối tượng đơn giản, do đó JSX sẽ biên dịch thành các đối tượng JavaScript đơn giản.

Tại đây, bạn có thể thấy giá trị được ghi của phần tử div JSX kèm theo một số văn bản:

![Giá trị được ghi của phần tử JSX](https://cdn.statically.io/gh/TheOdinProject/curriculum/1100fab7884b195379664e41b62dfa95588f3a15/react/getting_started_with_react/what_is_jsx/imgs/00.png)

### Tại sao chúng ta sử dụng JSX?

Hầu hết thời gian trong ứng dụng, logic kết xuất và đánh dấu vốn được kết hợp với nhau nhưng chúng tôi thường tách chúng ra bằng cách đặt logic và đánh dấu trong các tệp riêng biệt. JSX cho phép React phân tách các mối quan tâm bằng cách chứa cả logic hiển thị và nội dung ở cùng một nơi (một thành phần).

Trên hết, đây là một cách trực quan, trực quan hơn để làm việc với UI bên trong mã của bạn và cho phép React hiển thị các thông báo cảnh báo và lỗi hữu ích hơn.

### Quy tắc của JSX

Nếu bạn lấy một số HTML hợp lệ và sao chép thẳng vào thành phần React của bạn, nó sẽ không hoạt động. Điều này là do một số quy tắc triển khai JSX không có trong HTML.

1. Trả về một phần tử gốc duy nhất.

   Nếu bạn muốn trả về nhiều phần tử trong một thành phần, bạn có thể làm như vậy bằng cách gói chúng trong thẻ cha. Đây có thể là `<div>` hoặc nếu bạn không muốn các phần tử có vùng chứa, bạn có thể sử dụng [đoạn React](https://react.dev/reference/react/Fragment), như vậy: `<>Children</>`

   Chính xác:

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

   Chính xác:

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

   Không đúng:

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

   Chính xác:

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

   Không đúng:

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

Bây giờ chúng ta đã đề cập đến Quy tắc của JSX, chúng ta sẽ tiến hành chuyển đổi một đoạn HTML thành JSX.

```jsx
<h1>Test title</h1>
<svg>
  <circle cx="25" cy="75" r="20" stroke="green" stroke-width="2" />
</svg>
<form>
  <input type="text">
</form>
```

Nếu bạn cố gắng trả về cái này từ một thành phần React, bạn sẽ gặp nhiều lỗi, vì vậy chúng tôi sẽ khắc phục điều đó!

 Đảm bảo làm theo ví dụ này trong môi trường địa phương của bạn. Ngoài ra, bạn có thể đi đến[react.new](https://react.new/)để thiết lập và sử dụng môi trường React nhanh chóng trong trình duyệt web của bạn.

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

Bạn sẽ thấy một lỗi khác xuất hiện sau khi chúng tôi đã sửa lỗi ban đầu. Điều này không có nghĩa là chúng tôi đã tạo ra lỗi với những thay đổi trước đó, chỉ là React chưa hiển thị lỗi này.

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

Nếu bạn đang theo dõi, tại thời điểm này, bạn sẽ không còn thấy lỗi hiển thị trên màn hình nữa, lần này nó sẽ xuất hiện trong bảng điều khiển.

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

Bây giờ chúng tôi đã áp dụng tất cả các bản sửa lỗi cho các lỗi mà React gây ra, đây là mã JSX hoàn chỉnh có thể được sử dụng trong thành phần React mà không gặp bất kỳ sự cố nào.

### Phân công

<div class="lesson-content__panel" markdown="1">

Trong các bài học tiếp theo, bạn sẽ dành chút thời gian để đọc tài liệu React. Hầu hết trong số họ sẽ có những nhiệm vụ nhỏ ở cuối để kiểm tra những gì bạn đã đọc. Chúng tôi sẽ không đề cập đến chúng một cách rõ ràng mỗi lần, nhưng hãy chắc chắn thực hiện chúng. Luyện tập tạo nên sự hoàn hảo!

1. Đọc qua tài liệu React[Viết đánh dấu với JSX](https://react.dev/learn/writing-markup-with-jsx)trang để xem lại nội dung chúng tôi đã đề cập ở đây.

1. Đọc qua tài liệu React[JavaScript ở JSX](https://react.dev/learn/javascript-in-jsx-with-curly-braces)trang để giới thiệu bản thân về cách viết logic JavaScript và tham chiếu các giá trị động bên trong đánh dấu của bạn.

</div>
