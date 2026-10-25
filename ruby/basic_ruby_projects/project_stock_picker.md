### Giới thiệu

Đến lúc này, bạn hẳn đã khá tự tin khi làm việc với chuỗi, mảng và hash. Trong vài bài học tiếp theo, chúng ta sẽ giải một số bài toán kinh điển dành cho phần "nhập môn lập trình" để củng cố kiến thức và làm quen với việc xây dựng các dự án nhỏ. Bạn sẽ cần vận dụng một số kiến thức cơ bản về câu lệnh điều kiện và điều khiển luồng trong phần chuẩn bị, nhưng không cần làm gì mà bạn chưa từng gặp trước đó.

<div class="lesson-note" markdown="1">

#### Các cách khác nhau để viết và chạy mã Ruby

Nếu muốn thực sự viết và chạy mã Ruby của riêng mình, bạn có thể dùng IRB từ dòng lệnh (gõ `irb` sau dấu nhắc), chạy mã từ một tệp script bằng `$ ruby ./your_file_name_in_the_current_directory.rb`, hoặc dùng bất kỳ REPL phù hợp nào khác.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy triển khai một phương thức `#stock_picker` nhận vào một mảng giá cổ phiếu, mỗi giá ứng với một ngày giả định. Phương thức cần trả về một cặp ngày tương ứng với ngày tốt nhất để mua và ngày tốt nhất để bán. Các ngày bắt đầu từ 0.

   ```ruby
     > stock_picker([17,3,6,9,15,8,6,1,10])
     => [1,4]  # for a profit of $15 - $3 == $12
   ```

   **Mẹo nhanh:**

   - Bạn cần mua trước khi có thể bán
   - Hãy chú ý đến những trường hợp đặc biệt, chẳng hạn khi ngày có giá thấp nhất là ngày cuối cùng hoặc ngày có giá cao nhất là ngày đầu tiên.

</div>
