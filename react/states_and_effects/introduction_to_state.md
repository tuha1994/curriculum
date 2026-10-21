### Giới thiệu

Các ứng dụng thú vị thường thay đổi trong quá trình người dùng tương tác. Thay đổi có thể đơn giản như mở một menu thả xuống, hoặc phức tạp như lấy dữ liệu từ API. React cung cấp các nguyên thủy để quản lý state của ứng dụng, cụ thể là state của component, nhờ đó giao diện có thể phản hồi linh hoạt. Trong bài này, chúng ta sẽ học cách dùng state trong React.

Trước khi bắt đầu, hãy đọc [State là gì? của Academind](https://academind.com/tutorials/what-is-state).

### Tổng quan bài học

Phần này tóm lược những nội dung bạn sẽ học trong bài.

- Trạng thái của React là gì.
- Cách sử dụng trạng thái trong React.
- Điều gì xảy ra trong React khi trạng thái được thay đổi.

### Trạng thái trong React là gì?

Trong React, ta viết rất nhiều component và thường muốn chúng thay đổi giao diện theo tương tác của người dùng hoặc theo dữ liệu. Vì vậy, component cần “ghi nhớ” thông tin về chính nó. Đó là lúc state phát huy tác dụng. **State là bộ nhớ của một component.**

Chúng ta hãy xem cách chúng ta có thể sử dụng trạng thái trong mã của mình. Đi đến của chúng tôi [repo ví dụ phản ứng](https://github.com/TheOdinProject/react-examples) sau đó rẽ nhánh và sao chép nó. Khi bạn đã sao chép nó, `cd` vào thư mục `state/`, nơi bạn có thể chạy `npm install` để thiết lập mọi thứ, sau đó `npm run dev` để khởi động máy chủ nhà phát triển.

Sau khi mở, bạn sẽ thấy chúng tôi có một ứng dụng thay đổi màu nền dựa trên nút được nhấp. Hãy dành thời gian đọc mã khi chúng ta bắt đầu ngay và xem mã trong `src/App.jsx`.

#### Cái móc useState

Hook `useState` là một hook tích hợp (chúng ta sẽ nói về hook sau) trong React cho phép bạn xác định trạng thái trong một thành phần chức năng. Nó lấy một giá trị ban đầu làm tham số và trả về một mảng có hai phần tử mà chúng ta có thể cấu trúc lại để nhận được:

1. Giá trị trạng thái hiện tại
1. Hàm cập nhật giá trị trạng thái

Định nghĩa trạng thái với `useState` thường tuân theo mẫu này:

```javascript
const [stateValue, setStateValue] = useState(initialValue);

// adapted for our use case:
const [backgroundColor, setBackgroundColor] = useState(COLORS[0]);
```

Ngay cả khi chưa biết nhiều về React, bạn vẫn có thể hình dung chuyện gì đang xảy ra. Hook xác định state `backgroundColor`. Mỗi nút được gắn một *trình xử lý sự kiện nhấp* để gọi `setBackgroundColor` với màu tương ứng. Sau đó, màu mới được áp dụng cho nền.

Việc thêm nhiều biến trạng thái cũng dễ dàng như việc thêm nhiều lệnh gọi `useState` hơn.

Tuyệt vời, bây giờ bạn có thể sử dụng trạng thái! Nhưng, những gì đang xảy ra dưới mui xe?

### Trạng thái hoạt động như thế nào trong React?

Hãy đánh bạn với một số lý thuyết.

Trong React, khi state hoặc props của component thay đổi, React sẽ chạy lại hàm component từ đầu để xác định nội dung cần render dựa trên state và props mới. Sau đó, React áp dụng các thay đổi đã tính toán vào DOM (commit). Nói cách khác, component được tạo lại, nhưng lần này `useState` trả về giá trị state mới nhất. Quá trình này gọi là render lại; đó là cơ chế giúp React cập nhật giao diện hiệu quả khi dữ liệu thay đổi.

<div class="lesson-note" markdown="1">

#### React thuật toán đối chiếu

Quá trình kết xuất lại tạo ra một cây ảo DOM (Mô hình đối tượng tài liệu) mới. DOM ảo là một đại diện nhẹ của DOM thực tế mà React sử dụng để theo dõi trạng thái hiện tại của UI. React sau đó so sánh cây DOM ảo mới với cây trước đó và tính toán tập hợp các thay đổi tối thiểu cần thiết để cập nhật DOM thực tế. Đây là thuật toán đối chiếu.

</div>

#### Giải thích việc hiển thị lại bằng ví dụ trên

Mỗi khi gọi `setBackgroundColor`, component `App` sẽ render lại. Về cơ bản, component được chạy lại, nên hàm `onButtonClick` cùng các `div` và `button` cũng được tạo lại. Vậy state `backgroundColor` có bị tạo lại không? Không. React theo dõi giá trị state mới nhất và cung cấp nó cho component. Giá trị khởi tạo chỉ được dùng ở lần render đầu tiên, sau đó bị bỏ qua.

![mã minh họa của kết xuất lại](https://cdn.statically.io/gh/TheOdinProject/curriculum/abf5962ab36c951bab907a9952d09400651eab10/react/states_and_effects/introduction_to_state/imgs/00.png)

Những gì chúng tôi đề cập trong phần này chỉ là tổng quan ngắn gọn về một phần nội bộ React. Bạn có thể tò mò muốn tìm hiểu thêm, nhưng hãy lưu ý, đây là một cái hố sâu. Những gì chúng tôi đề cập ở trên đủ để giúp bạn vượt qua khóa học React.

### móc

Móc là các hàm cho phép bạn sử dụng các tính năng React. Tất cả các hook đều có thể được nhận dạng bằng tiền tố `use`. Ví dụ: `useState` là một cái móc. Chúng ta sẽ sử dụng nhiều hơn những thứ này khi chúng ta tiến sâu hơn vào khóa học. Bây giờ, hãy nhớ rằng hook có những quy tắc mà chúng ta cần tuân theo:

1. Hook chỉ có thể được gọi từ cấp cao nhất của một thành phần chức năng.
1. Hook không thể được gọi từ vòng lặp hoặc điều kiện bên trong.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc các bài viết sau từ tài liệu React:
    1. [Trạng thái: Bộ nhớ của một thành phần](https://react.dev/learn/state-a-components-memory)
    1. [Kết xuất và cam kết](https://react.dev/learn/render-and-commit)
1. Đọc cái này [bài viết về React Thuật toán đối chiếu](https://www.geeksforgeeks.org/reactjs-reconciliation/) cho một lời giải thích tuyệt vời.
1. Quay lại ứng dụng ví dụ trạng thái từ kho lưu trữ `react-examples` của chúng tôi ở phần trước trong bài học. Thêm một biến trạng thái mới để theo dõi số lần màu nền được thay đổi. Hiển thị số lần màu nền được thay đổi trên trang.

</div>
