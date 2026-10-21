### Giới thiệu

Bạn đã làm được rồi! Đến đây, hẳn bạn đã nắm rất vững *các nền tảng* của JavaScript. Tất nhiên vẫn còn rất nhiều điều cần học, nhưng ở thời điểm này bạn đã có thể tạo ra kha khá thứ. Dự án cuối cùng của chúng ta sẽ kết hợp mọi thứ bạn đã học: bạn sẽ tạo một chiếc máy tính hiển thị trên màn hình bằng JavaScript, HTML và CSS.

Như thường lệ, một số phần của dự án này sẽ không hề đơn giản, nhưng nếu bạn đã theo sát khóa học từ đầu, chắc chắn bạn có đủ mọi thứ cần thiết để hoàn thành. Chúng tôi sẽ hướng dẫn các bước khác nhau mà bạn có thể thực hiện, nhưng một lần nữa, cách bạn triển khai cụ thể hoàn toàn do bạn quyết định!

<div class="lesson-note lesson-note--warning" markdown="1">

#### Cảnh báo về eval() và new Function()

Trước khi bắt tay vào dự án, chúng ta cần nói qua một lời cảnh báo. Khi tìm hiểu cách đánh giá các biểu thức toán học phức tạp trong JavaScript, bạn có thể bắt gặp hàm [`eval()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval) đầy hấp dẫn. Tuy nhiên, hàm này có thể rất nguy hiểm và MDN đã giải thích cặn kẽ vì sao bạn [không bao giờ nên dùng eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_direct_eval!)! Trong dự án này, bạn sẽ cần tự xây dựng các hàm để đánh giá biểu thức. Tương tự, khi tìm hiểu cách tính các biểu thức, bạn có thể gặp những lời giải đề xuất trả về một `new Function()` để đánh giá một chuỗi. Cũng như `eval()`, bạn không nên làm vậy [do những rủi ro tiềm ẩn khi đánh giá dữ liệu không an toàn](https://stackoverflow.com/questions/4599857/are-eval-and-new-function-the-same-thing). Hơn nữa, dùng những lời giải làm hết mọi việc thay bạn thì còn gì vui? Bắt tay vào thôi!

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

Đừng quên commit sớm và thường xuyên! Bạn có thể xem lại bài học [Thông điệp commit](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/commit-messages) tại đây!

Dưới đây là một số trường hợp sử dụng (các yêu cầu đối với dự án của bạn):

1. Máy tính của bạn sẽ có các hàm cho mọi phép toán cơ bản thường thấy trên máy tính, vì vậy hãy bắt đầu bằng cách tạo các hàm cho những phép toán sau và kiểm thử chúng trong console của trình duyệt:
   - cộng
   - trừ
   - nhân
   - chia
1. Một phép tính trên máy tính sẽ gồm một số, một toán tử và một số khác. Ví dụ: 3 + 5. Hãy tạo ba biến, mỗi biến tương ứng với một phần của phép tính. Sau này bạn sẽ dùng các biến này để cập nhật phần hiển thị.
1. Tạo một hàm mới `operate`, nhận vào một toán tử và hai số, sau đó gọi một trong các hàm ở trên với hai số đó.
1. Tạo một máy tính HTML cơ bản với các nút cho từng chữ số và toán tử (bao gồm cả `=`).
   - Chưa cần lo việc làm cho chúng hoạt động.
   - Máy tính cũng cần có phần hiển thị. Hãy điền thử một vài số giả để giao diện trông đúng như mong muốn.
   - Thêm một nút "clear".
1. Tạo các hàm cập nhật một trong những biến số khi người dùng nhấn các nút chữ số của máy tính. Phần hiển thị cũng phải cập nhật để phản ánh giá trị của biến số đó.
1. Làm cho máy tính hoạt động! Bạn cần lưu số thứ nhất và số thứ hai do người dùng nhập, sau đó gọi `operate()` với chúng khi người dùng nhấn nút `=`, theo toán tử đã được chọn giữa hai số.
   - Bạn hẳn đã có sẵn mã để điền nội dung vào phần hiển thị, vì vậy sau khi gọi `operate`, hãy cập nhật phần hiển thị bằng kết quả của phép tính.
   - Đây là phần khó nhất của dự án. Bạn cần tìm cách lưu tất cả giá trị và gọi hàm `operate` với các giá trị đó. Đừng ngại nếu bạn mất một lúc mới tìm ra logic.
1. Các lỗi dễ gặp: hãy chú ý và sửa những lỗi sau nếu chúng xuất hiện trong mã của bạn:
   - **Máy tính không được đánh giá nhiều hơn một cặp số tại một thời điểm.** Ví dụ, máy tính của bạn nên hoạt động như sau:
     1. Nhập một số (`12`).
     1. Nhập một toán tử (`+`).
     1. Nhập số thứ hai (`7`).
     1. Nhập toán tử thứ hai (`-`). Lúc này, máy tính phải tính cặp số ban đầu (`12 + 7`), rồi hiển thị kết quả (`19`).
     1. Nhập một số khác (`1`).
     1. Nhập một toán tử khác hoặc dấu bằng (`=`). Lúc này, máy tính phải dùng kết quả trước đó (`19`) làm số thứ nhất, dùng toán tử (`-`) và số mới (`1`) để tính phương trình mới `19 - 1`. Bạn sẽ thấy kết quả (`18`) trên màn hình.

     - Để xem quy trình này hoạt động như thế nào, bạn có thể nhập phương trình vừa nêu `12 + 7 - 1 =` vào [máy tính trực tuyến](https://www.calculatorsoup.com/calculators/math/basic.php) này.
   - Bạn nên làm tròn các kết quả có phần thập phân dài để chúng không tràn khỏi phần hiển thị.
   - Nhấn `=` trước khi nhập đủ các số hoặc toán tử có thể gây ra vấn đề!
   - Nhấn "clear" phải xóa mọi dữ liệu hiện có. Hãy đảm bảo người dùng thực sự bắt đầu lại từ đầu sau khi nhấn "clear".
   - Hiển thị một thông báo lỗi có chút mỉa mai nếu người dùng cố chia cho 0... và đừng để nó làm máy tính của bạn bị lỗi!
   - Hãy đảm bảo máy tính chỉ thực hiện phép tính khi người dùng đã cung cấp hai số và một toán tử. Ví dụ: bạn nhập một số (`2`), sau đó nhấn nút toán tử (`+`). Bạn lại nhấn nút toán tử (`+`) liên tiếp lần thứ hai. Máy tính không được đánh giá trường hợp này thành (`2 + 2`) và không được hiển thị kết quả (`4`). Nếu người dùng nhấn các nút toán tử liên tiếp, máy tính không được thực hiện phép tính nào; nó chỉ nên lấy toán tử cuối cùng để dùng cho phép tính tiếp theo.
   - Khi một kết quả đang được hiển thị, nhấn một chữ số mới phải xóa kết quả và bắt đầu một phép tính mới thay vì nối chữ số đó vào kết quả hiện tại. Hãy kiểm tra xem máy tính của bạn có làm đúng như vậy không!

#### Điểm cộng thêm

- Người dùng có thể nhận được số dấu phẩy động nếu thực hiện phép tính cần đến chúng, nhưng hiện chưa thể tự nhập số thập phân. Hãy thêm nút `.` và cho phép người dùng nhập số thập phân! Tuy nhiên, đừng cho phép họ nhập nhiều hơn một dấu, chẳng hạn như: `12.3.56.5`. Vô hiệu hóa nút `.` nếu phần hiển thị đã có dấu phân cách thập phân.
- Thêm nút "backspace" để người dùng có thể hoàn tác lần nhập gần nhất nếu nhấn nhầm số.
- Thêm hỗ trợ bàn phím!

</div>