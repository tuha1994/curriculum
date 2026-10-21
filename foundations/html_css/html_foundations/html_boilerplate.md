### Giới thiệu

Mọi tài liệu HTML đều có cùng một cấu trúc cơ bản, hay còn gọi là boilerplate, cần được chuẩn bị trước khi có thể làm bất cứ điều gì hữu ích. Trong bài học này, chúng ta sẽ tìm hiểu các phần khác nhau của boilerplate và xem chúng kết hợp với nhau như thế nào.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Cách viết boilerplate cơ bản cho một tài liệu HTML.
- Cách mở tài liệu HTML trong trình duyệt.

### Tạo tệp HTML

Để minh họa một boilerplate HTML, trước hết chúng ta cần một tệp HTML để làm việc.

Tạo một thư mục mới trên máy tính và đặt tên là `html-boilerplate`. Trong thư mục đó, tạo một tệp mới có tên `index.html`.

Có lẽ bạn đã quen với nhiều loại tệp khác nhau, chẳng hạn tệp doc, pdf và hình ảnh.

Để cho máy tính biết rằng chúng ta muốn tạo một tệp HTML, cần thêm phần mở rộng `.html` vào tên tệp, giống như khi tạo tệp `index.html`.

Cần lưu ý rằng chúng ta đã đặt tên tệp HTML là `index`. Tệp HTML chứa trang chủ của website luôn nên được đặt tên là `index.html`. Lý do là máy chủ web mặc định sẽ tìm trang `index.html` khi người dùng truy cập website -- nếu không có tệp này, nhiều vấn đề nghiêm trọng có thể xảy ra.

### DOCTYPE

Mọi trang HTML đều bắt đầu bằng một khai báo doctype. Doctype cho trình duyệt biết nên dùng phiên bản HTML nào để hiển thị tài liệu. Phiên bản HTML mới nhất là HTML5, và doctype cho phiên bản đó là `<!DOCTYPE html>`.

Doctype của các phiên bản HTML cũ phức tạp hơn một chút. Ví dụ, đây là khai báo doctype của HTML4:

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

Tuy nhiên, có lẽ chúng ta sẽ không bao giờ muốn sử dụng phiên bản HTML cũ, vì vậy luôn dùng `<!DOCTYPE html>`.

Mở tệp `index.html` đã tạo trước đó trong trình soạn thảo văn bản và thêm `<!DOCTYPE html>` vào dòng đầu tiên.

### Phần tử HTML

Sau khi khai báo doctype, chúng ta cần cung cấp một phần tử `<html>`. Đây được gọi là phần tử gốc của tài liệu, nghĩa là mọi phần tử khác trong tài liệu đều là hậu duệ của nó.

Điều này sẽ trở nên quan trọng hơn khi chúng ta học cách thao tác HTML bằng JavaScript. Hiện tại, bạn chỉ cần biết rằng phần tử `<html>` nên xuất hiện trong mọi tài liệu HTML.

Quay lại tệp `index.html`, hãy thêm phần tử `<html>` bằng cách viết thẻ mở và thẻ đóng của nó như sau:

```html
<!DOCTYPE html>
<html lang="en">
</html>
```

Bạn có để ý từ `lang` ở đây không? Nó là một thuộc tính HTML gắn với thẻ HTML tương ứng, trong trường hợp này là `<html>`. Các thuộc tính này cung cấp thêm thông tin về các phần tử HTML. (Bạn sẽ tìm hiểu thêm về `HTML attributes` trong bài học tiếp theo.)

#### Thuộc tính lang là gì?

`lang` chỉ định ngôn ngữ của nội dung văn bản trong phần tử đó. Thuộc tính này chủ yếu được dùng để cải thiện khả năng tiếp cận của trang web. Nó cho phép công nghệ hỗ trợ, chẳng hạn trình đọc màn hình, điều chỉnh theo ngôn ngữ và phát âm chính xác.

### Phần tử head

Phần tử `<head>` là nơi đặt những thông tin meta quan trọng **về** các trang web và những thứ cần thiết để trang web hiển thị chính xác trong trình duyệt.
Bên trong `<head>`, **không nên** dùng phần tử nào hiển thị nội dung trên trang web.

Quay lại tệp `index.html`, hãy thêm một phần tử `<head>` cùng với một phần tử `<meta>` và một tiêu đề bên trong. Phần tử `<head>` nằm trong phần tử `<html>` và luôn phải là phần tử đầu tiên ngay sau thẻ mở `<html>`:

```html
<!DOCTYPE html>

<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
  </head>
</html>
```

#### Phần tử meta

Chúng ta luôn nên có thẻ `<meta>` chỉ định mã hóa ký tự của trang web trong phần tử `<head>`: `<meta charset="UTF-8">`.

Việc thiết lập mã hóa rất quan trọng vì nó bảo đảm trình duyệt hiển thị chính xác các ký hiệu và ký tự đặc biệt từ nhiều ngôn ngữ khác nhau.

#### Phần tử title

Một phần tử khác luôn nên có trong head của tài liệu HTML là phần tử `<title>`:

 `<title>My First Webpage</title>`

Phần tử `<title>` dùng để đặt một tiêu đề dễ đọc cho con người, được hiển thị trên thẻ trình duyệt của trang web. Ví dụ, nếu nhìn vào tên thẻ hiện tại của trình duyệt, bạn sẽ thấy "HTML Boilerplate &#124; The Odin Project"; đây là `<title>` của tệp `.html` hiện tại.

Nếu không có phần tử `<title>`, tiêu đề trang web sẽ mặc định là tên tệp. Trong trường hợp này là `index.html`, vốn không có nhiều ý nghĩa với người dùng; điều đó khiến việc tìm trang web trở nên rất khó nếu người dùng mở nhiều thẻ trình duyệt.

Có nhiều phần tử khác có thể đặt trong head của tài liệu HTML. Tuy nhiên, hiện tại điều quan trọng là chỉ cần biết hai phần tử đã học ở đây. Chúng ta sẽ giới thiệu thêm các phần tử đặt trong head ở những phần còn lại của chương trình học.

### Phần tử body

Phần tử cuối cùng cần có để hoàn thiện boilerplate HTML là phần tử `<body>`. Đây là nơi đặt toàn bộ nội dung sẽ hiển thị cho người dùng - văn bản, hình ảnh, danh sách, liên kết và nhiều thứ khác.

Để hoàn thiện boilerplate, hãy thêm phần tử `<body>` vào tệp `index.html`. Phần tử `<body>` cũng nằm trong phần tử `<html>` và luôn ở bên dưới phần tử `<head>`, như sau:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
  </head>

  <body>
  </body>
</html>
```

### Xem tệp HTML trong trình duyệt

Đến đây, boilerplate HTML trong tệp `index.html` đã hoàn chỉnh, nhưng làm thế nào để xem nó trong trình duyệt? Có một vài lựa chọn:

- Bạn có thể kéo thả tệp HTML từ trình soạn thảo văn bản vào thanh địa chỉ của trình duyệt.
- Bạn có thể mở tệp trực tiếp qua trình duyệt bằng cách nhấn <kbd>Ctrl + O</kbd> (<kbd>Cmd + O</kbd> trên macOS), sau đó điều hướng đến tệp HTML.
- Bạn có thể tìm tệp HTML trong hệ thống tệp rồi nhấp đúp vào tệp. Tệp sẽ mở trong trình duyệt mặc định của hệ thống.
- Bạn có thể dùng terminal để mở tệp trong trình duyệt:
  - Ubuntu: Điều hướng đến thư mục chứa tệp rồi nhập `google-chrome index.html`
  - macOS: Điều hướng đến thư mục chứa tệp rồi nhập `open ./index.html`
  - WSL: Điều hướng đến thư mục chứa tệp rồi nhập `explorer.exe index.html`. Tệp sẽ mở trong trình duyệt mặc định của hệ thống. Lưu ý, bạn chỉ được dùng tên tệp. Dùng đường dẫn (ví dụ `./index.html` hoặc đường dẫn tuyệt đối) sẽ mở Windows File Explorer thay vì tệp.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Sử dụng Google Chrome

Để tránh phải chia nhỏ hướng dẫn của bài học nhằm đáp ứng mọi khác biệt giữa các trình duyệt, từ giờ chúng ta sẽ dùng Google Chrome làm trình duyệt chính. Mọi tham chiếu đến trình duyệt đều chỉ Google Chrome. Chúng tôi **thực sự** khuyên bạn dùng Google Chrome cho mọi hoạt động kiểm thử sau này.

</div>

Hãy dùng một trong các phương pháp trên để mở tệp `index.html` đang làm việc. Bạn sẽ thấy màn hình trống. Đó là vì phần body chưa có nội dung nào để hiển thị.

Quay lại tệp `index.html`, hãy thêm một tiêu đề (sẽ tìm hiểu thêm sau) vào body rồi lưu tệp:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
  </head>

  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

Bây giờ, nếu làm mới trang trong trình duyệt, bạn sẽ thấy thay đổi có hiệu lực và tiêu đề "Hello World!" được hiển thị.

### Phím tắt VSCode

VSCode có một phím tắt tích hợp giúp bạn tạo toàn bộ boilerplate chỉ trong một lần. Lưu ý rằng phím tắt này chỉ hoạt động khi đang chỉnh sửa tệp có phần mở rộng `.html` hoặc tệp văn bản đã chọn ngôn ngữ HTML. Để kích hoạt phím tắt, hãy xóa mọi thứ trong tệp `index.html` rồi chỉ nhập `!` ở dòng đầu tiên. Một vài lựa chọn sẽ xuất hiện. Nhấn phím <kbd>Enter</kbd> để chọn lựa chọn đầu tiên, và thế là bạn đã có toàn bộ boilerplate được điền sẵn.

Bạn có thể nhận thấy boilerplate do phím tắt này tạo ra có một dòng mà chúng ta chưa đề cập:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Đây chưa phải là điều chúng ta cần biết cho đến khi thảo luận về thiết kế đáp ứng, một chủ đề nâng cao liên quan đến các kích thước màn hình khác nhau mà chúng ta sẽ học mãi về sau trong chương trình học. Hiện tại, bạn có thể giữ nguyên dòng đó.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem và làm theo video tuyệt vời [Xây dựng trang web đầu tiên của bạn](https://www.youtube.com/watch?v=V8UAEoOvqFg&t=93s) của Kevin Powell.

1. Chạy boilerplate của bạn qua [trình xác thực HTML](https://validator.w3.org/#validate_by_input) của W3. Trình xác thực bảo đảm markup của bạn chính xác và là công cụ học tập tuyệt vời, vì chúng cung cấp phản hồi về những lỗi cú pháp bạn có thể thường xuyên mắc phải mà không nhận ra, chẳng hạn thẻ đóng bị thiếu và khoảng trắng thừa trong HTML.

</div>