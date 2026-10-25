### Giới thiệu

"Session" là khái niệm chỉ việc trạng thái của người dùng được duy trì theo một cách nào đó khi họ chuyển từ trang này sang trang khác. Hãy nhớ rằng HTTP là giao thức không lưu trạng thái, vì vậy trình duyệt hoặc ứng dụng của bạn phải đảm nhiệm việc "ghi nhớ" những gì cần được ghi nhớ.

Trong bài học này, bạn sẽ tìm hiểu về session, cookie của trình duyệt và cách xây dựng cơ chế xác thực trong Rails. Chúng ta sẽ đề cập đến cả việc tự xây dựng hệ thống xác thực và gem xác thực được sử dụng phổ biến nhất là Devise.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học này.

- Cookie là gì.
- Session là gì.
- Sự khác nhau giữa "hash" `session` và "hash" `cookies`.
- Các cách sử dụng "hash" `flash`.
- Khi nào nên dùng `flash.now` thay cho `flash`.
- Bộ lọc của controller và lý do chúng hữu ích.
- Cách chạy bộ lọc của controller chỉ cho một vài action cụ thể.
- Sự khác nhau giữa xác thực và phân quyền.
- Vì sao `#has_secure_password` là một phương thức tiện dụng.
- Tổng quan cơ bản về cách xác thực người dùng bằng phương thức đó.
- Các bước bổ sung cần thực hiện (ở mức khái quát) để thực sự "ghi nhớ" người dùng sau khi họ đóng trình duyệt.
- Gem Devise và lý do nó hữu ích.

### Cookie, session và flash

Cookie, session và flash là ba đối tượng đặc biệt mà Rails cung cấp; mỗi đối tượng hoạt động khá giống một hash. Chúng được dùng để duy trì dữ liệu giữa các request, có thể chỉ đến request kế tiếp, cho đến khi trình duyệt đóng, hoặc cho đến khi đạt thời hạn đã chỉ định. Ngoài khác biệt về thời gian tồn tại, mỗi đối tượng còn giải quyết những trường hợp sử dụng hơi khác nhau, như mô tả dưới đây.

### Cookie

Cookie là các cặp dữ liệu khóa-giá trị được lưu trong trình duyệt của người dùng cho đến khi đạt ngày hết hạn đã chỉ định. Chúng có thể được dùng cho gần như mọi thứ, phổ biến nhất là để "đánh dấu" vị trí của người dùng trên một trang web nếu họ bị ngắt kết nối, hoặc để lưu các tùy chọn hiển thị đơn giản của trang web. Bạn cũng có thể lưu thông tin giỏ hàng hoặc thậm chí mật khẩu, nhưng đó sẽ là một ý tưởng tồi -- bạn không nên lưu bất kỳ thứ gì cần được bảo mật hoặc cần tồn tại qua nhiều phiên trình duyệt trong cookie trình duyệt thông thường. Người dùng rất dễ xóa bộ nhớ đệm và/hoặc đánh cắp, thay đổi các cookie không được bảo vệ.

Để làm việc với cookie, Rails cho bạn truy cập vào một hash đặc biệt tên là `cookies`, trong đó mỗi cặp khóa-giá trị được lưu thành một cookie riêng trên trình duyệt của người dùng. Nếu bạn lưu `cookies[:hair-color] = "blonde"`, bạn có thể mở công cụ dành cho nhà phát triển của trình duyệt và thấy một cookie trên trình duyệt của người dùng có khóa là `hair-color` và giá trị là `blonde`. Hãy xóa nó bằng `cookies.delete(:hair-color)`.

Mỗi khi có request mới đến máy chủ, trình duyệt sẽ gửi kèm tất cả cookie, và bạn có thể truy cập chúng trong controller và view như một hash thông thường. Bạn cũng có thể đặt ngày hết hạn cho chúng, chẳng hạn bằng cú pháp `cookies[:name] = { value: "cookies YUM", expires: Time.now + 3600}`.

### Session

Hãy nghĩ xem các website theo dõi trạng thái đăng nhập của người dùng như thế nào khi trang được tải lại. Các request HTTP không lưu trạng thái, vậy làm sao bạn biết một request cụ thể thực sự đến từ đúng người dùng đang đăng nhập đó? Đây là lý do cookie quan trọng -- chúng cho phép bạn theo dõi người dùng từ request này sang request khác cho đến khi cookie hết hạn.

Một trường hợp đặc biệt là khi bạn muốn theo dõi dữ liệu trong "session" của người dùng, đại diện cho toàn bộ những gì người dùng thực hiện trong khoảng thời gian bạn chọn "ghi nhớ" họ, thường là cho đến khi cửa sổ trình duyệt đóng. Khi đó, mọi trang họ truy cập trước lúc trình duyệt đóng đều thuộc cùng một session.

Để nhận diện thông tin session của người dùng, Rails lưu một cookie đặc biệt, an toàn và chống giả mạo trong trình duyệt của người dùng; cookie này chứa toàn bộ hash session của họ (hãy tìm nó trong công cụ dành cho nhà phát triển, thường ở phần “Application”) và hết hạn khi trình duyệt đóng. Mỗi khi người dùng gửi request đến ứng dụng, request đó cũng tự động bao gồm cookie session này (cùng với các cookie khác), và bạn có thể dùng nó để theo dõi trạng thái đăng nhập của họ. Có thể lúc này tất cả còn hơi trừu tượng, nhưng chẳng bao lâu nữa bạn sẽ được thấy nó hoạt động thực tế.

Rails cho phép bạn truy cập hash `session` theo cách gần như giống hệt hash `cookies` đã nêu ở trên. Hãy dùng biến `session` trong view hoặc controller như sau:

```ruby
  # app/controllers/users_controller.rb
  ...
  # Set a session value
  session[:current_user_id] = user.id

  # Access a session value
  some_other_variable_value = session[:other_variable_key]

  # Reset a session key
  session[:key_to_be_reset] = nil

  # Reset the entire session
  reset_session
  ...
```

Tại sao bạn cần cả cookie lẫn session? Chúng tương tự nhưng không giống nhau. `session` là toàn bộ một hash được đặt vào cookie session an toàn, cookie này hết hạn khi người dùng đóng trình duyệt. Nếu xem trong công cụ dành cho nhà phát triển, giá trị "expiration" của cookie đó là "session". Mỗi giá trị trong hash `cookies` được lưu thành một cookie riêng.

Vì vậy, cookie và session phần nào giống những bảng cơ sở dữ liệu tạm thời, miễn phí để bạn sử dụng; chúng là duy nhất cho từng người dùng và tồn tại cho đến khi bạn xóa thủ công, chúng đạt ngày hết hạn hoặc session kết thúc (tùy theo điều bạn đã chỉ định).

#### Một số lưu ý bổ sung về session và cookie

- `session` và `cookies` thực ra không phải hash; Rails chỉ giả lập chúng như vậy để bạn dễ làm việc. Bạn vẫn có thể xem chúng là hash vì chúng hoạt động rất giống hash.
- Có giới hạn kích thước đối với lượng dữ liệu bạn có thể lưu trong hash session hoặc cookie trình duyệt (~4kb). Mức này đủ cho mọi cách sử dụng "thông thường", nhưng đừng coi chúng là vật thay thế cho cơ sở dữ liệu.

### Flash

Đến đây có lẽ bạn đã thấy và sử dụng hash `flash`, nhưng chúng ta sẽ xem lại nó dưới góc độ tìm hiểu session. `flash` là một hash đặc biệt (chính xác hơn là một phương thức hoạt động như hash), chỉ tồn tại từ request này đến request kế tiếp. Bạn có thể hình dung nó như một hash `session` tự hủy sau khi được mở. Nó thường được dùng để gửi thông báo từ controller đến view, để người dùng thấy thông báo thành công hoặc thất bại sau khi gửi form.

Nếu bạn muốn hiển thị "Cảm ơn bạn đã đăng ký!" trên trình duyệt của người dùng sau khi chạy action `#create` (action này thường dùng `redirect_to` để đưa người dùng đến một trang hoàn toàn mới khi thành công), bạn sẽ gửi thông báo thành công đó bằng cách nào? Bạn không thể dùng biến instance, vì việc redirect khiến trình duyệt gửi một request HTTP hoàn toàn mới và do đó mọi biến instance đều bị mất.

Flash xuất hiện để giải quyết việc đó! Chỉ cần lưu `flash[:success]` (hoặc đặt tên tùy ý), và nó sẽ có sẵn trong view ở request mới tiếp theo. Ngay khi view truy cập hash này, Rails sẽ xóa dữ liệu để nó không xuất hiện mỗi lần người dùng truy cập một trang mới. Thật gọn gàng và tiện lợi.

Còn trường hợp người dùng không thể đăng ký vì validation thất bại thì sao? Trong trường hợp này, action `#create` điển hình chỉ `render` action `#new` bằng các biến instance hiện có. Vì đây không phải một request hoàn toàn mới, bạn sẽ muốn thông báo lỗi có sẵn ngay lập tức. Đó là lý do hash `flash.now` rất tiện dụng, ví dụ `flash.now[:error] = "Fix your submission!"`. Giống flash thông thường, flash này cũng tự động hủy sau khi được mở.

Bạn vẫn phải viết code trong view để hiển thị các thông báo flash. Thông thường, người ta viết một view helper ngắn để ghim mọi thông báo flash hiện có lên đầu trình duyệt. Bạn cũng có thể thêm một class cho thông báo, nhờ đó viết CSS tùy chỉnh; chẳng hạn biến thông báo `:success` thành màu xanh lá và `:error` thành màu đỏ.

```erb
  # app/views/layouts/application.html.erb
  ...
  <% flash.each do |name, message| %>
    <div class="<%= name %>"><%= message %></div>
  <% end %>
```

### Bộ lọc của controller

Trước khi nói về xác thực, chúng ta cần tìm hiểu bộ lọc của controller. Ý tưởng của các bộ lọc này là chạy một đoạn code trong controller vào những thời điểm rất cụ thể, chẳng hạn trước khi bất kỳ code nào khác được chạy. Điều đó quan trọng vì nếu người dùng yêu cầu chạy một action mà họ chưa được cấp quyền, bạn cần chặn request ngay từ đầu và gửi về lỗi/redirect phù hợp trước khi họ có thể làm bất cứ điều gì khác. Về cơ bản, bạn đang "lọc bỏ" các request chưa được cấp quyền.

Chúng ta thực hiện việc này bằng một "bộ lọc trước", nhận tên của phương thức cần chạy:

```ruby
  # app/controllers/users_controller
  before_action :require_login
  before_action :do_something_cool
  ...
  private

  def require_login
    if current_user.logged_in?
    # allow the user to perform the action they wanted
    else
      redirect_to login_path
    end
  end

  def do_something_cool
    # do stuff here
  end
```

Phương thức `before_action` nhận symbol của phương thức cần chạy trước mọi thứ khác trong controller. Nếu callback này render hoặc redirect, request cũng như mọi callback được lên lịch chạy sau callback đó đều bị hủy. Vì vậy, trong ví dụ trên, nếu người dùng bị redirect đến trang đăng nhập thì callback `before_action :do_something_cool` cũng sẽ không được thực thi.

Bạn có thể chỉ áp dụng bộ lọc cho các action cụ thể bằng cách chỉ định tùy chọn `only`, ví dụ `before_action :require_login, only: [:edit, :update]`. Điều ngược lại được thực hiện bằng tùy chọn `:except`... bộ lọc sẽ chạy cho mọi action trừ những action được chỉ định.

Bạn nên đặt các phương thức bộ lọc phía sau khai báo `private` để chúng chỉ có thể được controller đó sử dụng.

Cuối cùng, các bộ lọc được kế thừa. Vì vậy, nếu muốn một bộ lọc áp dụng cho tuyệt đối mọi action của controller, hãy đặt nó trong file `app/controllers/application_controller.rb`.

### Xác thực

Mục đích cốt lõi của xác thực là bảo đảm người dùng đúng là người họ tự nhận. Cách quản lý tiêu chuẩn là cho người dùng đăng nhập qua form đăng nhập. Sau khi người dùng đăng nhập, bạn theo dõi người dùng đó bằng session cho đến khi họ đăng xuất.

Một khái niệm liên quan là phân quyền. Đúng, bạn có thể đã đăng nhập, nhưng liệu bạn có thực sự được phép truy cập thứ mình đang cố truy cập không? Ví dụ điển hình là sự khác nhau giữa người dùng thông thường và người dùng quản trị. Cả hai đều được xác thực với hệ thống, nhưng chỉ quản trị viên được phép thay đổi một số thứ.

Xác thực và phân quyền đi đôi với nhau -- trước hết bạn xác thực ai đó để biết họ là ai và kiểm tra xem họ có được phép xem một trang hoặc thực hiện một action hay không. Khi xây dựng ứng dụng, bạn sẽ có một hệ thống xác thực để đăng nhập người dùng và xác minh họ đúng là người họ tự nhận. Bạn phân quyền cho người dùng thực hiện một số việc nhất định (chẳng hạn xóa dữ liệu) dựa trên những phương thức được bảo vệ bởi các bộ lọc controller yêu cầu đăng nhập hoặc quyền hạn cao hơn (ví dụ trạng thái quản trị viên).

### Xác thực Basic và Digest

Nếu bạn đang tìm một cách xác thực người dùng rất đơn giản và thiếu an toàn, có thể dùng xác thực HTTP Basic. Chúng ta sẽ không đi vào chi tiết ở đây, nhưng về cơ bản nó gồm việc gửi tên người dùng và password vào một form rồi truyền chúng (không mã hóa) qua mạng. Bạn dùng phương thức `#http_basic_authenticate_with` để thực hiện việc đó (xem phần đọc thêm để biết ví dụ) và giới hạn quyền truy cập vào một số controller khi không có xác thực này.

Để có hệ thống xác thực an toàn hơn một chút (trên HTTP), hãy dùng HTTP Digest Authentication. Chúng ta cũng sẽ không trình bày chi tiết về nó. Cơ chế này dựa vào một `#before_action` chạy một phương thức gọi `#authenticate_or_request_with_http_digest`, phương thức này nhận một block phải trả về password "đúng" lẽ ra người dùng cần cung cấp.

Vấn đề của cả hai cách này là chúng ghi cứng tên người dùng và password trong controller (hoặc ở đâu đó), nên thực chất chỉ là giải pháp tạm thời.

### Tự xây dựng hệ thống xác thực

Nếu muốn có chức năng đăng nhập người dùng, bạn sẽ phải trải qua thêm vài bước. Cần nhấn mạnh rằng bạn không bao giờ nên tự xây dựng hệ thống xác thực, vì đã có những giải pháp được kiểm chứng kỹ lưỡng mà bạn có thể dùng trong dự án. Devise là một trong số đó và chúng ta sẽ tìm hiểu sau. Tuy vậy, biết một vài nguyên tắc vẫn rất hữu ích.

Trước hết, chúng ta không lưu password dạng văn bản thuần trong cơ sở dữ liệu. Làm vậy chẳng khác nào tự chuốc rắc rối (bạn đã đọc bao nhiêu tin về các website lớn bị hack và password bị lộ dưới dạng văn bản thuần?). Thay vào đó, bạn sẽ lưu phiên bản "password digest" đã được mã hóa của password.

Khi người dùng gửi password qua form đăng nhập, thay vì so sánh trực tiếp với phiên bản dạng văn bản thuần của password đó, bạn thực sự chuyển password được gửi sang dạng digest. Sau đó, bạn so sánh digest mới này với digest đã lưu trước đó của người dùng. Nếu chúng khớp nhau, bạn đã có một người dùng đang đăng nhập.

Cách này tốt hơn nhiều vì digest là dạng mã hóa một chiều. Bạn có thể dễ dàng tạo digest từ một chuỗi password, nhưng việc giải mã digest để lấy lại password gốc cực kỳ khó. Cách hiệu quả nhất để phá một loạt digest chỉ là lập một danh sách khổng lồ các password có thể có, chuyển chúng thành digest rồi xem các digest đó có khớp với digest bạn đang cố phá hay không (tức là đoán-và-kiểm tra trên quy mô lớn).

Rails không bắt bạn tự làm mọi thứ. Nó có một phương thức tên là `#has_secure_password`; bạn chỉ cần thêm phương thức này vào model User và nó sẽ bổ sung nhiều chức năng bạn cần. Để dùng phương thức tiện lợi đó, về cơ bản bạn thiết lập model User để tiếp nhận các thuộc tính `password` và `password_confirmation`, nhưng thực tế không lưu chúng vào cơ sở dữ liệu. `has_secure_password` chặn các giá trị đó và chuyển chúng thành password digest giúp bạn.

Để khởi tạo một session mới cho người dùng (khi người dùng đăng nhập), bạn sẽ cần tạo một controller mới (thường là `sessions_controller.rb`) cùng các route tương ứng cho `:new`, `:create` và `:destroy`. Nếu người dùng cung cấp thông tin xác thực đúng (có thể kiểm tra bằng phương thức `#authenticate`), bạn sẽ dùng biến `session` để lưu ID của họ, rồi dùng ID đó xác nhận họ đúng là người họ tự nhận. Đây là cách xác thực người dùng dựa trên hạ tầng session sẵn có của Rails, nhưng chỉ kéo dài chừng nào session còn tồn tại.

Nếu người dùng muốn được "ghi nhớ" (có lẽ bạn đã nhiều lần thấy checkbox "ghi nhớ tôi" trong các form đăng nhập), bạn cần một cách ghi nhớ họ lâu hơn thời lượng của session trình duyệt. Để làm vậy, bạn cần tạo thêm một cột trong bảng Users cho `remember_token` được mã hóa (hoặc tên tùy ý). Bạn sẽ dùng cột đó để lưu một chuỗi ngẫu nhiên cho người dùng, được dùng về sau để nhận diện họ.

Bạn sẽ đưa token chưa mã hóa vào trình duyệt của người dùng dưới dạng cookie vĩnh viễn (bằng `cookies.permanent[:remember_token]`). Cookie đó sẽ được gửi cùng mỗi request mới, vì vậy mỗi khi người dùng tạo request, bạn có thể đối chiếu nó với phiên bản đã mã hóa trong cơ sở dữ liệu để biết người dùng là ai. Về cơ bản, đây là phiên bản rõ ràng và lâu dài hơn của việc Rails đang làm với session. Một thông lệ tốt là đặt lại token sau mỗi lần đăng nhập mới nếu người dùng đăng xuất.

Thông thường, bạn nên tạo một số phương thức helper để xử lý các hành vi phổ biến như đăng nhập người dùng, kiểm tra xem người dùng đã đăng nhập chưa và so sánh người dùng hiện đang đăng nhập với một người dùng khác (hữu ích khi người dùng hiện tại đang xem trang của người dùng khác và không nên thấy liên kết để "chỉnh sửa" trang đó). Những phương thức này sẽ giúp cuộc sống của bạn dễ dàng hơn nhiều vì bạn có thể tái sử dụng chúng trong bộ lọc controller, view hoặc thậm chí cả test.

Tổng quan từng bước:

1. Thêm một cột vào bảng Users để chứa `password_digest` của người dùng.
1. Khi người dùng đăng ký, chuyển password họ gửi sang dạng digest rồi lưu CHÍNH digest đó vào cột cơ sở dữ liệu mới bằng cách thêm phương thức `has_secure_password` vào model User.
1. Đừng quên mọi validation cần thiết về độ dài của password và password confirmation.
1. Xây dựng một sessions controller (và các route tương ứng), rồi dùng phương thức `#authenticate` để đăng nhập người dùng khi họ đã gửi đúng thông tin xác thực bằng form đăng nhập.
1. Cho phép ghi nhớ người dùng bằng cách tạo một cột `remember_token` trong bảng Users và lưu token đó thành cookie vĩnh viễn trong trình duyệt của người dùng. Đặt lại sau mỗi lần đăng nhập mới.
1. Với mỗi lần tải trang yêu cầu xác thực (và dùng một `#before_action` trong các controller phù hợp), trước tiên kiểm tra cookie `remember_token` của người dùng với cơ sở dữ liệu để xem họ đã đăng nhập chưa. Nếu chưa, redirect đến trang đăng nhập.
1. Tạo các phương thức helper khi cần để dễ dàng xác định người dùng đã đăng nhập hay chưa, hoặc so sánh một người dùng khác với người dùng hiện đang đăng nhập.
1. Hưởng lợi.

### Devise

Devise là một gem được xây dựng để xử lý toàn bộ những việc này giúp bạn. Về lâu dài, nó tốt hơn việc tự xây dựng xác thực vì đã bao quát nhiều trường hợp biên và lỗ hổng bảo mật mà bạn có thể không nghĩ đến. Devise cho phép bạn tích hợp với các hệ thống xác thực nâng cao hơn để giao tiếp với những API như OAuth. Vì vậy, về sau nó sẽ khá hữu ích.

Nói ngắn gọn, Devise đóng gói sẵn một loạt form và phương thức đăng nhập, đăng ký để hỗ trợ bạn triển khai chúng. Nó gồm 10 module (và bạn có thể chọn những module muốn dùng). Bạn cài gem `devise` rồi chạy trình cài đặt để đưa các file của Devise vào ứng dụng. Bạn cũng cần chạy migration cơ sở dữ liệu để thêm các trường bổ sung của Devise vào bảng Users.

Cấu hình sẽ phụ thuộc vào trường hợp sử dụng của bạn. Bạn có muốn người dùng xác nhận việc đăng ký qua email (module `Confirmable`) không? Bạn có muốn cho phép người dùng đặt lại password (module `Recoverable`) không?

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết về [cách session Rails hoạt động](https://www.justinweiss.com/articles/how-rails-sessions-work/).
1. Xem video để [tìm hiểu sâu về session](https://www.youtube.com/watch?v=mqUbnZIY3OQ).
1. Đọc phần 5 và 6 trong [Rails Guides về Controller](http://guides.rubyonrails.org/action_controller_overview.html#cookies). Hiện tại, bạn không cần quá lo lắng về chi tiết cấu hình `session_store` trong mục 6.4.
1. Đọc phần 7 trong [Rails Guides về Controller](https://guides.rubyonrails.org/action_controller_overview.html#controller-callbacks) để hiểu về bộ lọc controller.
1. Đọc phần 4 trong [Rails Guides về các chủ đề nâng cao của Controller](https://guides.rubyonrails.org/action_controller_advanced_topics.html#http-authentication) để hiểu thêm về xác thực.
1. Xem qua [Tài liệu Devise](https://github.com/plataformatec/devise). Đọc về cách cài đặt nó trong ứng dụng Rails và chức năng của các module khác nhau. Bạn sẽ dùng nó trong những dự án sắp tới.

</div>

#### Kết luận

Xác thực có vẻ là một chủ đề khá phức tạp -- có rất nhiều thành phần cùng vận hành. Tuy nhiên, về cốt lõi, bạn chỉ đang kiểm tra xem người tạo request có thực sự là người dùng đã đăng nhập và có quyền thực hiện việc đó hay không, thông qua một hình thức cookie trình duyệt nào đó.

Bài học này hẳn đã giúp bạn phần nào hiểu được các hệ thống đăng nhập có thể phức tạp đến đâu, đồng thời vén bức màn về những website bạn đã truy cập vô số lần. Xác thực không phải khoa học tên lửa, và chẳng bao lâu nữa bạn sẽ tích hợp nó vào các ứng dụng của chính mình.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ trợ.

- [Xác thực trong Rails 3.1 từ Railscasts](http://railscasts.com/episodes/270-authentication-in-rails-3-1)... hay hơn cách chúng ta có thể giải thích.
- [Tất cả về Cookie (.org)](http://www.allaboutcookies.org/)
