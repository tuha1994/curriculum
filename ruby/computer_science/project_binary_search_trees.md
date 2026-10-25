### Giới thiệu

Bạn đã tìm hiểu về [cây tìm kiếm nhị phân](http://en.wikipedia.org/wiki/Binary_search_tree), trong đó một nhóm phần tử dữ liệu được biến thành một cái cây gồm nhiều nút, với mỗi nút bên trái có giá trị "nhỏ hơn" mỗi nút bên phải. Cây bắt đầu từ "nút gốc", còn mọi nút không có nút con được gọi là "nút lá". Bạn cũng đã tìm hiểu về các thuật toán duyệt cây như duyệt theo chiều rộng và duyệt theo chiều sâu.

Bây giờ, hãy cùng xem xét các cây tìm kiếm nhị phân cân bằng (BST). BST cho phép thực hiện nhanh các thao tác tìm kiếm, chèn và xóa phần tử dữ liệu. Hãy đọc [bài viết về cách xây dựng BST cân bằng](https://www.geeksforgeeks.org/sorted-array-to-balanced-bst/). Bạn cũng có thể xem [video về cách xây dựng BST cân bằng](https://youtu.be/VCTP81Ij-EM). Mặc dù hai tài liệu này không dùng Ruby, bạn vẫn nên hiểu đủ để tự phát triển mã giả của mình.

### Bài tập

Trong bài tập này, bạn sẽ xây dựng một BST cân bằng. Không sử dụng các giá trị trùng lặp vì chúng khiến bài toán phức tạp hơn và tạo ra những cây khó cân bằng hơn nhiều. Vì vậy, hãy luôn loại bỏ các giá trị trùng lặp hoặc kiểm tra xem giá trị đó đã tồn tại trước khi chèn.

<div class="lesson-content__panel" markdown="1">

1. Xây dựng lớp `Node`. Lớp này cần có một thuộc tính chứa dữ liệu mà nó lưu trữ, cùng với các nút con bên trái và bên phải.

1. Xây dựng lớp `Tree`, nhận một mảng khi được khởi tạo. Lớp `Tree` cần có thuộc tính `root`, sử dụng giá trị trả về của `#build_tree`, là phương thức bạn sẽ viết tiếp theo.

1. Viết phương thức `#build_tree`, nhận một mảng số (ví dụ: `[1, 7, 4, 23, 8, 9, 4, 3, 5, 7, 9, 67, 6345, 324]`) và biến nó thành một cây nhị phân cân bằng gồm các đối tượng `Node` được đặt đúng vị trí (đừng quên sắp xếp và loại bỏ các giá trị trùng lặp!). Phương thức `#build_tree` phải trả về nút gốc ở cấp 0. Bạn có thể đặt phương thức này ở chế độ `private` và gọi nó trong hàm khởi tạo để gán giá trị cho `@root`.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Biểu diễn cây trực quan

   Nếu muốn trực quan hóa cây tìm kiếm nhị phân, đây là phương thức `#pretty_print` do một học viên viết và chia sẻ trên Discord:

   ```ruby
   def pretty_print(node = @root, prefix = '', is_left: true)
     return unless node

     pretty_print(node.right, "#{prefix}#{is_left ? '│   ' : '    '}", is_left: false)
     puts "#{prefix}#{is_left ? '└── ' : '┌── '}#{node.data}"
     pretty_print(node.left, "#{prefix}#{is_left ? '    ' : '│   '}", is_left: true)
   end
   ```

   </div>

1. Viết phương thức `#include?(value)`, nhận một giá trị và trả về `true` nếu giá trị đó nằm trong cây. Nếu giá trị không nằm trong cây, phương thức phải trả về `false`.

1. Viết phương thức `#insert(value)`, nhận một giá trị và chèn một nút mới chứa giá trị đó vào cây. Hãy chèn theo cách duy trì thuộc tính "tìm kiếm nhị phân": với mỗi nút, mọi nút bên trái phải có giá trị nhỏ hơn, còn mọi nút bên phải phải có giá trị lớn hơn. Nếu phương thức được gọi với một giá trị đã tồn tại trong cây, nó không được làm gì cả. Nếu cần thêm tài liệu tham khảo, hãy xem [bài viết của Geeks for Geeks về cách chèn vào cây tìm kiếm nhị phân](https://www.geeksforgeeks.org/insertion-in-binary-search-tree/?ref=lbp).

   <div class="lesson-note" markdown="1">

   #### Tránh sử dụng mảng đầu vào ban đầu

   Bạn có thể muốn triển khai các phương thức này bằng mảng đầu vào ban đầu, nhưng vì hiệu năng của các thao tác này, điều quan trọng là không làm vậy. Nếu xem lại [Big O Cheatsheet](https://www.bigocheatsheet.com/), ta sẽ thấy cây tìm kiếm nhị phân có thể chèn/xóa trong thời gian `O(log n)`, nhanh hơn đáng kể so với mảng khi thực hiện cùng các thao tác. Để đạt được hiệu năng cao hơn này, phần triển khai các phương thức phải duyệt qua cây và thao tác trực tiếp trên các nút cùng mối liên kết giữa chúng.

   </div>

1. Viết phương thức `#delete(value)`, nhận một giá trị và xóa nó khỏi cây. Bạn sẽ phải xử lý nhiều trường hợp khác nhau tùy vào số lượng nút con của nút được chọn. Nếu giá trị được cung cấp không tồn tại trong cây, phương thức không được làm gì cả. Nếu cần thêm tài liệu tham khảo, hãy xem [bài viết của Geeks for Geeks về cách xóa trong cây tìm kiếm nhị phân](https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/?ref=lbp).

1. Viết phương thức `#level_order`, nhận một block. Phương thức này phải duyệt cây theo thứ tự cấp độ từ trên xuống dưới, tức duyệt theo chiều rộng, và truyền từng giá trị cho block được cung cấp. Khi có block, phương thức phải trả về `self`. Có thể triển khai phương thức này bằng phép lặp hoặc đệ quy (hãy thử cả hai!). Nếu không có block, phương thức phải trả về một `Enumerator`.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Sử dụng hàng đợi

   Bạn nên dùng một mảng đóng vai trò hàng đợi để theo dõi tất cả các nút con chưa được duyệt và thêm các nút mới vào danh sách. Nếu cần hình dung cách hoạt động, hãy xem [video của mycodeschool về duyệt theo thứ tự cấp độ](https://www.youtube.com/watch?v=86g8jAQug04).

   </div>

1. Viết các phương thức `#inorder`, `#preorder` và `#postorder`, cũng nhận một block. Mỗi phương thức phải duyệt cây theo thứ tự tương ứng trong phép duyệt theo chiều sâu và truyền từng giá trị cho block được cung cấp. Khi có block, chúng phải trả về `self`; khi không có block, chúng phải trả về một `Enumerator`. Nếu cần tài liệu về cách các kiểu duyệt khác nhau hoạt động, hãy xem [video của mycodeschool về duyệt cây nhị phân: Preorder, Inorder, Postorder](https://www.youtube.com/watch?v=gm8DUJJhmY4).

1. Viết phương thức `#height`, nhận một giá trị và trả về chiều cao của nút chứa giá trị đó. Chiều cao được định nghĩa là số cạnh trong đường đi dài nhất từ nút đó đến một nút lá. Nếu không tìm thấy giá trị trong cây, phương thức phải trả về `nil`.

1. Viết phương thức `#depth`, nhận một giá trị và trả về độ sâu của nút chứa giá trị đó. Độ sâu được định nghĩa là số cạnh trong đường đi từ nút đó đến nút gốc của cây. Nếu không tìm thấy giá trị trong cây, phương thức phải trả về `nil`.

1. Viết phương thức `#balanced?` để kiểm tra xem cây có cân bằng hay không. Một cây nhị phân được xem là cân bằng nếu, với mọi nút trong cây, chênh lệch chiều cao giữa cây con bên trái và bên phải không quá 1, đồng thời cả cây con bên trái và bên phải cũng đều cân bằng.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Sai lầm khi kiểm tra tính cân bằng

   Một sai lầm phổ biến là chỉ kiểm tra chênh lệch chiều cao giữa hai nút con bên trái và bên phải của nút gốc. Như vậy là chưa đủ — bạn phải kiểm tra điều kiện cân bằng cho mọi nút.

   </div>

1. Viết phương thức `#rebalance` để cân bằng lại một cây mất cân bằng. Bạn nên dùng một phương thức duyệt để tạo ra một mảng mới rồi truyền mảng đó cho phương thức `#build_tree`.

#### Kết hợp tất cả

Viết một script điều khiển thực hiện các việc sau:

1. Tạo một cây tìm kiếm nhị phân từ một mảng gồm các số ngẫu nhiên `(Array.new(15) { rand(1..100) })`
1. Xác nhận cây đang cân bằng bằng cách gọi `#balanced?`
1. In tất cả phần tử theo thứ tự cấp độ, trước, sau và giữa
1. Làm cây mất cân bằng bằng cách thêm một số giá trị lớn hơn 100
1. Xác nhận cây đã mất cân bằng bằng cách gọi `#balanced?`
1. Cân bằng cây bằng cách gọi `#rebalance`
1. Xác nhận cây đã cân bằng bằng cách gọi `#balanced?`
1. In tất cả phần tử theo thứ tự cấp độ, trước, sau và giữa.

</div>