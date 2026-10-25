### Giới thiệu

Trong bài học này, trước tiên bạn sẽ có cơ hội khám phá một API có sẵn của Flickr. Bạn sẽ cần đọc tài liệu để hiểu nên thực hiện những lời gọi nào, nhưng Flickr có một công cụ khám phá API khá tiện, giúp gửi các lời gọi API thay cho bạn.

Sau đó, bạn sẽ xây dựng một ứng dụng Rails đơn giản của riêng mình, hoạt động như một API RESTful.

### Khám phá API Flickr

<div class="lesson-content__panel" markdown="1">

1. Truy cập [Trang tài liệu API Flickr](http://www.flickr.com/services/api/). Bạn chỉ cần Google `XYZ API docs` để tìm những trang này; cách đó thường nhanh và dễ hơn nhiều so với việc tự lần theo menu trên các website.
1. Xem qua các method khác nhau hiện có. Flickr cung cấp một vài định dạng request khác nhau, nhưng hãy xem định dạng API RESTful bằng cách nhấp vào liên kết [REST](http://www.flickr.com/services/api/request.rest.html) ở bên trái, dưới mục "Request Formats".
1. Trang này cho thấy cấu trúc của một lời gọi API điển hình -- bạn sẽ gửi request đến endpoint `https://www.flickr.com/services/rest/` và đưa mọi dữ liệu bắt buộc vào query string của GET hoặc body của POST.
1. Xem qua các method khác nhau. Bạn sẽ upload một bức ảnh như thế nào? Còn lấy danh sách liên hệ thì sao? Nhiều method trong số này sẽ yêu cầu bạn xác thực ứng dụng hoặc tài khoản người dùng trước.
1. Xem [tài liệu về method Search](http://www.flickr.com/services/api/flickr.photos.search.html). Request này không yêu cầu xác thực, bạn chỉ cần cung cấp API key (lấy được bằng cách đăng ký làm developer trên nền tảng của họ). Hãy xem qua tất cả các argument có thể sử dụng.
1. Nếu kéo xuống cuối tài liệu Search, bạn sẽ thấy liên kết đến [Search API Explorer](http://www.flickr.com/services/api/explore/flickr.photos.search). Công cụ này cho phép bạn thực sự thực thi các request bằng API key của Flickr cho mục đích minh họa. Bạn chỉ cần nhập các tùy chọn mong muốn, rồi công cụ sẽ thực hiện request thay bạn. Hãy thử nhập "puppies" vào argument "tags", sau đó cuộn xuống cuối trang. Đổi menu "Output" của response thành JSON rồi nhấp vào "Call Method".
1. Khi trang tải lại, bạn sẽ thấy kết quả ở phía dưới. Bạn sẽ thấy một danh sách lớn các object ảnh (sau một ít metadata) được trả về từ lần tìm kiếm. Chúng trông như sau:

    ```json
    { "id": "11357337313", "owner": "84645040@N00", "secret": "6dd795c9c6", "server": "3805", "farm": 4, "title": "Gavin-Feb2013-0127", "ispublic": 1, "isfriend": 0, "isfamily": 0 },
    ```

    Điều thú vị hơn là ngay bên dưới, bạn có thể thấy URL được dùng để thực hiện request. Hãy tách URL đó ra ở đây để làm rõ hơn các parameter:

    ```bash
    https://www.flickr.com/services/rest/
    ?method=flickr.photos.search
    &api_key=bbee7f1e3a3f9cb847b87964d50bf4bc
    &tags=puppies
    &format=json
    &nojsoncallback=1
    &api_sig=d207eb20abbce7c40437a01f759e1388
    ```

1. URL chứa endpoint REST mà chúng ta đã xem trước đó, cùng với query tìm kiếm và một số tùy chọn khác như API key và format. Nếu copy rồi dán URL đó vào trình duyệt, bạn sẽ thấy cùng một nhóm output.
1. Tìm hiểu thêm về [cấu trúc URL ảnh của Flickr](http://www.flickr.com/services/api/misc.urls.html).
1. API của Flickr cần hai bước để thực sự hiển thị một bức ảnh -- bạn cần lấy thông tin metadata của ảnh (thứ chúng ta vừa nhận được trong kết quả tìm kiếm), rồi ghép thông tin đó thành một URL mà Flickr hiểu được để thực sự lấy ảnh về. Định dạng mà họ gọi là điển hình là:

    `https://live.staticflickr.com/{server-id}/{id}_{secret}_{size-suffix}.jpg`

    Chúng ta có thể điền các giá trị từ bức ảnh vừa lấy được để hiển thị ảnh:

    `https://live.staticflickr.com/3805/11357337313_6dd795c9c6.jpg`

    Kết quả trông như sau:

    ![Một chú chó trong tuyết](https://live.staticflickr.com/3805/11357337313_6dd795c9c6.jpg)

    Như bạn thấy, bỏ qua `_{size-suffix}` vẫn hoạt động và mặc định cạnh dài nhất có kích thước 500px.

1. Vậy là xong! Mỗi API đều khác nhau, và bạn phải đọc tài liệu của chúng để hiểu định dạng cơ bản khi sử dụng. Đôi khi bạn cũng có thể tìm một video [YouTube](http://www.youtube.com) hoặc [NetTuts](http://code.tutsplus.com) trình bày nhanh về chủ đề này.

</div>

### Xây dựng API kittens

Đây là một project nhanh và đơn giản, trong đó bạn sẽ thiết lập một ứng dụng Rails đóng vai trò API cung cấp dữ liệu... nói cách khác, tất cả method của controller sẽ render dữ liệu thay vì HTML. Hãy coi đây là bài luyện tập để nhanh chóng xây dựng một resource RESTful thuần túy. Chúng ta sẽ chưa làm việc với API bên ngoài cho đến project tiếp theo.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Chúng ta sẽ bắt đầu bằng cách xây dựng ứng dụng Kitten để hoạt động bình thường trong trình duyệt với HTML.

   1. Thiết lập một ứng dụng Rails mới (`odin-kittens`) và Git repo.
   1. Cập nhật README để mô tả ứng dụng và liên kết trở lại project này.
   1. Xây dựng model Kitten với các thuộc tính `:name`, `:age`, `:cuteness` và `:softness`.
   1. Xây dựng KittensController và các route `:kittens` cho đủ 7 action RESTful.
   1. Đặt route mặc định thành `kittens#index`.
   1. Hoàn thiện từng action của controller và view tương ứng để hiển thị một trang HTML thật cơ bản -- `#index` chỉ cần liệt kê tất cả Kitten, `#show` hiển thị một Kitten, `#new` render form tạo Kitten, `#edit` dùng cùng form đó (form này nên là một partial được dùng bởi cả view New và Edit) để chỉnh sửa Kitten, còn `#create` và `#update` thực hiện đúng chức năng của chúng.
   1. Tạo một liên kết `delete` trên các trang Show và Edit của Kitten, đồng thời đặt một liên kết bên cạnh mỗi Kitten được liệt kê trên trang Index.
   1. Hiển thị hash `flash`, trong đó chúc mừng bạn khi thêm, chỉnh sửa hoặc xóa kitten, đồng thời trêu bạn khi form có lỗi.
   1. Kiểm thử công cụ tạo Kitten để đảm bảo tất cả action của controller đều chạy đúng.

1. Bây giờ là lúc cung cấp resource Kittens thông qua API.

   1. Thêm gem HTTParty vào project và mở Rails console. Chúng ta sẽ dùng gem `HTTParty` để gửi request đến ứng dụng:

      ```bash
      bundle add httparty
      rails console
      ```

      Sau đó, trong console:

      ```ruby
      response = HTTParty.get("http://localhost:3000/kittens")
      ```

   1. Hãy xem chúng ta nhận lại được gì:

      ```ruby
      response.body #=> Should return a sloppy mess of HTML.
      # alternatively, you can do this:
      response.to_s
      ```

      Nếu kiểm tra output của server, có lẽ request đang được xử lý dưới dạng \*/\* (tức là mọi media type), chẳng hạn `Processing by KittensController#index as */*`
   1. Hãy thử yêu cầu cụ thể một response JSON bằng cách thêm tùy chọn headers:

      ```ruby
      json_response = HTTParty.get("http://localhost:3000/kittens", headers: { 'Accept' => 'application/json' })
      ```

      Nhiều khả năng bạn sẽ nhận lỗi 406 Not Acceptable - hãy kiểm tra console của server và bạn sẽ thấy ActionController báo UnknownFormat cho controller của mình.
   1. Bây giờ hãy sửa method `#index` của KittenController để `#respond_to` JSON và render các biến phù hợp.
   1. Kiểm thử bằng cách đảm bảo các lời gọi HTTParty trả về đúng chuỗi JSON, ví dụ:

      ```ruby
      json_response = HTTParty.get("http://localhost:3000/kittens", headers: { 'Accept' => 'application/json' })
      puts json_response.body
      ```

   1. Làm tương tự với method `#show`, nhưng bạn sẽ phải cung cấp ID khi thực hiện request. Cơ chế bảo vệ CSRF sẽ ngăn bạn tạo, cập nhật hoặc xóa kitten thông qua API, vì vậy không cần triển khai những thao tác đó.

Giờ đây bạn có một website vừa là backend tạo HTML thông thường, vừa là một API có thể được dùng để lấy dữ liệu. Bạn có thể dùng các lời gọi JavaScript từ frontend để làm mới dữ liệu động, hoặc thậm chí tải toàn bộ trang ngay từ đầu. Hoặc có thể bạn sẽ kết nối một ứng dụng Kittens với iPhone và cần một backend. Điều đó không quan trọng, vì giờ bạn đã có một API RESTful.

</div>