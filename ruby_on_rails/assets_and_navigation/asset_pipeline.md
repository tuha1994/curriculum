### Giới thiệu

Bạn đã tìm hiểu về Model, View và Controller. Đó là những bộ phận cốt lõi, nhưng vẫn còn rất nhiều điều thú vị giúp Rails hữu ích hơn cho bạn. Trong bài học này, chúng ta sẽ nói về Asset Pipeline và một vài chủ đề khác không thật sự phù hợp với các bài học khác nhưng vẫn quan trọng và cần được đề cập.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài học này.

- Các tệp được xử lý như thế nào trong Asset Pipeline.
- Cách tổ chức stylesheet và hình ảnh trong ứng dụng.

### Asset Pipeline

Assets trong ứng dụng là những tệp bổ sung được trình duyệt yêu cầu sau khi nhận được phần HTML ban đầu. Từ "assets" dùng để chỉ những thứ như stylesheet CSS, tệp JavaScript, hình ảnh, video, v.v.—về cơ bản là bất kỳ thứ gì cần một request bổ sung để tải xuống.

Asset Pipeline là một trong vài cách khác nhau để xử lý assets trong Rails. Trong suốt khóa học, bạn sẽ tìm hiểu thêm những cách quản lý CSS và JS khác. Cách phù hợp có thể phụ thuộc rất nhiều vào quy ước tại nơi bạn làm việc sau này, nhưng khi hiểu thêm, bạn sẽ nắm được Asset Pipeline khác các phương án thay thế ra sao cũng như cách sử dụng từng phương pháp sẵn có để xử lý assets trong ứng dụng. Ở đây, chúng ta sẽ tìm hiểu cách Asset Pipeline hoạt động; trong các bài học sau, chúng ta sẽ tìm hiểu những phương án thay thế. Với các dự án riêng, chúng tôi khuyên bạn dùng import maps cho mã JavaScript và Asset Pipeline cho các assets khác.

Quay lại với Asset Pipeline: để phục vụ việc phát triển, thông thường sẽ dễ tổ chức mã thành nhiều tệp khác nhau để bạn dễ theo dõi hơn. Nhưng nếu trình duyệt phải tải cả chục tệp CSS khác nhau, mỗi request sẽ làm chậm ứng dụng. Quá nhiều request sẽ khiến trải nghiệm của người dùng với ứng dụng bị ảnh hưởng nghiêm trọng.

Một vấn đề tổ chức tương tự liên quan đến việc lưu trữ những thứ như hình ảnh. Tách chúng ra trong thư mục sẽ dễ quản lý hơn, nhưng bạn cũng muốn liên kết đến chúng thật thuận tiện để các thẻ hình ảnh hoạt động ổn định.

Asset Pipeline giúp chúng ta giải quyết những vấn đề này.

### Lịch sử

Trước khi đi sâu vào chi tiết về Asset Pipeline, tìm hiểu một chút về lịch sử quản lý assets của Rails sẽ rất hữu ích. Trước đây, Asset Pipeline xử lý mọi loại asset, nhưng hiện nay, cách làm tốt hơn là xử lý assets JavaScript bằng những công cụ quản lý JavaScript khác đi kèm các ứng dụng Rails hiện đại. Trong bài học tiếp theo, bạn sẽ tìm hiểu về "import maps", vốn trở thành cách tiêu chuẩn để thêm và quản lý các tệp JavaScript kể từ khi Rails 7 được phát hành.

Trước Rails 8, Asset Pipeline được xử lý thông qua một thư viện có tên là `sprockets`. `sprockets` có nhiều khả năng hơn Asset Pipeline hiện tại, cung cấp các tính năng như thu nhỏ, nối, chuyển đổi cú pháp, v.v. Đội ngũ Rails quyết định rằng những tính năng này tạo ra quá nhiều gánh nặng bảo trì và không hoàn toàn cần thiết trong bối cảnh ES6 được hỗ trợ rộng rãi (giúp đưa các module JS vào trình duyệt) và HTTP/2.0 trở nên phổ biến (cho phép phục vụ nhiều request trên cùng một kết nối TCP).

Hiện nay, Asset Pipeline được xử lý thông qua một thư viện có tên là `propshaft`, với tập tính năng gọn hơn nhưng đơn giản hơn nhiều và cần ít cấu hình hơn. Nếu có nhu cầu phức tạp hơn khi phục vụ CSS và/hoặc JavaScript, các nhà phát triển được khuyến khích tìm đến những giải pháp bên ngoài Asset Pipeline.

Biết một phần lịch sử này có thể hữu ích vì nhiều ứng dụng dùng Rails không nhất thiết đang sử dụng phiên bản tiên tiến nhất. Nhiều ứng dụng Rails vẫn sẽ dùng `sprockets` để phục vụ assets; dù khóa học này không đi sâu vào nó, ít nhất bạn cũng nên biết đến nó và hiểu nó khác `propshaft` như thế nào.

### Fingerprinting

Rails sử dụng một kỹ thuật gọi là "fingerprinting" để thêm mã định danh duy nhất vào tên tệp asset. Khi trình duyệt tải `application.css` (chẳng hạn), bạn có thể thấy tệp này được gọi bằng tên như `application-1fc71ddbb281c144b2ee4af31cf0e308.css`. Chuỗi ký tự trông vô nghĩa trong URL đó là "fingerprint", và nó nhằm hỗ trợ bộ nhớ đệm của trình duyệt. Trình duyệt tự động cố gắng lưu nhiều assets vào bộ nhớ đệm dựa trên URL của chúng, nhờ đó hiệu năng được cải thiện vì trình duyệt không cần request lại một asset nếu đã lưu nó. Những fingerprint này hoạt động như một dạng versioning dành cho trình duyệt. Nếu bạn thay đổi `application.css`, Rails sẽ biết cần thay đổi fingerprint trong URL của asset đó; khi không tìm thấy URL asset mới trong bộ nhớ đệm, trình duyệt sẽ biết phải tải stylesheet mới.

Fingerprinting được tự động cung cấp khi dùng các helper liên kết của Rails. Vì vậy, một tệp CSS được liên kết trong phần tử `<head>` của `app/views/layouts/application.html.erb` sẽ trông như sau:

```erb
<head>
  <%= stylesheet_link_tag "application" %>
</head>
```

### Hình ảnh

Đối với hình ảnh, asset pipeline giữ chúng trong thư mục `/assets`, trừ khi bạn tự tạo các thư mục con. Hãy dùng `image_tag` để tránh nhầm lẫn; chẳng hạn, `<%= image_tag "fuzzy_slippers.jpg" %>` sẽ tìm tệp có tên `fuzzy_slippers.jpg` trong thư mục `/assets/images`. Bạn cũng có thể tổ chức hình ảnh trong các thư mục con, điều này rất hữu ích khi ứng dụng phát triển và sử dụng ngày càng nhiều hình ảnh, biểu tượng.

### Bài tập

Dưới đây là phần đọc cần thiết và tương đối dễ tiếp cận về Asset Pipeline:

<div class="lesson-content__panel" markdown="1">

  1. Đọc các mục 1 đến 3 trong [Rails Guides về Asset Pipeline](https://guides.rubyonrails.org/asset_pipeline.html).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [Railscast của Ryan Bates về asset pipeline](http://railscasts.com/episodes/279-understanding-the-asset-pipeline?view=asciicast)
- [Schneems nói về Asset Pipeline](https://www.youtube.com/watch?v=FYdBpNUVxuI)