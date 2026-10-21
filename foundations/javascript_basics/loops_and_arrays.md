### Giới thiệu

Một trong những thách thức của việc lập trình là sự lặp lại. Một chút lặp lại thủ công hoàn toàn không sao, nhưng đôi khi chúng ta muốn làm cho các chỉ dẫn lặp lại trở nên gọn gàng và dễ đọc hơn. Một thách thức khác là xử lý lượng dữ liệu lớn. Ví dụ, nếu muốn lưu tên của tất cả học sinh trong lớp, bạn sẽ làm thế nào? Bạn có thể tạo một biến cho mỗi cái tên, nhưng như vậy vừa tẻ nhạt vừa kém hiệu quả. Việc quản lý và cập nhật cũng sẽ khó khăn. Nếu sau này muốn thay đổi hoặc truy cập các tên đó thì sao?

May mắn là có những cách để xử lý các vấn đề này. Trong bài học này, bạn sẽ tìm hiểu về vòng lặp, những cấu trúc điều khiển cho phép thực thi lặp đi lặp lại một khối mã. Bạn cũng sẽ học về mảng, những cấu trúc dữ liệu có thể lưu nhiều giá trị trong một biến duy nhất. Mảng rất hữu ích để tổ chức và thao tác với lượng dữ liệu lớn. Vòng lặp thường đặc biệt tiện lợi khi cần thực hiện cùng một thao tác trên từng phần tử của mảng. Cuối cùng, bạn sẽ được làm quen với phát triển hướng kiểm thử (TDD), tức thực hành viết các bài kiểm thử cho mã trước khi viết chính mã đó.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Sử dụng vòng lặp.
- Sử dụng mảng.
- Sử dụng các phương thức mảng như `map`, `filter` và `reduce`.
- Bắt tay vào thực hiện các bài tập TDD.

### Vòng lặp

Máy tính không biết mệt, lại còn thực sự, *thực sự* nhanh! Vì vậy, chúng rất phù hợp để giải quyết những bài toán liên quan đến việc thực hiện phép tính nhiều lần. Trong một số trường hợp, máy tính có thể lặp lại một tác vụ *hàng nghìn* hoặc thậm chí *hàng triệu* lần chỉ trong vài giây, trong khi con người có thể mất nhiều giờ (dĩ nhiên, tốc độ còn phụ thuộc vào độ phức tạp của phép tính và tốc độ của chính máy tính).

Để lặp lại một tập hợp chỉ dẫn nhiều lần, chúng ta có thể dùng một **vòng lặp**. Hãy tìm hiểu các tài liệu sau về vòng lặp. Một số tài liệu có ví dụ sử dụng mảng; chúng ta sẽ tìm hiểu kỹ hơn về mảng ở phần tiếp theo, nhưng hiện tại chỉ cần biết rằng mảng đơn giản là danh sách các phần tử.

1. Đọc [Looping Code của MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Looping_code). Bài này khá dài, nhưng hãy nhớ làm các bài tập gần cuối trang.
1. Một lần nữa, hãy đọc thông tin tương tự trong bối cảnh hơi khác qua bài ["Loops: While and For" của JavaScript.info](http://javascript.info/while-for). Nhớ làm các bài tập ở cuối trang. Cách học tốt nhất là thực hành.

### Mảng

Chuỗi và số có thể là những khối xây dựng cơ bản, nhưng khi các tập lệnh trở nên phức tạp hơn, bạn sẽ cần một cách để xử lý số lượng lớn các giá trị đó. May mắn là JavaScript có một vài kiểu dữ liệu phục vụ chính mục đích này. Mảng là một tập hợp có thứ tự gồm các phần tử (chuỗi, số hoặc những thứ khác). Bạn có thể nhận ra rằng một số phương thức mảng trong các tài liệu sau thực hiện những việc tương tự vòng lặp. Đôi khi bạn cần nhiều quyền kiểm soát hơn thông qua một vòng lặp thực sự, nhưng đôi khi phương thức mảng phù hợp đã đủ và bạn thích cách nó được diễn đạt hơn.

1. Khóa học cấp tốc về [Mảng JavaScript](https://www.youtube.com/watch?v=7W4pQQ20nJg) này trình bày tổng quan về mảng trong JavaScript trong khoảng 6 phút.
1. Đọc phần [giới thiệu về mảng](https://javascript.info/array) của JavaScript.info. Bạn không cần làm các bài tập ở cuối trang.
1. Đọc [hướng dẫn về phương thức mảng](https://javascript.info/array-methods) của JavaScript.info để tìm hiểu sâu hơn về một số phương thức mảng khác trong JavaScript. Một lần nữa, bạn không cần làm bài tập ở cuối trang; chúng ta sẽ quay lại với chúng sau.
1. Đánh dấu trang tài liệu [Array của MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array). Bạn không cần học thuộc bất cứ điều gì ở đây. Đây là nơi bạn sẽ tìm thấy tài liệu về tất cả thuộc tính và phương thức mảng dựng sẵn. Có lẽ bạn sẽ thường xuyên quay lại đây khi viết mã và giải quyết vấn đề.

### Một vài ví dụ về phép màu của mảng

Ngoài việc là một cách nhanh chóng và tiện lợi để lưu dữ liệu, mảng còn có một tập hợp các hàm cho phép thao tác với dữ liệu theo những cách rất mạnh mẽ. Khi có thêm kinh nghiệm với các hàm này, bạn sẽ bắt đầu thấy chúng có thể được dùng ở khắp nơi! Thực ra chỉ có một số ít hàm như vậy, nhưng bạn sẽ sớm thấy những khả năng mà chúng mở ra gần như vô tận.

Để minh họa, hãy xét một hàm `sumOfTripledEvens`. Hàm này sẽ:

1. Nhận vào một mảng.
1. Nhân ba mọi số chẵn.
1. Sau đó cộng tất cả những số chẵn đó lại.

Bạn có nghĩ ra cách triển khai một hàm như vậy bằng mã giả không?

1. Chúng ta cần thực hiện thao tác chỉ trên các số chẵn.
1. Chúng ta cần biến đổi *những* số đó bằng cách nhân chúng với 3.
1. Cuối cùng, chúng ta cần cộng kết quả của phép biến đổi trước đó.

Dựa trên logic đó, có thể bạn sẽ triển khai một hàm như sau:

```javascript
function sumOfTripledEvens(array) {
  let sum = 0;
  for (let i = 0; i < array.length; i++) {
    // Step 1: If the element is an even number
    if (array[i] % 2 === 0) {
      // Step 2: Multiply this number by three
      const tripleEvenNumber = array[i] * 3;

      // Step 3: Add the new number to the total
      sum += tripleEvenNumber;
    }
  }
  return sum;
}
```

Trong đoạn mã trên, có 3 phần quan trọng cần lưu ý:

- `if (array[i] % 2 === 0)`: kiểm tra một số đã cho có phải số chẵn hay không.
- `const tripleEvenNumber = array[i] * 3`: lấy số đó nhân ba.
- `sum += tripleEvenNumber`: cộng số đó vào tổng.

Mỗi phần đều giải quyết một vấn đề quan trọng trong mã của chúng ta. Tuy nhiên, chúng ta có thể làm cùng một việc bằng một số phương thức mảng, và đôi khi kết quả sẽ dễ đọc, dễ hiểu hơn một chút. Hãy dành chút thời gian xem chúng ta có thể sử dụng các phương thức đó như thế nào.

#### Phương thức map

`map` là một hàm như vậy. Nó nhận một `callback` làm đối số; nói một cách hoa mỹ, điều đó có nghĩa là “tôi muốn bạn truyền một hàm khác làm đối số cho hàm của tôi”.

Giả sử chúng ta có một hàm `addOne`, nhận `num` làm đối số và trả về `num` sau khi tăng 1, cùng một mảng số `[1, 2, 3, 4, 5]`. Giả sử chúng ta muốn dùng hàm `addOne` để tăng tất cả các số này lên 1.

Thay vì tạo một vòng lặp `for` và lặp qua mảng trên, chúng ta có thể dùng phương thức mảng `map`, phương thức này **tự động** lặp qua một mảng thay chúng ta. Chúng ta không cần làm gì thêm ngoài việc truyền vào hàm muốn sử dụng:

```javascript
function addOne(num) {
  return num + 1;
}
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map(addOne);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

`map` trả về một mảng mới và không thay đổi mảng ban đầu.

```javascript
// The original array has not been changed!
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

Dùng `map` theo cách này có thể thanh thoát hơn so với việc viết vòng lặp `for` rồi lặp qua mảng. Nhưng chúng ta còn có thể làm tốt hơn. Vì không dùng `addOne` ở nơi nào khác và đó là một hàm đơn giản, chúng ta có thể định nghĩa nó ngay bên trong `map` bằng một hàm mũi tên như sau:

```javascript
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map((num) => num + 1);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

#### Phương thức filter

`filter` khá giống `map`. Nó vẫn lặp qua mảng và áp dụng hàm callback lên từng phần tử. Tuy nhiên, thay vì biến đổi các giá trị trong mảng, nó trả về một mảng mới trong đó mỗi phần tử chỉ được đưa vào *nếu* hàm callback trả về `true` cho phần tử đó.

Giả sử chúng ta có một hàm `isOdd`, trả về `true` nếu một số là số lẻ hoặc `false` nếu không phải.

Phương thức `filter` yêu cầu `callback` trả về `true` hoặc `false`. Nếu trả về `true`, giá trị sẽ được đưa vào kết quả; nếu không thì không. Xét mảng trong ví dụ trước, `[1, 2, 3, 4, 5]`. Nếu muốn loại bỏ tất cả số chẵn khỏi mảng này, chúng ta có thể dùng `.filter()` như sau:

```javascript
function isOdd(num) {
  return num % 2 !== 0;
}
const arr = [1, 2, 3, 4, 5];
const oddNums = arr.filter(isOdd);
console.log(oddNums); // Outputs [1, 3, 5];
console.log(arr); // Outputs [1, 2, 3, 4, 5], original array is not affected
```

- `filter` sẽ lặp qua `arr` và lần lượt truyền **mọi giá trị** vào hàm callback `isOdd`.
- `isOdd` sẽ trả về `true` khi giá trị là số lẻ, nghĩa là giá trị đó được đưa vào kết quả.
- Nếu là số chẵn, `isOdd` sẽ trả về `false` và không đưa nó vào kết quả cuối cùng.

#### Phương thức reduce

Cuối cùng, giả sử chúng ta muốn nhân tất cả các số trong `arr` với nhau như sau: `1 * 2 * 3 * 4 * 5`. Trước hết, chúng ta phải khai báo một biến `total` và khởi tạo nó bằng 1. Sau đó, chúng ta sẽ lặp qua mảng bằng vòng lặp `for` và nhân `total` với số hiện tại.

Nhưng thực ra chúng ta không cần làm tất cả những việc đó; đã có phương thức `reduce` giúp chúng ta. Cũng giống `.map()` và `.filter()`, nó nhận một hàm callback. Tuy nhiên, phương thức mảng này có hai điểm khác biệt chính:

- Hàm callback nhận hai đối số thay vì một. Đối số đầu tiên là `accumulator`, giá trị kết quả hiện tại *tại thời điểm đó trong vòng lặp*. Ở lần đầu tiên, giá trị này sẽ được đặt thành `initialValue` (được mô tả ở gạch đầu dòng tiếp theo), hoặc thành phần tử đầu tiên trong mảng nếu không cung cấp `initialValue`. Đối số thứ hai của callback là giá trị `current`, tức phần tử đang được lặp qua.
- Bản thân `reduce` cũng nhận `initialValue` làm đối số thứ hai tùy chọn (sau callback). Điều này hữu ích khi chúng ta không muốn giá trị ban đầu là phần tử đầu tiên của mảng. Chẳng hạn, nếu muốn tính tổng mọi số trong mảng, chúng ta có thể gọi reduce mà không có `initialValue`; nhưng nếu muốn tính tổng mọi số rồi cộng thêm 10, chúng ta có thể dùng 10 làm `initialValue`.

```javascript
const arr = [1, 2, 3, 4, 5];
const productOfAllNums = arr.reduce((total, currentItem) => {
  return total * currentItem;
}, 1);
console.log(productOfAllNums); // Outputs 120;
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

Trong hàm trên, chúng ta:

- Truyền vào một hàm callback, đó là `(total, currentItem) => total * currentItem`.
- Khởi tạo `total` bằng `1` trong đối số thứ hai.

Vậy `.reduce()` sẽ đi qua từng phần tử trong `arr` và áp dụng hàm `callback` lên phần tử đó. Nó cập nhật `total` mà không thực sự thay đổi chính mảng. Sau khi hoàn tất, nó trả về `total`.

#### Tóm tắt

Bạn đã tìm hiểu về ba phương thức mảng mạnh mẽ là `map`, `filter` và `reduce`. Chúng cho phép chúng ta viết mã có khả năng dễ đọc hơn, từ đó có thể ít phát sinh lỗi hơn.

Để ôn nhanh các phương thức mảng này, hãy xem bức hình minh họa chúng dưới dạng các bước làm bánh sandwich:

![ví dụ sử dụng các phương thức filter, map và reduce để minh họa trực quan việc làm một chiếc bánh sandwich](https://cdn.statically.io/gh/TheOdinProject/curriculum/335ab97a10d66d0f07e81d01fc6b1c63d31dc5ae/foundations/javascript_basics/loops_and_arrays/imgs/00.jpg)

Trước khi chuyển sang phần tiếp theo, hãy xem cách viết lại hàm `sumOfTripledEvens(array)` bằng ba phương thức này:

```javascript
function sumOfTripledEvens(array) {
  return array
    .filter((num) => num % 2 === 0) // filter the array to only contain even numbers
    .map((num) => num * 3) // multiply each number in the array by 3
    .reduce((acc, curr) => acc + curr); // calculate the sum of all numbers in the array
}
```

### Phát triển hướng kiểm thử

Phát triển hướng kiểm thử (TDD) là một cụm từ bạn thường nghe trong thế giới phát triển phần mềm. Nó chỉ việc viết các bài kiểm thử tự động mô tả cách mã cần hoạt động trước khi thực sự viết mã. Ví dụ, nếu muốn viết một hàm cộng hai số, trước tiên bạn sẽ viết một bài kiểm thử sử dụng hàm đó và cung cấp kết quả mong đợi. Bài kiểm thử sẽ thất bại trước khi bạn viết mã, và khi bài kiểm thử chạy thành công, bạn có thể biết mã của mình hoạt động đúng.

Xét trên nhiều phương diện, TDD hiệu quả hơn nhiều so với việc viết mã không có kiểm thử. Nếu không có bài kiểm thử cho hàm cộng ở trên, chúng ta sẽ phải tự chạy mã hết lần này đến lần khác, thử nhiều số khác nhau cho đến khi chắc chắn rằng nó hoạt động... với một phép cộng đơn giản như `add(2, 2)` thì không đáng kể, nhưng hãy tưởng tượng phải làm vậy với các hàm phức tạp hơn, chẳng hạn kiểm tra xem ai đó đã thắng trò tic-tac-toe hay chưa: (`game_win(["o", null,"x",null,"x",null,"x", "o", "o"])`). Nếu không dùng TDD, có lẽ bạn sẽ phải tự chơi nhiều ván với chính mình chỉ để kiểm tra hàm có hoạt động đúng hay không!

Sau này trong khóa học, chúng tôi sẽ dạy bạn cách thực sự viết các bài kiểm thử đó. Còn bây giờ, bạn sẽ tiếp tục làm các bài tập JavaScript từ trước.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Truy cập [các bài tập về phương thức mảng](https://javascript.info/array-methods#tasks) ở cuối bài viết về phương thức mảng của JavaScript.info và chỉ làm các bài sau:
   - `Translate border-left-width to borderLeftWidth`
   - `Filter range`
   - `Filter range "in place"`
   - `Sort in decreasing order`
   - `Copy and sort array`
   - `Shuffle an array`
   - `Filter unique array members`
1. Truy cập [thư mục `foundations/loops_and_arrays` của kho bài tập JavaScript](https://github.com/TheOdinProject/javascript-exercises/tree/main/foundations/loops_and_arrays). Đọc từng tệp README trước khi làm các bài tập sau theo đúng thứ tự (có thể đã lâu rồi, nên hãy dành chút thời gian đọc lại [README của chính kho mã](https://github.com/TheOdinProject/javascript-exercises#how-to-use-these-exercises "JavaScript exercises repo README") để xem hướng dẫn chung):
    - `01_repeatString`
    - `02_reverseString`
    - `03_removeFromArray`
    - `04_sumAll`
    - `05_leapYears`
    - `06_tempConversion`

   Lưu ý: Có thể tìm lời giải cho các bài tập này trong thư mục `solution` của từng bài.

</div>