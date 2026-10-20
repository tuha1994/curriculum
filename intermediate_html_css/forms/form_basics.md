### Giới thiệu

Form là một trong những phần quan trọng nhất của website. Chúng là cánh cổng để người dùng truy cập backend của bạn — người dùng cung cấp dữ liệu trong form, còn bạn xử lý dữ liệu đó.

Bạn cần chỉ định đúng loại input cho từng mẩu dữ liệu có thể có, vì thường có nhiều cách thu thập một mẩu dữ liệu nhưng chỉ một cách là dễ nhất đối với người dùng.

Trong bài học này, chúng ta sẽ tìm hiểu những điều cơ bản về form HTML và một số loại input khác nhau mà bạn có thể sử dụng.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài này.

- Tạo form bằng HTML.
- Có hiểu biết cơ bản về cách tạo kiểu cho form.

### Phần tử form

Phần tử form là một phần tử chứa, giống như phần tử div mà chúng ta đã học trước đó trong curriculum. Phần tử form bao bọc tất cả các input mà người dùng sẽ tương tác trong một biểu mẫu.

Phần tử form nhận hai thuộc tính thiết yếu; đầu tiên là thuộc tính `action`, nhận một giá trị URL cho biết biểu mẫu cần gửi dữ liệu đến đâu để xử lý.
Sau này trong curriculum, chúng ta sẽ học cách kết nối hệ thống backend với các biểu mẫu frontend bằng thuộc tính này. Hiện tại, điều quan trọng là hiểu thuộc tính `action` được dùng để làm gì.

Thứ hai là thuộc tính `method`, cho trình duyệt biết nó nên dùng [phương thức yêu cầu HTTP nào](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) để gửi biểu mẫu.
GET và POST là hai phương thức yêu cầu bạn sẽ sử dụng nhiều nhất.

Chúng ta dùng GET khi muốn lấy một thứ gì đó từ máy chủ. Ví dụ, Google dùng yêu cầu GET khi bạn tìm kiếm, vì nó *lấy* về các kết quả tìm kiếm.

POST được dùng khi muốn thay đổi điều gì đó trên máy chủ, chẳng hạn khi người dùng tạo tài khoản hoặc thanh toán trên một website.

Markup để tạo một phần tử form có dạng như sau:

```html
<form action="example.com/path" method="post">

</form>
```

### Bộ điều khiển biểu mẫu

Để bắt đầu thu thập dữ liệu người dùng, chúng ta cần dùng các phần tử điều khiển biểu mẫu. Đây là tất cả những phần tử mà người dùng sẽ tương tác trong biểu mẫu, chẳng hạn hộp văn bản, danh sách thả xuống, checkbox và nút. Trong vài phần tiếp theo, chúng ta sẽ tìm hiểu một số bộ điều khiển biểu mẫu thường dùng nhất.

### Phần tử input

Phần tử input là phần tử linh hoạt nhất trong số các phần tử điều khiển biểu mẫu. Nó nhận thuộc tính `type`, cho trình duyệt biết nên mong đợi *loại* dữ liệu nào và hiển thị phần tử input ra sao.

Một input văn bản có dạng như sau:

```html
<form action="example.com/path" method="post">
  <input type="text">
</form>
```

Input văn bản nhận mọi nội dung dạng chữ. Ví dụ, bạn có thể dùng nó để thu thập họ và tên của người dùng.

#### Nhãn

Một input đứng riêng không hữu ích lắm vì người dùng sẽ không biết họ cần cung cấp loại dữ liệu nào. Thay vào đó, chúng ta có thể thêm nhãn cho input để cho người dùng biết họ cần nhập loại dữ liệu gì.

Để tạo nhãn, chúng ta dùng phần tử `<label>`. Văn bản muốn hiển thị trong nhãn sẽ nằm giữa thẻ mở và thẻ đóng của nó:

```html
<form action="example.com/path" method="post">
  <label for="first_name">First Name:</label>
  <input type="text" id="first_name">
</form>
```

Nhãn nhận thuộc tính `for`, dùng để liên kết nó với một input cụ thể. Input muốn liên kết với nhãn cần có thuộc tính `id` mang cùng giá trị với thuộc tính `for` của nhãn.

Khi nhãn được liên kết với một input và được nhấp, con trỏ sẽ chuyển tiêu điểm vào input đó, sẵn sàng để người dùng nhập dữ liệu. Điều này giúp biểu mẫu dễ tiếp cận hơn với những người dùng phụ thuộc vào công nghệ hỗ trợ.

#### Thuộc tính placeholder

Để hướng dẫn người dùng cần nhập gì vào các phần tử biểu mẫu, chúng ta có thể thêm văn bản placeholder vào các trường input.

Ta thực hiện việc này bằng cách thêm thuộc tính `placeholder` vào input. Giá trị của thuộc tính sẽ là văn bản *placeholder* muốn hiển thị trong input:

```html
<label for="first_name">First Name:</label>
<input type="text" id="first_name" placeholder="Bob...">
```

Dùng văn bản placeholder để minh họa cách nhập và định dạng văn bản.

#### Thuộc tính name

Chúng ta cần dùng nhãn để người dùng hiểu dữ liệu nhập vào một trường input đại diện cho điều gì. Tương tự, chúng ta cũng cần cho backend — nơi nhận dữ liệu — biết mỗi mẩu dữ liệu đại diện cho điều gì.

Ta thực hiện việc này bằng cách thêm thuộc tính `name` vào các input:

```html
<label for="first_name">First Name:</label>
<input type="text" id="first_name" name="first_name">
```

Thuộc tính `name` dùng làm tham chiếu đến dữ liệu được nhập vào một bộ điều khiển biểu mẫu sau khi gửi biểu mẫu. Bạn có thể hình dung nó như tên biến của input. Input trong biểu mẫu luôn nên có thuộc tính `name`; nếu không, nó sẽ bị bỏ qua khi biểu mẫu được gửi.

Để hiểu rõ hơn kết quả này trông như thế nào, chúng ta có thể gửi một biểu mẫu đến [httpbin](http://httpbin.org/). Dịch vụ này sẽ trả về phản hồi cho phép xem dữ liệu đã được gửi. Hãy điền vào biểu mẫu bên dưới rồi nhấp submit:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="dyVRMbq" data-preview="true" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/dyVRMbq">
  form-basics-name-attribute</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Phần chúng ta quan tâm trong phản hồi là đối tượng "form". Nó sẽ có dạng tương tự như sau:

```json
"form": {
    "age": "33",
    "first_name": "John",
    "last_name": "Doe"
  },
```

Hãy thử thay đổi thuộc tính `name` của một vài trường input trong biểu mẫu và xóa hẳn thuộc tính đó, sau đó gửi lại biểu mẫu để xem dữ liệu biểu mẫu trong phản hồi thay đổi như thế nào.

#### Dùng bộ điều khiển biểu mẫu bên ngoài form

Cần lưu ý rằng bạn có thể dùng bất kỳ bộ điều khiển biểu mẫu nào HTML cung cấp bên ngoài phần tử `<form>`, ngay cả khi bạn không có máy chủ backend để gửi dữ liệu đến.

Ví dụ, bạn có thể muốn có một input nhận dữ liệu từ người dùng rồi hiển thị dữ liệu đó ở nơi khác trên trang bằng JavaScript:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="result" data-slug-hash="PoJjNYr" data-preview="true" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PoJjNYr">
  using-form-controls-outside-of-form</a> bởi TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Sau này trong curriculum, chúng ta sẽ cần thao tác với dữ liệu từ những bộ điều khiển biểu mẫu như thế này trong các project.

#### Thuộc tính type

Input `Email` là input văn bản chuyên dụng dành cho địa chỉ email. Chúng khác input văn bản ở chỗ trên thiết bị di động, chúng hiển thị bàn phím khác có cả ký hiệu @ để nhập địa chỉ email dễ hơn.

Chúng cũng xác thực xem người dùng đã nhập địa chỉ email đúng định dạng hay chưa, nhưng phần validation sẽ được trình bày thêm ở phía sau.

Để tạo input email, chúng ta dùng phần tử input với thuộc tính `type` có giá trị là "email":

```html
<label for="user_email">Email Address:</label>
<input type="email" id="user_email" name="email" placeholder="you@example.com">
```

Input `Password` là một loại input văn bản chuyên dụng khác. Chúng khác input văn bản thông thường ở chỗ che dữ liệu đã nhập bằng một ký tự khác — thường là dấu sao (*) hoặc dấu chấm (•) — để ngăn người khác nhìn thấy nội dung đã nhập.

Có thể tạo input mật khẩu bằng phần tử input với type là "password":

```html
<label for="user_password">Password:</label>
<input type="password" id="user_password" name="password">
```

Input `number` chỉ nhận giá trị số và bỏ qua mọi ký tự khác mà người dùng cố nhập.

Ta tạo input số bằng phần tử input với thuộc tính `type` là "number":

```html
<label for="amount">Amount:</label>
<input type="number" id="amount" name="amount">
```

Để thu thập ngày tháng từ người dùng, chúng ta có thể dùng input `date`. Input này đặc biệt vì cung cấp trải nghiệm chọn ngày tốt hơn bằng cách hiển thị lịch chọn ngày.

Để tạo input ngày tháng, chúng ta dùng phần tử input với thuộc tính `type` là "date":

```html
<label for="dob">Date of Birth:</label>
<input type="date" id="dob" name="dob">
```

#### Vùng văn bản

Dù về mặt kỹ thuật không phải phần tử input, phần tử text area cung cấp một hộp nhập có thể nhận văn bản nhiều dòng, chẳng hạn bình luận và đánh giá của người dùng. Hộp này cũng có thể được thay đổi kích thước bằng cách nhấp và kéo góc dưới bên phải.

Để tạo vùng văn bản, chúng ta dùng phần tử `<textarea>`:

```html
<textarea></textarea>
```

Khác với các phần tử input, phần tử Textarea có thẻ đóng. Nhờ đó, bạn có thể đặt nội dung ban đầu muốn vùng văn bản hiển thị vào giữa hai thẻ:

```html
<textarea>Some initial content</textarea>
```

Phần tử text area nhận một vài thuộc tính riêng mà các bộ điều khiển biểu mẫu khác không có. Đó là thuộc tính `rows` và `cols`. Chúng cho phép kiểm soát chiều cao ban đầu (rows) và chiều rộng (cols) của vùng văn bản:

```html
<textarea rows="20" cols="60"></textarea>
```

### Phần tử lựa chọn

Đôi khi bạn muốn người dùng chọn một giá trị từ danh sách định sẵn. Đây là lúc phần tử select phát huy tác dụng.

#### Danh sách select thả xuống

Phần tử select hiển thị danh sách thả xuống để người dùng chọn một tùy chọn. Về cú pháp, phần tử select có markup tương tự danh sách không có thứ tự. Phần tử select bao bọc các phần tử option, tức những tùy chọn có thể chọn.

Để tạo danh sách select thả xuống, chúng ta dùng phần tử `<select>`. Mọi tùy chọn muốn hiển thị bên trong phần tử select đều được định nghĩa bằng các phần tử `<option>`:

```html
<select name="Car">
  <option value="mercedes">Mercedes</option>
  <option value="tesla">Tesla</option>
  <option value="volvo">Volvo</option>
  <option value="bmw">BMW</option>
  <option value="mini">Mini</option>
  <option value="ford">Ford</option>
</select>
```

Mọi phần tử option nên có thuộc tính `value` (nếu không, nội dung văn bản bên trong sẽ được dùng). Giá trị này sẽ được gửi đến máy chủ khi biểu mẫu được gửi.

Ta có thể đặt một option làm phần tử được chọn mặc định khi trình duyệt hiển thị biểu mẫu lần đầu bằng cách thêm thuộc tính `selected` cho option đó:

```html
<select name="Car">
  <option value="mercedes">Mercedes</option>
  <option value="tesla">Tesla</option>
  <option value="volvo" selected>Volvo</option>
  <option value="bmw">BMW</option>
  <option value="mini">Mini</option>
  <option value="ford">Ford</option>
</select>
```

Ta cũng có thể chia danh sách option thành các nhóm bằng phần tử `<optgroup>`. Phần tử optgroup nhận thuộc tính `label`, được trình duyệt dùng làm nhãn cho mỗi nhóm:

```html
<select name="fashion">
  <optgroup label="Clothing">
    <option value="t_shirt">T-Shirts</option>
    <option value="sweater">Sweaters</option>
    <option value="coats">Coats</option>
  </optgroup>
  <optgroup label="Foot Wear">
    <option value="sneakers">Sneakers</option>
    <option value="boots">Boots</option>
    <option value="sandals">Sandals</option>
  </optgroup>
</select>
```

#### Nút radio

Danh sách select thả xuống rất hữu ích để tiết kiệm không gian khi có nhiều tùy chọn cho người dùng lựa chọn. Tuy nhiên, khi danh sách chỉ có 5 tùy chọn trở xuống, thường sẽ dễ dùng hơn nếu hiển thị chúng ngay trên trang thay vì ẩn sau danh sách thả xuống.

Trong trường hợp này, chúng ta có thể dùng nút radio. Nút radio cho phép tạo nhiều tùy chọn mà người dùng chỉ có thể chọn một. Để tạo nút radio, ta lại dùng phần tử input linh hoạt với thuộc tính `type` là "radio":

```html
<h1>Ticket Type</h1>
<div>
  <input type="radio" id="child" name="ticket_type" value="child">
  <label for="child">Child</label>
</div>

<div>
  <input type="radio" id="adult" name="ticket_type" value="adult">
  <label for="adult">Adult</label>
</div>

<div>
  <input type="radio" id="senior" name="ticket_type" value="senior">
  <label for="senior">Senior</label>
</div>
```

Khi chọn một nút radio rồi chọn nút khác, nút đầu tiên sẽ bị bỏ chọn. Nút radio biết phải làm vậy vì chúng có cùng thuộc tính `name`. Nhờ đó, trình duyệt biết các phần tử này thuộc cùng một nhóm tùy chọn.

Ta có thể đặt nút radio được chọn mặc định bằng cách thêm thuộc tính `checked` vào nó:

```html
<h1>Ticket Type</h1>
<div>
  <input type="radio" id="child" name="ticket_type" value="child">
  <label for="child">Child</label>
</div>

<div>
  <input type="radio" id="adult" name="ticket_type" value="adult" checked>
  <label for="adult">Adult</label>
</div>

<div>
  <input type="radio" id="senior" name="ticket_type" value="senior">
  <label for="senior">Senior</label>
</div>
```

#### Checkbox

Checkbox tương tự nút radio ở chỗ cho phép người dùng chọn từ một tập tùy chọn định sẵn. Nhưng khác nút radio, checkbox cho phép chọn nhiều tùy chọn cùng lúc.

Để tạo checkbox, chúng ta dùng phần tử input với thuộc tính `type` là "checkbox":

```html
<h1>Pizza Toppings</h1>

<div>
  <input type="checkbox" id="sausage" name="topping" value="sausage">
  <label for="sausage">Sausage</label>
</div>

<div>
  <input type="checkbox" id="onions" name="topping" value="onions">
  <label for="onions">Onions</label>
</div>

<div>
  <input type="checkbox" id="pepperoni" name="topping" value="pepperoni">
  <label for="pepperoni">Pepperoni</label>
</div>

<div>
  <input type="checkbox" id="mushrooms" name="topping" value="mushrooms">
  <label for="mushrooms">Mushrooms</label>
</div>
```

Ta cũng có thể dùng một checkbox đơn khi muốn người dùng bật tắt một giá trị đúng hoặc sai. Ví dụ, đăng ký nhận bản tin khi họ tạo tài khoản:

```html
<div>
  <input type="checkbox" id="newsletter" name="news_letter">
  <label for="newsletter">Send me the news letter</label>
</div>
```

Ta có thể đặt checkbox được chọn mặc định khi tải trang bằng cách thêm thuộc tính `checked` cho chúng:

```html
<div>
  <input type="checkbox" id="newsletter" name="news_letter" checked>
  <label for="newsletter">Send me the news letter</label>
</div>
```

### Nút

Phần tử button tạo các nút có thể nhấp để người dùng tương tác, gửi biểu mẫu và kích hoạt những hành động khác.

Để tạo nút, chúng ta dùng phần tử `<button>`. Nội dung hoặc văn bản muốn hiển thị bên trong nút sẽ nằm giữa thẻ mở và thẻ đóng:

```html
<button>Click Me</button>
```

Phần tử button cũng nhận thuộc tính `type`, cho trình duyệt biết đó là loại nút nào. Có ba loại nút để chúng ta sử dụng.

#### Nút submit

Khi điền xong biểu mẫu, người dùng cần một cách để gửi biểu mẫu. Nút submit là nút chuyên dụng cho việc này. Khi được nhấp, nút submit sẽ gửi biểu mẫu chứa nó. Theo mặc định, thuộc tính `type` có giá trị là submit, tức là khi không chỉ định `type` hoặc giá trị cung cấp không hợp lệ.

Để tạo nút submit, chúng ta dùng phần tử button với thuộc tính `type` là "submit":

```html
<button type="submit">Submit</button>
```

#### Nút reset

Nút reset xóa toàn bộ dữ liệu người dùng đã nhập vào biểu mẫu và đưa mọi input trong biểu mẫu về trạng thái ban đầu.

Để tạo nút reset, chúng ta dùng phần tử button với thuộc tính `type` là "reset":

```html
<button type="reset">Reset</button>
```

#### Nút thông thường

Loại nút thứ ba và cuối cùng là nút thông thường, có thể dùng cho bất kỳ mục đích nào. Nó thường được dùng cùng JS để tạo UI tương tác.

Để tạo nút thông thường, chúng ta dùng phần tử button với thuộc tính `type` là "button":

```html
<button type="button">Click to Toggle</button>
```

<div class="lesson-note lesson-note--tip" markdown=1>

#### Giá trị mặc định của type nút trong form

Điều quan trọng cần nhớ là một nút trong form có giá trị `type` là submit (vốn là giá trị mặc định) sẽ luôn cố tạo yêu cầu mới và gửi dữ liệu về máy chủ. Vì vậy, với các nút trong form dùng cho mục đích khác ngoài gửi dữ liệu, luôn phải chỉ định thuộc tính `type` để tránh việc gửi form ngoài ý muốn.

</div>

### Sắp xếp các phần tử biểu mẫu

Dùng đúng input cho loại dữ liệu muốn người dùng nhập góp phần lớn giúp biểu mẫu thân thiện hơn. Tuy nhiên, trong các biểu mẫu lớn, người dùng dễ bị quá tải và nản lòng nếu phải điền quá nhiều input.

May mắn là HTML cung cấp một vài phần tử giúp dễ dàng chia biểu mẫu thành các phần khác biệt về mặt hiển thị và dễ tiếp nhận hơn.

#### Phần tử fieldset

Phần tử fieldset là phần tử chứa cho phép nhóm các input biểu mẫu liên quan thành một đơn vị logic.

Để tạo fieldset, chúng ta dùng phần tử `<fieldset>`. Các input biểu mẫu muốn nhóm cùng nhau sẽ nằm giữa thẻ mở và thẻ đóng fieldset:

```html
<fieldset>
  <label for="first_name">First Name</label>
  <input type="text" id="first_name" name="first_name">

  <label for="last_name">Last Name</label>
  <input type="text" id="last_name" name="last_name">
</fieldset>
```

#### Legend

Phần tử legend dùng để đặt tiêu đề hoặc chú thích cho fieldset, giúp người dùng biết một nhóm input dùng cho mục đích gì.

Để tạo legend, chúng ta dùng phần tử `<legend>` với văn bản muốn hiển thị nằm giữa thẻ mở và thẻ đóng. Legend luôn nên nằm ngay sau thẻ mở fieldset:

```html
<fieldset>
  <legend>Contact Details</legend>

  <label for="name">Name:</label>
  <input type="text" id="name" name="name">

  <label for="phone_number">Phone Number:</label>
  <input type="tel" id="phone_number" name="phone_number">

  <label for="email">Email:</label>
  <input type="email" id="email" name="email">
</fieldset>

<fieldset>
  <legend>Delivery Details</legend>

  <label for="street_address">Street Address:</label>
  <input type="text" id="street_address" name="street_address">

  <label for="city">City:</label>
  <input type="text" id="city" name="city">

  <label for="zip_code">Zip Code:</label>
  <input type="text" id="zip_code" name="zip_code">
</fieldset>
```

Một trường hợp sử dụng phổ biến của các phần tử này là dùng fieldset để nhóm các nút radio và dùng legend để cho người dùng biết nhóm tùy chọn đó dùng cho việc gì:

```html
<fieldset>
  <legend>What would you like to drink?</legend>

  <div>
    <input type="radio" name="drink" id="coffee" value="coffee">
    <label for="coffee">Coffee</label>
  </div>

  <div>
    <input type="radio" name="drink" id="tea" value="tea">
    <label for="tea">Tea</label>
  </div>

  <div>
    <input type="radio" name="drink" id="soda" value="soda">
    <label for="soda">Soda</label>
  </div>
</fieldset>
```

### Lưu ý về việc tạo kiểu cho biểu mẫu

Trong phần bài tập tiếp theo, chúng tôi sẽ cung cấp các tài liệu đi sâu vào việc tạo kiểu cho biểu mẫu. Tuy nhiên, trước khi đến bài tập, chúng ta nên nói về một số thách thức khi tạo kiểu cho form HTML và cách khắc phục:

#### Kiểu mặc định của trình duyệt

Mỗi trình duyệt có kiểu mặc định riêng cho các bộ điều khiển biểu mẫu, khiến biểu mẫu hiển thị khác nhau tùy trình duyệt người dùng đang dùng.

Để có thiết kế nhất quán giữa các trình duyệt, chúng ta phải ghi đè các kiểu mặc định này và tự tạo kiểu cho chúng.

#### Bộ điều khiển biểu mẫu khó, thậm chí không thể, tạo kiểu

Các bộ điều khiển biểu mẫu dạng văn bản như text, email, password và text area tương đối dễ tạo kiểu. Chúng hoạt động như các phần tử HTML khác và có thể dùng hầu hết thuộc tính CSS trên chúng.

Việc tạo kiểu tùy chỉnh cho nút radio và checkbox khó hơn. Tuy vậy, có nhiều hướng dẫn giúp bạn đạt thiết kế mong muốn, chẳng hạn [hướng dẫn tạo kiểu checkbox tùy chỉnh](https://moderncss.dev/pure-css-custom-checkbox-style). Gần đây cũng có [các thuộc tính CSS mới](https://developer.mozilla.org/en-US/docs/Web/CSS/accent-color) giúp việc tạo kiểu cho nút radio và checkbox dễ hơn nhiều.

Một số khía cạnh của các phần tử khác gần như không thể tạo kiểu, chẳng hạn lịch hoặc bộ chọn ngày. Nếu muốn tạo kiểu tùy chỉnh cho chúng, ta phải xây dựng bộ điều khiển biểu mẫu tùy chỉnh bằng JavaScript hoặc dùng một trong nhiều thư viện JavaScript cung cấp giải pháp có sẵn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Kiến thức cơ bản về form

1. Đọc và làm theo ["Introductory Tutorials" về form của MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms#introductory_tutorials).
1. Đọc và làm theo các hướng dẫn ["The Different Form Controls" của MDN](https://developer.mozilla.org/en-US/docs/Learn/Forms#the_different_form_controls).

#### Tạo kiểu cho form

1. Đọc và làm theo ba hướng dẫn tạo kiểu form của MDN:
   - [Styling web forms](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
   - [Advanced form styling](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)
   - [UI pseudo-classes](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes)
1. Đọc và làm theo [hướng dẫn về form của Interneting Is Hard](https://internetingishard.netlify.app/html-and-css/forms/index.html).

</div>
