### Giới thiệu

Làm tốt lắm khi đã hoàn thành Restaurant Page! Việc thiết lập và sử dụng Webpack cùng tất cả loader và plugin của nó có thể đã khiến bạn thấy hơi rườm rà, và có thể bạn vẫn tự hỏi tất cả những việc đó để làm gì. Khi tiếp tục học, nhiều phần trong số này sẽ trở nên tự nhiên hơn với bạn, và hy vọng những khái niệm nền tảng này sẽ giúp bạn xử lý các vấn đề có thể xuất hiện sau này!

Bây giờ bạn đã thử dùng Webpack một chút, hãy cùng xem một vài điều có thể giúp cải thiện trải nghiệm thiết lập và quy trình phát triển của bạn. Một số điều không chỉ giới hạn ở Webpack mà còn có thể tiếp tục được sử dụng với những công cụ khác khi bạn tiến bộ.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Cách viết và chạy npm scripts.
- Webpack mode là gì và cách tự động chuyển đổi giữa các mode khi cần.
- Cách tạo và sử dụng template repository.

### npm scripts

Mặc dù `npx webpack` và `npx webpack serve` không phải là những lệnh đặc biệt dài để gõ, bạn đã gặp `git subtree push --prefix dist origin gh-pages` trong hướng dẫn triển khai của dự án Restaurant Page. Chắc chắn bạn sẽ không muốn phải gõ lại lệnh đó mỗi khi cần sử dụng.

May mắn là chúng ta có thể viết npm scripts trong file `package.json` bằng cách thêm một thuộc tính `"scripts"` chứa một object các script. Script có thể được viết dưới dạng `"name": "command"` và được thực thi bằng cách chạy `npm run <name>` trong terminal. Ví dụ:

```json
{
  // ... other package.json stuff
  "scripts": {
    "build": "webpack",
    "dev": "webpack serve",
    "deploy": "git subtree push --prefix dist origin gh-pages"
  },
  // ... other package.json stuff
}
```

Ở đây chúng ta có ba npm script. Trong trường hợp này, chạy `npm run build` sẽ tương đương với chạy `npx webpack` (bạn có thể đã thấy lệnh này trong các hướng dẫn Webpack của bài học Webpack trước), `npm run dev` sẽ tương đương với `npx webpack serve`, còn `npm run deploy` sẽ chạy `git subtree push --prefix dist origin gh-pages` giúp chúng ta. Cách này không chỉ thường giúp tiết kiệm thời gian mà còn thuận tiện vì chúng ta có những tên hợp lý và phần nào được chuẩn hóa cho các lệnh. `npm run build` thường chứa lệnh của một công cụ để build/bundle/compile. `npm run dev` thường được dùng để khởi động dev server, v.v. Tên có thể không phải lúc nào cũng giống nhau, nhưng chúng có thể giải thích mục đích tốt hơn `npx webpack`.

Lưu ý rằng khi đặt các lệnh thành script, chúng ta bỏ `npx` ở đầu các lệnh `webpack` và `webpack serve`, vì chỉ cần `npx` khi chạy trực tiếp chúng trong terminal mà không thông qua npm scripts.

### Webpack modes

Cho đến giờ, chúng ta vẫn dùng Webpack ở development mode, vốn đương nhiên phù hợp nhất khi đang phát triển. Tuy nhiên, khi build dự án để triển khai, production mode chuyên dụng sẽ thực hiện một số tối ưu hóa khác cho chúng ta. Hãy thử xem! Bạn có thể mở dự án Restaurant Page, đổi mode thành `"production"` trong `webpack.config.js`, rồi chạy lại lệnh build. Hãy xem JavaScript bundle trong `dist` để thấy một mớ ký tự còn "lộng lẫy" hơn nữa!

Chúng ta *thực sự* không cần biết chính xác những tối ưu hóa nào đã được áp dụng, cũng không cần biết các chi tiết khác của production mode, nhưng hiểu rằng có hai mode tồn tại và được thiết kế cho những mục đích cụ thể là điều hữu ích.

Để bạn không phải chỉnh sửa thủ công file cấu hình mỗi khi muốn chuyển mode, chẳng hạn trước khi bundle vào `dist` hoặc trước khi quay lại dùng dev server, bạn có thể có hai file cấu hình khác nhau (ví dụ `webpack.dev.js` và `webpack.prod.js`), sau đó để các npm script build và dev chỉ rõ file cấu hình nào cần dùng (nếu bỏ tùy chọn `--config`, Webpack sẽ mặc định tìm `webpack.config.js`):

```json
"build": "webpack --config webpack.prod.js",
"dev": "webpack serve --config webpack.dev.js"
```

Trong bài tập, chúng ta sẽ giới thiệu một công cụ tên là `webpack-merge`, có thể giúp việc sử dụng nhiều file cấu hình Webpack trở nên dễ xử lý hơn và giảm tối đa việc lặp lại. Làm theo cách này rất thuận tiện. Chúng ta thiết lập một lần, rồi có thể quên việc đó vì mỗi script sẽ sử dụng đúng file cấu hình và mode phù hợp!

### Template repositories

Có thể bạn đã nhận thấy việc thiết lập Webpack bao gồm nhiều file, thư mục và khá nhiều cấu hình. Mỗi lần thiết lập một dự án mới với Webpack, bạn có thể phải xem lại những gì đã cấu hình trước đây để sao chép và dán phần cấu hình muốn dùng lại. Có thể bạn cũng đã nhận thấy mỗi khi tạo repository mới trên Github, có một tùy chọn tên là `Start with a template` trong phần `Configuration`.

Đây là lúc template repository thực sự hữu ích. Bất kỳ repository hiện có nào của bạn cũng có thể được chuyển thành template trong phần cài đặt (ngay bên dưới nơi bạn có thể đổi tên repository là một checkbox cho biết repository có phải template hay không). Nếu bạn chọn checkbox này, xin chúc mừng! Bạn đã làm xong mọi việc cần thiết. Bây giờ, khi tạo repository mới, menu thả xuống `Start with a template` sẽ liệt kê các template để bạn chọn. Chọn một template nghĩa là repository mới của bạn sẽ là bản sao của template đã chọn, chứ không phải một repository trống!

Có thể bạn chưa biết chắc mình sẽ muốn hoặc cần gì trong một template, nhưng khi nhận thấy mình đang dùng lại nhiều đoạn mã thiết lập cho nhiều dự án, bạn có thể tạo một repository mới với toàn bộ đoạn mã thiết lập cần thiết, đánh dấu nó là template, rồi cập nhật khi cần. Sau đó, khi tạo repository cho dự án mới, bạn có thể chọn template đó để tiết kiệm thời gian và sớm bắt tay vào làm chính dự án hơn!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [hướng dẫn "Production" của Webpack](https://webpack.js.org/guides/production/) để xem cách sử dụng `webpack-merge` và tách file cấu hình. Các ví dụ mã chính xác được dùng trong hướng dẫn đó tiếp nối một phần trước của một tutorial dài hơn, nhưng những phần chính về `webpack-merge` và tách file cấu hình vẫn có thể làm theo với một dự án mới; hoặc bạn thậm chí có thể thử chuyển dự án Restaurant Page của mình sang cách làm này!
   - Bạn có thể bỏ qua phần "Specify the Mode" và các ví dụ của phần đó.

</div>