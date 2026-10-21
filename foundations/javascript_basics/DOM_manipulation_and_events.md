### Giới thiệu

Một trong những khả năng độc đáo và hữu ích nhất của JavaScript là thao tác với DOM. Nhưng DOM *là gì*, và chúng ta thay đổi nó bằng cách nào? Hãy bắt đầu ngay...

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích DOM là gì trong mối liên hệ với một trang web.
- Giải thích sự khác nhau giữa “node” và “element”.
- Giải thích cách nhắm đến các node bằng “selector”.
- Giải thích các phương thức cơ bản để tìm, thêm, xóa và thay đổi node DOM.
- Giải thích sự khác nhau giữa “NodeList” và “mảng các node”.
- Giải thích “bubbling” là gì và nó hoạt động ra sao.

### Mô hình đối tượng tài liệu

DOM (hay Document Object Model) là một biểu diễn dạng cây của nội dung trang web — một cây gồm các “node” với những mối quan hệ khác nhau tùy theo cách chúng được sắp xếp trong tài liệu HTML. Có nhiều loại node, phần lớn không được dùng thường xuyên. Trong bài học này, chúng ta sẽ tập trung vào các node “element”, vốn chủ yếu được dùng để thao tác với DOM.

```html
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
```

Trong ví dụ trên, `<div class="display"></div>` là một “child” của `<div id="container"></div>` và là “sibling” của `<div class="controls"></div>`. Hãy hình dung nó giống như một cây gia phả. `<div id="container"></div>` là **parent**, với các **child** ở cấp tiếp theo, mỗi child nằm trên một “nhánh” riêng.

### Nhắm đến node bằng selector

Khi làm việc với DOM, bạn dùng “selector” để nhắm đến những node muốn thao tác. Bạn có thể kết hợp các selector theo phong cách CSS với các thuộc tính quan hệ để nhắm đến node cần tìm. Hãy bắt đầu với selector theo phong cách CSS. Trong ví dụ trên, bạn có thể dùng các selector sau để tham chiếu đến `<div class="display"></div>`:

- `div.display`
- `.display`
- `#container > .display`
- `div#container > div.display`

Bạn cũng có thể dùng các selector quan hệ (ví dụ `firstElementChild` hoặc `lastElementChild`, v.v.) cùng với những thuộc tính đặc biệt mà node sở hữu.

```javascript
// selects the #container div (don't worry about the syntax, we'll get there)
const container = document.querySelector("#container");

// selects the first child of #container => .display
const display = container.firstElementChild;
console.log(display);  // <div class="display"></div>
```

```javascript
// selects the .controls div
const controls = document.querySelector(".controls");

// selects the prior sibling => .display
const display = controls.previousElementSibling;
console.log(display); // <div class="display"></div>
```

Như vậy, bạn đang xác định một node cụ thể dựa trên mối quan hệ của nó với các node xung quanh.

<div class="lesson-note" markdown="1">

#### Đối tượng JavaScript

Có thể bạn đang tự hỏi tại sao lại có ký hiệu dấu chấm (`.`) sau các biến của chúng ta. Ví dụ: `container.firstElementChild`.

Đây là cách chúng ta truy cập các đối tượng trong JavaScript. Một đối tượng trong JavaScript là kiểu dữ liệu chứa các thuộc tính. Một thuộc tính là một cặp “key: value”. Chẳng hạn, `container` có nhiều thuộc tính, và chúng ta có thể truy cập phần tử con đầu tiên bằng cách truy cập key `firstElementChild` để lấy giá trị của nó. Trong trường hợp này, giá trị là một phần tử `<div>` có class `display`.

Hiện tại, đó là tất cả những gì bạn cần biết về đối tượng JavaScript. Hãy chờ đến bài Objects Basics để tìm hiểu thêm!

</div>

### Các phương thức DOM

Khi trình duyệt web phân tích mã HTML, nó chuyển mã thành DOM như đã đề cập ở trên. Một trong những điểm khác biệt chính là các node này là những đối tượng JavaScript có nhiều thuộc tính và phương thức gắn với chúng. Những thuộc tính và phương thức này là các công cụ chính chúng ta sẽ dùng để thao tác với trang web bằng JavaScript.

#### Query selector

- `element.querySelector(selector)` - trả về một tham chiếu đến kết quả khớp đầu tiên của *selector*.
- `element.querySelectorAll(selectors)` - trả về một “NodeList” chứa các tham chiếu đến mọi kết quả khớp của *selectors*.

<div class="lesson-note" markdown="1">

#### Lưu ý về hiệu năng

Có một số truy vấn khác, cụ thể hơn, có thể đem lại lợi ích hiệu năng (dù không đáng kể), nhưng hiện tại chúng ta sẽ không tìm hiểu chúng.

</div>

Điều quan trọng cần nhớ là khi dùng querySelectorAll, giá trị trả về **không phải** là một mảng. Nó trông giống mảng và hoạt động phần nào như mảng, nhưng thực sự là một “NodeList”. Điểm khác biệt lớn là NodeList thiếu một số phương thức mảng. Nếu gặp vấn đề, một cách giải quyết là chuyển NodeList thành mảng. Bạn có thể làm điều đó bằng `Array.from()` hoặc [spread operator.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)

#### Tạo element

- `document.createElement(tagName, [options])` - tạo một element mới thuộc loại tagName. `[options]` ở đây có nghĩa là bạn có thể thêm một số tham số tùy chọn vào hàm. Hiện tại chưa cần lo về chúng.

```javascript
const div = document.createElement("div");
```

Hàm này KHÔNG đưa element mới vào DOM — nó tạo element trong bộ nhớ. Nhờ vậy, bạn có thể thao tác với element (thêm style, class, id, text, v.v.) trước khi đặt nó lên trang. Bạn có thể đưa element vào DOM bằng một trong các phương thức sau.

#### Thêm element

- `parentNode.appendChild(childNode)` - thêm *childNode* vào cuối danh sách con của *parentNode*.
- `parentNode.insertBefore(newNode, referenceNode)` - chèn *newNode* vào *parentNode* trước *referenceNode*.

#### Xóa element

- `parentNode.removeChild(child)` - xóa *child* khỏi DOM của *parentNode* và trả về một tham chiếu đến *child*.

#### Thay đổi element

Khi có tham chiếu đến một element, bạn có thể dùng tham chiếu đó để thay đổi các thuộc tính của chính element. Điều này cho phép thực hiện nhiều thay đổi hữu ích, như thêm, xóa hoặc thay đổi thuộc tính, thay đổi class, thêm thông tin style nội tuyến, v.v.

```javascript
// creates a new div referenced in the variable 'div'
const div = document.createElement("div");
```

#### Thêm style nội tuyến

```javascript
// sets the indicated style rule to the element in the div variable
div.style.color = "blue";

// set the entire inline style
div.setAttribute("style", "color: blue; background: white;");
```

Khi truy cập một thuộc tính CSS dạng kebab-case như `background-color` bằng JS, bạn cần dùng camelCase với ký hiệu dấu chấm hoặc dùng ký hiệu ngoặc vuông. Khi dùng ký hiệu ngoặc vuông, bạn có thể dùng camelCase hoặc kebab-case, nhưng tên thuộc tính phải là một chuỗi.

```javascript
// dot notation with kebab case: doesn't work as it attempts to subtract color from div.style.background
// equivalent to: div.style.background - color
div.style.background-color;

// dot notation with camelCase: works, accesses the div's background-color style
div.style.backgroundColor;

// bracket notation with kebab-case: also works
div.style["background-color"];

// bracket notation with camelCase: also works
div.style["backgroundColor"];
```

#### Chỉnh sửa thuộc tính

```javascript
// if id exists, update it to 'theDiv', else create an id with value "theDiv"
div.setAttribute("id", "theDiv");

// returns value of specified attribute, in this case "theDiv"
div.getAttribute("id");

// removes specified attribute
div.removeAttribute("id");
```

Xem phần [HTML Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes) của MDN để biết thêm thông tin về các thuộc tính hiện có.

#### Làm việc với class

```javascript
// adds class "new" to your new div
div.classList.add("new");

// removes "new" class from div
div.classList.remove("new");

// if div doesn't have class "active" then add it, or if it does, then remove it
div.classList.toggle("active");
```

Thông thường, cách làm chuẩn (và sạch hơn) là bật/tắt một style CSS thay vì thêm rồi xóa CSS nội tuyến.

#### Thêm nội dung văn bản

```javascript
// creates a text node containing "Hello World!" and inserts it in div
div.textContent = "Hello World!";
```

#### Thêm nội dung HTML

```javascript
// renders the HTML inside div
div.innerHTML = "<span>Hello World!</span>";
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Rủi ro bảo mật của innerHTML

Nên ưu tiên textContent thay cho innerHTML khi thêm văn bản, vì innerHTML chỉ nên được dùng hạn chế để tránh các rủi ro bảo mật tiềm ẩn.
Để hiểu nguy hiểm khi dùng innerHTML, hãy xem [video về cách ngăn chặn cuộc tấn công cross-site scripting phổ biến nhất này](https://youtube.com/watch?v=ns1LX6mEvyM).

</div>

Hãy dành một phút ôn lại những gì đã học và thực hành trước khi tiếp tục. Xem ví dụ sau về cách tạo và thêm một element DOM vào trang web.

```html
<!-- your HTML file: -->
<body>
  <h1>THE TITLE OF YOUR WEBPAGE</h1>
  <div id="container"></div>
</body>
```

```javascript
// your JavaScript file
const container = document.querySelector("#container");

const content = document.createElement("div");
content.classList.add("content");
content.textContent = "This is the glorious text-content!";

container.appendChild(content);
```

Trong tệp JavaScript, trước hết chúng ta lấy tham chiếu đến element `container` đã tồn tại trong HTML. Sau đó, chúng ta tạo một div mới và lưu nó vào biến `content`. Chúng ta thêm một class và một đoạn văn bản vào div `content`, rồi cuối cùng thêm div đó vào `container`. Sau khi mã JavaScript chạy, cây DOM sẽ trông như sau:

```html
<!-- The DOM -->
<body>
  <h1>THE TITLE OF YOUR WEBPAGE</h1>
  <div id="container">
    <div class="content">This is the glorious text-content!</div>
  </div>
</body>
```

Hãy nhớ rằng JavaScript *không* thay đổi HTML của bạn mà thay đổi DOM — tệp HTML vẫn trông như cũ, nhưng JavaScript làm thay đổi những gì trình duyệt hiển thị.

<div id="important-note" class="lesson-note" markdown="1">

#### Thời điểm chạy JavaScript

Phần lớn thời gian, JavaScript được chạy khi tệp JS được chạy hoặc khi trình duyệt gặp thẻ script trong HTML. Nếu đưa JavaScript lên đầu tệp, nhiều phương thức thao tác DOM sẽ không hoạt động vì mã JS được chạy *trước* khi các node được tạo trong DOM. Cách đơn giản nhất để khắc phục là đưa JavaScript xuống cuối tệp HTML để nó chạy sau khi các node DOM được phân tích và tạo ra.

Ngoài ra, bạn có thể liên kết tệp JavaScript trong `<head>` của tài liệu HTML. Dùng thẻ `<script>` với thuộc tính `src` chứa đường dẫn đến tệp JS, đồng thời thêm từ khóa `defer` để tệp được tải *sau* khi HTML được phân tích, như sau:

```html
<head>
  <script src="js-file.js" defer></script>
</head>
```

Tìm hiểu thêm về [thuộc tính `defer` của thẻ script](https://javascript.info/script-async-defer#defer).

</div>

### Bài tập

Sao chép ví dụ trên vào các tệp trên máy tính của bạn. Để nó hoạt động, bạn cần cung cấp phần còn lại của khung HTML và liên kết tệp JavaScript hoặc đặt JavaScript vào một thẻ script trên trang. Hãy đảm bảo mọi thứ hoạt động trước khi tiếp tục!

Chỉ dùng JavaScript và các phương thức DOM đã trình bày ở trên để thêm các element sau vào container:

1. một `<p>` có văn bản màu đỏ, nội dung là "Hey I'm red!"
1. một `<h3>` có văn bản màu xanh dương, nội dung là "I'm a blue h3!"
1. một `<div>` có viền đen và màu nền hồng, bên trong chứa các element sau:
   - một `<h1>` khác có nội dung "I'm in a div"
   - một `<p>` có nội dung "ME TOO!"
   - Gợi ý: sau khi tạo `<div>` bằng createElement, hãy thêm `<h1>` và `<p>` vào đó trước khi thêm nó vào container.

### Event

Bây giờ bạn đã nắm được cách thao tác DOM bằng JavaScript, bước tiếp theo là học cách thực hiện việc đó một cách động hoặc theo yêu cầu! Event là cách bạn tạo ra điều kỳ diệu ấy trên các trang của mình. Event là những hành động xảy ra trên trang web, chẳng hạn nhấp chuột hoặc nhấn phím. Bằng JavaScript, chúng ta có thể khiến trang web lắng nghe và phản ứng với những event này.

Có ba cách chính để thực hiện việc này:

- Bạn có thể chỉ định trực tiếp các thuộc tính hàm trên element HTML.
- Bạn có thể đặt các thuộc tính theo dạng `on<eventType>`, chẳng hạn `onclick` hoặc `onmousedown`, trên các node DOM bằng JavaScript.
- Bạn có thể gắn event listener vào các node DOM bằng JavaScript.

Event listener chắc chắn là phương pháp được ưu tiên, nhưng bạn sẽ thường xuyên gặp hai cách còn lại, nên chúng ta sẽ tìm hiểu cả ba.

Chúng ta sẽ tạo ba nút, và cả ba đều hiển thị cảnh báo "Hello World" khi được nhấp. Hãy thử cả ba cách bằng tệp HTML của riêng bạn hoặc dùng công cụ như [CodePen](https://codepen.io/).

#### Phương pháp 1

```html
<button onclick="alert('Hello World')">Click Me</button>
```

Giải pháp này chưa lý tưởng vì chúng ta làm HTML lộn xộn bằng JavaScript. Ngoài ra, mỗi element DOM chỉ có thể đặt một thuộc tính "onclick", nên với cách này chúng ta không thể chạy nhiều hàm riêng biệt để phản hồi cùng một event nhấp chuột.

#### Phương pháp 2

```html
<!-- the HTML file -->
<button id="btn">Click Me</button>
```

```javascript
// the JavaScript file
const btn = document.querySelector("#btn");
btn.onclick = () => alert("Hello World");
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Ôn lại cú pháp hàm mũi tên

Nếu cần ôn lại cú pháp mũi tên `() =>`, hãy xem [bài viết về hàm mũi tên](http://javascript.info/arrow-functions-basics) này.

</div>

Cách này tốt hơn một chút. Chúng ta đã chuyển JS ra khỏi HTML và đưa vào tệp JS, nhưng vẫn còn vấn đề là một element DOM chỉ có thể có một thuộc tính "onclick".

#### Phương pháp 3

```html
<!-- the HTML file -->
<button id="btn">Click Me Too</button>
```

```javascript
// the JavaScript file
const btn = document.querySelector("#btn");
btn.addEventListener("click", () => {
  alert("Hello World");
});
```

Giờ đây, chúng ta duy trì sự tách biệt các mối quan tâm và cũng cho phép có nhiều event listener nếu cần. Phương pháp 3 linh hoạt và mạnh mẽ hơn nhiều, dù thiết lập nó phức tạp hơn đôi chút.

Lưu ý rằng cả ba phương pháp đều có thể dùng với các hàm được đặt tên như sau:

```html
<!-- the HTML file -->
<!-- METHOD 1 -->
<button onclick="alertFunction()">CLICK ME BABY</button>
```

```javascript
// the JavaScript file
// METHOD 1
function alertFunction() {
  alert("YAY! YOU DID IT!");
}
```

```html
<!-- the HTML file -->
<!-- METHODS 2 & 3 -->
<button id="btn">CLICK ME BABY</button>
```

```javascript
// the JavaScript file
// METHODS 2 & 3
function alertFunction() {
  alert("YAY! YOU DID IT!");
}
const btn = document.querySelector("#btn");

// METHOD 2
btn.onclick = alertFunction;

// METHOD 3
btn.addEventListener("click", alertFunction);
```

Dùng các hàm được đặt tên có thể làm mã của bạn gọn gàng hơn đáng kể, và đây là một ý tưởng *thực sự* tốt nếu hàm đó là thứ bạn muốn thực hiện ở nhiều nơi.

Với cả ba phương pháp, chúng ta có thể truy cập thêm thông tin về event bằng cách truyền một tham số vào hàm đang gọi. Hãy thử trên máy của bạn:

```javascript
btn.addEventListener("click", function (e) {
  console.log(e);
});
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Tìm hiểu callback

Khi truyền `alertFunction` hoặc `function (e) {...}` làm đối số cho `addEventListener`, chúng ta gọi đó là một `callback`. Callback đơn giản là một hàm được truyền vào một hàm khác dưới dạng đối số.

</div>

Tham số `e` trong hàm callback đó chứa một đối tượng tham chiếu đến chính **event**. Bên trong đối tượng ấy, bạn có thể truy cập nhiều thuộc tính và phương thức hữu ích (các hàm nằm bên trong một đối tượng), chẳng hạn nút chuột hoặc phím nào đã được nhấn, hay thông tin về **target** của event — node DOM đã được nhấp. Không có gì thần kỳ về tên `e` hay nguồn gốc của nó. JavaScript biết đây là tham số event vì theo định nghĩa, callback của event listener nhận một đối tượng `Event`. Khi callback chạy, event handler truyền vào tham chiếu của chính event đó. Bạn có thể đọc thêm về các đối tượng event trong [phần giới thiệu event của MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events#event_objects).

Hãy thử:

```javascript
btn.addEventListener("click", function (e) {
  console.log(e.target);
});
```

và bây giờ:

```javascript
btn.addEventListener("click", function (e) {
  e.target.style.background = "blue";
});
```

Khá thú vị, đúng không?

#### Gắn listener cho một nhóm node

Nếu phải gắn nhiều event listener tương tự cho nhiều element, bạn có thể thấy đây là khá nhiều mã. Có vài cách thực hiện việc đó hiệu quả hơn. Ở trên, chúng ta đã học rằng có thể lấy một NodeList gồm tất cả item khớp với selector cụ thể bằng `querySelectorAll('selector')`. Để thêm listener vào từng item, chúng ta cần lặp qua toàn bộ danh sách, như sau:

```html
<div id="container">
  <button id="one">Click Me</button>
  <button id="two">Click Me</button>
  <button id="three">Click Me</button>
</div>
```

```javascript
// buttons is a node list. It looks and acts much like an array.
const buttons = document.querySelectorAll("button");

// we use the .forEach method to iterate through each button
buttons.forEach((button) => {
  // and for each one we add a 'click' listener
  button.addEventListener("click", () => {
    alert(button.id);
  });
});
```

Đây mới chỉ là phần nổi của tảng băng khi nói về thao tác DOM và xử lý event, nhưng chừng đó đủ để bạn bắt đầu làm bài tập. Trong các ví dụ cho đến giờ, chúng ta chỉ dùng event 'click', nhưng còn *rất nhiều* event khác để bạn sử dụng.

Một số event hữu ích gồm:

- click
- dblclick
- keydown
- keyup

Bạn có thể tìm danh sách đầy đủ hơn cùng phần giải thích từng event trong [trang tham khảo Event JavaScript của W3Schools](https://www.w3schools.com/jsref/dom_obj_event.asp).

### Bài tập

<div class="lesson-content__panel" markdown="1">

Thao tác với trang web là lợi ích chính của ngôn ngữ JavaScript! Đây là những kỹ thuật bạn có thể sẽ dùng *mỗi ngày* với tư cách một lập trình viên front-end, vậy hãy cùng thực hành!

1. Đọc các phần sau trong loạt bài về DOM của JavaScript Tutorial để hiểu rộng hơn cách event có thể được dùng trên trang của bạn.

   Khi đọc, hãy nhớ rằng các ý tưởng tổng quát có thể áp dụng cho mọi event, không chỉ những event trong ví dụ — còn thông tin cụ thể về từng loại event luôn có thể tìm thấy trong tài liệu. Mục tiêu ở đây không phải là hiểu thật sâu các khái niệm và lập tức áp dụng được chúng. Thay vào đó, bạn nên xây dựng nhận thức về những công cụ liên quan đến event mà mình có thể sử dụng.

   - [Các event JavaScript](https://www.javascripttutorial.net/javascript-dom/javascript-events/)
   - [Các event chuột](https://www.javascripttutorial.net/javascript-dom/javascript-mouse-events/)
   - [Các event bàn phím](https://www.javascripttutorial.net/javascript-dom/javascript-keyboard-events/)
   - [Ủy quyền event](https://www.javascripttutorial.net/javascript-dom/javascript-event-delegation/)
   - [Phương thức dispatchEvent](https://www.javascripttutorial.net/javascript-dom/javascript-dispatchevent/)
   - [Event tùy chỉnh](https://www.javascripttutorial.net/javascript-dom/javascript-custom-events/)

1. Đọc [Tìm hiểu callback trong JavaScript](https://dev.to/i3uckwheat/understanding-callbacks-2o9e) để hiểu sâu hơn về callback.

1. Hoàn thành các phần sau của MDN để kiểm tra kỹ năng:
   - [Thực hiện một số thao tác DOM cơ bản](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting#doing_some_basic_dom_manipulation)
   - [Tạo danh sách mua sắm động](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting#creating_a_dynamic_shopping_list)

</div>