### Giới thiệu

Bạn đã chán việc phải viết các truy vấn SQL thuần trong những dự án của mình? Trong bài học này, chúng ta sẽ nói về Object Relational Mapper (gọi tắt là ORM). ORM là những công cụ cho phép bạn thao tác với dữ liệu trong cơ sở dữ liệu và được sử dụng rộng rãi trong ngành phần mềm. Chúng ta sẽ tìm hiểu sâu về một ORM phổ biến trong hệ sinh thái Node.js: [Prisma ORM](https://www.prisma.io/orm).

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về những chủ đề bạn sẽ học trong bài học này.

- ORM là gì và vì sao chúng được sử dụng.
- Giới thiệu Prisma ORM.
- Mô tả các tính năng của Prisma ORM.

### Những thách thức của SQL thuần

Với một vài dự án bạn đã thực hiện trước đây, có thể bạn đã gặp sự rườm rà của các truy vấn SQL thuần. Hãy cùng thảo luận ngắn gọn về một số thách thức này.

#### Quá nhiều mã hơn

Bạn cần một câu lệnh `SELECT`? Được thôi, hãy viết một truy vấn. Cần một câu lệnh `SELECT` cho một bảng khác? Hãy viết một truy vấn khác. Hoặc nếu bản năng lập trình của bạn đang mách bảo, bạn có thể tạo một hàm tiện ích cho `SELECT`. Nhưng giờ bạn cần một câu lệnh `SELECT` cho các cột cụ thể? Bạn sẽ phải sửa đổi hàm tiện ích đó. Còn bộ lọc và sắp xếp thì sao? Bạn hiểu ý rồi đấy.

Hãy lặp lại những việc trên với các truy vấn insert và tất cả biến thể của chúng. Hoặc bạn có thể đi theo một hướng khác. Tạo các module theo từng entity. Ví dụ, module books sẽ có:

```javascript
class Book {
  async getBooks(filters) {}
  async getBookById(id) {}
  async createBook(data) {}
  async updateBook(id, data) {}
  async deleteBook(id) {}
  async getBookAuthors(id) {}
  async getBookGenres(id) {}
  // and so on
}
```

Hoặc bạn có thể kết hợp hai cách tiếp cận này. Tạo một lớp `Database` rồi kế thừa nó trong các lớp entity. Hoặc nếu thích composition, bạn chỉ muốn làm việc với các hàm thuần.

Bây giờ hãy làm đi làm lại việc này cho từng entity, trong nhiều dự án. Bạn hiểu ý rồi đấy. Có thể lập luận rằng viết nhiều mã hơn không nhất thiết là điều xấu, vì bạn đang học và luyện tập, và đúng là như vậy. Cách này đủ dùng cho các dự án cá nhân, thậm chí chúng tôi còn khuyến khích bạn khám phá những ý tưởng trên. Nhưng khi làm việc theo nhóm và với phần mềm quy mô lớn, việc có một cách chuẩn hóa để tương tác với cơ sở dữ liệu trở nên bắt buộc, dù đó là thư viện bên ngoài hay giải pháp tự xây dựng. Bạn sẽ nhận ra ORM thực sự giúp mình tập trung vào việc viết những mã quan trọng đối với nghiệp vụ đến mức nào.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Refactor các dự án trước đây

Nếu bạn chưa áp dụng những mô hình trên vào các dự án trước đây, chúng tôi đặc biệt khuyến khích bạn quay lại và refactor chúng. Có thể cuối cùng bạn sẽ tạo ra một mini ORM rất cơ bản cho dự án của mình. Việc này sẽ giúp bạn hiểu rõ hơn ORM đúng nghĩa có thể làm cho công việc của bạn dễ dàng hơn như thế nào.

</div>

#### Điều hướng codebase

Khi mọi tương tác với cơ sở dữ liệu đều được thực hiện bằng SQL thuần, trong codebase sẽ không có nơi nào giúp bạn hiểu các bảng cơ sở dữ liệu, quan hệ giữa chúng và kiểu dữ liệu của các cột. Có thể bạn sẽ phải đăng nhập vào cơ sở dữ liệu để hiểu codebase đang làm gì. Để đạt được hiểu biết kỹ thuật về dự án, giờ đây bạn phải dựa vào cả codebase lẫn quyền truy cập vào cơ sở dữ liệu.

Hầu hết ORM hiện có đều giải quyết vấn đề này bằng cách đưa các định nghĩa cơ sở dữ liệu vào codebase. Đây được gọi là “schema”. Nhờ đó, bạn có thể nhanh chóng xem qua schema của một bảng và hiểu bảng đó có những cột nào, vân vân.

#### Thay đổi dữ liệu production

Cơ sở dữ liệu chắc chắn sẽ thay đổi khi các yêu cầu của dự án phát triển. Có thể bạn phải thêm một cột mới hoặc điền dữ liệu hiện có vào một bảng mới. Về mặt kỹ thuật, việc này được gọi là migration. Nếu không có ORM hoặc thư viện tương tự, bạn sẽ phải tự viết các migration, một công việc dễ phát sinh lỗi và rất tẻ nhạt. ORM chuẩn hóa migration thông qua changelog và có các quy trình để xử lý xung đột. Đúng là trong các dự án curriculum, bạn sẽ không chạy nhiều migration đến vậy, nhưng khi làm việc chuyên nghiệp, có thể bạn sẽ thực hiện việc này cách ngày.

### Giới thiệu Prisma ORM

ORM gần như giải quyết tất cả những vấn đề khó chịu đã nêu trên, nhưng mọi chuyện không phải lúc nào cũng hoàn toàn thuận lợi. Có thể cần một thời gian học để hiểu đầy đủ các chi tiết của một ORM, và thậm chí một số ORM không hỗ trợ trọn vẹn mọi tính năng SQL. Dù có những hạn chế này, sử dụng ORM vẫn có thể cực kỳ đáng giá.

Trong hệ sinh thái Node.js, có *rất nhiều* ORM để lựa chọn, và cộng đồng vẫn chưa thống nhất một lựa chọn mặc định. Chúng tôi quyết định giới thiệu [Prisma ORM](https://www.prisma.io/orm) trong curriculum vì mức độ phổ biến và sự hỗ trợ của cộng đồng.

Prisma ORM có tất cả các tính năng chúng ta cần để hoàn thành curriculum này và còn hơn thế nữa. Prisma ORM gồm một số thư viện, vì vậy chúng ta có thể dùng npm để cài đặt những thư viện mà ứng dụng cần. Trước hết, chúng ta sẽ thảo luận về một số tính năng của nó, sau đó liên kết đến một số hướng dẫn Prisma ORM chính thức để giúp bạn bắt đầu.

### Prisma Schema

Prisma schema là một file nơi bạn định nghĩa các model. Ví dụ, hãy xem xét một bảng message trong ứng dụng trò chuyện:

```text
model Message {
   id        Int      @id @default(autoincrement())
   content   String   @db.VarChar(255) 
   createdAt DateTime @default(now())
   author    User     @relation(fields: [authorId], references: [id])
   authorId  Int     
}

model User {
   // user's fields
}
```

Có khá nhiều điều mới mẻ ở đây. Các file Prisma schema được viết bằng Prisma Schema Language. Bạn có thể thấy không chỉ có định nghĩa các cột của bảng, mà còn có một quan hệ với bảng khác được định nghĩa bên trong model `Message`. Bạn sẽ học thêm về điều này trong các tài liệu đọc của bài tập.

File schema này nằm trong codebase và được theo dõi bằng hệ thống quản lý phiên bản. Đến đây chắc bạn đã đoán được điều này hữu ích đến mức nào.

### Prisma Client

Client là một thư viện riêng mà bạn sẽ dùng để tương tác với cơ sở dữ liệu. Prisma client đặc biệt ở chỗ nó được tùy chỉnh theo schema của bạn. Ừm... là sao?

```javascript
// instantiate the client
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

// when creating a new message
await prisma.message.create({
   data: {
      content: 'Hello, world!',
      authorId: 1
   }
})

// when fetching all messages
const messages = await prisma.message.findMany();
```

Bạn có để ý đối tượng `prisma.message` không? Làm thế nào Prisma Client biết có một model `message`? Sau khi tạo hoặc cập nhật file schema, tất cả những gì bạn cần làm là chạy

```bash
npx prisma generate
```

trong CLI, và Prisma ORM sẽ tạo client cho bạn. Client có thể xử lý đủ loại truy vấn: join, lọc, sắp xếp, phân trang và nhiều thao tác khác.

Có thể bạn gặp một truy vấn phức tạp mà không thể viết đúng bằng Prisma client, hoặc bạn cảm thấy thoải mái hơn khi tự viết một truy vấn thuần; bạn cũng có thể làm vậy. Prisma Client cũng hỗ trợ các truy vấn thuần.

### Prisma Migrate

Prisma migrate là một công cụ giúp bạn thực hiện migration cơ sở dữ liệu. Bạn sẽ không sử dụng nó quá nhiều trong curriculum, nhưng vẫn nên biết về nó. Khi quyết định thay đổi schema theo bất kỳ cách nào, bạn chạy một Prisma migration để áp dụng các thay đổi schema vào cơ sở dữ liệu. Những thay đổi này được theo dõi trong thư mục `migrations` của codebase.

<div class="lesson-note lesson-note--warning" markdown="1">

#### Hạn chế của Prisma ORM

Trong [bài Using PostgreSQL](https://www.theodinproject.com/lessons/nodejs-using-postgresql), chúng ta đã học về các cột Identity. PostgreSQL khuyến nghị sử dụng cột Identity vì chúng tuân thủ tiêu chuẩn SQL. Tuy nhiên, Prisma ORM không hỗ trợ các cột này và thay vào đó sẽ tạo [Serial Types](https://www.postgresql.org/docs/16/datatype-numeric.html#DATATYPE-SERIAL) đặc thù của PostgreSQL. Điều này nhiều khả năng sẽ không ảnh hưởng đến các dự án của bạn, nhưng vẫn là điều quan trọng cần ghi nhớ. Hãy xem câu trả lời trên Stackoverflow này để đọc mô tả ngắn về [sự khác nhau giữa Serial và Identity](https://stackoverflow.com/a/55300741/1882858).

</div>

<div class="lesson-note lesson-note--tip" markdown="1">

#### Extension Prisma chính thức trên Visual Studio Code

Nếu sử dụng VS Code, bạn có thể muốn cài đặt [extension Prisma VS Code chính thức](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma) để có các tính năng như tô màu cú pháp, IntelliSense / tự động hoàn thành, lint schema và điều hướng dễ dàng hơn giữa các model. Nó giúp việc làm việc với các file Prisma schema dễ chịu hơn nhiều.

</div>

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Làm theo [Quickstart with Prisma v7 ORM and PostgreSQL](https://www.prisma.io/docs/v7/prisma-orm/quickstart/postgresql), tài liệu này trình bày về migration, schema và Prisma client.

   <div class="lesson-note" markdown="1">

   #### Để Prisma hoạt động với JavaScript

   Gần đây, Prisma đã quyết định chỉ tiếp tục hỗ trợ Typescript. Điều này có nghĩa là chúng ta cần sửa đổi hướng dẫn Quickstart của họ để nó hoạt động với JavaScript.

   Khi làm theo hướng dẫn Prisma Quickstart, hãy sửa các bước của Quickstart như mô tả dưới đây.

   **Bước 1:** Bỏ qua các lệnh sau:

   ```bash
   npm init
   npm install typescript tsx @types/node --save-dev
   npx tsc --init
   ```

   **Bước 2:** Bạn không cần cài đặt `@types/pg`. Nếu nhận được cảnh báo về “install scripts not yet covered by allowScripts”, bạn có thể yên tâm bỏ qua vì Prisma sẽ thực hiện các thao tác cần thiết ở bước sau.

   **Bước 3:** Chúng ta không sử dụng Typescript, vì vậy sẽ bỏ qua hoàn toàn bước này.

   **Bước 4:** Chúng ta muốn sử dụng generator `prisma-client-js` thay vì generator mặc định, vì vậy hãy thêm `--generator-provider prisma-client-js` vào lệnh Prisma init. Chúng ta cũng không muốn mặc định cài đặt các tính năng AI từ Prisma Skills catalog, nên hãy thêm `--no-skills` để chọn không tham gia:

   ```bash
   npx prisma init --datasource-provider postgresql --output ../generated/prisma --generator-provider prisma-client-js --no-skills
   ```

   Chúng ta cũng sẽ đổi tên `prisma7.config.ts` thành `prisma7.config.js`.

   **Bước 5 và 6:** Không cần thay đổi gì.

   **Bước 7:** Thay vì `lib/prisma.ts`, hãy tạo `lib/prisma.js`. Bạn cũng phải thêm phần mở rộng file `.js` khi import `PrismaClient` trong file đó:

   ```javascript
   import { PrismaClient } from '../generated/prisma/client.js';
   ```

   **Bước 8:** File được tạo phải có tên `script.js`, và khi import `prisma` trong file đó, hãy thêm phần mở rộng file `.js`, để dòng đầu tiên là:

   ```javascript
   import { prisma } from './lib/prisma.js';
   ```

   Để chạy script, hãy chạy `node script.js`.

   **Bước 9:** Không cần thay đổi gì.

1. Đọc các bài viết sau trong tài liệu Prisma. Hãy thử code theo các ví dụ. Đừng lo nếu bạn chưa nhớ ngay mọi thứ; chúng ta sẽ luyện tập *rất nhiều* Prisma trong các dự án tiếp theo.
   - [What is Prisma ORM?](https://www.prisma.io/docs/orm/v7)
   - [Prisma schema overview](https://www.prisma.io/docs/orm/v7/prisma-schema/overview)
   - [Data models](https://www.prisma.io/docs/orm/v7/prisma-schema/data-model/models)
   - [Relations](https://www.prisma.io/docs/orm/v7/prisma-schema/data-model/relations)
   - [Prisma client CRUD](https://www.prisma.io/docs/orm/v7/prisma-client/queries/crud)
   - [Raw SQL](https://www.prisma.io/docs/orm/v7/prisma-client/using-raw-sql)
   - [Prisma migrate getting started](https://www.prisma.io/docs/orm/v7/prisma-migrate/getting-started)
   - [Prisma migrate mental model](https://www.prisma.io/docs/orm/v7/prisma-migrate/understanding-prisma-migrate/mental-model)
   - [Data migrations](https://www.prisma.io/docs/guides/v7/database/data-migration)

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [Prisma Crash Course](https://www.youtube.com/watch?v=CYH04BJzamo) của Traversy Media.