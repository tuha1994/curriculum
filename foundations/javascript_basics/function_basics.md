### Giới thiệu

Mọi thứ sắp trở nên *thật sự* thú vị. Cho đến nay, bạn đã viết một lượng code ấn tượng để giải quyết nhiều vấn đề khác nhau, nhưng code đó chưa hữu ích được như khả năng vốn có.

Hãy tưởng tượng bạn lấy một script, đóng gói nó thành một package nhỏ để có thể dùng đi dùng lại mà không phải viết lại hay thay đổi code. Đó chính là sức mạnh của function, và chúng được sử dụng *liên tục* trong JavaScript.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Cách định nghĩa và gọi nhiều loại function khác nhau.
- Cách sử dụng giá trị trả về.
- Function scope là gì.

### Function

Hãy cùng bàn về parameter và argument trong ngữ cảnh của function ví dụ sau:

```javascript
function favoriteAnimal(animal) {
    return animal + " is my favorite animal!"
}

const message = favoriteAnimal('Goat')
console.log(message)
```

Trong JavaScript, parameter là các mục được liệt kê giữa hai dấu ngoặc đơn `()` trong khai báo function. Argument của function là những giá trị thực tế mà chúng ta quyết định truyền vào function.

Trong ví dụ trên, phần định nghĩa function được viết ở dòng đầu tiên: `function favoriteAnimal(animal)`. Parameter `animal` nằm bên trong dấu ngoặc đơn. Chúng ta cũng có thể dễ dàng thay `animal` bằng `pet`, `x` hoặc `blah`. Nhưng trong trường hợp này, đặt tên parameter là `animal` giúp người đọc code có thêm ngữ cảnh, để họ không phải đoán xem sau này `animal` có thể chứa gì.

Khi đặt `animal` bên trong dấu ngoặc đơn của khai báo function `favoriteAnimal`, chúng ta đang cho JavaScript biết rằng sẽ truyền *một* giá trị nào đó vào function `favoriteAnimal`. Điều này có nghĩa là `animal` chỉ là một **placeholder** cho một giá trị sẽ xuất hiện sau. Nhưng giá trị chúng ta truyền vào là gì?

Ở dòng thứ năm, `favoriteAnimal('Goat')` là nơi chúng ta gọi function `favoriteAnimal` và truyền giá trị `'Goat'` vào trong lời gọi function đó. Ở đây, `'Goat'` là argument của chúng ta. Chúng ta đang nói với function `favoriteAnimal`: "Hãy truyền `'Goat'` vào function `favoriteAnimal` và dùng `'Goat'` ở mọi nơi có placeholder `animal`." Nhờ tính linh hoạt mà parameter mang lại, chúng ta có thể chọn bất kỳ con vật nào làm con vật yêu thích.

Sơ đồ dưới đây sẽ giúp bạn hình dung cách parameter được truyền vào một function và cách các giá trị được trả về từ đó.

![cách parameter được truyền vào một function và cách các giá trị được trả về từ đó](https://cdn.statically.io/gh/TheOdinProject/curriculum/c53dd9a12f0c9afde0d9229f82a176170f12e120/foundations/javascript_basics/function_basics/imgs/00.png)

Lưu ý rằng dù chúng ta lưu kết quả của `favoriteAnimal('Goat')` vào biến `message`, rồi sau đó truyền `message` làm argument cho `log()`, ta cũng có thể truyền một lời gọi function làm argument trong một lời gọi function khác.

```javascript
function favoriteAnimal(animal) {
    return animal + " is my favorite animal!"
}

console.log(favoriteAnimal('Goat'))
```

Trong ví dụ trên, chúng ta truyền `favoriteAnimal('Goat')` làm argument cho `log()`. Khi đó, giá trị trả về của lời gọi function `favoriteAnimal('Goat')` (chuỗi `"Goat is my favorite animal!"`) được dùng làm argument cho `log()` mà không cần lưu trước vào một biến riêng.

Hãy ghi nhớ khả năng này, vì bạn sẽ thường xuyên truyền các lời gọi function làm argument. Nếu chỉ gọi riêng `favoriteAnimal('Goat')` mà không truyền nó làm argument cho `console.log` để in giá trị trả về, nó vẫn trả về chuỗi đó... nhưng chẳng đi đến đâu cả. Không có gì xuất hiện trong console và chuỗi cũng không được lưu vào biến nào.

Bạn cứ tự thử nghiệm với code và thay `'Goat'` bằng con vật yêu thích của mình. Bạn thấy chúng ta có thể thay argument bằng bất kỳ thứ gì mình muốn chứ? Hãy thử thay `animal` trong phần khai báo function và trong phần thân function nữa. Điều gì xảy ra?

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note lesson-note--warning" markdown="1">

#### Bài tập MDN

Lưu ý rằng các bài viết MDN được giao cũng có bài tập riêng đi kèm; bạn **không nên** làm những bài đó, vì chúng cần đến các kiến thức chúng ta chưa học.

</div>

1. Đọc bài viết [function basics](http://javascript.info/function-basics) của JavaScript.info. Chúng ta đã đề cập đến điều này trước đây, nhưng JavaScript đã thay đổi đôi chút qua nhiều năm và function gần đây đã có thêm một số cải tiến. Bài viết này trình bày một khả năng mới khá hữu ích: 'default parameters'. \(LƯU Ý: Phần "Functions == Comments" cũng như "task" cuối bài sử dụng vòng lặp, nội dung mà bạn sẽ học trong bài tiếp theo. Đừng lo về chúng.\)
1. Đọc [bài viết của MDN về function](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions). Đừng lo nếu có một số function nằm ngoài phạm vi của bài học này, nhưng hãy đặc biệt chú ý đến các phần về 'Function Scope'. Scope là chủ đề thường khiến cả người mới bắt đầu lẫn lập trình viên trình độ trung cấp vướng mắc, nên dành thời gian tìm hiểu sớm sẽ rất đáng giá.
1. Đọc bài viết này về [giá trị trả về](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Return_values).
1. Tiếp theo, đọc bài về [function expressions](http://javascript.info/function-expressions) trong JavaScript để có thêm ngữ cảnh, rồi đọc bài về [arrow functions](http://javascript.info/arrow-functions-basics) để làm quen với arrow function. Arrow function hữu ích nhưng chưa mang tính thiết yếu, nên hiện tại bạn chưa cần quá lo về chúng. Chúng tôi đưa phần này vào đây vì có thể bạn sẽ gặp chúng khi học tiếp, và tốt hơn hết là bạn có ít nhất *một chút* hình dung về thứ mình đang nhìn thấy mỗi khi chúng xuất hiện.
1. Cuối cùng, tìm hiểu về [JavaScript Call Stack](https://www.javascripttutorial.net/javascript-call-stack/). Bài viết đi sâu vào call stack và cách `return` hoạt động trong ngữ cảnh của các lời gọi function liên kết với nhau. Đừng lo nếu bạn chưa hiểu hoàn toàn ngay bây giờ, nhưng điều quan trọng là phải để ý các giá trị được `return` của bạn đang đi về đâu. Đây cũng là một phần nhập môn khoa học máy tính.

Hãy cùng viết một số function! Viết chúng trong thẻ `script` của một file HTML khung. Nếu bạn quên cách thiết lập, hãy xem lại hướng dẫn của chúng tôi về [cách chạy code JavaScript](https://www.theodinproject.com/lessons/foundations-fundamentals-part-1#how-to-run-javascript-code).

Hiện tại, chỉ cần viết từng function và kiểm tra output bằng `console.log`.

1. Viết một function có tên `add7`, nhận một số và trả về số đó + 7.
    - `add7(10)` phải trả về `17`
1. Viết một function có tên `multiply`, nhận 2 số và trả về tích của chúng.
    - `multiply(3, 2)` phải trả về `6`
1. Viết một function có tên `capitalize`, nhận một chuỗi và trả về chuỗi đó với *chỉ* chữ cái đầu tiên được viết hoa. Hãy bảo đảm function xử lý được chuỗi viết thường, VIẾT HOA hoặc ViẾt HoA.
    - `capitalize("abcd")` phải trả về `"Abcd"`
    - `capitalize("ABCD")` phải trả về `"Abcd"`
    - `capitalize("aBcD")` phải trả về `"Abcd"`
1. Viết một function có tên `lastLetter`, nhận một chuỗi và trả về chữ cái cuối cùng của chuỗi đó:
    - `lastLetter("abcd")` phải trả về `"d"`

</div>