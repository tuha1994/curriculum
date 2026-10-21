### Giới thiệu

Một khái niệm cơ bản quan trọng trong testing là tính cô lập. Bạn chỉ nên test từng method một, và lý tưởng nhất là các test cho một function không phụ thuộc vào việc một function bên ngoài hoạt động đúng - đặc biệt khi function đó đang được test ở nơi khác (mặc dù như mọi điều lý tưởng khác, điều này không phải lúc nào cũng khả thi hoặc thực tế). Lý do chính là khi test thất bại, bạn muốn có thể thu hẹp nguyên nhân của lỗi này nhanh nhất có thể. Nếu một test phụ thuộc vào nhiều function, có thể rất khó biết chính xác điều gì đang sai.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Giải thích tightly coupled code là gì.
- Mô tả pure function và mối liên hệ của nó với TDD.
- Giải thích mocking là gì.

### Pure functions

Có *rất nhiều* lợi ích khi sử dụng TDD lúc viết code. Một trong những lợi ích lớn nhất ban đầu không dễ nhận thấy - TDD giúp bạn viết code tốt hơn. Nếu nhìn lại một số project đầu tiên, có lẽ bạn sẽ nhận thấy mọi thứ *tightly coupled* đến mức nào. Tất cả function đều chứa tham chiếu đến các function ở những phần khác của code, và toàn bộ chương trình thì *đầy* các method DOM hoặc `console.log()`.

Tightly coupled code rất khó test! Hãy thử tưởng tượng việc viết test cho một function như sau:

```javascript
function guessingGame() {
  const magicNumber = 22;
  const guess = prompt('guess a number between 1 and 100!');
  if (guess > magicNumber) {
    alert('YOUR GUESS IS TOO BIG');
  } else if (guess < magicNumber) {
    alert('YOUR GUESS IS TOO SMALL');
  } else if (guess == magicNumber) {
    alert('YOU DID IT! 🎉');
  } else {
    return 'INVALID INPUT';
  }
}
```

Để test được đoạn này, chúng ta cần tách riêng tất cả những việc khác nhau đang diễn ra. Trước hết, chúng ta không cần test các function `prompt` và `alert` vì chúng được tích hợp sẵn trong browser. Chúng nằm bên ngoài chương trình của chúng ta và người viết chúng đã test chúng rồi. Điều *cần* test là logic về số, việc này sẽ dễ hơn nhiều nếu tháo nó khỏi các function khác:

```javascript
function evaluateGuess(magicNumber, guess) {
  if (guess > magicNumber) {
    return 'YOUR GUESS IS TOO BIG';
  } else if (guess < magicNumber) {
    return 'YOUR GUESS IS TOO SMALL';
  } else if (guess == magicNumber) {
    return 'YOU DID IT! 🎉';
  } else {
    return 'INVALID INPUT';
  }
}

function guessingGame() {
  const magicNumber = 22;
  const guess = prompt('guess a number between 1 and 100!');
  const message = evaluateGuess(magicNumber, guess);
  alert(message);
}

guessingGame();
```

Trong ví dụ này, điều duy nhất chúng ta thực sự cần test là function `evaluateGuess`, function này dễ test hơn nhiều vì có input và output rõ ràng, đồng thời không gọi function bên ngoài nào. Cách triển khai này cũng *tốt* hơn nhiều vì dễ mở rộng hơn. Nếu muốn thay `prompt` và `alert` bằng các method thao tác DOM, giờ đây chúng ta có thể làm việc đó dễ hơn; và nếu muốn làm game nâng cao hơn bằng cách cho phép người dùng đoán nhiều lần, việc đó cũng dễ hơn.

Nếu viết chương trình này theo TDD, rất có thể ngay từ đầu nó đã trông giống ví dụ thứ hai hơn. Test driven development khuyến khích kiến trúc chương trình tốt hơn vì khuyến khích bạn viết *Pure Functions*.

### Mocking

Có hai giải pháp cho vấn đề 'tightly coupled code'. Giải pháp đầu tiên và tốt nhất là loại bỏ các dependency khỏi code như chúng ta đã làm ở trên, nhưng điều đó không phải lúc nào cũng khả thi. Giải pháp thứ hai là **mocking** - viết các phiên bản "giả" của một function, luôn hoạt động *chính xác* theo cách bạn muốn. Ví dụ, nếu đang test một function lấy thông tin từ input DOM, bạn thực sự không muốn phải thiết lập một webpage rồi chèn động một thứ gì đó vào input chỉ để chạy test. Với một mock function, bạn chỉ cần tạo một phiên bản giả của function lấy input, phiên bản này luôn trả về một giá trị cụ thể, rồi dùng CHÍNH phiên bản đó trong test.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết ngắn này về [giá trị của "pure functions"](https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c).
1. Xem [video về mock trong testing](https://www.youtube.com/watch?v=ajiAl5UNzBU&t=3024s).
1. Bây giờ bạn đã có một chút thực hành và bối cảnh về TDD, phần [Setup and Teardown trong tài liệu Jest](https://jestjs.io/docs/setup-teardown) có lẽ sẽ trở nên dễ hiểu với bạn.
1. Đọc về [các function mocking rất tiện dụng của Jest](https://jestjs.io/docs/mock-functions).
1. Xem video tuyệt vời này trình bày [nên test gì trong codebase](https://www.youtube.com/watch?v=URSWYvyc42M). Video nói cụ thể về việc test ngôn ngữ Ruby, nhưng điều đó *hoàn toàn* không quan trọng. Các khái niệm trong video đúng với mọi ngôn ngữ, và may mắn là Ruby đủ rõ ràng để bạn có thể dễ dàng theo dõi.

</div>