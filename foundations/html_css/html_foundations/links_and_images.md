### Giới thiệu

Liên kết là một trong những tính năng then chốt của HTML. Chúng cho phép chúng ta liên kết đến các trang HTML khác trên web. Thực ra, đó chính là lý do nó được gọi là web.

Trong bài học này, chúng ta sẽ học cách tạo liên kết và thêm điểm nhấn trực quan cho website bằng cách nhúng hình ảnh.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Cách tạo liên kết đến các trang trên những website khác trên internet.
- Cách tạo liên kết đến các trang khác trên website của chính bạn.
- Sự khác nhau giữa liên kết tuyệt đối và liên kết tương đối.
- Cách hiển thị hình ảnh trên trang web bằng HTML.

### Chuẩn bị

Để luyện tập sử dụng liên kết và hình ảnh trong suốt bài học này, chúng ta cần một dự án HTML để làm việc.

1. Tạo một thư mục mới có tên `odin-links-and-images`.
1. Trong thư mục đó, tạo một tệp mới có tên `index.html`.
1. Mở tệp trong VS Code và điền boilerplate HTML thông thường.
1. Cuối cùng, thêm h1 sau vào body:

```html
<h1>Homepage</h1>
```

### Phần tử anchor

Để tạo liên kết trong HTML, chúng ta dùng phần tử anchor. Phần tử anchor được định nghĩa bằng cách bao quanh văn bản hoặc một phần tử HTML khác mà chúng ta muốn biến thành liên kết bằng thẻ `<a>`.

Thêm đoạn sau vào body của trang `index.html` đã tạo rồi mở trang trong trình duyệt:

```html
<a>About The Odin Project</a>
```

Có thể bạn nhận thấy việc nhấp vào liên kết này không làm gì cả. Đó là vì chỉ riêng thẻ anchor không biết chúng ta muốn liên kết đến đâu. Chúng ta phải chỉ cho nó một đích đến. Việc này được thực hiện bằng một thuộc tính HTML.

Thuộc tính HTML cung cấp thêm thông tin cho một phần tử HTML và luôn nằm trong thẻ mở của phần tử đó. Một thuộc tính thường gồm hai phần: tên và giá trị; tuy nhiên, không phải thuộc tính nào cũng cần giá trị. Trong trường hợp này, chúng ta cần thêm thuộc tính `href` (hypertext reference) vào thẻ mở anchor. Giá trị của thuộc tính href là đích đến mà chúng ta muốn liên kết hướng tới.

Thêm thuộc tính `href` sau vào phần tử anchor đã tạo trước đó rồi thử nhấp lại; đừng quên làm mới trình duyệt để các thay đổi mới có hiệu lực.

```html
<a href="https://www.theodinproject.com/about">About The Odin Project</a>
```

Theo mặc định, mọi văn bản được bao quanh bởi thẻ anchor nhưng không có thuộc tính `href` sẽ trông như văn bản thông thường. Nếu có thuộc tính `href`, trình duyệt sẽ hiển thị văn bản màu xanh và gạch chân để cho biết đó là một liên kết.

Cần lưu ý rằng bạn có thể dùng thẻ anchor để liên kết đến bất kỳ loại tài nguyên nào trên internet, không chỉ các tài liệu HTML khác. Bạn có thể liên kết đến video, tệp pdf, hình ảnh, v.v., nhưng phần lớn thời gian bạn sẽ liên kết đến các tài liệu HTML khác.

### Mở liên kết trong tab mới

Phương pháp trên mở liên kết trong cùng tab với trang web chứa liên kết. Đây là hành vi mặc định của hầu hết trình duyệt và có thể dễ dàng thay đổi. Tất cả những gì chúng ta cần là một thuộc tính khác: thuộc tính `target`.

Trong khi `href` chỉ định đích đến của liên kết, `target` chỉ định nơi tài nguyên được liên kết sẽ mở. Nếu không có thuộc tính này, theo mặc định nó nhận giá trị `_self`, mở liên kết trong tab hiện tại. Để mở liên kết trong tab hoặc cửa sổ mới (tùy cài đặt trình duyệt), bạn có thể đặt giá trị là `_blank` như sau:

```html
<a href="https://www.theodinproject.com/about" target="_blank" rel="noreferrer">About The Odin Project</a>
```

Có thể bạn đã nhận thấy chúng ta thêm thuộc tính `rel` ở trên. Thuộc tính này dùng để mô tả mối quan hệ giữa trang hiện tại và tài liệu được liên kết, đồng thời có thể nhận nhiều giá trị. Những giá trị liên quan nhất lúc này là:

- `noopener`: Ngăn tab mới truy cập trang gốc, nhờ đó loại bỏ khả năng xảy ra các cuộc tấn công lừa đảo như [tabnabbing](https://owasp.org/www-community/attacks/Reverse_Tabnabbing). Các trình duyệt hiện đại đều tự động đặt thuộc tính này cho mọi liên kết có `target="_blank"`, nhưng bạn vẫn thường thấy nó được đặt thủ công để tương thích với các trình duyệt cũ.
- `noreferrer`: Tương tự `noopener` nhưng còn ngăn một số thông tin nhất định về trang gốc được truyền sang trang mới; [thông tin referrer không phải lúc nào cũng nên được chia sẻ](https://developer.mozilla.org/en-US/docs/Web/Privacy/Guides/Referer_header:_privacy_and_security_concerns#the_referrer_problem).

### Liên kết tuyệt đối và tương đối

Nhìn chung, chúng ta sẽ tạo hai loại liên kết:

- Liên kết đến các trang trên những website khác trên internet.
- Liên kết đến các trang nằm trên website của chính chúng ta.

#### Liên kết tuyệt đối

Liên kết đến các trang trên những website khác trên internet được gọi là liên kết tuyệt đối. Một liên kết tuyệt đối điển hình gồm các phần sau: `scheme://domain/path`. Liên kết tuyệt đối luôn chứa [scheme và domain](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#basics_anatomy_of_a_url) của đích đến.

Chúng ta đã thấy một liên kết tuyệt đối hoạt động. Liên kết tạo đến trang About của The Odin Project trước đó là liên kết tuyệt đối vì nó chứa scheme và domain.

`https://www.theodinproject.com/about`

#### Liên kết tương đối

Liên kết đến các trang khác trong website của chính chúng ta được gọi là liên kết tương đối. Liên kết tương đối không bao gồm tên domain vì đó là một trang khác trên cùng website; nó giả định tên domain sẽ giống với trang nơi chúng ta tạo liên kết.

Liên kết tương đối chỉ bao gồm đường dẫn tệp đến trang khác, *tương đối* với trang nơi bạn tạo liên kết. Khái niệm này khá trừu tượng, hãy xem một ví dụ thực tế.

Trong thư mục `odin-links-and-images`, hãy tạo một tệp HTML khác có tên `about.html` và dán đoạn mã sau vào đó:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Odin Links and Images</title>
  </head>

  <body>
    <h1>About Page</h1>
  </body>
</html>
```

Quay lại trang index, thêm phần tử anchor sau để tạo liên kết đến trang about:

```html
<body>
  <h1>Homepage</h1>
  <a href="https://www.theodinproject.com/about">About The Odin Project</a>

  <a href="about.html">About</a>
</body>
```

Mở tệp index trong trình duyệt rồi nhấp vào liên kết about để bảo đảm mọi thứ đã được nối đúng. Khi nhấp vào liên kết, bạn sẽ được đưa đến trang about vừa tạo.

Điều này hoạt động vì trang index và trang about nằm trong cùng một thư mục. Vì vậy, chúng ta có thể dùng tên của nó (`about.html`) làm giá trị `href` của liên kết.

Tuy nhiên, thông thường chúng ta sẽ muốn tổ chức các thư mục của website gọn gàng hơn. Thông thường chỉ có `index.html` nằm ở thư mục gốc, còn mọi tệp HTML khác nằm trong thư mục riêng.

Tạo một thư mục có tên `pages` bên trong thư mục `odin-links-and-images` rồi chuyển tệp `about.html` vào thư mục mới này.

Làm mới trang index trong trình duyệt rồi nhấp vào liên kết about. Bây giờ liên kết sẽ bị hỏng vì vị trí của tệp about đã thay đổi.

Để sửa lỗi, chỉ cần cập nhật giá trị `href` của liên kết about để thêm thư mục `pages/`, vì đó là vị trí mới của tệp about *tương đối* với tệp index.

```html
<body>
  <h1>Homepage</h1>
  <a href="pages/about.html">About</a>
</body>
```

Làm mới trang index trong trình duyệt rồi thử nhấp lại vào liên kết about; bây giờ liên kết sẽ hoạt động bình thường.

Trong nhiều trường hợp, cách này sẽ hoạt động tốt; tuy nhiên, bạn vẫn có thể gặp các vấn đề bất ngờ. Thêm `./` vào trước liên kết thường sẽ ngăn được các vấn đề đó. Khi thêm `./`, bạn đang chỉ cho mã biết rằng nó phải bắt đầu tìm tệp/thư mục *tương đối* với thư mục `current`.

```html
<body>
  <h1>Homepage</h1>
  <a href="./pages/about.html">About</a>
</body>
```

#### Một phép ẩn dụ

Liên kết tuyệt đối và tương đối là một khái niệm khó xây dựng mô hình tư duy chính xác; một phép ẩn dụ có thể giúp ích:

Hãy hình dung tên domain (`town.com`) là một thị trấn, thư mục chứa website (`/museum`) là một viện bảo tàng, và mỗi trang trên website là một căn phòng trong bảo tàng (`/museum/movie_room.html` và `/museum/shops/coffee_shop.html`). Các liên kết tương đối như `./shops/coffee_shop.html` là chỉ dẫn từ căn phòng hiện tại (phòng chiếu phim của bảo tàng `/museum/movie_room.html`) đến một căn phòng khác (cửa hàng trong bảo tàng). Ngược lại, liên kết tuyệt đối là chỉ dẫn đầy đủ, bao gồm giao thức (`https`), tên domain (`town.com`) và đường dẫn từ domain đó (`/museum/shops/coffee_shop.html`): `https://town.com/museum/shops/coffee_shop.html`.

### Hình ảnh

Website sẽ khá nhàm chán nếu chỉ hiển thị được văn bản. May mắn là HTML cung cấp nhiều phần tử để hiển thị đủ loại phương tiện khác nhau. Phần tử được dùng phổ biến nhất trong số đó là phần tử hình ảnh.

Để hiển thị hình ảnh trong HTML, chúng ta dùng phần tử `<img>`. Không giống các phần tử đã gặp, phần tử `<img>` là một phần tử rỗng. Như đã học trước đó, phần tử rỗng không cần thẻ đóng vì vốn dĩ chúng trống và không chứa nội dung.

Thay vì bao quanh nội dung bằng thẻ mở và thẻ đóng, phần tử này nhúng hình ảnh vào trang bằng thuộc tính `src`, cho trình duyệt biết tệp hình ảnh nằm ở đâu. Thuộc tính `src` hoạt động gần giống thuộc tính `href` của thẻ anchor. Nó có thể nhúng hình ảnh bằng cả đường dẫn tuyệt đối và tương đối.

Ví dụ, bằng đường dẫn tuyệt đối, chúng ta có thể hiển thị một hình ảnh nằm trên website The Odin Project:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="gORbExZ" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/gORbExZ">
  absolute-path-image</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Để hiển thị hình ảnh trên website được lưu trữ trên máy chủ web của chính bạn, có thể dùng đường dẫn tương đối.

#### Linux, macOS, ChromeOS

1. Tạo một thư mục mới có tên `images` trong dự án `odin-links-and-images`.
1. Tiếp theo, [tải xuống hình ảnh thực hành](https://unsplash.com/photos/Mv9hjnEUHR4/download?force=true&w=640) rồi chuyển nó vào thư mục images vừa tạo.
1. Đổi tên hình ảnh thành `dog.jpg`.

#### WSL2

Khi tải tệp từ internet, Windows có một tính năng bảo mật tạo một tệp `Zone.Identifier` ẩn cùng tên với tệp đã tải xuống; tệp đó có dạng `mypicture.jpg:Zone.Identifier`. Tệp này không gây hại, nhưng chúng ta muốn tránh sao chép nó sang WSL2 và làm lộn xộn các thư mục.

1. Tạo một thư mục mới có tên `images` trong dự án `odin-links-and-images`.

1. Tiếp theo, [tải xuống hình ảnh chú chó](https://unsplash.com/photos/Mv9hjnEUHR4/download?force=true&w=640).

1. Nhấp chuột phải vào tệp vừa tải ở cuối cửa sổ chrome rồi chọn "Hiển thị trong thư mục".

   1. Ngoài ra, nếu không thấy gì ở cuối cửa sổ chrome, hãy mở "Tùy chỉnh và điều khiển Google Chrome" rồi chọn mục "Tệp tải xuống". Thao tác này sẽ hiển thị tất cả tệp đã tải xuống, mỗi tệp có nút "Hiển thị trong thư mục" riêng.

1. Đổi tên hình ảnh thành `dog.jpg`.

1. Kéo tệp từ thư mục tải xuống vào trình duyệt tệp của VSCode, thả vào thư mục `images` mới tạo.

   1. Ngoài ra, bằng terminal Ubuntu, hãy điều hướng đến thư mục bạn muốn sao chép hình ảnh vào (`cd ~/odin-links-and-images` chẳng hạn)

   1. Nhập `cp <space>`

   1. Kéo hình ảnh `dog.jpg` từ cửa sổ Windows Explorer rồi thả vào cửa sổ terminal; lúc đó nó sẽ xuất hiện dưới dạng `"/mnt/c/users/username/Downloads/dog.jpg"`

   1. Nhập `<space> .` để cho cp biết bạn muốn sao chép tệp vào thư mục làm việc hiện tại. Lệnh đầy đủ sẽ có dạng:

   ```bash
   cp "/mnt/c/users/username/Downloads/dog.jpg" .
   ```

   1. Nhấn <kbd>Enter</kbd> để hoàn tất lệnh, rồi dùng `ls` để xác nhận tệp đã tồn tại.

Kéo tệp từ Windows vào trình duyệt tệp của VSCode sẽ ngăn các tệp `Zone.Identifier` bị sao chép theo. Từ giờ, mỗi khi cần sao chép hình ảnh hoặc các tệp đã tải xuống khác vào WSL2, bạn có thể làm theo cách này. Nếu vô tình sao chép các tệp `Zone.Identifier` vào WSL2, bạn có thể xóa chúng an toàn mà không gặp vấn đề gì.

#### Thêm hình ảnh vào tệp HTML

Cuối cùng, thêm hình ảnh vào tệp `index.html`:

```html
<body>
  <h1>Homepage</h1>
  <a href="https://www.theodinproject.com/about">About The Odin Project</a>

  <a href="./pages/about.html">About</a>

  <img src="./images/dog.jpg">
</body>
```

Lưu tệp `index.html` rồi mở trong trình duyệt để ngắm Charles trong tất cả vẻ đáng yêu của nó.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Hình ảnh bạn dùng phải miễn phí cho mục đích dự định

Có rất nhiều hình ảnh miễn phí, nhưng hãy nhớ ghi công người tạo ra hình ảnh trong dự án của bạn.

Một cách dễ dàng để ghi công là đưa tên và thông tin liên hệ của tác giả vào tệp README trong repository của bạn, hoặc cung cấp [thông tin ghi công](https://support.freepik.com/s/article/Attribution-How-when-and-where?language=en_US).

Để tìm hình ảnh, hãy tìm trên Google Images, rồi trong kết quả hình ảnh chọn Tools -> Usage rights -> "Creative Commons". Chọn và nhấp vào một hình ảnh, sau đó nhấp vào thông tin giấy phép.

Chúng tôi khuyên bạn luôn xem lại các yêu cầu giấy phép của bất kỳ hình ảnh nào muốn sử dụng.

</div>

### Thư mục cha

Nếu muốn dùng hình ảnh chú chó trong trang about thì sao? Trước hết, chúng ta phải đi lên một cấp, ra khỏi thư mục pages vào thư mục cha của nó để có thể truy cập thư mục images.

Để đi đến thư mục cha, chúng ta dùng hai dấu chấm trong đường dẫn tương đối như sau: `../`. Hãy xem cách này hoạt động: trong phần body của tệp `about.html`, thêm hình ảnh sau bên dưới tiêu đề đã tạo trước đó:

```html
<img src="../images/dog.jpg">
```

Phân tích đoạn mã:

1. Trước hết, chúng ta đi đến thư mục cha của thư mục `pages`, tức `odin-links-and-images`.
1. Sau đó, từ thư mục cha, chúng ta đi vào thư mục `images`.
1. Cuối cùng, chúng ta có thể truy cập tệp `dog.jpg`.

Dùng phép ẩn dụ ở trên, sử dụng `../` trong đường dẫn tệp cũng giống như bước ra khỏi căn phòng hiện tại vào hành lang chính để đi đến một căn phòng khác.

### Thuộc tính alt

Ngoài thuộc tính `src`, mọi phần tử hình ảnh cũng nên có thuộc tính `alt` (văn bản thay thế).

Thuộc tính `alt` dùng để mô tả một hình ảnh. Nó sẽ được dùng thay cho hình ảnh nếu hình ảnh không thể tải. Trình đọc màn hình cũng dùng thuộc tính này để mô tả hình ảnh cho người dùng khiếm thị.

Ví dụ logo The Odin Project đã dùng trước đó sẽ có dạng sau khi thêm thuộc tính `alt`:

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="ExXjoEp" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/ExXjoEp">
  image-alt-attribute</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Để luyện tập, hãy thêm thuộc tính `alt` vào hình ảnh chú chó đã thêm trong dự án `odin-links-and-images`.

### Thuộc tính kích thước hình ảnh

Dù không bắt buộc tuyệt đối, việc chỉ định thuộc tính chiều cao và chiều rộng
trong thẻ hình ảnh giúp trình duyệt bố cục trang mà không khiến trang nhảy hoặc chớp.

Bạn nên tạo thói quen luôn chỉ định các thuộc tính này trên mọi hình ảnh, ngay cả khi hình ảnh đã đúng kích thước hoặc bạn đang dùng CSS để điều chỉnh. Mục đích là
ngăn nội dung bị nhảy khi hình ảnh tải. Hãy dùng kích thước thực tế của hình ảnh khi chỉ định kích thước.

Đây là ví dụ logo The Odin Project khi đã có thuộc tính chiều cao và chiều rộng:

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="PogmYGp" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/PogmYGp">
  Chiều cao và chiều rộng hình ảnh</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Hãy cập nhật dự án `odin-links-and-images` bằng các thuộc tính width và height cho hình ảnh chú chó.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem các video của Kevin Powell:
   - [Liên kết HTML](https://www.youtube.com/watch?v=tsEQgGjSmkM).
   - [Hình ảnh HTML](https://www.youtube.com/watch?v=0xoztJCHpbQ).
   - [Cấu trúc tệp](https://www.youtube.com/watch?v=ta3Oxx7Yqbo).
1. Đọc và lập trình theo bài viết của Interneting is Hard về [Liên kết và hình ảnh](https://internetingishard.netlify.app/html-and-css/links-and-images). Hãy chú ý phần trình bày bốn định dạng hình ảnh chính. Bài viết này cho rằng nên đặt kích thước hình ảnh bằng CSS. Tuy nhiên, chúng tôi vẫn khuyên bạn đặt thuộc tính width và height cho mọi hình ảnh như đã đề cập ở phần trước của bài học.

</div>