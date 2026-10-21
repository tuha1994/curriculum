### Giới thiệu

Trong bài học này, chúng ta sẽ đề cập đến các khóa trong React. Chìa khóa là đạo cụ đặc biệt cho các thành phần của chúng ta và chúng ta sẽ tìm hiểu lý do tại sao chúng được sử dụng.

### Tổng Quan Bài Học

Phần này bao gồm tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Chìa khóa là gì và React sử dụng chúng như thế nào.
- Sử dụng khóa tốt và xấu trong các ứng dụng React.

### Tại sao React cần chìa khóa?

Bạn có thể nhớ lại rằng React sử dụng một "DOM ảo" dưới mui xe để quyết định những thứ trong DOM thực để cập nhật, giảm thiểu các hành động không cần thiết. Khi một kết xuất lại xảy ra, đầu tiên nó tái tạo DOM ảo này, khác biệt (so sánh các thay đổi) các Dom ảo mới và trước đó, sau đó chỉ cập nhật DOM thực sự với những thứ thực sự đã thay đổi.

React cần phân biệt được từng component vì mỗi component có props và state riêng. Vì vậy, mỗi component được gán một ID ở bên dưới — một **key**. Ví dụ, khi bạn cập nhật state, đó vẫn là cùng một instance của component và React biết điều này vì key của component chưa thay đổi, nên có thể tránh thao tác không cần thiết. Nếu key thay đổi, React hiểu đây là một instance hoàn toàn mới và tạo nó với state mới.

Thông thường, bạn không cần phải cung cấp các khóa thành phần theo cách thủ công, vì React sẽ tự động xử lý việc này. Tuy nhiên, có một vài trường hợp chúng ta có thể sử dụng chúng.

### Các phím khi kết xuất danh sách

Trong bài học trước, chúng ta đã sử dụng phương pháp `.map()` để lặp qua một mảng dữ liệu và trả về một thành phần, hiển thị danh sách các thành phần. Bây giờ hãy tưởng tượng, nếu bất kỳ mục nào trong danh sách thay đổi, làm thế nào React biết được mục nào cần cập nhật đằng sau hậu trường?

Nếu danh sách thay đổi, chúng ta có thể muốn một trong hai điều xảy ra:

1. Hoàn toàn kết xuất lại toàn bộ danh sách
1. Săn lùng các mục cụ thể đã được thay đổi và chỉ kết xuất lại các mục đó

Chúng tôi muốn React chỉ săn lùng (các) mục đã thay đổi và KHÔNG hiển thị lại toàn bộ danh sách (để tránh công việc không cần thiết). Do đó, mỗi mục trong danh sách đó cần một chìa khóa.

Nếu chúng ta mã hóa cứng các thành phần trong JSX, chúng ta có thể để React tự động xử lý các khóa, vì các hướng dẫn là rõ ràng và tĩnh; trên các lần render lại, không có sự mơ hồ giữa thành phần này và thành phần khác.

Tuy nhiên, nếu kết xuất danh sách bằng cách ánh xạ qua một mảng thì mọi thứ là động. Ta chỉ viết một giá trị trả về cho phương thức `.map()`, nhưng kết quả có thể là 2, 3 hoặc thậm chí 100 instance của component đó; cả số lượng và thứ tự đều có thể thay đổi giữa các lần render. React không thể tự cung cấp key theo cách đáng tin cậy để ghép đúng từng instance giữa các lần render. Nếu số lượng hoặc thứ tự thay đổi, làm sao React biết component nào cần tạo lại từ đầu với state mới và component nào là instance hiện có cần giữ hoặc cập nhật state?

Đây là lý do tại sao chúng ta phải cung cấp khóa riêng của mình khi tự động hiển thị danh sách, để khi danh sách được cập nhật vì bất kỳ lý do gì (từ máy chủ hoặc tương tác người dùng), React khớp các khóa của từng mục danh sách trước đó với danh sách được cập nhật. Nếu có bất kỳ thay đổi nào, React sẽ chỉ cập nhật các mục đã thay đổi.

Miễn là `keys` vẫn nhất quán và duy nhất, React có thể xử lý Dom một cách hiệu quả và hiệu quả.

### Sử dụng chìa khóa

Các khóa được truyền vào thành phần hoặc phần tử Dom dưới dạng prop. Bạn nên làm quen với cú pháp.

```jsx
<Component key={keyValue} />
<div key={keyValue}></div>
```

Một điểm khác biệt quan trọng cần lưu ý là prop `key` là riêng tư, chỉ được sử dụng cho các công cụ React nội bộ. Nó không được chuyển đến chính thành phần thông qua đối tượng tham số `props`.

Bây giờ chúng ta đã biết cú pháp, câu hỏi tiếp theo là: cái gì nên được sử dụng làm khóa? Lý tưởng nhất là nên có một số mã định danh duy nhất cho từng mục trong danh sách. Hầu hết các cơ sở dữ liệu chỉ định một id duy nhất cho mỗi mục nhập, vì vậy bạn không cần phải lo lắng về việc tự chỉ định một id. Nếu bạn đang tự xác định dữ liệu, bạn nên chỉ định một `id` duy nhất cho mỗi mục. Ví dụ: bạn có thể sử dụng hàm [crypto.randomUUID ()](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) để tạo một id duy nhất. Hãy xem một ví dụ:

```jsx
// a list of todos, each todo object has a task and an id
const todos = [
  { task: "mow the yard", id: crypto.randomUUID() },
  { task: "Work on Odin Projects", id: crypto.randomUUID() },
  { task: "feed the cat", id: crypto.randomUUID() },
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo) => (
        // here we are using the already generated id as the key.
        <li key={todo.id}>{todo.task}</li>
      ))}
    </ul>
  );
}
```

Ngoài ra, nếu bạn chắc chắn rằng danh sách sẽ không thay đổi trong suốt vòng đời của ứng dụng,  bạn có thể sử dụng chỉ mục mảng làm khóa. Tuy nhiên, điều này không được khuyến khích vì nó có thể dẫn đến lỗi khó hiểu nếu danh sách thay đổi khi các mục bị xóa, chèn hoặc sắp xếp lại. Bạn sẽ tìm hiểu thêm về điều này trong bài viết được liên kết của phần bài tập.

```jsx
const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

function MonthList() {
  return (
    <ul>
      {/* here we are using the index as key */}
      {months.map((month, index) => (<li key={index}>{month}</li>))}
    </ul>
  );
}
```

Chìa khóa rất dễ sử dụng, mặc dù có một mẫu chống lại bạn nên biết. Không bao giờ được tạo khóa khi đang di chuyển. Sử dụng `key={Math.random()}` hoặc `key={crypto.randomUUID()}` *trong khi* kết xuất danh sách đánh bại mục đích của khóa, vì bây giờ một `key` mới sẽ được tạo cho mọi kết xuất của danh sách. Như được thể hiện trong ví dụ trên, `key` nên được suy ra từ chính dữ liệu.

```jsx
const todos = [
  { task: "mow the yard", id: crypto.randomUUID() },
  { task: "Work on Odin Projects", id: crypto.randomUUID() },
  { task: "feed the cat", id: crypto.randomUUID() },
];

function TodoList() {
  return (
    <ul>
      {todos.map((todo) => (
        // DON'T do the following i.e. generating keys during render
        <li key={crypto.randomUUID()}>{todo.task}</li>
      ))}
    </ul>
  );
}
```

### Khóa và trạng thái

Tự động hiển thị danh sách chắc chắn là tình huống phổ biến nhất mà bạn cần cung cấp khóa theo cách thủ công, nhưng chắc chắn đó không phải là lần duy nhất. Vì các khóa chỉ là ID nội bộ để React phân biệt giữa các phiên bản thành phần, chúng tôi cũng có thể cung cấp các khóa riêng của mình nếu chúng tôi cố tình muốn kiểm soát khi nào một thành phần cụ thể phải là cùng một phiên bản với các cập nhật trạng thái của nó hoặc một phiên bản hoàn toàn mới với trạng thái mới.

Hãy tưởng tượng bạn có một trò chơi và khi nó kết thúc, bạn muốn có thể đặt lại nó về trạng thái ban đầu. Làm mới trang có thể không hợp lý, đặc biệt nếu có các thành phần khác có trạng thái riêng mà bạn muốn giữ nguyên. Bạn có thể viết một hàm sẽ đặt từng trạng thái có liên quan thành giá trị ban đầu của nó, nhưng sau đó bạn phải đảm bảo rằng bạn không bỏ lỡ bất kỳ trạng thái nào hoặc đặt sai giá trị và bạn cần cập nhật thông tin này nếu bạn thêm hoặc xóa bất kỳ trạng thái nào.

Điều gì sẽ xảy ra nếu chúng ta có một cách đơn giản hơn để nói với React "render thành phần này từ đầu với trạng thái mới"? Vâng, chúng ta chỉ có thể cập nhật khóa. Ví dụ:

```jsx
function GamePage() {
  const [key, setKey] = useState(0);

  return <Game key={key} resetGame={() => setKey(key + 1)} />;
}
```

`Game` có state riêng và render các component riêng. Khi render lại, nó vẫn là cùng một instance của `Game` vì key chưa thay đổi, nên state được giữ hoặc cập nhật. Nếu có nút bên trong `Game` gọi `resetGame()` khi được nhấn, việc nhấn nút sẽ làm state `key` thay đổi. Sau đó `GamePage` render lại; vì `Game` có key mới, React xem đó là một instance hoàn toàn mới của `Game` và tạo lại nó với state mới.

### Nhiệm vụ

<div class="lesson-content__panel" markdown="1">

1. Read this [section on keys in the React docs](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key).
1. Watch this short video demonstrating [index as key being an anti-pattern](https://youtu.be/xlPxnc5uUPQ).

</div>
