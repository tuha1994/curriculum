### Giới thiệu

Tệp là một cách tuyệt vời để lưu và tải lại một trò chơi, vì vậy ở đây chúng ta sẽ thử xây dựng [Hangman](https://www.wikihow.com/Play-Hangman).

### Bài tập

Bạn sẽ xây dựng một trò chơi Hangman chạy trên dòng lệnh, trong đó một người chơi đấu với máy tính, nhưng trò chơi sẽ nâng cao hơn một chút.

<div class="lesson-content__panel" markdown="1">

  1. Tải tệp từ điển `google-10000-english-no-swears.txt` từ kho GitHub của first20hours [google-10000-english](https://raw.githubusercontent.com/first20hours/google-10000-english/master/google-10000-english-no-swears.txt).
  1. Khi bắt đầu một ván chơi mới, tập lệnh của bạn phải tải từ điển và chọn ngẫu nhiên một từ dài từ 5 đến 12 ký tự làm từ bí mật.
  1. Bạn không cần vẽ một hình người que thật sự (dù muốn thì vẫn có thể!), nhưng hãy hiển thị một dạng bộ đếm nào đó để người chơi biết mình còn được đoán sai bao nhiêu lần trước khi trò chơi kết thúc. Bạn cũng cần hiển thị những chữ cái đúng đã được chọn (cùng vị trí của chúng trong từ, chẳng hạn như `_ r o g r a _ _ i n g`) và những chữ cái sai đã được chọn.
  1. Ở mỗi lượt, cho phép người chơi đoán một chữ cái. Việc đoán phải không phân biệt chữ hoa và chữ thường. Cập nhật phần hiển thị để phản ánh chữ cái đó đúng hay sai. Nếu hết lượt đoán, người chơi sẽ thua.
  1. Bây giờ hãy triển khai chức năng cho phép người chơi, vào đầu bất kỳ lượt nào, chọn lưu trò chơi thay vì đoán. Hãy nhớ những gì bạn đã học về tuần tự hóa đối tượng.
  1. Khi chương trình khởi động lần đầu, hãy thêm một tùy chọn cho phép người chơi mở một trong các trò chơi đã lưu; trò chơi phải đưa bạn trở lại chính xác trạng thái tại thời điểm bạn lưu. Tiếp tục chơi nhé!

</div>