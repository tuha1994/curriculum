### Giới thiệu

Bạn đã học về [binary search tree](http://en.wikipedia.org/wiki/Binary_search_tree), trong đó một nhóm phần tử dữ liệu được biến thành một cây gồm các node, với mỗi node bên trái có giá trị "thấp hơn" mỗi node bên phải. Cây bắt đầu bằng "root node" và node không có con được gọi là "leaf node". Bạn cũng đã học các algorithm duyệt cây như breadth-first và depth-first.

Bây giờ hãy xem xét balanced binary search tree (BST). BST cho phép thực hiện nhanh các thao tác tìm kiếm, chèn và xóa phần tử dữ liệu. Đọc [article on building balanced BSTs](https://www.geeksforgeeks.org/sorted-array-to-balanced-bst/). Đây cũng là một [video on building balanced BSTs](https://youtu.be/VCTP81Ij-EM). Dù tài nguyên cuối không dùng JavaScript, bạn vẫn nên hiểu đủ để tự xây dựng pseudocode.

### Bài tập

Trong bài tập này, bạn sẽ xây dựng một BST cân bằng. Không sử dụng các giá trị trùng lặp vì chúng khiến bài toán phức tạp hơn và tạo ra những cây khó cân bằng hơn nhiều. Vì vậy, hãy luôn xóa giá trị trùng lặp hoặc kiểm tra giá trị đã tồn tại trước khi chèn.

<div class="lesson-content__panel" markdown="1">

1. Xây dựng class/factory `Node`. Nó cần có một thuộc tính chứa dữ liệu được lưu trữ, cùng với node con bên trái và bên phải.

1. Xây dựng class/factory `Tree`, nhận một array khi khởi tạo. Class `Tree` cần có thuộc tính `root`, sử dụng giá trị trả về của `buildTree()` mà bạn sẽ viết tiếp theo.

1. Viết function `buildTree(array)` nhận một array các number (ví dụ: `[1, 7, 4, 23, 8, 9, 4, 3, 5, 7, 9, 67, 6345, 324]`) và biến nó thành một binary tree cân bằng gồm các object `Node` được đặt đúng vị trí (đừng quên sắp xếp và xóa giá trị trùng lặp!). Function `buildTree()` phải trả về root node ở level 0. Bạn có thể để function này private (bằng tính năng privacy của class syntax hoặc không đưa nó vào object mà factory trả về), rồi chỉ gọi nó để khởi tạo giá trị của node `root`.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Biểu diễn cây bằng hình ảnh

   Nếu muốn hình dung binary search tree, đây là function `prettyPrint()` sẽ `console.log` cây theo một cấu trúc dễ đọc. Function này nhận root của cây làm giá trị cho tham số node:

   ```javascript
   const prettyPrint = (node, prefix = '', isLeft = true) => {
     if (node === null || node === undefined) {
       return;
     }

     prettyPrint(node.right, `${prefix}${isLeft ? '│   ' : '    '}`, false);
     console.log(`${prefix}${isLeft ? '└── ' : '┌── '}${node.data}`);
     prettyPrint(node.left, `${prefix}${isLeft ? '    ' : '│   '}`, true);
   }
   ```

   </div>

1. Viết function `includes(value)` nhận một giá trị và trả về `true` nếu giá trị đó nằm trong cây. Nếu không có, nó phải trả về `false`.

1. Viết function `insert(value)` nhận một giá trị và chèn node mới chứa giá trị đó vào cây. Hãy chèn sao cho vẫn giữ thuộc tính "binary search": với mỗi node, mọi node bên trái phải có giá trị thấp hơn, còn mọi node bên phải phải có giá trị cao hơn. Nếu gọi function với giá trị đã tồn tại trong cây, function không làm gì. Nếu cần tài nguyên bổ sung, xem [Geeks for Geeks article on binary search tree insertion](https://www.geeksforgeeks.org/insertion-in-binary-search-tree/?ref=lbp).

   <div class="lesson-note" markdown="1">

   #### Tránh dùng array đầu vào ban đầu

   Bạn có thể muốn triển khai các method này bằng array đầu vào ban đầu dùng để xây dựng cây, nhưng để các thao tác hiệu quả, điều quan trọng là không làm vậy. Nếu xem lại [Big O Cheatsheet](https://www.bigocheatsheet.com/), ta sẽ thấy binary search tree có thể chèn/xóa trong thời gian `O(log n)`, nhanh hơn đáng kể so với array cho cùng thao tác. Để có hiệu quả này, implementation của các method nên duyệt cây và thao tác trên các node cùng liên kết của chúng.

   </div>

1. Viết function `deleteItem(value)` nhận một giá trị và xóa nó khỏi cây. Bạn sẽ phải xử lý nhiều trường hợp tùy vào số node con của node mục tiêu. Nếu giá trị không tồn tại, function không làm gì. Nếu cần tài nguyên bổ sung, xem [Geeks for Geeks article on deleting in a binary search tree](https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/?ref=lbp).

1. Viết function `levelOrderForEach(callback)` nhận một callback function làm tham số. `levelOrderForEach()` phải duyệt cây theo thứ tự level breadth-first và gọi callback trên từng giá trị khi duyệt, truyền từng giá trị (*không phải* node) làm đối số, tương tự cách `Array.prototype.forEach()` hoạt động với array. `levelOrderForEach()` có thể dùng iteration hoặc recursion (hãy thử cả hai!). Nếu không có callback function, [throw an Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/throw) thông báo rằng callback là bắt buộc.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Dùng queue

   Bạn nên dùng một array đóng vai trò queue để theo dõi các node con chưa duyệt và thêm node mới vào danh sách. Nếu cần hình dung, xem [mycodeschool's video on level order traversal](https://www.youtube.com/watch?v=86g8jAQug04).

   </div>

1. Viết các function `inOrderForEach(callback)`, `preOrderForEach(callback)` và `postOrderForEach(callback)`, cũng nhận callback làm tham số. Mỗi function phải duyệt cây theo thứ tự depth-first tương ứng và truyền từng giá trị cho callback được cung cấp. Các function phải throw Error nếu không có callback, giống `levelOrderForEach()`. Nếu cần tài nguyên về cách các kiểu duyệt hoạt động, xem [mycodeschool's video on Binary Tree Traversal: Preorder, Inorder, Postorder](https://www.youtube.com/watch?v=gm8DUJJhmY4).

1. Viết function `height(value)` trả về height của node chứa giá trị đã cho. Height được định nghĩa là số edge trong đường đi dài nhất từ node đó đến leaf node. Nếu không tìm thấy giá trị trong cây, function trả về `undefined`.

1. Viết function `depth(value)` trả về depth của node chứa giá trị đã cho. Depth được định nghĩa là số edge trong đường đi từ node đó đến root node. Nếu không tìm thấy giá trị trong cây, function trả về `undefined`.

1. Viết function `isBalanced()` kiểm tra cây có cân bằng hay không. Binary tree được xem là cân bằng nếu với mọi node trong cây, chênh lệch height giữa subtree trái và phải không quá 1, đồng thời cả hai subtree trái và phải cũng cân bằng.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Cạm bẫy khi kiểm tra độ cân bằng

   Một lỗi thường gặp là chỉ kiểm tra chênh lệch height giữa các node con trái và phải của root. Như vậy chưa đủ — bạn phải kiểm tra điều kiện cân bằng với mọi node.

   </div>

1. Viết function `rebalance()` để cân bằng lại một cây mất cân bằng. Bạn sẽ muốn dùng một method duyệt để cung cấp array mới cho function `buildTree()`.

#### Kết hợp tất cả

Viết một driver script thực hiện các việc sau:

1. Tạo binary search tree từ một array các number ngẫu nhiên, trong đó mỗi phần tử có giá trị nhỏ hơn 100. Nếu muốn, bạn có thể tạo function trả về một array number ngẫu nhiên mỗi lần được gọi.
1. Xác nhận cây cân bằng bằng cách gọi `isBalanced()`.
1. In tất cả phần tử theo thứ tự level, pre, post và in.
1. Làm mất cân bằng cây bằng cách thêm một số number có giá trị lớn hơn 100.
1. Xác nhận cây mất cân bằng bằng cách gọi `isBalanced()`.
1. Cân bằng cây bằng cách gọi `rebalance()`.
1. Xác nhận cây cân bằng bằng cách gọi `isBalanced()`.
1. In tất cả phần tử theo thứ tự level, pre, post và in.

</div>