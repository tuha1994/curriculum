### Giới thiệu

Bây giờ chúng ta đã tìm hiểu về các thành phần, đạo cụ và JSX, bài học này sẽ đề cập đến cách chúng ta có thể kết xuất nhiều phần tử và kết xuất có điều kiện UI trong JSX.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiển thị danh sách các thành phần trong JSX.
- Hiển thị có điều kiện UI.

### Hiển thị danh sách các phần tử trong JSX

Giả sử chúng ta muốn tạo một thành phần liệt kê nhiều loài động vật:

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

Điều này hoàn toàn có thể chấp nhận được, nhưng nếu chúng ta muốn hiển thị nhiều hơn bốn thì sao? Việc này có thể tẻ nhạt và dài dòng, và hầu hết thời gian, chúng ta sẽ xử lý một cấu trúc dữ liệu (như một mảng) thay vì mã hóa cứng từng con vật. Trước đây bạn đã biết rằng chúng ta có thể nhúng biểu thức bên trong JSX bằng dấu ngoặc nhọn. Vì vậy, chúng ta hãy làm điều đó:

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

Chúng tôi xác định một mảng có tên `animals`. Bây giờ, bên trong JSX, chúng ta sử dụng `map` để trả về một mảng phần tử `li` mới, thêm `animal` làm văn bản. Bây giờ nó sẽ hiển thị giống như đoạn mã trước đó mà chúng tôi đã viết. Điều này là do JSX có khả năng tự động hiển thị mảng. Đoạn mã sau giống hệt nhau:

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

Bạn có thể tò mò về prop `key` trong phần tử `<li>` của chúng ta là gì. Khi kết xuất động danh sách các thành phần, chúng tôi phải cung cấp cho thành phần cấp cao nhất một khóa mà bây giờ bạn có thể coi là ID cho thành phần. Đối với bài học này, bạn không cần biết tại sao chúng ta cần phím này hoặc nó hoạt động như thế nào vì chúng ta sẽ khám phá các phím trong bài học tiếp theo.

### Hiển thị danh sách các thành phần trong JSX

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

Chúng tôi đã chuyển phần tử `<ul>` của mình sang một thành phần khác có tên `List`. Nó vẫn trả về phần tử `<ul>`, nhưng chúng ta có thể làm được nhiều việc hơn với nó như là thành phần của chính nó. `List` chấp nhận mảng động vật làm chỗ dựa và chuyển từng con vật riêng lẻ làm chỗ dựa cho thành phần `ListItem`.

Tất nhiên, chúng tôi không chỉ giới hạn ở `<li>`. Khi chúng tôi nói "danh sách", chúng tôi muốn nói điều đó rất chung chung. Bạn có thể kết xuất linh hoạt nhiều phần tử `<option>` trong `<select>` hoặc nhiều `<div>` như một phần của lưới.

<div class="lesson-note lesson-note--tip" markdown="1">

#### "Thiếu xác thực đạo cụ"

Bạn có thể nhận thấy các dòng nguệch ngoạc bên dưới đạo cụ của mình, chẳng hạn như bên dưới `animal` bên trong thành phần `<ListItem />` ở trên.

Di chuột qua những thứ này sẽ cho bạn biết chúng là `missing in props validation`. Hiện tại, điều này có thể được bỏ qua một cách an toàn vì đây chỉ là cảnh báo quy tắc ESLint mặc định về các loại prop, nội dung nào đó sẽ được đề cập ở phần sau của khóa học.

Bạn có thể muốn tắt quy tắc này bằng cách thêm phần sau vào tệp `eslint.config.js` của mình:

```javascript
  rules: {
    // Your other rules
    "react/prop-types": "off"
  }
```

</div>

### Hiển thị có điều kiện UI

Hãy để chúng tôi đưa ra một số quyết định trong thành phần của chúng tôi. Điều gì sẽ xảy ra nếu chúng ta chỉ muốn hiển thị một con vật bắt đầu bằng chữ L? Để đưa ra những quyết định này, chúng ta sẽ sử dụng một số loại biểu thức điều kiện. Chúng ta hãy tiếp tục sử dụng mã ở trên, nhưng để cho ngắn gọn, chúng tôi sẽ xóa thành phần `<ListItem />`.

#### Sử dụng toán tử ternary

Một cách để hiển thị có điều kiện một phần tử là sử dụng toán tử bậc ba, sử dụng giá trị boolean để quyết định nội dung cần hiển thị:

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

Chúng tôi đang sử dụng [Phương thức chuỗi `startsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith) để kiểm tra xem `animal` có bắt đầu bằng chữ L hay không. Phương thức này trả về đúng hoặc sai.

Nếu con vật bắt đầu bằng chữ L thì chúng ta trả về phần tử `<li>` để hiển thị con vật cụ thể. Nếu không, chúng ta trả về `null` để cho biết rằng sẽ không có phần tử nào được hiển thị.

#### Sử dụng toán tử &&

Một cách nhanh chóng khác để hiển thị có điều kiện một phần tử là sử dụng toán tử `&&`.

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

Chúng ta sẽ tận dụng giá trị trả về của `startsWith` với toán tử `&&`. Nếu kết quả của hàm `startsWith` là `true` thì nó sẽ trả về toán hạng thứ hai, là phần tử `<li>` và hiển thị nó. Ngược lại, nếu điều kiện là `false` thì `false` sẽ được trả về và JSX sẽ không hiển thị gì ở vị trí của nó.

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Các số có logic AND (&&) - một cạm bẫy phổ biến

Khi sử dụng `&&` để hiển thị có điều kiện, không đặt số ở phía bên trái. Tài liệu React trên [kết xuất có điều kiện](https://react.dev/learn/conditional-rendering#logical-and-operator-) cung cấp thêm chi tiết về điều này trong hộp `Pitfall` trong phần về `&&`.

</div>

#### Các cách khác để kết xuất có điều kiện

Chúng ta cũng có thể sử dụng `if`, `if/else` và `switch` để hiển thị một cái gì đó có điều kiện.

Lần này chúng ta sẽ có hai điều kiện:

1. Kiểm tra xem thuộc tính `animals` có được cung cấp không
1. Kiểm tra xem độ dài `animals` có lớn hơn 0 không

Chúng ta sẽ thường xuyên xử lý các danh sách trong tương lai và chúng ta cũng cần xem xét những gì sẽ hiển thị nếu danh sách trống hoặc hoàn toàn không tồn tại. Bạn chắc chắn sẽ không muốn nhìn thấy một trang trống phải không? Chúng ta hãy thử thực hiện điều đó:

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

Trong thành phần `<List />` của chúng tôi, chúng tôi có hai câu lệnh `if` đóng vai trò là người bảo vệ trả về ngay một phần tử dựa trên điều kiện.

Một là kiểm tra xem thuộc tính `animals` có tồn tại hay không và hai là kiểm tra xem độ dài của danh sách có lớn hơn 0 hay không. Trong trường hợp này, danh sách của chúng tôi trống, vì vậy câu lệnh if thứ hai sẽ thực thi, ngay lập tức trả về phần tử `<div>` có chứa văn bản "Không có động vật nào trong danh sách".

Nếu chúng ta xóa thuộc tính `animals`:

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

Câu lệnh `if` đầu tiên bây giờ sẽ thực thi và trả về `<div>` với nội dung "Đang tải..." Điều này thường xảy ra khi bạn đang tìm nạp từ API, vì có thể mất một chút thời gian để thực sự truy xuất dữ liệu, nên bạn nên hiển thị chỉ báo cho điều đó.

Nếu không có bước kiểm tra nào vượt qua thì chúng tôi có dữ liệu cần thiết để hiển thị danh sách thành công. Hãy dùng thử bằng cách thêm các mục vào danh sách `animals` và thêm lại thuộc tính.

Tất nhiên, bạn cũng có thể thực hiện việc này chỉ với các toán tử bậc ba và `&&`.

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

Các chim nhạn lồng nhau và nhiều toán tử `&&` có thể khó nhìn, vì vậy hãy nhớ thử nghiệm mọi thứ!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Tài liệu React có hướng dẫn tuyệt vời về [kết xuất có điều kiện](https://react.dev/learn/conditional-rendering). Hãy củng cố sự hiểu biết của bạn bằng cách đọc qua nó và giải quyết tất cả các ví dụ!
1. Từ cùng các tài liệu đó, hãy khám phá thêm về những gì bạn có thể làm với danh sách bằng cách làm việc thông qua [Danh sách hiển thị bài viết](https://react.dev/learn/rendering-lists). Bạn không cần phải lo lắng về phần cuối cùng về phím vì chúng ta sẽ học về chúng trong bài học tiếp theo.

</div>
