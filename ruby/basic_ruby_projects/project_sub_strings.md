### Giới thiệu

Đến lúc này, bạn hẳn đã khá tự tin khi làm việc với chuỗi, mảng và hash. Trong vài bài học tiếp theo, chúng ta sẽ giải một số bài toán kinh điển dành cho phần "nhập môn lập trình" để củng cố kiến thức và làm quen với việc xây dựng các dự án nhỏ. Bạn sẽ cần vận dụng một số kiến thức cơ bản về câu lệnh điều kiện và điều khiển luồng trong phần chuẩn bị, nhưng không cần làm gì mà bạn chưa từng gặp trước đó.

<div class="lesson-note" markdown="1">

#### Các cách khác nhau để viết và chạy mã Ruby

Nếu muốn thực sự viết và chạy mã Ruby của riêng mình, bạn có thể dùng IRB từ dòng lệnh (gõ `irb` sau dấu nhắc), chạy mã từ một tệp script bằng `$ ruby ./your_file_name_in_the_current_directory.rb`, hoặc dùng bất kỳ REPL phù hợp nào khác.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy triển khai một phương thức `#substrings` nhận một từ làm đối số đầu tiên, sau đó nhận một mảng các chuỗi con hợp lệ (từ điển của bạn) làm đối số thứ hai. Phương thức cần trả về một hash liệt kê từng chuỗi con (không phân biệt chữ hoa chữ thường) được tìm thấy trong chuỗi ban đầu và số lần chuỗi đó xuất hiện.

   ```ruby
     > dictionary = ["below","down","go","going","horn","how","howdy","it","i","low","own","part","partner","sit"]
     => ["below","down","go","going","horn","how","howdy","it","i","low","own","part","partner","sit"]
     > substrings("below", dictionary)
     => { "below" => 1, "low" => 1 }
   ```

   Tiếp theo, hãy đảm bảo phương thức của bạn có thể xử lý nhiều từ:

   ```ruby
     > substrings("Howdy partner, sit down! How's it going?", dictionary)
     => { "down" => 1, "go" => 1, "going" => 1, "how" => 2, "howdy" => 1, "it" => 2, "i" => 3, "own" => 1, "part" => 1, "partner" => 1, "sit" => 1 }
   ```

   **Mẹo nhanh:**

   - Hãy nhớ cách chuyển chuỗi thành mảng và mảng thành chuỗi.

</div>
