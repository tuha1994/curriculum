### Giới thiệu

Bài học này tiếp nối bài trước về state và giới thiệu thêm một số khái niệm, kỹ thuật quản lý state. Hãy cùng tìm hiểu sâu hơn.

### Tổng quan bài học

Phần này tóm lược những nội dung bạn sẽ học trong bài.

- Cách cấu trúc trạng thái.
- Trạng thái cập nhật như thế nào.
- Tìm hiểu về các thành phần được kiểm soát.

### Cách cấu trúc trạng thái

Tổ chức và quản lý state hiệu quả là một trong những phần quan trọng nhất khi xây dựng ứng dụng. Nếu làm không tốt, state dễ trở thành nguồn gốc của lỗi và những vấn đề khó chịu.

Các bài tập sẽ đề cập đến chủ đề này ở nhiều nơi, nhưng nguyên tắc chung là: đừng lưu trong state những giá trị có thể tính được từ props, state hoặc các giá trị hiện có.

#### Không được đột biến state

Đột biến state là điều cần tránh trong React vì có thể tạo ra kết quả khó đoán. Giá trị nguyên thủy vốn bất biến; còn với các giá trị tham chiếu như mảng và object, bạn không được thay đổi trực tiếp chúng. Theo tài liệu React, hãy coi state là *bất biến*. Muốn cập nhật state, luôn dùng hàm `setState`—trong ví dụ dưới đây là `setPerson`.

```jsx
function Person() {
  const [person, setPerson] = useState({ name: "John", age: 100 });

  // BAD - Don't do this!
  const handleIncreaseAge = () => {
    // mutating the current state object
    person.age = person.age + 1;
    setPerson(person);
  };

  // GOOD - Do this!
  const handleIncreaseAge = () => {
    // copy the existing person object into a new object
    // while updating the age property
    const newPerson = { ...person, age: person.age + 1 };
    setPerson(newPerson);
  };

  return (
    <>
      <h1>{person.name}</h1>
      <h2>{person.age}</h2>
      <button onClick={handleIncreaseAge}>Increase age</button>
    </>
  );
}
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Các đối tượng và mảng ở trạng thái

Trong ví dụ trên, hãy chú ý cách chúng ta*tạo nên*một đối tượng mới rồi sao chép các giá trị trạng thái hiện có vào đối tượng mới đồng thời cung cấp giá trị mới cho `age`.
Đó là bởi vì nếu chúng tôi không cung cấp một đối tượng mới cho `setState` thì không đảm bảo trang sẽ được kết xuất lại. Do đó, chúng ta phải luôn cung cấp một đối tượng mới cho `setState` để kích hoạt việc kết xuất lại. `setState` sử dụng [Object.is()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) để xác định trạng thái trước đó có thay đổi hay không.

Đối với các đối tượng và mảng lồng nhau, trạng thái có thể gặp khó khăn nhanh chóng vì bạn cũng sẽ phải sao chép các mục lồng nhau. Hãy cẩn thận khi sử dụng chúng.

</div>

### Cách cập nhật trạng thái

Cập nhật trạng thái không đồng bộ. Điều này ngụ ý rằng bất cứ khi nào bạn gọi hàm `setState`, React sẽ áp dụng bản cập nhật trong **Kế tiếp** kết xuất thành phần. Khái niệm này có thể mất một thời gian để bạn hiểu rõ. Với việc luyện tập nhiều, bạn sẽ nhanh chóng thành thạo nó.

Hãy nhớ rằng, các biến trạng thái không có tác dụng phản ứng; thành phần đó là Điều này có thể được hiểu bởi thực tế là việc gọi `setState` sẽ hiển thị lại toàn bộ thành phần thay vì chỉ thay đổi biến trạng thái một cách nhanh chóng.

```jsx
function Person() {
  const [person, setPerson] = useState({ name: "John", age: 100 });

  const handleIncreaseAge = () => {
    console.log("in handleIncreaseAge (before setPerson call): ", person);
    setPerson({ ...person, age: person.age + 1 });
    // we've called setPerson, surely person has updated?
    console.log("in handleIncreaseAge (after setPerson call): ", person);
  };

  // this console.log runs every time the component renders
  // what do you think this will print?
  console.log("during render: ", person);

  return (
    <>
      <h1>{person.name}</h1>
      <h2>{person.age}</h2>
      <button onClick={handleIncreaseAge}>Increase age</button>
    </>
  );
}
```

Đây là nhật ký:

![bảng điều khiển trình duyệt của đoạn mã trên](https://cdn.statically.io/gh/TheOdinProject/curriculum/103edd69831b1f0e946258009fe36a462c70c163/react/states_and_effects/more_on_state/imgs/00.png)

Uh-ồ, chuyện gì đang xảy ra vậy? Hãy chia nhỏ nó ra (bỏ qua `console.log`s kép trong trường hợp kết xuất; điều này sẽ được đề cập trong các bài học sắp tới).

1. Thành phần này hiển thị lần đầu tiên. Biến trạng thái `person` được khởi tạo thành `{ name: 'John', age: 100 }`. "Trong khi kết xuất" `console.log` in biến trạng thái.
1. Nút được nhấp vào sẽ gọi `handleIncreaseAge`. Điều thú vị là `console.log` trước và sau lệnh gọi `setPerson` in ra cùng một giá trị.
1. Thành phần này được hiển thị lại. Biến trạng thái `person` được cập nhật thành `{ name: 'John', age: 101 }`.

Trạng thái `person` vẫn giữ nguyên trong suốt quá trình kết xuất hiện tại của thành phần. Đây là những gì "trạng thái như một ảnh chụp nhanh" đề cập đến. Lệnh gọi `setState` kích hoạt kết xuất lại thành phần và trạng thái `person` được cập nhật lên giá trị mới.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Vòng lặp vô hạn bất ngờ

Điều sau đây sẽ gây ra một vòng lặp vô hạn, nhưng trong một số trường hợp nhất định, React có thể phát hiện các lần hiển thị lại vô hạn và đưa ra lỗi. Bạn có thể đoán tại sao React sẽ đưa ra lỗi ở đây, cảnh báo bạn về quá nhiều lần kết xuất lại không? Ghé qua trong [Máy chủ Discord HÀNG ĐẦU](https://discord.gg/fbFCkYabZB), hãy cho chúng tôi biết lý do và ghi điểm bánh hạnh nhân!

```jsx
function Component() {
  const [count, setCount] = useState(0);

  setCount(count + 1);

  return <h1>{count}</h1>;
}
```

</div>

#### Chức năng cập nhật trạng thái

Một câu hỏi mẹo. Hãy xem cách triển khai khác của `handleIncreaseAge`; bạn nghĩ nó làm gì?

```jsx
const handleIncreaseAge = () => {
  setPerson({ ...person, age: person.age + 1 });
  setPerson({ ...person, age: person.age + 1 });
};
```

Chắc chắn là nó sẽ tăng tuổi lên 2? Không. Đoạn mã trên đang nói với React:

> Xin chào, hãy thay thế `person` của kết xuất hiện tại bằng mức tăng tuổi lên 1. Sau đó, thay thế `person` của kết xuất hiện tại bằng mức tăng tuổi lên 1.

Hãy chú ý đến từ "thay thế". Khi bạn chuyển giá trị vào hàm `setState`, React sẽ thay thế trạng thái hiện tại bằng giá trị bạn đã chuyển vào. Bạn có thể thắc mắc, nếu tôi muốn cập nhật trạng thái nhiều lần bằng trạng thái mới nhất thì sao? Đây là lúc chức năng cập nhật trạng thái xuất hiện.

```jsx
const handleIncreaseAge = () => {
  setPerson((prevPerson) => ({ ...prevPerson, age: prevPerson.age + 1 }));
  setPerson((prevPerson) => ({ ...prevPerson, age: prevPerson.age + 1 }));
};
```

Khi một lệnh gọi lại được chuyển đến hàm `setState`, nó đảm bảo rằng trạng thái mới nhất được chuyển vào dưới dạng đối số cho lệnh gọi lại.

Việc sử dụng trình cập nhật không phải lúc nào cũng cần thiết. Nếu bạn muốn thay đổi trạng thái bằng trạng thái trước đó và muốn tính nhất quán hơn là chi tiết thì bạn có thể cân nhắc sử dụng trình cập nhật.

<div class="lesson-note" markdown=1>

#### React cập nhật trạng thái hàng loạt

Có hai lệnh gọi `setPerson` trong ví dụ trên và từ những gì chúng ta đã tìm hiểu cho đến nay, lệnh gọi `setState` sẽ kích hoạt kết xuất lại thành phần. Vì vậy, thành phần này sẽ phải hiển thị lại hai lần, phải không? Bạn sẽ nói có, nhưng React rất thông minh. Bất cứ khi nào có thể, React sẽ sắp xếp các bản cập nhật trạng thái theo đợt. Ở đây, thành phần chỉ hiển thị lại một lần. Chúng tôi khuyến khích bạn sử dụng `console.log`s để xác minh điều này.

</div>

### Thành phần được kiểm soát

Có các phần tử HTML gốc duy trì trạng thái bên trong của riêng chúng. Phần tử `input` là một ví dụ tuyệt vời. Bạn gõ vào `input` và nó sẽ cập nhật giá trị của chính nó sau mỗi lần nhấn phím. Đối với nhiều trường hợp sử dụng, bạn muốn*điều khiển*giá trị của phần tử `input`, tức là, hãy tự đặt giá trị của nó. Đây là nơi các thành phần được kiểm soát xuất hiện.

```jsx
function CustomInput() {
  const [value, setValue] = useState("");

  return (
    <input
      type="text"
      value={value}
      onChange={(event) => setValue(event.target.value)}
    />
  );
}
```

Thay vì để `input` duy trì trạng thái của chính nó, chúng ta xác định trạng thái của chính mình bằng cách sử dụng hook `useState`. Sau đó, chúng tôi đặt prop `value` của `input` thành biến trạng thái và cập nhật biến trạng thái trên mỗi sự kiện `onChange`. Giờ đây, mỗi khi người dùng nhập nội dung nào đó vào dữ liệu đầu vào, React sẽ đảm bảo bạn có nhận xét, bài đánh giá hoặc bài đăng mới nhất (bất kể nội dung người dùng đang nhập) trong `value`.

Mẫu này cực kỳ hữu ích khi bạn cần kiểm soát nhiều hơn đối với thông tin đầu vào của người dùng, chẳng hạn như xác thực trực tiếp hoặc bất kỳ điều gì khác có thể yêu cầu bạn truy cập giá trị trạng thái mới nhất từ ​​những thông tin đầu vào này. Ngược lại, có, phần tử `input` có thể không được kiểm soát và truy cập giá trị của nó thông qua một số phương thức khác. Bạn không cần phải lo lắng về điều đó vì nó sẽ được đề cập ở phần sau của khóa học. Hiện tại, hãy kiểm soát các thành phần của bạn!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc các bài viết sau từ tài liệu React:

   - [Trạng thái dưới dạng ảnh chụp nhanh](https://react.dev/learn/state-as-a-snapshot)
   - [Lựa chọn cấu trúc nhà nước](https://react.dev/learn/choosing-the-state-structure)
   - [Trạng thái chia sẻ giữa các thành phần](https://react.dev/learn/sharing-state-between-components)

1. Cập nhật thành phần `Person` mà chúng tôi đang sử dụng ở trên.
   - Thêm hai trường nhập riêng biệt cho họ và tên. Một trong hai thứ này sẽ có thể cập nhật tên đầy đủ trong phần tử `h1` sau mỗi lần nhấn phím.
   - Có nhiều cách bạn có thể làm điều này. Hãy ghi nhớ những gì bạn đã học được trong bài học này khi viết mã.

</div>
