### Giới thiệu

Khi mã bạn đang kiểm thử phải tương tác với cơ sở dữ liệu, phần thiết lập cần thực hiện sẽ phức tạp hơn khá nhiều. Rõ ràng bạn không muốn chạy mã kiểm thử trên cơ sở dữ liệu production vì có nguy cơ làm lộ hoặc ảnh hưởng đến dữ liệu của người dùng. Trong bài học này, chúng ta sẽ tìm hiểu cách xử lý cơ sở dữ liệu trong môi trường kiểm thử unit và integration.

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về những chủ đề bạn sẽ học trong bài học này.

- Tìm hiểu các bài kiểm thử unit và integration trong bối cảnh một server Express.
- Tạo và sử dụng một cơ sở dữ liệu riêng cho kiểm thử integration.

### Unit test - bạn có thực sự cần chúng không?

Trước khi đi sâu hơn, bạn có thể cân nhắc xem các thao tác cơ sở dữ liệu đang kiểm thử có thực sự cần được kiểm thử ngay từ đầu hay không. Nếu bạn đọc và ghi trực tiếp từ cơ sở dữ liệu bằng `pg` hoặc một module cơ sở dữ liệu khác, có thể bạn không thực sự cần kiểm thử đoạn mã đó. `pg` (và có lẽ mọi module cơ sở dữ liệu phổ biến khác) vốn đã có rất nhiều bài kiểm thử cho tất cả hành động của nó, vì vậy nếu bạn chỉ đang phục vụ một JSON API và tất cả những gì bạn làm là tận dụng các hàm từ một module khác thì những thao tác đó đã được bao phủ rồi.

Nếu các truy vấn của bạn phức tạp, bạn có thể biện minh cho việc thêm unit test để đảm bảo mình đang sử dụng chúng đúng cách và đoạn mã đã viết thực hiện đúng điều dự định. Nếu bạn dùng mã của riêng mình để lọc, sắp xếp hoặc thao tác dữ liệu theo cách khác, bạn cũng sẽ muốn kiểm thử những phần đó. Tuy nhiên, với mã của riêng mình, tốt hơn hết là bạn nên tách các phần đó ra thành những module riêng, độc lập với các thao tác cơ sở dữ liệu, để có thể kiểm thử chúng mà không phải động vào cơ sở dữ liệu.

### Integration test

Tuy nhiên, có những trường hợp bạn sẽ muốn kiểm thử các phần có tương tác với cơ sở dữ liệu. Ví dụ, chúng ta đã thực hiện một integration test trên server trong [bài Testing Routes and Controllers](https://www.theodinproject.com/lessons/nodejs-testing-routes-and-controllers) bằng `supertest`.

Hãy tạo một cơ sở dữ liệu riêng cho việc kiểm thử; đặt tiền tố `test_` để dễ nhận diện. Bạn có thể chạy một prisma migrate bằng cách thay URL cơ sở dữ liệu kiểm thử. Sau đó, hãy dùng một seed script trên instance cơ sở dữ liệu này, hoặc bạn có thể chèn dữ liệu thủ công trong hàm `beforeAll` của test suite.

Chúng ta sẽ tận dụng biến môi trường để codebase xác định cơ sở dữ liệu nào cần sử dụng. Trước tiên, thêm cơ sở dữ liệu kiểm thử vào file `.env`:

```properties
NODE_ENV=development
DATABASE_URL=postgresql://<user>:<password>@localhost:5432/inventory_application
TEST_DATABASE_URL=postgresql://<user>:<password>@localhost:5432/test_inventory_application
```

Tiếp theo, thiết lập các npm script phù hợp trong file `package.json`:

```json
{
  // other stuff
  "scripts": {
    "dev": "node app.js",
    "test": "jest"
  },
  // even more stuff
}
```

Để nạp các biến môi trường, bạn có thể dùng phương pháp mình thích cho `app.js`, nhưng sẽ không thể dùng `--env-file` hoặc `--env-file-if-exists` với Jest vì bạn không trực tiếp gọi `node`. Trong trường hợp này, bạn sẽ phải dùng một cách như `process.loadEnvFile()` để nạp các biến môi trường bằng chương trình vào môi trường kiểm thử. Chẳng hạn, bạn có thể làm việc này trong một [file global setup của Jest](https://jestjs.io/docs/configuration#globalsetup-string). Ngoài ra, [Jest mặc định đặt `NODE_ENV` thành `'test'`](https://jestjs.io/docs/environment-variables#node_env), giá trị này sẽ ghi đè file `.env` bằng `NODE_ENV=development`, vì vậy bạn không cần làm gì cầu kỳ ở đây.

Dựa trên `NODE_ENV`, bạn có thể chuyển đổi URL cơ sở dữ liệu bằng chương trình:

```javascript
const connectionString = process.env.NODE_ENV === 'test'
  ? process.env.TEST_DATABASE_URL
  : process.env.DATABASE_URL;

const adapter = new PrismaPg({ connectionString });
const prisma = new PrismaClient({ adapter });
```

### Reset cơ sở dữ liệu giữa các test

Khi kiểm thử các thao tác cơ sở dữ liệu, chúng ta luôn phải cô lập các test với nhau để không test nào phụ thuộc vào test khác. Ví dụ, chúng ta không muốn một test thất bại chỉ vì trong một test khác mình đã không thêm hoặc xóa một dòng trong bảng, ngay cả khi hành vi chính đang được kiểm thử thực sự hoạt động. Điều này có nghĩa là khi tương tác với cơ sở dữ liệu trong một test (đọc, tạo, cập nhật hoặc xóa bản ghi), lý tưởng nhất là chúng ta nên dọn dẹp bằng cách reset cơ sở dữ liệu về trạng thái ban đầu trước test tiếp theo.

Bạn có thể xử lý việc này một cách thuận tiện bằng một hàm như `beforeEach`. Ví dụ, nếu có một cơ sở dữ liệu với users và projects, ta có thể viết như sau:

```javascript
beforeEach(async () => {
  await prisma.$transaction([
    prisma.user.deleteMany(),
    prisma.project.deleteMany(),
  ]);
});
```

Điều này đảm bảo các bảng users và projects được reset trước khi chạy bất kỳ test nào khác.

Ngoài ra, nếu chia test thành các file riêng, bạn cần thiết lập test runner để chạy chúng tuần tự (từng file một) thay vì chạy song song. Việc này ngăn các thao tác trên cơ sở dữ liệu kiểm thử của các test khác nhau bị trộn lẫn. Vì Jest mặc định thực thi các file song song, bạn có thể thêm [cờ `--runInBand`](https://jestjs.io/docs/cli#--runinband) vào script `test` trong `package.json` để đảm bảo chúng được thực thi tuần tự.

Vậy là xong phần thiết lập. Giờ thì biến đi... và bắt đầu test thôi.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Không cần tìm hiểu quá sâu, nhưng hãy xem qua [thư mục tests trong repo GitHub của pg](https://github.com/brianc/node-postgres/tree/master/packages/pg/test) để thấy phần lớn các thư viện phổ biến được kiểm thử kỹ đến mức nào.

</div>