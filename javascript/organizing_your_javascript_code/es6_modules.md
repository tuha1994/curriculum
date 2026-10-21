### Giới thiệu

Chúng ta đã học về **module pattern** trong một bài trước và thử dùng nó để giúp tổ chức các biến và function. Có lúc trong một vài project gần đây, bạn thậm chí có thể đã tự hỏi: "Chúng ta sẽ quản lý những project phức tạp hơn như thế nào? File sẽ dài quá! Sẽ thật tuyệt nếu có thể chia code thành nhiều file để tổ chức tốt hơn!" Việc sử dụng nhiều file sẽ cực kỳ hữu ích chính vì lý do này.

Trước đây, module pattern đóng vai trò lớn trong việc giúp chúng ta quản lý điều này, nhưng sự ra mắt của ES6 (đôi khi được gọi là ES2015) đã mang đến các "module" thực sự, vì vậy chúng thường được gọi là "ES6 module" hoặc "ESM".

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Giải thích ES6 module là gì và cách import, export từ chúng.
- Mô tả sự khác nhau giữa default export và named export.
- Giải thích những khác biệt chính giữa CommonJS module và ES6 module.

### Trước ES6 module: Vấn đề global scope

<div class="lesson-note" markdown="1">

#### Dùng cú pháp hiện đại trong ví dụ pre-ES6

Mặc dù `let`/`const` và arrow function chưa tồn tại trước ES6, chúng ta vẫn sẽ dùng chúng trong các ví dụ pre-ES6. Chúng không thay đổi cách hoạt động liên quan đến global scope và module pattern, vốn là trọng tâm chính của phần này.

</div>

Giả sử chúng ta có hai script, `one.js` và `two.js`, và link chúng trong HTML dưới dạng các script riêng biệt.

```html
<script src="one.js" defer></script>
<script src="two.js" defer></script>
```

```javascript
// one.js
const greeting = "Hello, Odinite!";
```

```javascript
// two.js
console.log(greeting);
```

Khi mở HTML, chúng ta thấy `"Hello, Odinite!"` được log vào console, dù `greeting` chưa từng được định nghĩa trong `two.js`! Đó là vì hai script được load lần lượt vào cùng một global scope, như thể chúng ta chỉ viết một file chứa hai dòng theo đúng thứ tự đó. Nếu đặt thẻ script của `two.js` lên trước, thay vào đó chúng ta sẽ nhận lỗi `greeting is not defined`, vì nó sẽ cố log vào console trước khi chúng ta định nghĩa biến.

Điều này có nghĩa là dù sử dụng nhiều file JavaScript, cuối cùng chúng vẫn chia sẻ cùng một global scope. Các biến ở top-level của chúng ta không an toàn!

Trước ESM, chúng ta có thể bọc một số thứ trong IIFE, khiến nó vẫn chạy theo cách tương tự, nhưng giờ mọi biến bên trong sẽ thuộc scope của function đó thay vì ở global.

```javascript
// one.js
(() => {
  const greeting = "Hello, Odinite!";
})();
```

Bây giờ, console báo lỗi `greeting is not defined`, vì không có biến global nào tên `greeting` để chúng ta log! Nhưng nếu chỉ muốn *một số* thứ được expose cho các file khác thì sao? Chúng ta có thể return những thứ đó từ IIFE vào global scope và giữ những thứ khác ở private!

```javascript
// one.js
const greeting = (() => {
  const greetingString = "Hello, Odinite!";
  const farewellString = "Bye bye, Odinite!";
  return greetingString;
})();
```

Bây giờ, biến global `greeting` sẽ chứa `"Hello, Odinite!"`, nên code từ `two.js` log thành công giá trị này vào console. Tuy nhiên, cả `greetingString` và `farewellString` đều là local với IIFE chứ không phải global. Điểm khác biệt là `greetingString` được return rõ ràng từ IIFE và gán cho `greeting`, khiến nó có thể được truy cập bên ngoài function, còn `farewellString` không được return nên hoàn toàn không thể truy cập trong `two.js`. Nhờ vậy, chúng ta có thể chọn những gì cần expose từ một file để cung cấp cho tất cả file đứng sau nó! Đây là lý do IIFE thường được gọi là "module pattern": chúng cho phép viết code dạng module qua nhiều file trước khi có "module thực sự".

Nhưng giờ đây, với ESM, chúng ta không còn cần dùng IIFE cho mục đích cụ thể này nữa.

### ES6 module

Với ESM, chúng ta có thêm một chút quyền kiểm soát. Mỗi file mặc định có scope riêng và private, đồng thời chúng ta không chỉ chọn được thứ gì export từ file đó mà còn chọn được thứ gì import vào các file khác. Vì vậy, chỉ vì đã export một thứ không có nghĩa là nó tự động khả dụng ở nơi khác; nó chỉ khả dụng trong một file khác nếu chúng ta import nó ở đó một cách rõ ràng. Rất nhiều quyền kiểm soát!

<div class="lesson-note" markdown="1">

#### Module scope không phải global scope

Khi dùng ESM, mỗi module có scope riêng và private, nơi chúng ta dùng import/export để giao tiếp giữa các file. Một biến top-level trong module sẽ không thể truy cập trong global scope.

</div>

### Import và export

Vậy thực sự import và export như thế nào? Đúng phong cách JavaScript, chúng ta không chỉ có một mà có hai loại import và export: `default` và `named`. Về cơ bản, chúng làm cùng một việc nhưng khác nhau một chút. Chúng thậm chí còn có thể được kết hợp trong cùng một file.

Trước tiên, chúng ta sẽ chỉ trình bày các cú pháp import/export khác nhau, nhưng code chưa chạy được vì bạn cũng cần link các script dưới dạng ES6 module. Chúng ta sẽ trình bày việc đó sau phần này.

#### Named export

Hãy dùng lại các ví dụ `one.js` và `two.js` ở trên. Trước tiên, chúng ta cần export các chuỗi greeting và farewell từ `one.js`.

Để export một thứ dưới dạng **named export**, chúng ta có thể đặt từ khóa `export` trước phần khai báo, hoặc thêm `export { }` ở đâu đó trong file (thường là cuối file), trong đó dấu ngoặc nhọn chứa danh sách tên của những thứ cần export. Cách nào cũng được, và chúng ta có thể export bao nhiêu thứ tùy ý dưới dạng named export.

```javascript
// one.js
export const greeting = "Hello, Odinite!";
export const farewell = "Bye bye, Odinite!";
```

Hoặc ở một dòng riêng:

```javascript
// one.js
const greeting = "Hello, Odinite!";
const farewell = "Bye bye, Odinite!";
export { greeting, farewell };
```

Bây giờ hãy import các biến này trong `two.js`! Hãy nhớ rằng chúng ta có thể kiểm soát thứ mình import, nên nếu chỉ cần biến `greeting`, chúng ta có thể chỉ import riêng nó. Nếu một file khác cần biến `farewell` (hoặc cả hai), file đó có thể import những gì nó cần. Không import thì không dùng được. Để thực hiện named import, chúng ta phải chỉ rõ tên những thứ muốn import bên trong `{ }` và cung cấp path tới file được import (khi xử lý library bên thứ ba sau này, bạn có thể chỉ dùng tên library thay vì full file path).

Lưu ý rằng [bạn không thể dùng template string cho file path](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#module-name), chỉ có thể dùng string đặt trong dấu nháy đơn hoặc nháy kép.

```javascript
// two.js
import { greeting, farewell } from "./one.js";

console.log(greeting); // "Hello, Odinite!"
console.log(farewell); // "Bye bye, Odinite!"
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Named import/export không giống object literal!

Dùng `{ }` với named import/export là cú pháp đặc biệt và hoàn toàn không liên quan đến việc khai báo object literal hay [destructuring object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#object_destructuring).

```javascript
export { greeting, farewell };

import { greeting, farewell } from "./one.js";
```

Trong ví dụ trên, chúng ta không export một object chứa các key `greeting` và `farewell`, cũng không destructure một object có các key đó khi import. Chúng ta chỉ đang dùng cú pháp named import/export.

</div>

#### Default export

Trái với named export, một file chỉ có thể default export một thứ duy nhất. Thứ được export theo cách này không có tên gắn kèm, nên khi import ở đâu đó, bạn có thể quyết định tên cho nó.

Để export một thứ từ file dưới dạng default export, chúng ta cũng có thể làm inline bằng cách thêm `export default` trước phần khai báo phù hợp, hoặc export ở cuối file, lần này *không có* dấu ngoặc nhọn. Một lần nữa, cách nào cũng hoàn toàn được. Lưu ý rằng nếu muốn inline default export một biến, từ khóa `default` sẽ *thay thế* phần khai báo biến, nên bạn export trực tiếp expression.

Hãy default export chuỗi greeting từ `one.js`.

```javascript
// one.js
export default "Hello, Odinite!";
```

Hoặc ở một dòng riêng:

```javascript
// one.js
const greeting = "Hello, Odinite!";
export default greeting;
```

Bây giờ trong `two.js`, chúng ta có thể default import chuỗi đó. Hãy nhớ rằng vì đang import thứ được default export, chúng ta có thể đặt tên tùy ý. Mặc dù biến trong `one.js` được gọi là `greeting`, chúng ta không nhất thiết phải gọi nó như vậy trong `two.js`. Khi default import, chúng ta không dùng dấu ngoặc nhọn vì chúng dành cho named import.

```javascript
// two.js
import helloOdinite from "./one.js";

console.log(helloOdinite); // "Hello, Odinite!"
```

Bạn có thể dùng cả default export và named export trong cùng một file. Khá khó hiểu là không có quy tắc được thống nhất rộng rãi về lúc nào nên dùng loại nào, ngoài việc một file có thể có nhiều named export nhưng chỉ có một default export. Khi chỉ cần export một thứ duy nhất từ module, có người thích default export, trong khi người khác thích một named export duy nhất. Cả hai đều hoạt động, nên hãy dùng cách bạn thích, hoặc nếu làm việc trong team thì dùng cách team ưa chuộng.

Hãy default export chuỗi greeting từ `one.js` và export chuỗi farewell dưới dạng named export.

```javascript
// one.js
export default "Hello, Odinite!";
export const farewell = "Bye bye, Odinite!";
```

Sau đó chúng ta có thể import cả hai trong `two.js`. Chúng ta cần default import chuỗi greeting (điều này cũng có nghĩa là có thể đặt tên tùy ý) và named import chuỗi farewell.

```javascript
// two.js
import greeting, { farewell } from "./one.js";

console.log(greeting); // "Hello, Odinite!"
console.log(farewell); // "Bye bye, Odinite!"
```

### Entry point

Khi dùng ESM, thay vì thêm lần lượt mọi file JavaScript vào HTML, chúng ta chỉ cần link một file duy nhất - **entry point**.

```html
<script src="two.js" type="module"></script>
```

Vì sao `two.js` là entry point? Trong các ví dụ trên, `two.js` import biến từ `one.js`, nghĩa là `two.js` phụ thuộc vào `one.js`, nên chúng ta có **dependency graph** sau:

```text
importer  depends on  exporter
two.js <-------------- one.js
```

Khi load `two.js` dưới dạng module, trình duyệt sẽ thấy nó phụ thuộc vào `one.js` và cũng load code từ file đó. Nếu dùng `one.js` làm entry point, trình duyệt sẽ thấy nó không phụ thuộc file nào khác nên sẽ không làm gì thêm. Code từ `two.js` sẽ không được dùng và không có gì được log!

Nếu có thêm file `three.js` export một thứ và `two.js` import từ đó, `two.js` vẫn là entry point, lúc này phụ thuộc vào cả `one.js` và `three.js`.

```text
two.js <-------------- one.js
              └------- three.js
```

Hoặc có thể thay vì `two.js`, `one.js` import từ `three.js`. Khi đó `two.js` vẫn là entry point và phụ thuộc gián tiếp vào `three.js` thông qua `one.js`.

```text
two.js <-------------- one.js <-------------- three.js
```

Lưu ý rằng chúng ta chỉ cần một thẻ script, vì trình duyệt sẽ tự xử lý các dependency bổ sung của file. Chúng ta cũng không cần thêm thuộc tính `defer`, vì `type="module"` sẽ tự động defer việc thực thi script.

Nếu bạn đã làm theo ví dụ IIFE ở đầu bài, hãy thử viết lại JavaScript để dùng `import` và `export`, rồi chỉ link entry point dưới dạng module script. Vì lý do bảo mật của trình duyệt, ES6 module không thể được load nếu bạn mở trực tiếp file HTML trong trình duyệt, nên hãy đảm bảo dùng extension [Live Preview của Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server) nếu bạn chưa dùng.

### CommonJS

Trong quá trình học, có thể bạn đã gặp CommonJS (CJS), hệ thống dùng cú pháp như `require` và `module.exports` thay cho `import` và `export`. Bạn có thể nhớ đã thấy nó trong các bài tập JavaScript ở khóa Foundations (bạn đã đi được một chặng đường dài)! Đây là một module system được thiết kế để dùng với Node.js, hoạt động hơi khác ESM và không phải thứ mà trình duyệt có thể hiểu.

CJS vẫn được sử dụng khá nhiều trong code Node.js, dù những năm gần đây ESM trong Node.js đang ngày càng phổ biến. Hiện tại, chúng ta tập trung viết code chạy trong trình duyệt nên sẽ dành thời gian cho ESM. Nếu đang theo lộ trình Full Stack JavaScript, sau này chúng ta sẽ học CJS chi tiết hơn trong khóa Node.js.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Như thường lệ, bạn có thể tìm hiểu hầu hết keyword và khái niệm JavaScript qua tài liệu MDN, nên hãy xem [tài liệu về export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) và [tài liệu về import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import). Có một số phần mở rộng nhỏ về chúng mà chúng ta chưa đề cập trong bài này, chẳng hạn alias và namespace import.

</div>