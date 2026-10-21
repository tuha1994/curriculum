### Giới thiệu

Trước khi bắt đầu đào sâu vào một số JavaScript khá thú vị, chúng ta cần bắt đầu nói về *việc giải quyết vấn đề*: kỹ năng quan trọng nhất mà một nhà phát triển cần có.

Giải quyết vấn đề là công việc cốt lõi của các nhà phát triển phần mềm. Những ngôn ngữ lập trình và công cụ họ sử dụng chỉ đứng sau kỹ năng nền tảng này.

Trong cuốn sách *"Think Like a Programmer"*, V. Anton Spraul định nghĩa việc giải quyết vấn đề trong lập trình như sau:

> Giải quyết vấn đề là viết một chương trình nguyên bản thực hiện một tập hợp tác vụ cụ thể và đáp ứng tất cả các ràng buộc đã nêu.

Tập hợp tác vụ có thể trải dài từ việc giải các bài tập lập trình nhỏ cho đến xây dựng một trang mạng xã hội như Facebook hoặc một công cụ tìm kiếm như Google. Mỗi vấn đề có bộ ràng buộc riêng; chẳng hạn, hiệu năng cao và khả năng mở rộng có thể không quá quan trọng trong một bài tập lập trình, nhưng lại sống còn đối với những ứng dụng như Google, vốn phải phục vụ hàng tỷ truy vấn tìm kiếm mỗi ngày.

Những lập trình viên mới thường thấy giải quyết vấn đề là kỹ năng khó xây dựng nhất. Không hiếm trường hợp những người mới học lập trình dễ dàng tiếp thu cú pháp và các khái niệm lập trình, nhưng khi tự mình bắt tay viết mã, họ lại ngồi nhìn chằm chằm vào trình soạn thảo văn bản mà không biết bắt đầu từ đâu.

Cách tốt nhất để cải thiện khả năng giải quyết vấn đề là tích lũy kinh nghiệm bằng cách xây dựng thật nhiều chương trình. Bạn càng luyện tập nhiều thì càng sẵn sàng hơn để giải quyết các vấn đề trong thực tế.

Trong bài học này, chúng ta sẽ lần lượt tìm hiểu một vài kỹ thuật có thể hỗ trợ quá trình giải quyết vấn đề.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Giải thích ba bước trong quá trình giải quyết vấn đề.
- Giải thích pseudocode là gì và có thể sử dụng nó để giải quyết vấn đề.
- Có thể chia một vấn đề thành các vấn đề con.

### Hiểu vấn đề

Bước đầu tiên để giải quyết một vấn đề là hiểu chính xác vấn đề đó là gì. Nếu không hiểu vấn đề, bạn sẽ không biết khi nào mình đã giải quyết thành công và có thể lãng phí rất nhiều thời gian cho một lời giải sai.

Để làm rõ và hiểu vấn đề, hãy viết nó ra giấy, diễn đạt lại bằng tiếng Anh thông thường cho đến khi bạn thấy nó có ý nghĩa, và vẽ sơ đồ nếu điều đó hữu ích. Khi có thể giải thích vấn đề cho người khác bằng tiếng Anh thông thường, nghĩa là bạn đã hiểu nó.

### Lập kế hoạch

Giờ bạn đã biết mình cần giải quyết điều gì, nhưng đừng vội viết mã. Trước hết, đã đến lúc lên kế hoạch xem bạn sẽ giải quyết vấn đề đó như thế nào.

Ở giai đoạn này, bạn nên trả lời một số câu hỏi sau:

- Chương trình của bạn có giao diện người dùng không? Nó sẽ trông như thế nào? Giao diện sẽ có những chức năng gì? Hãy phác thảo trên giấy.
- Chương trình sẽ nhận những đầu vào nào? Người dùng sẽ nhập dữ liệu hay bạn sẽ lấy đầu vào từ nơi khác?
- Đầu ra mong muốn là gì?
- Với những đầu vào đó, cần thực hiện các bước nào để trả về đầu ra mong muốn?

Câu hỏi cuối cùng là nơi bạn sẽ viết ra một thuật toán để giải quyết vấn đề. Bạn có thể hình dung thuật toán như một công thức để giải một vấn đề cụ thể. Nó xác định các bước máy tính cần thực hiện để giải quyết vấn đề bằng pseudocode.

### Pseudocode

Pseudocode là việc viết logic của chương trình bằng ngôn ngữ tự nhiên thay vì mã nguồn. Nó giúp bạn chậm lại và suy nghĩ kỹ về các bước mà chương trình phải thực hiện để giải quyết vấn đề.

Sau đây là một ví dụ về pseudocode cho một chương trình in ra tất cả các số cho đến số người dùng nhập:

```text
When the user inputs a number
Initialize a counter variable and set its value to zero
While counter is smaller than user inputted number increment the counter by one
Print the value of the counter variable
```

Đây là một chương trình cơ bản nhằm minh họa hình thức của pseudocode. Trong phần bài tập sẽ có thêm nhiều ví dụ về pseudocode.

### Chia để trị

Từ quá trình lập kế hoạch, bạn hẳn đã xác định được một số vấn đề con của vấn đề lớn đang cần giải quyết. Mỗi bước trong thuật toán đã viết ở phần trước đều là một vấn đề con. Hãy chọn vấn đề nhỏ nhất hoặc đơn giản nhất và bắt đầu viết mã từ đó.

Điều quan trọng cần nhớ là có thể bạn chưa biết ngay từ đầu tất cả các bước cần thực hiện, vì vậy thuật toán của bạn có thể chưa hoàn chỉnh — điều đó không sao. Bắt đầu và giải quyết một trong những vấn đề con đã xác định ở giai đoạn lập kế hoạch thường sẽ hé lộ vấn đề con tiếp theo mà bạn có thể xử lý. Hoặc nếu bạn đã biết vấn đề con tiếp theo là gì, việc giải quyết vấn đề con đầu tiên thường sẽ khiến nó trở nên đơn giản hơn.

Nhiều người mới học cố giải quyết vấn đề lớn chỉ trong một lần. **Đừng làm vậy**. Nếu vấn đề đủ phức tạp, bạn sẽ tự làm mình rối tung và khiến mọi việc khó khăn hơn nhiều. Phân rã vấn đề thành những vấn đề con nhỏ hơn, dễ giải quyết hơn là một cách tiếp cận tốt hơn nhiều. Phân rã là cách chủ yếu để xử lý sự phức tạp, giúp vấn đề trở nên dễ tiếp cận và dễ hiểu hơn.

Tóm lại, hãy chia vấn đề lớn thành các phần nhỏ rồi giải quyết từng phần cho đến khi giải quyết được toàn bộ vấn đề lớn.

### Giải bài Fizz Buzz

Để minh họa quy trình này trong thực tế, hãy cùng giải bài [Fizz Buzz](https://en.wikipedia.org/wiki/Fizz_buzz)

#### Hiểu vấn đề

> Viết một chương trình nhận đầu vào từ người dùng và in các số từ một đến số người dùng đã nhập. Tuy nhiên, với các bội số của ba, hãy in `Fizz` thay cho số; với các bội số của năm, hãy in `Buzz`. Với các số đồng thời là bội số của cả ba và năm, hãy in `FizzBuzz`.

Đây là bức tranh tổng thể của vấn đề chúng ta sẽ giải quyết. Tuy nhiên, chúng ta luôn có thể làm cho nó rõ ràng hơn bằng cách diễn đạt lại.

Viết một chương trình cho phép người dùng nhập một số, in từng số từ một đến số người dùng đã nhập; nhưng với những số chia hết cho 3, hãy in `Fizz` thay thế. Với những số chia hết cho 5, hãy in `Buzz`, và cuối cùng với những số chia hết cho cả 3 và 5, hãy in `FizzBuzz`.

#### Lập kế hoạch

Chương trình của bạn có giao diện không? Nó sẽ trông như thế nào?
Chương trình FizzBuzz của chúng ta sẽ chạy trong console của trình duyệt, nên không cần giao diện. Tương tác duy nhất với người dùng là cho phép họ nhập một số.

Chương trình sẽ nhận những đầu vào nào? Người dùng sẽ nhập dữ liệu hay bạn sẽ lấy đầu vào từ nơi khác?
Người dùng sẽ nhập một số thông qua prompt (hộp thoại bật lên).

Đầu ra mong muốn là gì?
Đầu ra mong muốn là danh sách các số từ 1 đến số người dùng đã nhập. Tuy nhiên, mỗi số chia hết cho 3 sẽ cho ra `Fizz`, mỗi số chia hết cho 5 sẽ cho ra `Buzz`, và mỗi số chia hết cho cả 3 và 5 sẽ cho ra `FizzBuzz`.

#### Viết pseudocode

Cần thực hiện những bước nào để trả về đầu ra mong muốn?
Sau đây là một thuật toán dưới dạng pseudocode cho vấn đề này:

```text
When a user inputs a number
Loop from 1 to the entered number
If the current number is divisible by 3 then print "Fizz"
If the current number is divisible by 5 then print "Buzz"
If the current number is divisible by 3 and 5 then print "FizzBuzz"
Otherwise print the current number
```

#### Chia để trị

Như có thể thấy từ thuật toán đã xây dựng, vấn đề con đầu tiên chúng ta có thể giải quyết là nhận đầu vào từ người dùng. Vì vậy, hãy bắt đầu từ đó và xác minh rằng nó hoạt động bằng cách in ra số đã nhập.

Với JavaScript, chúng ta sẽ sử dụng phương thức "prompt".

```javascript
let answer = parseInt(prompt("Please enter the number you would like to FizzBuzz up to: "));
```

Đoạn mã trên sẽ tạo một hộp thoại nhỏ yêu cầu người dùng nhập một số. Đầu vào nhận được sẽ được lưu trong biến `answer`.

<div class="lesson-note lesson-note--tip" markdown=1>

#### Sử dụng hàm parseInt

Chúng ta bọc lời gọi prompt trong hàm `parseInt` để giá trị trả về từ đầu vào của người dùng là một số.

</div>

Sau khi hoàn tất, hãy chuyển sang vấn đề con tiếp theo: "Lặp từ 1 đến số đã nhập". Có nhiều cách thực hiện điều này trong JavaScript. Một cách phổ biến - cũng xuất hiện trong nhiều ngôn ngữ khác như Java, C++ và Ruby - là dùng [vòng lặp for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for):

```javascript
let answer = parseInt(prompt("Please enter the number you would like to FizzBuzz up to: "));

for (let i = 1; i <= answer; i++) {
  console.log(i);
}
```

Nếu bạn chưa từng thấy cách viết này và thấy nó kỳ lạ, thực ra nó khá đơn giản. Chúng ta khai báo một biến `i` và gán cho nó giá trị 1: giá trị ban đầu của biến `i` trong vòng lặp. Mệnh đề thứ hai, `i <= answer`, là điều kiện của chúng ta. Ta muốn lặp cho đến khi `i` lớn hơn `answer`. Mệnh đề thứ ba, `i++`, yêu cầu vòng lặp tăng `i` thêm 1 sau mỗi lần lặp. Vì vậy, nếu người dùng nhập 10, vòng lặp này sẽ in các số từ 1 đến 10 ra console.

<div class="lesson-note lesson-note--tip" markdown=1>

#### Bắt đầu vòng lặp từ 1

Phần lớn thời gian, lập trình viên sẽ lặp từ 0. Do nhu cầu của chương trình này, chúng ta bắt đầu từ 1.

</div>

Khi phần này đã hoạt động, hãy chuyển sang vấn đề tiếp theo: Nếu số hiện tại chia hết cho 3 thì in `Fizz`.

```javascript
let answer = parseInt(prompt("Please enter the number you would like to FizzBuzz up to: "));

for (let i = 1; i <= answer; i++) {
  if (i % 3 === 0) {
    console.log("Fizz");
  } else {
    console.log(i);
  }
}
```

Ở đây, chúng ta dùng toán tử modulo (`%`) để chia số hiện tại cho ba. Nếu bạn còn nhớ bài học trước, toán tử modulo trả về phần dư của phép chia. Vì vậy, nếu phép chia trả về phần dư bằng 0, điều đó có nghĩa là số `current` chia hết cho 3.

Sau thay đổi này, khi chạy chương trình với đầu vào của người dùng là 10, đầu ra sẽ là:

```bash
1
2
Fizz
4
5
Fizz
7
8
Fizz
10
```

Chương trình đã bắt đầu thành hình. Một vài vấn đề con cuối cùng sẽ dễ giải quyết, vì cấu trúc cơ bản đã có sẵn và chúng chỉ là những biến thể khác của điều kiện hiện tại. Hãy xử lý vấn đề tiếp theo: Nếu số hiện tại chia hết cho 5 thì in `Buzz`.

```javascript
let answer = parseInt(prompt("Please enter the number you would like to FizzBuzz up to: "));

for (let i = 1; i <= answer; i++) {
  if (i % 3 === 0) {
    console.log("Fizz");
  } else if (i % 5 === 0) {
    console.log("Buzz");
  } else {
    console.log(i);
  }
}
```

Khi chạy chương trình lúc này, nếu người dùng nhập 10, bạn sẽ thấy đầu ra sau:

```bash
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
```

Chúng ta còn một vấn đề con nữa cần giải quyết để hoàn thiện chương trình: Nếu số hiện tại chia hết cho cả 3 và 5 thì in `FizzBuzz`.

```javascript
let answer = parseInt(prompt("Please enter the number you would like to FizzBuzz up to: "));

for (let i = 1; i <= answer; i++) {
  if (i % 3 === 0 && i % 5 === 0) {
    console.log("FizzBuzz");
  } else if (i % 3 === 0) {
    console.log("Fizz");
  } else if (i % 5 === 0) {
    console.log("Buzz");
  } else {
    console.log(i);
  }
}
```

Chúng ta phải sắp xếp lại các điều kiện một chút để chương trình hoạt động. Điều kiện đầu tiên giờ kiểm tra xem `i` có chia hết cho 3 và 5 hay không, thay vì chỉ kiểm tra `i` có chia hết cho 3 hay không. Sở dĩ phải làm vậy là vì nếu giữ nguyên cách cũ, chương trình sẽ chạy điều kiện đầu tiên `if (i % 3 === 0)`: khi `i` chia hết cho 3, nó sẽ in `Fizz` rồi chuyển sang số tiếp theo trong vòng lặp, ngay cả khi `i` cũng chia hết cho 5.

Khi điều kiện `if (i % 3 === 0 && i % 5 === 0)` được đặt trước, chúng ta kiểm tra xem `i` có chia hết cho cả 3 và 5 hay không trước khi chuyển sang kiểm tra riêng xem nó có chia hết cho 3 hoặc 5 trong các điều kiện `else if` hay không.

Chương trình đã hoàn tất! Nếu chạy ngay bây giờ với đầu vào của người dùng là 20, bạn sẽ nhận được đầu ra sau:

```bash
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
16
17
Fizz
19
Buzz
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc bài [Cách suy nghĩ như một lập trình viên - Bài học về giải quyết vấn đề](https://www.freecodecamp.org/news/how-to-think-like-a-programmer-lessons-in-problem-solving-d1d8bf1de7d2/) của Richard Reis.
  1. Xem bài [Bắt đầu suy nghĩ như một lập trình viên như thế nào](https://www.youtube.com/watch?v=azcrPFhaY9k) của Coding Tech. Video dài một giờ nhưng chứa rất nhiều thông tin và chắc chắn đáng để bạn dành thời gian xem.
  1. Đọc bài viết [Pseudocode: Nó là gì và cách viết](https://www.builtin.com/data-science/pseudocode) từ Built In.

</div>