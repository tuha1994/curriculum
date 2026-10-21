### Giới thiệu

Đọc và hiểu thông báo lỗi là một yêu cầu đối với mọi nhà phát triển. Thoạt nhìn, nhiều người mới bắt đầu thường né tránh thông báo lỗi vì chúng có vẻ “đáng sợ” và khó hiểu, do chứa những thuật ngữ mà họ có thể chưa quen.

Tuy nhiên, thông báo lỗi cung cấp cho nhà phát triển cả một kho tàng kiến thức và cho bạn biết mọi điều cần thiết để xử lý chúng! Có thể bình tĩnh phân tích thông báo lỗi và cảnh báo sẽ giúp bạn gỡ lỗi ứng dụng hiệu quả, nhận được sự trợ giúp có ích từ người khác và tự tin tiến lên khi gặp lỗi.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Kể tên ít nhất ba loại lỗi JavaScript.
- Xác định hai phần của một thông báo lỗi giúp bạn tìm ra nơi lỗi bắt nguồn.
- Hiểu cách nghiên cứu và xử lý lỗi.

### Cấu tạo của một lỗi

Lỗi là một kiểu đối tượng được tích hợp trong ngôn ngữ JS, gồm tên/loại lỗi và thông báo. Lỗi chứa những thông tin quan trọng giúp bạn xác định đoạn mã gây lỗi, tìm hiểu lý do xảy ra lỗi và xử lý lỗi.

Giả sử chúng ta đã viết đoạn mã sau trong tệp `script.js` được liên kết với một trang HTML:

```javascript
const a = "Hello";
const b = "World";

console.log(c);
```

Đoạn mã này sẽ chạy, nhưng nó sẽ tạo ra một lỗi. Theo thuật ngữ kỹ thuật, việc này được gọi là "throwing" một lỗi. Phần đầu tiên của lỗi hiển thị loại lỗi. Đây là manh mối đầu tiên cho biết bạn đang đối mặt với vấn đề gì. Chúng ta sẽ tìm hiểu thêm về các loại lỗi khác nhau ở phần sau của bài. Trong ví dụ này, chúng ta có một `ReferenceError`.

![Ví dụ về Reference Error](https://cdn.statically.io/gh/TheOdinProject/curriculum/175b5ef2a1b4758a7b75f4ef43d7e27203e5707b/foundations/javascript_basics/understanding_errors/imgs/00.png)

Một `ReferenceError` được ném ra khi ta tham chiếu đến một biến chưa được khai báo và/hoặc khởi tạo trong phạm vi hiện tại. Trong trường hợp của chúng ta, thông báo lỗi giải thích rằng lỗi xảy ra vì `c is not defined`.

Các lỗi khác nhau thuộc loại này có những thông báo khác nhau, tùy vào nguyên nhân gây ra `ReferenceError`. Ví dụ, một thông báo khác bạn có thể gặp là `ReferenceError: can't access lexical declaration 'X' before initialization`.

Như có thể thấy, thông báo này chỉ ra một lý do hoàn toàn khác so với `ReferenceError` ban đầu ở trên. Hiểu cả loại lỗi lẫn thông báo lỗi là điều cốt yếu để nắm được vì sao bạn nhận được lỗi.

Phần tiếp theo của lỗi cho biết tên tệp nơi bạn có thể tìm thấy lỗi (trong trường hợp này là `script.js`), cũng như số dòng.

Điều này cho phép bạn dễ dàng đi đến dòng có vấn đề trong mã. Ở đây, lỗi bắt nguồn từ dòng thứ tư của `script.js`, được hiển thị dưới thông báo lỗi dưới dạng liên kết với nội dung `at script.js:4`. Nếu nhấp vào liên kết này, hầu hết trình duyệt sẽ chuyển đến chính xác dòng mã đó và phần còn lại của script trong thẻ Sources của Developer Tools.

Đôi khi console của trình duyệt cũng hiển thị cột (hoặc ký tự) trong dòng nơi lỗi xảy ra. Trong ví dụ của chúng ta, đó sẽ là `at script.js:4:13`.

Một phần quan trọng khác của lỗi là **stack trace**. Nó giúp bạn hiểu lỗi được ném ra lúc nào trong ứng dụng và những hàm nào đã được gọi dẫn đến lỗi. Ví dụ, giả sử chúng ta có đoạn mã sau:

```javascript
const a = 5;
const b = 10;

function add() {
  return c;
}

function print() {
  add();
}

print();
```

Hàm `print()` của chúng ta sẽ gọi `add()`, hàm này trả về một biến có tên `c`, nhưng biến đó hiện chưa được khai báo. Lỗi tương ứng như sau:

![Stack Trace của Reference Error](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/foundations/javascript_basics/understanding_errors/imgs/01.png)

Stack trace cho chúng ta biết rằng:

1. `c is not defined` trong phạm vi của `add()`, hàm được khai báo ở dòng 5.
1. `add()` được gọi bởi `print()`, hàm được khai báo ở dòng 9.
1. Bản thân `print()` được gọi ở dòng 12.

Nhờ đó, stack trace cho phép bạn lần theo diễn biến của lỗi ngược về nguồn gốc; trong trường hợp này là phần khai báo `add()`.

### Các loại lỗi thường gặp

Đây là một số lỗi phổ biến nhất bạn sẽ gặp, vì vậy việc hiểu chúng rất quan trọng.

#### Lỗi cú pháp

Lỗi cú pháp xảy ra khi mã bạn định chạy được viết không đúng, tức là không tuân theo các quy tắc ngữ pháp của JavaScript. Ví dụ:

```javascript
function helloWorld() {
  console.log "Hello World!"
}
```

sẽ ném ra lỗi sau vì chúng ta quên dấu ngoặc đơn cho `console.log()`!

![Ví dụ về Syntax Error](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/foundations/javascript_basics/understanding_errors/imgs/02.png)

#### Lỗi tham chiếu

Chúng ta đã đề cập đến lỗi tham chiếu trong ví dụ đầu tiên của bài này, nhưng điều quan trọng cần nhớ là lỗi này xảy ra vì biến bạn đang cố tham chiếu không tồn tại (trong phạm vi hiện tại) - hoặc bạn đã viết sai chính tả biến!

#### Lỗi kiểu

Những lỗi này được ném ra vì một vài lý do khác nhau:

Theo MDN, một `TypeError` có thể được ném ra khi:

> - toán hạng hoặc đối số truyền vào một hàm không tương thích với kiểu mà toán tử hoặc hàm đó yêu cầu;
> - hoặc khi cố sửa đổi một giá trị không thể thay đổi;
> - hoặc khi cố sử dụng một giá trị theo cách không phù hợp.

Giả sử chúng ta có hai chuỗi muốn kết hợp để tạo thành một thông báo, như dưới đây:

```javascript
const str1 = "Hello";
const str2 = "World!";
const message = str1.push(str2);
```

![Ví dụ về Type Error](https://cdn.statically.io/gh/TheOdinProject/curriculum/4ed59981b4ce2c60b5b83bf7415d3127b61821f5/foundations/javascript_basics/understanding_errors/imgs/03.png)

Ở đây, chúng ta nhận được một `TypeError` với thông báo `str1.push is not a function`. Đây là một thông báo lỗi thường khiến học viên bối rối vì bạn có thể biết `.push()` chắc chắn là một hàm (chẳng hạn nếu trước đây bạn đã dùng nó để thêm phần tử vào *mảng*).

Nhưng đó chính là điểm mấu chốt - `.push()` không phải là phương thức của String mà là phương thức của Array. Vì vậy, nó “không phải là một hàm” mà bạn có thể tìm thấy trong các phương thức của String. Nếu đổi `.push()` thành `.concat()`, một phương thức hợp lệ của String, mã sẽ chạy như mong đợi!

Một điều hữu ích cần nhớ khi gặp `TypeError` là xem xét kiểu dữ liệu mà bạn đang cố thực hiện phương thức hoặc phép toán trên đó. Có thể bạn sẽ phát hiện nó không phải kiểu bạn nghĩ, hoặc phép toán hay phương thức đó không tương thích với kiểu dữ liệu ấy.

### Mẹo xử lý lỗi

Đến đây, có lẽ bạn đang tự hỏi làm thế nào để xử lý những lỗi này.

1. Trước hết, hãy hiểu rằng thông báo lỗi là bạn của mình - không phải kẻ thù. Thông báo lỗi cho bạn biết *chính xác* mã đang sai ở đâu và cần kiểm tra những dòng nào để tìm ra nguồn gốc lỗi. Nếu không có thông báo lỗi, việc gỡ lỗi mã sẽ là một *cơn ác mộng* - vì mã vẫn không hoạt động, nhưng chúng ta sẽ không biết tại sao!
1. Bây giờ là lúc Google lỗi! Rất có thể bạn sẽ tìm thấy cách sửa hoặc lời giải thích trên StackOverflow hay trong tài liệu. Nếu không có gì khác, ít nhất bạn sẽ hiểu rõ hơn vì sao mình gặp lỗi này.
1. Hãy dùng debugger! Như đã đề cập trước đó, debugger rất hữu ích cho việc xử lý các vấn đề phức tạp hơn và là một công cụ thiết yếu đối với nhà phát triển. Bạn có thể đặt breakpoint, xem giá trị của bất kỳ biến nào tại bất kỳ thời điểm nào trong quá trình chạy ứng dụng, thực thi từng dòng mã và nhiều hơn nữa! Đây là một công cụ cực kỳ giá trị mà mọi lập trình viên nên biết cách sử dụng.
1. Hãy tận dụng console! `console.log()` là lựa chọn phổ biến để gỡ lỗi nhanh. Với việc xử lý vấn đề phức tạp hơn, dùng debugger có thể phù hợp hơn, nhưng `console.log()` rất hữu ích để nhận phản hồi ngay lập tức mà không cần thực thi từng bước qua các hàm. Ngoài ra còn có những phương thức hữu ích khác như `console.table()`, `console.trace()` và nhiều phương thức khác!

### Lỗi và cảnh báo khác nhau thế nào

Cuối cùng, nhiều người gặp cảnh báo rồi xem chúng như lỗi. Lỗi sẽ dừng việc thực thi chương trình hoặc bất kỳ quy trình nào bạn đang cố chạy, đồng thời ngăn các hành động tiếp theo. Ngược lại, cảnh báo là những thông báo cho bạn biết về các vấn đề tiềm ẩn, nhưng các vấn đề đó không nhất thiết làm chương trình bị lỗi lúc chạy, hoặc thậm chí không làm chương trình dừng.

Dù nên xử lý các cảnh báo nếu có thể và càng sớm càng tốt, cảnh báo không nghiêm trọng như lỗi và thường mang tính thông tin nhiều hơn. Cảnh báo thường hiển thị màu vàng, còn lỗi thường hiển thị màu đỏ. Dù màu sắc này không phải quy tắc bắt buộc, hai loại thường được phân biệt trực quan bằng cách nào đó, bất kể nền tảng bạn đang sử dụng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đã đến lúc đọc tài liệu! Hãy tìm hiểu thêm về [ReferenceError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError), [SyntaxError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) và [TypeError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) trong tài liệu MDN. Đừng lo nếu hiện giờ bạn chưa hiểu hoàn toàn mọi thứ trong tài liệu; mục tiêu là làm quen với các khái niệm. Các ví dụ sử dụng câu lệnh "try...catch", trong đó mã bên trong khối "try" được thực thi. Nếu xảy ra lỗi, lỗi đó sẽ tự động được khối "catch" bắt lại. Điều này cho phép bạn xử lý lỗi trước khi chúng kết thúc script, để bạn có thể xử lý chúng phù hợp trong khối "catch". Hiện tại, chỉ cần nhớ rằng câu lệnh "try...catch" tồn tại và chúng sẽ trở nên hữu ích khi bạn tiến bộ trong chương trình học.

1. Thực hiện bài ["Có gì sai? Khắc phục sự cố JavaScript"](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_went_wrong). Hãy nhớ tải mã khởi đầu có chứa các lỗi cố ý.

</div>