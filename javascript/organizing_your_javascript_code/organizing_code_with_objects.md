### Giới thiệu

Trong khóa học cơ bản về JavaScript của chúng tôi, bạn đã chắc chắn được học [cơ sở về sử dụng đối tượng](https://www.theodinproject.com/lessons/foundations-object-basics) để lưu trữ và truy xuất dữ liệu. Trong bài học này, chúng ta sẽ bắt đầu xem lại một chút, sau đó khám phá cách sử dụng các đối tượng một cách chi tiết hơn. Những người tham gia khóa học Ruby, bạn sẽ có thêm một chút kinh nghiệm với một số khái niệm này, nhưng bạn vẫn nên học qua bài học này vì JavaScript linh hoạt hơn một chút trong công việc sử dụng các đối tượng.

### Tổng quan bài học

Phần này chứa thông tin tổng hợp về các chủ đề mà bạn sẽ học trong bài học này.

- Sử dụng các đối tượng để tổ chức dữ liệu
- Sử dụng các đối tượng để tổ chức các chức năng.
- Đối tượng phương thức và từ khóa `this`.

### Làm mới

Có nhiều cách để xác định đối tượng trong JavaScript, nhưng trong nhiều trường hợp, cú pháp **object Literal** được sử dụng như sau:

```javascript
const myObject = {
  property: "Value!",
  otherProperty: 77,
  "obnoxious property": function() {
    // do stuff!
  },
};
```

Ngoài ra, còn có 2 cách để lấy thông tin từ đối tượng: dấu chấm và dấu ngoặc ký hiệu.

```javascript
// dot notation
console.log(myObject.property); // "Value!"

// bracket notation
console.log(myObject["obnoxious property"]); // [Function]
```

Phương pháp nào bạn sử dụng sẽ phụ thuộc vào ngữ cảnh. Dấu chấm rõ ràng hơn và thường được sử dụng thích hợp hơn, nhưng có nhiều trường hợp không thể sử dụng nó. Ví dụ: `myObject."obnoxious property"` sẽ không hoạt động vì thuộc tính đó là một chuỗi có khoảng trắng trong đó. Tương tự, bạn không thể sử dụng các biến trong dấu chấm ký hiệu:

```javascript
const variable = "property";

// "undefined" because it's looking for a property named "variable" in our object
console.log(myObject.variable);

// this is equivalent to myObject["property"] and returns "Value!"
console.log(myObject[variable]);
```

### Object là dữ liệu cấu trúc

Bạn đã được giới thiệu cách sử dụng đối tượng cơ bản JavaScript - lưu trữ thông tin liên quan bằng cặp khóa/giá trị. Đây là một trong những cách đơn giản nhất để bạn có thể bắt đầu sắp xếp mã của mình! Lấy những ví dụ sau từ trò chơi 'tic tac toe' - đầu tiên không có đồ vật:

```javascript
// without objects
const playerOneName = "tim";
const playerTwoName = "jenn";
const playerOneMarker = "X";
const playerTwoMarker = "O";
```

Show đang sử dụng các đối tượng:

```javascript
// with objects
const playerOne = {
  name: "tim",
  marker: "X",
};

const playerTwo = {
  name: "jenn",
  marker: "O",
};
```

nhìn, cách đầu tiên có vẻ không tệ lắm... nhưng hữu ích của cách tiếp cận thứ hai là rất có lợi! Vì các giá trị được gán cho các khóa trong đối tượng, thay vì một loạt các biến tên dài và đặc biệt, chúng tôi có thể sử dụng các biến tên ngắn gọn hơn, dễ đọc hơn trong mắt, trong đó đối tượng tên giúp cung cấp cảnh. Thông thường, `name` và `marker` sẽ không thể tái sử dụng trong cùng một phạm vi, nhưng thông tin qua "không có tên", chúng ta vẫn có thể sử dụng chúng như một phần của `playerOne.name` hoặc `playerTwo.marker`, v.v. Những người theo dõi Ruby có thể thực hiện công việc tương tự với các mô-đun của Ruby; trong JavaScript, việc sử dụng các đối tượng cho mục đích này là điều bình thường.

Nhưng việc đặt tên không phải là lợi ích duy nhất. Việc nhóm các liên kết lại với nhau thành các đối tượng cho phép bạn truyền tải tất cả dữ liệu một cách dễ dàng hơn. Ví dụ:

```javascript
function gameOver(winningPlayer) {
  console.log("Congratulations!");
  console.log(`${winningPlayer.name} (${winningPlayer.marker}) is the winner!`);
}
```

Thay vào đó, họ phải có các tham số riêng cho tên và dấu đánh dấu, chúng tôi chỉ có một tham số cho toàn bộ đối tượng và hiện tại, họ có quyền truy cập vào tất cả các thuộc tính bên trong. Nếu cuối cùng chúng ta bổ sung thêm nhiều thuộc tính hơn cho việc phát hiện các đối tượng và cần chúng trong hàm `gameOver`, thì một lần nữa, chúng ta lại có quyền truy cập vào thuộc tính đó vì chúng ta đã chuyển toàn bộ đối tượng vào, thay vào đó vào các lẻ tham số riêng.

Nhưng nếu chúng ta không tạo ra một trò chơi 2 người đơn giản thì sao? Một cái gì đó phức tạp hơn ngoài giới hạn như một trang web mua sắm trực tuyến có số lượng hàng tồn tại kho lớn? Sử dụng các đối tượng để nhóm tên, giá, mô tả và các thứ khác của từng mặt hàng cụ thể lại với nhau là cách duy nhất. Bạn sẽ tiếp tục sử dụng và khảo sát các đồ vật được sử dụng theo cách này trong suốt chương trình giảng dạy.

### Làm thiết kế mẫu đối tượng

Khả năng nhóm của đối tượng không chỉ hữu ích để tổ chức dữ liệu mà còn hữu ích để tổ chức *chức năng*! Sử dụng đối tượng cho mục đích này là một trong những nguyên lý cốt lõi của lập trình hướng đối tượng (OOP), một mô hình lập trình dựa trên khái niệm "đối tượng", có thể chứa dữ liệu và mã: dữ liệu ở dạng trường (thường được gọi là thuộc tính) và mã ở dạng thủ tục (thường được gọi là phương thức). Trong OOP, chương trình máy tính được thiết kế bằng cách tạo ra các đối tượng tương tác với nhau.

Điều này có nghĩa là chúng tôi không bị giới hạn trong việc lưu trữ dữ liệu trong các đối tượng, chúng tôi cũng có thể lưu trữ logic thông tin qua **methods** (chỉ là các hàm là một phần của đối tượng), sau đó sử dụng các phương thức đó để tương tác với dữ liệu.

Gần giống như *bất cứ thứ gì* bạn có thể nghĩ đến đều có thể được mô tả như một đối tượng. Để làm như vậy, tất cả những gì bạn phải tự hỏi mình là "Đồ vật của tôi có những đặc tính gì (vật lý hoặc khái niệm)?" và "Làm cách nào để tôi có thể tương tác với nó?". Các thuộc tính hoặc thuộc tính của *thing* có thể hiển thị dưới dạng thuộc tính khác nhau và cách bạn có thể tương tác với thứ có thể hiển thị dưới dạng phương thức.

Vui lòng lấy một số ví dụ về *thing* - chúng tôi sẽ chọn một chiếc ô cho. Một chiếc ô có thể có nhãn hiệu, kiểu dáng, năm đăng ký, màu sắc và cả giá cả. Chúng tôi có thể hiển thị dưới dạng thuộc tính của một đối tượng:

```javascript
const car = {
  make: "Volkswagen",
  model: "Golf",
  year: 2026,
  color: "blue",
  priceUSD: 40000,
};
```

Bạn có thể muốn có khả năng áp dụng giảm giá cho chiếc xe hoặc nhận được bản tóm tắt tất cả các chi tiết một lần. Đối với điều này, bạn có thể muốn sử dụng các phương pháp. Cách dễ dàng nhất để bắt đầu tạo các phương thức tương tác với các đối tượng của bạn có thể là kết hợp cú pháp nghĩa đen của đối tượng với từ khóa `this` của JavaScript. Từ khóa `this` được sử dụng để chỉ đối tượng mà một phương thức có thể được gọi từ đó.

```javascript
const car = {
  make: "Volkswagen",
  model: "Golf",
  year: 2026,
  color: "blue",
  priceUSD: 40000,

  // a method is just a function assigned to a property
  applyDiscount: function(discountPercentage) {
    const multiplier = 1 - discountPercentage / 100;
    this.priceUSD *= multiplier;
  },
  // shorthand way to add a method to an object literal
  getSummary() {
    return `${this.year} ${this.make} ${this.model} in ${this.color}, priced at $${this.priceUSD} (USD).`;
  },

  // ...any other methods...
};
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Hàm mũi tên và "cái này"

Từ khóa `this` hoạt động khác nhau trong các hàm mũi tên nên với các biểu thức truyền hàm hàm (bao gồm cú pháp tốc ký). Chúng tôi không cần phải đi sâu vào công thức hoặc lý do khác, chỉ cần biết rằng nếu bạn sử dụng các hàm tên mũi tên trong ví dụ trên, chúng sẽ không hoạt động như bạn mong đợi.

</div>

Phương thức này sử dụng từ khóa `this` để chỉ đối tượng mà chúng được gọi từ đó (`car`). Từ khóa `this` có thể được sử dụng để đọc và gán các thuộc tính của một đối tượng theo cách tương tự như cách bạn làm với bất kỳ biến nào khác trỏ đến một đối tượng và chúng tôi sử dụng các phương thức có thể giống như cách chúng tôi sử dụng một hàm - tạo mã có thể sử dụng lại dưới một tên trực tuyến. Tốt hơn nhiều so với việc viết logic ra một cách thủ công mỗi khi chúng ta muốn thực hiện một số công việc đó.

#### Đối tượng cho các khái niệm đối tượng hơn

Di chuyển qua các vật phẩm, chúng ta cũng có thể cố gắng giải thích những vật thể nhỏ hơn một chút như trò chơi như một đồ vật. Vì chúng tôi đã khám phá Rock Paper Scissors trong Foundations, hãy lấy nó làm ví dụ.

Trò chơi độc quyền có thể bao gồm một số điều cơ bản:

- Điểm của người chơi
- Khả năng chơi một vòng (và chơi một vòng sẽ cập nhật điểm của người chơi)

Và cũng có thể bao gồm một vài điều thú vị:

- Khả năng xác định người chơi chiến thắng hiện tại
- Khả năng khởi động lại trò chơi

Vì vậy, về cơ bản nhất, một đối tượng đại diện cho trò chơi có thể trông giống như thế này (giả sử chúng ta đang chơi với một người chơi trên máy tính):

```javascript
const rps = {
  playerScore: 0,
  computerScore: 0,
  playRound(playerChoice) {
    // code to play the round, update score if needed, then return the result
  },
};
```

Và nếu chúng ta bổ sung nó, đối tượng của chúng ta có thể trông giống như thế này:

```javascript
const rps = {
  playerScore: 0,
  computerScore: 0,
  playRound(playerChoice) {
    // code to play the round, update score if needed, then return the result
  },
  getWinningPlayer() {
    // return the player with the most points ("player", "computer", or "tie")
  },
  reset() {
    // reset both players' scores to 0
  },
};
```

Hãy chơi một chút và xem ai là người dẫn đầu!

```javascript
rps.playRound("rock"); // returns "player" because we're awesome at RPS
console.log(rps.playerScore); // 1 - we won and so our score increased

rps.playRound("rock"); // returns "computer" because... luck...
console.log(rps.computerScore); // 1

rps.playRound("scissors"); // returns "player" because we're awesome at RPS (again)
console.log(rps.playerScore); // 2
console.log(rps.getWinningPlayer()); // "player" since we're 2-1 up
```

Chúng ta đã có đủ niềm vui cho một ngày rồi nên hãy thiết lập lại mọi thứ cho người tiếp theo nhé.

```javascript
rps.reset();
console.log(rps.playerScore); // 0
console.log(rps.computerScore); // 0
```

<div class="lesson-note" markdown="1">

#### Thuộc tính thuộc tính bên dưới

Ngoài ra, tự nhiên, bạn có thể tìm thấy mã thuộc tính đối tượng bắt đầu bằng `_`, ví dụ: `_someProperty`. Đây là quy định hoàn toàn dành cho nhà phát triển cho biết tài sản được coi là "riêng tư". Thuộc tính riêng tư là thuộc tính chỉ dành riêng cho mục đích sử dụng nội bộ và không thể đọc hoặc gọi bên ngoài đối tượng (một cách hạn chế như trợ giúp phương pháp hỗ trợ).

JavaScript thực sự không có khái niệm về thực tế thuộc tính riêng cho các đối tượng, ít nhất là không dành cho các nghĩa đen của đối tượng. Về mặt lịch sử, các nhà phát triển sẽ bổ sung thêm hàng `_` đầu tiên để biểu thị một thuộc tính nên được xử lý vì nó có thể được đặt riêng, ngay cả khi về mặt kỹ thuật, họ vẫn có thể truy cập được bên ngoài nội bộ đối tượng ("công khai"). Có nhiều cách để phát triển quyền riêng tư thực tế (điều này thực sự sẽ ngăn chặn việc truy cập vào thứ gì đó bên ngoài đối tượng) nhưng chúng tôi liên quan đến những thứ nâng cao hơn và sẽ được đề cập trong các bài học sau.

</div>

### Đối tượng là máy móc

Như chúng ta đã thấy, chúng ta có thể sử dụng các đồ vật để biểu thị không chỉ những vật chất mà còn cả những thứ mang tính khái niệm, ngoại trừ trò chơi Rock Paper Scissors. Các đối tượng có thể được sử dụng để hoàn tất mọi thứ bạn có thể nghĩ tới. Sẽ không thể đưa ra danh sách đầy đủ các ví dụ, nhưng một số cách sử dụng bao gồm:

- Một đối tượng quản lý các đối tượng khác, được coi là "kho" đối tượng chứa các mục đối tượng trong một mảng và các phương thức có thể được thực thi để tương tác với mảng đó
- Một đối tượng có thể lắng nghe các sự kiện xảy ra và phản hồi một cách thích hợp (hãy nghĩ đến `.addEventListener` trên các phần tử DOM)
- Một đối tượng quản lý tất cả các liên kết thứ yếu đến DOM, bằng cách thiết lập phương thức gọi sự kiện xử lý của các đối tượng khác và hiển thị dữ liệu từ các đối tượng khác trên trang web.

Lúc đầu, bạn có thể gặp khó khăn trong công việc tìm ra những loại đồ vật này có thể chứa gì, nhưng những thứ thứ này sẽ đi kèm với kinh nghiệm, đặc biệt là khi học tập sau này. Tuy nhiên, một cách mà bạn có thể khái niệm hóa hóa những đối tượng này là những tưởng tượng chúng giống như những "cỗ máy" nhỏ mà bạn đang tạo ra từ mã.

Các thuộc tính của máy có thể được coi là màn hình hiển thị thông tin như:

- Danh sách các sản phẩm bạn đã thu thập và số lượng sản phẩm tối đa mà bạn có thể mang theo
- Danh sách các chức năng đang lắng nghe một sự kiện
- Phần tử DOM dành riêng cho các nút tương tác và các phần tử để hiển thị dữ liệu

Các phương thức trên máy của bạn có thể giống với các nút và các phương thức làm cho máy *do* trở thành một công cụ thứ hai, mặc dù hạn chế như:

- Xóa mục bạn sở hữu khỏi danh sách và thêm mục mới
- Kích hoạt tất cả các chức năng đang nghe sự kiện "nhấp chuột" hoặc thêm chức năng mới để nghe sự kiện "nhấp chuột"
- Đọc dữ liệu từ nơi khác và đặt `.textContent` của một số phần tử DOM nhất

Một lần nữa, các đối tượng có thể được sử dụng để thực hiện hầu hết mọi thứ bạn có thể nghĩ tới. Giới hạn là trí tưởng tượng của bạn!

### Phân công

<div class="lesson-content__panel" markdown="1">

Không có bài tập cho bài học cụ thể này! Mặc dù JavaScript là một ngôn ngữ rất linh hoạt bao gồm các khái niệm từ nhiều mô hình cài đặt khác nhau, nhưng phần lớn trong số đó được xây dựng xung quanh "Lập trình đối tượng hướng dẫn" (OOP).

Mặc dù bài học này đã đề cập đến những ý tưởng cơ bản đằng sau lý do tại sao chúng ta có thể sử dụng đồ vật ngay từ đầu, nhưng chúng ta sẽ khám phá những khái niệm này bằng một cách thực tế hơn nhiều trong các bài học sắp xếp tới thông qua vô số kỹ thuật tạo và sử dụng đồ vật.

</div>
