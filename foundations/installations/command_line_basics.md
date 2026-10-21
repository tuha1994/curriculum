### Giới thiệu

Bạn cảm thấy sợ dòng lệnh ư? Bạn không cô độc đâu. Chúng ta thường hình dung các nhà phát triển chăm chú nhìn vào màn hình đen với những dòng chữ trắng hoặc xanh lá nhấp nháy, đồng thời gõ loạn những lệnh khó hiểu để đột nhập vào máy chủ của tập đoàn (chắc hẳn vừa tu nước ngọt vừa phủi bụi Cheetos màu cam neon khỏi bàn phím).

Màn hình hoặc cửa sổ trống có dấu nhắc và con trỏ nhấp nháy đó chính là giao diện dòng lệnh (CLI), nơi bạn có thể nhập các lệnh để máy tính thực hiện. Dù bạn không cần tái hiện cảnh tượng ở trên, làm việc với dòng lệnh là một kỹ năng thiết yếu mà bạn cần học với tư cách nhà phát triển. Dòng lệnh giống như căn cứ hoạt động của chúng ta, từ đó ta có thể khởi chạy các chương trình khác và tương tác với chúng. Dòng lệnh có cú pháp riêng cần học, nhưng vì bạn sẽ nhập đi nhập lại cùng một số lệnh hàng chục lần, bạn sẽ nhanh chóng ghi nhớ những lệnh mình dùng thường xuyên nhất.

Trong bài học nhập môn về dòng lệnh này, bạn sẽ học cách điều hướng máy tính và thao tác trực tiếp với các tệp và thư mục (còn gọi là folder) từ dòng lệnh. Bạn sẽ sớm thấy việc này không khó như mình tưởng. Những lệnh bạn học trong bài rất đơn giản, vì vậy đừng để việc lần đầu sử dụng dòng lệnh làm bạn thấy e ngại.

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về các chủ đề bạn sẽ học trong bài.

- Mô tả dòng lệnh là gì.
- Mở dòng lệnh trên máy tính.
- Dùng dòng lệnh để điều hướng các thư mục và hiển thị nội dung thư mục.
- Dùng dòng lệnh để tạo thư mục mới và tệp mới.
- Dùng dòng lệnh để đổi tên hoặc xóa thư mục và tệp.
- Dùng dòng lệnh để mở tệp hoặc thư mục bằng một chương trình.

### Thử dùng terminal

Mở terminal trên máy tính của bạn.

- **Linux**: Mở menu chương trình và tìm "Terminal". Bạn cũng có thể mở terminal bằng cách nhấn <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>T</kbd> trên bàn phím.

- **macOS**: Mở thư mục Applications > Utilities và tìm "Terminal". Bạn cũng có thể dùng tìm kiếm Spotlight để mở Terminal. Nhấn <kbd>Cmd</kbd> + <kbd>Space</kbd> để mở Spotlight rồi tìm "Terminal". Nhấn <kbd>Enter</kbd> để mở.

Cửa sổ mở ra hầu như sẽ trống, ngoài một số dòng chữ thay đổi tùy hệ điều hành. Trên Linux và các máy Mac đời cũ, dòng này sẽ kết thúc bằng `$`, còn trên các máy Mac đời mới sẽ kết thúc bằng `%`. Ký hiệu này — gọi là dấu nhắc — cho biết terminal đang chờ bạn nhập lệnh. Hãy thử ngay. Gõ `whoami` rồi nhấn <kbd>Enter</kbd>.

Lệnh sẽ trả về tên người dùng của bạn. Hay quá!

Thông thường, các hướng dẫn dùng terminal sẽ đặt ký hiệu này trước lệnh, chẳng hạn `$ whoami`. Điều đó có nghĩa là bạn cần gõ lệnh vào terminal, nhưng không gõ ký hiệu `$`. Và hãy nhớ rằng nếu bạn đang dùng máy Mac đời mới, `%` cũng tương đương với `$`.

#### Tại sao nên học điều này ngay bây giờ?

Bạn sẽ sử dụng dòng lệnh rất nhiều trong suốt chương trình học này, và dự án cài đặt sắp tới sẽ yêu cầu bạn cài nhiều chương trình phần mềm khác nhau bằng dòng lệnh. Ngoài ra, phần lớn thời gian bạn sẽ dùng Git trong dòng lệnh (sẽ tìm hiểu thêm về điều này sau). Nhìn rộng hơn, trong sự nghiệp phát triển phần mềm, rất có thể bạn sẽ sử dụng dòng lệnh hằng ngày, khiến đây trở thành một kỹ năng không thể thiếu trong bộ công cụ của bạn.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Nhập mật khẩu trong terminal

  Khi dùng một lệnh trong terminal yêu cầu bạn nhập mật khẩu để xác thực (chẳng hạn như sudo), bạn sẽ thấy các ký tự không hiển thị khi gõ. Dù terminal có vẻ không phản hồi, đừng lo!

  Đây là tính năng bảo mật nhằm bảo vệ thông tin bí mật, tương tự như cách các trường mật khẩu trên trang web dùng dấu hoa thị hoặc dấu chấm. Không hiển thị các ký tự bạn gõ giúp terminal giữ an toàn cho mật khẩu của bạn.

  Bạn vẫn cứ nhập mật khẩu như bình thường rồi nhấn Enter để gửi.

</div>

### Sử dụng dòng lệnh như một chuyên gia

Có một điều quan trọng bạn cần biết về các lập trình viên. Lập trình viên rất lười. Cực kỳ lười là đằng khác. Khi bị buộc phải làm đi làm lại một việc, họ thường sẽ tìm cách tự động hóa việc đó. Tin tốt là bạn được hưởng lợi từ vô số phím tắt mà họ đã tạo ra. Đã đến lúc học cách sử dụng dòng lệnh như một chuyên gia (hay nói cách khác là theo cách thật lười biếng).

Trước hết, có thể bạn đã nhận thấy việc sao chép và dán trong dòng lệnh không hoạt động như mong đợi. Khi đang ở trong dòng lệnh, hãy dùng <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> (Mac: <kbd>Cmd</kbd> + <kbd>C</kbd>) để sao chép và <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>V</kbd> (Mac: <kbd>Cmd</kbd> + <kbd>V</kbd>) để dán. Ví dụ, để sao chép và dán lệnh từ trình duyệt vào dòng lệnh, hãy bôi đen phần lệnh và dùng <kbd>Ctrl</kbd> + <kbd>C</kbd> như bình thường, sau đó dán vào terminal bằng <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>V</kbd>. Hãy thử xem!

Thứ hai, bạn cần tìm hiểu về [tự động hoàn thành bằng phím Tab](https://en.wikipedia.org/wiki/Command-line_completion). Nghiêm túc đấy, mẹo này sẽ giúp bạn tiết kiệm rất nhiều thời gian và công sức. Giả sử bạn đang ở dòng lệnh và cần chuyển vào một thư mục ở xa, chẳng hạn `~/Documents/Odin-Project/foundations/javascript/calculator/`. Đó là một lệnh dài phải gõ, và mọi thứ phải chính xác tuyệt đối thì mới hoạt động. Không đời nào, chúng ta *quá* lười để làm thế! Về cơ bản, khi nhấn <kbd>Tab</kbd>, dòng lệnh sẽ tự động hoàn thành phần lệnh bạn đã bắt đầu gõ khi chỉ còn một lựa chọn. Ví dụ, thư mục home thường có cả thư mục `Documents` và `Downloads`. Nếu bạn đã gõ `cd D` rồi nhấn <kbd>Tab</kbd>, dòng lệnh sẽ cho biết nó chưa chắc bạn muốn thư mục nào bằng cách hiển thị các lựa chọn khớp với phần bạn đã gõ:

```bash
$ cd D
Documents/ Downloads/
$ cd D
```

Nhưng khi gõ thêm một chút, nó sẽ tự hoàn thành tên thư mục cho bạn. Nhờ vậy, bạn có thể gõ toàn bộ đường dẫn tệp ở trên chỉ bằng cách nhập ít như `cd Doc[tab]O[tab]f[tab]j[tab]cal[tab]` (tùy những thư mục khác có trên máy tính của bạn). Hãy thử và làm quen với cách hoạt động này. Bạn sẽ thích nó đấy.

Thứ ba, có một phím tắt rất tiện để mở mọi thứ bên trong thư mục dự án: `.` Sau khi cài một trình soạn thảo văn bản, bạn có thể dùng phím tắt này để mở toàn bộ dự án và tất cả tệp trong đó chỉ bằng một lần. Phím tắt này cũng thường được dùng với Git (sẽ tìm hiểu kỹ sau) trong các lệnh như `git add .` để thêm tất cả tệp trong một thư mục vào vùng staging của Git. Ví dụ, nếu đã cài VS Code, bạn có thể `cd` vào thư mục dự án rồi gõ `code .` (kèm dấu chấm). VS Code sẽ khởi chạy và mở thư mục dự án trong thanh bên. Hãy xem phần tiếp theo của bài để có ví dụ chi tiết hơn.

### Mở tệp trong VSCode từ dòng lệnh

- **Linux**: Bạn có thể mở VSCode từ dòng lệnh bằng cách gõ `code`, và có thể mở thư mục hoặc tệp bằng cách thêm tên vị trí phía sau: `code my_awesome_project/`.

- **macOS**: Cần thực hiện một số bước thiết lập. Sau khi cài VSCode, hãy khởi chạy bằng cách nào bạn thấy thuận tiện. Khi chương trình đang chạy, mở Command Palette bằng <kbd>Cmd</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>. Trong hộp thoại nhỏ hiện ra, gõ `shell command`. Một trong các lựa chọn sẽ là `Shell Command: Install 'code' command in PATH`. Chọn mục đó rồi khởi động lại terminal nếu terminal đang mở.

- **WSL2**: Mở VSCode từ dòng lệnh trong WSL2 cũng dễ như trên Linux. Chỉ cần nhập `code`, VSCode sẽ mở trong WSL2.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note lesson-note--warning" markdown="1">

#### Tạo thư mục Desktop

Một số tài liệu dưới đây giả định rằng hệ thống của bạn có thư mục Desktop. Tuy nhiên, nếu không có thư mục này ở bất kỳ đâu trong hệ thống, hãy làm như sau: Mở terminal và bảo đảm bạn đang ở thư mục home bằng cách nhập `cd ~` vào dòng lệnh. Sau đó, nhập `mkdir Desktop` và nhấn `enter`. Để chắc chắn lệnh đã chạy đúng, hãy nhập `ls` và kiểm tra xem thư mục Desktop có xuất hiện cùng các thư mục khác hay không.

</div>

<div class="lesson-note lesson-note--warning" markdown=1>

#### Dùng đúng hệ điều hành

Nhiều tài liệu trong số này giả định bạn đang dùng môi trường Mac hoặc Linux. Nếu đã học bài cài đặt trước đó, có lẽ bạn đã cài Linux theo chế độ dual-boot hoặc trên máy ảo. Hoặc bạn có thể đang dùng macOS. Nếu không có macOS hoặc bất kỳ phiên bản Ubuntu chính thức nào, hãy quay lại [bài học Cài đặt](https://www.theodinproject.com/lessons/foundations-installations).

</div>

1. Truy cập khóa học [The Unix Shell](https://swcarpentry.github.io/shell-novice/) do Software Carpentry Foundation thiết kế. Tại đó có đầy đủ các bài học về sử dụng CLI, nhưng hiện tại bạn chỉ cần hoàn thành những bài sau:

   <div class="lesson-note" markdown="1">

   #### Thực hiện khóa học SWCarpentry trên WSL2

   Dùng lệnh `wget` cùng liên kết trong phần `Download files` để tải tệp zip vào bản cài đặt WSL2 của bạn (`wget https://swcarpentry.github.io/shell-novice/data/shell-lesson-data.zip`). Bạn cũng cần cài unzip bằng lệnh `sudo apt install unzip`, sau đó dùng `unzip shell-lesson-data.zip` để giải nén tệp. Lưu ý rằng trong suốt khóa học này, kết quả terminal của bạn có thể hơi khác so với kết quả được hiển thị trong bài. Bất cứ khi nào khóa học yêu cầu bạn đi đến Desktop, thay vào đó bạn sẽ đi đến thư mục home bằng lệnh cd (`cd ~`).

   </div>

   - [Download files](https://swcarpentry.github.io/shell-novice/#download-files) - chỉ làm theo hướng dẫn trong phần này. Bạn không cần cài phần mềm nào và có thể chuyển sang mục tiếp theo trong danh sách.
   - [Giới thiệu về Shell](https://swcarpentry.github.io/shell-novice/01-intro.html)
   - [Điều hướng tệp và thư mục](https://swcarpentry.github.io/shell-novice/02-filedir.html)
   - [Làm việc với tệp và thư mục](https://swcarpentry.github.io/shell-novice/03-create.html)

1. Với những siêu năng lực CLI mới khám phá, hãy luyện tạo một thư mục và vài tệp bằng các lệnh `mkdir`, `touch` và `cd` đã giới thiệu ở bước trước. Ví dụ, một trang web cơ bản có thể có tệp `index.html` chính, tệp biểu định kiểu CSS tên là `style.css` và một thư mục `images`. Hãy nghĩ cách tạo các tệp này bằng lệnh rồi thực hành!

1. Hãy luyện tạo và xóa tệp cũng như thư mục! Bạn sẽ cần nhập các lệnh cho những bước dưới đây vào terminal. Nếu không nhớ cách mở terminal, hãy cuộn lên để xem lại hướng dẫn.

    1. Tạo một thư mục mới tên `test` trong thư mục home.
    1. Điều hướng vào thư mục `test`.
    1. Tạo một tệp mới tên `test.txt`. *Gợi ý: dùng lệnh `touch`.*
    1. Mở tệp vừa tạo trong VSCode, chỉnh sửa một vài chỗ, lưu tệp rồi đóng lại.
    1. Điều hướng ra khỏi thư mục `test`.
    1. Xóa thư mục `test`.

    Vậy là xong phần luyện tập! Nếu từ giờ bạn duy trì thói quen thực hiện phần lớn tác vụ từ dòng lệnh, những lệnh này sẽ nhanh chóng trở nên quen thuộc. Di chuyển và sao chép tệp bằng dòng lệnh hiệu quả hơn nhiều, dù hiện tại việc đó có thể vẫn khiến bạn thấy phiền hơn.

</div>