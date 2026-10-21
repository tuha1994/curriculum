### Giới thiệu

HTML (HyperText Markup Language - Ngôn ngữ đánh dấu siêu văn bản) xác định cấu trúc và nội dung của các trang web. Chúng ta sử dụng các phần tử HTML để tạo ra mọi đoạn văn, tiêu đề, danh sách, hình ảnh và liên kết cấu thành một trang web thông thường. Trong bài học này, chúng ta sẽ tìm hiểu cách các phần tử HTML hoạt động.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Giải thích thẻ HTML là gì.
- Giải thích phần tử HTML là gì.

### Phần tử và thẻ

Hầu hết các phần tử trên một trang HTML chỉ là những mẩu nội dung được bao quanh bởi thẻ HTML mở và đóng.

Thẻ HTML là những khối xây dựng thiết yếu, dùng để xác định cấu trúc và nội dung của một trang web.

Thẻ mở cho trình duyệt biết đây là phần bắt đầu của một phần tử HTML. Thẻ mở gồm một từ khóa được đặt trong dấu ngoặc nhọn `<>`. Ví dụ, thẻ mở của một đoạn văn có dạng: `<p>`.

Thẻ đóng cho trình duyệt biết một phần tử kết thúc ở đâu. Thẻ đóng gần giống thẻ mở; điểm khác biệt duy nhất là nó có dấu gạch chéo trước từ khóa. Ví dụ, thẻ đóng của một đoạn văn có dạng: `</p>`.

Một phần tử đoạn văn hoàn chỉnh có dạng:

```html
<p>some text content</p>
```

Hãy phân tích ví dụ này:

- `<p>` là thẻ mở.
- `some text content` đại diện cho nội dung được bao quanh bởi thẻ mở và thẻ đóng.
- `</p>` là thẻ đóng.

Bạn có thể hình dung các phần tử như những chiếc hộp chứa nội dung. Thẻ mở và thẻ đóng cho trình duyệt biết phần tử chứa nội dung nào. Sau đó, trình duyệt có thể dùng thông tin đó để xác định cách diễn giải và định dạng nội dung.

HTML có một [danh sách rất lớn các thẻ được định nghĩa sẵn](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) mà bạn có thể dùng để tạo đủ loại phần tử khác nhau. Việc sử dụng đúng thẻ cho nội dung là rất quan trọng. Dùng đúng thẻ có thể ảnh hưởng lớn đến hai khía cạnh của trang web: thứ hạng của trang trên các công cụ tìm kiếm và khả năng tiếp cận đối với những người dùng dựa vào công nghệ hỗ trợ, chẳng hạn như trình đọc màn hình, để sử dụng internet.

Việc sử dụng đúng phần tử cho nội dung được gọi là HTML ngữ nghĩa. Chúng ta sẽ tìm hiểu chủ đề này sâu hơn nhiều ở phần sau của chương trình học.

### Phần tử rỗng

Một số phần tử HTML không có thẻ đóng. Những phần tử này chỉ có một thẻ duy nhất, chẳng hạn `<br>` hoặc `<img>`. Chúng được gọi là phần tử rỗng vì chúng không chứa nội dung nào bên trong. Không có thẻ đóng nghĩa là chúng không thể bao quanh nội dung như các thẻ khác.

Bạn cũng có thể thấy chúng được gọi là thẻ tự đóng. Tuy nhiên, đó chỉ là các phần tử rỗng có thêm dấu gạch chéo(/) ở cuối, chẳng hạn `<br />` hoặc `<img />`. Vì lý do lịch sử, bạn có thể sẽ thường xuyên bắt gặp thẻ tự đóng. Trình duyệt vẫn hiển thị chúng bình thường, nhưng phiên bản mới nhất của đặc tả HTML không khuyến khích sử dụng và xem chúng là không hợp lệ.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem video [HTML & CSS cho người mới bắt đầu: HTML là gì?](https://www.youtube.com/watch?v=X4sClFRMJ00) của Kevin Powell.

</div>