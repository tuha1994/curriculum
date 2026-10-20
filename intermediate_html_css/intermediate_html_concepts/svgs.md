### Giới thiệu

SVG là một định dạng hình ảnh rất phổ biến trên web. Ban đầu chúng có thể hơi khó hiểu, nhưng một khi biết cách sử dụng, SVG là công cụ cực kỳ mạnh để tạo ra những hình ảnh chất lượng cao, sống động cho trang web của bạn.

Trong bài học này, chúng ta sẽ tìm hiểu chính xác SVG là gì, chúng được dùng để làm gì và cách nhúng chúng vào trang web.

### Tổng quan bài học

Phần này giới thiệu khái quát các chủ đề bạn sẽ học trong bài.

- SVG, Vector Graphics và XML là gì.
- Cách tạo SVG đơn giản và thêm chúng vào trang web.
- Khi nào nên dùng SVG và khi nào một định dạng hình ảnh khác sẽ phù hợp hơn.

### SVG là gì?

SVG là một định dạng hình ảnh *có thể co giãn*, nghĩa là chúng dễ dàng thay đổi kích thước đến bất kỳ mức nào mà vẫn giữ nguyên chất lượng, không làm tăng kích thước tệp. SVG cũng rất hữu ích nếu bạn cần tạo hoặc chỉnh sửa hình ảnh bằng chương trình, vì bạn có thể thay đổi thuộc tính của chúng thông qua CSS và JavaScript.

SVG thường được dùng cho:

1. Biểu tượng
1. Đồ thị/biểu đồ
1. Hình ảnh lớn, đơn giản
1. Nền có họa tiết
1. Áp dụng hiệu ứng cho các phần tử khác bằng bộ lọc SVG

### Được rồi, nhưng chúng là gì?

"SVG" là viết tắt của "Scalable Vector Graphics". Đồ họa vector là những hình ảnh được định nghĩa bằng toán học, trái ngược với "đồ họa raster" truyền thống, trong đó hình ảnh được định nghĩa bằng một lưới pixel. Với đồ họa raster, mức độ chi tiết bị giới hạn bởi kích thước của lưới pixel đó. Nếu muốn tăng kích thước hình ảnh (*scale*), bạn phải tăng kích thước lưới. Bạn quyết định tất cả những pixel mới đó sẽ trông như thế nào bằng cách nào? Không có giải pháp đơn giản. Ngoài ra, lưới càng lớn thì kích thước tệp càng tăng.

Ngược lại, đồ họa vector không có lưới. Thay vào đó, bạn có các công thức cho những hình dạng và đường nét khác nhau. Vì chúng chỉ là các công thức, việc bạn muốn chúng hiển thị lớn hay nhỏ không quan trọng -- chúng có thể co giãn đến bất kỳ kích thước nào mà không ảnh hưởng đến chất lượng hoặc kích thước tệp.

SVG còn có một điểm thú vị khác: chúng được định nghĩa bằng XML. XML (hay còn gọi là "Extensible Markup Language") là một cú pháp giống HTML, được dùng cho rất nhiều thứ, từ [API](https://en.wikipedia.org/wiki/API), [RSS](https://en.wikipedia.org/wiki/RSS), cho đến [phần mềm bảng tính và soạn thảo văn bản](https://en.wikipedia.org/wiki/Office_Open_XML).

Việc mã nguồn SVG là XML mang lại một vài lợi ích chính.

Trước hết, điều đó có nghĩa là nó *dễ đọc đối với con người*. Nếu mở một JPEG trong trình soạn thảo văn bản, bạn chỉ thấy những ký tự vô nghĩa. Tuy nhiên, nếu mở một SVG, bạn sẽ thấy nội dung tương tự như sau:

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect x=0 y=0 width=100 height=50 />
  <circle class="svg-circle" cx="50" cy="50" r="10"/>
</svg>
```

Nội dung này vẫn có thể gây khó hiểu, nhưng này -- đó là các từ! Các tag! Các thuộc tính! So với những [định dạng tệp nhị phân](https://en.wikipedia.org/wiki/Binary_file) như JPEG, chúng ta đang ở một vùng quen thuộc hơn nhiều.

Lợi ích thứ hai của XML là nó được thiết kế để có thể tương tác với HTML. Điều đó có nghĩa là bạn có thể đặt trực tiếp đoạn mã trên vào tệp HTML mà không cần thay đổi gì, và hình ảnh sẽ được hiển thị. Vì các phần tử này có thể trở thành phần tử trong DOM giống như phần tử HTML, bạn có thể chọn chúng bằng CSS và tạo chúng bằng [Element WebAPI](https://developer.mozilla.org/en-US/docs/Web/API/Element) mà bạn đã sử dụng!

### Nhược điểm

Rõ ràng SVG thật tuyệt! Vậy đã đến lúc chuyển tất cả hình ảnh sang SVG, đúng không? Không hẳn. SVG *rất phù hợp* với những hình ảnh tương đối đơn giản, nhưng vì mọi chi tiết của hình ảnh đều phải được viết ra dưới dạng XML, chúng cực kỳ kém hiệu quả khi lưu trữ những hình ảnh phức tạp. Nếu hình ảnh của bạn cần trông như ảnh chụp, hoặc có chi tiết hay kết cấu tinh vi ("[kết cấu grunge](https://unsplash.com/s/photos/grunge-texture)" là một ví dụ điển hình), SVG không phải công cụ phù hợp.

### Giải phẫu một SVG

Thông thường, bạn sẽ không muốn tự tạo SVG từ đầu trong mã của mình. Phần lớn thời gian, bạn sẽ tải tệp xuống hoặc sao chép mã từ một trang web hay một trình chỉnh sửa hình ảnh có thể tạo SVG ( [Material icons](https://fonts.google.com/icons) và [Feather icons](https://feathericons.com/) là những ví dụ về thư viện biểu tượng SVG phổ biến). Tuy nhiên, việc tải một SVG xuống rồi muốn tinh chỉnh hoặc điều chỉnh một chút là khá thường gặp, nên biết từng phần là gì và chúng hoạt động ra sao rất hữu ích.

<p class="codepen" data-height="300" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="NWaGdmL" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>Xem Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/NWaGdmL">
  Simple SVG Example</a> của TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)

  trên <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

1. `xmlns` - là viết tắt của "XML NameSpace". Thuộc tính này chỉ định bạn đang dùng *phương ngữ* XML nào. Trong trường hợp của chúng ta, đó là đặc tả ngôn ngữ SVG. Nếu không có thuộc tính này, một số trình duyệt sẽ không hiển thị hình ảnh hoặc hiển thị không chính xác.
1. `viewBox` - định nghĩa giới hạn của SVG. Khi phải xác định vị trí của những điểm khác nhau trong các phần tử SVG, đây là giới hạn được tham chiếu. Nó cũng định nghĩa tỷ lệ khung hình *và* gốc tọa độ của SVG. Vì vậy, thuộc tính này đảm nhiệm khá nhiều việc! Hãy thử thay đổi các giá trị khác nhau trong ví dụ trên để cảm nhận ảnh hưởng của nó đến các hình dạng.
1. `class`, `id` - các thuộc tính này hoạt động giống như trong HTML. Dùng chúng trong SVG cho phép bạn dễ dàng chọn một phần tử bằng CSS hoặc JavaScript, hoặc tái sử dụng một phần tử thông qua [phần tử `use`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use).
1. Các phần tử như `<circle>`, `<rect>`, `<path>` và `<text>` được định nghĩa bởi namespace SVG. Đây là những khối xây dựng cơ bản của chúng ta. Mặc dù bạn có thể tạo những hình ảnh SVG cực kỳ phức tạp, phần lớn chúng chỉ được tạo bằng khoảng một tá phần tử cơ bản này. Đây là [danh sách đầy đủ các phần tử SVG](https://developer.mozilla.org/en-US/docs/Web/SVG/Element).
1. Nhiều thuộc tính SVG, chẳng hạn `fill` và `stroke`, có thể được thay đổi trong CSS. Tìm hiểu thêm trong [bài viết về thuộc tính SVG và CSS](https://css-tricks.com/svg-properties-and-css/).

Hãy thử thay đổi đoạn mã trên và cảm nhận điều gì đang xảy ra. Điều gì xảy ra khi bạn thay đổi kích thước viewBox? Hoặc các thuộc tính của một phần tử?

### Nhúng SVG

Có hai cách tiếp cận chính khi quyết định cách thực sự đặt SVG vào tài liệu: liên kết và inline.

Việc liên kết SVG về cơ bản giống với việc liên kết bất kỳ hình ảnh nào khác. Bạn có thể dùng một phần tử hình ảnh HTML như `<img>`, hoặc liên kết nó trong CSS bằng `background-image: url(./my-image.svg)`. SVG vẫn sẽ co giãn đúng cách, nhưng nội dung của SVG sẽ không thể được truy cập từ trang web.

Cách thay thế là inline SVG bằng cách dán trực tiếp nội dung của nó vào mã trang web, thay vì liên kết nó như một hình ảnh. SVG vẫn được hiển thị chính xác, nhưng các thuộc tính của SVG sẽ hiển thị với mã của bạn, cho phép bạn thay đổi hình ảnh một cách động bằng CSS hoặc JavaScript.

Inline SVG giúp bạn khai thác toàn bộ tiềm năng của chúng, nhưng cũng có một số nhược điểm nghiêm trọng: mã khó đọc hơn, trang khó được cache hơn, và nếu SVG lớn thì nó có thể làm chậm việc tải phần HTML còn lại.

Một số nhược điểm của việc inline mã SVG có thể được tránh khi bạn đã học một thư viện JavaScript front-end như React hoặc một build-tool như webpack. Chúng ta chưa sẵn sàng tìm hiểu những thứ đó, nên hiện tại chỉ cần ghi nhớ điều này.

Bây giờ, hãy dùng cách phù hợp nhất với trường hợp của bạn. Liên kết thường gọn gàng và đơn giản hơn, vì vậy hãy ưu tiên cách đó trừ khi bạn cần tinh chỉnh mã SVG cùng với HTML.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [A Friendly Introduction to SVG](https://www.joshwcomeau.com/svg/friendly-introduction-to-svg/) của Josh Comeau, một hướng dẫn tuyệt vời có các bản minh họa tương tác về hình dạng và co giãn. Dừng lại khi đến phần về animation (animation sẽ được đề cập ở phần sau của curriculum).

</div>