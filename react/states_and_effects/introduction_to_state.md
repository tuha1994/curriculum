### Giới thiệu

Bất kỳ ứng dụng thú vị nào bạn xây dựng đều có khả năng thay đổi theo thời gian người dùng khám phá nó. Những thay đổi có thể cơ bản như chuyển đổi menu thả xuống hoặc phức tạp như tìm nạp dữ liệu từ API. React cung cấp các nguyên hàm để thao tác trạng thái ứng dụng của bạn, cụ thể hơn là các thành phần, để làm cho chúng trở nên linh hoạt. Trong bài học này, chúng ta sẽ học cách sử dụng trạng thái trong React.

Đọc [Nhà nước là gì? bởi Học viện](https://academind.com/tutorials/what-is-state) trước khi chúng ta bắt đầu.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Trạng thái của React là gì.
- Cách sử dụng trạng thái trong React.
- Điều gì xảy ra trong React khi trạng thái được thay đổi.

### Trạng thái trong React là gì?

Chúng tôi viết các thành phần bằng React, RẤT NHIỀU thành phần và nhiều khi chúng tôi muốn chúng trải qua những thay đổi trực quan do tương tác của người dùng hoặc máy tính. Với mục đích này, một thành phần cần phải "ghi nhớ" mọi thứ về chính nó. Đây là nơi nhà nước xuất hiện.**Trạng thái là bộ nhớ của một thành phần.**

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

Ngay cả khi không có nhiều kiến ​​thức về React, ở một mức độ nào đó, bạn vẫn có thể hiểu được chuyện gì đang xảy ra. Trạng thái `backgroundColor` được xác định bằng hook. Sau đó trên mỗi nút, chúng tôi thiết lập một*bấm vào*trình xử lý sự kiện gọi hàm `setBackgroundColor` với giá trị tương ứng. Sau đó, màu mới được áp dụng cho nền một cách kỳ diệu.

Việc thêm nhiều biến trạng thái cũng dễ dàng như việc thêm nhiều lệnh gọi `useState` hơn.

Tuyệt vời, bây giờ bạn có thể sử dụng trạng thái! Nhưng, những gì đang xảy ra dưới mui xe?

### Trạng thái hoạt động như thế nào trong React?

Hãy đánh bạn với một số lý thuyết.

Trong React, khi trạng thái hoặc đạo cụ của một thành phần thay đổi, React sẽ chạy lại chức năng thành phần của bạn ngay từ đầu để tìm ra nội dung nào sẽ được hiển thị dựa trên trạng thái và đạo cụ mới được thiết lập. Sau đó, tất cả các thay đổi được tính toán sẽ được áp dụng cho DOM (đã cam kết).
Nghĩa là, theo một nghĩa nào đó, toàn bộ thành phần được tạo lại, nhưng lần này giá trị trạng thái mới nhất sẽ được trả về từ `useState`. Quá trình này được gọi là kết xuất lại. Hiển thị lại là tính năng chính của React cho phép nó cập nhật giao diện người dùng một cách hiệu quả để đáp ứng với những thay đổi trong dữ liệu cơ bản.

<div class="lesson-note" markdown="1">

#### React thuật toán đối chiếu

Quá trình kết xuất lại tạo ra một cây ảo DOM (Mô hình đối tượng tài liệu) mới. DOM ảo là một đại diện nhẹ của DOM thực tế mà React sử dụng để theo dõi trạng thái hiện tại của UI. React sau đó so sánh cây DOM ảo mới với cây trước đó và tính toán tập hợp các thay đổi tối thiểu cần thiết để cập nhật DOM thực tế. Đây là thuật toán đối chiếu.

</div>

#### Giải thích việc hiển thị lại bằng ví dụ trên

Bất cứ khi nào `setBackgroundColor` được gọi, thành phần `App` của chúng tôi sẽ được hiển thị lại. Về cơ bản, thành phần này được tạo lại, có nghĩa là hàm `onButtonClick` và các `div` và `button` của chúng tôi cũng được tạo lại. Bạn có thể thắc mắc, có nên tạo lại trạng thái `backgroundColor` không? Chà, React chịu trách nhiệm theo dõi trạng thái mới nhất và cung cấp nó cho thành phần. Giá trị trạng thái ban đầu chỉ được sử dụng cho lần hiển thị đầu tiên của thành phần và bị bỏ qua trong các lần hiển thị tiếp theo.

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
