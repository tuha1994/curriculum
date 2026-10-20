### Giới thiệu

Đến lúc này, có lẽ bạn đã nắm khá vững những khái niệm nền tảng quan trọng của CSS, nhưng CSS còn có thể làm được nhiều điều hơn nữa để tạo ra những trang web thực sự bắt mắt. Đã đến lúc tìm hiểu những tính năng CSS hữu ích mà bạn có thể dùng để tạo thêm điểm nhấn cho các dự án của mình.

Có *rất nhiều* thuộc tính CSS. May mắn là bạn không cần ghi nhớ tất cả: số thuộc tính bạn thực sự dùng hằng ngày nhỏ hơn nhiều. Bài học này sẽ đề cập đến phần lớn những thuộc tính mà bạn sẽ thường xuyên sử dụng. Cấu trúc của bài hơi khác một chút vì về cơ bản đây chỉ là danh sách các thuộc tính CSS. Chúng tôi sẽ mô tả ngắn gọn thuộc tính rồi liên kết đến tài liệu nơi bạn có thể xem tất cả các tùy chọn có sẵn.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài.

- Bạn sẽ tìm hiểu nhiều thuộc tính CSS hữu ích.

#### Background

Có lẽ bạn đã thử đặt màu nền cho các phần tử, nhưng thuộc tính `background` còn làm được nhiều hơn thế. Thuộc tính `background` thực ra là dạng viết tắt của 8 thuộc tính liên quan đến nền, và bạn có thể đọc về tất cả chúng trong tài liệu được liên kết. Ngoài việc thay đổi màu nền, bạn còn có thể chỉ định ảnh nền, thay đổi vị trí và kích thước ảnh nền, cũng như thay đổi cách ảnh nền lặp lại hoặc lát nền khi ảnh quá nhỏ để lấp đầy phần tử chứa. Bạn cũng có thể sử dụng nhiều lớp nền.

Cần lưu ý rằng bạn *có thể* dùng riêng từng thuộc tính này, và trong một số trường hợp, cách đó có thể dễ hơn và rõ ràng hơn so với việc mặc định dùng dạng viết tắt. Điều này trái ngược với một số thuộc tính viết tắt khác, trong đó gần như lúc nào cũng nên ưu tiên dạng viết tắt (flex, margin, padding, v.v.).

Tài liệu chứa rất nhiều thông tin về dạng viết tắt này và tất cả các thuộc tính liên quan. Như chúng tôi đã đề cập trước đây, bạn KHÔNG cần *ghi nhớ* thứ tự và cú pháp chính xác của từng thuộc tính. Chỉ cần biết chúng tồn tại và nắm được ý nghĩa khái quát của chúng là đủ.

Một lưu ý nữa là phần **Formal Syntax** ở đây *rất* phức tạp. Đừng để điều đó làm bạn nản. Cú pháp cơ bản hơi khó định nghĩa vì nhiều thuộc tính tạo nên dạng viết tắt là tùy chọn, hoặc có thể xuất hiện ở những vị trí khác nhau trong định nghĩa.

#### Borders

Đến lúc này, có lẽ bạn đã gặp `border` và `border-radius`. Thuộc tính `border` cũng là một dạng viết tắt, nhưng *đơn giản hơn nhiều* so với dạng viết tắt của background. Với đường viền, về cơ bản bạn chỉ cần xác định kích thước, kiểu và màu.

`border-radius` là thuộc tính dùng để tạo các góc bo tròn cho phần tử. Như bạn sẽ thấy trong tài liệu, bạn có thể định nghĩa bán kính khác nhau cho từng góc, nhưng điều này hiếm khi hữu ích. Hãy xếp thông tin đó vào nhóm "những thứ sẽ tra cứu nếu sau này cần".

#### Box-shadow

Đúng như tên gọi, `box-shadow` thêm hiệu ứng đổ bóng xung quanh một phần tử. Thuộc tính này hữu ích để tạo cảm giác chiều sâu cho trang và tạo sự phân tách tinh tế giữa các phần tử.

Cách sử dụng khá đơn giản, nhưng hãy nhớ rằng bạn nên dùng nó tiết chế và nhẹ nhàng. Ưu tiên những bóng mờ, khó nhận thấy hơn là các màu tối hoặc quá nổi bật.

#### Overflow

Bạn có thể dùng `overflow` để xác định điều gì xảy ra với một phần tử khi nội dung của nó quá lớn, không thể vừa vào bên trong. Cách dùng phổ biến nhất có lẽ là thêm thanh cuộn cho một phần tử bên trong trang web, chẳng hạn một phần tử kiểu `card` có nội dung cuộn được.

#### Opacity

Opacity cũng là một thuộc tính đơn giản nhưng có thể rất hữu ích trong một số trường hợp. Thuộc tính `opacity` cho phép bạn làm cho một phần tử trong suốt nhiều hơn hoặc ít hơn. Điều này có thể hữu ích khi tạo hiệu ứng hover hoặc xếp các phần tử chồng lên nhau.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem tài liệu MDN về các thuộc tính sau:
   - [background](https://developer.mozilla.org/en-US/docs/Web/CSS/background)
   - [border](https://developer.mozilla.org/en-US/docs/Web/CSS/border)
   - [border-radius](https://developer.mozilla.org/en-US/docs/Web/CSS/border-radius)
   - [box-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)
   - [overflow](https://developer.mozilla.org/en-US/docs/Web/CSS/overflow)
   - [opacity](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity)

</div>