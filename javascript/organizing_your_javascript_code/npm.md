### Giới thiệu

Trong bài trước, chúng ta đã học về ES6 module và cú pháp dùng để import và export mọi thứ giữa các module. Khi xây dựng những ứng dụng lớn và phức tạp hơn, có thể chúng ta không muốn tự viết *mọi thứ*. Chúng ta có thể muốn import code của bên thứ ba để xử lý một số việc, từ các helper function do người khác viết cho đến toàn bộ framework để xây dựng ứng dụng. Để dễ dàng tìm và import các package bên thứ ba này, chúng ta cần sự trợ giúp của một package manager như npm!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Hiểu npm là gì.
- Hiểu mục đích của file `package.json`.

### npm

**npm** (không viết hoa!) là một package manager - một repository khổng lồ chứa plugin, library và các tool khác, đồng thời cung cấp cho chúng ta một command-line tool để cài đặt những tool này (chúng ta gọi chúng là "package") vào ứng dụng. Sau đó, code của mọi package đã cài sẽ có ở local để chúng ta import vào các file của mình. Chúng ta thậm chí còn có thể publish code của chính mình lên npm!

Bạn có thể nhớ đã cài npm trong khóa Foundations để cài framework kiểm thử Jest cho các bài tập JavaScript. Thật thú vị, [npm không phải viết tắt của "Node Package Manager"](https://www.npmjs.com/package/npm#is-npm-an-acronym-for-node-package-manager), dù bạn sẽ thường thấy nó được gọi như vậy.

Nếu bạn đang theo lộ trình Full Stack Ruby on Rails, bạn có thể đã được giới thiệu Yarn, một package manager JavaScript khác. Trong khóa học này, chúng ta sẽ dùng npm.

Khi ứng dụng trở nên phức tạp hơn và cần ngày càng nhiều file (dù là file của chúng ta hay file từ các package đã cài và import), việc quản lý nhiều dependency như vậy có thể trở nên khá rắc rối, đặc biệt khi package được cập nhật. Điều này còn rắc rối hơn khi xét đến việc chúng ta có thể phải gửi *rất nhiều* file JavaScript để trình duyệt tải xuống. Trong bài tiếp theo, chúng ta sẽ giới thiệu bundler, những tool cho phép chúng ta viết nhiều file dễ làm việc hơn, rồi bundle chúng thành ít file nhỏ hơn để cuối cùng gửi cho trình duyệt.

### package.json

npm xoay quanh một file có tên `package.json`. Đây là một file JSON chứa thông tin về project, chẳng hạn như tên hoặc các dependency và số version của chúng. npm có thể đọc file này và làm những việc như cài đặt mọi dependency được liệt kê với đúng version, cũng như chạy các command bạn đã thiết lập thành npm script (chúng ta sẽ học về npm script trong một bài sau).

Ví dụ, đây là file `package.json` của repo curriculum của The Odin Project, nơi chứa tất cả file bài học (bao gồm cả bài bạn đang học):

```json
{
  "name": "curriculum",
  "version": "1.0.0",
  "description": "[The Odin Project](https://www.theodinproject.com/) (TOP) is an open-source curriculum for learning full-stack web development. Our curriculum is divided into distinct courses, each covering the subject language in depth. Each course contains a listing of lessons interspersed with multiple projects. These projects give users the opportunity to practice what they are learning, thereby reinforcing and solidifying the theoretical knowledge learned in the lessons. Completed projects may then be included in the user's portfolio.",
  "scripts": {
    "lint": "markdownlint-cli2",
    "fix": "markdownlint-cli2 --fix"
  },
  "license": "CC BY-NC-SA 4.0",
  "devDependencies": {
    "markdownlint-cli2": "^0.12.1"
  }
}
```

Có khá nhiều thứ ở đây và hiện tại chúng ta chưa cần hiểu tất cả. Điểm chính là nếu bạn clone repo curriculum rồi chạy `npm install`, npm sẽ đọc file `package.json` này và thấy cần cài package `markdownlint-cli2`. Sau khi package này được cài, bạn có thể chạy một trong các script sử dụng package đó. Bản thân repo curriculum không thực sự chứa code của package `markdownlint-cli2`, vì bất kỳ ai clone repo cũng có thể chạy `npm install` để npm lấy code giúp họ.

Trong các project của mình, khi dùng npm để cài (hoặc gỡ cài đặt!) package mới, npm sẽ tự động cập nhật `package.json` với mọi thông tin mới. Chúng ta sẽ thấy điều này hoạt động trong bài tiếp theo khi giới thiệu việc bundle module bằng package có tên Webpack.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc thêm một chút về npm, package và dependency:
   1. [Cài đặt package bằng npm](https://docs.npmjs.com/downloading-and-installing-packages-locally).
   1. Đọc về [file `package.json`](https://docs.npmjs.com/creating-a-package-json-file), file lưu trữ phần lớn thông tin cho ứng dụng của chúng ta.
   1. Mọi package chúng ta cài đều được gọi là "dependency", nhưng nếu package chỉ được dùng trong quá trình phát triển và code của nó không cần cho app mà người dùng nhìn thấy (chẳng hạn framework kiểm thử Jest), chúng ta gọi chúng là [development dependency](https://dev.to/mshertzberg/demystifying-devdependencies-and-dependencies-5ege).
1. Đây là một [bài học lịch sử ngắn về JavaScript và việc quản lý package qua nhiều file](https://peterxjang.com/blog/modern-javascript-explained-for-dinosaurs.html). Chỉ đọc đến phần "Using a JavaScript module bundler (webpack)", vì chúng ta sẽ học về bundler và webpack trong bài tiếp theo.

</div>