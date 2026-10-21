### Giới thiệu

Đã đến lúc thực sự phát huy năng lực của bạn. Test Driven Development (TDD) chắc chắn có thể khiến bạn thấy không thoải mái lúc đầu, nhưng sẽ trở nên tự nhiên hơn khi luyện tập. Chúng ta sẽ triển khai trò chơi kinh điển 'Battleship'. Nếu bạn chưa từng chơi hoặc cần ôn lại, bạn có thể đọc về [luật chơi Battleship](https://en.wikipedia.org/wiki/Battleship_(game)) và [chơi phiên bản Battleship trực tuyến](http://en.battleship-game.org/).

Vì chúng ta đang thực hiện TDD, điều quan trọng là bạn không bị quá tải. Hãy làm từng bước một. Viết một test, sau đó làm cho nó pass. Vì project này có nhiều phần chuyển động hơn trong các module khác nhau so với những project trước đây có sử dụng testing, nếu bạn không chắc nên test gì hoặc tiếp cận việc testing như thế nào, hãy cân nhắc xem lại các ý tưởng và bài tập trong [bài học More Testing](https://www.theodinproject.com/lessons/node-path-javascript-more-testing).

Chúng ta chưa thảo luận về việc test giao diện của một webpage, việc này cần một bộ công cụ riêng và nằm ngoài phạm vi của unit này, nhưng đây là nội dung chúng ta sẽ học sau trong curriculum. Vì vậy, bạn không nên test DOM cho assignment này. Thay vào đó, hãy cố gắng tách mọi phần chức năng của application khỏi các phần thực sự thao tác với DOM.

### Assignment

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note lesson-note--warning" markdown="1">

#### Jest và ESM

Hãy nhớ rằng Jest chưa có hỗ trợ ổn định được tích hợp sẵn cho ESM, vì vậy bạn sẽ cần [thiết lập Babel để chuyển đổi Jest ESM/CJS](https://www.theodinproject.com/lessons/node-path-javascript-testing-basics#using-es6-import-statements-with-jest).

</div>

1. Bắt đầu app bằng cách tạo class/factory `Ship` (bạn tự chọn).
   1. 'Ships' của bạn sẽ là các object chứa length, số lần chúng bị hit và việc chúng đã bị sunk hay chưa.
   1. **HÃY NHỚ** rằng bạn chỉ cần test public interface của object. Chỉ những method hoặc property được dùng bên ngoài object 'ship' mới cần unit test.
   1. Ships nên có một function `hit()` làm tăng số 'hits' trong ship.
   1. `isSunk()` nên là một function tính toán xem một ship có được xem là sunk hay không dựa trên length và số hits mà nó đã nhận.
1. Tạo một class/factory `Gameboard`.
   1. Lưu ý rằng chúng ta vẫn chưa tạo User Interface. Chúng ta nên biết code của mình đang dần hoàn thiện bằng cách chạy các test. Bạn không nên dựa vào `console.log` hoặc các method của DOM để bảo đảm code hoạt động như mong đợi.
   1. Gameboards phải có khả năng đặt ships tại các tọa độ cụ thể bằng cách gọi ship factory hoặc class.
   1. Gameboards nên có một function `receiveAttack` nhận một cặp tọa độ, xác định cuộc tấn công có đánh trúng ship hay không, rồi gọi function 'hit' cho ship thích hợp, hoặc ghi lại tọa độ của cú bắn trượt.
   1. Gameboards phải theo dõi các cuộc tấn công trượt để có thể hiển thị chúng đúng cách.
   1. Gameboards phải có khả năng báo cáo tất cả ships của chúng đã bị sunk hay chưa.
1. Tạo một class/factory `Player`.
   1. Trong game sẽ có hai loại player, player 'real' và player 'computer'.
   1. Mỗi player object nên chứa gameboard riêng của nó.
1. Import các class/factory của bạn vào một file khác và điều khiển game bằng event listeners để tương tác với các object. Tạo một module giúp bạn quản lý những action cần diễn ra trong DOM.
   1. Đến lúc này, bạn có thể bắt đầu xây dựng User Interface.
   1. Thiết lập game mới bằng cách tạo Players. Hiện tại, chỉ cần điền các tọa độ được định trước vào Gameboard của mỗi player. Sau này bạn sẽ triển khai hệ thống cho phép players đặt ships.
   1. Hiện tại, chúng tôi để việc triển khai HTML cho bạn tự quyết định, nhưng bạn nên hiển thị cả hai board của players và render chúng bằng thông tin từ class/factory Gameboard.
      - Bạn sẽ cần các method để render Gameboard của từng player, vì vậy hãy đặt chúng trong một module phù hợp.
   1. Event listeners của bạn nên đi qua từng lượt của game, lần lượt từng lượt, chỉ sử dụng các method từ những object khác. Nếu tại bất kỳ thời điểm nào bạn muốn viết một function mới, hãy dừng lại và xác định class hoặc module nào nên chứa function đó.
   1. Đối với các cuộc tấn công, hãy để user click vào một tọa độ trên Gameboard của enemy. Gửi input của user đến các method trên object của bạn, rồi render lại các board để hiển thị thông tin mới.
      - Players nên lần lượt chơi bằng cách tấn công Gameboard của enemy. Nếu bạn cần theo dõi lượt của player hiện tại, việc quản lý điều đó trong module này là phù hợp, thay vì trong một object đã nêu ở trên.
      - Game được chơi với computer, vì vậy hãy làm cho player 'computer' có khả năng thực hiện các nước đi ngẫu nhiên. Computer không cần thông minh, nhưng phải biết một nước đi có hợp lệ hay không (tức là không bắn cùng một tọa độ hai lần).
   1. Tạo các điều kiện để game kết thúc khi tất cả ships của một player đã bị sunk. Function này cũng phù hợp với module này.
1. Hoàn thiện bằng cách triển khai một hệ thống cho phép players đặt ships. Ví dụ, bạn có thể cho họ nhập tọa độ cho từng ship hoặc có một button để xoay vòng qua các vị trí ngẫu nhiên.

#### Điểm cộng

Làm cho project battleship của bạn ấn tượng hơn bằng cách thêm bất kỳ thay đổi nào dưới đây.

- Triển khai drag and drop để cho phép players đặt ships.
- Tạo tùy chọn 2-player, cho phép users lần lượt chơi bằng cách chuyền laptop qua lại hoặc xoay màn hình trên desktop. Triển khai một màn hình 'pass device' để players không nhìn thấy board của nhau!
- Cải thiện intelligence của computer player bằng cách cho nó thử các ô liền kề sau khi 'hit'.

</div>