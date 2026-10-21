### Giới thiệu

Ở bài trước, chúng ta đã học cú pháp CSS và các bộ chọn cơ bản. Bây giờ là lúc kết hợp kiến thức về bộ chọn với chữ *C* trong CSS — cascade (cơ chế xếp tầng).

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Cascade làm gì.
- Độ đặc hiệu và cách kết hợp các bộ chọn CSS.
- Cơ chế kế thừa ảnh hưởng đến một số thuộc tính.

### Cascade của CSS

Đôi khi các quy tắc xung đột với nhau và cho ra kết quả ngoài dự đoán. "Nhưng tôi muốn *những* đoạn văn này màu xanh, sao chúng lại đỏ giống những đoạn kia?!" Dù điều này có thể gây bực bội, điều quan trọng là phải hiểu CSS không tự ý *làm* trái ý chúng ta. CSS chỉ làm những gì ta bảo nó làm. Một ngoại lệ là các kiểu mặc định do trình duyệt cung cấp. Những kiểu mặc định này khác nhau giữa các trình duyệt; chúng giải thích vì sao một số phần tử tạo ra "khoảng trống" lớn với phần tử khác, hoặc vì sao các nút trông như vậy dù ta không viết quy tắc CSS nào để tạo kiểu cho chúng.

Vì vậy, nếu gặp hành vi ngoài dự đoán, nguyên nhân có thể là các kiểu mặc định đó, việc chưa hiểu cách một thuộc tính hoạt động, hoặc chưa hiểu cơ chế nhỏ gọi là cascade.

Cascade quyết định quy tắc nào thực sự được áp dụng vào HTML. Cascade dùng nhiều yếu tố để đưa ra quyết định. Chúng ta sẽ xem xét ba yếu tố trong số đó, hy vọng giúp bạn tránh những khoảnh khắc bực bội kiểu "Tôi ghét CSS".

#### Độ đặc hiệu

Một khai báo CSS đặc hiệu hơn sẽ được ưu tiên hơn khai báo kém đặc hiệu. Kiểu inline, đã học ở bài trước, có độ đặc hiệu cao nhất so với các bộ chọn; mỗi loại bộ chọn cũng có một mức độ đặc hiệu riêng, góp phần quyết định độ đặc hiệu của khai báo. Các bộ chọn khác cũng ảnh hưởng, nhưng hiện tại chúng ta chỉ tập trung vào những loại đã học:

1. Bộ chọn ID (đặc hiệu nhất)
1. Bộ chọn class
1. Bộ chọn kiểu

Độ đặc hiệu chỉ được xét khi một phần tử có nhiều khai báo xung đột cùng nhắm đến nó, gần giống một quy tắc phân định hòa. Bộ chọn ID luôn thắng mọi số lượng bộ chọn class, bộ chọn class luôn thắng mọi số lượng bộ chọn kiểu, và bộ chọn kiểu luôn thắng mọi số lượng bộ chọn kém đặc hiệu hơn. Nếu so sánh độ đặc hiệu chưa xác định được bên thắng, hãy so sánh số bộ chọn trong cùng nhóm (ID, class hoặc kiểu). Quy tắc có nhiều bộ chọn hơn sẽ được ưu tiên.

Hãy xem một vài ví dụ nhanh để hình dung cách độ đặc hiệu hoạt động.
Xét đoạn mã HTML và CSS sau:

```html
<!-- index.html -->

<div class="main">
  <div class="list subsection">Red text</div>
</div>
```

```css
/* rule 1 */
.subsection {
  color: blue;
}

/* rule 2 */
.main .list {
  color: red;
}
```

Trong ví dụ trên, cả hai quy tắc chỉ dùng bộ chọn class, nhưng quy tắc 2 đặc hiệu hơn vì dùng nhiều bộ chọn class hơn; do đó khai báo `color: red` sẽ được ưu tiên.

Bây giờ hãy thay đổi một chút:

```html
<!-- index.html -->

<div class="main">
  <div class="list" id="subsection">Blue text</div>
</div>
```

```css
/* rule 1 */
#subsection {
  color: blue;
}

/* rule 2 */
.main .list {
  color: red;
}
```

Trong ví dụ này, dù quy tắc 2 có nhiều bộ chọn class hơn số bộ chọn ID, quy tắc 1 vẫn đặc hiệu hơn vì ID thắng class. Vì vậy, khai báo `color: blue` sẽ được ưu tiên.

Và một ví dụ phức tạp hơn:

```html
<!-- index.html -->

<div class="main">
  <div class="list" id="subsection">Red text on yellow background</div>
</div>
```

```css
/* rule 1 */
#subsection {
  background-color: yellow;
  color: blue;
}

/* rule 2 */
.main #subsection {
 color: red;
}
```

Trong ví dụ cuối, quy tắc đầu dùng bộ chọn ID, còn quy tắc hai kết hợp bộ chọn ID với bộ chọn class. Vì vậy, không quy tắc nào dùng bộ chọn đặc hiệu hơn quy tắc kia. Cascade tiếp tục kiểm tra số lượng của từng loại bộ chọn. Cả hai quy tắc chỉ có một bộ chọn ID, nhưng quy tắc 2 có thêm một bộ chọn class bên cạnh ID, nên quy tắc 2 có độ đặc hiệu cao hơn!

Khai báo `color: red` sẽ được ưu tiên, còn khai báo `background-color: yellow` vẫn được áp dụng vì không có khai báo nào xung đột với nó.

#### Không phải mọi thứ đều tăng độ đặc hiệu

Khi so sánh các bộ chọn, bạn có thể gặp các ký hiệu đặc biệt của bộ chọn toàn cục (`*`) cũng như các bộ kết hợp (`+`, `~`, `>`, và khoảng trắng). Những ký hiệu bạn chưa gặp sẽ được trình bày ở phần sau của chương trình, nên hiện tại chưa cần đi sâu. Ý chính là bản thân các ký hiệu này không mang thêm độ đặc hiệu.

```css
/* rule 1 */
.class.second-class {
  font-size: 12px;
}

/* rule 2 */
.class .second-class {
  font-size: 24px;
}
```

Ở đây, quy tắc 1 và quy tắc 2 có cùng độ đặc hiệu. Quy tắc 1 dùng bộ chọn nối (không có khoảng trắng), còn quy tắc 2 dùng bộ kết hợp hậu duệ (khoảng trắng). Nhưng cả hai đều có hai class và bản thân ký hiệu bộ kết hợp không làm tăng độ đặc hiệu.

```css
/* rule 1 */
.class.second-class {
  font-size: 12px;
}

/* rule 2 */
.class > .second-class {
  font-size: 24px;
}
```

Ví dụ này cũng cho thấy điều tương tự. Dù quy tắc 2 dùng bộ kết hợp con (`>`), điều đó không làm thay đổi giá trị độ đặc hiệu. Cả hai quy tắc vẫn có hai class nên có cùng độ đặc hiệu.

```css
/* rule 1 */
* {
  color: black;
}

/* rule 2 */
h1 {
  color: orange;
}
```

Trong ví dụ này, quy tắc 2 có độ đặc hiệu cao hơn và giá trị `orange` sẽ được ưu tiên cho phần tử. Quy tắc 2 dùng bộ chọn kiểu, vốn có mức độ đặc hiệu thấp nhất. Nhưng quy tắc 1 dùng bộ chọn toàn cục (`*`), không có giá trị độ đặc hiệu.

#### Kế thừa

Kế thừa là cơ chế trong đó một số thuộc tính CSS khi áp dụng cho một phần tử sẽ được truyền xuống các phần tử hậu duệ, ngay cả khi ta không viết quy tắc riêng cho chúng. Các thuộc tính về typography (`color`, `font-size`, `font-family`, v.v.) thường được kế thừa, còn phần lớn thuộc tính khác thì không. Bạn có thể xem một thuộc tính có được kế thừa hay không trong tài liệu MDN của nó, ở phần **Formal Definition**. Chẳng hạn, [định nghĩa hình thức của thuộc tính CSS `color`](https://developer.mozilla.org/en-US/docs/Web/CSS/color#formal_definition) cho biết `color` được kế thừa, còn [định nghĩa hình thức của thuộc tính `display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display#formal_definition) cho biết `display` thì không.

Ngoại lệ là khi nhắm trực tiếp đến một phần tử, vì cách đó luôn thắng kế thừa:

```html
<!-- index.html -->

<div id="parent">
  <div class="child"></div>
</div>
```

```css
/* styles.css */

#parent {
  color: red;
}

.child {
  color: blue;
}
```

Dù phần tử `parent` có độ đặc hiệu cao hơn nhờ ID, phần tử `child` vẫn được áp dụng kiểu `color: blue` vì khai báo đó nhắm trực tiếp đến nó, còn `color: red` từ phần tử cha chỉ được kế thừa.

#### Thứ tự quy tắc

Đây là yếu tố cuối cùng, chốt hạ sau mọi yếu tố phân định khác. Giả sử sau khi xét tất cả yếu tố khác, vẫn còn nhiều quy tắc xung đột cùng nhắm đến một phần tử. Cascade quyết định áp dụng quy tắc nào bằng cách nào?

Quy tắc được định nghĩa *sau cùng* sẽ thắng.

```css
/* styles.css */

.alert {
  color: red;
}

.warning {
  color: yellow;
}
```

Với một phần tử có cả class `alert` và `warning`, cascade sẽ xét mọi yếu tố khác, bao gồm kế thừa (ở đây không có) và độ đặc hiệu (hai quy tắc có độ đặc hiệu ngang nhau). Vì quy tắc `.warning` được định nghĩa sau cùng và không yếu tố nào khác phân định được quy tắc cần áp dụng, quy tắc đó sẽ được áp dụng cho phần tử.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. [The CSS Cascade](https://2019.wattenberger.com/blog/css-cascade) là một bài đọc tương tác rất hay, trình bày chi tiết hơn một chút về những yếu tố khác ảnh hưởng đến các quy tắc CSS thực sự được áp dụng.
1. Hoàn thành bài tập trong [thư mục `foundations/cascade` của kho bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/foundations/cascade) của chúng ta (nhớ rằng hướng dẫn nằm trong README):
   - `01-cascade-fix`

    Lưu ý: Bạn có thể tìm lời giải cho bài tập này trong thư mục `solution` của bài.

</div>