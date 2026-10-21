### Giới thiệu

Trong dự án này, bạn sẽ tạo một trang web hoàn chỉnh dựa trên bản thiết kế chúng tôi cung cấp. Nếu đã học theo tiến độ, bạn sẽ có những kỹ năng cần thiết để hoàn thành, nhưng việc này có thể không dễ!

Bản thiết kế là hình ảnh của toàn bộ website, kèm theo thông tin về font chữ và màu sắc trong phần bài tập bên dưới.

Đừng *ngại* dùng Google hoặc quay lại các bài học trước để tra cứu. **Trong thực tế, các lập trình viên chuyên nghiệp dùng Google *liên tục* cho cả những việc họ đã làm suốt nhiều năm.** Ở giai đoạn này, bạn chưa cần ghi nhớ mọi thứ, nên đừng lo. Ngoài ra, có một vài chi tiết nhỏ chúng ta chưa đề cập trong các bài học. *Đó là có chủ ý.* Những chi tiết này không lớn và rất dễ tìm kiếm (ví dụ: Google `css rounded corners`).

Hãy làm dự án gần với bản thiết kế nhất có thể, nhưng đừng lo phải đạt độ chính xác đến từng pixel. Đừng lấy thước đo hoặc đếm pixel để tìm chính xác khoảng margin giữa các phần. Mục tiêu của bài tập là tạo một thứ gì đó từ đầu và đặt các phần tử vào vị trí tương đối phù hợp. Không sao nếu bạn dùng `margin: 24px` trong khi bản thiết kế thực tế dùng `margin: 48px`.

*Cuối cùng*, bạn cứ thoải mái thay nội dung của riêng mình vào thiết kế này. Hình ảnh có một số nội dung mẫu vô nghĩa, nhưng nếu muốn nghĩ ra một doanh nghiệp và cá nhân hóa trang web, bạn hoàn toàn có thể làm vậy! Hãy chèn hình ảnh thật vào các vị trí giữ chỗ, đồng thời thử thay đổi màu sắc và font chữ nếu thích.

<div class="lesson-note" markdown="1">

#### Lưu ý về hình ảnh trên web

Bạn không có quyền pháp lý để sử dụng tùy ý bất kỳ hình ảnh nào tìm thấy trên web. Có rất nhiều hình ảnh miễn phí, nhưng hãy chắc chắn hình ảnh bạn dùng thực sự được phép sử dụng, đồng thời ghi công tác giả trong dự án. Một cách dễ để ghi công là đặt tên và thông tin liên hệ của tác giả trong file README của kho lưu trữ.

Một số nơi tốt để tìm hình ảnh miễn phí được phép sử dụng trên web gồm [Pexels](https://www.pexels.com/), [Pixabay](https://pixabay.com/) và [Unsplash](https://unsplash.com/).

</div>

### Thiết lập kho GitHub cho dự án

Giống như trong dự án Recipes, hãy thiết lập một kho git trước khi bắt đầu làm dự án này. Git và GitHub sẽ trở thành portfolio của bạn về sau, vì vậy hãy lưu mọi thứ ở đó! Xem lại [dự án Recipes](https://www.theodinproject.com/lessons/foundations-recipes) nếu cần nhắc lại cách thiết lập.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Đừng quên commit sớm và thường xuyên! Bạn có thể [tham khảo bài học về Commit Message](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/commit-messages)!

1. [Tải xuống hình ảnh thiết kế đầy đủ](https://cdn.statically.io/gh/TheOdinProject/curriculum/81a5d553f4073e593d23a6ab00d50eef8620796d/foundations/html_css/project/imgs/01.png) và xem thử sản phẩm bạn sẽ tạo. Một số font, màu sắc và kích thước như sau (bạn có thể dùng giá trị riêng nếu muốn):
   - Font: `Roboto` (bạn cần tải xuống và cài đặt font này nếu máy chưa có)
   - Màu nền tối (header, hero section và footer): `#1F2937`
   - Chữ logo:
     - Cỡ font: `24px`
     - Màu: `#F9FAF8`
   - Chữ chính của hero section ("This website is awesome"):
     - Cỡ font: `48px`
     - Màu: `#F9FAF8`
     - Độ đậm: `900`
   - Chữ phụ của hero section ("This website has some subtext...") và các liên kết trong header:
     - Cỡ font: `18px`
     - Màu: `#E5E7EB`
   - Màu nền của button và "Call to action": `#3882F6`
   - Tiêu đề phần thông tin:
     - Cỡ font: `36px`
     - Màu: `#1F2937`
     - Độ đậm: `900`
   - Trích dẫn:
     - Màu nền: `#E5E7EB`
     - Cỡ font: `36px`
     - Màu: `#1F2937`
     - Độ đậm: `300`
     - Kiểu font: `italic`
1. Có nhiều cách để bắt đầu một dự án như thế này, và nhìn vào một tài liệu HTML trống mà không biết bắt đầu từ đâu có thể khiến bạn choáng ngợp. Gợi ý của chúng tôi: làm từng phần một. Website bạn đang tạo có 4 phần chính (và một footer), vì vậy hãy chọn một phần và hoàn thiện tương đối trước khi chuyển sang phần tiếp theo. Bắt đầu từ trên xuống luôn là một kế hoạch tốt.
1. Với phần đang làm, hãy đưa toàn bộ nội dung lên trang trước khi bắt đầu tạo kiểu. Nói cách khác, làm HTML *trước*, rồi mới làm CSS. Có thể bạn sẽ phải quay lại HTML sau khi bắt đầu tạo kiểu, nhưng việc liên tục qua lại ngay từ đầu sẽ tốn nhiều thời gian hơn và dễ gây bực bội. (Lưu ý: bạn không cần dùng nhiều hơn một stylesheet. Chỉ một file CSS là đủ cho dự án này).
1. Nhiều phần tử trên trang này rất giống những gì bạn đã thấy trong các bài tập flexbox... nếu cần ôn lại, hãy thoải mái xem lại các bài đó.
1. Bạn **không** cần làm dự án này responsive. Điều đó có nghĩa là bạn không phải điều chỉnh thiết kế cho các kích thước màn hình, mức thu phóng hoặc thiết bị di động khác nhau. Hiện tại, chỉ cần tập trung làm cho trang trông đẹp trên màn hình máy tính thông thường. Sau này bạn sẽ học cách xử lý các kích thước màn hình khác nhau.
1. Khi hoàn thành, đừng quên push dự án lên GitHub!

#### Xem dự án trên web

Để chia sẻ website với người khác, bạn cần **deploy nó bằng GitHub Pages**. Hướng dẫn đầy đủ cho quy trình này có trong bài học [dự án Recipes](https://www.theodinproject.com/lessons/foundations-recipes#viewing-your-project-on-the-web).

</div>