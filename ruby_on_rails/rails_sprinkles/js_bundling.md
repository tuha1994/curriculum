### Giới thiệu

Bài học này sẽ trình bày gem [jsbundling-rails](https://github.com/rails/jsbundling-rails) mới cùng một số ưu điểm và nhược điểm khi sử dụng gem này.

Vai trò của jsbundling-rails là cung cấp các trình cài đặt để thiết lập esbuild, rollup hoặc webpack, từ đó cho phép đóng gói. Sau khi thiết lập trình đóng gói mình chọn, đầu ra đã đóng gói sẽ sử dụng `app/assets/builds` để lưu bundle; bạn có thể tham chiếu đến bundle này theo cách tiếp cận pipeline asset tiêu chuẩn.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Cách sử dụng trình đóng gói với một ứng dụng Rails dựa trên import map.
- Vì sao bạn có thể chọn đóng gói JS thay vì import map.
- Một số nhược điểm của việc đóng gói JS.
- Vì sao Webpacker được sử dụng cho Rails 6.
- Các tùy chọn đóng gói JS.

### Các tùy chọn đóng gói JS

Trước khi cài gem jsbundling-rails, hãy cùng xem qua một vài tùy chọn. Có ba tùy chọn:

#### Esbuild

Esbuild là một trình đóng gói nhẹ và nhanh, được viết bằng [Go](https://go.dev/doc/). Các trình đóng gói khác có thể chậm hơn nhưng nhiều tính năng hơn, khiến thời gian chạy dài hơn. Esbuild đạt tốc độ cao bằng nhiều cách, chẳng hạn tận dụng tính song song để sử dụng mọi lõi CPU hiện có.

#### Rollup

Rollup là một trình đóng gói khác, cho phép bạn sử dụng cú pháp JavaScript để import và export các hàm, dữ liệu nhằm chia sẻ chúng giữa các script riêng biệt, sau đó chuyển chúng sang những định dạng được hỗ trợ hiện có.

#### Webpack

Trình đóng gói cuối cùng được cung cấp thông qua jsbundling-rails là webpack. Đây là một trình đóng gói module tĩnh, sử dụng một entry point trong ứng dụng để tạo đồ thị phụ thuộc, rồi kết hợp mọi module mà dự án cần thành một hoặc nhiều bundle để ứng dụng sử dụng.

Giờ bạn đã được giới thiệu về các trình đóng gói do jsbundling-rails cung cấp; hãy bắt đầu thiết lập một trình đóng gói. Nhưng trước hết, hãy [cài đặt Yarn Classic](https://classic.yarnpkg.com/en/docs/install) (việc chọn Yarn Classic là có chủ ý).

Tương tự Bundler đối với gem Ruby, Yarn là một trình quản lý package JavaScript, dùng để quản lý các dependency JavaScript trong dự án. Bạn đã có npm (một trình quản lý package JavaScript khác) được cài từ Foundations, nhưng các thiết lập mặc định của Yarn Classic không yêu cầu chúng ta cấu hình thêm gì cho Rails, và Yarn Classic vẫn được sử dụng rộng rãi với Rails trong ngành.

### Sử dụng yarn build watch

Trước khi giới thiệu cách sử dụng jsbundling-rails, hãy cùng tìm hiểu một lệnh mới mạnh mẽ của yarn.

```bash
yarn build --watch
```

Lệnh này bắt đầu bằng việc gọi yarn với lệnh build, lệnh này xử lý tất cả entry point JavaScript thành `app/assets/builds/application.js`.

Cờ `--watch` cho phép trình đóng gói theo dõi các thay đổi trong những file JavaScript của dự án. Khi có thay đổi, nó bắt đầu đóng gói các entry point đó thành JavaScript và cập nhật dự án.

Khi làm vậy, bạn cũng cần chạy `rails server` trong một terminal khác. Sau khi nhập lệnh, bạn sẽ thấy chương trình vẫn tiếp tục chạy và hiển thị `[watch] build finished, watching for changes...`. Đây là hành vi đúng như dự kiến.

Chương trình sẽ chờ các thay đổi xuất hiện trong file JavaScript, rồi đóng gói lại chúng; việc này cập nhật file bundle được chuyển đến asset pipeline.

Một lệnh hữu ích khác là `./bin/dev`, lệnh này khởi chạy `yarn build --watch` và `rails server` cùng lúc. Nhờ vậy, bạn không phải chuyển qua lại giữa các cửa sổ terminal khi phát triển. Trong chế độ development, lệnh `./bin/dev` rất hữu ích vì cho phép trình theo dõi bundle JavaScript và Rails server tự cập nhật mà không cần bạn cập nhật thủ công; yarn sẽ tự động theo dõi các thay đổi đó.

### Sử dụng jsbundling-rails

Trước tiên, hãy đảm bảo Yarn và Node đã được cài đặt. Chạy `yarn -v` và `node -v` trong terminal. Cả hai lệnh đều phải trả về một số cho biết phiên bản.

Giờ hãy thực hiện các bước này với một app mới; ở bước cuối, chọn esbuild cho JavaScript. Chạy lệnh dưới đây.

```bash
rails new testapp
```

Thêm gem jsbundling-rails vào Gemfile.

```ruby
gem 'jsbundling-rails'
```

Sau đó chạy lệnh dưới đây trong terminal, tại thư mục ứng dụng Rails.

```bash
bundle install
```

Hãy nhớ rằng ba tùy chọn trình đóng gói là esbuild, rollup và webpack; bạn chỉ chọn một.

```bash
bin/rails javascript:install:<replace the text and <> with your bundler choice>
```

Sau khi chạy lệnh này, bạn sẽ thấy một vài lỗi, nhưng đừng lo! Chúng ta sẽ xem xét từng lỗi một.

```bash
✘ [ERROR] Could not resolve "@hotwired/turbo-rails"

app/javascript/application.js:2:7:
2 │ import "@hotwired/turbo-rails"
╵        ~~~~~~~~~~~~~~~~~~~~~~~
```

Nội dung lỗi khá rõ ràng. Resolve chỉ là cách nói hoa mỹ rằng trình đóng gói không thể xử lý import này. Điều đó hợp lý, vì testapp ban đầu được thiết lập bằng import map chứ không phải esbuild hay một trình đóng gói khác.

Giờ hãy xem liệu chúng ta có thể resolve lỗi này trong `app/javascript/application.js` hay không. File trông có vẻ ổn, vậy vấn đề là gì? Hãy nhớ rằng ứng dụng Rails này được khởi tạo bằng *import map*, không phải trình đóng gói. Chúng ta cần cài package để nó có thể được đóng gói đúng cách.

Giờ hãy chạy lệnh dưới đây.

```bash
yarn add @hotwired/turbo-rails
```

Tiếp theo, chạy lệnh build.

```bash
yarn run build
```

Lỗi đã biến mất, nghĩa là trình đóng gói có thể resolve nó vì package đã được định dạng đúng. Hãy xem lỗi tiếp theo:

```bash
✘ [ERROR] Could not resolve "controllers"

    app/javascript/application.js:3:7:
      3 │ import "controllers"
        │        ~~~~~~~~~~~~~
        ╵        "./controllers"
```

Với lỗi này, trình đóng gói đang muốn tìm một package có tên `controllers`, nhưng điều chúng ta muốn là import code từ thư mục `javascript/controllers/` trong dự án. Để xử lý đúng, ta có thể làm theo gợi ý của lỗi và đổi thành một đường dẫn tương đối:

```javascript
// app/javascript/application.js

import "@hotwired/turbo-rails"
import "./controllers"
```

Giờ chạy lại lệnh build, bạn sẽ thấy lỗi sau:

```bash
✘ [ERROR] Could not resolve "@hotwired/stimulus-loading"

    app/javascript/controllers/index.js:6:41:
      6 │ import { eagerLoadControllersFrom } from "@hotwired/stimulus-loading"
        ╵                                          ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

Lỗi này khó hiểu hơn một chút so với các lỗi trước. Nếu thử chạy `yarn add @hotwired/stimulus-loading`, bạn sẽ thấy không tìm được package có tên đó. Package này chỉ được dùng với import map như một cách quản lý việc nạp các Stimulus controller.

Để giải quyết lỗi này và các vấn đề khác trong file `app/javascript/controllers/index.js`, bạn có thể chạy lệnh sau:

```bash
bin/rails stimulus:manifest:update
```

Lệnh này sẽ tự động tạo code phù hợp trong `app/javascript/controllers/index.js`.

Bây giờ nếu chạy `yarn build`, bạn sẽ gặp một lỗi cuối cùng cần giải quyết:

```bash
✘ [ERROR] Could not resolve "@hotwired/stimulus"

    app/javascript/controllers/hello_controller.js:1:27:
      1 │ import { Controller } from "@hotwired/stimulus"
```

Code từ các Stimulus controller hiện đã được nạp đúng, nhưng nó tham chiếu đến stimulus, một package chúng ta chưa cài. Bạn có thể làm tương tự như với `turbo-rails` để giải quyết lỗi này.

```bash
yarn add @hotwired/stimulus
```

Giờ khi chạy `yarn run build`, chúng ta sẽ nhận được kết quả đúng! Bạn cũng sẽ có thể chạy `bin/dev` và thấy trang splash của Rails tại `http://localhost:3000`.

Giờ chúng ta đã đi qua cách cài gem jsbundling-rails trong một ứng dụng Rails được tạo bằng import map; hãy làm mọi thứ dễ dàng hơn một chút bằng cách thiết lập ngay từ đầu! Hãy nhập lệnh dưới đây.

```bash
rails new myapp -j <replace the text and <> with your bundler choice>
```

Hãy xem một ví dụ với esbuild. Nhập lệnh này trong terminal sẽ bắt đầu quá trình tạo ứng dụng Rails.

```bash
rails new bundling -j esbuild
```

Khi chạy app build, bạn sẽ thấy đầu ra khác với đầu ra trong bài học cài đặt Rails. Ở cuối, bạn sẽ thấy nội dung tương tự như sau.

```bash
info esbuild-windows-arm64@0.14.13: The CPU architecture "x64" is incompatible with this module.
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency.
info Direct dependencies
└─ @hotwired/stimulus@3.0.1
info All dependencies
└─ @hotwired/stimulus@3.0.1
```

Một điểm khác biệt quan trọng là trong đầu ra terminal, bạn sẽ thấy nhiều nội dung hơn, cùng với phần info từ esbuild khi lệnh này chạy. Hãy xem kết quả trong ứng dụng Rails của chúng ta.

File `package.json` là nơi lưu script build. Tại đây, bạn sẽ thấy lệnh Rails đã chạy khi tạo app. Khi mở `app/javascript/application.js`, bạn có thể xem những gì đã được import.

Một điểm cần lưu ý quan trọng với esbuild là trong `app/javascript/controllers/index.js`, bạn cần chạy một lệnh để thêm tất cả controller vào file `index.js`. Lệnh đó là

```bash
./bin/rails stimulus:manifest:update
```

Nhờ vậy, bạn có thể thêm các controller vào quá trình đóng gói và sử dụng chúng. Tuy nhiên, *hãy chắc chắn* chạy lệnh này; nếu không, sẽ không phải tất cả controller đều được đưa vào.

### Vì sao sử dụng đóng gói JS

Bạn có thể thắc mắc vì sao chúng ta cần đóng gói JS khi đã có import map. Một trong những điều *quan trọng nhất* cần nhớ là quyết định này phụ thuộc vào hoàn cảnh của dự án; dù import map loại bỏ nhu cầu đóng gói và transpile, bạn sẽ bỏ lỡ một số lợi ích của việc đóng gói JS.

Một số framework như React, vốn yêu cầu biên dịch JSX, không hoạt động với import map. Điều này dẫn đến một điểm quan trọng khác: hệ sinh thái JavaScript rất rộng, và việc đóng gói được đưa vào để giảm chi phí hiệu năng liên quan đến HTTP 1. Vì HTTP 1 giới hạn số request trên mỗi connection, việc gửi nhiều file gây ra chi phí hiệu năng.

Dù HTTP 2 đã giảm các chi phí này, nhiều package JS vẫn yêu cầu transpile hoặc đóng gói rõ ràng vì lý do hiệu năng hoặc yêu cầu của framework. Nếu dự định sử dụng những framework như vậy cho dự án, bạn có thể nên chọn đóng gói JS thay vì import map.

### Khi nào nên sử dụng đóng gói JS

Đến đây, bạn có thể tự hỏi import map còn cần thiết để làm gì. Có thể sử dụng toàn bộ hệ sinh thái JavaScript chắc hẳn có nghĩa là bạn hoàn toàn không cần import map, đúng không? Không hẳn. Mọi công cụ đều có ưu và nhược điểm.

Điều quan trọng cần nhớ là vào thời HTTP 1, việc tạo nhiều request *có* một hình phạt về hiệu năng. Kể từ khi HTTP 2 ra đời, hình phạt hiệu năng đó không còn nữa. Vì vậy, một trong những lợi ích chính của việc đóng gói — giảm chi phí liên quan đến HTTP 1 — đã trở nên kém quan trọng hơn.

Một nhược điểm khác của việc đóng gói JS là bất kỳ thay đổi nào trong một module cũng làm toàn bộ bundle hết hạn, buộc trình duyệt phải tải xuống và phân tích lại mọi thứ từ đầu.

Ngược lại, import map giữ các module tách biệt, nên bạn không phải tải lại từng module mỗi khi có thay đổi. Mức tăng hiệu năng này có thể khiến bạn cân nhắc dùng import map thay vì đóng gói JS.

Thông thường, câu trả lời cho những quyết định như vậy là không có *đáp án đúng*. Tất cả phụ thuộc vào điều bạn mong muốn ở dự án. Muốn dùng React với JSX và không ngại hiệu năng giảm? Hãy cân nhắc đóng gói JS. Có một số module muốn dùng nhưng không cần transpile hoặc đóng gói, đồng thời muốn tăng hiệu năng? Hãy cân nhắc import map.

Một điều cần nhấn mạnh là không có lựa chọn nào nhất thiết tệ hơn lựa chọn kia; điều đó phụ thuộc rất nhiều vào nhu cầu dự án. Đóng gói JS cho phép bạn tiếp cận nhiều framework như React với JSX, cũng như các tính năng nhất định như tree-shaking, trong khi import map không yêu cầu đóng gói và bạn không cần cập nhật mọi module khi chỉ cập nhật một module.

### Webpacker

Có thể bạn đã nghe về Webpacker hoặc từng bắt gặp nó trong thực tế, nên đáng nói qua vì nó từng được sử dụng trong khá nhiều năm cho Rails 6.

 Webpacker là một gem Ruby được tạo ra để hoạt động với chính webpack, cho phép Rails sử dụng ES6 (viết tắt của ECMAScript 6, được giới thiệu để chuẩn hóa JavaScript), vì khi đó ES6 cần được transpile để dùng được trên trình duyệt.

Trong ngữ cảnh Rails, Webpacker lập một bản đồ từ code JavaScript của bạn để xây dựng đồ thị phụ thuộc, nhờ đó tạo ra các bundle code. Rails sẽ lấy một pack từ `app/javascript/packs` và tải nó khi người dùng truy cập form đó.

Với gem Ruby, chúng ta dùng Gemfile; còn với Webpacker, chúng ta dùng package.json để quản lý các thư viện JavaScript. Điểm khác biệt chính là với Gemfile, bạn phải mở file, tự thêm gem và phiên bản rồi chạy bundle; còn với webpack, bạn có thể dùng Yarn từ terminal để thêm thư viện như sau.

```bash
yarn add bootstrap
```

Sau đó, bạn có thể tham chiếu đến thư viện trong file pack và sử dụng nó!

Dù có thể bắt gặp Webpacker trong thực tế, điều quan trọng cần lưu ý là Rails 7 không còn sử dụng nó nữa mà thay thế bằng jsbundling-rails. Nếu muốn dùng đóng gói, hãy chắc chắn chọn một trong các trình đóng gói đã liệt kê ở phần đầu bài học.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Nếu muốn tìm hiểu sâu về bất kỳ trình đóng gói JS nào, hãy xem nhanh tài liệu của chúng.
   - [Esbuild](https://esbuild.github.io/).
   - [Rollup](https://rollupjs.org/guide/en/#introduction).
   - [Webpack](https://webpack.js.org/concepts/).

</div>

### Tài nguyên bổ sung

Phần này có các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- Một bài viết đáng tham khảo về [vì sao đóng gói JS vẫn là một lựa chọn hợp lệ](https://world.hey.com/dhh/modern-web-apps-without-javascript-bundling-or-transpiling-a20f2755) của David Heinemeier Hansson.
- Nếu quan tâm đến sự khác biệt, đây là bản so sánh cấp cao giữa [jsbundling-rails và webpacker](https://github.com/rails/jsbundling-rails/blob/main/docs/comparison_with_webpacker.md).