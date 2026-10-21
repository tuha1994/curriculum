### Giới thiệu

Từ đầu đến giờ, chúng ta chủ yếu làm việc với các thành phần hàm, xét cả cách viết lẫn cú pháp. Đây là cách viết phổ biến hiện nay, nhưng bạn cũng sẽ gặp cú pháp dựa trên `class`. Trong bài học này, chúng ta sẽ tìm hiểu cách viết một thành phần lớp và cách dùng các khái niệm như props và state trong đó.

### Tổng quan bài học

Phần này tóm lược những nội dung bạn sẽ học trong bài.

- Các thành phần lớp được viết như thế nào
- Cách sử dụng props và state trong các thành phần của lớp.
- Công dụng của từ khóa `this` trong các thành phần lớp.

### Các mẫu thành phần lịch sử React

Ở các bài trước, bạn đã làm quen với thành phần hàm và cách viết component hiện nay. Tuy nhiên, khi React mới ra mắt, component không được viết như vậy.

Nếu xem một codebase React cũ, bạn sẽ thấy rất nhiều class. Đó là các thành phần lớp. Trước tháng 2 năm 2019, thành phần hàm còn được gọi là “stateless” vì chưa có cách quản lý state bên trong chúng. Điều này thay đổi khi hook xuất hiện, giúp component ngắn gọn và dễ đọc hơn.

Trong công việc, rất có thể bạn sẽ phải bảo trì mã kế thừa, nên việc gặp thành phần lớp là điều khó tránh. Hãy cùng tìm hiểu cách chúng hoạt động.

### Xây dựng thành phần lớp

Sau khi đã biết thành phần hàm, hãy thử xây dựng một thành phần lớp từ đó. Thông thường, bạn sẽ tách một component lớn thành những component nhỏ, có thể tái sử dụng; nhưng trong bài tập này, chúng ta sẽ giữ mọi thứ trong một component.

Trước hết, hãy fork rồi clone [repo ví dụ về React](https://github.com/TheOdinProject/react-examples) nếu bạn chưa làm. `cd` vào thư mục `class-components/`, cài dependency bằng `npm install`, rồi chạy máy chủ dev bằng `npm run dev`. Bạn sẽ thấy hai phiên bản của cùng một tính năng: một dùng component hàm, một dùng component lớp. Hãy bắt đầu bằng cách đọc component hàm trong `src/components/FunctionalInput.jsx`. Đây là đoạn mã khá dài, nên hãy đọc kỹ vài lượt.

#### Sự khởi đầu của một thành phần dựa trên lớp

Trong `src/components/ClassInput.jsx`, bạn sẽ thấy phiên bản component lớp hoàn chỉnh. Hãy lùi lại một chút và xem từng bước cách nó được xây dựng từ phiên bản hàm. Điều đầu tiên cần có là, *drumroll*, một class! Nhưng đây không thể là class bất kỳ: nó phải có những thuộc tính cần thiết để trở thành component React. React cung cấp các thuộc tính đó qua class `Component`, và ta tạo component bằng cách kế thừa class này như dưới đây:

```jsx
import { Component } from "react";

class ClassInput extends Component {
  // Some code goes here
}

export default ClassInput;
```

#### Việc sử dụng hàm tạo và đạo cụ

Một lớp thường không đầy đủ nếu không có hàm tạo, vì vậy hãy thêm một lớp.

Các props được truyền vào thành phần này sẽ được chuyển đến `constructor` của lớp. Điều này, cùng với phương thức `super`, cho phép bạn sử dụng các đạo cụ trong ngữ cảnh `this`, trong đó,*cái này* trường hợp, đề cập đến thành phần. Nếu bạn thực sự tò mò về công dụng thực sự của `super`, hãy xem [Tài liệu MDN về từ khóa `super`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super).

Nếu thành phần của bạn không có bất kỳ đạo cụ nào, bạn có thể để `constructor` và `super` không có đối số.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);
  }
  // Some more code goes here
}

export default ClassInput;
```

#### Cách bạn có thể kết xuất JSX

Bây giờ props có thể được truy cập bên trong thành phần lớp, bước tiếp theo là tìm cách render JSX.

Bạn chỉ cần trả về JSX từ phương thức `render`. Các props đã khai báo trong constructor cũng có thể dùng ở đây.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);
  }
  // Some more code goes here

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
       {/* The input field to enter Todos */}
        <form>
          <label htmlFor="task-entry">Enter a task: </label>
          <input type="text" id="task-entry" name="task-entry" />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        {/* The list of all the Todos, displayed */}
        <ul></ul>
      </section>
    );
  }
}

export default ClassInput;
```

Hãy lưu ý cách các đạo cụ được cung cấp bởi `this`, không giống như thành phần chức năng mà chúng ta đã thấy ban đầu?

#### Dùng state và quản lý ngữ cảnh

Tiếp theo là state. Trong thành phần lớp, state được khởi tạo trong constructor.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [],
      inputVal: "",
    };
  }
  // Some more code goes here

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
        <form>
          <label htmlFor="task-entry">Enter a task: </label>
          <input type="text" id="task-entry" name="task-entry" />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        <ul></ul>
      </section>
    );
  }
}

export default ClassInput;
```

Phương thức `setState` có sẵn dùng để cập nhật state. Hãy nhớ rằng không được thay đổi trực tiếp state; mỗi lần cập nhật, bạn phải tạo state mới.

Bây giờ hãy hoàn thiện component bằng cách bổ sung toàn bộ chức năng. Mọi thứ gần như giống nhau, chỉ có một điểm cần lưu ý: mỗi phương thức phải `bind` `this` của nó với instance của class để có thể sử dụng đúng ngữ cảnh. Theo mặc định, phương thức trong class không được bind với instance. Thông thường, ta làm việc này trong constructor thay vì lúc chạy [trong phương thức render]. Nếu định nghĩa phương thức bằng cú pháp hàm mũi tên, `this` sẽ tự động bind với instance của class và không cần bind trong constructor.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [],
      inputVal: "",
    };

    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleInputChange(e) {
    this.setState((state) => ({
      ...state,
      inputVal: e.target.value,
    }));
  }

  handleSubmit(e) {
    e.preventDefault();
    this.setState((state) => ({
      todos: state.todos.concat(state.inputVal),
      inputVal: "",
    }));
  }

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
        <form onSubmit={this.handleSubmit}>
          <label htmlFor="task-entry">Enter a task: </label>
          <input
            type="text"
            id="task-entry"
            name="task-entry"
            value={this.state.inputVal}
            onChange={this.handleInputChange}
          />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        <ul>
          {this.state.todos.map((todo) => (
            <li key={todo}>{todo}</li>
          ))}
        </ul>
      </section>
    );
  }
}

export default ClassInput;
```

Và thế là chúng ta đã tạo thành công một thành phần dựa trên lớp dễ dàng như vậy!

### Phân công

<div class="lesson-content__panel" markdown="1">

Vì chúng ta đã có một thành phần dựa trên lớp hoàn chỉnh nên hãy thêm chức năng bổ sung cho nó.

1. Thực hiện một nút xóa cho mỗi nhiệm vụ. Nút xóa sẽ xóa tác vụ cụ thể đó khỏi mảng trạng thái, do đó xóa chính tác vụ đó! Việc tạo kiểu không phải là ưu tiên tại thời điểm này nhưng thẻ nút phải được tạo kiểu theo mặc định.
1. Triển khai thành phần lớp mới, `Count`, hiển thị số lượng việc cần làm tại bất kỳ thời điểm nào. Kết xuất nó ở đâu đó trong thành phần `ClassInput`.
1. Thực hiện một nút chỉnh sửa cho mỗi nhiệm vụ. Nó sẽ thay thế việc cần làm bằng một trường đầu vào và tự thay đổi nút thành 'Gửi lại' để có thể lưu các chỉnh sửa. Đây là một nhiệm vụ tương đối khó hơn, bạn sẽ được khen ngợi khi hoàn thành nó!

</div>
