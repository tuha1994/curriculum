### Giới thiệu

Một trong những cấu trúc dữ liệu được sử dụng nhiều nhất trong các ngôn ngữ lập trình là bảng băm, còn gọi là hash map. Hash map tương tự như một mảng thông thường, ngoại trừ việc nó sử dụng các "khóa" làm chỉ mục thay vì cách đánh số tuần tự vô nghĩa cho các giá trị. Nhờ dữ liệu được tổ chức theo cách này, chúng ta có tốc độ tìm kiếm khóa nhanh trong cấu trúc. Nếu bạn từng làm việc với hash của Ruby, thì bạn đã sử dụng các cấu trúc dựa trên bảng băm. Nhưng bên trong chúng hoạt động như thế nào? Làm sao chúng ta có thể lưu các cặp khóa–giá trị rồi truy xuất chúng sau đó?

Trong bài học này, bạn sẽ tìm hiểu toàn bộ cơ chế hoạt động và thậm chí tự triển khai hash map trong project tiếp theo! Trước hết, đây là mô tả ngắn gọn về hash map: hash map nhận vào một cặp khóa–giá trị, tạo ra một mã băm rồi lưu cặp đó vào một bucket. Mã băm? Bucket? Gì cơ? Đừng lo, chúng ta sẽ tìm hiểu tất cả những khái niệm này và nhiều điều khác nữa. Hãy sẵn sàng và cùng bắt đầu!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài.

- Mã băm và cách tạo mã băm.
- Hash map và cách chúng hoạt động bên trong.

### Mã băm là gì?

Hãy bắt đầu bằng việc tìm hiểu ý nghĩa của thao tác băm một giá trị. Băm là quá trình nhận một đầu vào và tạo ra đầu ra tương ứng. Một hàm băm nên là một hàm thuần túy. Cùng một đầu vào luôn phải trả về cùng một mã băm, và không được có thành phần tạo ngẫu nhiên. Ví dụ, hãy xem một phương thức băm nhận vào một cái tên và trả về chữ cái đầu tiên của tên đó:

```ruby
def hash(name)
  name[0]
end
```

Chúng ta vừa tạo phương thức băm cơ bản đầu tiên.

Có một điểm khác biệt quan trọng giữa băm và mã hóa: khả năng đảo ngược.
Băm là một quá trình một chiều. Với ví dụ trên, bạn có thể tạo mã băm từ một cái tên, nhưng không thể lấy mã băm rồi khôi phục nó thành cái tên ban đầu. Nếu có cái tên `"Carlos"`, chúng ta có thể băm nó thành `"C"`. Nhưng không thể đảo ngược `"C"` về dạng ban đầu. Bạn không thể biết đó là `"Carlos"`, hay có thể là `"Carla"` hoặc `"Carrot"`. Chúng ta không biết được.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Giảm thiểu va chạm

Băm rất hữu ích cho bảo mật. Với một mật khẩu, bạn có thể lưu mã băm của mật khẩu đó thay vì lưu văn bản thuần của mật khẩu. Nếu ai đó đánh cắp các mã băm, họ không thể biết mật khẩu ban đầu vì không thể đảo ngược mã băm để lấy lại mật khẩu.

</div>

#### Trường hợp sử dụng

Chúng ta có thể làm gì với những mã băm đó? Có lẽ bạn từng thấy trong trường học cách một thư mục được tổ chức thành các thư mục nhỏ hơn, trong đó mỗi thư mục chứa thông tin về những người có cùng chữ cái đầu tiên:

```text
C:
  carlos.txt
  carla.txt
B:
  bryan.txt
  bob.txt
  beatrice.txt
  bella.txt
  benjamin.txt
  bianca.txt
```

Nếu trường học có thêm một học sinh tên `"Carlos"`, chúng ta có thể chạy phương thức băm để tìm thư mục cần đặt bạn ấy vào. `hash("Carlos") -> "C"`, vì vậy chúng ta đặt `"Carlos"` vào thư mục có nhãn `C`.

Bạn có thể đã nhận ra một vấn đề: nếu trường học có nhiều người có tên bắt đầu bằng chữ `C` thì sao? Khi đó chúng ta sẽ có một thư mục mang nhãn `C` chứa quá nhiều tên, trong khi các thư mục khác có thể trống. Để loại bỏ sự trùng lặp này và phân chia học sinh tốt hơn, chúng ta cần thiết kế lại phương thức băm.

```ruby
def hash(name, surname)
  name[0] + surname[0]
end
```

Thay vì chỉ lấy chữ cái đầu của tên, chúng ta lấy chữ cái đầu của cả tên và họ. `"Carlos Smith"` sẽ có mã băm là `"CS"`. Cách này sẽ phân tán học sinh vào nhiều thư mục hơn và loại bỏ nhiều mã băm trùng lặp được tạo ra.

![Ví dụ băm một cái tên bằng chữ cái đầu tiên của tên và chữ cái đầu tiên của họ](https://cdn.statically.io/gh/TheOdinProject/curriculum/7ea463cfb7c05c330d72f5977cc5fe3b0c640b86/javascript/computer_science/hash_map_data_structure/imgs/00.png)

Tuy nhiên, cách này vẫn chưa giải quyết được vấn đề. Nếu nhiều học sinh có cùng một tổ hợp chữ cái đầu phổ biến thì sao? Khi đó kích thước các thư mục vẫn sẽ mất cân đối. Chúng ta cần giúp việc tìm người mình cần trở nên dễ dàng hơn, nên hãy tiếp tục thiết kế lại mã băm.

```ruby
def string_to_number(string)
  hash_code = 0
  string.each_char { |char| hash_code += char.ord }
  hash_code
end

def hash(name, surname)
  string_to_number(name) + string_to_number(surname)
end
```

Với kỹ thuật này, chúng ta không chỉ xét các chữ cái đầu. Thay vào đó, chúng ta lấy toàn bộ tên và chuyển nó thành các số.

Bạn có thể nghĩ rằng lưu cả cái tên làm mã băm chẳng phải sẽ tốt hơn sao? Điều đó đúng. Cách này sẽ tạo giá trị duy nhất cho mỗi tên, nhưng trong ngữ cảnh của hash map, chúng ta cần mã băm là một con số. Con số này sẽ đóng vai trò là chỉ mục của bucket lưu cặp khóa–giá trị. Chúng ta sẽ tìm hiểu thêm về bucket trong phần tiếp theo.

### Bucket

Bucket là nơi lưu trữ các phần tử. Nói đơn giản, đó là một mảng. Với một khóa cụ thể, chúng ta quyết định sử dụng bucket nào bằng phương thức băm. Phương thức băm trả về một con số, đóng vai trò là chỉ mục của mảng nơi chúng ta lưu cặp khóa–giá trị cụ thể đó. Giả sử chúng ta muốn lưu tên đầy đủ của một người làm khóa là "Fred" với giá trị là "Smith":

1. Truyền "Fred" vào phương thức băm để lấy mã băm là `508`.
1. Tìm bucket tại chỉ mục `508`.
1. Lưu cặp khóa–giá trị vào bucket đó. Trong trường hợp này, khóa là "Fred" và giá trị là "Smith".

Đây là một cách giải thích được đơn giản hóa quá mức; chúng ta sẽ thảo luận thêm về cơ chế bên trong ở phần sau của bài học.

Nếu bây giờ muốn lấy một giá trị bằng khóa:

1. Đặt mỗi mục vào trong một bucket dưới dạng một phần tử `Node`, phần tử này chứa cả khóa và giá trị.
1. Để truy xuất giá trị, chúng ta băm khóa và tính số hiệu bucket tương ứng.
1. Nếu bucket không trống, chúng ta đi đến bucket đó.
1. Sau đó, chúng ta so sánh xem khóa của node có giống với khóa được dùng để truy xuất hay không.
1. Nếu giống, chúng ta có thể trả về giá trị của node. Nếu không, chúng ta trả về `nil`.

Có thể bạn thắc mắc tại sao phải so sánh các khóa nếu chúng ta đã tìm được chỉ mục của bucket. Hãy nhớ rằng mã băm chỉ là vị trí. Những khóa khác nhau có thể tạo ra cùng một mã băm. Chúng ta cần đảm bảo khóa là khóa cần tìm bằng cách so sánh cả hai khóa nằm trong bucket.

Vậy là xong; triển khai như vậy sẽ tạo ra một bảng băm có `has?`, `set` và `get`.

Nếu chúng ta tìm được mã băm nhưng khóa cũng giống với khóa đã có trong bucket thì sao? Chúng ta kiểm tra xem đó có phải cùng một mục hay không bằng cách so sánh các khóa, sau đó ghi đè giá trị bằng giá trị mới. Đây là cách chúng ta chỉ có các giá trị duy nhất bên trong một `Set`. `Set` tương tự như hash map, nhưng điểm khác biệt cốt lõi (chơi chữ có chủ ý) là `Set` có các node chỉ chứa khóa mà không có giá trị.

#### Không duy trì thứ tự chèn

Hash map không đảm bảo thứ tự chèn khi bạn lặp qua nó. Việc chuyển mã băm thành các chỉ mục không tuân theo tiến trình tuyến tính từ chỉ mục đầu tiên đến chỉ mục cuối cùng. Thay vào đó, thứ tự này khó đoán hơn, không phụ thuộc vào thứ tự các mục được chèn. Điều đó có nghĩa là nếu bạn lấy mảng khóa và giá trị để lặp qua, chúng sẽ không xuất hiện theo thứ tự bạn đã chèn chúng.

Một số triển khai vẫn duy trì thứ tự chèn, chẳng hạn như `Hash` của chính Ruby. Tuy nhiên, trong project sắp tới, chúng ta sẽ triển khai một bảng băm không có thứ tự.
Ví dụ: nếu chèn các giá trị `Mao`, `Zach`, `Xari` theo thứ tự này, khi gọi một iterator, chúng ta có thể nhận lại `["Zach", "Mao", "Xari"]`.

Nếu mục tiêu của bạn là lặp qua hash map thường xuyên, thì cấu trúc dữ liệu này không phù hợp; một mảng đơn giản sẽ tốt hơn.

### Va chạm

Chúng ta còn một vấn đề khác cần xử lý: va chạm. Va chạm xảy ra khi hai khóa khác nhau tạo ra chính xác cùng một mã băm. Vì có cùng mã băm, chúng sẽ rơi vào cùng một bucket.

Hãy lấy một ví dụ: băm tên `"Sara"` và tên `"raSa"` sẽ tạo ra cùng một mã băm. Đó là vì các chữ cái trong hai cái tên giống nhau, chỉ khác cách sắp xếp.
Chúng ta có thể thiết kế lại phương thức `string_to_number` để nó tạo ra các mã băm duy nhất, phụ thuộc vào vị trí xuất hiện của các chữ cái trong tên, bằng một thuật toán.

```ruby
def string_to_number(string)
  hash_code = 0
  prime_number = 31

  string.each_char { |char| hash_code = prime_number * hash_code + char.ord }

  hash_code
end
```

Với phương thức mới, chúng ta sẽ có các mã băm khác nhau cho tên `"Sara"` và `"raSa"`. Đó là vì dù hai tên có cùng các chữ cái, một số chữ cái xuất hiện ở vị trí khác nhau. Mã băm bắt đầu thay đổi vì chúng ta nhân mã băm cũ ở mỗi lần lặp mới rồi cộng thêm mã của chữ cái.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Lợi ích của việc băm

Hãy chú ý đến việc sử dụng một số nguyên tố. Chúng ta có thể chọn bất kỳ con số nào, nhưng số nguyên tố được ưu tiên hơn. Nhân với một số nguyên tố sẽ làm giảm khả năng các mã băm chia hết cho độ dài bucket, từ đó giúp giảm thiểu số lần va chạm.

</div>

Dù đã thiết kế lại phương thức băm để tránh va chạm `"Sara"/"raSa"`, va chạm vẫn luôn có thể xảy ra. Vì chúng ta chỉ có hữu hạn bucket, không có cách nào loại bỏ hoàn toàn va chạm. Hãy cố gắng giảm thiểu chúng.

#### Xử lý va chạm

Cho đến lúc này, hash map của chúng ta là một cấu trúc dữ liệu một chiều. Nếu mỗi `Node` bên trong bucket có thể lưu nhiều hơn một giá trị thì sao? Đó chính là lúc `Linked Lists` xuất hiện. Bây giờ, mỗi bucket sẽ là một Linked List. Khi chèn vào bucket, nếu bucket trống, chúng ta chèn phần tử đầu của Linked List. Nếu bucket đã có phần tử đầu, chúng ta lần theo Linked List để thêm vào cuối danh sách.

Đến đây, có lẽ bạn đã hiểu tại sao chúng ta phải viết một phương thức băm tốt, có thể loại bỏ càng nhiều va chạm càng tốt. Phần lớn khả năng là bạn sẽ không tự viết phương thức băm, vì hầu hết ngôn ngữ đã tích hợp sẵn, nhưng hiểu cách các phương thức băm hoạt động vẫn rất quan trọng.

### Tăng trưởng của bảng băm

Hãy nói về việc các bucket tăng trưởng. Chúng ta không có bộ nhớ vô hạn, nên không thể có vô hạn bucket. Chúng ta cần bắt đầu từ đâu đó, nhưng bắt đầu quá lớn cũng là lãng phí bộ nhớ nếu hash map chỉ có một giá trị. Vì vậy, để xử lý vấn đề này, chúng ta nên bắt đầu với một mảng bucket nhỏ. Chúng ta sẽ dùng một mảng có kích thước `16`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Đặt kích thước ban đầu của mảng

Hầu hết ngôn ngữ lập trình bắt đầu với kích thước mặc định là `16` vì đây là lũy thừa của 2, giúp ích cho một số kỹ thuật tối ưu hiệu năng cần thao tác bit để tính chỉ mục.

</div>

Chúng ta sẽ chèn vào các bucket đó thế nào khi phương thức băm tạo ra những con số lớn như `20353924`? Chúng ta sử dụng phép toán modulo `%`, với quy tắc `given any number modulo by 16 we will get a number between 0 and 15`.
Ví dụ, nếu muốn tìm bucket mà giá trị `"Manon"` sẽ rơi vào, chúng ta làm như sau:
![ví dụ băm bằng mã băm và phép toán modulo](https://cdn.statically.io/gh/TheOdinProject/curriculum/7ea463cfb7c05c330d72f5977cc5fe3b0c640b86/javascript/computer_science/hash_map_data_structure/imgs/01.png)

Khi tiếp tục thêm node vào các bucket, va chạm ngày càng dễ xảy ra. Tuy nhiên, đến một lúc sẽ có nhiều node hơn số bucket, điều này chắc chắn gây ra va chạm (hãy xem phần tài nguyên bổ sung để tìm hiểu lời giải thích cho sự thật này nếu bạn tò mò).

Hãy nhớ rằng chúng ta không muốn có va chạm. Trong thế giới lý tưởng, mỗi bucket chỉ có 0 hoặc 1 node, vì vậy chúng ta tăng kích thước mảng bucket để tăng khả năng các node được phân tán thay vì chồng chất trong cùng một bucket. Để tăng kích thước mảng, chúng ta tạo một mảng mới có kích thước gấp đôi rồi sao chép tất cả node hiện có sang các bucket của mảng mới này, đồng thời băm lại khóa của chúng.

#### Làm sao biết đã đến lúc tăng kích thước bucket?

Để xử lý việc này, lớp hash map cần theo dõi thêm hai trường mới là `capacity` và `load factor`.

- `capacity` là tổng số bucket hiện có.

- `load factor` là một con số được gán cho hash map ngay từ đầu. Đây là hệ số quyết định thời điểm thích hợp để tăng kích thước bucket. Các triển khai hash map trong nhiều ngôn ngữ sử dụng load factor nằm giữa `0.75` và `1`.

Tích của hai con số này cho chúng ta một giá trị, và chúng ta biết đã đến lúc tăng kích thước khi số mục trong hash map lớn hơn giá trị đó. Ví dụ, nếu có `16` bucket và load factor là `0.8`, chúng ta cần tăng kích thước bucket khi có hơn `16 * 0.8 = 12.8` mục — tức là khi thêm mục thứ 13. Đặt giá trị quá thấp sẽ tiêu tốn quá nhiều bộ nhớ vì có quá nhiều bucket trống, còn đặt quá cao sẽ cho phép nhiều va chạm xảy ra trước khi chúng ta tăng kích thước.

### Độ phức tạp tính toán

Hash map rất hiệu quả trong các thao tác chèn, truy xuất và xóa. Đó là vì chúng ta dùng chỉ mục mảng để thực hiện các thao tác này. Trong trường hợp trung bình, hash map có độ phức tạp `O(1)` cho các phương thức sau:

- Chèn
- Truy xuất
- Xóa

Giả sử chúng ta đã viết một hash map tốt. Trường hợp xấu nhất của các thao tác này là `O(n)`, xảy ra khi toàn bộ dữ liệu của chúng ta được băm vào chính xác cùng một bucket. Độ phức tạp này xuất hiện do linked list; `O(n)` là vì chúng ta phải duyệt linked list để tiếp tục chèn thêm một node vào cùng bucket, điều xảy ra cụ thể do va chạm.

Việc tăng kích thước hash map luôn có độ phức tạp `O(n)`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

- Xem video này của CS50, trong đó giải thích khái niệm [hash map bằng bucket](https://www.youtube.com/watch?v=btT4bCOvqjs).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Cuộc thảo luận này trình bày [cách sử dụng số nguyên tố trong các hàm băm](https://stackoverflow.com/questions/299304/why-does-javas-hashcode-in-string-use-31-as-a-multiplier/299748).

- [Nguyên lý Dirichlet](https://en.wikipedia.org/wiki/Pigeonhole_principle) đảm bảo về mặt toán học rằng sẽ có va chạm khi số node nhiều hơn số hộp.
