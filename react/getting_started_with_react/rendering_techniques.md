### Giới thiệu

Bây giờ chúng ta đã tìm hiểu về các thành phần, đạo cụ và JSX, bài học này sẽ đề cập đến cách chúng ta có thể hiển thị nhiều phần tử và hiển thị giao diện người dùng có điều kiện trong JSX.

### Tổng Quan Bài Học

Phần này bao gồm tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiển thị danh sách các thành phần trong JSX.
- Kết xuất UI có điều kiện.

### Kết xuất danh sách các phần tử trong JSX

Giả sử chúng ta muốn tạo một thành phần liệt kê nhiều động vật:

```jsx
function App() {
  return (
    <div>
      <h1>Animals: </h1>
      <ul>
        <li>Lion</li>
        <li>Cow</li>
        <li>Snake</li>
        <li>Lizard</li>
      </ul>
    </div>
  );
}
```

Nó hoàn toàn có thể chấp nhận được, nhưng nếu chúng ta muốn kết xuất nhiều hơn bốn thì sao? Nó có thể tẻ nhạt và dài, và hầu hết thời gian, chúng ta sẽ xử lý một cấu trúc dữ liệu (như một mảng) thay vì mã hóa cứng từng con vật. Trước đây bạn đã biết rằng chúng ta có thể nhúng các biểu thức bên trong JSX bằng dấu ngoặc nhọn. Vì vậy, chúng ta hãy làm điều đó:

```jsx
function App() {
  const animals = ["Lion", "Cow", "Snake", "Lizard"];

  return (
    <div>
      <h1>Animals: </h1>
      <ul>
        {animals.map((animal) => {
          return <li key={animal}>{animal}</li>;
        })}
      </ul>
    </div>
  );
}
```

Chúng tôi xác định một mảng gọi là `animals`. Bây giờ bên trong JSX của chúng tôi, chúng tôi sử dụng `map` để trả về một mảng mới của các phần tử `li`, thêm `animal` làm văn bản của nó. Bây giờ nó sẽ hiển thị giống như đoạn mã trước đó chúng tôi đã viết. Điều này là do JSX có khả năng tự động hiển thị các mảng. Mã sau giống hệt nhau:

```jsx
function App() {
  const animals = ["Lion", "Cow", "Snake", "Lizard"];
  const animalsList = animals.map((animal) => <li key={animal}>{animal}</li>)

  return (
    <div>
      <h1>Animals: </h1>
      <ul>
        {animalsList}
      </ul>
    </div>
  );
}
```

Bạn có thể tò mò về `key` prop là gì trong phần tử `<li>` của chúng tôi. Khi chúng tôi tự động hiển thị danh sách các thành phần, chúng tôi phải cung cấp cho thành phần cấp cao nhất một khóa, mà bạn có thể nghĩ đến bây giờ là ID cho thành phần. Đối với bài học này, các em không cần phải biết lý do tại sao chúng ta cần chìa khóa này hoặc cách thức hoạt động của nó, vì chúng ta sẽ khám phá các chìa khóa trong bài học tiếp theo.

### Kết xuất danh sách các thành phần trong JSX

```jsx
function ListItem(props) {
  return <li>{props.animal}</li>
}

function List(props) {
  return (
    <ul>
      {props.animals.map((animal) => {
        return <ListItem key={animal} animal={animal} />;
      })}
    </ul>
  );
}

function App() {
  const animals = ["Lion", "Cow", "Snake", "Lizard"];

  return (
    <div>
      <h1>Animals: </h1>
      <List animals={animals} />
    </div>
  );
}
```

Chúng tôi đã chuyển phần tử `<ul>` của mình sang một thành phần khác có tên là `List`. Nó vẫn trả về phần tử `<ul>`, nhưng chúng ta có thể làm nhiều hơn nữa với nó như là thành phần riêng của nó. `List` chấp nhận dãy động vật làm chỗ dựa và chuyển mỗi động vật riêng lẻ làm chỗ dựa cho thành phần `ListItem`.

Tất nhiên, chúng tôi không chỉ giới hạn ở `<li>`. Khi chúng tôi nói "danh sách", chúng tôi có nghĩa là rất chung chung. Bạn có thể tự động kết xuất nhiều phần tử `<option>` trong `<select>` hoặc nhiều `<div>` như một phần của lưới.

<div class="lesson-note lesson-note--tip" markdown="1">

#### "Thiếu trong xác thực đạo cụ"

Bạn có thể nhận thấy các đường ngoằn ngoèo bên dưới đạo cụ của mình, ví dụ, bên dưới `animal` bên trong thành phần `<ListItem />` ở trên.

Di chuột qua những thứ này sẽ cho bạn biết chúng là `missing in props validation`. Hiện tại, điều này có thể được bỏ qua một cách an toàn vì nó chỉ là một cảnh báo quy tắc ESLint mặc định về các loại prop, một cái gì đó sẽ được đề cập sau trong khóa học.

Bạn có thể muốn tắt quy tắc này bằng cách thêm những điều sau vào tệp `eslint.config.js` của mình:

```javascript
  rules: {
    // Your other rules
    "react/prop-types": "off"
  }
```

</div>

### Kết xuất UI có điều kiện

Hãy để chúng tôi đưa ra một số quyết định trong thành phần của chúng tôi. Điều gì sẽ xảy ra nếu chúng ta chỉ muốn kết xuất một con vật bắt đầu bằng chữ L? Để đưa ra những quyết định này, chúng tôi sẽ sử dụng một số loại biểu thức có điều kiện. Hãy để chúng tôi tiếp tục sử dụng mã ở trên, nhưng để ngắn gọn, chúng tôi sẽ loại bỏ thành phần `<ListItem />`.

#### Sử dụng toán tử bậc ba

Một cách để kết xuất có điều kiện một phần tử là với một toán tử bậc ba, sử dụng một giá trị boolean để quyết định những gì cần kết xuất:

```jsx
function List(props) {
  return (
    <ul>
      {props.animals.map((animal) => {
        return animal.startsWith("L") ? <li key={animal}>{animal}</li> : null;
      })}
    </ul>
  );
}

function App() {
  const animals = ["Lion", "Cow", "Snake", "Lizard"];

  return (
    <div>
      <h1>Animals: </h1>
      <List animals={animals} />
    </div>
  );
}
```

Chúng tôi đang sử dụng [String method `startsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith) to check if the `animal` starts with the letter L. This method either returns true or false.

Nếu con vật bắt đầu bằng chữ L, thì chúng ta trả về phần tử `<li>`, thể hiện con vật cụ thể. Nếu không, chúng tôi trả về `null` để cho biết rằng sẽ không có phần tử nào được hiển thị.

#### Sử dụng toán tử &&

Một cách nhanh chóng khác để kết xuất có điều kiện một phần tử là sử dụng toán tử `&&`.

```jsx
function List(props) {
  return (
    <ul>
      {props.animals.map((animal) => {
        return animal.startsWith("L") && <li key={animal}>{animal}</li>;
      })}
    </ul>
  );
}

function App() {
  const animals = ["Lion", "Cow", "Snake", "Lizard"];

  return (
    <div>
      <h1>Animals: </h1>
      <List animals={animals} />
    </div>
  );
}
```

Chúng tôi sẽ tận dụng giá trị hoàn vốn của `startsWith` với toán tử `&&`. Nếu kết quả của hàm `startsWith` là `true`, thì nó trả về toán hạng thứ hai, là phần tử `<li>` và hiển thị nó. Nếu không, nếu điều kiện là `false`, thì `false` sẽ được trả lại và JSX sẽ không hiển thị gì thay thế.

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Các số có logic VÀ (&&) - một cạm bẫy phổ biến

Khi sử dụng `&&` để kết xuất có điều kiện, không đặt các số ở phía bên trái. Tài liệu React về [conditional rendering](https://react.dev/learn/conditional-rendering#logical-and-operator-) cung cấp thêm chi tiết về điều này trong hộp `Pitfall` trong phần về `&&`.

</div>

#### Các cách khác để kết xuất có điều kiện

Chúng ta cũng có thể sử dụng `if`, `if/else`, và `switch` để kết xuất một cái gì đó có điều kiện.

Lần này chúng ta sẽ có hai điều kiện:

1. Kiểm tra xem thuộc tính `animals` có được cung cấp không
1. Kiểm tra xem độ dài `animals` có lớn hơn 0 không

Chúng tôi sẽ thường xuyên xử lý các danh sách trong tương lai và chúng tôi cũng cần xem xét những gì cần kết xuất nếu danh sách trống hoặc hoàn toàn không tồn tại. Bạn chắc chắn sẽ không muốn nhìn thấy một trang trống, phải không? Hãy để chúng tôi cố gắng thực hiện điều đó:

```jsx
function List(props) {
  if (!props.animals) {
    return <div>Loading...</div>;
  }

  if (props.animals.length === 0) {
    return <div>There are no animals in the list!</div>;
  }

  return (
    <ul>
      {props.animals.map((animal) => {
        return <li key={animal}>{animal}</li>;
      })}
    </ul>
  );
}

function App() {
  const animals = [];

  return (
    <div>
      <h1>Animals: </h1>
      <List animals={animals} />
    </div>
  );
}
```

Trong thành phần `<List />` của chúng tôi, chúng tôi có hai câu lệnh `if` hoạt động như một người bảo vệ ngay lập tức trả về một phần tử dựa trên điều kiện.

Một là kiểm tra xem thuộc tính `animals` có tồn tại hay không và hai là kiểm tra xem độ dài của danh sách có lớn hơn 0 hay không. Trong trường hợp này, danh sách của chúng tôi trống, vì vậy câu lệnh if thứ hai thực thi, ngay lập tức trả về phần tử `<div>` có chứa văn bản "Không có động vật nào trong danh sách".

Nếu chúng tôi xóa thuộc tính `animals`:

```jsx
function App() {
  const animals = [];

  return (
    <div>
      <h1>Animals: </h1>
      <List />
    </div>
  );
}
```

Câu lệnh `if` đầu tiên bây giờ sẽ thực thi và trả về `<div>` với văn bản "Đang tải..." Điều này thường xảy ra khi bạn đang tìm nạp từ API, vì có thể mất một thời gian để thực sự truy xuất dữ liệu, nên thực hành tốt là hiển thị một chỉ báo cho điều đó.

Nếu không có kiểm tra nào được thông qua, thì chúng tôi có dữ liệu cần thiết để hiển thị danh sách thành công. Hãy thử bằng cách thêm các mục vào danh sách `animals` và thêm lại thuộc tính.

Tất nhiên, bạn cũng có thể thực hiện điều này chỉ với các toán tử ternary và `&&`.

```jsx
function List(props) {
  return (
    <>
      {!props.animals ? (
        <div>Loading...</div>
      ) : props.animals.length > 0 ? (
        <ul>
          {props.animals.map((animal) => {
            return <li key={animal}>{animal}</li>;
          })}
        </ul>
      ) : (
        <div>There are no animals in the list!</div>
      )}
    </>
  );
}

// or
function List(props) {
  return (
    <>
      {!props.animals && <div>Loading...</div>}
      {props.animals && props.animals.length > 0 && (
        <ul>
          {props.animals.map((animal) => {
            return <li key={animal}>{animal}</li>;
          })}
        </ul>
      )}
      {props.animals && props.animals.length === 0 && <div>There are no animals in the list!</div>}
    </>
  );
}

function App() {
  const animals = [];

  return (
    <div>
      <h1>Animals: </h1>
      <List animals={animals} />
    </div>
  );
} 
```

Các bậc ba lồng nhau và nhiều nhà khai thác `&&` có thể đáng sợ khi nhìn vào, vì vậy hãy chắc chắn kiểm tra mọi thứ!

### Nhiệm vụ

<div class="lesson-content__panel" markdown="1">

1. Tài liệu React có hướng dẫn tuyệt vời về [conditional rendering](https://react.dev/learn/conditional-rendering). Tăng cường sự hiểu biết của bạn bằng cách đọc qua và giải quyết tất cả các ví dụ!
1. Từ cùng một tài liệu, hãy khám phá thêm về những gì bạn có thể làm với danh sách bằng cách xem qua bài viết [Danh sách kết xuất] ](https://react.dev/learn/rendering-lists). Bạn không phải lo lắng về phần cuối cùng của các phím, vì chúng ta sẽ tìm hiểu về chúng trong bài học tiếp theo.

</div>
