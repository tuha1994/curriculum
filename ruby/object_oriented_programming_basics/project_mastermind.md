### Giới thiệu

Nếu bạn chưa từng chơi [Mastermind](https://www.wikihow.com/Play-Mastermind), đây là một trò chơi trong đó bạn phải đoán mã bí mật của đối thủ trong một số lượt nhất định (giống trò đoán chữ, nhưng dùng các chốt màu). Sau mỗi lượt, bạn sẽ nhận được phản hồi về mức độ chính xác của dự đoán — liệu nó hoàn toàn đúng hay chỉ đúng màu nhưng sai vị trí.

### Bài tập

Xây dựng một trò chơi Mastermind trên dòng lệnh, trong đó bạn có 12 lượt để đoán mã bí mật; bắt đầu với việc bạn đoán mã ngẫu nhiên do máy tính tạo ra.

<div class="lesson-content__panel" markdown="1">

  1. Hãy suy nghĩ xem bạn sẽ thiết kế bài toán này như thế nào!
  1. Xây dựng trò chơi với giả định máy tính chọn ngẫu nhiên các màu bí mật và người chơi phải đoán chúng.  Hãy nhớ rằng bạn cần đưa ra phản hồi phù hợp về mức độ chính xác của dự đoán sau mỗi lượt!
  1. Tiếp theo, tái cấu trúc mã để cho phép người chơi chọn vai trò tạo mã bí mật hoặc đoán mã.
  1. Phát triển trò chơi để máy tính có thể đoán nếu bạn chọn tự quyết định các màu bí mật. Bạn có thể triển khai một chiến lược cho máy tính tuân theo luật chơi, hoặc thay đổi các luật đó.
  1. Nếu chọn thay đổi luật chơi, bạn có thể cung cấp cho máy tính thêm thông tin về mỗi dự đoán. Chẳng hạn, ban đầu bạn có thể để máy tính đoán ngẫu nhiên, nhưng giữ lại những màu khớp chính xác. Bạn có thể bổ sung một chút khả năng suy luận cho máy tính để nếu máy tính đoán đúng màu nhưng sai vị trí, dự đoán tiếp theo của nó phải bao gồm màu đó ở một vị trí nào đó.
  1. Nếu muốn tuân theo luật chơi, bạn sẽ cần tìm hiểu về [các chiến lược giải Mastermind](https://puzzling.stackexchange.com/questions/546/clever-ways-to-solve-mastermind).
  1. Hãy đăng lời giải của bạn bên dưới!

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải là phần bắt buộc, vì vậy bạn có thể xem nó như tài liệu bổ sung.

- Không trực tiếp hữu ích, nhưng đây là một số [thư viện game Ruby](https://www.ruby-toolbox.com/categories/game_libraries) để bạn tham khảo cho vui.