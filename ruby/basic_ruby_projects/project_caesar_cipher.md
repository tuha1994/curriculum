### Giới thiệu

Đến lúc này, hẳn bạn đã khá tự tin khi làm việc với chuỗi, mảng và hash. Trong vài bài học tiếp theo, chúng ta sẽ thực hiện một số bài toán kinh điển dành cho phần "nhập môn lập trình" để củng cố kiến thức của bạn và làm quen với việc xây dựng các dự án nhỏ. Bạn sẽ cần dựa vào một phần kiến thức cơ bản về câu lệnh điều kiện và điều khiển luồng từ phần chuẩn bị, nhưng sẽ không cần làm điều gì mà bạn chưa từng thấy trước đây.

<div class="lesson-note" markdown="1">

#### Các cách khác nhau để viết và chạy mã Ruby

Nếu muốn thực sự viết và chạy mã Ruby của riêng mình, bạn có thể sử dụng IRB từ dòng lệnh (gõ `irb` sau dấu nhắc), chạy mã từ một tệp script bằng `$ ruby ./your_file_name_in_the_current_directory.rb`, hoặc sử dụng bất kỳ REPL phù hợp nào khác.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

Theo Wikipedia:

> Trong lĩnh vực mật mã học, mật mã Caesar, còn được gọi là mật mã của Caesar, mật mã dịch chuyển, mã Caesar hoặc phép dịch Caesar, là một trong những kỹ thuật mã hóa đơn giản và phổ biến nhất. Đây là một dạng mật mã thay thế, trong đó mỗi chữ cái trong bản rõ được thay thế bằng một chữ cái nằm cách đó một số vị trí cố định về phía dưới trong bảng chữ cái. Ví dụ, với phép dịch sang trái 3 vị trí, D sẽ được thay bằng A, E sẽ trở thành B, và cứ tiếp tục như vậy. Phương pháp này được đặt theo tên Julius Caesar, người đã sử dụng nó trong thư từ cá nhân của mình.

Lớp học CS50 của Harvard có một [video về mật mã Caesar](https://www.youtube.com/watch?v=36xNpbosfTY).

1. Xây dựng một mật mã Caesar nhận vào một chuỗi và hệ số dịch chuyển, sau đó xuất ra chuỗi đã được biến đổi bằng cách dịch sang phải:

   ```ruby
     > caesar_cipher("What a string!", 5)
     => "Bmfy f xywnsl!"
   ```

   **Một vài gợi ý nhanh:**

   - Bạn sẽ cần nhớ cách chuyển một chuỗi thành một số.
   - Đừng quên quay vòng từ `z` về `a`.
   - Đừng quên giữ nguyên kiểu chữ.
   - Đoạn trích Wikipedia nói về mật mã Caesar sử dụng phép dịch sang trái.

</div>
