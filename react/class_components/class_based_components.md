### Giới thiệu

Tất cả các thành phần cho đến nay đều có chức năng về kiểu dáng và cú pháp. Điều này hiện phổ biến nhưng bạn cũng sẽ thấy cú pháp dựa trên `class` khác. Trong bài học này, chúng ta khám phá cách viết một thành phần dựa trên lớp và cách sử dụng các khái niệm như đạo cụ và trạng thái trong một thành phần.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Các thành phần lớp được viết như thế nào
- Cách sử dụng props và state trong các thành phần của lớp.
- Công dụng của từ khóa `this` trong các thành phần lớp.

### Các mẫu thành phần lịch sử React

Trong các bài học trước, bạn đã được giới thiệu về các thành phần chức năng và các mẫu cơ bản mà ngày nay các thành phần được viết. Tuy nhiên, các thành phần React không giống như vậy khi React được giới thiệu.

Nếu bạn nhìn vào bất kỳ cơ sở mã React nào cũ hơn, bạn sẽ nhận thấy rất nhiều lớp. Chúng được gọi là các thành phần dựa trên lớp. Trước tháng 2 năm 2019, các thành phần chức năng còn được gọi là không có trạng thái vì không có cách nào để quản lý trạng thái trong đó. Điều này đã được thay đổi khi các hook được giới thiệu, dẫn đến các thành phần ít dài dòng hơn và 'gọn gàng hơn'.

Trong sự nghiệp của mình, rất có thể bạn sẽ phải xử lý mã kế thừa, vì vậy sẽ có những ngày bạn phải xử lý các thành phần của lớp. Chúng ta hãy xem xét sự phức tạp của một thành phần dựa trên lớp và cách chúng hoạt động.

### Xây dựng thành phần lớp

Như chúng ta đã biết về các thành phần chức năng, chúng ta hãy xây dựng một thành phần dựa trên lớp từ một thành phần chức năng. Thông thường, bạn sẽ muốn chia nội dung của một thành phần, giống như thành phần chúng ta sử dụng, thành các thành phần nhỏ hơn, có thể tái sử dụng, nhưng với mục đích của bài tập này, chúng ta sẽ tập trung vào mọi thứ trong một thành phần.

Đầu tiên, đi và phân nhánh rồi sao chép [repo ví dụ phản ứng](https://github.com/TheOdinProject/react-examples) nếu bạn chưa làm như vậy. `cd` vào thư mục `class-components/`, cài đặt các phụ thuộc với `npm install` sau đó mở máy chủ dev với `npm run dev`. Bạn sẽ thấy chúng ta có hai phiên bản của cùng một tính năng - một phiên bản được tạo bằng thành phần chức năng, phiên bản còn lại có thành phần lớp. Hãy bắt đầu bằng cách xem thành phần chức năng bên trong `src/components/FunctionalInput.jsx`. Đó là một đoạn mã chắc chắn, vì vậy hãy dành chút thời gian, nhấp một ít nước và đọc nó vài lần.

#### Sự khởi đầu của một thành phần dựa trên lớp

Bạn sẽ thấy trong `src/components/ClassInput.jsx` rằng chúng tôi có phiên bản hoàn chỉnh dựa trên lớp học của nó. Hãy lùi lại vài bước và xem từng bước cách nó được tạo lại từ phiên bản chức năng. Điều đầu tiên nó phải có là,*đánh trống*, một lớp học! Nhưng nó không thể chỉ là một lớp khác, nó sẽ cần phải có một số thuộc tính nhất định đủ tiêu chuẩn để trở thành thành phần React. React cung cấp cho chúng ta tất cả các thuộc tính đó trên một lớp có tên là `Component` và chúng ta có thể viết các thành phần của mình bằng cách mở rộng lớp đã cho, như được hiển thị bên dưới:

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

Bây giờ các đạo cụ có thể được truy cập bên trong thành phần lớp, vấn đề tiếp theo là tìm cách hiển thị JSX.

Chà, bạn có thể làm điều đó bằng cách trả về JSX của bạn từ phương thức `render`! Bạn cũng có thể sử dụng các đạo cụ mà bạn đã khai báo trong hàm tạo!

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

#### Cách sử dụng trạng thái và quản lý bối cảnh

Tiếp theo là nhà nước. Trong thành phần dựa trên lớp, trạng thái được khởi tạo như một phần của hàm tạo.

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

Phương thức `setState` được xác định trước có thể được sử dụng để thiết lập lại! Hãy nhớ rằng, trạng thái không được thay đổi, do đó, trạng thái mới phải được thiết lập mọi lúc.

Bây giờ, đã đến lúc hoàn thiện nó bằng cách thêm tất cả các chức năng! Nó gần như giống nhau, ngoại trừ một điểm khác biệt duy nhất. Bất cứ khi nào một phương thức được khai báo, bạn phải `bind` của phương thức đó so với phương thức của lớp để làm việc với nó, vì theo mặc định, các phương thức trong một lớp không bị ràng buộc với nó. Thông thường, bạn thực hiện việc này bên trong hàm tạo chứ không phải trong thời gian chạy [trong phương thức kết xuất]. Ngoài ra, nếu bạn xác định các phương thức của mình bằng cú pháp hàm mũi tên, `this` sẽ tự động được liên kết với thể hiện của lớp và bạn có thể bỏ qua việc liên kết nó trong hàm tạo.

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
