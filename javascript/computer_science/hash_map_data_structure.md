### Giới thiệu

Một trong những cấu trúc dữ liệu được dùng nhiều nhất trong các ngôn ngữ lập trình là hash table, còn gọi là hash map. Hash map tương tự array thông thường, ngoại trừ việc nó dùng "key" làm index thay vì cách đánh số tuần tự vô nghĩa cho value. Với dữ liệu được tổ chức như vậy, ta có tốc độ tìm kiếm key trong cấu trúc rất nhanh. Nếu từng làm việc với JavaScript Object Literal (`{}`), [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) hoặc [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), bạn đã dùng những cấu trúc dựa trên hash table. Nhưng bên trong chúng hoạt động thế nào? Làm sao lưu các cặp key-value rồi truy xuất chúng sau đó?

Trong bài học này, bạn sẽ tìm hiểu toàn bộ cách hoạt động và thậm chí triển khai hash map của riêng mình trong project tiếp theo! Trước hết, đây là mô tả ngắn về hash map: hash map nhận một cặp key-value, tạo hash code và lưu cặp đó vào một bucket. Hash code? Bucket? Gì cơ? Đừng lo, chúng ta sẽ học tất cả các khái niệm này và hơn thế nữa. Hãy thắt dây an toàn và cùng bắt đầu!

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài học này.

- Hash code và cách tạo chúng.
- Hash map và cách chúng hoạt động bên trong.

### Hash code là gì?

Hãy bắt đầu bằng việc tìm hiểu hash một value nghĩa là gì. Hashing là nhận một input và tạo output tương ứng. Hash function phải là pure function. Hash cùng một input phải luôn trả về cùng hash code và không được có thành phần tạo ngẫu nhiên. Ví dụ, hãy xem một hash function nhận tên và trả về chữ cái đầu tiên của tên:

```javascript
function hash(name) {
  return name.charAt(0);
}
```

Chúng ta đã tạo hash function cơ bản đầu tiên.

Có một khác biệt quan trọng giữa hashing và ciphering (encryption): khả năng đảo ngược.

Hashing là quá trình một chiều. Với ví dụ trên, bạn có thể tạo hash code từ một tên nhưng không thể lấy hash code để khôi phục tên. Nếu có tên `"Carlos"`, ta có thể hash nó thành `"C"`. Nhưng không thể đảo ngược từ `"C"` về dạng ban đầu. Bạn không thể biết đó là `"Carlos"`, có thể là `"Carla"` hoặc `"Carrot"`. Ta không biết.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Lợi ích của hashing

Hashing rất tốt cho security. Với một password, bạn có thể lưu hash của password thay vì plain text của password. Nếu ai đó lấy được các hash, họ không thể biết password ban đầu vì không thể đảo ngược hash về password.

</div>

#### Trường hợp sử dụng

Ta có thể làm gì với những hash đó? Có lẽ bạn từng thấy ở trường cách một thư mục được tổ chức thành các thư mục nhỏ hơn, mỗi thư mục chứa thông tin về những người có cùng chữ cái đầu:

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

Nếu trường có học sinh mới tên `"Carlos"`, ta có thể chạy hash function để tìm thư mục cần đặt bạn ấy. `hash("Carlos") -> "C"`, nên ta đặt `"Carlos"` vào thư mục có nhãn `C`.

Bạn có thể đã nhận ra một vấn đề: nếu trường có rất nhiều người tên bắt đầu bằng `C` thì sao? Khi đó ta có một thư mục `C` chứa quá nhiều tên trong khi các thư mục khác có thể rỗng. Để loại bỏ sự trùng lặp này và phân tách học sinh tốt hơn, ta cần chỉnh lại hash function.

```javascript
function hash(name, surname) {
  return name.charAt(0) + surname.charAt(0);
}
```

Thay vì chỉ lấy chữ cái đầu của tên, ta lấy chữ cái đầu của tên và họ. `"Carlos Smith"` sẽ có hash code là `"CS"`. Điều này phân tán học sinh vào nhiều thư mục hơn và loại bỏ nhiều hash code trùng lặp.

![Example of hashing a name using first name's first letter, and last name's first letter](https://cdn.statically.io/gh/TheOdinProject/curriculum/7ea463cfb7c05c330d72f5977cc5fe3b0c640b86/javascript/computer_science/hash_map_data_structure/imgs/00.png)

Nhưng vấn đề vẫn chưa được giải quyết. Nếu có một tổ hợp chữ cái đầu phổ biến trong tên học sinh thì sao? Khi đó kích thước các thư mục vẫn mất cân bằng. Ta cần làm cho việc tìm người dễ hơn, nên hãy chỉnh lại hash code.

```javascript
function stringToNumber(string) {
  let hashCode = 0;
  for (let i = 0; i < string.length; i++) {
    hashCode += string.charCodeAt(i);
  }

  return hashCode;
}

function hash(name, surname) {
  return stringToNumber(name) + stringToNumber(surname);
}
```

Với kỹ thuật này, ta không chỉ xét các chữ cái đầu. Thay vào đó, ta lấy toàn bộ tên và chuyển nó thành number.

Bạn có thể nghĩ: chẳng phải lưu cả tên làm hash code sẽ tốt hơn sao? Đúng vậy. Cách này làm mỗi tên trở nên duy nhất, nhưng trong ngữ cảnh hash map, ta cần hash code là một number. Number này sẽ làm index cho bucket lưu cặp key-value. Ta sẽ nói thêm về bucket ở phần tiếp theo.

### Bucket

Bucket là nơi lưu trữ các element. Ta có thể xem mỗi index của array là một bucket. Với một key cụ thể, ta quyết định dùng bucket nào để lưu trữ thông qua hash function. Hash function trả về một number đóng vai trò index của array, nơi ta lưu cặp key-value cụ thể này. Giả sử muốn lưu full name của một người với key là "Fred" và value là "Smith":

1. Truyền "Fred" vào hash function để nhận hash code là `385`.
1. Tìm bucket tại index `385`.
1. Lưu cặp key-value vào bucket đó. Trong trường hợp này, key là "Fred" và value là "Smith".

Nếu bucket tại index `385` đã chứa một item có cùng key "Fred" thì sao? Ta kiểm tra có phải cùng item không bằng cách so sánh các key, sau đó ghi đè value bằng value mới. Đây là cách ta có thể chỉ có các value duy nhất trong một `Set`. `Set` tương tự hash map, nhưng khác biệt chính (chơi chữ có chủ ý) là `Set` có node chỉ chứa key, không có value.

Đây là phần giải thích đơn giản hóa; chúng ta sẽ thảo luận thêm về cơ chế bên trong sau trong bài học.

Nếu muốn lấy value bằng key:

1. Để truy xuất value, ta hash key và tính index của bucket.
1. Nếu bucket không rỗng, ta đi tới bucket đó.
1. Bây giờ ta so sánh key của node có giống key được dùng để truy xuất hay không.
1. Nếu giống, ta trả về value của node. Nếu không, trả về `null`.

Có thể bạn thắc mắc tại sao phải so sánh các key nếu đã tìm được index của bucket. Hãy nhớ hash code chỉ là vị trí. Các key khác nhau có thể tạo ra cùng hash code. Ta cần bảo đảm key giống nhau bằng cách so sánh cả hai key nằm trong bucket.

Đó là toàn bộ ý tưởng; tạo như vậy sẽ cho ra hash map có `has`, `set` và `get`.

#### Thứ tự chèn không được giữ

Hash map không bảo đảm thứ tự chèn khi bạn iterate qua nó. Việc chuyển hash code thành index không tiến triển tuyến tính từ index đầu đến index cuối. Thay vào đó, nó khó dự đoán hơn, không phụ thuộc thứ tự chèn item. Điều đó có nghĩa nếu lấy array key và value để iterate, chúng sẽ không theo thứ tự bạn đã chèn.

Một số library triển khai hash map có xét thứ tự chèn, chẳng hạn `Map` của JavaScript. Tuy nhiên trong project sắp tới, chúng ta sẽ triển khai hash map không có thứ tự.
Ví dụ: nếu chèn value `Mao`, `Zach`, `Xari` theo thứ tự này, khi gọi iterator ta có thể nhận lại `["Zach", "Mao", "Xari"]`.

Nếu mục tiêu của bạn là iterate qua hash map thường xuyên, cấu trúc dữ liệu này không phù hợp. Một array đơn giản sẽ tốt hơn.

### Collision

Chúng ta còn một vấn đề cần xử lý: collision. Collision xảy ra khi hai key khác nhau tạo ra chính xác cùng hash code. Vì có cùng hash code, chúng rơi vào cùng bucket.

Ví dụ, hash tên `"Sara"` và tên `"raSa"` sẽ tạo cùng hash code. Đó là vì hai tên có cùng các chữ cái, chỉ được sắp xếp khác nhau.
Ta có thể chỉnh function `stringToNumber` để tạo hash code duy nhất phụ thuộc vào vị trí chữ cái trong tên bằng một algorithm.

```javascript
function stringToNumber(string) {
  let hashCode = 0;

  const primeNumber = 31;
  for (let i = 0; i < string.length; i++) {
    hashCode = primeNumber * hashCode + string.charCodeAt(i);
  }

  return hashCode;
}
```

Với function mới, tên `"Sara"` và `"raSa"` sẽ có hash code khác nhau. Điều này xảy ra vì dù hai tên có cùng chữ cái, một số chữ cái nằm ở vị trí khác nhau. Hash code bắt đầu thay đổi vì ta nhân hash cũ với mỗi iteration mới rồi cộng letter code.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Giảm collision

Hãy chú ý việc dùng số nguyên tố. Ta có thể chọn bất kỳ number nào, nhưng số nguyên tố được ưu tiên. Nhân với số nguyên tố làm giảm khả năng hash code chia hết cho bucket length, giúp giảm sự xuất hiện của collision.

</div>

Dù đã chỉnh hash function để tránh collision `"Sara"/"raSa"`, collision vẫn luôn có thể xảy ra. Vì số bucket hữu hạn, không có cách nào loại bỏ hoàn toàn collision. Hãy thử giảm thiểu chúng.

#### Xử lý collision

Đến giờ hash map của ta là cấu trúc dữ liệu một chiều. Nếu mỗi `Node` trong bucket có thể lưu nhiều value thì sao? Hãy dùng `Linked Lists`. Khi đó mỗi bucket sẽ là một Linked List. Khi chèn vào bucket, nếu bucket rỗng, ta chèn head của Linked List. Nếu bucket đã có head, ta đi theo Linked List để thêm vào cuối.

Có lẽ đến đây bạn đã hiểu tại sao phải viết hash function tốt để loại bỏ nhiều collision nhất có thể. Bạn gần như sẽ không tự viết hash function, vì hầu hết ngôn ngữ đều tích hợp sẵn, nhưng hiểu cách hash function hoạt động vẫn rất quan trọng.

### Tăng trưởng của hash map

Hãy nói về số bucket. Ta không có bộ nhớ vô hạn, nên không thể có vô hạn bucket. Cần bắt đầu từ đâu đó, nhưng bắt đầu quá lớn cũng lãng phí bộ nhớ nếu chỉ có một value trong hash map. Để giải quyết, ta nên bắt đầu bằng array bucket nhỏ. Ta sẽ dùng array kích thước `16`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Đặt kích thước ban đầu của array

Hầu hết ngôn ngữ lập trình bắt đầu với kích thước mặc định `16` vì đây là lũy thừa của 2, giúp một số kỹ thuật tối ưu cần bit manipulation cho index.

</div>

Làm thế nào chèn vào các bucket khi hash function tạo ra number lớn như `20353924`? Ta dùng phép toán modulo `%` `given any number modulo by 16 we will get a number between 0 and 15`.
Ví dụ, nếu muốn tìm bucket nơi value `"Manon"` sẽ rơi vào, ta làm như sau:
![hashing using hash code and modular operation example](https://cdn.statically.io/gh/TheOdinProject/curriculum/7ea463cfb7c05c330d72f5977cc5fe3b0c640b86/javascript/computer_science/hash_map_data_structure/imgs/01.png)

Khi tiếp tục thêm node vào bucket, collision ngày càng dễ xảy ra. Tuy nhiên cuối cùng sẽ có nhiều node hơn số bucket, điều này bảo đảm có collision (xem phần tài nguyên bổ sung để biết giải thích nếu bạn tò mò).

Hãy nhớ ta không muốn collision. Trong thế giới hoàn hảo, mỗi bucket chỉ có 0 hoặc 1 node, vì vậy ta tăng array bucket để các node có nhiều cơ hội phân tán hơn thay vì chồng lên cùng bucket. Để tăng array, ta tạo array mới có kích thước gấp đôi rồi sao chép tất cả node hiện có sang bucket của array mới, hash lại key của chúng.

#### Khi nào biết đã đến lúc tăng array bucket?

Để xử lý việc này, class hash map cần theo dõi thêm hai field: `capacity` và `load factor`.

- `capacity` là tổng số bucket hiện có.

- `load factor` là một number được gán cho hash map lúc bắt đầu. Đây là factor quyết định khi nào nên tăng array bucket. Các implementation hash map ở nhiều ngôn ngữ dùng load factor từ `0.75` đến `1`.

Tích của hai number này cho ta một number, và ta biết đã đến lúc tăng khi số entry trong hash map lớn hơn number đó. Ví dụ, nếu có `16` bucket và load factor là `0.8`, ta cần tăng array bucket khi có hơn `16 * 0.8 = 12.8` entry -- điều xảy ra ở entry thứ 13. Đặt quá thấp sẽ tốn nhiều bộ nhớ vì có quá nhiều bucket rỗng, còn đặt quá cao sẽ cho phép nhiều collision trước khi resize array.

### Độ phức tạp tính toán

Hash map rất hiệu quả trong các thao tác insertion, retrieval và removal. Đó là vì ta dùng array index để thực hiện các thao tác này. Hash map có complexity trung bình `O(1)` cho các method sau:

- Insertion
- Retrieval
- Removal

Giả sử ta viết hash map tốt. Trường hợp xấu nhất của các thao tác đó là `O(n)`, xảy ra khi toàn bộ data hash vào đúng một bucket. Complexity xuất hiện do linked list, và `O(n)` là vì ta phải duyệt linked list để lại chèn một node khác vào cùng bucket, cụ thể do collision.

Tăng trưởng của hash map luôn có complexity `O(n)`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

- Xem video từ CS50 giải thích khái niệm [hash maps using buckets](https://www.youtube.com/watch?v=btT4bCOvqjs).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Không bắt buộc, hãy xem như tài liệu bổ sung.

- Bài thảo luận này trình bày [usages of prime numbers in hash functions](https://stackoverflow.com/questions/299304/why-does-javas-hashcode-in-string-use-31-as-a-multiplier/299748).
- [Pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle) chứng minh về mặt toán học rằng collision xảy ra khi số node nhiều hơn số box.
- Xem [Hashing](https://samwho.dev/hashing/) nếu muốn hiểu nền tảng vững hơn về hash function.