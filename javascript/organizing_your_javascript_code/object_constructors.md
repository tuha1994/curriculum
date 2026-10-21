### Giới thiệu

Bây giờ bạn đã hiểu cơ bản về *why* và *how* bạn có thể sử dụng các đối tượng để sắp xếp dữ liệu và chức năng, điều quan trọng là phải tìm hiểu một số cơ sở chiến lược để tạo bản sao (thường được gọi là **instances**) của đối tượng và sử dụng các loại đối tượng hiện có để tạo mới đối tượng thông qua **inheritance**.

### Tổng quan bài học

Phần này chứa thông tin tổng hợp về các chủ đề mà bạn sẽ học trong bài học này.

- Khởi tạo các object bằng cách sử dụng các hàm tạo.
- Nguyên mẫu là gì và nó có thể được sử dụng như thế nào.
- Mẫu nguyên nguyên.
- Những điều nên và không nên làm cơ sở cho mẫu kế thừa.
- Cách từ khóa `this` hoạt động trong các vấn đề khác nhau.

### Hàm tạo đối tượng

Việc nhập nội dung thủ công của tất cả các đối tượng của chúng tôi bằng các đối tượng ký tự cũng không khả thi. Khi bạn có một loại công cụ mà bạn cần tạo thành nhiều đối tượng, cách tốt hơn để tạo chúng là sử dụng hàm tạo đối tượng, đây thực sự chỉ là một hàm:

```javascript
function Player(name, marker) {
  this.name = name;
  this.marker = marker;
}
```

Điểm khác biệt duy nhất là bạn sử dụng nó bằng cách gọi hàm với từ khóa `new`:

```javascript
const player = new Player("steve", "X");
console.log(player.name); // "steve"
```

Điều này không giống như gọi `Player("steve", "X")` (không có từ khóa `new`). Khi chúng ta gọi một hàm bằng `new`, nó sẽ tạo một đối tượng mới, làm cho `this` bên trong hàm tham chiếu đến đối tượng đó và làm cho đối tượng đó kế thừa thuộc tính `.prototype` của hàm (sẽ nói thêm về điều đó sau). Sau đó, mới đối tượng sẽ được trả về (mặc dù chúng tôi không định giá trị `return` trong hàm xây dựng).

Giống như các đối tượng được tạo bằng hằng đối tượng phương thức, bạn có thể thêm các hàm vào đối tượng:

```javascript
function Player(name, marker) {
  this.name = name;
  this.marker = marker;
  this.sayName = function() {
    console.log(this.name);
  };
}

const player1 = new Player("steve", "X");
const player2 = new Player("also steve", "O");
player1.sayName(); // logs "steve"
player2.sayName(); // logs "also steve"
```

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Bảo vệ hàm tạo

Vì các hàm có thể được gọi mà không sử dụng `new` nhầm lẫn, điều này sẽ gây ra các lỗi khó theo dõi vì nó sẽ không thực thi tất cả các đối tượng mới và liên kết nội dung `this`, chúng tôi nên bảo vệ chúng. Bạn có thể sử dụng meta thuộc tính `new.target` như thế này, điều này sẽ gây ra lỗi nếu `Player` được gọi mà không có `new`:

```javascript
function Player(name, marker) {
  if (!new.target) {
    throw Error("You must use the 'new' operator to call the constructor");
  }
  this.name = name;
  this.marker = marker;
  this.sayName = function() {
    console.log(this.name);
  };
}
```

</div>

### Bài tập

Viết hàm để tạo đối tượng "Sách". Chúng tôi sẽ xem xét lại điều này trong dự án tiếp theo. Đối tượng của bạn phải có `title`, `author` của cuốn sách, `pages` số và bạn có cuốn sách `read` hay không.

Đặt hàm `info()` vào hàm tạo để có thể báo cáo thông tin như sau:

```javascript
console.log(theHobbit.info()); // "The Hobbit by J.R.R. Tolkien, 295 pages, not read yet"
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### console.log vs return

Chúng tôi sử dụng các ví dụ về các hàm gọi `console.log()` để trình diễn, nhưng thay vào đó, chúng tôi tạo ra các hàm trực tiếp ghi lại mọi thứ, nói chung sẽ hợp lý hơn khi đặt chúng thành các `return` có giá trị. Bằng cách đó, bạn có thể chuyển các giá trị đến bất cứ nơi nào bạn muốn mà không bị ràng buộc với bất kỳ chức năng nào của chức năng đó; Không phải lúc nào bạn cũng có thể muốn ghi lại giá trị.

</div>

### Mẫu nguyên

Trong JavaScript, tất cả các đối tượng có thể liên kết với một đối tượng khác, được gọi là **prototype** của chúng (còn được gọi là `[[Prototype]]` của nó). Nếu thuộc tính hoặc phương thức không được tìm thấy trên đối tượng chính, JavaScript sẽ tìm kiếm nó trong `[[Prototype]]`.

Hãy phá vỡ nó.

#### 1. Tất cả các đối tượng trong JavaScript đều có [[Prototype]]

Câu nói khá đơn giản ở đây! Mọi đối tượng trong JavaScript đều có `[[Prototype]]`. Vì vậy, ví dụ, các đối tượng `player1` và `player2` từ trước, (được tạo bằng hàm tạo đối tượng `Player(name, marker)`) cũng có `[[Prototype]]`. Bây giờ, việc làm `[[Prototype]]` có nghĩa là gì? `[[Prototype]]` của một đối tượng là gì?

#### 2. [[Prototype]] là một đối tượng khác

Câu này cũng có vẻ khá đơn giản! `[[Prototype]]` * chỉ là một đối tượng khác* - một lần nữa, giống như các đối tượng `player1` và `player2`. Đối tượng `[[Prototype]]` có thể có các thuộc tính và chức năng, giống như các đối tượng `Player` này có các thuộc tính như `.name`, `.marker` và các chức năng như `.sayName()` được gắn vào chúng.

#### 3. ...object ban kế thừa và có quyền truy cập vào tất cả các phương thức và thuộc tính của [[Prototype]] của nó

Ở đây, "object ban start" đề cập đến một đối tượng như `player1` hoặc `player2`. Các đối tượng này được cho là "kế thừa", hay nói cách khác, các đối tượng này có quyền truy cập vào các thuộc tính hoặc chức năng của `[[Prototype]]`, nếu chúng đã được xác định rõ. Ví dụ: nếu có một hàm `.sayHello()` được xác định trên `[[Prototype]]`, thì `player1` có thể truy cập các hàm đó giống như đó là hàm của chính nó - `player1.sayHello()`. Nhưng không chỉ `player1` mới có thể gọi hàm `.sayHello()`, ngay cả `player2` cũng có thể gọi hàm này, vì nó được xác định trên `[[Prototype]]` của nó! Vui lòng đọc tiếp để biết chi tiết về cách thức hoạt động và cách bạn có thể tự thực hiện công việc này!

#### Truy cập [[Prototype]] của đối tượng

Về khái niệm, bây giờ bạn có thể cảm thấy như mình biết hoặc ít nhất có ý tưởng về `[[Prototype]]` của một đối tượng là gì. Nhưng làm thế nào để bạn *biết* hoặc thực sự *thấy* prototype của một đối tượng là gì? Hãy cùng tìm hiểu. Bạn có thể thử chạy đoạn mã sau trong bảng điều khiển dành cho nhà phát triển trình duyệt. (Đảm bảo rằng bạn đã tạo các đối tượng `player1` và `player2` từ trước!)

```javascript
Object.getPrototypeOf(player1) === Player.prototype; // returns true
Object.getPrototypeOf(player2) === Player.prototype; // returns true
```

Bây giờ, để hiểu mã này, hãy sử dụng ba điểm ở trên:

1. **Tất cả các đối tượng trong JavaScript đều có `[[Prototype]]`**:
   - Bạn có thể kiểm tra `[[Prototype]]` của đối tượng bằng cách sử dụng chức năng [`Object.getPrototypeOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) trên đối tượng, như `Object.getPrototypeOf(player1)`.
   - `Object.getPrototypeOf(player1)` sẽ trả về object thuộc tính `.prototype` của hàm tạo `Player` (tức là `Player.prototype`).
1. **`[[Prototype]]` là một đối tượng khác...**
   - *Giá trị* của `Player.prototype` chứa một đối tượng.
   - *tham chiếu* đến `Player.prototype` được lưu trữ trong mọi phiên bản của đối tượng `Player` dưới dạng `[[Prototype]]` của nó.
   - Do đó, `true` sẽ được thanh toán khi bạn nhận được `[[Prototype]]` của `player1` và kiểm tra sự tham chiếu đối tượng tại `Player.prototype`.
1. **...mà đối tượng ban đầu *inherit* từ đó và có quyền truy cập vào tất cả các phương thức và thuộc tính của `[[Prototype]]` của nó**:
   - Vì vậy, mọi thuộc tính hoặc phương thức được xác định trên `Player.prototype` sẽ có sẵn cho các đối tượng `Player` đã được tạo!

Mục phụ cuối cùng cần giải quyết thêm một chút. Việc xác định 'trên mẫu nguyên' có nghĩa là gì? Vui lòng xem đoạn mã sau:

```javascript
Player.prototype.sayHello = function() {
  console.log("Hello, I'm a player!");
};

player1.sayHello(); // logs "Hello, I'm a player!"
player2.sayHello(); // logs "Hello, I'm a player!"
```

Ở đây, chúng tôi đã định nghĩa hàm `.sayHello` “trên” đối tượng `Player.prototype`. Sau đó, nó đã có sẵn các đối tượng `player1` và `player2` để sử dụng! Tương tự, bạn có thể đính kèm các thuộc tính hoặc chức năng khác mà bạn muốn sử dụng trên tất cả các đối tượng `Player` bằng cách xác định chúng trên `[[Prototype]]` của đối tượng (là `Player.prototype`).

<div class="lesson-note" markdown="1">

#### .\_\_proto__

Trong một số tài liệu hoặc cơ sở dữ liệu, bạn có thể thấy `.__proto__` đang được sử dụng trên một đối tượng để lấy hoặc đặt `[[Prototype]]` của nó thay vì sử dụng `Object.getPrototypeOf()` hoặc `Object.setPrototypeOf()`. Điều này là do lý do lịch sử; [`.__proto__` là một cách tiếp cận không chuẩn và không được sử dụng nữa](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto), vì vậy, không nên sử dụng nó để truy cập `[[Prototype]]` của đối tượng.

</div>

<div class="lesson-note" markdown="1">

#### Object.getPrototypeOf() so với .prototype

Nguyên nhân phổ biến gây nhầm lẫn từ việc xử lý thuộc tính `.prototype` của các hàm xây dựng.

`.prototype` là thuộc tính của các chức năng xác định `[[Prototype]]` của mới phiên bản đối tượng sẽ được cài đặt thành công khi hàm được gọi bằng `new`. `.prototype` là *not* để truy cập `[[Prototype]]` của một đối tượng - đó là mục tiêu của `Object.getPrototypeOf()`.

![Sơ đồ kế thừa mẫu](https://cdn.statically.io/gh/TheOdinProject/curriculum/cffc199a8cfbfcd61160b00c4cf61e1d6bb6ff2e/javascript/organizing_your_javascript_code/object_constructors/imgs/00.png)

</div>

#### Mẫu nguyên nguyên kế hoạch

Bây giờ, bạn cũng có thể có câu hỏi - `[[Prototype]]` của một đối tượng có tác dụng gì? Mục đích của việc xác định các thuộc tính và chức năng theo mẫu nguyên là gì?

Chúng ta có thể thu nhỏ nó xuống còn hai lý do:

1. Chúng tôi có thể định nghĩa các thuộc tính và chức năng chung giữa tất cả các đối tượng trên mẫu nguyên để tiết kiệm bộ nhớ. Việc xác định mọi thuộc tính và hàm sử dụng rất nhiều bộ nhớ, đặc biệt nếu bạn có nhiều thuộc tính và hàm chung cũng có nhiều đối tượng được tạo! Xác định chúng trên một đối tượng được chia sẻ, tập trung mà các đối tượng có quyền truy cập sẽ tiết kiệm bộ nhớ.
1. Lý do thứ hai cho phép nhiều đối tượng chia sẻ và sử dụng lại hành vi thông qua cơ chế kế thừa mẫu của JavaScript, mặc dù cho phép `player1` và `player2` truy cập các chức năng như `.sayHello` được xác định trên `Player.prototype`.

Bây giờ chúng ta hãy thử làm như sau:

```javascript
// Player.prototype.__proto__
Object.getPrototypeOf(Player.prototype) === Object.prototype; // true

// Output may slightly differ based on the browser
player1.valueOf(); // Output: Object { name: "steve", marker: "X", sayName: sayName() }
```

Hàm `.valueOf` này là gì và nó đến từ đâu nếu chúng ta không định nghĩa nó? Nó xuất hiện `Object.getPrototypeOf(Player.prototype)` có giá trị `Object.prototype`! Điều này có nghĩa là `Player.prototype` được kế thừa từ `Object.prototype`. Hàm `.valueOf` này được xác định trên `Object.prototype` tương tự như `.sayHello` được xác định trên `Player.prototype`.

Làm sao chúng tôi biết rằng hàm `.valueOf` này được xác định trên `Object.prototype`? Chúng tôi sử dụng một chức năng khác được gọi là `.hasOwnProperty`:

```javascript
player1.hasOwnProperty("valueOf"); // false
Object.prototype.hasOwnProperty("valueOf"); // true
```

Bây giờ [Chức năng `.hasOwnProperty`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty) này đến từ đâu? Việc kiểm tra nhanh sẽ giúp:

```javascript
Object.prototype.hasOwnProperty("hasOwnProperty"); // true
```

Về cơ bản, đây là cách JavaScript sử dụng mẫu nguyên. Một đối tượng kế thừa từ đối tượng `[[Prototype]]` của nó, đối tượng này lại kế thừa từ `[[Prototype]]` của chính nó, v.v., do đó tạo thành một chuỗi. Do đó, thừa kế kiểu sử dụng mẫu nguyên này được đặt tên là mẫu kế thừa nguyên. JavaScript tìm các thuộc tính tồn tại (hoặc không tồn tại) trên đối tượng và bắt đầu trình duyệt qua chuỗi để tìm thuộc tính hoặc hàm, chẳng hạn như sau:

1. Hàm `.valueOf` có phải là một phần của đối tượng `player1` không? Không, không phải vậy. (Xin nhớ rằng, các thuộc tính duy nhất `name`, `marker` và `sayName` là một thành phần của `Player` object.)
1. Chức năng này có phải là một thành phần của `[[Prototype]]` của `player1` (giá trị `Object.getPrototypeOf(player1)`, tức là `Player.prototype`) không? Không, `.sayHello` chức năng chỉ là một thành phần của nó.
1. Vậy thì no có phải là một phần của `Object.getPrototypeOf(Player.prototype)` (=== `Object.prototype`) không? Có, `.valueOf` được xác định trên `Object.prototype`!

Tuy nhiên, chuỗi này không tồn tại mãi mãi và nếu bạn đã thử ghi lại giá trị của `Object.getPrototypeOf(Object.prototype)`, bạn sẽ thấy rằng đó là `null`, hãy biết điểm cuối của chuỗi. Và ở chuỗi cuối cùng này, nếu thuộc tính hoặc hàm cụ thể không được tìm thấy, `undefined` sẽ được trả về.

Ghi chú:

1. Mọi đối tượng nguyên mẫu đều được kế thừa từ `Object.prototype` theo mặc định, dù trực tiếp hay gián đoạn.
1. Giá trị `Object.getPrototypeOf()` của một đối tượng chỉ có thể là *one* có giá trị (một đối tượng không thể có nhiều `[[Prototype]]`).

#### Phương pháp được xuất ra để kế thừa mẫu nguyên

Bây giờ, bạn làm cách nào để sử dụng mẫu Kế hoạch nguyên nguyên? Bạn cần làm gì để sử dụng nó? Giống như chúng ta sử dụng `Object.getPrototypeOf()` để 'tìm' hoặc xem `[[Prototype]]` của một đối tượng, chúng ta có thể sử dụng `Object.setPrototypeOf()` để 'đặt' hoặc thay đổi nó. Vui lòng xem nó hoạt động như thế nào bằng cách thêm Trình xây dựng đối tượng `Person` vào ví dụ `Player` và làm cho `Player` kế thừa từ `Person`!

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayName = function() {
  console.log(`Hello, I'm ${this.name}!`);
};

function Player(name, marker) {
  this.name = name;
  this.marker = marker;
}

Player.prototype.getMarker = function() {
  console.log(`My marker is "${this.marker}"`);
};

Object.getPrototypeOf(Player.prototype); // returns Object.prototype

// Now make `Player` objects inherit from `Person`
Object.setPrototypeOf(Player.prototype, Person.prototype);
Object.getPrototypeOf(Player.prototype); // returns Person.prototype

const player1 = new Player("steve", "X");
const player2 = new Player("also steve", "O");

player1.sayName(); // Hello, I'm steve!
player2.sayName(); // Hello, I'm also steve!

player1.getMarker(); // My marker is "X"
player2.getMarker(); // My marker is "O"
```

Từ mã, chúng tôi có thể thấy rằng chúng tôi đã xác định một `Person` mà `Player` kế thừa các thuộc tính và chức năng, đồng thời các đối tượng `Player` đã tạo có thể truy cập cả hai chức năng `.sayName` và `.getMarker`, mặc dù chúng được xác định rõ ràng trên hai đối tượng `.prototype` riêng biệt! Điều này được kích hoạt bằng cách sử dụng chức năng `Object.setPrototypeOf()`. Phải mất hai đối số - đối số đầu tiên là đối số kế thừa và đối số thứ hai là đối số mà bạn muốn đối số thứ nhất tiếp theo. Điều này đảm bảo rằng các đối tượng `Player` được tạo có thể truy cập các hàm `.sayName` và `.getMarker` thông tin qua mẫu nguyên chuỗi của chúng.

Ghi chú:

Mặc dù đây có vẻ là một cách dễ dàng để thiết lập Kế thừa nguyên mẫu bằng `Object.setPrototypeOf()`, nhưng mẫu nguyên chuỗi phải được thiết lập bằng chức năng này * trước khi* tạo bất kỳ đối tượng nào. Sử dụng `setPrototypeOf()` sau khi các đối tượng đã được tạo có thể dẫn đến các vấn đề về hiệu suất.

Một cảnh báo...điều này không có hiệu quả:

```javascript
Player.prototype = Person.prototype;
```

Cả `Player.prototype` và `Person.prototype` đều trở thành cùng một đối tượng trong bộ nhớ. Điều này có nghĩa là bất kỳ thay đổi nào được thực hiện đối với `Player.prototype` cũng sẽ ảnh hưởng đến `Person.prototype`, đây không phải là hành vi dự kiến. Thay vào đó, chúng tôi nên làm cho `Player.prototype` kế thừa từ `Person.prototype`, thay vì các biến chúng thành cùng một đối tượng. Hãy xem xét một ví dụ nữa:

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayName = function() {
  console.log(`Hello, I'm ${this.name}!`);
};

function Player(name, marker) {
  this.name = name;
  this.marker = marker;
}

// Don't do this!
// Use Object.setPrototypeOf(Player.prototype, Person.prototype)
Player.prototype = Person.prototype;

function Enemy(name) {
  this.name = name;
  this.marker = "^";
}

// Not again!
// Use Object.setPrototypeOf(Enemy.prototype, Person.prototype)
Enemy.prototype = Person.prototype;

Enemy.prototype.sayName = function() {
  console.log("HAHAHAHAHAHA");
};

const carl = new Player("carl", "X");
carl.sayName(); // Uh oh! this logs "HAHAHAHAHAHA" because we edited the sayName function!
```

Nếu chúng tôi đã sử dụng `Object.setPrototypeOf()` trong ví dụ này, chúng tôi có thể chỉnh sửa hàm `Enemy.prototype.sayName` một cách an toàn mà không cần thay đổi hàm cho `Player`.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc về mẫu khái niệm nguyên từ các bài viết dưới đây.
   1. Đọc bài viết [Tìm hiểu về nguyên mẫu và tính kế thừa trong JavaScript](https://www.digitalocean.com/community/tutorials/understanding-prototypes-and-inheritance-in-javascript) từ Digital Ocean. Đây là một bài đánh giá hay về các mẫu xây dựng và kế hoạch nguyên tử, bao gồm một số ví dụ.
   1. Để tìm hiểu sâu hơn một chút về cả chuỗi và kế hoạch chi tiết, hãy dành thời gian xem [bài viết của JavaScript.Info về Kế thừa mẫu](http://javascript.info/prototype-inheritance). Theo quy định thông thường, việc làm bài tập cuối cùng sẽ giúp bạn cố định kiến ​​thức này ngay từ đầu. Đừng bỏ qua chúng! Lưu ý quan trọng: Bài viết này sử dụng nhiều `__proto__`, điều này thường không được khuyến khích. Các khái niệm ở đây là những gì chúng tôi đang tìm kiếm vào lúc này. Chúng ta sẽ sớm tìm hiểu một hoặc hai phương pháp khác để thiết lập mẫu nguyên.
1. Bạn có thể đã nhận thấy chúng tôi sử dụng từ khóa `this` trong hàm tạo đối tượng và mẫu nguyên thức phương pháp trong các ví dụ trên. [Bài viết của Hướng dẫn JavaScript về từ khóa `this`](https://www.javascripttutorial.net/javascript-this/) trình bày cách `this` thay đổi trong các vấn đề khác nhau. Đặc biệt chú ý đến những bẫy được đề cập trong mỗi phần.

</div>
