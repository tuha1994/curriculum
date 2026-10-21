### Giới thiệu

Bạn có thể đã nghe nói về bộ giảm tốc trước đây. Trong bài học này chúng ta sẽ đi
học hỏi*Gì*họ là vậy,*khi* sử dụng chúng và*Làm sao*để sử dụng chúng trong React với móc `useReducer`.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Bộ giảm tốc là gì.
- Khi nào nên sử dụng bộ giảm tốc.
- Móc `useReducer` là gì.

### Bộ giảm tốc là gì?

Bộ giảm tốc là các hàm thuần túy lấy trạng thái trước đó và thực hiện hành động để trả về trạng thái mới.

Hành động là một đối tượng có*kiểu*thuộc tính mô tả những gì người dùng đã làm. Đối tượng này cũng có thể chứa
bất kỳ thuộc tính nào khác mà bộ giảm tốc của chúng tôi có thể cần để tạo ra trạng thái mới.

Chúng ta có thể định nghĩa một bộ giảm tốc cập nhật trạng thái của bộ đếm như thế này:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "incremented_count": {
      return { count: state.count + 1 };
    }
    case "decremented_count": {
      return { count: state.count - 1 };
    }
    case "set_count": {
      return { count: action.value };
    }
    default: {
      throw new Error("unknown action: " + action.type);
    }
  }
}
```

Hãy nhớ rằng, bộ giảm tốc là các hàm thuần túy nên chúng ta không nên thay đổi trạng thái.

### Khi nào nên sử dụng bộ giảm tốc?

Nếu một thành phần chỉ cần cập nhật trạng thái của nó theo một vài cách đơn giản thì bạn không cần sử dụng bộ giảm tốc.
Có thể nói điều ngược lại khi một thành phần trở nên quá lớn, khó đọc hoặc khó gỡ lỗi do trạng thái logic của nó.

Bằng cách sử dụng bộ giảm tốc, chúng ta có thể tách logic trạng thái và thậm chí lưu trữ nó trong một tệp hoặc thư mục khác, dẫn đến
thành các thành phần nhỏ hơn dễ đọc hơn.

Kể từ khi sử dụng bộ giảm tốc*hành động*, chúng tôi có thể dễ dàng theo dõi mọi lỗi liên quan đến trạng thái đối với hành động được gửi đi,
và bởi vì bộ giảm tốc của chúng tôi chỉ là các hàm thuần túy nên chúng tôi có thể kiểm tra chúng một cách riêng biệt.

### Cái móc useReducer

React cho phép chúng ta sử dụng bộ giảm tốc trong các thành phần của mình thông qua một hook có tên `useReducer`. Móc này có chức năng giảm tốc và
trạng thái ban đầu làm đối số, sau đó trả về một mảng có hai giá trị: trạng thái hiện tại và hàm `dispatch`.

Hàm `dispatch` này nhận được một **hoạt động** đối tượng làm đối số, được truyền cho hàm rút gọn của chúng tôi và
giá trị trả về từ nó được sử dụng để cập nhật trạng thái.

```javascript
const [state, dispatch] = useReducer(reducer, { count: 0 });

function handleClick() {
  dispatch({ type: "incremented_count" });
}
```

Tương tự như cách React cập nhật trạng thái bằng hàm `set` trong `useState`, React*chỉ cập nhật trạng thái trong lần kết xuất tiếp theo*sau khi gọi hàm `dispatch`.
Hãy nhớ rằng các chức năng này sử dụng [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) để xác định xem trạng thái có thay đổi hay không, nếu không thì thành phần đó sẽ không hiển thị lại.

Việc bạn sử dụng `useState` hay `useReducer` là tùy thuộc vào bạn, cả hai đều tương đương nhau và bạn có thể sử dụng cả hai trong cùng một thành phần.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc qua tài liệu React [Trích xuất logic trạng thái vào bộ giảm tốc](https://react.dev/learn/extracting-state-logic-into-a-reducer) để tìm hiểu thêm về cách sử dụng bộ giảm tốc trong React và cách tái cấu trúc `useState` thành `useReducer`. Đừng quên hoàn thành các thử thách ở cuối.

1. Đọc [useReducer](https://react.dev/reference/react/useReducer) React docs để tìm hiểu thêm về hook này nhé. Hãy chú ý đến phần khắc phục sự cố để biết các sự cố thường gặp mà bạn có thể gặp phải.

</div>
