### Giới thiệu

Trong các phần trước, bạn đã học cách cấu trúc trang web bằng HTML và tạo kiểu cho chúng bằng CSS. Bước tiếp theo là làm cho trang web *có tính tương tác*, và đó chính là việc JavaScript đảm nhiệm.

Trong phần này, chúng ta sẽ tập trung vào những nền tảng của JavaScript và cách bạn có thể dùng nó để xử lý mọi tương tác khác nhau giữa trang web và người dùng.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Chạy mã JavaScript bằng tệp HTML.
- Khai báo biến bằng `let` và `const`.
- Thực hiện các phép toán với số.
- Thực hiện các phép toán với chuỗi.
- Sử dụng các toán tử logic và toán học.

### Cách chạy mã JavaScript

Phần lớn mã JavaScript chúng ta viết trong khóa Foundations sẽ được chạy thông qua trình duyệt. Các bài học sau trong Foundations và lộ trình NodeJS sẽ chỉ cho bạn cách chạy JavaScript bên ngoài môi trường trình duyệt. Điều này có nghĩa là chúng ta sẽ để trình duyệt thực thi mã, ngay cả khi mã đó đến từ một tệp do chúng ta tạo.

Ngoài những bài học này, hiện tại bạn nên luôn mặc định chạy JavaScript trong trình duyệt trừ khi có chỉ dẫn khác; nếu không, bạn có thể gặp những lỗi không mong muốn.

Cách đơn giản nhất để bắt đầu là tạo một tệp HTML có mã JavaScript bên trong. Dùng snippet VS Code <kbd>!</kbd> + <kbd>TAB</kbd> để tạo bộ khung HTML cơ bản trong một tệp ở đâu đó trên máy tính. Hãy nhớ bao gồm thẻ `<script>`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

  <script>
    // Your JavaScript goes here!
    console.log("Hello, World!")
  </script>

</body>
</html>
```

Lưu và mở tệp này trong trình duyệt web, sau đó mở console của trình duyệt:

1. Nhấp chuột phải vào trang web trống.
1. Nhấp vào "Inspect" hoặc "Inspect Element" để mở Developer Tools.
1. Tìm và chọn thẻ "Console", nơi bạn sẽ thấy kết quả của câu lệnh `console.log`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Xem trước trực tiếp

Bạn có thể dùng [tiện ích Live Preview trong Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server) để tự động cập nhật trình duyệt khi lưu tệp, thay vì phải tự làm mới trang mỗi khi chỉnh sửa mã. Hãy thử sửa văn bản thành một nội dung khác!

</div>

`console.log()` là câu lệnh dùng để in thứ gì đó ra console dành cho nhà phát triển trong trình duyệt. Bạn có thể dùng nó để in kết quả từ bất kỳ bài viết và bài tập nào sau đây vào console. Chúng tôi khuyến khích bạn viết mã theo tất cả ví dụ trong bài này và các bài học sau.

Một cách khác để đưa JavaScript vào trang web là thông qua một script bên ngoài. Cách này rất giống với việc liên kết các tài liệu CSS bên ngoài vào trang web.

```html
<script src="javascript.js"></script>
```

Các tệp JavaScript có phần mở rộng `.js`, tương tự như `.css` đối với biểu định kiểu. Tệp JavaScript bên ngoài được dùng cho các script phức tạp hơn.

Chúng ta đặt tên tệp là `javascript.js`, nhưng có thể chọn bất kỳ tên nào như `my-script.js` hoặc `i-love-the-odin-project.js`.

### Biến

Đây là những khối xây dựng của mọi chương trình. Bạn có thể hình dung biến như những "vật chứa lưu trữ" dữ liệu trong mã của mình.

![Minh họa hộp biến](https://cdn.statically.io/gh/TheOdinProject/curriculum/d39eaf2ca95e80705f703bb218216c10508f5047/foundations/javascript_basics/fundamentals-1/imgs/00.png)

Bạn có thể khai báo biến bằng từ khóa `let`. Hãy thử nhé! (Không phải chơi chữ đâu).

```javascript
let firstName = "John";
let lastName = "Doe";

console.log(firstName);
console.log(lastName);
```

`console.log` sẽ xuất ra gì? Hãy thử xem!

Bạn cũng có thể gán lại biến:

```javascript
let age = 11;
console.log(age); // outputs 11 to the console

age = 54;

console.log(age); // what will be output now?
```

Hãy chú ý dòng 4 không có `let` - chúng ta không cần nó vì biến đã được *khai báo* trước đó và hiện tại chỉ đang được gán lại!

Gán lại biến thì khá thú vị, nhưng nếu chúng ta *không muốn* việc đó xảy ra thì sao? Ví dụ, ta có thể có một `pi` *hằng số* không bao giờ cần được gán lại. Chúng ta có thể làm điều này bằng từ khóa `const`.

```javascript
const pi = 3.14;
pi = 10;

console.log(pi); // What will be output?
```

Theo trực giác, bạn có thể nghĩ rằng `3.14` sẽ được xuất ra. Hãy thử xem!

Một lỗi sẽ được ném ra. Mã thậm chí không chạy đến `console.log`! Bạn có thể tự hỏi tại sao chúng ta lại *muốn* có lỗi trong mã. Sự thật là lỗi cực kỳ hữu ích trong việc cho chúng ta biết mã sai ở đâu và vấn đề chính xác nằm ở đâu. Nếu không có lỗi, mã vẫn không làm được điều ta muốn, nhưng việc tìm ra sai ở đâu sẽ vô cùng khó khăn!

Tóm lại, có hai cách để khai báo một biến:

- `let`, có thể được gán lại.
- `const`, **không thể** được gán lại và sẽ ném ra lỗi nếu chúng ta cố làm vậy.

Ngoài ra còn có cách thứ ba là `var`, cách khai báo biến ban đầu của JavaScript. `var` tương tự `let` ở chỗ các biến được gán theo cách này có thể được gán lại, nhưng nó có những đặc điểm khác đã được giải quyết khi ngôn ngữ đưa vào `let` và `const`. Nhìn chung, hiện nay var không còn được dùng nữa. Tuy nhiên, có thể bạn sẽ gặp mã sử dụng `var` vào một lúc nào đó, vì vậy biết rằng nó tồn tại vẫn hữu ích.

### Số

Số là những khối xây dựng của logic lập trình! Thực tế, thật khó nghĩ ra một tác vụ lập trình hữu ích nào không liên quan ít nhất đến một chút toán cơ bản, nên rõ ràng biết cách số hoạt động là điều rất quan trọng. May mắn là trong JavaScript, cách số hoạt động khá quen thuộc.

Chẳng hạn, JavaScript tuân theo **thứ tự thực hiện phép toán** tiêu chuẩn trong toán học (thường được ghi nhớ bằng các từ viết tắt như **PEMDAS** hoặc **BODMAS**). Điều này có nghĩa là:

1. **Ngoặc đơn** (hoặc ngoặc vuông) được tính trước.
1. **Lũy thừa** được tính tiếp theo, từ phải sang trái.
1. **Phép nhân** và **phép chia** được tính tiếp theo, từ trái sang phải.
1. **Phép cộng** và **phép trừ** được tính cuối cùng, từ trái sang phải.

Ví dụ, biểu thức toán học `(3 + 2) - 76 * (1 + 1)` hoàn toàn hợp lệ trong JavaScript và sẽ được tính đúng như bạn mong đợi nếu đặt biểu thức đó vào một `console.log`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Hãy thử các bài tập sau bằng cách thêm mã vào thẻ script trong tệp HTML:

1. Cộng 2 số với nhau! Trong script, nhập `console.log(23 + 97)`. Khi chạy, câu lệnh này phải in ra `120`.
1. Làm tương tự nhưng cộng 6 số khác nhau với nhau.
1. Bây giờ hãy in giá trị của biểu thức sau: `(4 + 6 + 9) / 77`. Console phải in ra xấp xỉ `0.24675`.
1. Hãy dùng một vài biến!
   1. Thêm câu lệnh này vào thẻ script: `let a = 10`.
   1. Bên dưới, thêm `console.log(a)`. Khi chạy, console của trình duyệt phải in ra `10`.
   1. Sau đó, gán lại cho `a` một giá trị số khác. In `a` lần nữa; nó phải hiển thị giá trị đã cập nhật (lần in trước vẫn phải hiển thị giá trị cũ là `10` vì nó xảy ra trước khi `a` được gán lại).
   1. Bây giờ thêm `let b = 7 * a` vào cuối script.
   1. In giá trị của `b`. Nó phải in ra kết quả của 7 nhân với giá trị bạn đã gán lại cho `a`.
1. Thử chuỗi thao tác này:
   1. Khai báo một biến const `max` với giá trị `57`.
   1. Khai báo một biến const khác là `actual` và gán cho nó `max - 13`.
   1. Khai báo một biến const khác là `percentage` và gán cho nó `actual / max`.
   1. Bây giờ nếu in `percentage`, bạn sẽ thấy một giá trị như `0.7719` trong console.
1. Dành vài phút tiếp tục thử nghiệm nhiều thứ khác nhau trong thẻ script. Cuối cùng, chúng ta sẽ học cách làm cho những thứ này thực sự xuất hiện trên trang web, nhưng toàn bộ logic vẫn sẽ giữ nguyên. Hãy đảm bảo bạn đã thấy thoải mái với chúng trước khi tiếp tục.

Hãy đọc các bài viết sau để củng cố kiến thức.

1. Xem qua ["JavaScript là gì?" của MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) để hiểu vai trò của JavaScript trên một trang web.
1. Đọc về [các biến trong JavaScript](https://javascript.info/variables) trên JavaScript.info.
1. Bài viết MDN về [toán học trong JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Math) trình bày cùng thông tin từ một góc nhìn hơi khác, đồng thời hướng dẫn bạn áp dụng một số phép toán cơ bản trong JavaScript. Bạn có thể làm nhiều việc hơn với số, nhưng hiện tại chỉ cần đến đây.
1. Đọc qua (và viết mã theo!) bài viết về [các toán tử JavaScript](http://javascript.info/operators). Đừng quên làm phần "Tasks" ở cuối trang! Nó sẽ giúp bạn hiểu khá rõ những gì có thể làm với số (cùng nhiều thứ khác!) trong JavaScript.

</div>