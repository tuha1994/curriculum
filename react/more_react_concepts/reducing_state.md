### Giới thiệu

Có thể bạn đã từng nghe đến reducer. Trong bài học này, chúng ta sẽ tìm hiểu reducer là *gì*, *khi nào nên dùng* và *cách dùng* reducer trong React với hook `useReducer`.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Reducer là gì.
- Khi nào nên sử dụng reducer.
- Hook `useReducer` là gì.

### Reducer là gì?

Reducer là các hàm thuần túy nhận state trước đó cùng một action rồi trả về state mới.

Action là một object có thuộc tính *type* mô tả thao tác người dùng vừa thực hiện. Object này cũng có thể chứa
bất kỳ thuộc tính nào khác mà reducer của chúng tôi có thể cần để tạo ra trạng thái mới.

Chúng ta có thể định nghĩa một reducer cập nhật trạng thái của bộ đếm như thế này:

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

Hãy nhớ rằng, reducer là các hàm thuần túy nên chúng ta không nên thay đổi trạng thái.

### Khi nào nên sử dụng reducer?

Nếu một component chỉ cần cập nhật trạng thái của nó theo một vài cách đơn giản thì bạn không cần sử dụng reducer.
Có thể nói điều ngược lại khi một component trở nên quá lớn, khó đọc hoặc khó gỡ lỗi do trạng thái logic của nó.

Dùng reducer giúp tách logic state, thậm chí đưa logic đó sang một tệp hoặc thư mục riêng, nhờ vậy component sẽ nhỏ và dễ đọc hơn.

Vì reducer sử dụng *action*, ta có thể dễ dàng lần ngược lỗi liên quan đến state về action đã được dispatch,
và bởi vì reducer của chúng tôi chỉ là các hàm thuần túy nên chúng tôi có thể kiểm tra chúng một cách riêng biệt.

### Cái hook useReducer

React cho phép dùng reducer trong component thông qua hook `useReducer`. Hook này nhận một hàm reducer và state ban đầu, sau đó trả về một mảng gồm hai giá trị: state hiện tại và hàm `dispatch`.

Hàm `dispatch` nhận một object **action**, chuyển object đó cho reducer; giá trị reducer trả về sẽ được dùng để cập nhật state.

```javascript
const [state, dispatch] = useReducer(reducer, { count: 0 });

function handleClick() {
  dispatch({ type: "incremented_count" });
}
```

Tương tự như cách React cập nhật trạng thái bằng hàm `set` trong `useState`, React*chỉ cập nhật trạng thái trong lần kết xuất tiếp theo*sau khi gọi hàm `dispatch`.
Hãy nhớ rằng các chức năng này sử dụng [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) để xác định xem trạng thái có thay đổi hay không, nếu không thì component đó sẽ không render lại.

Việc bạn sử dụng `useState` hay `useReducer` là tùy thuộc vào bạn, cả hai đều tương đương nhau và bạn có thể sử dụng cả hai trong cùng một component.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc qua tài liệu React [Trích xuất logic trạng thái vào reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer) để tìm hiểu thêm về cách sử dụng reducer trong React và cách tái cấu trúc `useState` thành `useReducer`. Đừng quên hoàn thành các thử thách ở cuối.

1. Đọc [useReducer](https://react.dev/reference/react/useReducer) React docs để tìm hiểu thêm về hook này nhé. Hãy chú ý đến phần khắc phục sự cố để biết các sự cố thường gặp mà bạn có thể gặp phải.

</div>
