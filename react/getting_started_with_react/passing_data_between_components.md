### Giới thiệu

Đến bây giờ, bạn nên bắt đầu hiểu React và các component có thể tái sử dụng mạnh mẽ như thế nào, nhưng bạn có thể vẫn thắc mắc 'Làm cách nào tôi có thể chia sẻ thông tin giữa các component?' hoặc 'Tôi có thể tùy chỉnh hành vi của các component mỗi lần tôi sử dụng chúng không?'. Trong bài học này, chúng ta sẽ tìm hiểu về React props (viết tắt của thuộc tính) và cách sử dụng props để truyền dữ liệu giữa các component.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Truyền dữ liệu giữa các component.
- Sử dụng dữ liệu để tạo các component có thể tùy chỉnh và tái sử dụng.

### Truyền dữ liệu trong React

Trong React, dữ liệu được truyền từ các component cha mẹ sang các component con thông qua các props. Việc truyền dữ liệu này là một chiều, nghĩa là nó chỉ chảy theo một hướng. Mọi thay đổi được thực hiện đối với dữ liệu này sẽ chỉ ảnh hưởng đến các component con đang sử dụng dữ liệu chứ không ảnh hưởng đến các component cha mẹ hoặc anh chị em. Hạn chế đối với luồng dữ liệu này giúp chúng tôi kiểm soát dữ liệu rõ ràng hơn, dẫn đến ít lỗi hơn trong ứng dụng của chúng tôi.

### Sử dụng props trong React

Bây giờ đã biết *cách* truyền dữ liệu giữa các component, hãy xem vì *sao* đây là một tính năng hữu ích trong React. Xét component `Button` sau, được render nhiều lần trong component `App`:

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

Điều gì sẽ xảy ra nếu chúng ta muốn văn bản trong nút thứ hai là “Đừng nhấp vào tôi!”? Ngay bây giờ, chúng ta sẽ phải tạo component nút thứ hai với văn bản khác này.

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

Điều này có vẻ không phải là một vấn đề lớn hiện nay, nhưng nếu chúng ta có 10 nút thì sao? Mỗi cái có văn bản, phông chữ, màu sắc, kích thước khác nhau và bất kỳ biến thể nào khác mà bạn có thể nghĩ ra. Việc tạo một component mới cho mỗi biến thể nút này sẽ nhanh chóng dẫn đến RẤT NHIỀU mã trùng lặp.

Hãy xem cách dùng props cho phép một *component nút duy nhất* đáp ứng bao nhiêu biến thể cũng được.

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

- Component chức năng `Button` hiện nhận được `props` dưới dạng tham số hàm. Các thuộc tính riêng lẻ sau đó được tham chiếu trong component thông qua `props.propertyName`.
- Khi render các component `Button` trong `App`, các giá trị `prop` được xác định trên mỗi component.
- Kiểu nội tuyến được tạo động và sau đó được áp dụng cho phần tử `button`.

### Phá hủy chống đỡ

Một mẫu rất phổ biến mà bạn sẽ gặp trong React là prop [phá hủy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring). Việc giải nén các props của bạn trong các tham số component sẽ cho phép mã ngắn gọn và dễ đọc hơn. Hãy xem hoạt động phá hủy prop trong ví dụ dưới đây.

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

### Props mặc định

Bạn có thể nhận thấy trong các ví dụ trên rằng có một số sự lặp lại khi xác định props trên các component `Button` trong `App`. Để ngừng lặp lại bằng cách xác định lại các giá trị chung này và để bảo vệ ứng dụng của chúng tôi khỏi các giá trị không xác định, chúng tôi có thể xác định các tham số mặc định để đặt giá trị mặc định cho props.

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

Như bạn có thể thấy, giờ đây chúng ta chỉ cần cung cấp các giá trị prop cho `Button` khi render trong `App` nếu chúng khác với các giá trị mặc định được xác định trong các tham số hàm.

Bạn cũng có thể gặp phải việc sử dụng `defaultProps` trong một số cơ sở mã. Theo truyền thống, điều này được sử dụng để đặt giá trị mặc định cho props, đặc biệt là trong các component lớp. Đây là giao diện của nó:

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

Mặc dù React không còn chấp nhận cách tiếp cận thứ hai đối với các component hàm, nhưng việc hiểu `defaultProps` vẫn hữu ích, đặc biệt là khi làm việc với các component lớp hoặc cơ sở mã cũ hơn.

### Chức năng như props

Ngoài việc truyền các biến tới các component con dưới dạng props, bạn cũng có thể truyền các hàm. Hãy xem xét ví dụ sau.

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

- Hàm `handleButtonClick` được xác định trong component chính.
- Một tham chiếu đến hàm này được chuyển qua dưới dạng giá trị cho prop `handleClick` trên component `Button`.
- Hàm này được nhận trong `Button` và được gọi trong một sự kiện nhấp chuột.

Có vài điểm cần lưu ý:

- Chúng ta chỉ chuyển qua một tham chiếu tới `handleButtonClick`, tức là chúng tôi không bao gồm dấu ngoặc đơn khi truyền hàm tới `Button`. Nếu chúng ta làm điều gì đó như `handleClick={handleButtonClick()}` thì hàm này sẽ được gọi khi nút render.

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

Khi truyền đối số cho hàm, ta không thể viết trực tiếp `onClick={handleClick('https://www.theodinproject.com')}`. Thay vào đó, cần truyền một hàm ẩn danh rồi gọi hàm bên trong nó. Cũng như ví dụ trước, cách này ngăn hàm được gọi ngay trong lúc render.

<div class="lesson-note" markdown="1" >

#### Cách triển khai khác bằng hàm curry

Có những cách khác để triển khai hành vi này. Gợi ý: [hàm curry!](https://javascript.info/currying-partials)

</div>

Hy vọng rằng, từ các ví dụ trong bài học này, bây giờ bạn có thể hiểu được các props cực kỳ hữu ích như thế nào trong việc viết các component React có thể tái sử dụng và có thể tùy chỉnh. Tuy nhiên, chúng tôi vẫn chỉ sơ lược về những gì React có thể mang lại cho chúng tôi. Hãy tiếp tục sang phần tiếp theo để tìm hiểu thêm!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc qua tài liệu React trên [Truyền props cho một component](https://react.dev/learn/passing-props-to-a-component). Đảm bảo chỉnh sửa các ví dụ về mã và thử nghiệm các giá trị prop khác nhau.

</div>
