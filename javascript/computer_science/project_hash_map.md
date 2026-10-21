### Giới thiệu

Bạn đã biết nguyên lý phía sau hash map. Bây giờ là lúc tự viết implementation của mình!

#### Giới hạn

Trước khi bắt đầu, chúng ta cần đặt ra một số quy tắc. Tính chất dynamic của array trong JavaScript cho phép chèn và truy xuất các index nằm ngoài phạm vi kích thước array. Ví dụ, nếu tạo array kích thước `16` để đại diện cho các bucket, không có gì ngăn chúng ta lưu phần tử tại index `500`. Điều này đi ngược mục đích giới hạn dung lượng lưu trữ của hash map, vì vậy chúng ta cần áp đặt một số hạn chế.

Dùng đoạn snippet sau mỗi khi truy cập bucket thông qua một index. Ta muốn throw error nếu cố truy cập index nằm ngoài phạm vi:

```javascript
if (index < 0 || index >= buckets.length) {
  throw new Error("Trying to access index out of bounds");
}
```

### Bài tập

<div class="lesson-content__panel" markdown="1">

Bắt đầu bằng cách tạo class hoặc factory function `HashMap`. Bạn có thể tự chọn cách nào. Nó phải có ít nhất hai property hoặc variable cho `load factor` và `capacity`. Với `load factor` bằng `0.75`, `capacity` ban đầu phải có kích thước `16`. Sau đó tạo các method sau:

1. `hash(key)` nhận một string key và tạo hash code từ đó (trong thực tế, hash map có thể dùng nhiều kiểu dữ liệu làm key, như number hoặc thậm chí object khác, nhưng hiện tại ta sẽ giữ mọi thứ đơn giản). Chúng ta đã triển khai một function `hash` khá tốt trong bài trước. Nhắc lại:

   ```javascript
   function hash(key) {
     let hashCode = 0;

     const primeNumber = 31;
     for (let i = 0; i < key.length; i++) {
       hashCode = primeNumber * hashCode + key.charCodeAt(i);
     }

     return hashCode;
   } 
   ```

   Bạn có thể dùng function đó hoặc tự nghiên cứu về hashing algorithm. Hãy cẩn thận, đây là một cái hố thỏ rất, rất sâu. Để bảo đảm index vừa với các bucket (bất kể có tăng trưởng), ta cần lấy hash code modulo (`%`) với capacity hiện tại trước khi trả về. Một edge case quan trọng là với key rất dài, ta có thể vượt quá [maximum safe integer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER), khiến phép tính không chính xác. Vì vậy, hợp lý hơn là lấy hash code modulo với capacity hiện tại ở mỗi vòng lặp, thay vì chỉ làm một lần sau khi vòng lặp kết thúc.

   Về sau, khi truy cập các cặp key-value, bạn có thể nhầm key với hash code. Key là các string do user cung cấp và được truyền vào function hash, function này sau đó trả về hash code. Bạn sẽ không bao giờ truy cập trực tiếp bucket bằng key, mà chỉ bằng hash code.

1. `set(key, value)` nhận hai đối số: đối số đầu là key và đối số thứ hai là value gắn với key. Nếu key đã tồn tại trong hash map, value cũ phải được ghi đè bằng value mới.

   Lưu ý: khi hai key *khác nhau* tạo ra cùng hash code và được gán vào cùng bucket, đó là collision chứ không phải update. Ví dụ, giả sử `Rama` và `Sita` đều được hash thành `3`. Chúng sẽ nằm trong cùng bucket nhưng không ghi đè lên nhau; ta biết đây là collision chứ không phải update vì hai key khác nhau. Nếu cần, xem lại [hash map lesson on collisions](https://www.theodinproject.com/lessons/javascript-hashmap-data-structure#collisions).

   Hãy nhớ tăng các bucket lên gấp đôi capacity khi hash map vượt `load factor`. Các method được đề cập sau trong bài tập có thể giúp bạn xử lý logic tăng trưởng, nên bạn có thể để việc triển khai hành vi này đến sau.

1. `get(key)` nhận một key và trả về value gắn với nó. Nếu không tìm thấy key, trả về `undefined`.

1. `has(key)` nhận một key và trả về boolean dựa trên việc key có nằm trong hash map hay không.

1. `remove(key)` nhận một key. Nếu key nằm trong hash map, xóa entry có key đó rồi trả về `true`. Nếu key không nằm trong hash map, trả về `false`.

1. `length()` trả về số key được lưu trong hash map.

1. `clear()` xóa mọi entry trong hash map.

1. `keys()` trả về một array chứa tất cả key (không phải value) bên trong hash map.

1. `values()` trả về một array chứa tất cả value (không phải key) bên trong hash map.

1. `entries()` trả về một array chứa từng cặp key-value trong các array riêng, ví dụ: `[[firstKey, firstValue], [secondKey, secondValue]]`.

Hãy nhớ hash map của chúng ta không giữ thứ tự chèn khi truy xuất dữ liệu. Việc key và value xuất hiện không theo thứ tự đã chèn là bình thường và được dự kiến.

#### Kiểm thử hash map

1. Tạo một file JavaScript mới.

1. Tạo một instance mới của hash map và đặt load factor thành `0.75`.

   ```javascript
   const test = new HashMap() // or HashMap() if using a factory
   ```

1. Nạp dữ liệu vào hash map bằng method `set(key, value)` và sao chép các dòng sau:

   ```javascript
   test.set('apple', 'red')
   test.set('banana', 'yellow')
   test.set('carrot', 'orange')
   test.set('dog', 'brown')
   test.set('elephant', 'gray')
   test.set('frog', 'green')
   test.set('grape', 'purple')
   test.set('hat', 'black')
   test.set('ice cream', 'white')
   test.set('jacket', 'blue')
   test.set('kite', 'pink')
   test.set('lion', 'golden')
   ```

1. Sau khi nạp dữ liệu trên, mức tải hiện tại của hash map phải là `0.75` (đầy capacity).

1. Với hash map đã đầy, thử ghi đè một vài node bằng `set(key, value)`. Việc này chỉ ghi đè `values` hiện có của node, không thêm node mới, vì vậy `length()` vẫn phải trả về cùng giá trị và `capacity` phải giữ nguyên.

1. Sau đó, nạp node cuối cùng bên dưới vào hash map. Việc này khiến mức tải vượt `load factor`, kích hoạt chức năng tăng trưởng và tăng gấp đôi `capacity`:

   ```javascript
   test.set('moon', 'silver')
   ```

1. Nếu triển khai hash map đúng, mức tải của hash map đã mở rộng sẽ giảm xuống thấp hơn nhiều so với load factor, và các entry sẽ được phân bố đều giữa các bucket mở rộng.

1. Với hash map mới, thử ghi đè một vài node bằng `set(key, value)`. Một lần nữa, việc này chỉ ghi đè `values` hiện có của node.

1. Kiểm thử các method khác của hash map như `get(key)`, `has(key)`, `remove(key)`, `length()`, `clear()`, `keys()`, `values()` và `entries()`, để kiểm tra chúng vẫn hoạt động như dự kiến sau khi hash map được mở rộng.

#### Điểm cộng

- Tạo class hoặc factory function `HashSet` hoạt động giống `HashMap` nhưng chỉ chứa `keys`, không có `values`.

</div>