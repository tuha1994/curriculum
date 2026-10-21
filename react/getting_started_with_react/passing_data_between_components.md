### Giới thiệu

Bây giờ bạn sẽ bắt đầu hiểu React và các thành phần có thể tái sử dụng mạnh mẽ như thế nào, nhưng bạn có thể tự hỏi ‘Làm thế nào tôi có thể chia sẻ thông tin giữa các thành phần?’ hoặc ‘Tôi có thể tùy chỉnh hành vi của các thành phần của mình mỗi khi tôi sử dụng chúng không?'. Trong bài học này, chúng ta sẽ tìm hiểu về React props (viết tắt của properties) và cách sử dụng props để truyền dữ liệu giữa các component.

### Tổng Quan Bài Học

Phần này bao gồm tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Truyền dữ liệu giữa các thành phần.
- Sử dụng dữ liệu để tạo các thành phần có thể tùy chỉnh có thể tái sử dụng.

### Truyền dữ liệu trong React

Trong React, dữ liệu được chuyển từ các component cha sang component con thông qua các props. Việc truyền dữ liệu này là một chiều, có nghĩa là nó chỉ chảy theo một hướng. Bất kỳ thay đổi nào được thực hiện đối với dữ liệu này sẽ chỉ ảnh hưởng đến các thành phần con sử dụng dữ liệu chứ không ảnh hưởng đến các thành phần cha mẹ hoặc anh chị em ruột. Hạn chế này đối với luồng dữ liệu cho phép chúng ta kiểm soát dữ liệu rõ ràng hơn, dẫn đến ít lỗi hơn trong ứng dụng của chúng ta.

### Sử dụng đạo cụ trong React

Bây giờ chúng ta đã biết *cách* truyền dữ liệu giữa các thành phần, hãy khám phá *tại sao* đây có thể là một tính năng hữu ích trong React. Hãy xem xét thành phần `Button` sau đây, được hiển thị nhiều lần trong thành phần `App` của chúng tôi.

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

Cho đến nay rất tốt phải không? Chúng tôi có một nút có thể tái sử dụng đẹp mắt mà chúng tôi có thể sử dụng bao nhiêu lần tùy thích, chỉ có một vấn đề nhỏ.

Điều gì sẽ xảy ra nếu chúng ta muốn văn bản trong nút thứ hai của mình là "Đừng nhấp vào tôi!"? Ngay bây giờ, chúng ta sẽ phải tạo một thành phần nút thứ hai với văn bản khác này.

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

Điều này có vẻ không phải là một vấn đề lớn ngay bây giờ, nhưng nếu chúng ta có 10 nút thì sao? Mỗi loại có văn bản, phông chữ, màu sắc, kích thước và bất kỳ biến thể nào khác mà bạn có thể nghĩ đến. Tạo một thành phần mới cho mỗi biến thể nút này sẽ rất nhanh chóng dẫn đến rất NHIỀU mã trùng lặp.

Hãy xem làm thế nào bằng cách sử dụng các đạo cụ, chúng ta có thể giải thích cho bất kỳ số lượng biến thể nào với một thành phần nút *đơn*.

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

- Thành phần chức năng `Button` hiện nhận `props` dưới dạng tham số chức năng. Các thuộc tính riêng lẻ sau đó được tham chiếu trong thành phần thông qua `props.propertyName`.
- Khi kết xuất các thành phần `Button` trong `App`, các giá trị `prop` được xác định trên mỗi thành phần.
- Các kiểu nội tuyến được tạo động và sau đó được áp dụng cho phần tử `button`.

### Đề xuất phá hủy cấu trúc

Một mô hình rất phổ biến mà bạn sẽ gặp trong React là prop [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring). Giải nén các đạo cụ của bạn trong các tham số thành phần cho phép mã ngắn gọn và dễ đọc hơn. Hãy xem hoạt động phá hủy prop trong ví dụ dưới đây.

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

Bạn có thể nhận thấy trong các ví dụ trên rằng có một số sự lặp lại khi xác định đạo cụ trên các thành phần `Button` trong `App`. Để ngừng lặp lại chính mình bằng cách xác định lại các giá trị chung này và để bảo vệ ứng dụng của chúng ta khỏi các giá trị không xác định, chúng ta có thể xác định các tham số mặc định để đặt giá trị mặc định cho đạo cụ.

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

Như bạn có thể thấy, bây giờ chúng ta chỉ cần cung cấp các giá trị prop cho `Button` khi kết xuất trong `App` nếu chúng khác với các giá trị mặc định được xác định trong các tham số hàm.

Bạn cũng có thể bắt gặp việc sử dụng `defaultProps` trong một số cơ sở mã. Theo truyền thống, giá trị này được sử dụng để đặt các giá trị mặc định cho đạo cụ, đặc biệt là trong các thành phần lớp. Dưới đây là giao diện:

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

Mặc dù React không còn chấp nhận cách tiếp cận thứ hai cho các thành phần chức năng, nhưng việc hiểu `defaultProps` vẫn hữu ích, đặc biệt là khi làm việc với các thành phần lớp hoặc các cơ sở mã cũ hơn.

### Chức năng làm đạo cụ

Ngoài việc chuyển các biến thông qua các thành phần con làm đạo cụ, bạn cũng có thể chuyển qua các hàm. Hãy xem ví dụ sau.

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

- Hàm `handleButtonClick` được xác định trong thành phần cha.
- Tham chiếu đến hàm này được truyền qua dưới dạng giá trị cho prop `handleClick` trên thành phần `Button`.
- Hàm được nhận trong `Button` và được gọi trên một sự kiện nhấp chuột.

Có một vài điều cần lưu ý ở đây.

- Chúng tôi chỉ chuyển qua tham chiếu đến `handleButtonClick`, tức là chúng tôi không bao gồm dấu ngoặc đơn khi chuyển hàm sang `Button`. Nếu chúng ta làm điều gì đó như `handleClick={handleButtonClick()}` thì chức năng sẽ được gọi là kết xuất nút.

- Mỗi `Button` gọi hàm này sẽ điều hướng đến cùng một trang. Chúng ta có thể cấu trúc lại chức năng và cung cấp một đối số trong `Button` để tùy chỉnh chức năng này.

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

Khi cung cấp một đối số cho hàm, chúng ta không thể chỉ viết `onClick={handleClick('https://www.theodinproject.com')}`, và thay vào đó phải đính kèm một tham chiếu đến một hàm ẩn danh mà sau đó gọi hàm với đối số. Giống như ví dụ trước, điều này là để ngăn chặn chức năng được gọi trong quá trình kết xuất.

<div class="lesson-note" markdown="1" >

#### Thực hiện thay thế với các chức năng cà ri

Ngoài ra còn có các cách khác để thực hiện hành vi này. Gợi ý: [chức năng curried!](https://javascript.info/currying-partials)

</div>

Hy vọng rằng, bây giờ bạn có thể hiểu từ các ví dụ trong bài học này về cách các đạo cụ cực kỳ hữu ích để viết các thành phần React có thể tái sử dụng và có thể tùy chỉnh. Tuy nhiên, chúng tôi vẫn chỉ mới làm sáng tỏ những gì React có thể cung cấp cho chúng tôi. Tiếp tục đến phần tiếp theo để tìm hiểu thêm!

### Nhiệm vụ

<div class="lesson-content__panel" markdown="1">

1. Đọc qua các tài liệu React về [Chuyển đạo cụ cho Component](https://react.dev/learn/passing-props-to-a-component). Đảm bảo chỉnh sửa các ví dụ mã và thử nghiệm với các giá trị prop khác nhau.

</div>
