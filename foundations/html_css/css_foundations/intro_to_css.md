### Giới thiệu

Ở bài học trước, bạn đã học cách viết HTML để quyết định cấu trúc của một trang web. Bước tiếp theo là làm cho cấu trúc đó trông đẹp hơn bằng một chút *kiểu dáng* — đó chính là mục đích của CSS. Trong vài bài tiếp theo, chúng ta sẽ tập trung vào những khái niệm CSS nền tảng mà mọi người nên biết ngay từ đầu — dù mới bắt đầu hay đang cần ôn lại.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Thêm kiểu cho HTML bằng CSS.
- Cách sử dụng các thuộc tính `class` và `id`.
- Cách các bộ chọn CSS khác nhau hoạt động.

### Thêm CSS vào HTML

Trước khi học cách viết quy tắc CSS, trước hết hãy liên kết một tệp CSS với `index.html` để bạn có thể thử nghiệm mọi ví dụ trong bài.

Có ba cách thêm CSS vào HTML:

- CSS bên ngoài
- CSS nội bộ
- CSS inline

Hiện tại chúng ta sẽ tập trung vào CSS bên ngoài vì đây là phương pháp bạn sẽ dùng thường xuyên nhất. Hai phương pháp còn lại sẽ được trình bày ở phần sau của bài.

#### CSS bên ngoài

CSS bên ngoài là phương pháp phổ biến nhất; nó gồm việc tạo một tệp CSS riêng rồi liên kết tệp đó trong HTML.

```html
<!-- index.html -->

<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Trước tiên, chúng ta thêm phần tử rỗng `<link>` vào giữa thẻ mở và thẻ đóng `<head>` của tệp HTML. Thuộc tính `href` là vị trí của tệp CSS; trong trường hợp này, đó là đường dẫn tương đối với vị trí của tệp HTML (nhưng cũng có thể là URL đến một stylesheet được lưu trữ ở nơi khác). Trong ví dụ trên, chúng ta giả định hai tệp nằm cùng một thư mục. Thuộc tính `rel` là bắt buộc và chỉ mối quan hệ giữa tệp HTML với tệp được liên kết.

Sau đó, tạo một tệp `styles.css` rỗng trong cùng thư mục với tệp `index.html`. Chúng ta sẽ bắt đầu thêm các quy tắc CSS vào tệp này trong phần tiếp theo.

<div class="lesson-note" markdown="1">

#### Tên tệp CSS

`styles.css` chỉ là tên tệp chúng ta chọn trong ví dụ này. Bạn có thể đặt tên tùy ý, miễn loại tệp là `.css`; "style" hoặc "styles" chỉ là những tên được dùng phổ biến nhất.

</div>

Một vài ưu điểm của phương pháp này là:

1. Nó tách HTML và CSS, giúp tệp HTML nhỏ hơn và mọi thứ trông gọn gàng hơn.
1. Chúng ta chỉ cần chỉnh sửa CSS ở *một* nơi, đặc biệt tiện lợi với các website có nhiều trang dùng chung kiểu tương tự.

Để luyện viết quy tắc CSS, trước hết hãy tạo một tệp `index.html` (có phần khung cơ bản thường dùng) cùng một tệp `styles.css` trong cùng thư mục, rồi liên kết chúng như mô tả ở trên.

### Cú pháp cơ bản

Ở mức cơ bản nhất, CSS được tạo thành từ nhiều quy tắc. Mỗi quy tắc gồm một bộ chọn (sẽ nói thêm ngay sau đây) và một danh sách khai báo được phân tách bằng dấu chấm phẩy; mỗi khai báo lại gồm một cặp thuộc tính–giá trị.

![Cú pháp CSS cơ bản](https://cdn.statically.io/gh/TheOdinProject/curriculum/05ce472eabf8e04eeb2cc9139e66db884074fd7d/foundations/html_css/css-foundations/imgs/00.jpg)

<div class="lesson-note" markdown="1">

#### HTML ngữ nghĩa

`<div>` là một trong những phần tử HTML cơ bản. Nó là một vùng chứa rỗng. Nhìn chung, trong dự án bạn nên dùng các thẻ khác như `<h1>` hoặc `<p>` cho nội dung, nhưng khi tìm hiểu thêm về CSS, bạn sẽ thấy nhiều trường hợp thứ bạn cần chỉ là một vùng chứa cho các phần tử khác. Nhiều bài tập của chúng ta dùng các `<div>` đơn giản để dễ hiểu. Những bài sau sẽ trình bày sâu hơn về thời điểm thích hợp để dùng từng phần tử HTML.

</div>

### Bộ chọn

Bộ chọn trỏ đến các phần tử HTML mà quy tắc CSS áp dụng; chúng là thứ thực sự được "chọn" trong mỗi quy tắc. Các phần dưới đây không bao quát mọi bộ chọn hiện có, nhưng đây là những bộ chọn phổ biến nhất và bạn nên làm quen với chúng trước.

Khi xem từng ví dụ, hãy thử thêm cả HTML và CSS vào các tệp tương ứng để quan sát kết quả.

#### Bộ chọn toàn cục

Bộ chọn toàn cục sẽ chọn phần tử thuộc mọi loại (tức toàn bộ tài liệu), nên được gọi là "toàn cục"; cú pháp của nó chỉ là một dấu hoa thị. Trong ví dụ dưới đây, mọi phần tử sẽ được áp dụng kiểu `color: purple;`.

```css
* {
  color: purple;
}
```

#### Bộ chọn kiểu

Bộ chọn kiểu (hay bộ chọn phần tử) sẽ chọn tất cả phần tử thuộc kiểu được chỉ định; cú pháp chỉ là tên phần tử:

```html
<!-- index.html -->

<div>Hello, World!</div>
<div>Hello again!</div>
<p>Hi...</p>
<div>Okay, bye.</div>
```

```css
/* styles.css */

div {
  color: white;
}
```

Ở đây, cả ba phần tử `<div>` sẽ được chọn, còn phần tử `<p>` thì không.

#### Bộ chọn class

Bộ chọn class sẽ chọn tất cả phần tử có class được chỉ định — một thuộc tính bạn đặt trên phần tử HTML. Cách thêm class vào thẻ HTML và chọn nó trong CSS như sau:

```html
<!-- index.html -->

<div class="alert-text">Please agree to our terms of service.</div>
```

```css
/* styles.css */

.alert-text {
  color: red;
}
```

Hãy chú ý cú pháp của bộ chọn class: một dấu chấm đứng ngay trước giá trị phân biệt hoa thường của thuộc tính class. Class không cần gắn với một phần tử cụ thể, nên bạn có thể dùng cùng một class cho bao nhiêu phần tử tùy ý.

<div class="lesson-note" markdown="1">

#### Chữ số ở đầu tên class

Bộ chọn class sẽ không hoạt động nếu tên class bắt đầu bằng một chữ số. Ví dụ, nếu đặt tên class của phần tử là `4lert-text`, dùng `.4lert-text` làm bộ chọn sẽ không khớp.

</div>

Bạn cũng có thể thêm nhiều class vào một phần tử dưới dạng danh sách cách nhau bằng khoảng trắng, chẳng hạn `class="alert-text severe-alert"`. Vì khoảng trắng dùng để phân tách tên class, bạn không nên dùng khoảng trắng trong tên gồm nhiều từ mà nên dùng dấu gạch nối.

#### Bộ chọn ID

Bộ chọn ID tương tự bộ chọn class. Nó chọn một phần tử có ID được chỉ định, cũng là một thuộc tính bạn đặt trên phần tử HTML. Điểm khác biệt chính giữa class và ID là một phần tử chỉ có thể có **một** ID. ID không được lặp lại trên cùng một trang và không nên chứa khoảng trắng:

```html
<!-- index.html -->

<div id="title">My Awesome 90's Page</div>
```

```css
/* styles.css */

#title {
  background-color: red;
}
```

Với ID, thay vì dấu chấm, chúng ta dùng dấu thăng ngay trước giá trị phân biệt hoa thường của thuộc tính ID. Một lỗi thường gặp là lạm dụng ID dù không thực sự cần và class đã đủ dùng. Dù có những trường hợp dùng ID là hợp lý hoặc bắt buộc, chẳng hạn tận dụng độ đặc hiệu hoặc chuyển liên kết đến một phần trên trang hiện tại, bạn nên dùng ID **tiết chế** (nếu dùng).

<div class="lesson-note" markdown="1">

#### Chữ số ở đầu ID

Giống bộ chọn class, bộ chọn ID không thể bắt đầu bằng một chữ số. Ví dụ, nếu đặt ID của phần tử là `7itle`, bộ chọn `#7itle` sẽ không hoạt động — đó không phải bộ chọn CSS hợp lệ.

</div>

#### Bộ chọn nhóm

Nếu có hai nhóm phần tử dùng chung một số khai báo kiểu thì sao?

```css
.read {
  color: white;
  background-color: black;
  /* several unique declarations */
}

.unread {
  color: white;
  background-color: black;
  /* several unique declarations */
}
```

Hai bộ chọn `.read` và `.unread` cùng có các khai báo `color: white;` và `background-color: black;`, nhưng mỗi bộ chọn cũng có một số khai báo riêng. Để giảm lặp lại, ta có thể nhóm hai bộ chọn thành danh sách phân tách bằng dấu phẩy:

```css
.read,
.unread {
  color: white;
  background-color: black;
}

.read {
  /* several unique declarations */
}

.unread {
  /* several unique declarations */
}
```

Hai ví dụ trên (có và không nhóm) cho cùng kết quả, nhưng ví dụ thứ hai giảm lặp khai báo và giúp chỉnh sửa `color` hoặc `background-color` cho cả hai class cùng lúc dễ hơn.

#### Nối bộ chọn

Một cách khác để dùng bộ chọn là nối chúng thành một danh sách không có dấu phân cách. Giả sử ta có HTML sau:

```html
<div>
  <div class="subsection header">Latest Posts</div>
  <p class="subsection preview">This is where a preview for a post might go.</p>
</div>
```

Có hai phần tử mang class `subsection` và có một số kiểu riêng, nhưng nếu chỉ muốn áp dụng một quy tắc khác cho phần tử đồng thời có class thứ hai là `header` thì sao? Ta có thể nối hai bộ chọn class trong CSS như sau:

```css
.subsection.header {
  color: red;
}
```

`.subsection.header` chọn mọi phần tử có cả hai class `subsection` *và* `header`. Hãy chú ý không có khoảng trắng giữa các bộ chọn class `.subsection` và `.header`. Cú pháp này về cơ bản dùng được để nối mọi tổ hợp bộ chọn, ngoại trừ việc nối nhiều hơn một [bộ chọn kiểu](#type-selectors).

Cách này cũng dùng được để nối class với ID, như sau:

```html
<div>
  <div class="subsection header">Latest Posts</div>
  <p class="subsection" id="preview">
    This is where a preview for a post might go.
  </p>
</div>
```

Bạn có thể kết hợp hai phần tử trên bằng đoạn sau:

```css
.subsection.header {
  color: red;
}

.subsection#preview {
  color: blue;
}
```

Nhìn chung, bạn không thể nối nhiều hơn một bộ chọn kiểu vì một phần tử không thể đồng thời là hai kiểu khác nhau. Ví dụ, nối hai bộ chọn kiểu `div` và `p` sẽ cho bộ chọn `divp`, không hoạt động vì nó tìm một phần tử `<divp>` đúng nghĩa, mà phần tử đó không tồn tại.

#### Bộ kết hợp hậu duệ

Bộ kết hợp cho phép kết hợp nhiều bộ chọn theo cách khác với nhóm hoặc nối, vì chúng thể hiện quan hệ giữa các bộ chọn. Có tổng cộng bốn loại bộ kết hợp, nhưng hiện tại chúng ta chỉ giới thiệu **bộ kết hợp hậu duệ**, được biểu diễn trong CSS bằng một khoảng trắng giữa các bộ chọn. Bộ kết hợp hậu duệ chỉ chọn những phần tử khớp với bộ chọn cuối nếu chúng cũng có tổ tiên (cha, ông, v.v.) khớp với bộ chọn trước đó.

Chẳng hạn, `.ancestor .child` sẽ chọn phần tử có class `child` nếu nó có tổ tiên mang class `ancestor`. Có thể hiểu cách khác là `child` chỉ được chọn khi nằm bên trong `ancestor`, bất kể lồng sâu đến đâu. Hãy xem nhanh ví dụ dưới đây và thử đoán những phần tử nào sẽ được chọn theo quy tắc CSS:

```html
<!-- index.html -->

<div class="ancestor">
  <div class="contents">
    <div class="contents"></div>
  </div>
</div>

<div class="contents"></div>
```

```css
/* styles.css */

.ancestor .contents {
  /* some declarations */
}
```

Trong ví dụ trên, hai phần tử đầu tiên có class `contents` (ở dòng 4 và 5) sẽ được chọn, còn phần tử cuối (ở dòng 9) thì không. Bạn đoán đúng chứ?

Thực ra không có giới hạn về số bộ kết hợp có thể thêm vào một quy tắc, nên `.one .two .three .four` hoàn toàn hợp lệ. Nó sẽ chọn phần tử có class `four` nếu phần tử đó có tổ tiên mang class `three`, tổ tiên đó lại có tổ tiên mang class `two`, và cứ thế tiếp tục. Tuy nhiên, bạn thường nên tránh chọn các phần tử cần lồng sâu đến mức này vì quy tắc sẽ khó hiểu, dài dòng và có thể gây vấn đề về độ đặc hiệu.

### Các thuộc tính để bắt đầu

Có một số thuộc tính CSS bạn sẽ dùng liên tục, hoặc ít nhất là thường xuyên hơn phần lớn thuộc tính khác. Chúng ta sẽ giới thiệu một số thuộc tính trong đó, dù đây hoàn toàn chưa phải danh sách đầy đủ. Học những thuộc tính sau là đủ để giúp bạn bắt đầu. Lưu ý rằng mọi thuộc tính đều có giá trị khởi tạo mặc định; giá trị này được dùng nếu cả bạn lẫn trình duyệt không đặt giá trị khác (tài liệu MDN của từng thuộc tính sẽ nêu rõ điều này).

#### Color và background-color

Thuộc tính `color` đặt màu chữ của phần tử, còn `background-color` đặt — đúng như tên gọi — màu nền của phần tử. Vậy là xong rồi nhỉ?

Gần như vậy. Cả hai thuộc tính đều chấp nhận một trong nhiều loại giá trị. Một loại phổ biến là từ khóa, chẳng hạn tên màu như `red` hoặc từ khóa `transparent`. Chúng cũng chấp nhận giá trị HEX, RGB và HSL; có thể bạn đã quen với chúng nếu từng dùng phần mềm chỉnh sửa ảnh hoặc website cho phép tùy chỉnh màu hồ sơ.

```css
p {
  /* hex example: */
  color: #1100ff;
}

p {
  /* rgb example: */
  color: rgb(100, 0, 127);
}

p {
  /* hsl example: */
  color: hsl(15, 82%, 56%);
}
```

Hãy xem nhanh [CSS Legal Color Values](https://www.w3schools.com/cssref/css_colors_legal.asp) để biết cách điều chỉnh độ mờ của các màu này bằng cách thêm giá trị alpha.

#### Cơ bản về typography và text-align

`font-family` có thể là một giá trị đơn hoặc danh sách giá trị phân tách bằng dấu phẩy, quyết định phông chữ phần tử sử dụng. Mỗi phông chữ thuộc một trong hai nhóm: "font family name" như `"Times New Roman"` (cần dấu ngoặc kép vì có khoảng trắng giữa các từ), hoặc "generic family name" như `serif` (tên họ phông chữ chung không dùng dấu ngoặc kép).

Nếu trình duyệt không tìm thấy hoặc không hỗ trợ phông chữ đầu tiên trong danh sách, nó sẽ dùng phông tiếp theo, rồi tiếp tục như vậy cho đến khi tìm được phông hợp lệ và được hỗ trợ. Vì vậy, nên cung cấp một danh sách giá trị cho thuộc tính này, bắt đầu bằng phông muốn ưu tiên nhất và kết thúc bằng một họ phông chữ chung làm phương án dự phòng, ví dụ `font-family: "Times New Roman", serif;`

`font-size`, đúng như tên gọi, đặt kích thước phông chữ. Giá trị của thuộc tính này không được chứa khoảng trắng, ví dụ `font-size: 22px` không có khoảng trắng giữa "22" và "px".

`font-weight` ảnh hưởng đến độ đậm của chữ, nếu phông chữ hỗ trợ độ đậm được chỉ định. Giá trị có thể là từ khóa, chẳng hạn `font-weight: bold`, hoặc một số từ 1 đến 1000, chẳng hạn `font-weight: 700` (tương đương `bold`). Thông thường, các giá trị số tăng theo bước 100 đến 900, nhưng điều này còn tùy phông chữ.

`text-align` căn chỉnh văn bản theo chiều ngang trong một phần tử; bạn có thể dùng những từ khóa quen thuộc trong các trình xử lý văn bản, chẳng hạn `text-align: center`.

#### Chiều cao và chiều rộng của ảnh

Ảnh không phải phần tử duy nhất có thể điều chỉnh chiều cao và chiều rộng, nhưng ở đây chúng ta sẽ tập trung vào ảnh.

Theo mặc định, phần tử `<img>` có các giá trị `height` và `width` bằng chiều cao và chiều rộng thực tế của tệp ảnh. Nếu muốn đổi kích thước ảnh mà không làm ảnh mất tỷ lệ, hãy đặt `height` hoặc `width` thành một giá trị cụ thể và đặt thuộc tính còn lại thành `auto`. Ví dụ:

```css
img {
  height: auto;
  width: 500px;
}
```

Ví dụ, nếu ảnh ban đầu cao 500px và rộng 1000px, CSS trên sẽ cho chiều cao là 250px.

Các thuộc tính này hoạt động cùng với thuộc tính height và width trong HTML. Tốt nhất nên khai báo cả hai thuộc tính CSS và thuộc tính HTML cho phần tử ảnh, ngay cả khi bạn không định thay đổi giá trị ban đầu của tệp ảnh. Nếu không khai báo, khi ảnh tải lâu hơn phần còn lại của nội dung trang, ban đầu nó sẽ không chiếm chỗ nào, nhưng khi tải xong sẽ đột ngột làm các nội dung khác dịch chuyển mạnh. Khai báo rõ `height` và `width` ngăn điều này xảy ra, vì không gian sẽ được "dành sẵn" trên trang và để trống cho đến khi ảnh tải xong.

### Các phương pháp khác để thêm CSS vào HTML

Giờ đã học cú pháp cơ bản, chúng ta sẽ tìm hiểu thêm hai cách thêm CSS vào HTML (bên cạnh phương pháp bên ngoài đã học ở đầu bài).

#### CSS nội bộ

CSS nội bộ (hay CSS nhúng) là việc thêm CSS ngay trong tệp HTML thay vì tạo một tệp riêng hoàn toàn. Với phương pháp này, bạn đặt mọi quy tắc bên trong cặp thẻ mở và đóng `<style>`, rồi đặt cặp thẻ đó giữa thẻ mở và đóng `<head>` của tệp HTML. Vì kiểu được đặt trực tiếp trong `<head>`, ta không còn cần phần tử `<link>` mà phương pháp bên ngoài yêu cầu.

Ngoài những khác biệt đó, cú pháp hoàn toàn giống phương pháp bên ngoài (bộ chọn, dấu ngoặc nhọn, khai báo):

```html
<head>
  <style>
    div {
      color: white;
      background-color: black;
    }

    p {
      color: red;
    }
  </style>
</head>
<body>
  ...
</body>
```

Phương pháp này hữu ích khi thêm kiểu riêng cho *một trang duy nhất* của website, nhưng không tách biệt mọi thứ như phương pháp bên ngoài; tùy số lượng quy tắc và khai báo, nó có thể khiến tệp HTML khá lớn.

#### CSS inline

CSS inline cho phép thêm kiểu trực tiếp vào các phần tử HTML, dù đây không phải phương pháp được khuyến nghị:

```html
<body>
  <div style="color: white; background-color: black;">...</div>
</body>
```

Điểm đầu tiên cần lưu ý là ở đây chúng ta không thực sự dùng bộ chọn nào, vì kiểu được thêm trực tiếp vào thẻ mở `<div>`. Tiếp theo là thuộc tính `style`; giá trị nằm giữa cặp dấu ngoặc kép chính là các khai báo.

Nếu cần thêm một kiểu *riêng* cho *một* phần tử, phương pháp này có thể hoàn toàn phù hợp. Tuy nhiên, nhìn chung đây không phải cách được khuyến nghị để thêm CSS vào HTML vì một số lý do:

- Nó có thể nhanh chóng trở nên lộn xộn khi bạn bắt đầu thêm *nhiều* khai báo vào một phần tử, khiến tệp HTML phình to không cần thiết.
- Nếu muốn nhiều phần tử có cùng kiểu, bạn phải sao chép và dán cùng một kiểu vào từng phần tử, gây lặp lại và phình to không cần thiết.
- CSS inline sẽ ghi đè hai phương pháp còn lại, có thể dẫn đến kết quả không mong muốn. (Dù chưa đi sâu ở đây, bạn thực ra có thể tận dụng điều này.)

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hãy vào [kho bài tập CSS](https://github.com/TheOdinProject/css-exercises) của chúng ta và đọc tệp README.
1. Sau khi biết cách sử dụng các bài tập, hãy chuyển đến [thư mục `foundations/intro-to-css` của kho bài tập CSS](https://github.com/TheOdinProject/css-exercises/tree/main/foundations/intro-to-css). Đọc từng tệp README trước khi hoàn thành các bài tập sau theo thứ tự:

    - `01-css-methods`
    - `02-class-id-selectors`
    - `03-grouping-selectors`
    - `04-chaining-selectors`
    - `05-descendant-combinator`

    Lưu ý: Bạn có thể tìm lời giải cho các bài tập này trong thư mục `solution` của từng bài.

</div>