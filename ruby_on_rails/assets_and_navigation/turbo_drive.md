### Giới thiệu

Rails từ lâu đã tự giới thiệu mình là một framework giúp việc xây dựng ứng dụng web trở nên dễ dàng. Nhưng trong một thế giới được định hình bởi yêu cầu phản hồi nhanh như chớp của người dùng cùng giao diện ngày càng phức tạp, Rails khó có thể đưa ra một tầm nhìn thuyết phục về cách vượt qua sự phức tạp đó mà vẫn trung thành với những triết lý cốt lõi của Rails. Tuy nhiên, ngay trước khi Rails 7 ra mắt, cuối cùng chúng ta cũng được thấy phần nào cách đội ngũ Rails hình dung về tương lai đáp ứng những yêu cầu này. Hotwire!

Hotwire thực ra là một thuật ngữ bao trùm ba framework khác nhau. Các framework đó là:

1. Turbo
1. Stimulus
1. Strada

Stimulus là nội dung chúng ta sẽ học sau, còn trong phạm vi khóa học này bạn không cần bận tâm đến Strada; chỉ cần quen với tên gọi này vì đôi khi bạn sẽ thấy nó được nhắc đến.

Bản thân Turbo cũng là một thuật ngữ bao trùm một số kỹ thuật khác nhau để tạo ra các ứng dụng web nhanh và hiện đại. Kỹ thuật chúng ta sẽ tập trung ở đây là kỹ thuật có hành vi thuộc chức năng cốt lõi của Rails: Turbo Drive. Những kỹ thuật còn lại sẽ được học trong bài Advanced Turbo sau này.

Liên kết và biểu mẫu là nền tảng của hầu hết ứng dụng web. Bạn cần liên kết để điều hướng quanh trang web và biểu mẫu để người dùng gửi thông tin cho bạn. Turbo Drive là cách Rails xử lý những thành phần này trong ứng dụng của bạn.

Nếu nhiều phần tiếp theo có vẻ khó hiểu thì đừng lo! Phần lớn hoạt động của Turbo Drive diễn ra ở phía sau, vì vậy chúng ta thường cần thảo luận về khái niệm thay vì cách triển khai. Điều này ban đầu có thể khiến việc nắm bắt khó hơn, nhưng may mắn là nó cũng có nghĩa bạn thường có thể tiếp tục công việc mà không cần chủ động suy nghĩ về nó.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài này.

- Mục đích của Turbo Drive.
- Cách Turbo Drive xử lý việc điều hướng trang.
- Hai loại lượt truy cập trang trong Turbo.
- Cách tắt Turbo Drive.
- Turbolinks là gì.

### Turbo Drive

Trước khi đi sâu vào Turbo Drive, hãy cùng nói về Turbo nói chung. Đây là một thư viện JavaScript được Rails cung cấp mặc định và chạy trong trình duyệt. Theo cách truyền thống, việc tải lại toàn bộ trang chậm, làm mất trạng thái JavaScript, còn việc tải lại tài nguyên thì tốn thời gian. Turbo xuất hiện: nó giúp việc tạo các ứng dụng web nhanh và hiện đại trở nên dễ dàng hơn bằng cách cho phép máy chủ cung cấp HTML trực tiếp cho trình duyệt.

Turbo có một vài kỹ thuật làm nên đặc trưng của nó: Turbo Drive được dùng để tăng tốc điều hướng mà không cần tải lại toàn bộ trang. Turbo Frames giúp xác định phần nội dung cụ thể của trang cần được thay thế mà không phải yêu cầu toàn bộ trang. Turbo Streams phản hồi các thay đổi theo thời gian thực ở phía client thông qua WebSocket. Cuối cùng, Turbo Native cho phép điều hướng nhanh trong các ứng dụng di động.

Turbo Drive là phần của Turbo giúp tăng tốc việc điều hướng trang nói chung. Turbo Drive theo dõi khi người dùng nhấp vào một liên kết hoặc gửi biểu mẫu, xử lý yêu cầu do người dùng tạo ra, rồi cập nhật trang cho người dùng mà không cần tải lại toàn bộ! Đây là hành vi mặc định, không cần cấu hình gì thêm. Tuy vậy, việc hiểu chính xác Turbo Drive đang làm gì vẫn rất quan trọng, phòng khi bạn cần sửa đổi hoặc tắt thủ công hành vi của nó ở một số phần trong trang web.

#### Điều hướng trang

Turbo Drive định nghĩa việc điều hướng trang là một *lượt truy cập* đến một *địa chỉ* (URL) cùng một *hành động*.

Một lượt truy cập là vòng đời điều hướng bắt đầu khi người dùng nhấp vào một liên kết và kéo dài cho đến khi trang được hiển thị. Vòng đời này bao gồm yêu cầu HTTP, cập nhật lịch sử trình duyệt, khôi phục trang từ bộ nhớ đệm (nếu có), hiển thị phản hồi cuối cùng và cập nhật vị trí cuộn của người dùng.

Có hai loại lượt truy cập:

1. **Lượt truy cập ứng dụng**, là lượt truy cập có hành động Drive là *tiến tới* hoặc *thay thế*.
1. **Lượt truy cập khôi phục**, là lượt truy cập có hành động Drive là *khôi phục*.

#### Lượt truy cập ứng dụng

Vòng đời của lượt truy cập ứng dụng có thể được tóm tắt như sau:

1. Lượt truy cập ứng dụng bắt đầu khi người dùng nhấp vào một liên kết được Turbo Drive bật (hãy nhớ rằng Turbo Drive mặc định được bật trên các liên kết!).
1. Một yêu cầu mạng HTTP được gửi đi. Turbo Drive tiếp nhận yêu cầu đó và sẽ hiển thị HTML.
1. Nếu có thể, Turbo Drive sẽ dùng bộ nhớ đệm của trình duyệt để hiển thị ngay bản xem trước của trang sau khi lượt truy cập bắt đầu, bằng HTML của lượt truy cập trước đó đến cùng URL.
1. Lịch sử trình duyệt được cập nhật để phản ánh việc điều hướng trang này. Cách cập nhật được quyết định bởi hành động của lượt truy cập.
    - **Tiến tới**: Đây là hành động mặc định và sẽ thêm một mục mới vào lịch sử trình duyệt.
    - **Thay thế**: Hành động này thay thế mục gần nhất trong lịch sử trình duyệt bằng địa chỉ mới.

Để thay đổi hành động của một liên kết Turbo Drive, bạn có thể dùng các thuộc tính dữ liệu bên trong thẻ liên kết Rails.

```erb
<%= link_to "Edit Article", edit_article_path(@article), data: { turbo_action: "replace" } %>
```

sẽ tạo ra:

```html
<a href="..." data-turbo-action="replace">Edit Article</a>
```

#### Lượt truy cập khôi phục

Vòng đời của lượt truy cập khôi phục có thể được tóm tắt như sau:

1. Lượt truy cập khôi phục bắt đầu khi người dùng điều hướng bằng các nút tiến và lùi của trình duyệt.
1. Nếu có thể, Turbo Drive sẽ dùng bộ nhớ đệm của trình duyệt để hiển thị ngay bản xem trước của trang sau khi lượt truy cập bắt đầu. Nếu không, nó sẽ lấy một bản sao mới của trang qua mạng.
1. Vị trí cuộn của trình duyệt được lưu trên mỗi trang trước khi rời đi và sẽ được khôi phục về vị trí đã lưu này.

Lượt truy cập khôi phục là các lượt truy cập có hành động *khôi phục*. Turbo Drive sử dụng hành động này ở bên trong, và bạn **không nên** gắn hành động khôi phục cho một liên kết.

#### Phương thức yêu cầu HTTP

Theo mặc định, thao tác nhấp vào liên kết được gửi bằng yêu cầu `GET`. Tuy nhiên, Turbo Drive sẽ quét các thẻ `<a>` trong ứng dụng để tìm thuộc tính `turbo-method`, qua đó ghi đè hành động `GET`.

Ví dụ:

```erb
<%= link_to "Delete Article", article_path(@article), data: { turbo_method: "delete" }  %>
```

sẽ tạo ra:

```html
<a href="..." data-turbo-method="delete">Delete Article</a>
```

Điều này tạo ra một liên kết dùng phương thức `DELETE`. Tuy nhiên, bạn nên dùng nút hoặc biểu mẫu cho bất kỳ thao tác nào không phải yêu cầu `GET`.

#### Tắt Turbo Drive

Sẽ có lúc bạn không muốn Turbo Drive kiểm soát việc điều hướng và muốn đặt lại toàn bộ trang.

Bạn có thể tắt Turbo Drive bằng cách thêm `data-turbo="false"` trực tiếp vào các liên kết hoặc vào phần tử cha chứa chúng.

Ví dụ:

```erb
<div data-turbo="false">
  <%= link_to "foo", "bar" %>
  <%= link_to "baz", "qux", data: { turbo: "true" } %>
</div>
```

Trong ví dụ trên, chúng ta tạo một div cha có `data-turbo="false"`, qua đó tắt Turbo Drive trên mọi phần tử bên trong nó. Tuy nhiên, chúng ta cũng thêm `data: { turbo: "true" }` vào `link_to` thứ hai. Điều này sẽ bật lại Turbo Drive cho riêng phần tử đó.

Ví dụ này cho thấy bạn có thể tắt và bật lại Turbo Drive trong các vùng khác nhau của trang web, đồng thời chọn những phần tử cụ thể để bật hoặc tắt nó. Hãy nhớ rằng khi dùng các thẻ ERB (chẳng hạn `link_to`), chúng ta dùng cú pháp `data: { foo: "bar" }` thay vì cú pháp HTML `data-foo="bar"`.

### Biểu mẫu

Chúng ta sẽ học sâu hơn về biểu mẫu Rails ở phần sau, nhưng vì biểu mẫu gắn liền với Turbo Drive nên cần đề cập một số thông tin phổ biến, những thông tin này sẽ kết nối với các bài học về biểu mẫu tiếp theo.

Nếu bạn chưa hiểu một vài thông tin được giải thích dưới đây thì đừng lo; hãy chờ đến bài Form Basics, mọi thứ sẽ dần sáng tỏ.

#### Gửi biểu mẫu

Trong khi hầu hết yêu cầu từ liên kết được gửi dưới dạng yêu cầu GET, biểu mẫu lại khác. Bạn thường gửi biểu mẫu dưới dạng yêu cầu POST, PATCH hoặc DELETE để thao tác với dữ liệu nằm trên máy chủ.

Theo mặc định, Turbo Drive chặn tất cả thao tác gửi biểu mẫu và thay mặt bạn gửi chúng đến máy chủ. Khi học tiếp các bài về biểu mẫu, bạn phải nhớ rằng Turbo mong máy chủ trả về mã trạng thái HTTP 303, hay nói cách khác là một chuyển hướng.

Hãy hình dung như sau: sau khi gửi biểu mẫu, thông thường bạn muốn được chuyển đến một nơi nào đó. Có thể là quay lại chính trang đó với biểu mẫu trống để gửi lần nữa, hoặc xem tài nguyên bạn vừa tạo bằng biểu mẫu; nhưng bạn mong được chuyển khỏi trang hiện tại, nơi biểu mẫu đã điền vẫn còn đó. Có hai ngoại lệ cho kỳ vọng này:

1. Khi máy chủ phản hồi bằng mã trạng thái 4XX. Điều này thường xảy ra nếu bạn gửi biểu mẫu với thông tin sai và máy chủ phản hồi bằng trạng thái 422 Unprocessable Entity.
1. Khi máy chủ phản hồi bằng mã trạng thái 5XX, trong đó phổ biến nhất là 500 Internal Server Error.

Nếu máy chủ phản hồi bằng bất kỳ trạng thái nào khác, Turbo sẽ không thể xử lý và có vẻ như chưa có gì xảy ra vì trang không cập nhật. Điều cần nhớ ở đây là trạng thái HTTP 200. Trạng thái này được trả về khi một yêu cầu thành công, vì vậy ban đầu việc Turbo không thể xử lý loại yêu cầu này có thể khá lạ.

Lý do là nếu bạn từng gửi biểu mẫu rồi làm mới trang trước khi thao tác POST hoàn tất, trình duyệt sẽ hiện hộp thoại hỏi bạn có muốn gửi lại biểu mẫu hay không. Trình duyệt làm vậy vì khi làm mới, thực ra bạn đã tạo một yêu cầu mới, và máy chủ phản hồi bằng trạng thái 200 vì yêu cầu đã được xử lý mà không gặp lỗi hay chuyển hướng. Trình duyệt xử lý trường hợp này bằng cách đề nghị gửi lại biểu mẫu dưới dạng yêu cầu POST. Vì Turbo đã chặn yêu cầu, bạn sẽ không nhận được hành vi mặc định đó của trình duyệt, còn Turbo không thể mô phỏng nó và có hai lựa chọn:

1. Nó có thể giữ nguyên URL hiện tại trong trình duyệt; hoặc
1. Nó có thể thử điều hướng đến action mà biểu mẫu gửi tới.

Nếu điều hướng đến action của biểu mẫu, nó sẽ thực hiện một yêu cầu GET mới, nhưng vì biểu mẫu đã gửi yêu cầu POST nên máy chủ có thể chưa được cấu hình để xử lý yêu cầu GET đến URL đó và bạn sẽ gặp lỗi. Vì Turbolinks không xử lý thao tác gửi biểu mẫu, đây chính là điều đã xảy ra trong các phiên bản Rails trước. Vì vậy Turbo Drive chọn phương án 1: giữ nguyên URL hiện tại trong trình duyệt, khiến người dùng có cảm giác như chưa có gì xảy ra. Do đó, bạn phải luôn bảo đảm biểu mẫu phản hồi bằng một trong các mã trạng thái hợp lệ nêu trên.

### Turbolinks

Trước khi kết thúc, chúng ta nên nói ngắn gọn về tiền thân của Turbo là Turbolinks. Turbolinks **không còn được phát triển tích cực**! Dù vậy, nó vẫn đáng được nhắc đến vì đã tồn tại khá lâu (bản phát hành lớn đầu tiên vào năm 2013), và bạn có thể gặp những ứng dụng hiện có hoặc các bài viết trực tuyến đề cập đến Turbolinks. Mục tiêu của Turbolinks là bắt các yêu cầu đối với mọi liên kết được nhấp trong ứng dụng web; nếu liên kết đó trỏ đến một trang khác trong ứng dụng web chứ không phải tài nguyên bên ngoài, Turbolinks sẽ giữ nguyên instance trang hiện tại và chỉ thay thế nội dung nằm giữa các thẻ `<body>` của tài liệu. Nó không yêu cầu tải lại toàn bộ trang (vì vậy nhanh hơn), bởi không cần tải xuống các tài nguyên trong phần head của tài liệu khi những tài nguyên đó gần như không thay đổi.

Điều này chỉ áp dụng khi điều hướng trang bằng liên kết; nó không chặn thao tác gửi biểu mẫu. Dù Turbolinks không còn được phát triển chủ động, khái niệm đứng sau nó đã phát triển thành một framework mới ngày nay được gọi là Turbo.

Nhắc lại, Turbolinks là **tiền thân** của Turbo, và **Turbo không phải cách viết tắt của Turbolinks**. Đây là hai thực thể khác nhau.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc chương 1 và 2 của [sổ tay Turbo](https://turbo.hotwired.dev/handbook/introduction). Tập trung bảo đảm bạn hiểu việc điều hướng bằng liên kết và cách nó hoạt động, vì chúng ta sẽ học về biểu mẫu sau.

</div>