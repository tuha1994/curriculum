### Giới thiệu

Thuật ngữ "migrations" có thể khiến người ta hiểu nhầm rằng chủ đề của bài học này là data migrations, tức việc di chuyển dữ liệu từ nơi này sang nơi khác. Tuy nhiên, trong ngữ cảnh Rails và bài học này, một "migration" là một quá trình migration *schema*, tức sửa đổi schema của cơ sở dữ liệu mà không di chuyển cơ sở dữ liệu đó đến một địa điểm mới.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Migrations là gì.
- Khi nào cần migrations.
- Hai cách chính để tạo migration.
- Cách viết migrations.
- Cách chạy migrations.
- Vì sao migrations hữu ích.

### Migrations là gì?

Một Rails migration là một script thiết lập hoặc sửa đổi schema của cơ sở dữ liệu theo cách có thể hoàn tác và áp dụng lại. Như bạn đã học trong khóa học Databases, schema là cấu trúc của cơ sở dữ liệu. Việc tạo và xóa bảng, thêm và xóa cột, cũng như một số sửa đổi khác, là những thay đổi đối với schema; còn thêm và xóa các row thì không. Có thể chạy nhiều file migration tuần tự như một script. Bên dưới, migrations sử dụng các lệnh SQL thay đổi schema, tương tự cách Model thực hiện các truy vấn SQL thay bạn.

### Khi nào cần migrations

Hãy tưởng tượng bạn đang nhìn chằm chằm vào một màn hình máy tính trống và cần bắt đầu dự án Rails mới. Việc đầu tiên bạn làm là gì? Bạn gõ `$ rails new MyProjectName`, rồi `cd` vào thư mục đó... Sau đó thì sao?

Trước hết, hãy xác định các data model bạn sẽ cần cho phiên bản đầu tiên của trang web và bắt đầu thiết lập chúng. Trong phạm vi của chúng ta, cứ giả sử tất cả những gì bạn cần là model User quen thuộc để theo dõi hàng chục người dùng sẽ có mặt trên trang web một ngày nào đó (đùa thôi, bạn sẽ thành công lớn). Sau khi thực sự tạo cơ sở dữ liệu trước tiên (bằng `$ rails db:create`), để tạo model đó, bạn cần thực hiện hai bước:

1. Tạo một file model trong `app/models`, được thiết lập như bạn vừa học ở trên.
1. Tạo một bảng cơ sở dữ liệu có tên "users" với các cột phù hợp. Việc này được thực hiện bằng một file migration, sau đó chạy migration đó.

#### Tóm tắt

Migrations cần thiết ở giai đoạn đầu khi thiết lập các data model của dự án, vì một phần của quá trình đó là tạo các bảng cơ sở dữ liệu -- một thay đổi schema. Do đó, cần có một file migration, bởi trong Rails, sửa đổi schema đồng nghĩa với việc tạo rồi chạy một file migration.

### Cách tạo file migration

Có hai cách chính để tạo file migration. Cách thứ nhất là dùng model generator `$ rails generate model YourModelNameHere`. Cách thứ hai là dùng migration generator `$ rails generate migration NameYourMigration`.

#### Dùng model generator

Bạn có thể tạo file migration bằng cách chạy các generator, chẳng hạn shortcut tiện dụng `$ rails generate model YourModelNameHere`. Shortcut này không chỉ tạo file model mà còn tạo cả file migration. Khi gõ lệnh, bạn sẽ thấy trong output của Terminal những file đang được tạo. Đừng lo về các file spec hoặc test khác cũng được tạo; những file quan trọng là file model và file migration. Rails có rất nhiều generator tiện dụng như vậy; chúng chủ yếu chỉ tạo các file mới vào đúng vị trí trong ứng dụng của bạn. Output sẽ trông gần giống như sau:

```bash
  invoke  active_record
  create    db/migrate/20131223154310_create_testmodels.rb
  create    app/models/testmodel.rb
  invoke    rspec
  create      spec/models/testmodel_spec.rb
```

File model mà generator tạo ra chỉ là một file model tối giản trong thư mục `app/models` (bạn hoàn toàn có thể tự tạo file này). File chính còn lại là file migration trong thư mục `db/migrate`, bắt đầu bằng một timestamp trông khá phức tạp như `20130924230504_create_users.rb`. Con số đó là thời điểm migration được tạo, để Rails có thể theo dõi các file migration khác nhau.

Nếu mở file đó ra xem, bạn sẽ thấy bên trong hầu như không có gì ngoài một Ruby class tối giản khác kế thừa từ `ActiveRecord::Migration` cùng một vài timestamp. Các timestamp này tạo các cột `created_at` và `updated_at` cho bạn, để bạn theo dõi thời điểm các bản ghi cơ sở dữ liệu được tạo hoặc sửa đổi. Hai cột này hữu ích đến mức chúng được đưa vào như một thông lệ tiêu chuẩn.

#### Dùng migration generator

Nếu chỉ muốn tạo file migration của cơ sở dữ liệu (không tạo Model hay bất kỳ file test nào), hãy dùng `$ rails generate migration NameYourMigration`. Bạn sẽ dùng cách này nhiều hơn sau khi mọi thứ đã bắt đầu vận hành, vì có lẽ bạn sẽ sửa đổi bảng dữ liệu thay vì tạo bảng mới. Có một cú pháp để chỉ định các tham số bổ sung khi gọi lệnh này (bạn sẽ thấy trong phần đọc thêm), nhưng không cần phải ghi nhớ cú pháp đó vì bạn cũng có thể mở file migration và chỉnh sửa thủ công.

### Cách viết migrations

Để thêm các chỉ dẫn vào nội dung file migration, chỉ cần thêm phương thức Ruby phù hợp vào file migration, chẳng hạn phương thức có tên rất đúng nghĩa là `create_table`, và cung cấp các tham số cần thiết cho nó. Sau khi file migration đã được tạo và nội dung đã được viết, bước tiếp theo là chạy migration.

### Cách chạy migrations

Để hoàn tất migration và thực sự áp dụng các thay đổi vào schema, hãy dùng lệnh `$ rails db:migrate`, lệnh này chạy mọi migration chưa từng được chạy. Rails biết một migration đã được chạy hay chưa vì mỗi migration là một file có timestamp trong tên file. Khi chạy lệnh đó, Rails sẽ thực thi phần mã SQL phù hợp để thiết lập bảng cơ sở dữ liệu, rồi bạn có thể quay lại xây dựng trang web.

### Vì sao migrations hữu ích

Rõ ràng, migrations cho phép bạn thiết lập cơ sở dữ liệu bằng Ruby code thân thiện với người dùng thay vì SQL, nhưng lợi ích không chỉ có vậy. Theo thời gian, bạn sẽ tạo ra một loạt file migration. Nếu quyết định xóa toàn bộ cơ sở dữ liệu để bắt đầu lại từ đầu, bạn có thể dễ dàng làm vậy rồi chạy lại các migration. Nếu quyết định triển khai lên web, bạn sẽ chạy chính những migration đó và cơ sở dữ liệu production sẽ sẵn sàng chờ bạn... ngay cả khi đó là một loại cơ sở dữ liệu khác! Một lần nữa, Active Record sẽ gánh phần việc nặng ở đây để bạn có thể tập trung xây dựng trang web.

Tính năng hữu ích nhất trước mắt của migrations là khả năng xử lý khi bạn làm hỏng một thứ gì đó, vì chúng (thường) có thể hoàn tác. Giả sử bạn vừa chạy migration để tạo một cột cơ sở dữ liệu mới nhưng quên mất cột lưu email của người dùng... ôi không! Bạn chỉ cần gõ `$ rails db:rollback`, rồi loạt migration gần nhất đã chạy sẽ được hoàn tác và bạn quay lại trạng thái trước đó. Sau đó chỉ cần sửa file, chạy lại migrations và tiếp tục công việc.

Điều này dẫn đến điểm tinh tế cuối cùng về migrations mà chúng ta sẽ bàn ở đây -- khả năng hoàn tác. Với mỗi phương thức sử dụng trong migration, bạn cần chỉ định cách hoàn tác nếu cần. Phép đảo của việc thêm một bảng là xóa bảng đó, của việc thêm một cột là xóa cột đó, vân vân. Nhiều phương thức có cách hoàn tác quá rõ ràng, nên bạn không cần nêu cụ thể và có thể thiết lập toàn bộ file migration bằng phương thức `change`. Nhưng một số phương thức thì không như vậy, nên bạn sẽ phải chỉ định riêng các phương thức `up` và `down`. Bạn sẽ đọc thêm về việc này trong phần bài tập.

Một lưu ý cuối: bạn không bao giờ muốn rollback migrations trừ khi đã làm hỏng thứ gì đó. Trong những trường hợp chính đáng cần xóa một cột (vì bạn không còn cần nó cho bất kỳ mục đích nào), bạn thực sự sẽ tạo một migration mới để xóa cột đó bằng phương thức `remove_column`. Cách này bảo toàn cơ sở dữ liệu. Khi đã nâng cao hơn với những nội dung này, bạn có thể xây dựng cơ sở dữ liệu chỉ bằng file schema... nhưng bạn chưa đến đó đâu :)

### Chúng ta cần biết bao nhiêu về cơ sở dữ liệu?

Migrations không yêu cầu viết SQL, nhưng bạn vẫn cần hiểu đủ về cơ sở dữ liệu để biết mình muốn cấu trúc cơ sở dữ liệu như thế nào! Bạn muốn những cột nào? Cột nào nên được lập index (và tại sao)? Có nên đặt giá trị mặc định không? Cột của bạn sẽ lưu kiểu dữ liệu nào... string hay text?

Đây đều là những câu hỏi rất hay, và bạn nên thoải mái đặt ra chúng ngay cả khi chưa hoàn toàn chắc chắn về câu trả lời. Nếu không hiểu chúng ta đang nói về điều gì, bạn cần quay lại đọc về những kiến thức cơ bản của cơ sở dữ liệu trong [khóa học Databases](/paths/full-stack-ruby-on-rails/courses/databases).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương Migrations trong Rails Guides](http://guides.rubyonrails.org/active_record_migrations.html).
    - Không cần bận tâm đến mục 3.6-3.8.
    - Chỉ cần đọc lướt phần 7.
    - Seeds (phần 8) rất hữu ích và sau này bạn sẽ dùng đến. Chúng giúp bạn tiết kiệm rất nhiều công sức, đặc biệt trong giai đoạn học tập khi bạn sẽ thường xuyên xóa toàn bộ cơ sở dữ liệu và bắt đầu lại.

</div>