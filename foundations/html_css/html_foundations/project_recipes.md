### Giới thiệu

Đã đến lúc thực hành toàn bộ kiến thức HTML mà bạn đã học. Trong dự án này, bạn sẽ xây dựng một trang web công thức nấu ăn cơ bản.

Trang web sẽ gồm một trang chỉ mục chính chứa các liên kết đến một vài công thức. Khi hoàn thành, trang web có thể sẽ không trông đẹp mắt lắm, nhưng điều quan trọng cần nhớ là mục đích của dự án này là rèn luyện kỹ năng HTML; sau này chúng ta sẽ quay lại dự án để tạo kiểu bằng CSS.

### Thiết lập kho GitHub cho dự án

Như đã đề cập trong [bài học Giới thiệu về Git](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/introduction-to-git), bạn nên sắp xếp tất cả dự án của mình như một danh mục sản phẩm và liên kết chúng với GitHub để người khác có thể xem.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Cẩn thận khi tạo tệp trên GitHub

GitHub cho phép chúng ta thực hiện thay đổi trực tiếp trên trang web của họ. Nếu làm vậy sau khi đã sao chép kho về máy, mã nguồn trên máy của bạn sẽ cũ hơn bản ở kho từ xa, khiến việc đẩy thay đổi lên gặp thêm khó khăn. Phần lớn thời gian, bạn nên tạo tệp trên máy của mình.

 Khi tiếp tục học trong khóa học, bạn sẽ biết cách xử lý những tình huống này, nhưng hiện tại, điều quan trọng là hãy làm theo hướng dẫn cẩn thận để đi theo quy trình đơn giản.

</div>

Nếu chưa biết cách thiết lập một kho, hãy làm theo bước 1 và 2 trong [Kiến thức cơ bản về Git](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/git-basics) để học cách thực hiện trước khi tiếp tục với các bước dưới đây:

1. Tạo một kho mới cho dự án này trên GitHub.com và đặt tên là `odin-recipes`.

1. Sao chép kho đó về máy, vào trong thư mục `repos` mà bạn đã tạo trước đó trong bài học Kiến thức cơ bản về Git. Lệnh cần có dạng `git clone git@github.com:username/odin-recipes.git` (sử dụng SSH).

1. Bây giờ, dùng `cd` để chuyển vào thư mục dự án `odin-recipes` vừa được tạo trên máy.

1. Thiết lập tệp `README.md` và viết một phần giới thiệu ngắn mô tả dự án hiện tại là gì cũng như những kỹ năng bạn sẽ thể hiện sau khi hoàn thành. (Bạn cũng có thể viết phần này như một bài tự nhìn lại ở cuối dự án; đây là một cách hay để ôn lại những gì đã học.)

Nếu gặp khó khăn:

- Mọi lệnh Git cần được chạy từ bên trong thư mục dự án (bạn có quên dùng `cd` để chuyển vào thư mục `odin-recipes` không?).

- Hãy chắc chắn bạn đã làm theo hướng dẫn [tạo khóa SSH](https://www.theodinproject.com/lessons/foundations-setting-up-git#step-2-configure-git-and-github:~:text=Step%202.3%3A%20Create%20an%20SSH%20key) để sao chép từ GitHub bằng SSH.

- Tham khảo quy trình trong [bài học Kiến thức cơ bản về Git](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/git-basics).

#### Mẹo về thời điểm commit

Đừng quên những gì chúng ta đã học trong [bài học Thông điệp commit](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/commit-messages) nhé!

Trong quá trình xây dựng dự án, có lẽ bạn sẽ thực hiện vài chu kỳ `git add` + `git commit` trước khi sẵn sàng đẩy dự án lên GitHub bằng `git push origin main`.

Khi viết mã, commit sớm và thường xuyên được xem là một thực hành tốt. Hãy commit mỗi khi mã có một thay đổi có ý nghĩa. Việc này sẽ tạo ra dòng thời gian cho quá trình tiến bộ của bạn và cho thấy mã nguồn hoàn chỉnh không tự nhiên xuất hiện.

Sau khi nhập `git push origin main`, hãy chuyển sang trình duyệt và mở kho của bạn trên GitHub. Lúc này, bạn sẽ thấy tất cả tệp vừa đẩy lên.

Được rồi, tạm thời học Git như vậy là đủ -- giờ là lúc thật sự xây dựng dự án!

### Cảnh báo về việc xem mã nguồn dự án hoàn chỉnh của người khác trước khi hoàn thành dự án của bạn

Trước khi tiếp tục: tất cả dự án của chúng ta đều có một phần "Bài làm của học viên" ở cuối, nơi mọi người đăng mã nguồn đã hoàn thành và trang web đang hoạt động của dự án. Những bài làm này giúp bạn vừa so sánh mã nguồn của mình, vừa xem những hướng đi khác mà mọi người đã chọn cho mã nguồn và thiết kế dự án.

Chúng tôi biết bạn có thể muốn xem các bài làm khi bị mắc kẹt hoặc không biết bắt đầu từ đâu, nhưng **đừng làm vậy**! Dưới đây là một số lý do:

- Tất cả dự án được gửi lên đều là bài làm của *những học viên khác*. Điều đó có nghĩa chúng do những người cũng đang học viết ra, nên không nhất thiết thể hiện "thực hành tốt nhất" hoặc mã nguồn chất lượng cao.
- Xem bài làm sẽ lấy đi cơ hội phát triển kỹ năng giải quyết vấn đề và nghiên cứu của bạn. Trong thế giới thực, không phải lúc nào cũng có "lời giải" để bạn xem, và bạn sẽ phải tự tìm ra cách của mình.
- Có rất nhiều cách khác nhau để đạt được cùng một kết quả cuối cùng - không có cách nào là "một khuôn mẫu dùng cho tất cả."
- Một phép so sánh: "Học lập trình bằng cách xem mã nguồn hoàn chỉnh cũng giống như học làm bột pizza bằng cách nhìn một chiếc pizza đã nướng."  Có rất nhiều công đoạn hơn những gì bạn có thể hình dung nếu chỉ nhìn sản phẩm cuối cùng mà không thấy quá trình tạo ra nó.

Sau khi hoàn thành dự án, bạn có thể thoải mái xem các bài làm khác để tìm hiểu những cách hoàn thành dự án khác nhau.

**ĐỪNG XEM TRƯỚC. Hãy đến hỏi trong [máy chủ Discord của chúng tôi!](https://discord.com/channels/505093832157691914/516751477306294273) *(Bạn cần [tham gia máy chủ Discord](https://discord.gg/fbFCkYabZB) trước thì mới xem được kênh này).***

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Lần lặp 1: cấu trúc ban đầu

1. Trong thư mục `odin-recipes`, tạo một tệp `index.html`.
1. Điền nội dung HTML khung thông thường vào tệp và thêm một tiêu đề `h1` có nội dung "Odin Recipes" vào phần thân trang.

#### Lần lặp 2:  trang công thức

1. Tạo một thư mục mới bên trong thư mục `odin-recipes` và đặt tên là `recipes`.
1. Tạo một tệp HTML mới bên trong thư mục  `recipes` và đặt tên theo công thức mà tệp sẽ chứa. Ví dụ: `lasagna.html`. Bạn có thể dùng tên món ăn yêu thích của mình hoặc, nếu cần gợi ý, tìm một công thức trên [Allrecipes](https://www.allrecipes.com/). Hãy nhớ thêm HTML khung thông thường. Mã khung này cần có trong mọi tệp `.html` mà bạn tạo.
1. Trước mắt, chỉ cần thêm một tiêu đề `h1` với tên công thức làm nội dung.
1. Quay lại tệp `index.html`, thêm một liên kết đến trang công thức vừa tạo. Ví dụ: bên dưới tiêu đề `<h1>Odin Recipes</h1>`, viết liên kết như sau: `<a href="recipes/recipename.html">Recipe Title</a>`. Phần văn bản của liên kết cũng nên là tên công thức.
1. **Thêm một liên kết quay lại trang chỉ mục** trên trang công thức để dễ điều hướng. Bạn có thể đặt liên kết ở đầu hoặc cuối trang công thức (ví dụ: `lasagna.html`). Đây là một ví dụ:

   ```html
   <a href="../index.html">Home</a>
   ```

   Nhờ vậy, người dùng có thể nhanh chóng quay lại trang chủ sau khi xem công thức.

#### Lần lặp 3:  nội dung trang công thức

Trang công thức mới cần có các nội dung sau:

1. Một hình ảnh miễn phí của món ăn đã hoàn thành, đặt bên dưới tiêu đề `h1` mà bạn đã thêm trước đó.

1. Bên dưới hình ảnh, thêm một tiêu đề "Mô tả" có kích thước phù hợp, sau đó là một hoặc hai đoạn văn mô tả công thức.

1. Bên dưới phần mô tả, thêm tiêu đề "Nguyên liệu", sau đó là một **danh sách không có thứ tự** gồm các nguyên liệu cần cho công thức.

1. Cuối cùng, bên dưới danh sách nguyên liệu, thêm tiêu đề "Các bước", sau đó là một **danh sách có thứ tự** gồm các bước cần thực hiện để làm món ăn.

#### Lần lặp 4: thêm công thức

1. Tạo thêm hai trang công thức. Đảm bảo chúng có bố cục hoàn toàn giống trang công thức đầu tiên.
1. Đừng quên liên kết các công thức mới trên trang chỉ mục. Bạn cũng có thể đặt tất cả liên kết trong một danh sách không có thứ tự để chúng không nằm trên cùng một dòng.

Ví dụ:

```html
 <ul>
    <li><a href="recipes/yourrecipe.html">Recipe Title 1</a></li>
    <li><a href="recipes/yourrecipe.html">Recipe Title 2</a></li>
    <li><a href="recipes/yourrecipe.html">Recipe Title 3</a></li>
  </ul>
```
  
Các liên kết của bạn sẽ chưa bắt mắt, nhưng hiện tại hãy chỉ tập trung xây dựng chúng cho đầy đủ.

</div>

### Xem dự án trên web

Nếu muốn cho người khác xem sản phẩm của mình (dự án) hoặc gửi bài làm bên dưới, bạn sẽ cần xuất bản trang web để người khác có thể truy cập qua web thay vì chỉ trên máy của bạn. Tin tốt là nếu dự án đã ở trên GitHub (như mô tả ở trên), việc này khá đơn giản.

GitHub cho phép bạn xuất bản trực tiếp các dự án web từ một kho GitHub. Nhờ vậy, bạn có thể truy cập dự án tại `your-github-username.github.io/your-github-repo-name`.

<div class="lesson-note" markdown="1">

#### Xuất bản từ kho riêng tư

Cần có tài khoản GitHub trả phí để xuất bản dự án web từ một kho riêng tư. Tài khoản miễn phí chỉ có thể xuất bản từ các kho công khai.

</div>

Có một vài cách để thực hiện việc này, nhưng cách đơn giản nhất là:

1. Đảm bảo tệp HTML chính của bạn có tên `index.html` **và** nằm trong thư mục gốc (cấp cao nhất) của kho.
1. Truy cập kho GitHub trên web và nhấp vào nút **Cài đặt**, như trong ảnh chụp màn hình bên dưới.
    ![Ảnh chụp màn hình chỉ vào mục Cài đặt trong một kho ví dụ](https://cdn.statically.io/gh/TheOdinProject/curriculum/90b1a362af0bb8635af9593cd8911c9aefb68569/foundations/html_css/html-foundations/imgs/01.png)
1. Nhấp vào **Pages** ở thanh bên trái.
1. Đổi **Branch** từ *none* thành *main branch* rồi nhấp **Save**.
1. Có thể sẽ mất vài phút (trang web GitHub nói tối đa 10 phút, nhưng chúng tôi từng thấy mất đến một giờ. Đừng thêm "theme" vào dự án, nếu không bạn có thể gặp xung đột git; thay vào đó, hãy kiên nhẫn), nhưng dự án sẽ có thể truy cập trên web tại `your-github-username.github.io/your-github-repo-name` (dĩ nhiên bạn cần thay thông tin của mình vào liên kết).
1. Nếu dự án chưa được xuất bản sau 1 giờ, hãy đảm bảo bạn có tệp tên `index.html` trong thư mục gốc của kho và mọi thiết lập đều chính xác.  Truy cập kho trên GitHub và nhấp vào Actions; nếu không có mục nào, hãy quay lại phần cài đặt, đổi **Branch** từ *main branch* thành *none* rồi nhấp **Save**, sau đó đổi **Branch** từ *none* thành *main branch* và nhấp **Save**.

<!-- Note: this header gets referenced in https://github.com/TheOdinProject/theodinproject/blob/main/app/views/lessons/project_submissions/index.html.erb. If you change it, make sure to make a corresponding change there as well. -->
### Bài làm của cộng đồng

<div class="lesson-note" markdown="1">

#### Nhiều bài làm của cộng đồng sẽ có tạo kiểu

Trong các bài học sau, bạn sẽ học CSS và thêm kiểu dáng cho dự án này. Khi xem các bài làm của cộng đồng, bạn sẽ nhận thấy nhiều bài đã được tạo kiểu như vậy.

</div>

Khi xem các bài làm của cộng đồng (sau khi hoàn thành dự án), hãy ghi nhớ một vài điều:

- Học viên thường quay lại các dự án cũ để cải thiện chúng cho danh mục sản phẩm của mình
- Một số người bắt đầu khóa học với kinh nghiệm lập trình từ trước
- Yêu cầu của dự án đôi khi thay đổi theo thời gian

**Hãy tập trung đáp ứng yêu cầu của dự án thay vì so sánh mình với người khác.** Bài làm của cộng đồng hữu ích để bạn thấy những cách tiếp cận khác nhau, nhưng chúng không đại diện cho việc dự án của bạn "nên" trông như thế nào ở giai đoạn này.

Điều này cũng áp dụng cho các dự án sau. Để biết thêm thông tin, hãy đọc ["Học lập trình" trong "Câu chuyện thành công của một học viên TOP"](https://dev.to/theodinproject/learning-code-f56).