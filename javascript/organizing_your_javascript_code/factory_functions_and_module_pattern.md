### Giới thiệu

Chúng ta đã thảo luận về object constructor trong bài trước. Tuy nhiên, chúng chỉ là một trong nhiều cách để tổ chức code. Dù khá phổ biến và là một khối xây dựng nền tảng của ngôn ngữ JavaScript, chúng vẫn có những hạn chế.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Variable scope.
- Closure.
- Factory function.
- Biến private.
- Composition.
- IIFE và module pattern.
- Encapsulation.

### Những phần nhỏ của scope

Từ "scoping" về cơ bản đặt ra câu hỏi: "Một biến nào đó khả dụng ở đâu?" - nó cho biết context hiện tại của một biến. Khi một biến không được khai báo bên trong **bất kỳ** function nào, tồn tại bên ngoài mọi `{ curly braces }`, người ta nói nó nằm trong **global scope**, nghĩa là nó khả dụng ở mọi nơi. Nếu nằm bên trong một function hoặc `{ curly braces }`, nó được xem là có **local scope**.

Trước ECMAScript 6, JavaScript chỉ có một keyword để khai báo biến là `var`. Các biến này có thể được định nghĩa lại và cập nhật, và được xem là thuộc **function scope**, nghĩa là chúng chỉ khả dụng bên trong function nơi được khai báo.

Trong ECMAScript 6, các keyword `let` và `const` được giới thiệu. Trong khi biến `var` có function scope, các keyword này cho phép bạn định nghĩa biến có **block scope** - về cơ bản là giới hạn biến chỉ khả dụng trong tập `{ curly braces }` gần nhất nơi nó được định nghĩa. Những dấu ngoặc này có thể là của vòng lặp `for`, điều kiện `if-else`, hoặc cấu trúc tương tự khác, và được gọi là block. Hãy xem một ví dụ để tổng hợp tất cả điều này.

```javascript
// This is a global variable
let globalAge = 23;

// This is a function - and hey, a curly brace indicating a block
function printAge(age) {
  // This is a function scoped variable
  var varAge = 34;

  // This is yet another curly brace, and thus a block
  if (age > 0) {
    // This is a block-scoped variable that exists
    // within its nearest enclosing block: the if's block
    const constAge = age * 2;
    console.log(constAge);
  }

  // ERROR! We tried to access a block scoped variable
  // outside its scope
  console.log(constAge);
}

printAge(globalAge);

// ERROR! We tried to access a function scoped variable
// outside the function it's defined in
console.log(varAge);
```

Hãy dành một chút thời gian ngẫm về ví dụ đó. Cuối cùng, đây không phải một khái niệm quá bất ngờ, nhưng có khá nhiều thuật ngữ trong đó - tất cả sẽ giúp chúng ta hiểu được chủ đề khổng lồ tiếp theo - closure.

### Closure không đáng sợ

Bạn biết rằng nếu cần lặp lại việc lấy hoặc tạo một giá trị dựa trên một loại input nào đó, bạn sẽ dùng function với parameter? Chẳng hạn, bạn có thể viết một function nhận argument và return array, string hoặc number do việc sử dụng các argument đó tạo ra. Function cũng không khác gì. Bạn có thể dùng một function để tạo ra một function khác.

Cách tốt nhất để tiếp cận điều này là bắt đầu bằng một ví dụ - hãy xem đoạn code dưới đây.

```javascript
function makeAddingFunction(firstNumber) {
  // firstNumber is scoped anywhere within makeAddingFunction,
  // including returnedFunction
  // any variables declared here will also be accessible within returnedFunction

  // we don't need to name the returned function
  // this is just to reference more easily in explanation
  return function returnedFunction(secondNumber) {
    // secondNumber is scoped only within returnedFunction
    return firstNumber + secondNumber;
  }
}
```

Sau đó chúng ta có thể tạo function bằng cách gọi `makeAddingFunction`:

```javascript
const add5 = makeAddingFunction(5);
console.log(add5(2)); // 7

const add8 = makeAddingFunction(8);
console.log(add8(2)); // 10

const add79100105110 = makeAddingFunction(79100105110);
console.log(add79100105110(111687378)); // 79211792488
```

Thay vì viết function mới mỗi lần, chúng ta chỉ cần dùng một function để tạo function khác - giống như cách ta có thể viết function `toFormattedDateString(date)` và gọi nó nhiều lần với các date khác nhau, thay vì hardcode logic và string kết quả cho từng lần.

`returnedFunction` tạo một closure quanh parameter `firstNumber`. Closure là sự kết hợp giữa một function và **state xung quanh** nơi function được khai báo. State xung quanh này, còn được gọi là **lexical environment**, bao gồm mọi biến local nằm trong scope tại thời điểm closure được tạo. Ở đây, `add5` là một reference tới function được return từ lời gọi `makeAddingFunction(5)`. Sau khi `makeAddingFunction(5)` thực thi xong, số 5 không bị xóa khỏi memory vì function được return vẫn cần nó: function được return có quyền truy cập lexical environment của nó (trong trường hợp này chứa parameter `firstNumber`).

Đây là một hành vi **cốt lõi** của function; nó cho phép chúng ta gắn data với function và thao tác trên data đó ở bất kỳ đâu bên ngoài function bao quanh. Chúng ta sẽ tận dụng điều này trong factory function ở phần sắp tới.

### Vậy constructor có vấn đề gì?

Có một vài điểm về constructor mà trong lịch sử một số người không thích. Điều đó không có nghĩa chúng tệ hoặc nên tránh dùng, chỉ là đôi khi khi tạo object, một số người không hài lòng về cách constructor hoạt động so với các lựa chọn khác.

Một ví dụ là chúng không cung cấp cơ chế bảo vệ tự động ngăn chúng ta dùng sai, chẳng hạn gọi chúng mà không có keyword `new` (bạn phải tự thêm cơ chế bảo vệ). Vì có cú pháp giống hệt function thông thường, chúng vẫn "hoạt động" nếu được gọi mà không có `new`. Trong một số trường hợp, điều này được chủ động tận dụng, chẳng hạn constructor [`Date()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date), vốn return một thứ khác nhau tùy việc được gọi với `new` hay không. Tuy nhiên, điều này không phải lúc nào cũng mong muốn. Quên thêm cơ chế bảo vệ có thể khiến bạn cố construct một object mà không có `new` và thay vì báo lỗi ngay lúc đó, nó vẫn "hoạt động" nhưng với hành vi sai (có thể tạo ra một lỗi rất khó hiểu ở nơi khác trong program).

Một điểm không hài lòng khác trong lịch sử đến từ cách `instanceof` *thực sự* hoạt động. Trong một số ngôn ngữ khác, keyword này là cách đáng tin cậy để biết code nào đã tạo ra một object. Trong JavaScript, nó kiểm tra sự hiện diện của prototype của một constructor trong *toàn bộ* prototype chain của object, việc này không xác nhận rằng object thực sự được tạo bởi constructor đó; prototype thậm chí có thể được gán lại sau khi object được tạo.

Vì vậy, một số người (không phải tất cả) không thích dùng constructor trong một số tình huống, thay vào đó ưa chuộng một pattern tương tự giải quyết được một số vấn đề này. Dù hiện nay chúng ta còn có nhiều cách hơn để làm việc với object/prototype mà không gặp nhiều hạn chế của constructor (sau này bạn sẽ học cú pháp class), đây vẫn là thứ thường xuyên xuất hiện trong thực tế và có ưu/nhược điểm riêng.

Hãy đến với "factory function".

### Factory function 🏭

Những function có cái tên nghe khá cầu kỳ này hoạt động rất giống constructor, nhưng có một khác biệt quan trọng - chúng tận dụng sức mạnh của closure. Thay vì dùng keyword `new` để tạo object, factory function thiết lập và return object mới khi bạn gọi function. Thực ra, chúng chỉ là function bình thường return object, không có gì quá kỳ diệu. Chúng chỉ có cái tên cầu kỳ là "factory function" vì được dùng theo một cách nhất định trong một context cụ thể (giống như một nhà máy sản xuất object).

Chúng không trực tiếp sử dụng prototype, điều này gây ra một hình phạt về hiệu năng, nhưng theo quy tắc chung, hình phạt này không đáng kể trừ khi bạn tạo hàng nghìn object (hãy tưởng tượng mỗi array bạn tạo đều đi kèm các bản sao riêng của mọi method array và object mỗi lần... vì chúng cơ bản đến mức nào và bạn có thể dùng bao nhiêu trong một program, điều đó sẽ thật điên rồ).

Hãy xem một ví dụ cơ bản để so sánh chúng với constructor function.

```javascript
function User(name) {
  this.name = name;
  this.discordName = "@" + name;
}
```

Đây là constructor... giờ có thể refactor nó thành factory!

```javascript
function createUser(name) {
  const discordName = "@" + name;
  return { name, discordName };
}
```

Tất cả khá giống constructor, ngoại trừ đây chỉ là một function bình thường, nghĩa là chúng ta không cần gọi nó với keyword `new`.

<div class="lesson-note" markdown="1">

#### Cú pháp viết tắt của object

Một số người có thể bối rối bởi cách object được return trong ví dụ factory function. Năm 2015, JavaScript được bổ sung một shortcut để tạo object. Giả sử muốn tạo một object với name, age và color, chúng ta sẽ viết như sau:

```javascript
const name = "Bob";
const age = 28;
const color = "red";

const thatObject = { name: name, age: age, color: color };
```

Tuy nhiên, nếu có một biến trùng tên với property mà chúng ta đang gán, giờ có thể chỉ viết tên đó một lần!

```javascript
const nowFancyObject = { name, age, color };
```

Một lợi ích khác là giờ có thể console.log các value một cách gọn gàng! Nếu trước đây muốn log chung các biến name, age và color, bạn sẽ làm như sau:

```javascript
console.log(name, age, color);
```

Điều này sẽ log ra thứ gì đó như `Bob 28 red`. Không *tệ*, chỉ là không rõ ràng nhất về việc đâu là gì. Thay vào đó, hãy thử bọc chúng trong dấu ngoặc nhọn để tạo thành object:

```javascript
// shorthand for console.log({ name: name, age: age, color: color })
console.log({ name, age, color });
```

Bây giờ nó sẽ log thành `{ name: "Bob", age: 28, color: "red" }`, rõ ràng hơn nhiều mà không cần tự thêm label!

#### Destructuring

Một expression khác cho phép bạn "unpack" hoặc "extract" value từ object (hoặc array). Đây được gọi là **destructuring**. Với object, bạn có thể extract một property của object vào một biến cùng tên, hoặc dùng bất kỳ tên biến nào cho array. Hãy xem ví dụ dưới đây:

```javascript
const obj = { a: 1, b: 2 };

// equivalent of doing
// const a = obj.a;
// const b = obj.b;
const { a, b } = obj;
```

Với array:

```javascript
const array = [1, 2, 3, 4, 5];

// equivalent of doing
// const zerothEle = array[0];
// const firstEle = array[1];
const [zerothEle, firstEle] = array;
```

[Tài liệu MDN về destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) có một số ví dụ rất hay và sẽ là tài liệu tốt để đọc về khái niệm này.

</div>

### Biến và function private

Có thể bạn đang nghĩ - closure liên quan đến tất cả chuyện này ở đâu? Factory dường như chỉ return một object. Đây là lúc chúng ta có thể mở rộng factory `User` để thêm một vài biến và giới thiệu những biến "private". Hãy xem ví dụ này:

```javascript
function createUser(name) {
  const discordName = "@" + name;

  let reputation = 0;
  const getReputation = () => reputation;
  const giveReputation = () => { reputation++; };

  return { name, discordName, getReputation, giveReputation };
}

const josh = createUser("josh");
josh.giveReputation();
josh.giveReputation();

// logs { discordName: "@josh", reputation: 2 }
console.log({
  discordName: josh.discordName,
  reputation: josh.getReputation()
});
```

Chúng ta đã giới thiệu một metric mới cho user mới - reputation. Hãy chú ý rằng object được return trong factory function không chứa chính biến `reputation`, cũng không chứa bản sao value của nó. Thay vào đó, object được return chứa hai function - một function đọc value của biến `reputation`, và một function tăng value lên một. Biến `reputation` được gọi là biến "private", vì chúng ta không thể truy cập trực tiếp biến này trong object instance - nó chỉ có thể được truy cập thông qua các closure mà chúng ta định nghĩa.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Pitfall: "Return biến trong object"

Đoạn sau sẽ không hoạt động như nhiều người ban đầu mong đợi:

```javascript
return { name, discordName, reputation };
```

Với cú pháp đầy đủ, điều này tương đương:

```javascript
return { name: name, discordName: discordName, reputation: reputation };
```

Chúng ta chỉ đang tạo một property riêng của object (tình cờ tự động được gọi là `reputation` nhờ cú pháp viết tắt của object) và gán cho nó value của biến `reputation`. Điều này cũng giống như việc gán một biến khác bằng value của một biến. Biến kia không "theo dõi" biến ban đầu. Đây là cùng hành vi với:

```javascript
let a = 1;
let b = a;
console.log(a, b); // 1 1

a = 5; // only reassigns a
console.log(a, b); // 5 1

b = 23; // only reassigns b
console.log(a, b); // 5 23
```

Cách *duy nhất* để truy cập biến private là thông qua closure.

</div>

Với factory function, một biến hoặc function private dùng closure để tạo ra các biến và function nhỏ hơn, chuyên biệt bên trong chính factory function - những thứ chúng ta *không cần* return trong object. Nhờ vậy, chúng ta có thể tạo code gọn gàng hơn mà không làm object được return bị đầy lên bởi các biến không cần thiết. Thông thường, bạn không cần return mọi function bên trong factory cùng object, hoặc expose một biến nội bộ. Bạn có thể dùng chúng ở chế độ private nhờ đặc tính của closure.

Trong trường hợp này, chúng ta không cần kiểm soát trực tiếp biến `reputation`. Để tránh các lỗi bất cẩn như vô tình đặt `reputation` thành `-18000`, chúng ta expose những chi tiết cần thiết dưới dạng `getReputation` và `giveReputation`.

<div class="lesson-note" markdown="1" >

#### Constructor và closure

Lưu ý rằng về mặt kỹ thuật bạn cũng có thể dùng closure trong constructor bằng cách định nghĩa các method truy cập một "property private" bên trong constructor thay vì trên prototype. Nhưng như vậy chúng sẽ không thể kế thừa, làm mất mục đích của constructor.

</div>

### Composition với factory

Trong bài về constructor, chúng ta đã tìm hiểu sâu về prototype và inheritance, cũng như cách cho object truy cập các property của object khác. Với factory function, chúng ta có thể mô phỏng hành vi tương tự inheritance, dù không thông qua cùng cơ chế prototype. Hãy xem xét một tình huống giả định khác. Chúng ta cần một factory `Player` có quyền truy cập một số (hoặc tất cả) property của `User`, bên cạnh property riêng của nó - có vài cách để làm điều đó:

```javascript
function createPlayer(name, level) {
  const { getReputation, giveReputation } = createUser(name);

  const getLevel = () => level;
  const increaseLevel = () => { level++; };
  return {
    name,
    getReputation,
    giveReputation,
    getLevel,
    increaseLevel,
  };
}
```

Vậy là xong! Bạn có thể tạo User, extract những gì cần dùng từ nó, rồi return lại bất kỳ thứ gì muốn - ẩn phần còn lại dưới dạng một số biến hoặc function private! Bạn cũng có thể dùng [`Object.assign` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) nếu có nhiều object muốn lấy dữ liệu:

```javascript
function createPlayer(name, level) {
  const user = createUser(name);

  const getLevel = () => level;
  const increaseLevel = () => { level++; };
  return Object.assign({}, user, { getLevel, increaseLevel });
}
```

Cách này vẫn tạo các instance hoàn toàn mới của những method "được kế thừa" cho mỗi object tạo bởi `createPlayer` (khác với việc dùng chung một method trong memory khi sử dụng prototypal inheritance), nhưng như đã nói trước đó, trừ khi tạo một số lượng object cực kỳ lớn, đây khó có thể là vấn đề đáng kể trong thực tế.

Đây được gọi là **composition**. Mặc dù cho phép làm những việc tương tự inheritance, nó đem lại cho chúng ta nhiều linh hoạt hơn, vì không nhất thiết phải lấy mọi thứ từ object khác (dù vẫn có thể lấy tất cả nếu muốn). Chúng ta "compose" một object mới từ nhiều nguồn. Đôi khi một bài toán phù hợp với inheritance, nhưng đôi khi cách đó có thể mong manh, và sự linh hoạt của composition rất phù hợp. Về kỹ thuật, bạn có thể compose bằng constructor, nhưng constructor phù hợp với hướng inheritance hơn nhiều; composition thường đến tự nhiên hơn với factory function.

### Module pattern

#### IIFE

Nhiều khi bạn không cần factory để tạo nhiều object - thay vào đó, bạn dùng nó để bọc các phần code lại với nhau, ẩn những biến và function không cần dùng ở nơi khác dưới dạng private. Có thể dễ dàng làm vậy bằng cách bọc factory function trong dấu ngoặc đơn rồi gọi (invoke) nó ngay lập tức. Lời gọi function ngay lập tức này thường được gọi là Immediately Invoked Function Expression (đúng như tên gọi) hoặc viết tắt là IIFE. IIFE đúng nghĩa chỉ là những function expression được gọi ngay lập tức:

```javascript
// This is a function expression
() => console.log("foo");

// The function expression is now an IIFE!
// Although this one is not particularly useful of course
(() => console.log("foo"))();
```

#### Dùng IIFE để triển khai module pattern

<div class="lesson-note lesson-note--warning" markdown="1">

#### ES6 module

ECMAScript 6 (phát hành năm 2015) đã giới thiệu một tính năng JavaScript mới gọi là "module", đây là một tập cú pháp để import và export code giữa các file JavaScript khác nhau. Hiện tại, chúng ta sẽ nói tổng quát hơn về module pattern dùng IIFE, thứ mà bạn vẫn sẽ thấy trong thực tế. Trong một bài sau, chúng ta sẽ trình bày cách dùng ES6 module cho những mục đích tương tự.

</div>

Một cách hữu ích hơn để dùng IIFE là pattern bọc code "private" bên trong IIFE: module pattern. Việc này thường được thực hiện với factory function:

```javascript
const calculator = (() => {
  let lastResult;

  const add = (a, b) => {
    lastResult = a + b;
    return lastResult;
  };
  const subtract = (a, b) => {
    lastResult = a - b;
    return lastResult;
  };
  const multiply = (a, b) => {
    lastResult = a * b;
    return lastResult;
  };
  const divide = (a, b) => {
    lastResult = a / b;
    return lastResult;
  };
  const getLastResult = () => lastResult;

  return { add, subtract, multiply, divide, getLastResult };
})();

console.log(calculator.add(3, 5)); // 8
console.log(calculator.subtract(6, 2)); // 4
console.log(calculator.getLastResult()); // 4
console.log(calculator.multiply(14, 5534)); // 77476
```

Ở đây, chúng ta có một calculator với bốn method số học cơ bản và một method để đọc kết quả của phép tính gần nhất. Chúng ta chỉ muốn một object calculator nhưng vẫn dùng factory function! Tại sao không dùng object literal trực tiếp?

Mọi property của object đều public dù chúng ta có muốn hay không. Nếu chỉ tạo một object literal calculator, chúng ta sẽ có property `.lastResult` là public, nghĩa là có khả năng gán lại trực tiếp (ví dụ `calculator.lastResult = 111100105110`). Chúng ta muốn giữ `lastResult` ở private và chỉ expose value để đọc công khai. Việc gán lại chỉ nên xảy ra nội bộ theo điều kiện của chúng ta. Cách duy nhất để thực sự ẩn biến `lastResult` khỏi mọi thứ không cần đến nó là đặt nó bên trong một function, tách khỏi scope bên ngoài, rồi tạo object trong cùng scope đó và return object. Một factory function... mà chúng ta chỉ cần gọi một lần!

Đây là lúc chúng ta gặp từ **encapsulation**: gói data, code hoặc một thứ gì đó vào một unit duy nhất, với quyền truy cập có chọn lọc tới những thứ bên trong chính unit đó. Dù nghe khá khái quát, đây chính là điều xảy ra khi chúng ta bọc (hay encapsulate) code vào module. Chúng ta không expose mọi thứ ra phần thân của program, chỉ expose những gì cần để các thành phần khác tương tác với những thứ bên trong "module".

#### Tại sao dùng IIFE?

Nhưng tại sao không chỉ viết factory function rồi gọi nó một lần? Tại sao phải dùng IIFE? Nếu không có IIFE, chúng ta phải đặt tên cho function để có thể gọi nó sau đó. Khi đã có tên, function vừa chiếm một tên trong scope đó vừa có thể được tái sử dụng trong scope ấy. Điều này có thể không mong muốn - có thể chúng ta cố tình chỉ muốn invoke nó một lần. Bằng cách bọc factory trong IIFE, chúng ta đạt được cùng flow của code nhưng không còn cần đặt tên cho function, đồng nghĩa với việc không thể reference nó về sau. Chúng ta đóng gói code tạo calculator vào thứ về bản chất là một module, rồi chỉ expose những gì cần dùng về sau trong code: object `calculator`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [bài viết về scope của Wes Bos](https://wesbos.com/javascript/03-the-tricky-bits/scope).
1. Đọc [bài viết về closure của Wes Bos](https://wesbos.com/javascript/03-the-tricky-bits/closures).
1. Đọc [hướng dẫn về closure của MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) (tạm bỏ qua ES6 module; chúng ta sẽ nói về chúng trong một bài sau).

</div>