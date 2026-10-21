### Giới thiệu

Giờ bạn đã hiểu cú pháp cơ bản của HTML và CSS, chúng ta sẽ đi vào phần quan trọng hơn. Những kỹ năng quan trọng nhất cần thành thạo trong CSS là *định vị* và *bố cục*. Thay đổi phông chữ và màu sắc là kỹ năng thiết yếu, nhưng đặt mọi thứ chính xác vào vị trí mong muốn trên trang web còn thiết yếu hơn. Suy cho cùng, có bao nhiêu trang web mà tuyệt đối mọi phần tử đều chỉ xếp chồng lên nhau?

Khi đã hiểu một vài khái niệm then chốt, việc định vị các phần tử trên trang web không quá khó. Đáng tiếc là nhiều người học vội vàng đi qua HTML và CSS để đến JavaScript, rồi bỏ lỡ những khái niệm nền tảng này. Điều đó dẫn đến sự bực bội và đau đầu ([cùng những ảnh GIF hài hước](https://giphy.com/gifs/css-13FrpeVH09Zrb2)), vì mọi kỹ năng JavaScript trên đời đều trở nên vô nghĩa nếu bạn không thể đặt phần tử vào đúng vị trí cần thiết trên trang. Vì vậy, hãy bắt đầu.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Bạn sẽ tìm hiểu mọi điều về *mô hình hộp*.
- Bạn sẽ học cách đảm bảo các phần tử có kích thước vừa vặn bằng `margin`, `padding` và `borders`.

### Mô hình hộp

Khái niệm quan trọng đầu tiên cần hiểu để thành công với CSS là mô hình hộp. Nó không phức tạp, nhưng bỏ qua nó bây giờ sẽ khiến bạn vô cùng bối rối về sau.

Mọi thứ trên một trang web đều là một hộp hình chữ nhật. Những hộp này có thể chứa các hộp khác và nằm cạnh nhau. Bạn có thể hình dung sơ bộ cách hoạt động này bằng cách áp dụng đường viền ngoài cho mọi phần tử trên trang như sau:

```css
* {
  outline: 2px solid red;
}
```

![các hộp](https://cdn.statically.io/gh/TheOdinProject/curriculum/c547923a86efaccb0fc71adf70fda2ea340b4cb1/foundations/html_css/css_foundations/the_box_model/imgs/boxes.png)

Nếu muốn, bạn có thể dùng trình kiểm tra của trình duyệt để thêm CSS ở trên vào trang web này bằng cách nhấp vào nút `+` ở góc trên bên phải của bảng "Styles" trong thẻ "Elements". Hộp trong hộp!

![các đường](https://cdn.statically.io/gh/TheOdinProject/curriculum/c547923a86efaccb0fc71adf70fda2ea340b4cb1/foundations/html_css/css_foundations/the_box_model/imgs/odin-lined.png)

Được rồi, có thể hình trên có một vài hình tròn... nhưng khi nói về bố cục, chúng kết hợp với nhau giống những hộp chữ nhật chứ không phải hình tròn. Sau cùng, bố trí một trang web và định vị mọi phần tử trên đó chính là quyết định cách bạn lồng và xếp chồng các hộp này.

Điểm phức tạp duy nhất là có nhiều cách thao tác kích thước của những hộp này và khoảng cách giữa chúng bằng `padding`, `border` và `margin`. Các bài đọc được giao sẽ trình bày sâu hơn về khái niệm này, nhưng tóm tắt ngắn gọn:

- `padding` làm tăng khoảng cách giữa đường viền của hộp và nội dung trong hộp.
- `border` tạo thêm khoảng cách (dù chỉ một hay hai pixel) giữa margin và padding.
- `margin` làm tăng khoảng cách giữa đường viền của một hộp và đường viền của các hộp liền kề.

Hãy nhớ nghiên cứu kỹ các sơ đồ.

![mô hình hộp](https://cdn.statically.io/gh/TheOdinProject/curriculum/c547923a86efaccb0fc71adf70fda2ea340b4cb1/foundations/html_css/css_foundations/the_box_model/imgs/box-model.png)

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [Learn CSS Box Model In 8 Minutes](https://www.youtube.com/watch?v=rIO5326FgPE) là phần tổng quan dễ hiểu về mô hình hộp, padding và margin. Hãy xem ngay; nội dung này là nền tảng cho mọi phần còn lại.
1. [box-sizing: border-box (EASY!)](https://www.youtube.com/watch?v=HdZHcFWcAd8) bổ sung cho tài liệu trên bằng cách giải thích rõ hơn về 'box-sizing'.
1. Vì khái niệm mô hình hộp có tính nền tảng đặc biệt quan trọng, hãy tìm hiểu sâu hơn một chút với [bài học về mô hình hộp của MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model). Bài này trình bày cùng nội dung với các video ở trên và giới thiệu các hộp inline mà chúng ta sẽ khám phá trong bài tiếp theo. Bài viết cũng đề cập đến kiểu hiển thị bên trong và bên ngoài khi giải thích cách các phần tử được hiển thị.

   Bạn chưa cần hiểu hoàn toàn hay ghi nhớ những chi tiết đó. Hãy chú ý kỹ các ví dụ và dành thời gian thử nghiệm với trình chỉnh sửa ngay trong trình duyệt!
1. [Trang CSS Tricks về margin](https://css-tricks.com/almanac/properties/m/margin/) có thêm một số thông tin hữu ích về thuộc tính `margin`. Cụ thể, các phần nói về `auto` và hiện tượng gộp margin có những điều bạn cần biết.

</div>