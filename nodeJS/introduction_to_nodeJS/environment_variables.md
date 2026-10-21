### Giới thiệu

Khi chạy code, code của bạn chạy trong một môi trường cụ thể. Chạy code trên một máy khác ư? Đó là môi trường khác. Lưu trữ website ở nơi như Netlify hoặc Vercel ư? Đó cũng là môi trường khác với máy của bạn. Mỗi môi trường là một tập hợp công cụ và cấu hình riêng.

Chắc chắn bạn đã viết nhiều hàm và gọi chúng nhiều lần với các đối số khác nhau. Hàm có thể hoạt động khác hoặc trả về giá trị khác tùy vào các đối số đó. Tương tự tham số hàm, bản thân ứng dụng cũng có thể sử dụng **biến môi trường**. Khi chạy code, bạn có thể truyền giá trị vào các biến môi trường đó, và mỗi môi trường có thể nhận những giá trị khác nhau.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Mô tả biến môi trường là gì và lợi ích của chúng.
- Sử dụng tệp `.env` để lưu trữ và cung cấp biến môi trường cho code.
- Truy cập biến môi trường bằng đối tượng `process.env` của Node.

### Biến môi trường

Biến môi trường đơn giản là các biến có giá trị phụ thuộc vào môi trường. Vì chúng gắn với từng môi trường, ta có thể dùng chúng để:

- Cung cấp các giá trị khác nhau cho từng môi trường, chẳng hạn máy của bạn khi phát triển và máy chủ website khi triển khai, mà không phải sửa source code.
- Lưu trữ bí mật như URL và thông tin xác thực cơ sở dữ liệu hoặc API key.

Ví dụ, môi trường production có thể cần ghi log và phân tích bổ sung so với môi trường development. Vì vậy, source code có thể có một biến môi trường cho biết ứng dụng đang chạy ở chế độ "dev" hay "prod" để thực hiện các hành vi khác nhau. Đây là cách làm rất phổ biến; theo quy ước, biến môi trường này có tên `NODE_ENV`, dù dùng tên khác thì vẫn hoạt động.

Hoặc có thể bạn đang xây dựng một API kết nối với cơ sở dữ liệu nhưng muốn dùng cơ sở dữ liệu kiểm thử riêng trong quá trình phát triển thay vì cơ sở dữ liệu production. Khi phát triển cục bộ, bạn có thể truyền URL và thông tin xác thực của cơ sở dữ liệu kiểm thử vào app, còn bản triển khai sẽ nhận các giá trị của cơ sở dữ liệu production. Ngoài ra, bạn có thể lưu giá trị biến môi trường trong một tệp được thêm vào `.gitignore`, nhờ đó nội dung tệp không bị công khai khi đẩy các thay đổi.

### Nạp biến môi trường

Có nhiều cách nạp biến môi trường, dù một số cách rườm rà hơn hoặc chưa được hỗ trợ ổn định trong nhiều phiên bản Node. Một cách là định nghĩa biến môi trường và giá trị của chúng trực tiếp trong lệnh chạy code. Thay vì chỉ chạy app bằng `node index.js`, bạn có thể chạy lệnh sau (lưu ý rằng dấu ngoặc kép là tùy chọn với các giá trị không chứa những ký tự đặc biệt như dấu cách hoặc `=`):

```bash
NODE_ENV=prod VIDEO_URL="https://www.youtube.com/watch?v=X2CYWg9-2N0" node index.js
```

Trong ví dụ trên, ta định nghĩa các biến môi trường có tên `NODE_ENV` và `VIDEO_URL`, rồi gán cho chúng các giá trị tương ứng. Bất kỳ phần nào trong code sử dụng các biến đó sẽ nhận những giá trị này, giống như tham số hàm! Quy ước đặt tên biến môi trường là `UPPER_SNAKE_CASE` (đôi khi được gọi thân mật là `SCREAMING_SNAKE_CASE` hoặc `SHOUTY_CASE`).

Tuy nhiên, bạn sẽ thấy cách này nhanh chóng trở nên khá rườm rà, đặc biệt khi có nhiều biến môi trường. Nếu có dữ liệu nhạy cảm như thông tin xác thực cơ sở dữ liệu thì còn tệ hơn, vì bạn sẽ không muốn đẩy `package.json` nếu các giá trị đó nằm trong npm script!

#### export

Thay vì đặt biến môi trường trực tiếp trong lệnh chạy code, bạn có thể dùng lệnh shell `export`, lệnh này sẽ lưu biến môi trường và giá trị của chúng trong phiên shell hiện tại.

```bash
export NODE_ENV=prod VIDEO_URL="https://www.youtube.com/watch?v=X2CYWg9-2N0"
```

Lệnh trên chỉ đặt hai biến môi trường trong môi trường shell hiện tại. Nghĩa là nếu mở một shell mới, shell đó sẽ không truy cập được các biến này vì đó là một môi trường mới. Nếu chạy `node index.js` trong shell đầu tiên (shell đã đặt các biến môi trường), phần code sử dụng biến môi trường `NODE_ENV` sẽ nhận giá trị `"prod"`. Nếu chạy `node index.js` trong shell thứ hai, giá trị sẽ là `undefined` vì môi trường của nó chưa được đặt gì. Khá thú vị, phải không?

Cách này chắc chắn tốt hơn cách đặt biến trực tiếp trong lệnh trước đó và giúp ta giữ dữ liệu nhạy cảm kín hơn. Để ghi đè biến, chỉ cần chạy lại `export` với các giá trị mới như trên.

Để xem tất cả biến môi trường trong shell hiện tại, bạn có thể chạy `printenv`. Khoan đã, lệnh hiển thị *rất nhiều* thứ mà ta chưa từng tự đặt! Đó là vì bản thân shell đã có nhiều biến môi trường được đặt và nạp ngay khi khởi động. Hơi phiền nhỉ, ta chỉ muốn xử lý biến môi trường của app thôi! Ngoài ra, biến môi trường sẽ mất khi ta kết thúc shell. Nếu có nhiều biến thì sao? Việc cố nhớ tất cả rồi export lại trong shell mới sẽ thật sự là ác mộng. Vậy thì...

#### Tệp env của Node

Node (từ v24.10) có hỗ trợ ổn định được tích hợp sẵn cho các tệp chứa biến môi trường. Bạn có thể tạo một tệp có tên `.env` (theo quy ước) ở thư mục gốc của project; tệp sẽ chứa tất cả biến môi trường theo định dạng `NAME=VALUE`. Ví dụ:

```properties
# .env

NODE_ENV=prod
VIDEO_URL="https://www.youtube.com/watch?v=X2CYWg9-2N0"
```

**Bạn phải thêm tệp này vào tệp `.gitignore`** để giữ an toàn cho các bí mật và không công khai chúng!

Node cung cấp nhiều cách để nạp tệp, chẳng hạn dùng [tùy chọn CLI `--env-file`](https://nodejs.org/docs/latest-v24.x/api/cli.html#--env-filefile) (cho phép bạn chạy `node --env-file=.env index.js`, chẳng hạn) hoặc trực tiếp trong JavaScript bằng [method `process.loadEnvFile()`](https://nodejs.org/docs/latest-v24.x/api/process.html#processloadenvfilepath). Cả hai cách đều ném lỗi nếu thiếu tệp `.env`.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Biến môi trường và triển khai

Khi triển khai app sử dụng biến môi trường, repo của bạn sẽ không chứa tệp `.env`, vì vậy bạn phải tìm hiểu dịch vụ triển khai đã chọn xử lý việc đặt giá trị biến môi trường như thế nào. Thông thường sẽ có cách thực hiện qua giao diện website của dịch vụ, nhưng trong mọi trường hợp, hãy luôn kiểm tra tài liệu của họ!

Vì lý do này, bạn không nên nạp biến trong production giống như trong development, vì app có thể ném lỗi khi không tìm thấy tệp `.env`. Thay vào đó, bạn có thể ngăn điều này bằng nhiều cách, chẳng hạn thêm npm script cho production không dùng `--env-file`, dùng tùy chọn CLI `--env-file-if-exists`, hoặc xử lý lỗi bằng `process.loadEnvFile()`.

</div>

#### dotenv

Node không phải lúc nào cũng có sẵn hỗ trợ cho tệp biến môi trường. Bạn có thể sẽ bắt gặp các thư viện như [dotenv](https://www.npmjs.com/package/dotenv) được sử dụng trong thực tế. Nguyên tắc sẽ giống tính năng tích hợp sẵn, chỉ khác là cần làm theo hướng dẫn riêng của thư viện. Những thư viện này cũng có thể cung cấp các tính năng nâng cao hơn những gì Node tích hợp sẵn, có thể hữu ích cho các nhóm làm những project phức tạp hơn; nhưng trong phạm vi TOP, chúng không cần thiết.

Dù sao thì tuyệt! Giờ bạn đã có thể nạp biến môi trường, nhưng làm thế nào để thực sự truy cập chúng trong code?

### Truy cập biến môi trường

Có thể truy cập biến môi trường thông qua đối tượng tích hợp sẵn `process` của Node, cụ thể hơn là thuộc tính `env`. Node sẽ nạp từng biến môi trường vào đối tượng `process.env`, dùng tên biến làm tên thuộc tính. Sau đó bạn có thể truy cập chúng như bất kỳ thuộc tính đối tượng thông thường nào.

```javascript
if (process.env.NODE_ENV === "prod") {
    // do production-specific stuff
}

// don't want to ruin the surprise by hardcoding the URL!
// it might even change every few days!
redirectUserToSuperSecretVideo(process.env.VIDEO_URL);
```

Không cần hardcode các giá trị đó vào source code! Nếu muốn thay đổi giá trị biến môi trường, bạn chỉ cần sửa nó trong tệp `.env` rồi chạy lại chương trình. Cũng cần lưu ý rằng biến môi trường luôn là string, vì vậy bạn phải chuyển đổi nếu muốn dùng một biến làm number hoặc boolean chẳng hạn.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Ghi chép biến môi trường

Khi project có biến môi trường, những người hỗ trợ bạn sẽ cần biết project cần những biến nào để chạy. Vì vậy, chúng tôi thực sự khuyến nghị bạn ghi chép chúng trong tệp `README.md`, chẳng hạn biến nào bắt buộc và chúng phải chứa gì. Bạn cũng có thể cung cấp một tệp `.env` mẫu, chẳng hạn `.env.sample`, để người khác đổi tên rồi chỉnh sửa bằng các giá trị họ cần.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note lesson-note--critical" markdown="1">

#### Giữ an toàn cho các bí mật!

Biến môi trường không chỉ dành cho dữ liệu nhạy cảm, nhưng thường bạn sẽ cần chúng cho mục đích đó. Trong project, hãy chắc chắn thêm `.env` vào `.gitignore` để **không công khai tệp này**.

</div>

1. Xem [tài liệu của Node về biến môi trường](https://nodejs.org/docs/latest-v24.x/api/environment_variables.html) để biết thêm về cú pháp tệp và cách dùng CLI.

</div>