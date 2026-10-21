### Giới thiệu

Bây giờ bạn đã rất thành thạo DFS và BFS. Hãy thử dùng các algorithm tìm kiếm của mình cho một vấn đề thực tế.

Trong project này, bạn sẽ cần dùng graph, một cấu trúc dữ liệu tương tự (nhưng không giống hệt) binary tree. Để có phần giới thiệu hay về graph, tham khảo [Khan Academy's "Describing Graphs"](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/describing-graphs). Đừng quên xem [section on representing graphs](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs), vì nó có thể cho bạn ý tưởng tốt về cách triển khai graph trong code.

Với đủ số lượt đi, một knight trên bàn cờ chuẩn 8x8 có thể di chuyển từ bất kỳ ô nào đến bất kỳ ô nào khác. Nước đi cơ bản của nó là tiến hai bước và sang một bên một bước, hoặc tiến một bước và sang một bên hai bước. Nó có thể quay về mọi hướng.

Tất cả vị trí có thể đến sau một nước đi trông như sau:

![Knights Travails board](https://cdn.statically.io/gh/TheOdinProject/curriculum/d30038e0aaca1f35e58e205e37a21b2c9d31053d/javascript/computer_science/project_knights_travails/imgs/01.png)
Lưu ý: Hình ảnh chỉ nhằm giải thích vấn đề, bạn không cần tạo GUI.

Trong bài toán này, bàn cờ có thể được biểu diễn như một graph:

Mỗi ô trên bàn cờ là một node (hoặc vertex).
Các nước đi hợp lệ của knight từ mỗi ô biểu diễn các edge (hoặc connection) giữa các vertex.
Vì vậy, bài toán tìm đường đi ngắn nhất cho chuyển động của knight trở thành bài toán duyệt graph. Mục tiêu là duyệt graph (bàn cờ) để tìm tuyến đường ngắn nhất giữa hai node (vị trí bắt đầu và kết thúc).

#### Vertex và edge

Các vertex trong graph này là từng vị trí có thể có trên bàn cờ, được biểu diễn bằng một cặp tọa độ như `[x, y]`, trong đó x và y nằm giữa 0 và 7.
Các edge là những nước đi hợp lệ của knight giữa các vertex. Ví dụ, từ `[0,0]`, knight có thể đi tới `[2,1]`, `[1,2]` và các vị trí khác. Mỗi nước đi như vậy biểu diễn một connection giữa vertex `[0,0]` và các vertex có thể tới khác.

#### Biểu diễn graph

Khi giải bài toán này, bạn không cần tạo rõ ràng một graph object có vertex và edge. Thay vào đó, hãy nghĩ graph là implicit. Knight bắt đầu tại một vertex cụ thể, và algorithm sẽ tự động khám phá mọi nước đi khả dĩ (edge) tới các vertex khác (vị trí trên bàn cờ) khi duyệt bàn cờ.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Nhiệm vụ của bạn là xây dựng một function `knightMoves` cho biết cách ngắn nhất có thể để đi từ một ô này đến ô khác, bằng cách xuất ra tất cả ô mà knight dừng lại trên đường đi.

Bạn có thể xem bàn cờ như có tọa độ hai chiều. Vì vậy, việc gọi function sẽ như sau:

`knightMoves([0,0],[1,2]) // returns [[0,0],[1,2]]`

<div class="lesson-note" markdown="1">

#### Nhiều đường đi ngắn nhất

Đôi khi *có nhiều hơn một đường nhanh nhất*. Các ví dụ dưới đây minh họa điều đó. Bất kỳ câu trả lời nào cũng đúng miễn tuân thủ luật và đưa ra đường đi ngắn nhất có thể.

- `knightMoves([0,0],[3,3])` có thể trả về `[[0,0],[2,1],[3,3]]` hoặc `[[0,0],[1,2],[3,3]]`.
- `knightMoves([3,3],[0,0])` có thể trả về `[[3,3],[2,1],[0,0]]` hoặc `[[3,3],[1,2],[0,0]]`.
- `knightMoves([0,0],[7,7])` có thể trả về `[[0,0],[2,1],[4,2],[6,3],[4,4],[6,5],[7,7]]` hoặc `[[0,0],[2,1],[4,2],[6,3],[7,5],[5,6],[7,7]]`, hoặc các đường đi ngắn nhất khả dĩ khác.

</div>

1. Nghĩ về luật của bàn cờ và knight, đồng thời bảo đảm tuân thủ chúng.
1. Từ mỗi ô có nhiều nước đi khả dĩ. Chọn một cấu trúc dữ liệu cho phép bạn làm việc với chúng. Không để bất kỳ nước đi nào ra ngoài bàn cờ.
1. Cả DFS và BFS đều có thể dùng ở đây, nhưng hãy suy nghĩ cẩn thận về cách mỗi algorithm hoạt động. Một trong hai sẽ yêu cầu bạn xử lý khả năng bị mắc trong một cycle vô hạn.
1. Dùng algorithm tìm kiếm đã chọn để tìm đường ngắn nhất giữa ô bắt đầu (hoặc node) và ô kết thúc. Xuất toàn bộ đường đi, ví dụ:

   ```bash
   > knightMoves([3,3],[4,3])
   => You made it in 3 moves!  Here's your path:
     [3,3]
     [4,5]
     [2,4]
     [4,3]
   ```

</div>