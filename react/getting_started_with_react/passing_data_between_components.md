### Giới thiệu

Đến bây giờ, bạn nên bắt đầu hiểu React và các thành phần có thể tái sử dụng mạnh mẽ như thế nào, nhưng bạn có thể vẫn thắc mắc 'Làm cách nào tôi có thể chia sẻ thông tin giữa các thành phần?' hoặc 'Tôi có thể tùy chỉnh hành vi của các thành phần mỗi lần tôi sử dụng chúng không?'. Trong bài học này, chúng ta sẽ tìm hiểu về React props (viết tắt của thuộc tính) và cách sử dụng props để truyền dữ liệu giữa các thành phần.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Truyền dữ liệu giữa các thành phần.
- Sử dụng dữ liệu để tạo các thành phần có thể tùy chỉnh và tái sử dụng.

### Truyền dữ liệu trong React

Trong React, dữ liệu được truyền từ các thành phần cha mẹ sang các thành phần con thông qua các đạo cụ. Việc truyền dữ liệu này là một chiều, nghĩa là nó chỉ chảy theo một hướng. Mọi thay đổi được thực hiện đối với dữ liệu này sẽ chỉ ảnh hưởng đến các thành phần con đang sử dụng dữ liệu chứ không ảnh hưởng đến các thành phần cha mẹ hoặc anh chị em. Hạn chế đối với luồng dữ liệu này giúp chúng tôi kiểm soát dữ liệu rõ ràng hơn, dẫn đến ít lỗi hơn trong ứng dụng của chúng tôi.

### Sử dụng đạo cụ trong React

Bây giờ chúng ta đã biết*Làm sao*truyền dữ liệu giữa các thành phần, hãy cùng khám phá*Tại sao*đây có thể là một tính năng hữu ích trong React. Hãy xem xét thành phần `Button` sau, thành phần này được hiển thị nhiều lần trong thành phần `App` của chúng tôi.

```jsx
function Button() {
  return (
    <button>Click Me!</button>
  );
}

export default function App() {
  return (
    <div>
      <Button />
      <Button />
      <Button />
    </div>
  );
}
```

Cho đến nay rất tốt phải không? Chúng ta có một nút tái sử dụng rất đẹp mà chúng ta có thể sử dụng bao nhiêu lần tùy thích, chỉ có một vấn đề nhỏ.

Điều gì sẽ xảy ra nếu chúng ta muốn văn bản trong nút thứ hai là “Đừng nhấp vào tôi!”? Ngay bây giờ, chúng ta sẽ phải tạo thành phần nút thứ hai với văn bản khác này.

```jsx
function Button() {
  return (
    <button>Click Me!</button>
  );
}

function Button2() {
  return (
    <button>Don't Click Me!</button>
  );
}

export default function App() {
  return (
    <div>
      <Button />
      <Button2 />
      <Button />
    </div>
  );
}
```

Điều này có vẻ không phải là một vấn đề lớn hiện nay, nhưng nếu chúng ta có 10 nút thì sao? Mỗi cái có văn bản, phông chữ, màu sắc, kích thước khác nhau và bất kỳ biến thể nào khác mà bạn có thể nghĩ ra. Việc tạo một thành phần mới cho mỗi biến thể nút này sẽ nhanh chóng dẫn đến RẤT NHIỀU mã trùng lặp.

Hãy xem bằng cách sử dụng đạo cụ, chúng ta có thể giải thích bất kỳ số lượng biến thể nào bằng một*đơn*thành phần nút.

```jsx
function Button(props) {
  const buttonStyle = {
    color: props.color,
    fontSize: props.fontSize + 'px'
  };

  return (
    <button style={buttonStyle}>{props.text}</button>
  );
}

export default function App() {
  return (
    <div>
      <Button text="Click Me!" color="blue" fontSize={12} />
      <Button text="Don't Click Me!" color="red" fontSize={12} />
      <Button text="Click Me!" color="blue" fontSize={20} />
    </div>
  );
}
```

Có một vài điều đang diễn ra ở đây.

- Thành phần chức năng `Button` hiện nhận được `props` dưới dạng tham số hàm. Các thuộc tính riêng lẻ sau đó được tham chiếu trong thành phần thông qua `props.propertyName`.
- Khi hiển thị các thành phần `Button` trong `App`, các giá trị `prop` được xác định trên mỗi thành phần.
- Kiểu nội tuyến được tạo động và sau đó được áp dụng cho phần tử `button`.

### Phá hủy chống đỡ

Một mẫu rất phổ biến mà bạn sẽ gặp trong React là prop [phá hủy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring). Việc giải nén các đạo cụ của bạn trong các tham số thành phần sẽ cho phép mã ngắn gọn và dễ đọc hơn. Hãy xem hoạt động phá hủy prop trong ví dụ dưới đây.

```jsx
function Button({ text, color, fontSize }) {
  const buttonStyle = {
    color: color,
    fontSize: fontSize + "px"
  };

  return <button style={buttonStyle}>{text}</button>;
}

export default function App() {
  return (
    <div>
      <Button text="Click Me!" color="blue" fontSize={12} />
      <Button text="Don't Click Me!" color="red" fontSize={12} />
      <Button text="Click Me!" color="blue" fontSize={20} />
    </div>
  );
}
```

### Đạo cụ mặc định

Bạn có thể nhận thấy trong các ví dụ trên rằng có một số sự lặp lại khi xác định props trên các thành phần `Button` trong `App`. Để ngừng lặp lại bằng cách xác định lại các giá trị chung này và để bảo vệ ứng dụng của chúng tôi khỏi các giá trị không xác định, chúng tôi có thể xác định các tham số mặc định để đặt giá trị mặc định cho đạo cụ.

```jsx
function Button({ text = "Click Me!", color = "blue", fontSize = 12 }) {
  const buttonStyle = {
    color: color,
    fontSize: fontSize + "px"
  };

  return <button style={buttonStyle}>{text}</button>;
}

export default function App() {
  return (
    <div>
      <Button />
      <Button text="Don't Click Me!" color="red" />
      <Button fontSize={20} />
    </div>
  );
}
```

Như bạn có thể thấy, giờ đây chúng ta chỉ cần cung cấp các giá trị prop cho `Button` khi hiển thị trong `App` nếu chúng khác với các giá trị mặc định được xác định trong các tham số hàm.

Bạn cũng có thể gặp phải việc sử dụng `defaultProps` trong một số cơ sở mã. Theo truyền thống, điều này được sử dụng để đặt giá trị mặc định cho đạo cụ, đặc biệt là trong các thành phần lớp. Đây là giao diện của nó:

```jsx
function Button({ text, color, fontSize }) {
  const buttonStyle = {
    color: color,
    fontSize: fontSize + "px"
  };

  return <button style={buttonStyle}>{text}</button>;
}

Button.defaultProps = {
  text: "Click Me!",
  color: "blue",
  fontSize: 12
};

export default function App() {
  return (
    <div>
      <Button />
      <Button text="Don't Click Me!" color="red" />
      <Button fontSize={20} />
    </div>
  );
}
```

Mặc dù React không còn chấp nhận cách tiếp cận thứ hai đối với các thành phần hàm, nhưng việc hiểu `defaultProps` vẫn hữu ích, đặc biệt là khi làm việc với các thành phần lớp hoặc cơ sở mã cũ hơn.

### Chức năng như đạo cụ

Ngoài việc truyền các biến tới các thành phần con dưới dạng đạo cụ, bạn cũng có thể truyền các hàm. Hãy xem xét ví dụ sau.

```jsx
function Button({ text = "Click Me!", color = "blue", fontSize = 12, handleClick }) {
  const buttonStyle = {
    color: color,
    fontSize: fontSize + "px"
  };

  return (
    <button onClick={handleClick} style={buttonStyle}>
      {text}
    </button>
  );
}

export default function App() {
  const handleButtonClick = () => {
    window.location.href = "https://www.google.com";
  };

  return (
    <div>
      <Button handleClick={handleButtonClick} />
    </div>
  );
}
```

- Hàm `handleButtonClick` được xác định trong thành phần chính.
- Một tham chiếu đến hàm này được chuyển qua dưới dạng giá trị cho prop `handleClick` trên thành phần `Button`.
- Hàm này được nhận trong `Button` và được gọi trong một sự kiện nhấp chuột.

Có một vài điều cần lưu ý ở đây.

- Chúng ta chỉ chuyển qua một tham chiếu tới `handleButtonClick`, tức là chúng tôi không bao gồm dấu ngoặc đơn khi truyền hàm tới `Button`. Nếu chúng ta làm điều gì đó như `handleClick={handleButtonClick()}` thì hàm này sẽ được gọi khi nút hiển thị.

- Mỗi `Button` gọi chức năng này sẽ điều hướng đến cùng một trang. Chúng ta có thể cấu trúc lại hàm và cung cấp đối số trong `Button` để tùy chỉnh chức năng này.

```jsx
function Button({ text = "Click Me!", color = "blue", fontSize = 12, handleClick }) {
  const buttonStyle = {
    color: color,
    fontSize: fontSize + "px"
  };

  return (
    <button onClick={handleClick} style={buttonStyle}>
      {text}
    </button>
  );
}

export default function App() {
  const handleButtonClick = (url) => {
    window.location.href = url;
  };

  return (
    <div>
      <Button handleClick={() => handleButtonClick('https://www.theodinproject.com')} />
    </div>
  );
}
```

Khi cung cấp đối số cho hàm, chúng ta không thể chỉ viết `onClick={handleClick('https://www.theodinproject.com')}` mà thay vào đó phải đính kèm một tham chiếu đến một hàm ẩn danh để sau đó gọi hàm bằng đối số đó. Giống như ví dụ trước, điều này nhằm ngăn việc gọi hàm trong quá trình kết xuất.

<div class="lesson-note" markdown="1" >

#### Triển khai thay thế với các hàm đã được xử lý

Ngoài ra còn có những cách khác để thực hiện hành vi này. Gợi ý:[chức năng cà ri!](https://javascript.info/currying-partials)

</div>

Hy vọng rằng, từ các ví dụ trong bài học này, bây giờ bạn có thể hiểu được các đạo cụ cực kỳ hữu ích như thế nào trong việc viết các thành phần React có thể tái sử dụng và có thể tùy chỉnh. Tuy nhiên, chúng tôi vẫn chỉ sơ lược về những gì React có thể mang lại cho chúng tôi. Hãy tiếp tục sang phần tiếp theo để tìm hiểu thêm!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc qua tài liệu React trên [Truyền đạo cụ cho một thành phần](https://react.dev/learn/passing-props-to-a-component). Đảm bảo chỉnh sửa các ví dụ về mã và thử nghiệm các giá trị prop khác nhau.

</div>
