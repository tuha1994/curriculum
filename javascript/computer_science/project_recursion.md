### Giới thiệu

Hãy dùng những gì bạn đã học về recursion để giải quyết hai bài toán kinh điển có thể tận dụng recursion: Fibonacci và Merge Sort.

#### Fibonacci

[Fibonacci Sequence](http://en.wikipedia.org/wiki/Fibonacci_number) là một dãy số trong đó mỗi số là tổng của hai số đứng trước nó. Ví dụ, 0, 1, 1, 2, 3, 5, 8, 13 là tám số đầu tiên trong dãy.

Có lẽ bạn đã nghĩ rằng bài này có thể giải bằng iteration thay vì recursion, và bạn đúng. Tuy vậy, tạo dãy bằng recursion là một cách tuyệt vời để hiểu rõ hơn về recursion.

Bạn có thể xem [video from Khan Academy on recursive Fibonacci](https://www.youtube.com/watch?v=zg-ddPbzcKM) để hiểu thêm.

#### Merge sort

Một phần quan trọng của computer science dành cho việc sắp xếp dữ liệu. Một algorithm dùng cách tiếp cận 'divide and conquer' của recursion có thể thu nhỏ bài toán sắp xếp thành các bài toán con ngày càng nhỏ.

Merge sort là một algorithm sắp xếp như vậy và có thể nhanh hơn nhiều algorithm khác, chẳng hạn bubble sort, trên những tập dữ liệu phù hợp. Về cơ bản, merge sort đệ quy đi qua một array dữ liệu chưa sắp xếp cho đến khi đạt tập con nhỏ nhất, chỉ còn một phần tử. Dĩ nhiên, array một phần tử được xem là đã sắp xếp. Sau đó merge sort gộp các phần tử đơn lẻ trở lại theo thứ tự. Khá thông minh!

Để hiểu algorithm merge sort đang làm gì, hãy xem các tài nguyên sau:

1. Xem video giới thiệu về [merge sort from Harvard's CS50x course](https://youtu.be/Ns7tGNbtvV4).
1. Xem một giải thích khác về [how merge sort works](https://www.youtube.com/live/iCx3zwK8Ms8?si=t7z6bEv_ZXIJDoHU&t=6550) (chỉ xem đến 2:04:05), lần này từ một phần bài giảng Harvard CS50x.
1. [The concept of merging](https://youtu.be/6pV2IF0fgKY) và [Merge Sort -- How it Works part](https://youtu.be/mB5HXBb_HY8) trên YouTube sẽ cung cấp cách nhìn hình thức hơn về vấn đề này nếu bạn vẫn chưa rõ.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Chạy code của project

Bạn sẽ cần cách chạy các script tạo cho project này. Vì project không có thành phần GUI, nó nên được chạy trên command line thay vì browser.

Các file JavaScript có thể chạy trực tiếp từ command line bằng command node được cài cùng nodejs. Bạn có thể tìm hiểu những cách phổ biến để làm việc này bằng cách đọc [documentation for the Node CLI](https://github.com/nodejs/nodejs.dev/blob/aa4239e87a5adc992fdb709c20aebb5f6da77f86/content/learn/command-line/node-run-cli.en.md).

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

Trước hết hãy tạo một file và xử lý dãy fibonacci:

1. Dùng iteration, viết function `fibs` nhận một number và trả về một array chứa từng ấy number từ dãy Fibonacci. Với input mẫu là `8`, function phải trả về array `[0, 1, 1, 2, 3, 5, 8, 13]`.
1. Bây giờ viết function `fibsRec` giải quyết cùng bài toán bằng recursion.
1. Kiểm thử cả hai phiên bản function bằng cách truyền vào nhiều độ dài khác nhau.

Hy vọng bạn đã giải được bài toán bằng recursion! Nếu cần hiểu rõ hơn function đang hoạt động thế nào, phần "Thử nghiệm" bên dưới sẽ giúp ích.

Sau khi nắm chắc cách giải Fibonacci bằng recursion, hãy tạo file mới và làm merge sort:

1. Xây dựng function `mergeSort` nhận một array và trả về array đã sắp xếp, dùng phương pháp merge sort đệ quy.

Ví dụ kiểm thử:

- `mergeSort([])` → `[]`
- `mergeSort([73])` → `[73]`
- `mergeSort([1, 2, 3, 4, 5])` → `[1, 2, 3, 4, 5]`
- `mergeSort([3, 2, 1, 13, 8, 5, 0, 1])` → `[0, 1, 1, 2, 3, 5, 8, 13]`
- `mergeSort([105, 79, 100, 110])` → `[79, 100, 105, 110]`

Mẹo:

- Hãy nghĩ về base case và hành vi lặp đi lặp lại có thể được ủy thác cho một đối tượng khác (ví dụ chính function đó!).
- Nếu chưa hiểu rõ, xem lại các video nền tảng có thể hữu ích.

#### Thử nghiệm

Để minh họa hiệu ứng đệ quy được triển khai trong function Fibonacci, làm như sau:

1. Thêm đoạn sau vào đầu function:

   ```javascript
   console.log("This was printed recursively");
   ```

1. Gọi function với `8` làm đối số.
1. Nếu function được triển khai đúng, bạn sẽ thấy câu đó được in khoảng 8 lần (hãy nhớ rằng tùy cách triển khai, bạn có thể thấy 7 thay vì 8. Đây không phải bug! Điều đó chỉ phụ thuộc vào số lần function *thực sự* được lặp lại).

</div>