### Giới thiệu

Giờ đây bạn đã rất thành thạo DFS và BFS. Hãy thử áp dụng các thuật toán tìm kiếm của chúng ta vào một bài toán thực tế.

Trong project này, bạn sẽ cần sử dụng một cấu trúc dữ liệu tương tự (nhưng không hoàn toàn giống) cây nhị phân — đó là đồ thị. Để có phần giới thiệu dễ hiểu về đồ thị, hãy tham khảo ["Mô tả đồ thị" của Khan Academy](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/describing-graphs).

Nếu có đủ lượt đi, một quân mã trên bàn cờ vua tiêu chuẩn 8x8 có thể di chuyển từ bất kỳ ô nào đến bất kỳ ô nào khác. Nước đi cơ bản của nó là tiến hai ô và sang ngang một ô, hoặc tiến một ô và sang ngang hai ô. Quân mã có thể quay về bất kỳ hướng nào.

Tất cả những vị trí có thể đến sau một nước đi trông như sau:

![Bàn cờ quân mã](https://cdn.statically.io/gh/TheOdinProject/curriculum/d30038e0aaca1f35e58e205e37a21b2c9d31053d/ruby/computer_science/project_knights_travails/imgs/01.png)

Trong bài toán này, bàn cờ vua có thể được biểu diễn dưới dạng một đồ thị:

Mỗi ô trên bàn cờ là một nút (hay đỉnh).

Các nước đi hợp lệ của quân mã từ mỗi ô biểu diễn các cạnh (hay kết nối) giữa những đỉnh đó.
Vì vậy, bài toán tìm đường đi ngắn nhất cho chuyển động của quân mã trở thành một bài toán duyệt đồ thị. Mục tiêu là duyệt đồ thị (bàn cờ) để tìm tuyến đường ngắn nhất giữa hai nút (vị trí bắt đầu và kết thúc).

#### Đỉnh và cạnh

Các đỉnh trong đồ thị này là mọi vị trí có thể có trên bàn cờ, được biểu diễn bằng một cặp tọa độ như `[x, y]`, trong đó x và y nằm trong khoảng từ 0 đến 7.
Các cạnh là những nước đi hợp lệ của quân mã giữa các đỉnh. Ví dụ, từ `[0,0]`, quân mã có thể đi đến `[2,1]`, `[1,2]` và các vị trí khác. Mỗi nước đi này biểu diễn một kết nối giữa đỉnh `[0,0]` và những đỉnh khác có thể đến được.

#### Biểu diễn đồ thị

Khi giải bài toán này, bạn không cần tạo rõ ràng một đối tượng đồ thị chứa các đỉnh và cạnh. Thay vào đó, bạn có thể xem đồ thị là một đồ thị ẩn. Quân mã bắt đầu tại một đỉnh cụ thể, còn thuật toán sẽ linh hoạt khám phá mọi nước đi có thể (các cạnh) đến những đỉnh khác (các vị trí trên bàn cờ) trong quá trình duyệt bàn cờ.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Nhiệm vụ của bạn là xây dựng một hàm `knight_moves` để hiển thị cách ngắn nhất có thể đi từ một ô này đến một ô khác bằng cách xuất ra tất cả các ô mà quân mã sẽ dừng lại trên đường đi.

Bạn có thể xem bàn cờ như một hệ tọa độ hai chiều. Vì vậy, việc gọi hàm của bạn sẽ có dạng:

`knight_moves([0,0],[1,2]) # returns [[0,0],[1,2]]`

<div class="lesson-note" markdown="1">

#### Nhiều đường đi ngắn nhất

Đôi khi *có nhiều đường đi nhanh nhất*. Dưới đây là một số ví dụ. Bất kỳ đáp án nào cũng đúng, miễn là tuân theo các quy tắc và đưa ra đường đi ngắn nhất có thể.

- `knight_moves([0,0],[3,3])` có thể trả về `[[0,0],[2,1],[3,3]]` hoặc `[[0,0],[1,2],[3,3]]`.
- `knight_moves([3,3],[0,0])` có thể trả về `[[3,3],[2,1],[0,0]]` hoặc `[[3,3],[1,2],[0,0]]`.
- `knight_moves([0,0],[7,7])` có thể trả về `[[0,0],[2,1],[4,2],[6,3],[4,4],[6,5],[7,7]]`, hoặc `[[0,0],[2,1],[4,2],[6,3],[7,5],[5,6],[7,7]]`, hoặc những đường đi ngắn nhất khả dĩ khác.

</div>

1. Hãy suy nghĩ về các quy tắc của bàn cờ và quân mã, đồng thời bảo đảm tuân thủ chúng.
1. Từ mỗi ô có thể có nhiều nước đi. Hãy chọn một cấu trúc dữ liệu cho phép bạn làm việc với chúng. Đừng cho phép bất kỳ nước đi nào đi ra ngoài bàn cờ.
1. Cả DFS và BFS đều có thể áp dụng ở đây, nhưng hãy suy nghĩ cẩn thận về cách mỗi thuật toán hoạt động. Một trong hai sẽ yêu cầu bạn xử lý khả năng bị mắc kẹt trong một chu kỳ vô hạn.
1. Sử dụng thuật toán tìm kiếm đã chọn để tìm đường đi ngắn nhất giữa ô bắt đầu (hay nút bắt đầu) và ô kết thúc. Xuất ra toàn bộ đường đi đó, ví dụ:

   ```bash
   > knight_moves([3,3],[4,3])
   => You made it in 3 moves!  Here's your path:
     [3,3]
     [4,5]
     [2,4]
     [4,3]
   ```

</div>
