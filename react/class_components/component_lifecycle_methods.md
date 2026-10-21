### Giới thiệu

Trong phần trước, bạn đã tìm hiểu về vòng đời của một thành phần. Trong các thành phần chức năng, chúng tôi chủ yếu sử dụng hook `useEffect` để thực hiện các tác vụ trong suốt vòng đời, nhưng trong các thành phần lớp, chúng tôi phải sử dụng các phương thức vòng đời cụ thể để thay thế.

Chúng ta sẽ bắt đầu với phần tóm tắt ngắn gọn về vòng đời của một thành phần. Có ba giai đoạn trong vòng đời của một thành phần: gắn kết, cập nhật và ngắt kết nối. Mỗi trong số chúng có một phương thức được gán cho chúng trong các thành phần lớp, đó là những gì chúng ta sẽ đề cập trong bài học này.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Tìm hiểu cách sử dụng các phương thức vòng đời trong một thành phần lớp.

### render()

Hàm kết xuất là phương thức vòng đời được sử dụng nhiều nhất và là phương thức mà bạn đã gặp trong bài học về thành phần lớp trước. Đây là phương thức vòng đời bắt buộc duy nhất trong một thành phần lớp. Nó chạy trên mount và cập nhật một thành phần. Kết xuất phải thuần túy, nghĩa là nó không sửa đổi trạng thái thành phần, trả về cùng một thứ mỗi khi nó được gọi (với cùng một đầu vào) và không tương tác trực tiếp với trình duyệt.

### componentDidMount()

Phương thức này được chạy sau khi thành phần được gắn kết (được chèn vào cây DOM). Bạn nên thực hiện bất kỳ lệnh gọi nào để tìm nạp dữ liệu cần thiết cho thành phần tại đây. Đây cũng là nơi tốt để thực hiện bất kỳ thao tác nào phụ thuộc vào thành phần được gắn trong DOM.

### componentDidUpdate()

Phương pháp này được chạy sau khi một thành phần được hiển thị lại. Vì điều này, bạn phải cẩn thận về những gì bạn cập nhật trong phương pháp này, vì nếu bạn cập nhật trạng thái một cách bừa bãi, việc hiển thị lại sẽ xảy ra và bạn sẽ rơi vào một vòng lặp vô tận. Bạn có thể tránh vấn đề này bằng cách sử dụng các câu lệnh có điều kiện về sự bằng nhau của các prop trước đó và hiện tại khi cập nhật trạng thái.

Trong phương pháp này, bạn nên cập nhật mọi thứ cần thay đổi để phản hồi lại việc thay đổi DOM hoặc bất kỳ trạng thái nào mà bạn có thể muốn thực hiện khi thay đổi. Ví dụ: bạn muốn tìm nạp lại dữ liệu người dùng nếu người dùng thay đổi.

### componentWillUnmount()

Đây là phương thức vòng đời cuối cùng, được gọi trước khi một thành phần được ngắt kết nối và hủy. Trong phương pháp này, bạn nên thực hiện các hành động dọn dẹp, như hủy các yêu cầu mạng, xóa bộ hẹn giờ, v.v.

### Cách useEffect() kết hợp các phương pháp vòng đời

Bây giờ chúng ta đã tìm hiểu về các phương thức vòng đời của lớp, thật hữu ích khi hiểu rằng hook `useEffect` được sử dụng trong các thành phần chức năng về cơ bản là sự kết hợp của `componentDidMount`, `componentDidUpdate` và `componentWillUnmount`. Những phương thức này thực hiện công việc tùy thuộc vào mảng phụ thuộc và việc chúng có trả về giá trị nào hay không.

- Một mảng phụ thuộc trống sẽ tương đương với `componentDidMount`.
- Mảng phụ thuộc có các giá trị sẽ là sự kết hợp của `componentDidMount` và `componentDidUpdate`, nhưng chỉ cập nhật khi các phần phụ thuộc thay đổi.
- Không có mảng phụ thuộc nào tương đương với `componentDidMount` và `componentDidUpdate` kết hợp.
- Hàm trả về bên trong hook `useEffect()` sẽ tương đương với `componentWillUnmount`.

Ví dụ:

```javascript
  useEffect(() => {
    placeholderFunction();
    return () => cleanupFunction();
  }, [])
```

Trong đoạn mã này, useEffect chứa chức năng của `componentDidMount` và `componentWillUnmount` thông qua hàm return. Ví dụ này không có chức năng `componentDidUpdate` do mảng phụ thuộc trống.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Kiểm tra cái này [sơ đồ vòng đời thành phần](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) để xem cách trình bày trực quan tốt về các phương pháp vòng đời của một thành phần.

1. Đọc qua [tài liệu thành phần](https://react.dev/reference/react/Component) từ 'constructor(props)' cho đến 'componentWillUnmount()' trong phần Tham khảo, lưu ý đến APIs không được dùng nữa. Hãy coi APIs khác làm thông tin bổ sung.

</div>
