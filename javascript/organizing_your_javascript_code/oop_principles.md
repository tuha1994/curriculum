### Giới thiệu

Đến với thời điểm này, bạn sẽ học và có cơ hội thực hiện các tổ chức mẫu và tạo ra phổ biến đối tượng nhất trong JavaScript. Nhưng đó chỉ là *tip* của băng trôi. Điều quan trọng hơn là việc học các cú pháp cho các chức năng hoặc mô-đun của nhà để tìm ra cách sử dụng chúng một cách hiệu quả.

Toàn bộ bộ bài học này là về mô hình "Đối tượng hướng dẫn tập lệnh" (OOP). Các kiến ​​thức cơ bản về tạo đối tượng và lớp tương đối đơn giản. Nhưng thật không dễ dàng để quyết định nên đặt cái gì vào mỗi đối tượng, hay khi nào tạo một đối tượng mới, hoặc khi nào để một đối tượng 'kế thừa' từ một đối tượng khác.

May mắn thay, có một số khái niệm và nguyên tắc có thể hướng dẫn chúng tôi đưa ra quyết định đúng đắn khi liên quan đến đối tượng của mình. Bài học này là phần giới thiệu về những khái niệm quan trọng nhất. Hãy nhớ rằng thông thường không có câu trả lời rõ ràng nào cho các câu hỏi ứng dụng thiết kế của bạn. Một số mẫu và ý tưởng rõ ràng là tốt hơn những mẫu khác, nhưng thường có một số thay đổi khi quyết định vị trí đặt một công cụ chức năng. Nói cách khác... những nguyên tắc này không phải là *quy tắc*—chúng là những hướng dẫn hữu ích.

Khi bạn đọc những tài nguyên này, có thể hữu ích nếu bạn quay lại một số dự án bạn đã thực hiện và suy nghĩ xem những gì bạn đã viết phù hợp với những ví dụ bạn thấy như thế nào. Và tất nhiên, khi bạn tiếp tục, hãy ghi lại những điều này khi tạo dự án mới.

### Tổng quan bài học

Phần này chứa thông tin tổng hợp về các chủ đề mà bạn sẽ học trong bài học này.

- Giải thích "Nguyên tắc trách nhiệm duy nhất".
- Giải thích rút gọn các nguyên tắc bổ sung SOLID.
- Giải thích các đồ vật "có thể liên kết chặt chẽ" là gì và tại sao chúng tôi muốn tránh.
- Hiểu biết về sao bố cục thường được ưu tiên hơn là kế thừa.

### Trách nhiệm duy nhất

Khi bạn tạo các đối tượng của mình, một trong những điều quan trọng nhất cần nhớ là **Nguyên tắc Trách nhiệm duy nhất** nêu rõ rằng một lớp (hoặc đối tượng hoặc mô-đun... bạn hiểu) chỉ nên có *một* trách nhiệm. Điều này không có nghĩa là một đối tượng chỉ có thể làm một việc, nhưng mọi thứ đối tượng làm đều phải thuộc về cùng một trách nhiệm.

Đây là một ví dụ thực sự phổ biến. Hầu hết mã của chúng tôi đều có các hàm cập nhật và ghi mọi thứ vào DOM bên cạnh logic ứng dụng. Đó thực sự là một ý tưởng *rất hay* khi tách phần DOM khỏi logic ứng dụng.

Ở đây, chúng tôi có một chức năng sẽ kiểm tra xem trò chơi quá điều kiện có được đáp ứng hay không.  Có hai vấn đề với điều này:

```javascript
function isGameOver() {

  // game over logic goes here!

  if (gameOver) {
    const gameOverDiv = document.createElement('div');
    gameOverDiv.classList.add('game-over');
    gameOverDiv.textContent = `${this.winner} won the game!`;
    document.body.appendChild(gameOverDiv);
  }
}
```

Vấn đề đầu tiên là hàm (và mô-đun chứa nó) không thể thao tác trực tiếp với DOM. Bạn nên trích xuất tất cả các thao tác DOM vào mô-đun riêng của nó và sử dụng nó như sau:

```javascript
function isGameOver() {

  // game over logic goes here!

  if (gameOver){
    DOMStuff.gameOver(this.winner);
  }
}
```

Vấn đề thứ hai còn lại là hàm `isGameOver` chỉ chịu trách nhiệm kiểm tra xem điều kiện `gameOver` có đáp ứng hay không. Dựa trên giá trị trả về của `isGameOver`, hàm xử lý vòng lặp trò chơi sẽ đảm bảo trách nhiệm quyết định nên gọi `DOMStuff.gameOver(this.winner)` hay không.

Một cách khác để suy nghĩ về Nguyên tắc nhiệm duy nhất là một phương thức/lớp/thành phần nhất định phải có một lý do duy nhất để thay đổi. Ngược lại, nếu một đối tượng đang cố gắng đảm nhận nhiều trách nhiệm thì việc thay đổi một cạnh có thể ảnh hưởng đến một cạnh khác.

Nguyên tắc đảm nhiệm duy nhất là nguyên tắc đầu tiên trong bộ 5 nguyên tắc thiết kế thường được gọi là nguyên tắc **SOLID**. Bạn sẽ đọc thêm về những nguyên tắc này trong các bài viết dưới đây.

### Các đối tượng được ghép mảnh viền

Rõ ràng, tất cả các đối tượng của chúng ta đều hướng tới mục tiêu làm việc cùng nhau để tạo ra ứng dụng cuối cùng của chúng ta. Tuy nhiên, bạn nên cẩn thận để đảm bảo rằng các mảnh đồ riêng của bạn có thể độc lập nhiều nhất. Các đối tượng **Tđược kết hợp chặt chẽ** là các đối tượng phụ thuộc rất nhiều vào nhau đến khả năng loại bỏ hoặc thay đổi một đối tượng có nghĩa là bạn phải thay đổi hoàn toàn một đối tượng khác - một điều thực sự đáng sợ.

Điều này có liên quan khá chặt chẽ đến 'Trách nhiệm duy nhất' nhưng ở một góc độ khác. Ví dụ: nếu chúng tôi đang viết một trò chơi mong muốn và thay đổi hoàn toàn cách thức hoạt động của người dùng Giao diện, chúng tôi có thể thực hiện điều đó mà không cần phải thực hiện lại trò chơi hoàn toàn logic. Vì vậy, chúng tôi có thể bắt đầu viết trò chơi của mình bằng cách sử dụng chủ yếu `console.log()` và sau đó thêm vào một loạt chức năng `DOM` mà không cần phải chạm vào trò chơi logic.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bài viết và video dưới đây đề cập đến từ viết tắt **SOLID** trước khi chuyển sang nói về trách nhiệm duy nhất. nhiệm vụ duy nhất chắc chắn là phù hợp nhất trong số 5 trách nhiệm, nhưng bạn có thể thoải mái tìm hiểu phần còn lại của nguyên tắc SOLID nếu muốn.
    1. Đọc bài viết này về [nguyên tắc SOLID số 1: bội nhiệm duy nhất (JavaScript)](https://duncan-mcardle.medium.com/solid-principle-1-single-responsibility-javascript-5d9ce2c6f4a5). Nó đơn giản hóa nội dung được đề cập chi tiết hơn trong các video SOLID bên dưới.
    1. Xem [Nguyên tắc thiết kế SOLID của WDS](https://www.youtube.com/playlist?list=PLZlA0Gpn_vH9kocFX7R7BAe_CvvOCO_p9) để xem ví dụ về mã hóa cho từng nguyên tắc.
1. [Cách viết JavaScript có khả năng mở rộng và bảo trì cao: Khớp nối](https://web.archive.org/web/20170215102316/http://www.innoarchitech.com:80/scalable-maintainable-javascript-coupling) giải thích rõ ràng hơn về các đối tượng được ghép nối cân bằng.
1. FunFunction có một video tuyệt vời về [ưu tiên sáng hơn kế thừa](https://www.youtube.com/watch?v=wfMtDGfHWpA).

</div>
