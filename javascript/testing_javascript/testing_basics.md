### Giới thiệu

Test Driven Development, gọi tắt là TDD, là một khái niệm *rất* quan trọng trong bối cảnh phát triển hiện đại. Đây là khái niệm chúng ta đã giới thiệu từ khá sớm trong phần Fundamentals, qua các bài JavaScript Exercises. Ý tưởng chính là bạn bắt đầu làm việc với code bằng cách viết các automated test *trước* khi viết code được test. Cách làm này có rất nhiều lợi ích, tất cả sẽ được thảo luận trong các tài nguyên bên dưới.

Có nhiều hệ thống chạy test dành cho JavaScript: [Mocha](https://mochajs.org/), [Jasmine](https://jasmine.github.io/), [Tape](https://github.com/substack/tape) và [Jest](https://jestjs.io/), chỉ kể tên một vài hệ thống. May mắn là cú pháp của mỗi hệ thống khá giống nhau. Tất cả đều có tập hợp tính năng đặc biệt riêng, nhưng cú pháp cơ bản gần như giống hệt, nên cuối cùng việc bạn dùng hệ thống nào không quá quan trọng. Thực tế, việc chọn library nào cho curriculum này đã khá khó khăn!

Bài học này sẽ tập trung vào **Jest**. Chúng tôi nhận thấy Jest có một trong những bộ tài nguyên tốt nhất để giải thích JavaScript testing, cùng tài liệu tuyệt vời. Sau cùng, viết test không chỉ là vấn đề cú pháp mà còn là triết lý TDD. Những điều quan trọng nhất là biết **tại sao** chúng ta viết test và **test gì**, chứ không phải bằng cách nào.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Giải thích những điều cơ bản của TDD.
- Thiết lập và bắt đầu sử dụng Jest.
- Viết các test cơ bản.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc về [quy trình cơ bản và các lợi ích của TDD](https://web.archive.org/web/20211123190134/http://godswillokwara.com/index.php/2016/09/09/the-importance-of-test-driven-development/).
1. Xem ít nhất 3 video đầu tiên của [series video Unit Testing trong JavaScript](https://www.youtube.com/playlist?list=PL0zVEGEvSaeF_zoW9o66wa_UCNE3a7BEr).
1. Làm theo [tutorial Getting Started của Jest](https://jestjs.io/docs/getting-started) cho đến khi bạn đến phần "Additional Configuration". Jest cũng cung cấp các biến toàn cục như `test` và `expect`, nhưng nếu dùng ESLint, bạn sẽ cần import tường minh chúng trong các file test để tránh lỗi lint (xem phần "Using ESLint" trong chính tutorial đó).

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Dùng câu lệnh import ES6 với Jest

   Theo mặc định, phiên bản Jest hiện tại sẽ không nhận diện ESM, đó là lý do bạn thấy các hướng dẫn của họ dùng cú pháp CJS (ví dụ `module.exports`). Để có thể dùng ESM với Jest, bạn cần làm như sau:

   1. Cài đặt `@babel/preset-env` (tại thời điểm viết bài, Jest yêu cầu Babel v7 và chưa tương thích với v8 mới nhất):

      ```bash
      npm install --save-dev @babel/preset-env@^7
      ```

   1. Tạo file `babel.config.js` ở thư mục gốc của project với nội dung sau:

      ```javascript
      export default {
        presets: [["@babel/preset-env", { targets: { node: "current" } }]],
      };
      ```

   Điều này cho phép bạn viết cú pháp ESM `import`/`export` thay vì `require`/`module.exports`; bạn không cần thay đổi gì khác trong hướng dẫn, và Jest có thể chạy như bình thường. Ở phía sau, Babel sẽ chuyển ESM của bạn thành CJS trước khi chạy Jest (nó sẽ không ghi đè các file thực tế vì toàn bộ việc này diễn ra trong memory).

   </div>

1. Đọc và làm theo tài liệu [Using Matchers](https://jestjs.io/docs/using-matchers) trên website chính của Jest. Tài liệu này trình bày một số function hữu ích khác mà bạn có thể dùng trong test.
1. Bài viết này giải thích thêm về [lý do/cách thức và giá trị của TDD](https://web.archive.org/web/20260824053151/https://jrsinclair.com/articles/2016/one-weird-trick-that-will-change-the-way-you-code-forever-javascript-tdd/) và cũng bao gồm một số ví dụ tuyệt vời về cách áp dụng TDD.

</div>