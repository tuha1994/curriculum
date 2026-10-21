### Giới thiệu

Trong bài học này, chúng ta sẽ tìm hiểu các phím trong React. Chìa khóa là công cụ đặc biệt cho các thành phần của chúng ta và chúng ta sẽ tìm hiểu lý do tại sao chúng được sử dụng.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Khóa là gì và React sử dụng chúng như thế nào.
- Cách sử dụng khóa tốt và xấu trong ứng dụng React.

### Tại sao React cần chìa khóa?

Bạn có thể nhớ lại rằng React sử dụng "DOM ảo" để quyết định những gì trong DOM thực cần cập nhật, giảm thiểu các hành động không cần thiết. Khi kết xuất lại xảy ra, trước tiên, nó sẽ tạo lại DOM ảo này, khác biệt (so sánh các thay đổi) DOM ảo mới và trước đó, sau đó chỉ thực hiện cập nhật DOM thực cho những thứ thực sự đã thay đổi.

React cần có khả năng phân biệt từng thành phần này, vì mỗi thành phần có props và state riêng. Do đó, mỗi thành phần được cấp một mã định danh cơ bản — một **khóa**. Ví dụ: nếu bạn cập nhật state, đó vẫn là cùng một phiên bản của thành phần và React biết điều này vì khóa của thành phần không thay đổi, nhờ đó có thể tránh thao tác không cần thiết. Tuy nhiên, nếu khóa thay đổi, React biết rằng đây là một phiên bản hoàn toàn mới của thành phần và có thể tạo phiên bản mới với state mới.

Thông thường, bạn không cần phải cung cấp khóa thành phần theo cách thủ công, vì React sẽ tự động xử lý việc này. Tuy nhiên, có một số trường hợp chúng ta có thể sử dụng chúng.

### Các phím khi hiển thị danh sách

Trong bài học trước, chúng ta đã sử dụng phương thức `.map()` để lặp qua một mảng dữ liệu và trả về một thành phần, hiển thị danh sách các thành phần. Bây giờ hãy tưởng tượng, nếu bất kỳ mục nào trong danh sách thay đổi, làm sao React biết mục nào cần cập nhật ở hậu trường?

Nếu danh sách thay đổi, chúng ta có thể muốn một trong hai điều xảy ra:

1. Kết xuất lại hoàn toàn toàn bộ danh sách
1. Tìm kiếm các mục cụ thể đã được thay đổi và chỉ hiển thị lại những mục đó

Chúng tôi muốn React chỉ tìm kiếm item(s) đã thay đổi và KHÔNG hiển thị lại toàn bộ danh sách (để tránh những công việc không cần thiết). Vì vậy, mỗi mục trong danh sách đó đều cần có một khóa.

Nếu chúng ta mã hóa cứng các thành phần trong JSX, thì chúng ta có thể để React xử lý các khóa một cách tự động, vì các hướng dẫn rõ ràng và tĩnh; qua các lần kết xuất lại, không có sự mơ hồ giữa thành phần này và thành phần khác.

Tuy nhiên, nếu chúng ta kết xuất một danh sách thông qua ánh xạ trên một mảng thì mọi thứ sẽ động. Chúng tôi sẽ chỉ viết một giá trị trả về duy nhất cho phương thức `.map()` nhưng kết quả cuối cùng có thể là 2, 3 hoặc thậm chí 100 phiên bản của thành phần trả về đó và cả số lượng cũng như thứ tự của các giá trị này đều có thể thay đổi giữa các lần hiển thị. React sẽ không thể tự động cung cấp khóa cho từng thành phần này theo cách cho phép nó khớp một cách đáng tin cậy với các phiên bản cụ thể trong các lần kết xuất lại. Điều gì sẽ xảy ra nếu thứ tự đếm and/or của các mục thay đổi? Làm thế nào nó biết chắc chắn thành phần nào cần được làm lại từ đầu với trạng thái mới và thành phần nào hiện có trạng thái keep/update?

Đây là lý do tại sao chúng tôi phải cung cấp khóa riêng khi hiển thị động danh sách để khi danh sách được cập nhật vì bất kỳ lý do gì (từ máy chủ hoặc tương tác của người dùng), React sẽ khớp khóa của từng mục danh sách trước đó với danh sách đã cập nhật. Nếu có bất kỳ thay đổi nào, React sẽ chỉ cập nhật những mục đã thay đổi.

Miễn là `keys` vẫn nhất quán và duy nhất, React có thể xử lý DOM một cách hiệu quả và năng suất.

### Sử dụng phím

Các khóa được truyền vào thành phần hoặc phần tử DOM dưới dạng chỗ dựa. Bạn chắc hẳn đã quen thuộc với cú pháp.

```jsx
<Component key={keyValue} />
<div key={keyValue}></div>
```

Một điểm khác biệt quan trọng cần lưu ý là `key` prop là riêng tư, chỉ được sử dụng cho nội dung React nội bộ. Nó không được truyền đến chính thành phần đó thông qua đối tượng param `props`.

Bây giờ chúng ta đã biết cú pháp, câu hỏi tiếp theo là: nên dùng cái gì làm khóa? Lý tưởng nhất là nên có một số mã định danh duy nhất cho từng mục trong danh sách. Hầu hết các cơ sở dữ liệu đều chỉ định một id duy nhất cho mỗi mục nhập, vì vậy bạn không cần phải lo lắng về việc tự mình chỉ định id. Nếu bạn đang tự xác định dữ liệu, cách tốt nhất là gán `id` duy nhất cho mỗi mục. Ví dụ: bạn có thể sử dụng [Hàm crypto.randomUUID()](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) để tạo một id duy nhất. Hãy xem một ví dụ:

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

Ngoài ra, nếu bạn chắc chắn danh sách sẽ không thay đổi trong suốt vòng đời của ứng dụng, bạn có thể sử dụng chỉ mục mảng làm khóa. Tuy nhiên, điều này không được khuyến khích vì nó có thể dẫn đến lỗi khó hiểu nếu danh sách thay đổi khi các mục bị xóa, chèn hoặc sắp xếp lại. Bạn sẽ tìm hiểu thêm về điều này trong bài viết được liên kết của phần bài tập.

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

Các phím rất dễ sử dụng, mặc dù có một kiểu phản đối mà bạn nên lưu ý. Không bao giờ nên tạo khóa một cách nhanh chóng. Sử dụng `key={Math.random()}` hoặc `key={crypto.randomUUID()}`*trong khi* việc hiển thị danh sách sẽ làm mất đi mục đích của khóa, vì bây giờ `key` mới sẽ được tạo cho mỗi lần hiển thị danh sách. Như được hiển thị trong ví dụ trên, `key` phải được suy ra từ chính dữ liệu đó.

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

### Phím và trạng thái

Danh sách hiển thị động chắc chắn là trường hợp phổ biến nhất mà bạn cần cung cấp khóa theo cách thủ công nhưng chắc chắn đây không phải là trường hợp duy nhất. Vì khóa chỉ là ID nội bộ để React phân biệt giữa các phiên bản thành phần nên chúng tôi cũng có thể cung cấp khóa riêng nếu chúng tôi cố tình muốn kiểm soát thời điểm một thành phần cụ thể sẽ giống với phiên bản cập nhật trạng thái của nó hoặc một phiên bản hoàn toàn mới với trạng thái mới.

Hãy tưởng tượng bạn có một trò chơi và khi nó kết thúc, bạn muốn có thể đặt lại nó về trạng thái ban đầu. Làm mới trang có lẽ là không hợp lý, đặc biệt nếu có các thành phần khác có trạng thái riêng mà bạn muốn giữ nguyên. Bạn có thể viết một hàm sẽ đặt từng trạng thái liên quan về giá trị ban đầu của nó, nhưng sau đó bạn phải đảm bảo rằng bạn không bỏ sót bất kỳ trạng thái nào hoặc đặt sai giá trị và bạn cần phải cập nhật thông tin này nếu bạn thêm hoặc xóa bất kỳ trạng thái nào.

Điều gì sẽ xảy ra nếu chúng ta có một cách đơn giản hơn nhiều để nói React "kết xuất thành phần này từ đầu với trạng thái mới"? Chà, chúng ta chỉ có thể cập nhật chìa khóa. Ví dụ:

```jsx
function GamePage() {
  const [key, setKey] = useState(0);

  return <Game key={key} resetGame={() => setKey(key + 1)} />;
}
```

`Game` sẽ có trạng thái riêng và hiển thị các thành phần riêng. Khi hiển thị lại, nó vẫn là phiên bản tương tự của `Game` vì khóa của nó không thay đổi, vì vậy trạng thái của nó là preserved/updated. Nếu chúng ta có một nút ở đâu đó bên trong `Game` gọi `resetGame()` khi được nhấp vào, việc nhấp vào nút đó sẽ khiến trạng thái `key` thay đổi. `GamePage` sau đó hiển thị lại và vì `Game` hiện có khóa mới nên React coi đó là một phiên bản hoàn toàn mới của `Game` và sẽ tạo một khóa mới với các trạng thái mới.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc cái này [phần về các phím trong tài liệu React](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key).
1. Xem video ngắn này chứng minh [chỉ mục làm khóa là một mẫu chống](https://youtu.be/xlPxnc5uUPQ).

</div>
