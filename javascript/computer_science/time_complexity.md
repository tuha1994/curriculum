### Giới thiệu

Đến thời điểm này bạn đã viết khá nhiều code và hy vọng đã chuyển từ việc chỉ cố viết code chạy được sang cân nhắc readability và maintainability. Bạn có thể dành thời gian suy nghĩ cách tạo các abstraction cần thiết để code vẫn dễ làm việc ngay cả khi requirement phát triển.

Readability và maintainability của code cực kỳ quan trọng. Sau cùng, có lẽ bạn sẽ dành nhiều thời gian, nếu không muốn nói là nhiều hơn, để đọc code so với viết code. Bạn cần bảo đảm các feature mới được tích hợp dễ dàng.

Tuy nhiên, khi viết code còn một cân nhắc khác cũng quan trọng không kém. Đó là efficiency! Bạn cần hiểu code mình viết sẽ hoạt động thế nào. Bạn cũng cần hiểu lựa chọn của mình ảnh hưởng đến performance ra sao để chọn đúng data structure và algorithm cho requirement.

Trong lập trình, có hai cách đo efficiency của code: time complexity hoặc space complexity.

Trong bài học này, chúng ta giới thiệu các khái niệm cốt lõi về việc đo time efficiency của code bạn viết.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài học này.

- Efficiency của algorithm được đo như thế nào.
- Big O là gì.
- Những Big O notation nào được dùng để đo efficiency của algorithm.
- Còn cách nào khác để đo efficiency của algorithm.
- Làm gì khi hai algorithm có cùng complexity.

### Cơ bản về efficiency

Bước đầu tiên để làm chủ code hiệu quả là hiểu cách đo nó. Hãy xem một chương trình nhỏ in ra mọi số lẻ giữa 1 và 10.

```javascript
function oddNumbersLessThanTen() {
  let currentNumber = 1;

  while (currentNumber < 10) {
    if (currentNumber % 2 !== 0) {
      console.log(currentNumber);
    }

    currentNumber += 1;
  }
}
```

Nếu chạy trong terminal, bạn sẽ nhận các number `1`, `3`, `5`, `7` và `9` được in ra console. Có lẽ nó chỉ mất một phần giây để chạy. Chạy lại có thể mất cùng thời gian, nhanh hơn hoặc chậm hơn tùy những việc khác máy tính đang làm. Chạy trên máy khác cũng có thể nhanh hoặc chậm hơn. Vì vậy, điều quan trọng là không bao giờ đo efficiency của algorithm dựa trên thời gian thực thi.

Vậy đo thế nào?

Cách đo code efficiency là đánh giá nó cần bao nhiêu 'step' để hoàn thành. Nếu biết một algorithm cần 5 step và algorithm khác cần 20 step để làm cùng một việc, bạn có thể nói algorithm 5-step luôn chạy nhanh hơn algorithm 20-step trên cùng máy tính.

Hãy quay lại `oddNumbersLessThanTen function`. Algorithm cần bao nhiêu step?

1. Gán số 1 cho một variable. Đó là một step.

1. Có một loop. Trong mỗi iteration của loop, ta làm:

    1. So sánh `currentNumber` xem có nhỏ hơn 10 không. Đó là 1 step.
    1. Kiểm tra `currentNumber` có lẻ không. Đó là 1 step.
    1. Nếu có thì output ra terminal. Đó là 1 step mỗi 2 iteration.
    1. Tăng `currentNumber` lên 1. Đó là 1 step.

1. Để thoát loop, ta cần so sánh `currentNumber` thêm một lần để thấy nó không còn nhỏ hơn 10. Đó là step cuối cùng.

Có 3 step cho mỗi loop iteration và loop lặp 9 lần, tức 27 step. Sau đó có một step chỉ lặp trong một nửa số loop iteration, tức 5 step. Gán giá trị ban đầu cho `currentNumber` và kiểm tra điều kiện thoát loop mỗi việc là một step. 27 + 5 + 1 + 1 = 34 step.

Vì vậy algorithm cần 34 step để hoàn thành.

Dù hữu ích, con số này không thực sự giúp so sánh algorithm. Để thấy tại sao, hãy sửa algorithm ban đầu để nhận một number thay vì mặc định là 10.

```javascript
function oddNumbers(maxNumber) {
  let currentNumber = 1;

  while (currentNumber < maxNumber) {
    if (currentNumber % 2 !== 0) {
      console.log(currentNumber);
    }

    currentNumber += 1;
  }
}
```

Algorithm này cần bao nhiêu step?

Có lẽ bạn đã nhận ra câu trả lời là tùy. Nếu đặt `maxNumber` bằng 10 như trước, số step là 34, nhưng nhập number khác thì số step thay đổi. Không có một con số cụ thể để đo efficiency của code vì nó thay đổi theo input bên ngoài.

Điều ta thực sự muốn đo là số step của algorithm thay đổi thế nào khi data thay đổi. Điều này giúp trả lời câu hỏi code viết có scale được không.

Để làm vậy, ta cần tìm hiểu khái niệm mới: Asymptotic Notation, đặc biệt là Big O.

### Asymptotic notation

Asymptotic Notation dùng để mô tả running time của algorithm. Vì running time có thể khác nhau tùy input, có nhiều notation đo running time theo những cách khác nhau. Ba loại phổ biến nhất:

- Big O Notation - biểu diễn upper bound của algorithm, tức trường hợp xấu nhất về cách algorithm hoạt động.
- Omega Notation - biểu diễn lower bound của algorithm, tức trường hợp tốt nhất.
- Theta Notation - biểu diễn cả upper bound và lower bound, từ đó phân tích average case complexity của algorithm.

Big O là notation thường được nhắc đến nhất vì cần bảo đảm worst-case scenario của code có thể scale khi input tăng trong application.

Cũng cần lưu ý các notation bên dưới cho Big O cũng áp dụng cho Omega và Theta. Điểm khác nằm ở cách chúng đo efficiency của algorithm và vì vậy notation nào nên được dùng. Điều này sẽ rõ hơn khi bạn đọc tiếp.

### Big O là gì?

Big O cho ta cách nhất quán để đo efficiency của algorithm. Nó đo thời gian algorithm chạy khi input tăng, để có thể trực tiếp so sánh performance của hai algorithm và chọn algorithm tốt hơn.

Big O không phải đoạn code bạn đưa algorithm vào để nó nói algorithm hiệu quả thế nào. Bạn cần đo số step thay đổi ra sao khi data tăng, rồi dùng kết quả đó áp dụng Big O Notation và so sánh với algorithm khác. Trong nhiều trường hợp, bạn dùng data structure có cách tương tác đã được biết rõ, nên dễ đánh giá nó scale thế nào khi input thay đổi.

Trước hết, ta tóm tắt Big O Notation rồi cung cấp thêm bối cảnh cho từng loại. Các tài liệu đọc sẽ đi sâu hơn.

#### Big O notation

Các Big O Notation phổ biến nhất, theo thứ tự tốc độ từ nhanh đến chậm:

- O(1) - Constant Complexity
- O(log N) - Logarithmic Complexity
- O(N) - Linear Complexity
- O(N log N) - N x log N Complexity
- O(n&#178;) - Quadratic Complexity
- O(n&#179;) - Cubic Complexity
- O(2&#8319;) - Exponential Complexity
- O(N!) - Factorial Complexity

#### O(1) - Constant complexity

Để hiểu Constant Complexity, hãy dùng array.

```javascript
arr = [1, 2, 3, 4, 5];
```

Nếu muốn xem index 2 chứa gì, ta dùng `arr[2]` và nhận `3`. Việc này chỉ mất một step. Nếu nhân đôi array...

```javascript
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
```

Ta vẫn truy cập được mọi element chỉ trong một step. `arr[7]` cho `8` trong một step. Array có thể tiếp tục lớn lên nhưng ta luôn truy cập element trong một step. Nó là constant. Do đó có `O(1)`.

Tìm một thứ trong một step là mức tốt nhất của time complexity.

Khi xem dạng đơn giản nhất của Big O, hãy lưu ý một điểm dễ gây nhầm. Bạn có thể nghĩ: có thật chỉ một step không? Về kỹ thuật thì không; máy tính trước hết phải tìm array nằm ở đâu trong memory, rồi từ element đầu tiên nhảy tới index được cung cấp. Ít nhất phải vài step. Vì vậy viết `O(1 + 2(steps))` cũng không sai. Tuy nhiên, 2 step đó chỉ là incidental. Với array 10.000 element, số step vẫn bằng array 2 element. Do đó Big O không quan tâm các con số incidental này. Chúng không cho biết complexity tăng thế nào khi data size thay đổi vì chúng là constant, nên được bỏ qua trong Big O. Big O chỉ muốn mô tả complexity của algorithm tương đối với input size.

Số step có quan trọng không? Có thể có, và ta sẽ nói khi nào ở phần sau.

#### O(log N) - Logarithmic complexity

Logarithmic Complexity cho biết số step của algorithm tăng thêm 1 khi data tăng gấp đôi. Điều đó vẫn rất hiệu quả. Từ 5.000 lên 10.000 data element mà chỉ thêm một step có thể scale rất tốt.

Binary Search là một algorithm như vậy. Nó chỉ hoạt động trên sorted array, nhưng nếu có array 10 item theo thứ tự:

```javascript
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Để tìm array có số `7` không, Binary Search tính middle index bằng công thức:

```javascript
let middleIndex = Math.floor((startIndex + endIndex) / 2)
```

Ở đây, `startIndex` là đầu array (ban đầu là `0`), `endIndex` là element cuối (ban đầu là `9` với array 10 element). Element giữa tại index `4` là `5`.

Vì array đã sắp xếp và `7` lớn hơn `5`, ta loại `5` và mọi thứ bên trái vì chúng đều nhỏ hơn `7`.

```javascript
arr = [-, -, -, -, -, 6, 7, 8, 9, 10]
```

Chỉ trong một step, ta loại được nửa array. Bây giờ tính lại middle index với `startIndex` và `endIndex` mới. Middle index sẽ là `7`, và số tại index này là `8`.

Vì `7` nhỏ hơn `8`, ta loại `8` và mọi thứ bên phải.

```javascript
arr = [6, 7, -, -, -]
```

Lặp quá trình cho đến khi array chỉ còn một item. Nếu item đó khớp số đang tìm thì đã tìm thấy. Nếu không, số đó không có trong array.

Bảng dưới tóm tắt kích thước array tăng gấp đôi và số step theo Big O cần để đi đến một element nhằm kiểm tra nó có khớp thứ đang tìm không:

| Size | Steps |
| ---- | ----- |
| 1    | 1     |
| 2    | 2     |
| 4    | 3     |
| 8    | 4     |
| 16   | 5     |
| 32   | 6     |

Khá ấn tượng nhỉ!

#### O(N) - Linear complexity

Loại này khá dễ hiểu. Linear Complexity chỉ cho biết khi số item tăng, số step tăng đúng cùng tốc độ. Mỗi lần iterate qua array là một ví dụ Linear Complexity. Array 5 item có thể iterate mọi element trong 5 step. Array 10 item cần 10 step. Nếu gặp algorithm có Big O efficiency `O(N)`, bạn biết số step tăng cùng số element trong data structure.

#### O(N log N) - N x log N complexity

Không thể nói tên này không phù hợp. Notation này thường ngụ ý có algorithm ban đầu là `O(log N)`, như Binary Search liên tục chia array làm đôi ở ví dụ trước, nhưng với `O(N log N)`, mỗi nửa array lại được xử lý bởi algorithm complexity `O(N)`.

Một algorithm như vậy là merge sort ở bài trước. :)

Tuy nhiên, không phải mọi trường hợp `O(N log N)` đều được tạo như vậy. Có các trường hợp đặc biệt, như xây dựng [Cartesian tree](https://www.geeksforgeeks.org/cartesian-tree/), algorithm tự nhiên hoạt động như `O(N log N)` mà không dùng các phần nhỏ hơn có complexity `O(N)` hoặc `O(log N)` bên trong. Ai tò mò có thể xem algorithm này hoạt động ra sao. Điều đó cho thấy nested complexity phổ biến nhưng không phải cách duy nhất để algorithm đạt một time complexity nhất định.

#### O(n&#178;) - Quadratic complexity

Có lẽ bạn từng viết code có Quadratic Complexity. Nó thường xuất hiện khi loop qua một data set rồi trong mỗi loop lại loop qua data set đó lần nữa.

Ví dụ, nếu array có 3 item, nested loop cần 3&#178; = 9 sub-step. Thêm một item gần như gấp đôi thành 4&#178; = 16. Thêm item thứ 5 thành 5&#178; = 25 sub-step. Sau đó nhân đôi array lên 10 item làm sub-step từ 25 lên 100, tức cần nhiều hơn 4 lần công việc!
Chắc bạn thấy ta đang đi đến đâu...

#### O(n&#179;) - Cubic complexity

Hãy nghĩ đến triple nested loop. Nếu loop qua array n item, thêm 1 item tạo thêm outer loop, middle loop và innermost loop. Với triple nested loop trên array size n, tổng cộng cần n&#179; sub-step.
Ví dụ, array 3 item cần 3&#179; = 27 sub-step. Thêm một item làm con số hơn gấp đôi thành 4&#179; = 64. Với 5 item, tác vụ gần như lại tăng gấp đôi, 5&#179; = 125. Nhân đôi array lên 10 item cần 10&#179; = 1000 sub-step, gấp 8 lần trước! Array 100 item cần 1,000,000 sub-step. Đau đấy!

#### O(2&#8319;) - Exponential complexity

Exponential Complexity nghĩa là mỗi item thêm vào data size làm số step tăng gấp đôi so với trước. Hãy xem bảng để thấy nó nhanh chóng mất kiểm soát thế nào.

| Size | Steps |
| ---- | ----- |
| 1    | 2     |
| 2    | 4     |
| 3    | 8     |
| 4    | 16    |
| 5    | 32    |
| 6    | 64    |
| 7    | 128   |
| 8    | 256   |
| 9    | 512   |
| 10   | 1024  |

Nếu có thể, hãy tránh loại này, nếu không bạn sẽ không xử lý được nhiều data nhanh chóng.

#### O(N!) - Factorial complexity

Factorial của một number là tích của mọi number từ 1 đến number đó. Factorial của 4 (4!) là 4 \* 3 \* 2 \* 1.

Bạn sẽ gặp Factorial Complexity nếu cần tính permutation hoặc combination. Nếu có array và phải tìm mọi combination có thể tạo từ array, đó là Factorial complexity. Nó có thể xử lý với ít item, nhưng mỗi item mới trong dataset khiến con số tăng rất lớn.

Factorial của 3 là 6 (3 \* 2 \* 1). Factorial của 4 là 24. Factorial của 10? 3,628,800. Bạn có thể thấy mọi thứ nhanh chóng mất kiểm soát.

### Các lựa chọn thay Big O

Nếu Big O cho trường hợp xấu nhất về cách algorithm scale, có lựa chọn nào khác?

#### Big &#937; (Omega notation)

Omega Notation cho trường hợp tốt nhất của algorithm. Để hiểu, hãy xem một method và bàn cách đo complexity.

```javascript
function findValue(arr) {
  for (let i = 0; i < arr.length; i++) {
    let item = arr[i];
    if (item === 1) {
      return item;
    }
  }
}
```

Trong trường hợp xấu nhất (Big O), xảy ra khi item không có trong array hoặc là item cuối, algorithm có linear complexity `O(N)`. Điều này vì code phải iterate qua mọi element. Khi input size tăng gấp đôi, số iteration cần trong trường hợp xấu nhất cũng tăng gấp đôi.

Tuy nhiên, trường hợp tốt nhất là value đang tìm nằm ở item đầu tiên. Khi đó algorithm chỉ cần một step, complexity `O(1)`. Đây là Omega Complexity.

Omega Notation không được xem hữu ích bằng vì không có khả năng item thường xuyên đứng đầu data structure khi tìm kiếm, nên nó không cho biết algorithm scale tốt thế nào.

#### Big-Θ (Big-Theta notation)

Omega Notation đo trường hợp tốt nhất, Big O đo trường hợp xấu nhất, còn Theta cố gắng đưa ra giá trị chính xác hoặc khoảng hữu ích giữa upper bound và lower bound hẹp.

Nếu code loop qua mọi item trong array, kích thước array không quan trọng. Algorithm luôn chạy trong `O(N)` ở cả trường hợp tốt và xấu nhất. Khi đó performance chính xác trong mọi tình huống là `O(N)`, và đây là Theta performance của algorithm. Với algorithm khác, Theta có thể đại diện cho upper và lower bound của algorithm có complexity khác nhau. Ta không đi sâu hơn vì Big O là notation chính cho time complexity tổng quát của algorithm.

Đây chỉ là giải thích đơn giản để chủ đề dễ tiếp cận. Nếu bạn thiên về toán, có thể tìm các giải thích chi tiết hơn trên mạng.

### Tại sao dùng Big O

Sau khi chạm đến các cách khác nhau để định lượng efficiency của algorithm, hy vọng bạn hiểu tại sao ta chọn worst-case scenario khi đo efficiency.

Dùng worst-case scenario giúp bảo đảm algorithm scale trong mọi outcome. Nếu algorithm có thể chạy constant time nhưng cũng có thể chạy linear time trong trường hợp xấu nhất, nó chỉ scale khi input tăng nếu vẫn hoạt động lúc worst case xảy ra. Bạn cần tin code sẽ không bị treo khiến user bực bội nếu đột nhiên nhận input một triệu item thay vì 10.

### Algorithm có cùng complexity

Nếu viết hai algorithm có cùng complexity, có nghĩa chúng tốt như nhau không? Ta sẽ trả lời bằng hai ví dụ code rồi thảo luận thêm.

Ví dụ đầu là code đã thấy, function `oddNumbers`.

```javascript
function oddNumbers(maxNumber) {
  let currentNumber = 1;

  while (currentNumber < maxNumber) {
    if (currentNumber % 2 !== 0) {
      console.log(currentNumber);
    }

    currentNumber += 1;
  }
}
```

Time complexity của algorithm là `O(N)`. Khi data size tăng, số step tăng cùng tốc độ.

Xem phiên bản khác:

```javascript
function oddNumbers(maxNumber) {
  let currentNumber = 1;

  while (currentNumber < maxNumber) {
    if (currentNumber % 2 !== 0) {
      console.log(currentNumber);
    }

    currentNumber += 2;
  }
}
```

Thay đổi không nhiều, nhưng lần này tăng `currentNumber` thêm 2. Điều này ảnh hưởng runtime thế nào? Với input `n`, số step xấp xỉ một nửa vì mỗi lần iterate tăng 2. Đây là algorithm `O(N/2)`, nhưng như đã nói, ta không tìm thời gian chính xác mà tìm cách time tăng tương đối với input size. Big O không quan tâm constant vì chúng không liên quan đến cách algorithm scale khi input thay đổi, và sẽ thật khó chịu nếu phải so sánh `O(N/2 + 5 N)` với `O(N + 5 / 2N)`. Vì vậy Big O efficiency của cả hai algorithm là `O(N)`. Chúng scale cùng tốc độ khi input tăng.

Một cách khác là constant cuối cùng trở nên không đáng kể. Xem:

N=1, O(10N) = 10, O(n&#178;) = 1

N = 5, O(10N) = 50, O(n&#178;) = 25

N = 100, O(10N) = 1,000, O(n&#178;) = 10,000 *10x bigger*

N = 1000, O(10N) = 10,000, O(n&#178;) = 1,000,000 *100x bigger*

N = 10000, O(10N) = 100,000, O(n&#178;) = 100,000,000 *1000x bigger*

Vì vậy, O(10N) nhanh hơn O(n²) với N bằng 100. Trên thực tế, đôi khi algorithm n² nhanh hơn algorithm N với một tập input nhỏ, như hai ví dụ đầu cho thấy.

Hãy nhớ bảo đảm code viết hiệu quả nhất có thể trong time complexity của nó.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Big O Notation in JavaScript by Doable Danny](https://www.doabledanny.com/big-o-notation-in-javascript). Bài viết trình bày các complexity phổ biến bằng graph và ví dụ.
1. [Big-O cheat sheet](https://www.bigocheatsheet.com/) là tài nguyên tuyệt vời. Nó có chart complexity cho thấy các algorithm hoạt động thế nào khi data size tăng, đồng thời cho time complexity của các thao tác data structure và sorting algorithm phổ biến.
1. Đọc [Step-by-step Big O Complexity Analysis Guide, using JavaScript](https://www.sahinarslan.tech/posts/step-by-step-big-o-complexity-analysis-guide-using-javascript). Cuối bài có phần Space Complexity, bạn có thể bỏ qua lúc này.

</div>