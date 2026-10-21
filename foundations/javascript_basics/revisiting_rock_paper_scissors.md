### Giới thiệu

Giờ đây chúng ta đã có thể thao tác với DOM, đã đến lúc quay lại [Kéo, Búa, Bao](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/rock-paper-scissors) và thêm giao diện người dùng cho trò chơi.

Trước khi bắt đầu thay đổi dự án Kéo, Búa, Bao, bạn cần tìm hiểu một khái niệm trong Git gọi là **branching** để có thể thực hiện thay đổi mà không phải lo làm hỏng những gì đang có.

Các nhánh trong Git cho phép kho của bạn chứa đồng thời nhiều phiên bản *thực tại thay thế* của các tệp. Thực ra bạn đã (kiểu như) dùng nhánh từ khi tạo commit đầu tiên, chỉ là có thể bạn chưa biết! Trong [bài học thiết lập Git](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/setting-up-git), khi chạy `git config --global init.defaultBranch main`, bạn đã đặt tên cho thứ được gọi là nhánh *mặc định* của các kho. Nhánh mặc định đơn giản là tên gọi của nhánh được tạo khi bạn tạo commit đầu tiên trong một dự án; trong câu lệnh đó, chúng ta đặt tên là `main`, phù hợp với tiêu chuẩn hiện nay.

Giống như các nhánh trên cây (vì thế mới có tên này), mọi nhánh của một dự án đều mọc ra từ một “thân cây” (nhánh `main`) hoặc từ *những* nhánh khác.

Khi tạo commit trên một nhánh cụ thể, những thay đổi đó chỉ tồn tại trên **nhánh đó**, còn mọi nhánh khác vẫn hoàn toàn giống như lúc bạn tách nhánh từ chúng.

Điều này có nghĩa là bạn có thể giữ nhánh `main` làm nơi chỉ chứa các tính năng hoàn thiện mà bạn biết là đang hoạt động đúng, rồi thêm từng tính năng vào dự án thông qua các nhánh riêng, được gọi là *nhánh tính năng*.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Cách tạo một nhánh mới.
- Cách hợp nhất các nhánh.
- Các phương pháp hay nhất và trường hợp sử dụng của nhánh.

### Sử dụng các nhánh

Bạn có thể tạo nhánh mới bằng câu lệnh `git branch <branch_name>`. Sau đó, bạn có thể chuyển sang nhánh mới bằng `git checkout <branch_name>`. Bạn cũng có thể tạo một nhánh mới và chuyển sang đó bằng một câu lệnh duy nhất, dùng cờ `-b` với `checkout`, theo dạng `git checkout -b <branch_name>`.

Bạn có thể xem tất cả các nhánh hiện tại bằng `git branch` mà không cần đối số nào khác. Nhánh bạn đang đứng sẽ được đánh dấu bằng dấu sao. Nếu muốn chuyển lại về `main` từ một nhánh khác, bạn có thể làm giống như khi chuyển sang bất kỳ nhánh nào khác bằng `git checkout main`.

Sau khi làm xong trên nhánh tính năng và sẵn sàng đưa các commit đã tạo vào nhánh main, bạn sẽ cần thực hiện thao tác gọi là `merge`.

Có thể hợp nhất bằng câu lệnh `git merge <branch_name>`. Câu lệnh này lấy các thay đổi bạn đã commit trên `branch_name` và thêm chúng vào nhánh hiện tại. Sơ đồ dưới đây minh họa việc tạo một nhánh `develop`, commit trên đó rồi hợp nhất vào `main`.

<pre class="mermaid">

---
title: Example of Git Branching
---
gitGraph
   commit id: "commit1"
   commit id: "commit2"
   branch develop
   checkout develop
   commit id: "commit1a"
   commit id: "commit2a"
   checkout main
   merge develop id: "merge to main"

</pre>

Đôi khi, cùng một dòng trong một tệp đã bị hai nhánh khác nhau thay đổi. Khi đó, bạn sẽ gặp xung đột hợp nhất lúc cố gắng hợp nhất các nhánh ấy. Để hoàn tất việc hợp nhất, trước tiên bạn phải giải quyết xung đột; nội dung này sẽ được đề cập trong một bài học sau.

Khi không còn cần một nhánh, bạn có thể xóa nó bằng `git branch -d <branch_name>` nếu nhánh đã được hợp nhất vào `main`, hoặc bằng `git branch -D <branch_name>` nếu chưa được hợp nhất. Thông thường, bạn sẽ muốn xóa các nhánh sau khi dùng xong; nếu không, chúng có thể chất đống và khiến bạn khó tìm được nhánh cần dùng.

### Chia sẻ mã

Một trường hợp sử dụng tuyệt vời khác của nhánh là chia sẻ mã với người khác mà có thể bạn không muốn commit vào nhánh main (hoặc nhánh tính năng) chút nào.

Ví dụ: nếu một tính năng mới bạn đang làm có lỗi mà bạn không tìm ra được, khiến mã bị hỏng, bạn sẽ không muốn commit mã lỗi đó và để nó nằm trong “lịch sử vĩnh viễn” của dự án. Thay vào đó, bạn có thể tạo một nhánh tạm thời mới, chuyển sang đó rồi commit mã vào nhánh mới này. Sau đó, nếu đẩy nhánh tạm thời lên GitHub, bạn có thể chia sẻ nó với những người có khả năng giúp giải quyết vấn đề. Trong bài tập bên dưới, bạn sẽ được thực hành tạo các nhánh mới.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Khởi động

1. Trước tiên hãy luyện tập một chút về branching. [Công cụ trực quan tương tác về branching trong Git](https://learngitbranching.js.org/) của Peter Cottle rất tuyệt vời. Chỉ hoàn thành các cấp độ 1-3 của "Introduction Sequence".

#### Thêm giao diện cho Kéo, Búa, Bao

1. Thiết lập một nhánh mới trên kho Kéo, Búa, Bao trước đó.
   1. Vì chúng ta sẽ tạo giao diện cho trò chơi Kéo, Búa, Bao, hãy tạo một nhánh mới và chuyển sang đó bằng câu lệnh `git checkout -b rps-ui`.
   1. Hiện bạn đang làm việc cục bộ trên nhánh `rps-ui`. Tuy nhiên, nhánh này chưa tồn tại trong kho từ xa. Nếu vào trang kho github, bạn sẽ thấy mình chỉ có 1 nhánh, đó là `main`. Hãy đẩy nhánh mới lên kho từ xa bằng câu lệnh `git push origin rps-ui`. Bây giờ bạn sẽ thấy hai nhánh trong kho GitHub! Bạn có thể chọn nhánh mới trên GitHub bằng bộ chọn nhánh dạng danh sách như trong ảnh chụp màn hình dưới đây.

      ![Trình đơn thả xuống các nhánh trên GitHub](https://cdn.statically.io/gh/TheOdinProject/curriculum/46c18d8445051e016b1e415fe0227a0fa33cc825/foundations/javascript_basics/revisiting_rock_paper_scissors/imgs/00.png)

   1. Hãy đảm bảo bạn đang ở nhánh `rps-ui`. Bạn có thể kiểm tra bằng câu lệnh `git branch`. Nhánh hiện tại sẽ có dấu (\*)asterisk bên cạnh. Nếu vì lý do nào đó bạn đang ở nhánh khác, hãy chuyển sang `rps-ui` bằng câu lệnh `git checkout rps-ui`. Giờ bạn đã sẵn sàng làm tính năng mới! Lưu ý: Bạn có thể thêm tệp, commit vào nhánh này và đẩy thay đổi lên kho giống như với nhánh main. Mọi thứ đều như nhau, ngoại trừ khi đẩy thay đổi, bạn sẽ dùng `git push origin rps-ui` thay vì `git push origin main`, vì chúng ta đang đẩy lên nhánh mới.
1. Trong giao diện của chúng ta, người chơi phải có thể chơi bằng cách nhấp vào các nút thay vì nhập câu trả lời vào prompt.
   1. Trước mắt, hãy xóa logic chơi đúng năm vòng.
   1. Tạo ba nút, mỗi nút cho một lựa chọn. Thêm trình lắng nghe sự kiện vào các nút để gọi hàm `playRound` với `playerSelection` tương ứng mỗi khi một nút được nhấn. (bạn có thể giữ lại các `console.log`s trong bước này)
   1. Thêm một `div` để hiển thị kết quả và đổi tất cả các `console.log`s thành các phương thức DOM.
   1. Hiển thị điểm số đang cập nhật và công bố người thắng trò chơi ngay khi một người chơi đạt 5 điểm.
   1. Có thể bạn sẽ phải refactor (làm lại/viết lại) mã ban đầu để nó hoạt động theo cách này. Không sao cả! Làm lại mã cũ là một phần quan trọng trong công việc của lập trình viên.
1. Khi đã hoàn tất giao diện và đảm bảo mọi thứ đều ổn, hãy đảm bảo mọi thay đổi đã được commit vào nhánh `rps-ui` bằng `git status` trước khi tiếp tục.
1. Bây giờ hãy xem cách hợp nhất các thay đổi từ nhánh `rps-ui` trở lại nhánh `main`.
   1. Checkout nhánh mà chúng ta muốn hợp nhất VÀO, tức `main`, bằng câu lệnh `git checkout main`.
   1. Hợp nhất nhánh `rps-ui` vào `main`, là nhánh hiện tại, bằng `git merge rps-ui`.
   1. Nếu mọi thứ diễn ra suôn sẻ, nhánh `rps-ui` đã được hợp nhất thành công với main! Dùng `git log` và bạn sẽ thấy tất cả commit đã tạo trên nhánh tính năng nằm trên các commit đã tạo trên nhánh main. Giờ là bước cuối cùng!
   1. Đẩy nhánh `main` lên kho từ xa bằng cách chạy `git push origin main` . Truy cập kho GitHub và bạn sẽ thấy nhánh `main` có tất cả thay đổi và commit đã tạo trên nhánh `rps-ui`. Xin chúc mừng! Bạn đã đẩy thành công tính năng đầu tiên vào nhánh production!
   1. Giờ khi toàn bộ mã đã nằm trên nhánh main, chúng ta không còn thực sự cần nhánh `rps-ui` nữa. Hãy dọn dẹp cả kho cục bộ lẫn kho từ xa. Xóa nhánh khỏi kho cục bộ bằng `git branch -d rps-ui`, đồng thời xóa nó khỏi kho từ xa trên GitHub bằng `git push origin --delete rps-ui`. Chúc mừng, chúng ta đã dọn dẹp xong!
1. Hãy đảm bảo xuất bản dự án trên GitHub Pages và thêm liên kết xem trước trực tiếp vào [bài học dự án](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/rock-paper-scissors).

</div>