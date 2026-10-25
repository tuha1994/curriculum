### Giới thiệu

Còn nhớ JavaScript không? Chúng ta học nó để làm gì nhỉ? Đôi khi bạn muốn trình duyệt có thêm một chút tính tương tác,
chẳng hạn ẩn/hiện một phần trang. Việc thực hiện một vòng request đến server rồi quay lại là không thực tế. Đó là lúc JavaScript
phát huy tác dụng: Rails gợi ý sử dụng framework JS Stimulus để bổ sung khả năng cho HTML. Stimulus cho bạn cách tạo và sử dụng các
controller có thể tái sử dụng, giúp thêm một chút tính tương tác.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Cách gắn Stimulus controller vào HTML.
- Cách sử dụng target thay cho query selector.
- Cách sử dụng action attribute thay cho event listener.
- Nên lưu state ở đâu.
- Cách sử dụng lifecycle callback.
- Cách làm cho controller có thể tái sử dụng.
- Cách tùy biến controller bằng attribute.

### Ý tưởng

Stimulus là một framework khiêm tốn. Nó để phần lớn công việc cho bạn, nhưng cung cấp một cách nhất quán để thực hiện mọi việc.
Nó sử dụng HTML `data-attributes` để gắn và cấu hình hành vi cho HTML. Hãy xem một ví dụ HTML
trong handbook:

```html
<div data-controller="clipboard">
  PIN: <input data-clipboard-target="source" type="text" value="3737" readonly>
  <button data-action="click->clipboard#copy">Copy to Clipboard</button>
</div>
```

Hãy đọc kỹ ví dụ và đặc biệt chú ý đến các data attribute! Bạn đoán được ví dụ này làm gì không?

Ví dụ này sử dụng ba data attribute: `data-controller`, `data-clipboard-target`, `data-action`. Vậy chúng có
mục đích gì?

- **data-controller** được đặt thành `clipboard`, nghĩa là "Này Stimulus, chúng ta muốn bổ sung
một số hành vi clipboard cho HTML này."
- **data-clipboard-target** được đặt thành `source`, nghĩa là "Đây là `source` chúng ta sẽ dùng cho clipboard
controller đó."
- **data-action** được đặt thành `click->clipboard#copy`, nghĩa là "Khi click, hãy gọi action `copy` từ clipboard
controller tương ứng."

Kết hợp lại, ba attribute này tạo ra hành vi sau: Khi người dùng `click` vào button, hãy sử dụng hàm
`copy` của Stimulus controller `clipboard`, với input làm `source`. Stimulus controller JavaScript mà ta chưa thấy
sẽ xử lý phần còn lại và sao chép giá trị của source target vào clipboard của người dùng.

Chẳng phải đây là bài học JavaScript sao? Đúng, nhưng điểm cốt lõi là Stimulus là một framework ưu tiên HTML.
Nó thường không render HTML; thay vào đó, nó gắn hành vi vào HTML sẵn có của bạn bằng những mẩu JavaScript nhỏ.
Nhờ sử dụng nhất quán một số data attribute, chúng ta có thể đọc HTML và biết HTML được bổ sung hành vi ở đâu.

Hãy xem HTML này khi không có các data attribute đặc biệt mà Stimulus sử dụng:

```html
<input id="pin-code" value="1337" readonly>
<button id="pin-button">Copy to Clipboard</button>
```

Có lẽ button đó phải làm gì đó khi bạn click. Tuy nhiên, chỉ nhìn vào HTML,
bạn không thể biết mọi thứ được nối với nhau như thế nào. Bạn phải tìm một đoạn JavaScript xử lý hành vi này.

Vậy hãy cùng đi qua những khía cạnh cơ bản của Stimulus controller. Đừng lo nếu lần đọc đầu tiên bạn chưa hiểu
mọi thứ; các bài tập sẽ cung cấp thông tin chuyên sâu hơn.

### Stimulus controller

Một Stimulus controller được gắn vào phần tử DOM bằng cách khai báo nó với attribute `data-controller`.

```html
<input data-controller="input">
```

Thao tác này sẽ gắn Stimulus controller nằm trong `app/javascript/controllers/input_controller.js`.
Nếu muốn thêm một controller mới,
hãy tạo file `some_name_controller.js`, và nó sẽ được tự động nạp.

Một controller rỗng trông như sau:

```javascript
import { Controller } from "@hotwired/stimulus"
export default class extends Controller {}
```

Controller này không làm gì cả; nó chỉ cho thấy cách import class `Controller` mà chúng ta dùng làm cơ sở.
Như bạn thấy, không có chỗ nào nhắc đến tên controller. Tên được suy ra từ tên file
(`input_controller.js` trở thành `input`).

Điều này tạo ra một scope. Tất cả attribute sau đó chỉ hoạt động trong scope của controller / phần tử DOM đó.

### Kích hoạt một action

Bạn đã học cách kích hoạt event bằng event listener; còn trong Stimulus, bạn sử dụng attribute `data-action` để
thực thi JavaScript nhằm phản hồi một lần click hoặc input của người dùng.

Vì vậy, thay vì `document.querySelector("button").addEventListener("click", showAlert)`, chúng ta viết HTML sau:

```html
<div data-controller="alert">
  <button data-action="click->alert#show">Alert me!</button>
</div>
```

Giờ khi click button, action của Stimulus controller tương ứng sẽ được kích hoạt:

```javascript
// app/javascript/controllers/alert_controller.js
import { Controller } from "@hotwired/stimulus"
export default class extends Controller {
    show() {
        alert("Hey from Stimulus");    
    }
}
```

### Chọn phần tử

Bạn đã học cách truy cập phần tử DOM bằng selector, chẳng hạn `document.querySelector` và `document.getElementById`.
Một lần nữa, Stimulus cung cấp cách khai báo các phần tử bạn muốn chọn ngay trong HTML:

```html
<div data-controller="greeter">
  <input type="text" data-greeter-target="name">
  <button data-action="click->greeter#greet">Greet me</button>
  <div data-greeter-target="output"></div>
</div>
```

Hãy chú ý đến `data-greeter-target`. Sau đó, target có thể được sử dụng trong controller:

```javascript
// app/javascript/controllers/greeter_controller.js
import { Controller } from "@hotwired/stimulus"
export default class extends Controller {
    static targets = ["name", "output"]
    greet() {
        this.outputTarget.innerHTML = `Hello ${this.nameTarget.value}!`
    }
}
```

Bạn cần khai báo các target; sau khi khai báo, Stimulus cung cấp cho controller các property sau:

- `this.nameTarget` - phần tử DOM đầu tiên có tên target tương ứng.
- `this.nameTargets` - một mảng gồm tất cả target khớp với tên đó.
- `this.hasNameTarget` - giá trị Boolean cho biết target có tên đó có nằm trong scope hay không.

### Lưu state

Stimulus controller có thể có state nội bộ, nghĩa là bạn có thể lưu dữ liệu trong controller. Tuy vậy, thông thường bạn nên
giữ phần này ở mức tối thiểu.

Vì thế, mọi thứ bạn định nghĩa trên `this` đều có thể truy cập trong toàn bộ controller:

```javascript
export default class extends Controller {
    connect() {
        this.count = 0
    }
  
    addOne() {
        this.count++
    }
}
```

Stimulus cũng cho phép bạn khai báo các value attribute cụ thể để lắng nghe thay đổi:

```javascript
// app/javascript/controllers/counter_controller.js
export default class extends Controller {
    static values = { count: {type: Number, default: 0} }
  
    addOne() {
        this.countValue++
    }
    
    countValueChanged() {
        console.log(this.countValue)
    }
}
```

Ở đây, chúng ta khai báo một count value rồi tương tác với nó: hàm `countValueChanged` sẽ tự động được
gọi mỗi khi value thay đổi. Như đã nói, Stimulus ưu tiên HTML. Vì vậy, HTML thực sự hiển thị value trên
phần tử nơi controller cư trú:

```html
<div data-controller="counter" data-counter-count-value="0"></div>
```

Do đó, HTML ở trên sẽ luôn phản ánh giá trị của `this.countValue`. Nếu bạn thay đổi value thông qua một action trong
controller, giá trị HTML sẽ được cập nhật. Nhưng nếu bạn thay đổi value trong HTML, callback `countValueChange`
cũng được gọi.

Điều này có thể hơi trừu tượng lúc này, nhưng mở ra những khả năng rất thú vị cho nhiều trường hợp sử dụng.

### Dùng class attribute để controller dễ tùy biến hơn

Stimulus controller nên được viết theo hướng có thể tái sử dụng. Bạn thường sẽ muốn bật/tắt, xóa hoặc thêm
một CSS class cụ thể cho một phần tử. Nhưng class đó có thể khác nhau mỗi lần. Chẳng hạn, đôi khi bạn muốn
bật/tắt class `hidden`, nhưng đôi khi lại là class `active`. Trong những tình huống này, bạn có thể cấu hình controller
bằng một attribute để chỉ định CSS class cần dùng:

```html
<div data-controller="toggle" data-toggle-change-class="hidden"></div>
```

Và trong controller:

```javascript
// app/javascript/controllers/toggle_controller.js
export default class extends Controller {
  static classes = [ "change" ]

  toggle() {
    this.element.classList.toggle(this.changeClass)
  }
}
```

### Thiết lập bằng các hàm lifecycle

Bạn thường sẽ muốn dùng Stimulus để sử dụng các thư viện JavaScript bên thứ ba cùng với code của mình. Chẳng hạn, biến
một select input thông thường trong form thành một trường tương tác đẹp mắt có tính năng tự động hoàn thành. Điều này có nghĩa là bạn muốn thực thi
một đoạn JavaScript mỗi khi xuất hiện một trường như vậy.

```html
<select name="foods" id="foods" data-controller="select">
  <option value="Pizza">Pizza</option>
  ...
</select>
```

Trong tình huống này, không có hành động nào của người dùng để bạn phản hồi, vì có thể họ vừa chuyển đến từ một trang khác. Với
những tình huống như vậy, bạn có thể sử dụng các method lifecycle của Stimulus. Hiện tại, chúng ta chỉ đề cập đến method quan trọng nhất: `connect`

```javascript
import { Controller } from "@hotwired/stimulus"
import Choices from 'choices.js'

export default class extends Controller {
  connect() {
    new Choices(this.element)
  }
}
```

Connect là một hàm đặc biệt được gọi mỗi khi một phần tử có `data-controller="controller-name"`
xuất hiện trong DOM. Vì vậy, đây là giải pháp hoàn hảo để thay đổi HTML vừa xuất hiện trên trang.

### Gợi ý nhỏ về những thứ có nhiều từ

Vì Stimulus nằm giữa HTML và JavaScript, việc xác định cách đặt tên cho controller, action và target đôi khi có thể gây nhầm lẫn.
Một số thứ dùng `camelCase`, một số khác dùng `kebab-case`. Đoạn code nhỏ này giúp bạn xác định cần dùng gì:

```html
<div data-controller="reset-input">
  <input 
    data-reset-input-target="twoWords"
    data-action="keyup->reset-input#updateButton"
  >
</div>
```

### Tóm tắt

Stimulus cho bạn cách làm HTML có tính tương tác hơn. Bằng cách sử dụng data attribute, HTML cho biết rõ nơi
JavaScript sẽ tương tác với HTML. Đây mới chỉ là phần giới thiệu khái quát; các bài tập tiếp theo sẽ
đi sâu hơn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

- Đọc [Stimulus Handbook](https://stimulus.hotwired.dev/handbook/origin) để có cái nhìn tổng quan về cách viết code
với Stimulus.
- Xem [video hướng dẫn Stimulus 2.0](https://www.driftingruby.com/episodes/the-stimulus-2-0-tutorial); video có thể giúp
bạn hình dung phần nào cách làm việc với Stimulus controller. Bạn có thể bỏ qua phần cài đặt bằng Webpacker vì
chúng ta sẽ sử dụng tiêu chuẩn Rails mới là import map.
- Hãy nhớ đọc thêm [phần tham khảo Stimulus](https://stimulus.hotwired.dev/reference/controllers) nếu bạn
chưa đọc. Đừng lo nếu không nhớ hết mọi thứ, nhưng bạn nên biết nơi tra cứu điều mình cần.

</div>

#### Bài tập thực hành

Để thực hành, bạn cần tạo một ứng dụng Rails tiêu chuẩn mới. Stimulus sẽ được cài mặc định cùng Rails 7.0.

- Viết một chút HTML sử dụng controller mẫu trong `app/javascript/controllers/hello_controller.js`
- Tạo controller toggle của riêng bạn và sử dụng nó trong view. Controller này phải có khả năng ẩn/hiện phần tử khi click một
nút.
- Đảm bảo controller toggle có thể tái sử dụng. Hãy thử làm cho nó thực hiện các việc sau:
  - Click một button sẽ hiện một phần tử khác (chẳng hạn menu dropdown)
  - Click một phần tử sẽ ẩn phần tử vừa click và hiện một phần tử khác
  - Click một checkbox sẽ làm nổi bật phần tử chứa checkbox đó
- Viết một controller cho các text input có độ dài giới hạn. Cảnh báo người dùng khi họ sắp đạt hoặc đã vượt số ký tự tối đa
(giả sử người dùng đang viết một bài đăng có độ dài tối đa 280 ký tự)
- **Dự án**:
Quay lại dự án **Flight Booker** và cải thiện nó:
  - Thêm một controller cho phép người dùng thêm hành khách khác bằng cách click button "Add passenger"; thao tác này thêm một nhóm field khác để nhập thông tin hành khách (gợi ý: xem [thẻ \<template\>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)).
  - Cho phép xóa hành khách hiện có bằng cách click button "Remove"; thao tác này xóa một nhóm field thông tin hành khách (đảm bảo việc submit lên server vẫn hoạt động như mong đợi)
  - Ngăn không cho xóa nhóm thông tin hành khách cuối cùng.

### Tài nguyên bổ sung

Phần này có các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [Better Stimulus](https://www.betterstimulus.com/), một tài nguyên hay trình bày các phương pháp tốt nhất khi làm việc với Stimulus
- [Stimulus Components](https://stimulus-components.netlify.app/), một tập hợp các Stimulus component có thể tái sử dụng