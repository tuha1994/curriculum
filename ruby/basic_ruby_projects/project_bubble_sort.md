### Giới thiệu

Các thuật toán sắp xếp là một trong những loại thuật toán đầu tiên mà bạn thường được làm quen trong ngành Khoa học máy tính. Có thể ngay lập tức bạn chưa thấy rõ chúng quan trọng đến mức nào, nhưng hẳn không khó để nghĩ ra một vài ví dụ trong đó máy tính của bạn cần sắp xếp những tập dữ liệu khổng lồ trong các thao tác hằng ngày.

Một trong những cách đơn giản hơn (nhưng tiêu tốn nhiều năng lực xử lý hơn) để sắp xếp một nhóm phần tử trong một mảng là **sắp xếp nổi bọt**, trong đó mỗi phần tử được so sánh với phần tử ngay bên cạnh và chúng được hoán đổi nếu phần tử bên trái lớn hơn phần tử bên phải. Quá trình này tiếp tục cho đến khi mảng được sắp xếp hoàn toàn.

Hãy xem [video về Sắp xếp nổi bọt của CS50 tại Harvard](https://www.youtube.com/watch?v=8Kp-8OGwphY).

Ngoài ra còn có [mục viết về Sắp xếp nổi bọt trên Wikipedia](http://en.wikipedia.org/wiki/Bubble_sort) mà bạn nên xem qua.

![Cách hoạt động của sắp xếp nổi bọt](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/ruby_programming/basic_ruby_projects/bubble_sort/img/00.gif)

### Bài tập

<div class="lesson-content__panel" markdown="1">

- Xây dựng một phương thức `#bubble_sort` nhận vào một mảng và trả về một mảng đã được sắp xếp. Phương thức này phải sử dụng cách thức sắp xếp nổi bọt (nếu dùng `#sort` thì còn gì thú vị nữa, đúng không?).

```ruby
> bubble_sort([4,3,78,2,0,2])
=> [0,2,2,3,4,78]
```

</div>
