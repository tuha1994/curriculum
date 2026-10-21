### Giới thiệu

Chúc mừng bạn đã đi đến một trong những bài học cuối của phần Foundations! Đến đây, bạn đã học được nhiều nền tảng của JavaScript. Trong bài học này, bạn sẽ tìm hiểu về Đối tượng - một tập hợp các cặp khóa-giá trị - cùng với một số phương thức mảng mạnh mẽ và thường được sử dụng hơn.

Chẳng bao lâu nữa, bạn sẽ hiểu rõ hơn về sức mạnh của đối tượng và mảng, cũng như lý do cả hai có thể trở thành những phần không thể thiếu trong bộ công cụ JavaScript của bạn!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Tạo đối tượng.
- Truy cập các thuộc tính của đối tượng.
- Sử dụng nhiều toán tử đối tượng.
- Hiểu sự khác nhau giữa kiểu dữ liệu đối tượng và kiểu dữ liệu nguyên thủy.

### Đối tượng

Đối tượng là một phần *rất* quan trọng của ngôn ngữ JavaScript. Phần lớn thời gian, bạn có thể hoàn thành những tác vụ đơn giản, thậm chí trung cấp, mà chưa cần bận tâm nhiều đến chúng; tuy nhiên, bất kỳ dự án thực tế nào bạn thực hiện cũng sẽ sử dụng Đối tượng. Cách sử dụng Đối tượng trong JavaScript có thể nhanh chóng trở nên khá sâu, nên hiện tại chúng ta chỉ tập trung vào những điều cơ bản. Sau này sẽ có một bài tìm hiểu chuyên sâu hơn.

1. Bài viết [về đối tượng](https://javascript.info/object) trên JavaScript.info là nơi tốt nhất để bắt đầu. Bạn không cần làm các bài tập ở cuối trang.
1. Học theo bài ["Kiến thức cơ bản về đối tượng JavaScript" của MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics). Bỏ qua phần "Giới thiệu về constructor", vì đó là chủ đề nâng cao hơn mà chúng ta sẽ học sau trong chương trình.

### Sự khác nhau giữa đối tượng và kiểu dữ liệu nguyên thủy

Trước đó trong chương trình, bạn đã học về [các kiểu dữ liệu nguyên thủy](https://www.theodinproject.com/lessons/foundations-data-types-and-conditionals). Giờ đây, bạn cũng đã thấy kiểu dữ liệu đối tượng, bao gồm nhưng không chỉ giới hạn ở đối tượng ({key: value}), mảng và hàm. Điểm khác biệt chính giữa hai nhóm là kiểu nguyên thủy chỉ có thể chứa một giá trị duy nhất (chuỗi, số, v.v.). Kiểu dữ liệu đối tượng được dùng để lưu trữ một tập hợp dữ liệu và những thực thể phức tạp hơn.

Bên cạnh những khác biệt về mặt khái niệm, còn có một số khác biệt kỹ thuật ảnh hưởng đến cách chúng ta sử dụng từng kiểu dữ liệu. Khi bạn định nghĩa một biến kiểu nguyên thủy, biến đó sẽ chứa một bản sao của thông tin được gán cho nó:

```javascript
let data = 42;
// dataCopy will store a copy of what data contains, so a copy of 42
let dataCopy = data;

// which means that making changes to dataCopy won't affect data
dataCopy = 43;

console.log(data); // 42
console.log(dataCopy); // 43
```

Ngược lại, khi bạn định nghĩa một biến đối tượng, biến đó sẽ chứa một *tham chiếu* đến đối tượng được gán cho nó:

```javascript
// obj contains a reference to the object we defined on the right side
const obj = { data: 42 };
// objCopy will contain a reference to the object referenced by obj
const objCopy = obj;

// making changes to objCopy will make changes to the object that it refers to
objCopy.data = 43;

console.log(obj); // { data: 43 }
console.log(objCopy); // { data: 43 }
```

Hành vi này không xa lạ với bạn. Trong dự án gần đây, bạn đã thay đổi các ô trong lưới Etch-A-Sketch bằng cách sử dụng tham chiếu. Hãy lấy đoạn mã sau làm ví dụ:

```javascript
const element = document.querySelector("#container");
element.style.backgroundColor = "red";
```

Chúng ta đang thay đổi biến đã khai báo (`element`), nhưng những thay đổi đó lại tác động đến nút tương ứng trong DOM. Vì sao lại như vậy? Nút mà chúng ta có trong mã là **một tham chiếu** đến chính nút mà DOM đang sử dụng. Nếu `element` không phải là tham chiếu mà là một bản sao của chính nút đó (giống cách kiểu dữ liệu nguyên thủy hoạt động), việc thay đổi nó sẽ **không** có tác dụng, vì các thay đổi chỉ ảnh hưởng đến bản sao chứ không ảnh hưởng đến bản gốc.

Đây cũng là điều cần cân nhắc khi chúng ta truyền đối số cho một hàm. Hãy lấy các hàm sau làm ví dụ:

```javascript
function increaseCounterObject(objectCounter) {
  objectCounter.counter += 1;
}

function increaseCounterPrimitive(primitiveCounter) {
  primitiveCounter += 1;
}

const object = { counter: 0 };
let primitive = 0;

increaseCounterObject(object);
increaseCounterPrimitive(primitive);
```

Hãy dừng lại một chút và đoán xem điều gì sẽ xảy ra với `object` và `primitive` sau khi chúng ta gọi các hàm trên.

Nếu bạn trả lời rằng bộ đếm của đối tượng sẽ tăng thêm 1, còn bộ đếm nguyên thủy sẽ không thay đổi, thì bạn đã trả lời đúng. Hãy nhớ rằng tham số `objectCounter` chứa một *tham chiếu* đến cùng đối tượng với biến `object` được truyền vào, trong khi `primitiveCounter` chỉ chứa một bản sao của giá trị nguyên thủy.

<div class="lesson-note" markdown="1">

#### Gán lại các biến kiểu dữ liệu đối tượng

Khi chúng ta thay đổi một đối tượng được nhiều biến tham chiếu đến, thay đổi đó sẽ được nhìn thấy thông qua tất cả các biến ấy, vì chúng đều tham chiếu đến cùng một đối tượng. Tuy nhiên, việc gán một trong các biến đó sang một đối tượng mới sẽ không đồng thời gán lại các biến khác. Ví dụ:

```javascript
let animal = { species: "dog" };
let dog = animal;

// reassigning animal variable with a completely new object
animal = { species: "cat" };

console.log(animal); // { species: "cat" }
console.log(dog); // { species: "dog" }
```

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bây giờ bạn đã làm quen với đối tượng, hãy quay lại [các bài tập về phương thức mảng](https://javascript.info/array-methods#tasks) ở cuối bài viết về phương thức mảng trên JavaScript.info mà bạn đã học cách đây vài bài. Chỉ làm các bài sau:
   - `Map to names`
   - `Map to objects`
   - `Sort users by age`
   - `Get average age`
   - `Create keyed object from array`
1. Luyện tập thêm với đối tượng (bên trong mảng!). Fork và clone [repository JavaScript30](https://github.com/wesbos/JavaScript30) của Wes Bos. Để làm theo hai bài tập video này, bạn nên mở tệp `index-START.html`.
    - [04 - Cardio với mảng, ngày 1](https://www.youtube.com/watch?v=HB1ZC7czKRs)
    - [07 - Cardio với mảng, ngày 2](https://www.youtube.com/watch?v=QNmRfyNg1lw)
1. Truy cập [thư mục `foundations/object_basics` của repository bài tập JavaScript](https://github.com/TheOdinProject/javascript-exercises/tree/main/foundations/object_basics). Đọc từng tệp README trước khi hoàn thành các bài tập sau theo đúng thứ tự (nhắc lại rằng bạn luôn có thể xem [README của chính repository](https://github.com/TheOdinProject/javascript-exercises#how-to-use-these-exercises "JavaScript exercises repo README") để biết hướng dẫn chung):
   - `01_calculator`
   - `02_palindromes`
   - `03_fibonacci`
   - `04_getTheTitles`
   - `05_findTheOldest`

Lưu ý: Bạn có thể tìm thấy lời giải cho các bài tập này trong thư mục `solution` của từng bài.

Nếu cảm thấy quá tải hoặc bị mắc kẹt, đừng ngại quay lại ôn tập hoặc nhờ trợ giúp trong [máy chủ Discord của TOP](https://discord.gg/fbFCkYabZB)!

</div>