### Giới thiệu

Trong dự án này, bạn sẽ tạo trò chơi [Kéo, Búa, Bao](https://www.wikihow.com/Play-Rock,-Paper,-Scissors). Trò chơi này sẽ được chơi hoàn toàn trong console.

<div class="lesson-note" markdown="1">

#### Chưa có GUI

Một số lời giải của học viên bên dưới có nút, văn bản và các phần tử khác. Những phần tử này thuộc về thứ được gọi là giao diện người dùng đồ họa (GUI). Bạn sẽ tạo GUI ở một bài học sau. Trong lúc này, hãy nhớ commit mã của mình lên GitHub.

</div>

### Cách tiếp cận để giải quyết vấn đề

Vì đây là dự án JavaScript đầu tiên được xây dựng từ đầu, điều quan trọng là nhớ những lời khuyên sáng suốt từ [bài học Giải quyết vấn đề](https://www.theodinproject.com/lessons/foundations-problem-solving). Với mỗi bước trong dự án này, hãy nhớ thực hiện những việc sau:

1. Lập kế hoạch hoặc viết mã giả cho lời giải.
1. Viết mã.
1. Kiểm thử mã để đảm bảo nó hoạt động.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Tập trung vào nhiệm vụ trước mắt

Khi tạo các dự án tương tác như dự án này, bạn có thể bị cám dỗ thêm nhiều tính năng, cải thiện tính tương tác, trải nghiệm người dùng, thiết kế và kiểu dáng của trang web, vân vân.

Chúng tôi khuyên bạn không nên làm vậy, mà hãy dành công sức này cho các dự án portfolio. Để biết thêm thông tin, hãy đọc bài ["Xây dựng portfolio một cách chiến lược" trong "Trở thành một câu chuyện thành công của TOP"](https://dev.to/theodinproject/strategically-building-your-portfolio-1km4).

</div>

Hãy nhớ commit sớm và thường xuyên! Để ôn lại, hãy xem [bài học về thông điệp commit](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/commit-messages).

#### Bước 1: Thiết lập cấu trúc dự án

1. Tạo một kho Git mới cho dự án.
1. Tạo một tài liệu HTML trống có thẻ script.
1. Kiểm tra xem JavaScript đã được liên kết đúng chưa:
   - Viết `console.log("Hello World")` trong JavaScript.
   - Sau khi mở trang web, kiểm tra xem "Hello World" có được ghi trong console của trình duyệt hay không.

Cách làm tốt nhất là liên kết đến một tệp JavaScript bên ngoài bên trong thẻ script này. Dùng tệp JavaScript bên ngoài giúp tệp HTML của bạn sạch sẽ và có tổ chức hơn.

Bạn không cần viết thêm mã nào trong tệp HTML. Trò chơi này được chơi hoàn toàn thông qua console.

#### Bước 2: Viết logic để lấy lựa chọn của máy tính

Bạn sẽ chơi với máy tính. Bạn sẽ viết một hàm trả về ngẫu nhiên "rock", "paper" hoặc "scissors".

1. Tạo một hàm mới có tên `getComputerChoice`.
1. Viết mã để `getComputerChoice` ngẫu nhiên `return` một trong các giá trị chuỗi sau: "rock", "paper" hoặc "scissors".
   - **Gợi ý**: Phương thức [Math.random](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) trả về một số ngẫu nhiên lớn hơn hoặc bằng 0 và nhỏ hơn 1. Hãy suy nghĩ xem bạn có thể dùng điều này thế nào để trả về có điều kiện một trong nhiều lựa chọn.
   - **Lưu ý**: Một số học viên có thể nghĩ rằng cần dùng mảng cho bài toán này, nhưng không phải vậy. Nếu bạn chưa quen với mảng, đừng lo - chúng sẽ được đề cập ở phần sau của chương trình học.
1. Trước khi chuyển sang bước tiếp theo, hãy kiểm tra để đảm bảo hàm trả về đúng như mong đợi bằng `console.log` hoặc [công cụ dành cho nhà phát triển của trình duyệt](https://www.theodinproject.com/lessons/foundations-javascript-developer-tools).

#### Bước 3: Viết logic để lấy lựa chọn của người chơi

Trò chơi sẽ do một người chơi thực hiện. Bạn sẽ viết một hàm nhận lựa chọn của người dùng và trả về lựa chọn đó.

1. Tạo một hàm mới có tên `getHumanChoice`.
1. Viết mã để `getHumanChoice` trả về một lựa chọn hợp lệ tùy thuộc vào nội dung người dùng nhập.
   - **Gợi ý**: Dùng phương thức [prompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt) để lấy dữ liệu người dùng nhập.
   - Bạn không cần xử lý việc hỏi lại nếu người dùng nhập dữ liệu không hợp lệ, vì việc đó cần đến những nội dung chúng ta sẽ dạy sau. Hiện tại, cứ giả định rằng người dùng luôn nhập một lựa chọn hợp lệ.
1. Kiểm tra giá trị hàm trả về bằng cách dùng `console.log`.

#### Bước 4: Khai báo các biến điểm số của người chơi

Trò chơi sẽ theo dõi điểm số của người chơi. Bạn sẽ viết các biến để theo dõi điểm số đó.

1. Tạo hai biến mới có tên `humanScore` và `computerScore` trong phạm vi global.
1. Khởi tạo các biến đó với giá trị `0`.

#### Bước 5: Viết logic để chơi một vòng

Trò chơi sẽ diễn ra theo từng vòng. Bạn sẽ viết một hàm nhận lựa chọn của người chơi và máy tính làm đối số, chơi một vòng, tăng điểm cho người thắng vòng đó và ghi thông báo người thắng.

1. Tạo một hàm mới có tên `playRound`.
1. Định nghĩa hai tham số cho `playRound`: `humanChoice` và `computerChoice`. Dùng hai tham số này để nhận lựa chọn của người chơi và máy tính làm đối số.
1. Làm cho tham số `humanChoice` của hàm không phân biệt chữ hoa chữ thường để người chơi có thể nhập "rock", "ROCK", "RocK" hoặc các biến thể khác.
1. Viết mã cho hàm `playRound` để `console.log` một giá trị chuỗi thể hiện người thắng vòng, chẳng hạn: "You lose! Paper beats Rock".
1. Tăng biến `humanScore` hoặc `computerScore` dựa trên người thắng vòng.

Mã ví dụ:

```javascript
function playRound(humanChoice, computerChoice) {
  // your code here!
}

const humanSelection = getHumanChoice();
const computerSelection = getComputerChoice();

playRound(humanSelection, computerSelection);
```

#### Bước 6: Viết logic để chơi toàn bộ trò chơi

Trò chơi sẽ có 5 vòng. Bạn sẽ viết một hàm có tên `playGame`, hàm này gọi `playRound` để chơi 5 vòng, theo dõi điểm số và công bố người thắng ở cuối.

1. Tạo một hàm mới có tên `playGame`.
1. Di chuyển hàm `playRound` và các biến điểm số để chúng được khai báo bên trong hàm `playGame` mới.
1. Chơi 5 vòng bằng cách gọi `playRound` 5 lần.
   - **Gợi ý**: Khi bạn gán một lời gọi hàm cho một biến, giá trị trả về của lời gọi hàm đó được gán cho biến. Việc truy cập biến sau đó chỉ cung cấp giá trị đã gán; nó không gọi lại hàm. Bạn cần gọi lại các hàm lựa chọn để lấy lựa chọn mới cho mỗi vòng.
   - Nếu cần, hãy làm lại các hàm trước đó hoặc tạo thêm các hàm trợ giúp. Cụ thể, bạn có thể muốn thay đổi các giá trị trả về để chúng hữu ích hơn.
   - Nếu đã biết vòng lặp, bạn có thể dùng chúng. Nếu chưa biết thì cũng đừng lo! Vòng lặp sẽ được đề cập trong bài học tiếp theo.

</div>