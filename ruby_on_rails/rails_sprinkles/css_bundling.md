### Giới thiệu

Bạn đã tìm hiểu cách mặc định mà Rails xử lý CSS như một phần của Asset Pipeline. Tuy nhiên, cùng với sự phát triển không ngừng của các bộ tiền xử lý và bundler CSS khác nhau, đã xuất hiện một khoảng trống về một cách thức quy ước để sử dụng những công cụ này. Trong Rails 7, một gem tùy chọn có tên [cssbundling-rails](https://github.com/rails/cssbundling-rails) đã được phát hành. Gem này cung cấp các trình cài đặt cho những công cụ phổ biến nhất dùng cho CSS. Ở đây, chúng ta sẽ học cách sử dụng gem này để mở rộng CSS mà mình có thể sử dụng và viết. Chúng ta sẽ không đi sâu vào từng công cụ, vì cách sử dụng sẽ phụ thuộc vào nhu cầu của bạn, nhưng sẽ tìm hiểu lý do bạn có thể muốn lựa chọn từng công cụ.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài.

- Những công cụ có thể được cài đặt bằng gem cssbundling-rails.
- Vì sao bạn sẽ sử dụng cssbundling thay vì cách xử lý stylesheet tiêu chuẩn của asset pipeline.
- Những mặt hạn chế của việc sử dụng cssbundling-rails.
- Các tùy chọn dòng lệnh của cssbundling.

### Các tùy chọn công cụ của Cssbundling

Cssbundling-rails hiện hỗ trợ cài đặt năm công cụ khác nhau để bundling và xử lý CSS.

1. [Bootstrap](https://getbootstrap.com/): Đây có lẽ là một trong những thư viện nổi tiếng nhất để bắt đầu định hình giao diện và cảm nhận của một ứng dụng mới khi bạn cần một bản proof of concept.
1. [Bulma](https://bulma.io/): Một lựa chọn thay thế cho Bootstrap. Công cụ này có cách định kiểu mang tính định hướng rõ ràng, giúp bạn nhanh chóng bắt tay vào làm khi chưa có nhiều kiến thức CSS để tự thiết kế website.
1. [Dart Sass](https://sass-lang.com/dart-sass): Lựa chọn phổ biến hiện nay để xử lý các tệp sass. Công cụ này cho phép bổ sung các tính năng CSS nhằm mở rộng những gì bạn có thể làm.
1. [PostCSS](https://postcss.org/): Một công cụ biến đổi CSS bằng JavaScript. Nó cho phép bạn sử dụng những tính năng CSS tương lai chưa được mọi trình duyệt hỗ trợ, hoặc dùng một trong rất nhiều plugin PostCSS để nâng cao CSS.
1. [Tailwind CSS](https://tailwindcss.com/): Một framework CSS theo hướng utility-first. Nó hoạt động tương tự Bootstrap ở chỗ bạn thêm CSS vào ứng dụng bằng cách thêm các class vào HTML. Điểm khác biệt là Tailwind không phải cách để né tránh việc hiểu CSS; thay vào đó, nó cho phép bạn xây dựng thiết kế bằng các class có thể tái sử dụng, mỗi class chỉ thực hiện một việc. CSS được chuyển vào phần markup thay vì nằm dưới một tên class trong các tệp CSS. Bạn sẽ yêu thích nó hoặc không ưa nó.

### Cài đặt

Với một ứng dụng Rails 7 mới, việc cài đặt đơn giản chỉ là thêm một đối số dòng lệnh khi tạo ứng dụng:

```bash
rails new myapp --css [tailwind|bootstrap|bulma|postcss|sass]
```

Nếu muốn sử dụng Bootstrap, bạn sẽ chạy:

```bash
rails new myapp --css bootstrap
```

Với một ứng dụng hiện có, bạn chỉ cần thực hiện các bước sau:

1. Thêm `gem 'cssbundling-rails'` vào Gemfile.
1. Chạy `bundle install` trên dòng lệnh.
1. Sử dụng generator do gem cung cấp để cài đặt công cụ bạn cần `./bin/rails css:install:[tailwind|bootstrap|bulma|postcss|sass]`; chẳng hạn, nếu dùng Bootstrap, bạn sẽ viết `./bin/rails css:install:bootstrap`.

Chỉ có vậy.

### Tập lệnh build

Khi sử dụng cssbundling, gem sẽ thực hiện một vài việc cho bạn. Trước hết, bất kể bạn chọn tùy chọn nào, nó sẽ cài đặt các công cụ và tệp cần thiết để chạy chúng, đồng thời đảm bảo các câu lệnh import bắt buộc được thêm vào ứng dụng. Sau đó, gem hướng dẫn bạn về build script cần sử dụng.

Điều thú vị là gem này khá đơn giản. Nếu xem thư mục [lib/install](https://github.com/rails/cssbundling-rails/tree/main/lib/install) của gem, bạn sẽ nhận thấy nó có rất ít tệp. Tệp `install.rb` đảm bảo tất cả thành phần cần thiết đều có mặt để xử lý tùy chọn cài đặt mà bạn sử dụng, sau đó mỗi công cụ có một thư mục riêng chứa các tệp cần thiết để công cụ đó hoạt động. Hãy xem tệp cài đặt của [Bootstrap](https://github.com/rails/cssbundling-rails/blob/main/lib/install/bootstrap/install.rb). Bạn sẽ thấy nó ngắn đến mức nào.

Build script được dùng để lấy CSS bạn đã viết và xử lý nó thông qua công cụ đã chọn. Chẳng hạn, nếu chọn Tailwind, script cần quét tất cả utility class đó và đảm bảo chỉ những class bạn sử dụng được đưa vào, để tệp Tailwind nhỏ nhất có thể. Build script được đặt trong tệp package.json, đó là lý do bạn cần cài nodejs để sử dụng gem này. Trong môi trường production, build script được dùng để đảm bảo toàn bộ asset được build đúng cách; nhưng trong môi trường development, bạn cần build lại mỗi khi có thay đổi. Vì vậy, gem cung cấp một lệnh dùng để theo dõi các tệp và build lại khi cần.

Nếu sử dụng gem jsbundling hoặc cssbundling, bạn luôn nên khởi động ứng dụng bằng cách chạy `./bin/dev` trong terminal. Tác vụ này sẽ chạy Rails server cùng các lệnh theo dõi JS và CSS, nhằm đảm bảo asset được build lại khi cần.

Tác vụ `./bin/dev` sử dụng một gem có tên [foreman](https://github.com/ddollar/foreman). Gem này dùng Procfile để chạy nhiều tiến trình khác nhau trong cùng một terminal, nhờ đó có thể vừa chạy Rails server vừa theo dõi các asset để phát hiện thay đổi.

### Vì sao nên dùng CSS bundling?

Gem CSS bundling không phải cách duy nhất để tích hợp các công cụ như Bulma vào ứng dụng. Có những gem dành riêng cho các thư viện đó, các package npm, hoặc bạn thậm chí có thể tham chiếu trực tiếp đến các tệp đã minify được lưu trữ trên một CDN.

README của gem cssbundling-rails cũng đề cập rằng đã có các gem đưa Tailwind và Dart Sass vào ứng dụng, nhờ đó bạn không phải cài Node.js để sử dụng chúng.

Tuy vậy, gem CSS bundling cho phép chúng ta cài đặt liền mạch và đưa các công cụ đó vào hoạt động trong ứng dụng Rails với cấu hình và công sức tối thiểu. Đây là một lợi thế rất mạnh, giúp bạn phát triển nhanh hơn nhiều mà không cần đưa cả thư viện vào ứng dụng, từ đó góp phần giảm thời gian build.

Nếu bạn thấy mình muốn nhiều hơn cách tiêu chuẩn của Rails để tạo stylesheet được ánh xạ theo tên controller, thì đây nên là gem đầu tiên bạn tìm đến khi muốn sử dụng một giải pháp khác.

### Bài tập

Không có nhiều nội dung mới ngoài những gì đã trình bày ở trên, nhưng dưới đây là một vài gợi ý.

<div class="lesson-content__panel" markdown="1">

  1. Đọc [README của gem cssbundling-rails](https://github.com/rails/cssbundling-rails), vì phần FAQ có một số thông tin về cách sử dụng gem và thỉnh thoảng được cập nhật thêm thông tin mới.
  1. Nếu quan tâm đến bất kỳ công cụ nào mà gem cung cấp, hãy truy cập website tương ứng và xem cách bắt đầu sử dụng công cụ đó trong các ứng dụng của mình.

</div>