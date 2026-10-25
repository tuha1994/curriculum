### Giới thiệu

Hãy sử dụng những gì bạn đã học về đệ quy cho đến nay để giải quyết hai bài toán kinh điển có thể tận dụng đệ quy: Fibonacci và Merge Sort.

#### Dãy Fibonacci

[Chuỗi Fibonacci](http://en.wikipedia.org/wiki/Fibonacci_number) là một chuỗi số trong đó mỗi số là tổng của hai số đứng trước nó. Ví dụ: 0, 1, 1, 2, 3, 5, 8, 13 là tám số đầu tiên trong chuỗi.

Có lẽ bạn đã nghĩ ngay rằng bài toán này có thể được giải bằng cách lặp thay vì đệ quy, và bạn đã đúng. Tuy vậy, việc tạo chuỗi bằng đệ quy là một cách tuyệt vời để hiểu rõ hơn về đệ quy.

Bạn có thể xem [video của Khan Academy về Fibonacci đệ quy](https://www.youtube.com/watch?v=zg-ddPbzcKM) để hiểu thêm.

#### Sắp xếp trộn (Merge sort)

Một phần quan trọng của khoa học máy tính dành cho việc sắp xếp dữ liệu. Một thuật toán sử dụng cách tiếp cận 'chia để trị' của đệ quy có thể giảm một bài toán sắp xếp thành những bài toán con ngày càng nhỏ hơn.

Merge sort là một thuật toán sắp xếp như vậy và có thể nhanh hơn nhiều so với các thuật toán khác, chẳng hạn bubble sort, trên những tập dữ liệu phù hợp. Về cơ bản, merge sort đệ quy đi qua một mảng dữ liệu chưa được sắp xếp cho đến khi mảng đạt đến tập con nhỏ nhất, tức là một phần tử đơn lẻ. Dĩ nhiên, một mảng chỉ có một phần tử được xem là đã sắp xếp. Sau đó, merge sort gộp các phần tử đơn lẻ trở lại theo thứ tự đã sắp xếp. Khá thông minh!

Để hiểu thuật toán merge sort đang làm gì, hãy xem các tài nguyên sau:

1. Xem video nhập môn về [merge sort trong khóa học CS50x của Harvard](https://youtu.be/Ns7tGNbtvV4).
1. Xem một phần giải thích khác về [cách merge sort hoạt động](https://youtu.be/4oqjcKenCH8?t=6248) (chỉ xem đến 1:58:33), lần này từ một phần bài giảng CS50x của Harvard.
1. [Khái niệm gộp](https://youtu.be/6pV2IF0fgKY) và [phần Merge Sort -- Cách nó hoạt động](https://youtu.be/mB5HXBb_HY8) trên YouTube sẽ giúp bạn có cái nhìn chính thức hơn về bài toán này nếu vẫn chưa hiểu rõ.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Trước tiên, hãy tạo một tệp và giải quyết chuỗi fibonacci:

1. Sử dụng phép lặp, viết một phương thức `#fibs` nhận vào một số và trả về một mảng chứa số lượng phần tử tương ứng trong chuỗi Fibonacci. Với đầu vào mẫu là `8`, hàm này phải trả về mảng `[0, 1, 1, 2, 3, 5, 8, 13]`.
1. Bây giờ hãy viết một phương thức khác là `#fibs_rec` để giải quyết cùng bài toán bằng đệ quy.
1. Kiểm thử cả hai phiên bản phương thức bằng cách truyền vào các độ dài khác nhau.

Hy vọng bạn đã giải quyết được bài toán bằng đệ quy! Nếu cần trợ giúp để hiểu điều gì đang xảy ra trong hàm này, phần "Thử chạy" bên dưới sẽ hữu ích. Nếu vẫn còn hơi bối rối, ở cuối trang có liên kết đến một số tài nguyên bổ sung.

Khi đã nắm vững cách giải Fibonacci bằng đệ quy, hãy tạo một tệp mới và làm việc với merge sort:

1. Xây dựng một hàm `#merge_sort` nhận vào một mảng và trả về một mảng đã sắp xếp, sử dụng phương pháp merge sort đệ quy.

Ví dụ kiểm thử:

- `merge_sort([])` → `[]`
- `merge_sort([73])` → `[73]`
- `merge_sort([1, 2, 3, 4, 5])` → `[1, 2, 3, 4, 5]`
- `merge_sort([3, 2, 1, 13, 8, 5, 0, 1])` → `[0, 1, 1, 2, 3, 5, 8, 13]`
- `merge_sort([105, 79, 100, 110])` → `[79, 100, 105, 110]`

Mẹo:

- Hãy suy nghĩ về trường hợp cơ sở, cũng như hành vi nào đang lặp đi lặp lại và thực sự có thể được giao cho một đối tượng khác xử lý (ví dụ: chính phương thức đó!).
- Nếu chưa hiểu rõ điều gì nên xảy ra, bạn có thể xem lại các video nền tảng.

#### Thử chạy

Để minh họa hiệu ứng đệ quy được triển khai trong phương thức Fibonacci, hãy làm như sau:

1. Thêm đoạn sau vào đầu phương thức:

   ```ruby
   puts 'This was printed recursively'
   ```

1. Chạy phương thức với đầu vào là `8`.

1. Nếu phương thức được triển khai chính xác, bạn sẽ thấy câu đó được in ra khoảng 8 lần (Hãy nhớ rằng, tùy vào cách bạn triển khai phương thức, bạn có thể thấy 7 lần thay vì 8. Đây không phải lỗi! Điều đó đơn giản phụ thuộc vào số lần phương thức được lặp lại *thực sự*).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ trợ.

- Một lời giải JavaScript dạng bài viết đầy đủ cho [Fibonacci đệ quy](https://www.scaler.com/topics/fibonacci-series-in-javascript/)
- Một góc nhìn khác về [merge sort](https://www.toptal.com/developers/sorting-algorithms/merge-sort)
- Trực quan hóa và [tìm hiểu](https://www.educative.io/courses/recursion-for-coding-interviews-in-javascript/NEZ7kKgMJKK) cách cấp phát bộ nhớ cho các hàm đệ quy
- Để thử thêm các bài toán về đệ quy, hãy làm 5 bài đầu tiên trong [Project Euler](https://projecteuler.net/problems)
