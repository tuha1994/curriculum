### Giới thiệu

Trong bài học trước, chúng ta đã giới thiệu các module ES6 (ESM) và npm. Sự ra đời của ESM đã giảm đáng kể một số vấn đề liên quan đến việc quản lý từng file script và các dependency. Tuy nhiên, việc tải riêng lẻ một số lượng lớn file module có thể làm giảm hiệu năng, đặc biệt khi ngày càng có nhiều file của bên thứ ba được import.

May mắn là các công nghệ web mới hơn đã cải thiện đáng kể những khía cạnh này, nhưng bundler vẫn cung cấp cho chúng ta rất nhiều khả năng để xử lý và tối ưu hóa code theo nhiều cách. Tuy nhiên, khả năng đó đi kèm một chi phí nhỏ: chúng ta cần cấu hình bundler. Hiện tại, nhu cầu của chúng ta còn ít và đơn giản, nên có thể xem từng phần cơ bản một.

Hiểu biết về bundler và có trải nghiệm cơ bản với chúng là điều hữu ích. Mặc dù trong những năm gần đây đã xuất hiện các build tool mới có thể tự xử lý nhiều cấu hình cơ bản, trong thực tế bạn không phải lúc nào cũng có cơ hội sử dụng những công cụ mới mẻ đó. Việc cuối cùng làm việc với các codebase sử dụng công cụ đòi hỏi cấu hình thủ công là hoàn toàn có thể xảy ra. Ngay cả khi làm việc với các công cụ tự xử lý nhiều phần hơn, hiểu chúng thực sự đang làm gì cho bạn vẫn hữu ích.

Hãy đảm bảo bạn đang chạy phiên bản LTS mới nhất của Node.js trước khi tiếp tục. Nếu cần, bạn có thể dùng NVM để cài phiên bản LTS mới nhất theo bài học [Cài đặt Node.js](https://www.theodinproject.com/lessons/foundations-installing-node-js#installing-node).

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Giải thích mục đích của bundler và cách chúng hoạt động.
- Cấu hình Webpack để bundle các JavaScript module.
- Cấu hình Webpack để xử lý các file không phải JavaScript trong quá trình bundling, bao gồm sử dụng HtmlWebpackPlugin.
- Thiết lập development server của Webpack.

### Bundling

Trong bài học về ES6 module, chúng ta đã học **entry point** là gì, **dependency graph** là gì và cách thêm một file entry point vào HTML dưới dạng module script. Với bundling, các khái niệm về entry point và dependency graph vẫn được áp dụng: chúng ta cung cấp cho bundler một entry point. Sau đó, bundler xây dựng dependency graph từ file đó, kết hợp tất cả các file liên quan lại với nhau rồi xuất ra một file duy nhất chứa toàn bộ code cần thiết.

Trong quá trình này, chúng ta cũng có thể yêu cầu nó làm nhiều việc khác, chẳng hạn như [minify code](https://en.wikipedia.org/wiki/Minification_(programming)), tối ưu hóa hình ảnh, hoặc thậm chí ["tree shaking"](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking). Phần lớn các tối ưu hóa bổ sung này nằm ngoài phạm vi của khóa học; thay vào đó, chúng ta sẽ tập trung vào bundling JavaScript cơ bản và xử lý HTML, CSS cùng hình ảnh.

### Webpack

Webpack là một trong những JavaScript bundler phổ biến nhất, nếu không muốn nói là phổ biến nhất, và đã như vậy trong một thời gian dài. Hãy bắt đầu bundling!

Trước hết, chúng ta cần tạo một thư mục mới cho practice app, rồi tạo một file `package.json` trong đó để npm ghi lại thông tin về các package chúng ta sử dụng (chẳng hạn Webpack). Chạy lệnh sau trong terminal:

```bash
mkdir webpack-practice &&
cd webpack-practice &&
npm init -y --init-type=module
```

Lệnh này sẽ tạo file `package.json` bên trong thư mục `webpack-practice` mà bạn vừa tạo và chuyển vào. Bây giờ chúng ta có thể cài đặt Webpack:

```bash
npm install --save-dev webpack webpack-cli
```

<div class="lesson-note" markdown="1">

#### Lỗ hổng npm audit

Sau khi cài đặt một package bằng npm, output có thể nhắc đến một số lỗ hổng và việc chạy `npm audit fix`. Mặc dù về mặt kỹ thuật đây là các lỗ hổng, rất có khả năng chúng là những vấn đề cực kỳ đặc thù và trong phạm vi TOP thì hầu như không gây nguy hiểm thực tế nào cho những việc bạn làm trong curriculum, nên bạn có thể bỏ qua nếu gặp chúng.

Những vấn đề này phù hợp hơn với các ứng dụng production có mức độ rủi ro cao hơn, nơi bạn cần kiểm tra chính các lỗ hổng để xem liệu chúng có thể gây hại thực tế hay không.

</div>

Lưu ý rằng chúng ta đã thêm cờ `--save-dev` (bạn cũng có thể dùng `-D` làm dạng viết tắt), cờ này yêu cầu npm ghi hai package của chúng ta dưới dạng development dependencies. Chúng ta chỉ sử dụng Webpack trong quá trình phát triển. Code thực sự giúp Webpack chạy sẽ không phải là một phần của code mà browser chạy.

Ngoài ra, hãy chú ý rằng sau khi cài đặt xong, npm đã tạo thư mục `node_modules` và file `package-lock.json` cho chúng ta. `node_modules` là nơi chứa code thực tế của Webpack (cùng rất nhiều thứ khác), còn `package-lock.json` chỉ là một file khác mà npm sử dụng để theo dõi thông tin package cụ thể hơn.

### src và dist

Khi làm việc với Webpack (và thường là với bất kỳ bundler hoặc build tool nào khác), chúng ta có hai thư mục rất quan trọng: `src` (viết tắt của "source") và `dist` (viết tắt của "distribution"). Về mặt kỹ thuật, chúng ta có thể gọi các thư mục này bằng bất kỳ tên nào, nhưng đây là những tên theo quy ước.

`src` là nơi chúng ta lưu toàn bộ source code của website, về cơ bản là nơi thực hiện mọi công việc (ngoại lệ là việc chỉnh sửa các file cấu hình ở thư mục gốc của project). Khi chạy Webpack để bundle code, nó sẽ xuất các file đã bundle vào thư mục `dist`. Ý tưởng là nếu ai đó fork hoặc clone project, họ không cần thư mục `dist`, vì họ có thể tự chạy Webpack để build từ `src` vào `dist` của mình. Tương tự, để deploy website, chúng ta chỉ cần code trong `dist` và không cần gì khác. Hãy ghi nhớ điều này! Làm việc trong `src`, build vào `dist`, rồi deploy từ đó!

### Bundling JavaScript

<div class="lesson-note" markdown="1">

#### Phần mở rộng file khi import

Thông thường, với ESM, chúng ta cần chỉ rõ phần mở rộng file khi import từ các file khác (ví dụ `"./greeting.js"`). Với Webpack và nhiều bundler khác, một số phần mở rộng file như `.js` là tùy chọn, vì theo mặc định Webpack sẽ tự động kiểm tra các đường dẫn file không có phần mở rộng để tìm file `.js` (ví dụ `"./greeting"`).

Đây là một tính năng của Webpack, không phải ESM.

Trong bài học này, các ví dụ có import sẽ luôn bao gồm phần mở rộng file khi phù hợp để rõ ràng hơn.

</div>

Bây giờ chúng ta đã cài Webpack trong thư mục project, hãy tạo một thư mục `src` với hai file JavaScript bên trong: `index.js` và `greeting.js`.

```bash
mkdir src && touch src/index.js src/greeting.js
```

Bên trong hai file JavaScript, chúng ta sẽ có nội dung sau:

```javascript
// index.js
import { greeting } from "./greeting.js";

console.log(greeting);
```

```javascript
// greeting.js
export const greeting = "Hello, Odinite!";
```

Tốt lắm, bây giờ chúng ta có `index.js` import từ và do đó phụ thuộc vào `greeting.js`. Để bundle phần này, chúng ta cũng cần một file cấu hình Webpack, trong đó chứa mọi chi tiết cần thiết cho việc bundling, chẳng hạn entry point, nơi output, cùng các plugin và loader (chúng ta sẽ tìm hiểu ngay sau đây).

Quay lại thư mục gốc của project (tức là bên ngoài `src`), tạo file `webpack.config.js` chứa nội dung sau:

```javascript
// webpack.config.js
import path from "node:path";

export default {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(import.meta.dirname, "dist"),
    clean: true,
  },
};
```

Bạn sẽ thấy object được export có một vài phần chính:

- `mode`: Hiện tại, chúng ta chỉ để ở development mode vì mode này sẽ hữu ích hơn cho chúng ta. Chúng ta sẽ quay lại với mode này và production mode trong một bài học sau.
- `entry`: Đường dẫn file từ file config đến file được chọn làm entry point, trong trường hợp này là `src/index.js`.
- `output`: Một object chứa thông tin về bundle output.
  - `filename`: Tên của bundle output - bạn có thể đặt tùy ý.
  - `path`: Đường dẫn đến thư mục output, trong trường hợp này là `dist`. Nếu thư mục này chưa tồn tại khi chúng ta chạy Webpack, Webpack cũng sẽ tự động tạo nó. Đừng quá lo lắng về lý do có phần `path.resolve` - đây chỉ là cách Webpack khuyến nghị để chỉ rõ thư mục output.
  - `clean`: Nếu thêm tùy chọn này và đặt thành `true`, mỗi lần chạy Webpack để bundle, nó sẽ xóa thư mục output trước rồi mới bundle các file vào đó. Điều này giúp giữ `dist` sạch, để thư mục chỉ chứa các file được tạo bởi lần bundling gần nhất.
  
Khi đã có đủ các file này, hãy chạy Webpack và xem điều gì xảy ra!

```bash
npx webpack
```

Bạn sẽ thấy Webpack đã tạo thư mục `dist` cho chúng ta, bên trong có file `main.js`! Bên trong file này là... rất nhiều thứ... Đừng lo, phần lớn chỉ dành cho các công cụ phát triển mà chúng ta sẽ sử dụng sau. Nếu chạy file này bằng `node dist/main.js`, bạn sẽ thấy `Hello, Odinite!` được log trong terminal.

Chúc mừng! Bạn vừa tạo bundle đầu tiên bằng Webpack!

### Xử lý HTML

Hãy thực sự thêm HTML vào. Suy cho cùng, chúng ta quan tâm đến việc tạo website! Vì HTML không phải JavaScript, Webpack không thể bundle trực tiếp ngay lập tức, nhưng chúng ta có một công cụ tiện lợi tên là `HtmlWebpackPlugin` rất phù hợp cho việc này.

Chạy lệnh sau để cài đặt HtmlWebpackPlugin (cũng dưới dạng dev dependency):

```bash
npm install --save-dev html-webpack-plugin
```

Chúng ta cũng nên tạo một file `template.html` bên trong `src` (bạn có thể đặt tên file tùy ý) và điền vào đó boilerplate HTML thông thường. **Chúng ta không cần đặt thẻ script trong file này!** HtmlWebpackPlugin sẽ tự động thêm bundle output của chúng ta dưới dạng thẻ script. Chúng ta không muốn vô tình thêm trùng một thẻ nữa! Trong `webpack.config.js`, chúng ta có thể thêm một vài phần nhỏ.

```javascript
// webpack.config.js
import path from "node:path";
import HtmlWebpackPlugin from "html-webpack-plugin";

export default {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(import.meta.dirname, "dist"),
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html",
    }),
  ],
};
```

Tất cả những gì chúng ta làm ở đây là đảm bảo cấu hình Webpack có quyền truy cập vào HtmlWebpackPlugin, rồi thêm nó làm plugin cho config object. Trong lời gọi constructor của HtmlWebpackPlugin, chúng ta truyền vào các option. Hiện tại, chúng ta chỉ quan tâm đến option `template`.

Nếu cung cấp đường dẫn đến file `src/template.html` làm template, khi chạy lại `npx webpack`, bạn sẽ thấy thư mục `dist` không chỉ chứa file `main.js` mà còn có file `index.html` (Webpack không thể gộp chúng thành một file). Bạn cũng sẽ thấy HtmlWebpackPlugin tự động thêm một thẻ script có thuộc tính defer vào file `index.html` - thật đáng yêu! Nếu mở file này trong browser và kiểm tra browser console, bạn sẽ thấy chuỗi `"Hello, Odinite!"` đáng yêu của chúng ta được log.

Chúng ta đã cấu hình thành công Webpack để xử lý file HTML và chèn thẻ script phù hợp. Với mọi thay đổi đối với HTML, chúng ta chỉ cần chạy lại Webpack để tạo code `dist` mới.

Hãy xem chúng ta sẽ xử lý CSS như thế nào.

### Load CSS

Chúng ta không chỉ cần một package mới cho CSS mà cần *hai* package. Ôi, đúng là một thứ tham lam... Hãy cài đặt chúng.

```bash
npm install --save-dev style-loader css-loader
```

`css-loader` sẽ đọc mọi file CSS mà chúng ta import trong một file JavaScript và lưu kết quả vào một string. Sau đó, `style-loader` lấy string đó và thực sự thêm code JavaScript để áp dụng các style đó vào page. Vì vậy, chúng ta cần cả hai.

Quay lại `webpack.config.js`, chúng ta cần thêm các loader này để Webpack biết phải làm gì. Vì đây không phải plugin nên chúng được đặt trong một phần riêng:

```javascript
// webpack.config.js
import path from "node:path";
import HtmlWebpackPlugin from "html-webpack-plugin";

export default {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(import.meta.dirname, "dist"),
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

Tất cả những gì đoạn này làm là báo cho Webpack rằng nếu gặp một file được import có phần mở rộng `.css`, nó cần dùng các loader được liệt kê để xử lý file CSS đó.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Thứ tự loader quan trọng với CSS!

Hãy chú ý chúng ta đặt `css-loader` **ở cuối** array. Chúng ta **phải** đặt theo thứ tự này, không được đảo ngược.

Webpack sẽ chạy loader bắt đầu từ cuối, vì vậy trước tiên chúng ta muốn nó đọc file CSS thành một string bằng `css-loader`, sau đó dùng `style-loader` để inject JavaScript áp dụng CSS trong string đó vào page. Làm theo thứ tự ngược lại sẽ không hoạt động giống như vậy.

</div>

Bây giờ Webpack đã biết phải làm gì với các file CSS được import, hãy thêm CSS! Tạo `src/styles.css` với nội dung sau:

```css
/* styles.css */
body {
  background-color: rebeccapurple;
}
```

Bây giờ bạn có thể import file CSS vào một trong các file JavaScript. `src/index.js` là lựa chọn hợp lý. Chúng ta không cần gì từ chính file CSS được import. Vì CSS và style loader sẽ xử lý tất cả việc đó, chúng ta chỉ cần dùng một [side effect import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#import_a_module_for_its_side_effects_only).

```javascript
// src/index.js
import "./styles.css";
import { greeting } from "./greeting.js";

console.log(greeting);
```

Một lần nữa, hãy bundle bằng Webpack với `npx webpack`, rồi mở `dist/index.html` và tận hưởng màn hình tím tuyệt đẹp!

#### Còn thẻ link thì sao?

Hãy chú ý rằng chúng ta không link file CSS trong HTML template như cách trước đây. Mặc dù bạn có thể làm vậy bằng một trong các loader ở phần tiếp theo, trong thực tế các project thường có nhiều thành phần chuyển động và nhiều module. Cuối cùng, việc làm việc với nhiều file CSS nhỏ hơn, được import trong các module nơi chúng cần thiết, sẽ trở nên dễ dàng hơn. Thậm chí có những cách để các file đó chỉ được áp dụng trong những module tương ứng thay vì áp dụng toàn cục!

Chúng ta chỉ giới thiệu phần tối thiểu để bạn có thể import CSS vào JavaScript, nhưng nhiều build tool và cấu hình bundler phức tạp hơn sẽ làm được nhiều việc hơn với CSS được import so với những gì chúng ta trình bày ở đây.

### Load hình ảnh

Chúng ta gần hoàn tất cấu hình Webpack chính! Nếu muốn đưa các file hình ảnh cục bộ vào website, chúng cũng cần thêm một chút cấu hình vì chúng không phải file JavaScript.

Có ba cách khác nhau để xử lý các file hình ảnh cục bộ:

1. **File hình ảnh được dùng trong CSS bên trong `url()`**

   May mắn cho chúng ta! `css-loader` đã tự xử lý việc này, nên không cần làm gì thêm với đường dẫn hình ảnh trong CSS!

1. **File hình ảnh được tham chiếu trong HTML template, ví dụ như `src` của một `<img>`**

   Chúng ta cần cài đặt và yêu cầu Webpack sử dụng một thứ gọi là `html-loader`, loader này sẽ phát hiện đường dẫn file hình ảnh trong HTML template và load đúng file hình ảnh cho chúng ta. Nếu không có nó, một `src` như `./odin.png` chỉ là một đoạn text và sẽ không còn tham chiếu đúng file sau khi Webpack build vào `dist`. Hãy cài đặt nó:

   ```bash
   npm install --save-dev html-loader
   ```

   Sau đó, thêm object sau vào array `module.rules` trong `webpack.config.js`:

   ```javascript
   // webpack.config.js
   {
     test: /\.html$/i,
     use: ["html-loader"],
   }
   ```

1. **Hình ảnh được dùng trong JavaScript, khi chúng ta cần import file**

   Nếu cần dùng một file hình ảnh cục bộ trong JavaScript (chẳng hạn khi thao tác DOM để tạo hoặc chỉnh sửa các phần tử `img` và đặt thuộc tính `src`), chúng ta cần import hình ảnh vào JavaScript module. Vì hình ảnh không phải JavaScript, chúng ta cần báo cho Webpack biết các file này là asset bằng cách thêm rule `asset/resource`. Không cần cài thêm gì ở đây. Chỉ cần thêm object sau vào array `module.rules` trong `webpack.config.js`:

   ```javascript
   // webpack.config.js
   {
     test: /\.(png|svg|jpg|jpeg|gif)$/i,
     type: "asset/resource",
   }
   ```

   Bạn luôn có thể chỉnh sửa regex trong thuộc tính `test` để bỏ những phần mở rộng file không cần hoặc thêm những phần mở rộng cần dùng. Nội dung ở trên được lấy nguyên từ [hướng dẫn Asset Management của Webpack](https://webpack.js.org/guides/asset-management/#loading-images) và sẽ nhận diện hầu hết các phần mở rộng file hình ảnh phổ biến.

   Sau đó, trong bất kỳ JavaScript module nào muốn sử dụng hình ảnh, chúng ta chỉ cần default import nó.

   ```javascript
   // src/index.js
   import odinImage from "./odin.png";

   const image = document.createElement("img");
   image.src = odinImage;

   document.body.appendChild(image);
   ```

   Chúng ta phải import nó để biến `odinImage` chứa đúng đường dẫn file, ngay cả khi bundle vào `dist`. Nếu chỉ viết `image.src = "./odin.png";`, thì "đường dẫn file" chỉ là một string thông thường. Khi bundle vào `dist`, Webpack sẽ không tự động nhận ra string này trong JavaScript đang tham chiếu đến một file và do đó sẽ không đưa file đó vào bundle. Khi import hình ảnh và đặt rule `asset/resource` phù hợp, Webpack sẽ nhận ra import, đưa file hình ảnh vào khi bundle, đồng thời đảm bảo biến được import chứa đúng đường dẫn file cuối cùng.

Sau tất cả những việc đó, nếu thêm cả `html-loader` và rule hình ảnh `asset/resource`, `webpack.config.js` sẽ trông gần như sau:

```javascript
// webpack.config.js
import path from "node:path";
import HtmlWebpackPlugin from "html-webpack-plugin";

export default {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(import.meta.dirname, "dist"),
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.html$/i,
        use: ["html-loader"],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: "asset/resource",
      },
    ],
  },
};
```

Có thể bạn sẽ nhận thấy khi hình ảnh được đưa vào trong quá trình bundling, file hình ảnh output trong `dist` có tên file khác (rất có thể là một mớ số và chữ). Theo mặc định, Webpack đặt tên mới cho các file hình ảnh đã bundle bằng cách hash nội dung của chúng. Bạn không cần biết cách này hoạt động, cũng không cần đào sâu vào chi tiết vì sao hay cách thay đổi nó. Bạn chỉ cần biết đây là hành vi được mong đợi (mục đích là ngăn các vấn đề với browser cache và việc khớp tên file).

<div class="lesson-note lesson-note--tip" markdown="1">

#### Chỉ cấu hình những gì bạn cần

Có thể bạn không cần mọi thứ chúng ta đã đề cập. Nếu project không có hình ảnh cục bộ được tham chiếu trong thuộc tính file path của HTML template, bạn không cần thiết lập `html-loader`. Nếu không sử dụng hình ảnh cục bộ nào trong JavaScript, bạn không cần thiết lập rule hình ảnh `asset/resource`.

Tương tự, trong tương lai, bạn có thể làm việc với những thứ cần loader hoặc plugin đặc biệt, chẳng hạn custom font hoặc preprocessor. Khi đến lúc đó, bạn luôn có thể dùng Google hoặc tham khảo tài liệu Webpack để biết mình cần làm gì.

</div>

### Webpack dev server

Trong bài học này, bạn có thấy hơi khó chịu vì phải chạy `npx webpack` để bundle lại sau mỗi thay đổi không? May mắn là có nhiều giải pháp cho việc này, và chúng ta sẽ tập trung vào lựa chọn mà chúng tôi cho là hữu ích nhất: `webpack-dev-server`. Cài đặt nó như sau:

```bash
npm install --save-dev webpack-dev-server
```

Có thể trước đây bạn đã dùng một thứ như extension Live Preview của VSCode, extension này tự động refresh trang web mỗi khi bạn lưu thay đổi. `webpack-dev-server` rất tương tự, nghĩa là chúng ta không phải tiếp tục chạy `npx webpack` sau mỗi thay đổi.

Nó hoạt động bằng cách bundle code ở phía sau (như thể chúng ta chạy `npx webpack`, nhưng không lưu file vào `dist`) và thực hiện việc này mỗi khi bạn lưu một file được sử dụng trong bundle. Chúng ta cũng có thể dùng một thứ gọi là **source map** để mọi thông báo lỗi tham chiếu đến file và dòng trong code phát triển, thay vì mớ hỗn độn bên trong file `.js` bundle duy nhất!

Sau khi cài đặt, trong `webpack.config.js`, chúng ta chỉ cần thêm một vài thuộc tính nữa ở đâu đó trong config object (thứ tự các thuộc tính này không quan trọng):

```javascript
// webpack.config.js
import path from "node:path";
import HtmlWebpackPlugin from "html-webpack-plugin";

export default {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(import.meta.dirname, "dist"),
    clean: true,
  },
  devtool: "eval-source-map",
  devServer: {
    watchFiles: ["./src/template.html"],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.html$/i,
        use: ["html-loader"],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: "asset/resource",
      },
    ],
  },
};
```

Trước hết, chúng ta thêm một [source map](https://webpack.js.org/configuration/devtool/) bằng cách đặt `eval-source-map` làm option `devtool`. Nếu không làm vậy, mọi thông báo lỗi nhận được không nhất thiết khớp với đúng file và số dòng trong code phát triển. Trong tab "Sources" của devtools, chúng ta cũng sẽ không tìm thấy code gốc chưa bị thay đổi, khiến Chrome debugger khó sử dụng hơn. Thêm source map sẽ giải quyết cả hai vấn đề này cho chúng ta.

Thứ hai, theo mặc định, `webpack-dev-server` chỉ tự động khởi động lại khi phát hiện thay đổi trong các file được import vào JavaScript bundle, nên HTML template sẽ bị bỏ qua! Tất cả những gì cần làm là thêm nó vào array các file được dev server theo dõi - thật đơn giản!

Sau khi thiết lập, chúng ta có thể khởi động dev server bằng lệnh sau:

```bash
npx webpack serve
```

Website của chúng ta sau đó sẽ mặc định có tại [http://localhost:8080/](http://localhost:8080/).

<div class="lesson-note" markdown="1">

#### Khởi động lại dev server khi thay đổi config

Lưu ý rằng webpack-dev-server chỉ đọc cấu hình webpack khi bạn khởi động nó. Nếu thay đổi file webpack config trong lúc dev server đang chạy, nó sẽ không phản ánh những thay đổi cấu hình đó. Dùng <kbd>Ctrl</kbd> + <kbd>C</kbd> trong terminal để dừng nó, rồi chạy lại `npx webpack serve` để áp dụng config mới.

</div>

### Tổng kết

Đúng vậy, tất cả những điều này có thể trông khá nhiều. Bạn đã đi từ việc chỉ có các file HTML, CSS và JS cơ bản, không cần nhiều thứ khác, đến việc đột nhiên cần loader này, plugin kia, file config nọ, v.v. Trong thực tế, khi ứng dụng trở nên phức tạp hơn, chúng ta cần các công cụ có thể cải thiện trải nghiệm phát triển đồng thời tối ưu hóa mọi thứ trong production. Mặc dù hiện tại chúng ta chưa dùng hết các tính năng có sẵn, hiểu khái quát những công cụ dạng này đang làm gì cho chúng ta vẫn rất hữu ích. Ở các phần sau của curriculum, bạn sẽ sử dụng những công cụ khác giúp trừu tượng hóa nhiều cơ chế nền tảng này. Sử dụng chúng mà không biết chúng thực sự đang làm gì có thể khiến mọi việc khó hơn khi sau này bạn gặp một tình huống thực sự cần một dạng cấu hình thủ công nào đó.

Trong một bài học sau, chúng ta sẽ giới thiệu thêm một số thứ có thể giúp việc thiết lập và làm việc với Webpack nhanh hơn, đơn giản hơn nhiều. Tuy nhiên, hiện tại, thực hành thủ công một chút vẫn là điều tốt.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bắt đầu bằng cách đọc trang [Webpack concepts](https://webpack.js.org/concepts/) để có hiểu biết khái quát về một số thuật ngữ chính.
1. [Hướng dẫn Asset Management của Webpack](https://webpack.js.org/guides/asset-management/) trình bày một số ví dụ về cách xử lý nhiều loại asset khác nhau, như CSS, hình ảnh và font. Bạn sẽ thấy các ví dụ sử dụng `npm run build` để bundle file; trong các ví dụ đó, lệnh này tương đương với `npx webpack`. Chúng ta sẽ tìm hiểu npm scripts trong một bài học sau.

</div>