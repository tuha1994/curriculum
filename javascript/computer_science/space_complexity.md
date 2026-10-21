### Giới thiệu

Trong bài học trước, chúng ta tập trung đo complexity từ góc nhìn thời gian. Ta đã tìm hiểu nhiều cách đo complexity của algorithm và tại sao Big O được ưu tiên. Ta cũng xem một số ví dụ về việc áp dụng điều này để đo time complexity của algorithm.

Trong bài học này, chúng ta tập trung vào space complexity và xem cách dùng những notation đã học để đo việc thay đổi input ảnh hưởng thế nào đến lượng bộ nhớ algorithm sử dụng.

Khi nói về bộ nhớ, ta nói đến primary memory, tức working memory hệ thống có để thực thi algorithm. Bạn có thể đọc thêm trong [GeeksforGeeks Primary Memory article](https://www.geeksforgeeks.org/primary-memory/).

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài học này.

- Space complexity là gì.
- Tầm quan trọng của space complexity.
- Cách đo space complexity.
- Phân tích auxiliary space.

### Space complexity nghĩa là gì?

Space complexity có thể được xem là tổng không gian một algorithm sử dụng tương đối với kích thước input. Nhớ lại bài trước, bạn sẽ thấy ta không xem efficiency của algorithm dựa trên cách nó chạy trong một instance cụ thể. Thay vào đó, ta muốn biết efficiency thay đổi thế nào khi kích thước input thay đổi.

Đo space complexity xét không gian do input của algorithm và auxiliary space sử dụng. Auxiliary space là không gian bổ sung do algorithm dùng, chẳng hạn các variable tạm được tạo trong quá trình thực thi. Chúng không ảnh hưởng lâu dài đến bộ nhớ, nhưng trong lúc algorithm chạy thì cần được tính đến. Vì vậy, có thể xem space complexity là tổng working memory algorithm cần.

### Tầm quan trọng của space complexity

Nếu tự nghiên cứu về efficiency của algorithm, bạn có thể nghĩ space complexity không quan trọng lắm. Hầu hết bài viết về chủ đề này dành toàn bộ hoặc phần lớn thời gian nói về time complexity, và nếu có nhắc space complexity thì gần như chỉ là chú thích.

Cũng đúng khi nói hầu hết algorithm bạn viết có thể xử lý input khá vừa phải, vì vậy space thường không trở thành vấn đề. Bạn gần như chắc chắn gặp vấn đề chương trình chạy chậm trước khi gặp vấn đề hết bộ nhớ.

Mặt khác, hardware thường có lượng memory cố định. Trong hầu hết tình huống, bạn không thể dễ dàng mở rộng nó ngay khi chương trình chạy. Khi vấn đề là thời gian, bạn chỉ cần cho chương trình chạy lâu hơn và cuối cùng nó sẽ trả kết quả. Với space thì không thể làm vậy.

Nhìn chung, có lẽ bạn sẽ gặp nhiều vấn đề mà thời gian thực thi quan trọng hơn lượng space sử dụng, nhưng biết cách đo space complexity sẽ giúp bạn sẵn sàng khi gặp tình huống có giới hạn về space.

### Đo space complexity

Tin tốt là ta đo space complexity theo đúng cách đo time complexity. Bạn đã học Big O ở bài trước, nên đã biết cách đo efficiency của code. Khác biệt là bạn cần nghĩ về cách algorithm sử dụng memory thay vì time.

Điều đầu tiên cần biết là, giống time complexity, ta đo space complexity bằng cách xét mọi bước, bao gồm cả constant, rồi bỏ constant khi áp dụng Big O Notation cho algorithm. Vì vậy, ta có thể có algorithm dùng memory theo Linear Complexity khi input thay đổi, đồng thời tạo 3 variable tạm. Ta có thể nghĩ complexity của algorithm là O(N) + 3 auxiliary variable dùng memory. Vì 3 variable đó không đổi dù input size thế nào, ta không xét chúng khi tính space complexity. Vậy space complexity là O(N). Điều này hẳn quen thuộc từ bài time complexity.

Nhắc lại, các Big O Notation phổ biến nhất là:

- O(1) - Constant Complexity
- O(log N) - Logarithmic Complexity
- O(N) - Linear Complexity
- O(N log N) - N x log N Complexity
- O(n&#178;) - Quadratic Complexity
- O(n&#179;) - Cubic Complexity
- O(2&#8319;) - Exponential Complexity
- O(N!) - Factorial Complexity

Hãy cùng xem một số ví dụ. Ta không đi qua mọi complexity có thể có vì đa số không áp dụng cho các data structure bạn quen thuộc và dùng nhiều nhất. Ta sẽ đề cập những loại phổ biến nhất.

#### O(1) - Constant complexity

Xem ví dụ này:

```javascript
function multiply(num1, num2) {
  return num1 * num2;
}
```

Hy vọng rõ ràng rằng bất kể truyền argument nào vào function call, chỉ một value được tạo (tích của hai number). Nó không thay đổi. Vì vậy, space nó chiếm luôn là O(1).

#### O(N) - Linear complexity

Hầu hết data structure bạn gặp có space complexity O(N). Điều đó hợp lý: khi tăng số item trong data structure, space data structure chiếm cũng tăng tuyến tính.

```javascript
function sumArr(arr) {
  const copyArr = arr.slice();
  let sum = 0;
  copyArr.forEach((number) => {
    sum += number;
  });
  return sum;
}
```

Ta viết hơi dài hơn cách thường viết trong JavaScript để làm rõ hơn. Ở đây có một method nhận array. Bên trong có hai variable: một gọi là `sum`, variable còn lại là `copyArr` chứa bản sao của array truyền vào. Sau đó có loop `forEach` iterate qua array. Lượng space algorithm cần phụ thuộc vào array truyền vào. Array có thể có 3 element hoặc 300. Khi không biết độ dài array, ta gọi nó là N, nên ta có N + 1 variable gọi là `sum`. Ta biết Big O bỏ constant, nên còn N, hay notation Big O là O(N).

Tại sao phải sao chép array? Phần sau sẽ giải thích.

Complexity được lặp lại bất kể data structure:

```javascript
function sumObjectValues(obj) {
  const copyObject = { ...obj };
  let sum = 0;
  Object.values(copyObject).forEach((value) => {
    sum += value
  });
  return sum;
}
```

Ở đây, khi object tăng kích thước, space nó dùng cũng tăng theo cách tuyến tính.

#### Phân tích auxiliary space

Một lĩnh vực thường gây nhầm lẫn khi xét space complexity là điều gì được xem là dùng space trong ngữ cảnh algorithm. Ở ví dụ trước, ta viết method nhân bản argument là array và object. Ta làm vậy để rõ ràng. Nhưng nếu viết method như sau:

```javascript
function sumArr(arr) {
  let sum = 0;
  arr.forEach((number) => {
    sum += number;
  });
  return sum;
}
```

Khi một data structure được truyền làm argument, đặc biệt với ngôn ngữ truyền array bằng reference thay vì value, có thể không rõ method đó có tính space của data structure khi tính space complexity hay không. Ta vẫn có thể nói algorithm này cần O(N) space, tính cả kích thước input. Hoặc nói việc tạo input array thuộc về routine bên ngoài đã cấp phát nó trước khi gọi function. Khi đó algorithm này dùng O(1) space, vì mọi variable khác đều là constant. Cách thứ hai gọi là "auxiliary space analysis."

Với cách này, ta chỉ xem xét space *bổ sung* algorithm chiếm, không tính input. Điều này hữu ích tùy tình huống khi mô tả các algorithm dùng extra space khác nhau. Hãy xem ví dụ nhanh:

```javascript
function squareNumsInPlace(arr) {
  for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i] * arr[i];
  }

  return arr;
}

function squareNumsNewArr(arr) {
  // note that `Array.prototype.map()` makes a *new* array
  return arr.map((number) => number * number);
}
```

Ở đây có hai function nhận array number và trả về array trong đó mỗi number được bình phương. Function đầu thay đổi trực tiếp input array, function thứ hai tạo array mới trong đó mỗi number được bình phương. Với phân tích space truyền thống, cả hai đều dùng O(N) space. Nhưng trong tình huống này, điều đó không cho biết nhiều về khác biệt (rất rõ) trong cách hai algorithm sử dụng memory. Tốt hơn là dùng auxiliary space analysis và nói function trên dùng O(1) extra space, còn function dưới dùng O(N).

#### Complexity khác

Như đã nói, nhiều data structure có space complexity O(N), nên bạn sẽ không viết nhiều algorithm có space complexity khác.

Tuy nhiên, một số recursive function và sorting algorithm có space complexity khác, dù thông thường bạn không có nhiều lý do để xét các loại khác.

Ở bài trước, một bài tập có liên kết tới [Big-O cheat sheet](https://www.bigocheatsheet.com/). Nếu xem lại, có thể bạn sẽ đánh giá cao hơn mức độ tuyệt vời của nó như một tài liệu tham khảo về space và time complexity. Nếu kéo xuống phần data structure rồi đến sorting algorithm, bạn sẽ thấy time và space complexity. Hãy chú ý có bao nhiêu cái là O(N), đặc biệt với data structure. Nhiều sorting algorithm chỉ có O(1) space complexity, điều cần ghi nhớ khi bạn gặp các sorting algorithm khác trong quá trình học.

Đó là lý do ta không đi sâu vào ví dụ cho các Big O notation khác với space complexity. Ta sẽ phải tạo những ví dụ rắc rối không đại diện cho phần lớn code bạn sẽ viết. Nếu tự gặp một ví dụ thực tế hay trong code của mình, hãy cho chúng tôi biết và có thể chúng tôi sẽ thêm vào đây để người khác tham khảo.

### Tổng kết

Đo complexity của algorithm, dù là time hay space, có thể khó. Việc này cần thực hành và suy xét. Với phần lớn code luyện tập, đây không phải điều bạn nghĩ đến, đặc biệt khi còn đang vật lộn để code chạy được.

Tuy nhiên, khi code đã hoạt động và bạn muốn refactor, chắc chắn nên dành chút thời gian xem code có hiệu quả nhất có thể không. Bạn có tạo variable không cần thiết không? Hay algorithm dùng data structure có time complexity kém hơn cho mục đích sử dụng chính so với một data structure khác?

Ngoài các cân nhắc đó, bạn cũng cần cân bằng khả năng đọc của code. Nếu đưa [memoization](https://en.wikipedia.org/wiki/Memoization) vào để code hiệu quả hơn, liệu code có khó hiểu hơn nhiều không? Đánh đổi đó có đáng không? Cuối cùng bạn phải tự quyết định. Lời khuyên của chúng tôi là ưu tiên readability trước, rồi refactor để hiệu quả hơn nếu performance bị ảnh hưởng rõ ràng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc câu trả lời đầu tiên cho [analyzing space complexity on stack exchange](https://cs.stackexchange.com/questions/127933/analyzing-space-complexity-of-passing-data-to-function-by-reference) để hiểu một số cách đếm space khác nhau.
1. [Article on recursion and space complexity](https://dev.to/elmarshall/recursion-and-space-complexity-13gc) cung cấp thêm một chút bối cảnh về recursive function và space complexity của chúng.

</div>