### Giới thiệu

Vì JavaScript là ngôn ngữ của web, có những hàm nhất thiết sẽ mất khá nhiều thời gian để hoàn tất, chẳng hạn như lấy dữ liệu từ máy chủ để hiển thị trên trang của bạn. Vì lý do này, JavaScript hỗ trợ các hàm bất đồng bộ, hay nói cách khác là những hàm có thể chạy ở nền trong khi phần còn lại của mã tiếp tục thực thi.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Giải thích callback là gì.
- Giải thích promise là gì.
- Giải thích những trường hợp promise tốt hơn callback.
- Giải thích hàm `.then()` làm gì.

### Callback

Trước đây không lâu, cách phổ biến nhất để xử lý những việc này là dùng **callback**, và hiện nay chúng vẫn được sử dụng khá nhiều trong một số trường hợp.

Một hàm callback là một hàm được truyền vào một hàm khác dưới dạng đối số, sau đó được gọi bên trong hàm bên ngoài để hoàn thành một quy trình hoặc hành động nào đó ([tài liệu MDN về các hàm callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)).

Callback là những hàm được truyền vào các hàm khác. Ví dụ:

```javascript
myDiv.addEventListener("click", function(){
  // do something!
})
```

Ở đây, hàm `addEventListener()` nhận một callback (hàm “do something”) rồi gọi nó khi `myDiv` được nhấp.

Có lẽ bạn sẽ nhận ra mẫu này xuất hiện *rất thường xuyên* trong mã JavaScript. Tuy nhiên, dù hữu ích trong những tình huống như ví dụ trên, callback có thể trở nên khó kiểm soát, đặc biệt khi bạn cần nối nhiều callback theo một thứ tự cụ thể. Phần còn lại của bài học này thảo luận các mẫu và hàm giúp bạn tránh [callback hell](https://www.geeksforgeeks.org/javascript/what-to-understand-callback-and-callback-hell-in-javascript/).

### Promise

Có nhiều cách để xử lý mã bất đồng bộ trong JavaScript, và mỗi cách đều có trường hợp sử dụng riêng. Promise là một cơ chế như vậy, đồng thời là cơ chế bạn sẽ khá thường xuyên gặp khi sử dụng các thư viện hoặc framework khác. Biết promise là gì và cách sử dụng nó rất hữu ích.

Về cơ bản, promise là một đối tượng có thể tạo ra một giá trị vào một thời điểm nào đó trong tương lai. Đây là một ví dụ:

Giả sử `getData()` là một hàm lấy một số dữ liệu từ máy chủ và trả về dữ liệu đó dưới dạng một đối tượng mà chúng ta có thể sử dụng trong mã:

```javascript
const getData = function() {
  // go fetch data from some API...
  // clean it up a bit and return it as an object:
  return data
}
```

Vấn đề với ví dụ này là việc lấy dữ liệu mất một khoảng thời gian, nhưng nếu không thông báo cho mã biết điều đó, mã sẽ giả định rằng mọi thứ trong hàm diễn ra gần như ngay lập tức. Vì vậy, nếu thử làm thế này:

```javascript
const myData = getData()
const pieceOfData = myData['whatever']
```

Chúng ta sẽ gặp rắc rối, vì khi cố trích xuất `pieceOfData` từ dữ liệu được trả về, hàm `getData()` rất có thể vẫn đang lấy dữ liệu. Do đó, `myData` sẽ không phải dữ liệu mong đợi mà sẽ là `undefined`. Buồn thật.

Chúng ta cần một cách để giải quyết vấn đề này và bảo mã chờ cho đến khi dữ liệu được lấy xong rồi mới tiếp tục. Promise giải quyết vấn đề đó. Chúng ta sẽ để bạn học cú pháp cụ thể trong các bài viết sắp đọc, nhưng về cơ bản Promise cho phép bạn làm như sau:

```javascript
const myData = getData() // if this is refactored to return a Promise...

myData.then(function(data){ // .then() tells it to wait until the promise is resolved
  const pieceOfData = data['whatever'] // and THEN run the function inside
})
```

Tất nhiên, còn nhiều trường hợp khác mà bạn sẽ muốn dùng Promise ngoài việc lấy dữ liệu, nên học những điều này ngay bây giờ sẽ rất hữu ích.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [phần về callback trong bài viết Art of Node](https://github.com/maxogden/art-of-node#callbacks), giải thích cách callback xử lý các thao tác bất đồng bộ.
1. Đọc [bài viết về Promise](https://davidwalsh.name/promises). Đây là điểm bắt đầu tốt và bài viết ngắn gọn, đi thẳng vào vấn đề.
1. Xem [video về promise](https://youtu.be/DHvZLI7Db8E). Đây là một cách hay để hình dung cách người ta có thể thực sự sử dụng promise trong thực tế.
1. Xem [video có tiêu đề “What is Event Loop?”](https://www.youtube.com/watch?v=8aGhZQkoFbQ) để hiểu cách mã bất đồng bộ hoạt động trong JavaScript.
1. Xem [video minh họa Event loop](https://www.youtube.com/watch?v=eiC58R16hb8) để hiểu thêm về Event Loop.
1. Xem [video minh họa promise](https://www.youtube.com/watch?v=Xs1EMmBLpn4) để hiểu quá trình thực thi Promise trong JavaScript.
1. Đọc [bài viết cơ bản về promise](https://javascript.info/promise-basics), một tài nguyên tuyệt vời cho người mới bắt đầu.

</div>