### Giới thiệu

Trước Rails 7, đã tồn tại một vấn đề kéo dài về cách quản lý các package JavaScript bên thứ ba trong ứng dụng Rails. Cách tiếp cận ban đầu là bọc JavaScript trong một Ruby Gem rồi phát hành nó. Dù cách này mang lại khả năng quản lý phiên bản và độ ổn định, việc cập nhật lên bản phát hành mới nhất của các thư viện lại chậm, vì bạn phải chờ người duy trì Ruby Gem cập nhật, kiểm thử và phát hành phiên bản gem mới.

Rails 6 đã cố gắng giải quyết vấn đề này bằng cách tạo một Gem bọc quanh [webpack](https://webpack.js.org/). Gem này có tên Webpacker, sử dụng quy ước nổi tiếng của Rails là convention over configuration để cho phép dùng các thư viện JS mới nhất với một số thiết lập mặc định hợp lý. Tuy vậy, cách này không phải không có vấn đề; vấn đề chính là nếu muốn rời khỏi những quy ước đó, bạn cần hiểu Webpacker sâu hơn. Đáng tiếc là nó không giải quyết được các vấn đề vốn cần giải quyết, nên một cách tiếp cận mới là cần thiết.

Cùng với Rails 7, chúng ta có [importmap](https://github.com/rails/importmap-rails). Cách tiếp cận này được chọn để quản lý JavaScript trong ứng dụng vì một vài lý do. Thứ nhất, ES6 hiện đã được mọi trình duyệt đáng kể hỗ trợ: Chrome, Edge, Safari và Firefox. Thứ hai, HTTP/2 hiện đã trở thành tiêu chuẩn, nghĩa là bạn có thể dùng một kết nối duy nhất để trả về nhiều tệp nhỏ cho client. Nhờ vậy, bạn có thể loại bỏ bundler dành cho JavaScript.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài học này.

- Import maps là gì.
- Import maps hoạt động như thế nào.
- Cách sử dụng import maps.
- `pin` là gì.

### Import maps

Import maps cho phép bạn import các module JavaScript bằng những tên logic ánh xạ tới các tệp đã version/digest – trực tiếp từ trình duyệt. Nhờ đó, bạn có thể xây dựng các ứng dụng JavaScript hiện đại bằng những thư viện JavaScript dành cho ES modules (ESM) mà không cần [transpiling](https://en.wikipedia.org/wiki/Source-to-source_compiler) hoặc [bundling](https://www.crft.studio/glossary/bundling). Điều này giúp bạn không còn cần Webpack, Yarn, npm hay bất kỳ phần nào khác của toolchain JavaScript. Tất cả những gì bạn cần là asset pipeline vốn đã được tích hợp trong Rails.

Với cách tiếp cận này, bạn sẽ gửi nhiều tệp JavaScript nhỏ thay vì một tệp JavaScript lớn. Nhờ HTTP/2, điều đó không còn gây tổn thất hiệu năng đáng kể trong lần truyền tải ban đầu; trên thực tế, về lâu dài nó còn đem lại lợi ích đáng kể nhờ cơ chế caching tốt hơn. Trước đây, bất kỳ thay đổi nào ở một tệp JavaScript nằm trong bundle lớn đều làm mất hiệu lực cache của toàn bộ bundle; giờ đây, chỉ cache của riêng tệp đó bị vô hiệu hóa.

#### Import maps hoạt động như thế nào?

Điều đầu tiên cần biết là import maps không phải giải pháp riêng của Rails. Đây là một giải pháp đã có sẵn mà đội ngũ Rails cho rằng phù hợp nhất để đưa JavaScript vào ứng dụng Rails như một lựa chọn mặc định hợp lý. Ý tưởng cơ bản là bạn có thể import các thư viện JavaScript vào ứng dụng chỉ bằng cách tham chiếu tên của chúng, thay vì phải tham chiếu chính xác vị trí thư viện. Về bản chất, import maps thực hiện việc thay thế chuỗi cho các "bare module specifiers". Nếu chưa quen với "bare module specifiers", chúng có dạng như sau: `import React from "react"`. Tuy nhiên, chỉ viết import theo cách này hiện chưa phải là cách hợp lệ để nạp mã JavaScript vào tệp hiện tại. Các cách hợp lệ hiện nay được quy định trong thứ gọi là ESM loader spec; nếu muốn, bạn có thể đọc thêm về nó trong [tài liệu Node](https://nodejs.org/api/esm.html)

Để tương thích với ESM, khi nạp mã JavaScript, bạn phải cung cấp một trong các specifier sau:

- Đường dẫn tuyệt đối:
    `import React from "/Users/Odin/projects/TOP/node_modules/react"`

- Đường dẫn tương đối:
    `import React from "./node_modules/react"`

- Đường dẫn HTTP:
    `import React from "https://ga.jspm.io/npm:react@18.2.0/index.js"`

Gem `importmap-rails` cung cấp một API để ánh xạ các "bare module specifiers" vào 1 trong 3 cách nạp package JavaScript ESM.

Ví dụ

```ruby
# config/importmap.rb
pin "react", to: "https://ga.jspm.io/npm:react@18.2.0/index.js"
```

Điều này có nghĩa là mỗi khi bạn thấy `import React from "react"` trong mã, nó thực tế sẽ tương đương với việc viết `import React from "https://ga.jspm.io/npm:react@18.2.0/index.js"`.

#### Sử dụng import maps

Khi tạo một ứng dụng Rails 7+ mới, `importmap-rails` sẽ tự động được thêm vào Gemfile. `importmap-rails` thực hiện một vài việc cho bạn. Trước hết, dòng `<%= javascript_importmap_tags %>` được thêm vào `views/layouts/application.html.erb`; dòng này thiết lập cấu hình JSON bên trong thẻ `<script type="importmap">`. Sau đó, [es-module-shim](https://github.com/guybedford/es-module-shims) được nạp. Cuối cùng, entry point của ứng dụng được import qua `<script type="module">import "application"</script>`. Bên trong `app/javascript/application.js`, bạn thiết lập ứng dụng bằng cách import mọi module đã được định nghĩa trong import map. Nên dùng các tên logic khớp với tên package được NPM sử dụng; như vậy, nếu muốn bắt đầu transpile hoặc bundle mã, bạn sẽ không phải thay đổi các module import.

Bạn bắt đầu thiết lập thông qua tệp cấu hình nằm tại `config/importmap.rb`. Tệp này tự động được reload trong môi trường development khi có bất kỳ thay đổi nào. Tuy nhiên, nếu xóa các pin và muốn chúng biến mất khỏi importmap đã render hoặc danh sách preload, bạn BẮT BUỘC phải khởi động lại server.

### Import maps với các package NPM

`importmap-rails` tải xuống và vendor các dependency package npm của bạn thông qua các CDN JavaScript.

Bạn có thể dùng lệnh `./bin/importmap` được thêm vào trong quá trình cài đặt để `pin`, `unpin` hoặc `update` các package npm trong import map. Lệnh này phân giải các dependency của package và thêm các pin vào `config/importmap.rb`.

```bash
$ ./bin/importmap pin react react-dom
Pinning "react" to vendor/javascript/react.js via download from https://ga.jspm.io/npm:react@18.2.0/index.js
Pinning "react-dom" to vendor/javascript/react-dom.js via download from https://ga.jspm.io/npm:react-dom@18.2.0/index.js
Pinning "scheduler" to vendor/javascript/scheduler.js via download from https://ga.jspm.io/npm:scheduler@0.23.0/index.js
```

Lệnh này sẽ tạo các pin trong `config/importmap.rb` như sau:

```ruby
pin "react" # @18.2.0
pin "react-dom" # @18.2.0
pin "scheduler" # @0.23.0
```

Các package được tải xuống thư mục `vendor/javascript`, nơi bạn có thể đưa chúng vào hệ thống quản lý mã nguồn; sau đó chúng sẽ khả dụng thông qua asset pipeline của ứng dụng.

Giờ đây, bạn có thể import chúng trong mã JavaScript.

```javascript
import React from "react"
import ReactDOM from "react-dom"
```

Nếu muốn xóa một pin đã tải xuống:

```bash
$ ./bin/importmap unpin react
Unpinning and removing "react"
```

#### Preload các module đã pin

Để tránh việc trình duyệt phải tải lần lượt từng tệp trước khi đến được import lồng sâu nhất, `importmap-rails` mặc định sử dụng [modulepreload](https://developers.google.com/web/updates/2017/12/modulepreload). Nếu không muốn preload một dependency, hãy thêm `preload: false` vào pin.

```ruby
# config/importmap.rb
pin "@github/hotkey", to: "@github--hotkey.js" # file lives in vendor/javascript/@github--hotkey.js
pin "md5", preload: false # file lives in vendor/javascript/md5.js
```

```erb
# app/views/layouts/application.html.erb
<%= javascript_importmap_tags %>

# will include the following link before the importmap is setup:
<link rel="modulepreload" href="/assets/javascript/@github--hotkey.js">
```

### Những điều cần cân nhắc về importmap

Dù import maps là một cách tuyệt vời để bắt đầu với Rails và cho phép bạn nhanh chóng sử dụng các thư viện JavaScript mà không phải lo về bundling hay transpiling, vẫn có một số vấn đề rộng hơn mà bạn cần lưu ý.

Vấn đề đầu tiên là quản lý dependency. Đây là trường hợp package bạn đang dùng phụ thuộc vào các package khác để hoạt động. Dùng import maps sẽ khóa phiên bản dependency theo yêu cầu của package bạn đang dùng, nhưng điều gì xảy ra nếu bạn thêm một package khác phụ thuộc vào cùng dependency đó nhưng ở phiên bản khác? Bạn sẽ bắt đầu gặp lỗi, và việc lần theo nguyên nhân có thể rất khó. Ở giai đoạn này, import maps không quản lý dependency, vì vậy phù hợp nhất khi bạn phụ thuộc tương đối ít vào các package bên thứ ba.

Quản lý dependency cũng dẫn đến vấn đề tiếp theo: làm thế nào để quản lý phiên bản thư viện nhằm giữ chúng luôn cập nhật? Nếu dùng một công cụ như webpack, công cụ này yêu cầu một tệp package.json tường minh để ánh xạ thư viện tới phiên bản, rồi một tệp lock để quản lý toàn bộ dependency, bạn có thể dựa vào các dịch vụ như [Dependabot của GitHub](https://github.com/dependabot) để tự cập nhật các package bằng cách gửi pull request đến repository GitHub của bạn. Khi dùng import maps, hiện chưa có cách làm tương tự, nên bạn sẽ phải tự quản lý các bản cập nhật. Nếu có một bản cập nhật bảo mật nghiêm trọng cho thư viện mà bạn phụ thuộc nhưng không biết đến thì sao? Ứng dụng của bạn có thể bị bỏ ngỏ trước bất kỳ ai muốn khai thác lỗ hổng đó.

Một vấn đề chính khác là khi dùng import maps, bạn có ít quyền kiểm soát hơn đối với việc bundling asset. Vì vậy, nếu cần làm gì phức tạp hơn việc chỉ import mã package bên thứ ba vào ứng dụng, import maps sẽ không đáp ứng được. Nó không thể transpile hoặc bundle bất kỳ mã nào. Khi đó, bạn sẽ cần một lựa chọn khác mà chúng ta sẽ đề cập trong bài học sau.

Tất cả những điều trên có thể khiến bạn tự hỏi, xét cho cùng thì tại sao lại dùng import maps. Chúng tôi cho rằng chúng thực sự phản ánh rất đúng cách làm của Rails. Hãy có một lựa chọn mặc định mà bạn có thể sử dụng lâu nhất có thể; khi yêu cầu phát triển vượt ra ngoài lựa chọn đó, bạn sẽ có những phương án khác để sử dụng. Các phương án ấy thêm một lớp phức tạp, nhưng sự phức tạp đó là chính đáng. Với Webpacker, sự phức tạp bị áp đặt lên bạn ngay cả khi bạn không cần đến nó. Vì vậy, hãy bắt đầu với import maps cho mọi ứng dụng và chỉ thay đổi khi thực sự cần.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Trước hết, hãy [xem qua README của gem `importmap-rails` trên GitHub](https://github.com/rails/importmap-rails). Tài liệu này chứa RẤT NHIỀU thông tin trong bài học này và còn nhiều hơn nữa, trực tiếp từ nguồn chính.
1. Đọc [bài viết của David Heinemeier Hansson](https://world.hey.com/dhh/modern-web-apps-without-javascript-bundling-or-transpiling-a20f2755) về các ứng dụng web hiện đại không bundling hoặc transpiling JavaScript. Ông giải thích lý do và cách thức bổ sung `importmap` vào Rails 7.
1. Appsignal có một bài viết hay về [Import maps bên trong hoạt động như thế nào](https://blog.appsignal.com/2022/03/02/import-maps-under-the-hood-in-rails-7.html), rất đáng đọc để hiểu thêm về những nội dung đã đề cập ở trên.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [Bài viết về cách dùng `importmap`](https://blog.kiprosh.com/rails-7-brings-import-maps-into-the-limelight/)
- [Tài liệu của MDN về `importmap`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap) sẽ giải đáp những câu hỏi khác mà bạn có thể có về chủ đề này.