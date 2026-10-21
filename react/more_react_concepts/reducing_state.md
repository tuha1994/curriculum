### Giới thiệu

Có lẽ bạn đã từng nghe về reducer. Trong bài học này, chúng ta sẽ tìm hiểu *chúng là gì*, *khi nào* nên sử dụng chúng và *cách* sử dụng chúng trong React với hook `useReducer`.

### Tổng quan bài học

Phần này cung cấp tổng quan chung về những chủ đề bạn sẽ học trong bài học này.

- Reducer là gì.
- Khi nào nên sử dụng reducer.
- Hook `useReducer` là gì.

### Reducer là gì?

Reducer là các hàm thuần túy, nhận state trước đó và một action để trả về state mới.

Action là một đối tượng có thuộc tính *type* mô tả việc người dùng đã thực hiện. Đối tượng này cũng có thể chứa bất kỳ thuộc tính nào khác mà reducer cần để tạo ra state mới.

Chúng ta có thể định nghĩa một reducer cập nhật state của bộ đếm như sau:

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

Hãy nhớ rằng reducer là hàm thuần túy, vì vậy chúng ta không nên thay đổi trực tiếp state.

### Khi nào nên sử dụng reducer?

Nếu một component chỉ cần cập nhật state theo một vài cách đơn giản, bạn không cần dùng reducer. Ngược lại, reducer hữu ích khi component đã trở nên quá lớn, khó đọc hoặc khó gỡ lỗi vì logic state của nó.

Bằng cách sử dụng reducer, chúng ta có thể tách logic state ra, thậm chí lưu logic đó trong một tệp hoặc thư mục khác, từ đó tạo ra các component nhỏ hơn và dễ đọc hơn.

Vì reducer sử dụng *action*, chúng ta có thể dễ dàng lần ngược các lỗi liên quan đến state về action đã được dispatch. Ngoài ra, vì reducer chỉ là các hàm thuần túy, chúng ta có thể kiểm thử chúng một cách độc lập.

### Hook useReducer

React cho phép chúng ta sử dụng reducer trong component thông qua một hook có tên `useReducer`. Hook này nhận một hàm reducer và state ban đầu làm đối số, sau đó trả về một mảng gồm hai giá trị: state hiện tại và một hàm `dispatch`.

Hàm `dispatch` nhận một đối tượng **action** làm đối số. Đối tượng này được truyền cho hàm reducer, và giá trị mà reducer trả về sẽ được dùng để cập nhật state.

```javascript
const [state, dispatch] = useReducer(reducer, { count: 0 });

function handleClick() {
  dispatch({ type: "incremented_count" });
}
```

Tương tự cách React cập nhật state bằng hàm `set` trong `useState`, React *chỉ cập nhật state trong lần render tiếp theo* sau khi gọi hàm `dispatch`. Hãy nhớ rằng các hàm này sử dụng [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) để xác định state có thay đổi hay không; nếu state không thay đổi, component sẽ không render lại.

Việc sử dụng `useState` hay `useReducer` tùy thuộc vào bạn. Hai hook này tương đương nhau và bạn có thể sử dụng cả hai trong cùng một component.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc tài liệu React [Đưa logic state vào reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer) để tìm hiểu thêm về cách sử dụng reducer trong React và cách cấu trúc lại `useState` thành `useReducer`. Đừng quên hoàn thành các thử thách ở cuối.

1. Đọc tài liệu React [useReducer](https://react.dev/reference/react/useReducer) để tìm hiểu thêm về hook này. Hãy chú ý đến phần xử lý sự cố để biết những vấn đề thường gặp mà bạn có thể gặp phải.

</div>