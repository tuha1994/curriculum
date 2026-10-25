### Giới thiệu

Để thực sự hiểu Rails hoạt động như thế nào, bạn cần có nền tảng vững chắc về phần cốt lõi của web. Bạn đã học một số nội dung này trong các phần trước (và nếu đã học qua chương trình Ruby thì bạn chỉ cần đọc lướt), nhưng lần này bạn sẽ có cơ hội (trong dự án) tự mình thực hiện các request web thực tế.

### Tổng quan bài học

Phần này trình bày tổng quan các chủ đề bạn sẽ học trong bài học.

- Những kiến thức cơ bản về HTTP.
- 4 HTTP verb được sử dụng phổ biến nhất.
- 7 route RESTful của Rails.
- Các thành phần khác nhau của một URL.
- Những kiến thức cơ bản về MVC.
- API là gì.
- “Cookie” và “session” là gì.
- Sự khác nhau giữa “xác thực” và “phân quyền”.

### HTTP

HTTP chỉ đơn giản là cách tổ chức cuộc trao đổi request và response giữa trình duyệt với server. Thực ra đây thậm chí không phải là một cuộc trò chuyện, vì nó stateless... đúng hơn là kiểu “yêu cầu và nhận lại”. Protocol quy định đoạn trao đổi ngắn này sẽ diễn ra như thế nào.

Hãy xem các tài nguyên sau:

1. Bài [viết của tutsplus về HTTP](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177) mô tả những gì diễn ra với HTTP.
1. [Công cụ sniffer](http://testuri.org/sniffer) này - hãy thử tự truy xuất một vài website (chẳng hạn như `https://www.theodinproject.com/`).
1. Video hướng dẫn [kiểm tra hoạt động mạng bằng Chrome DevTools](https://www.youtube.com/watch?v=e1gAyQuIFQo) giúp bạn hiểu cách kiểm tra các tài nguyên web được truyền qua HTTP. Lưu ý rằng giao diện Chrome DevTools đã thay đổi kể từ khi video được thực hiện, nhưng phần lớn bạn vẫn có thể làm theo vì các tính năng cốt lõi vẫn giữ nguyên. Hãy dùng `https://www.theodinproject.com` để làm theo thay vì website `devtools.glitch.me` được sử dụng trong video, vì website đó không còn hoạt động.

Một thành phần quan trọng cần chú ý là cả request và response đều có phần header và (thường là) body. Header chứa thông tin về chính request hoặc response đó (siêu dữ liệu), bao gồm website cần gửi đến hoặc trả về và trạng thái của response. Body của request có thể chứa dữ liệu được gửi từ form, cookie hoặc token xác thực, trong khi response thường chứa trang HTML mà bạn đang cố truy cập.

Thành phần quan trọng khác là mỗi request sử dụng một trong bốn “verb” chính -- GET, POST, PUT và DELETE. Ngày nay, bạn gần như chỉ thấy request GET và POST (ngay cả khi muốn xóa thứ gì đó, người ta thường giả lập thao tác này bằng request POST), nhưng hiểu được sự khác nhau giữa các verb vẫn rất quan trọng.

### REST

REST (viết tắt của Representational state transfer) là một thuật ngữ bạn sẽ gặp đi gặp lại vì đây là một ý tưởng rất mạnh mẽ. Về cơ bản, REST nói rằng thường chỉ có 7 loại thao tác bạn muốn thực hiện với một resource riêng lẻ qua web, và bạn có thể thực hiện chúng bằng cách kết hợp các HTTP verb vừa tìm hiểu. “Resource” thường có nghĩa là một “thứ” trong cơ sở dữ liệu hoặc một data model của bạn. Trong trường hợp này, hãy giả sử resource đó là một model Post của blog mà bạn đã thiết lập:

1. GET tất cả bài viết (hay còn gọi là **“index”** các bài viết)
1. GET chỉ một bài viết cụ thể (hay còn gọi là **“show”** bài viết đó)
1. GET trang cho phép bạn tạo bài viết mới (hay còn gọi là xem trang **“new”** bài viết)
1. POST dữ liệu bạn vừa điền cho bài viết mới trở lại server để server có thể tạo bài viết đó (hay còn gọi là **“create”** bài viết)
1. GET trang cho phép bạn chỉnh sửa một bài viết hiện có (hay còn gọi là xem trang **“edit”** bài viết)
1. PUT (hoặc PATCH) dữ liệu bạn vừa điền khi chỉnh sửa bài viết trở lại server để server thực sự cập nhật bài viết (hay còn gọi là **“update”** bài viết)
1. DELETE một bài viết cụ thể bằng cách gửi request xóa đến server (hay còn gọi là **“destroy”** bài viết)

Các từ được nhấn mạnh tương ứng với những controller action tiêu chuẩn của Rails!

Điều này quan trọng vì nó cho bạn một cách rất có tổ chức để suy nghĩ về các resource. Đây là cách mô hình hóa request, và đó phải là cách DUY NHẤT để thực hiện các request ấy (ví dụ: bạn không nên thực sự gửi dữ liệu chỉnh sửa bài viết bằng request GET... việc đó phải dùng POST). Nếu gặp khó khăn khi hình dung bảy tình huống này (hoặc ít nhất một phần trong số đó) áp dụng cho resource bạn muốn tạo trong cơ sở dữ liệu như thế nào, có thể bạn cần suy nghĩ lại về cách thiết lập dữ liệu của mình.

Điều này cũng quan trọng vì Rails được xây dựng để tuân theo các quy ước này theo một cách rất trực tiếp. Miễn là bạn thực hiện những action đó, mọi việc sẽ rất dễ dàng và request nhận được từ trình duyệt có thể được định tuyến dễ dàng qua phần lõi của Rails.

Ban đầu, cách suy nghĩ này có thể có vẻ đơn giản hóa quá mức, nhưng khi data model của bạn trở nên phức tạp hơn một chút, bạn sẽ thấy việc quay về tư duy RESTful có thể giúp tháo gỡ các mối rối.

### URL

Bạn có thể nghĩ mình biết URL chứa những gì, nhưng đâu là host? protocol (hay scheme)? parameters? path?

Hãy xem bài viết của Matt Cutts về [cách các nhân viên Google phân tích từng thành phần của URL](http://www.mattcutts.com/blog/seo-glossary-url-definitions/).

#### Câu hỏi nhanh

URL là: <https://www.google.com/search?q=what+is+a+url>

1. “Path” là gì?
1. Phần “Parameter” là gì?
1. “Top Level Domain” là gì?
1. “Protocol” là gì?

Khi đã hiểu các thành phần này là gì, bạn có thể dễ dàng sử dụng các thư viện của Ruby để giúp xây dựng và gửi request của riêng mình. Khi dùng Rails, bạn cũng sẽ liên tục gặp lại những phần cụ thể như “path” và “parameters”.

Đáp án:

1. /search
1. q=what+is+a+url
1. com
1. https

### MVC

Bạn đã nghe về nó hết lần này đến lần khác, nhưng bạn có thực sự biết MVC là gì không? Errrrmmmmm, ummm....

MVC xoay quanh việc tổ chức, còn Rails xoay quanh MVC. Khi xây dựng một project Rails mới, bạn sẽ có cả một khối khổng lồ các thư mục và tệp được tạo ra. Dù số lượng tệp bên trong thư mục `app` có vẻ áp đảo, chúng được tổ chức rất chặt chẽ và được tạo ra cụ thể để tách biệt Model, View và Controller.

Mục đích của MVC là chia các chức năng của một ứng dụng web thành những phần tương đối độc lập. Mỗi phần có class Ruby riêng. Điều đó rất có lợi cho bạn với tư cách developer, vì khi muốn chỉnh sửa một phần cụ thể của code base hoặc sửa một bug, bạn biết chính xác cần sửa tệp nào và nó nằm ở đâu.

### Luồng đi qua MVC

Ở mức cơ bản nhất, khi một request từ trình duyệt đi vào ứng dụng:

1. Router xác định controller nào sẽ nhận request (ví dụ: với blog của bạn là controller Posts).
1. Controller đó yêu cầu model (ví dụ: model Post) cung cấp dữ liệu và trả lời mọi câu hỏi hóc búa khác mà nó có.
1. Sau đó controller chuyển mọi dữ liệu cần thiết cho các view (ví dụ: `index.html.erb`), về cơ bản là những template HTML đang chờ các biến đó.
1. Khi view phù hợp đã được bơm đầy đủ dữ liệu cần thiết (chẳng hạn tên người dùng hiện tại), nó được gửi trở lại client đã tạo request ban đầu. Xong!

betterexplained.com có [phần giải thích sâu hơn về MVC](http://betterexplained.com/articles/intermediate-rails-understanding-models-views-and-controllers/).

Nếu mô tả sơ lược ba thành phần này, model là mọt sách siêu thông minh ở phòng phía sau, controller là người trung gian xã giao nói chuyện với tất cả mọi người nhưng không thực hiện việc gì quá nặng (trong những trường hợp đó, nó hỏi model), còn view chỉ cần trông đẹp mắt và chờ controller đưa cho mình “trang phục”.

Cứ tạm làm quen với cách này, bạn sẽ thấy nó hoạt động và học được cách yêu thích nó.

### API

Khi máy tính hoặc server (mà bạn đang lập trình) muốn gửi request đến một website khác, nó không bận tâm đến việc nhấp vào các thứ trong trình duyệt; nó trực tiếp yêu cầu website kia cung cấp dữ liệu bằng API của website đó. API chỉ đơn giản là một interface. Trình duyệt web của chúng ta đi qua cửa trước để hiển thị một loạt thông tin từ Facebook, còn web server đi qua cửa bên để lấy chính dữ liệu đó (nhanh hơn và trực tiếp hơn nhiều) thông qua API.

Bạn muốn lấy dữ liệu từ Google Maps để hiển thị trên trang web của mình? Bạn gọi API của nó theo các quy tắc được nêu trong tài liệu API. Hầu như mọi website lớn đều cung cấp một phần dữ liệu của mình thông qua API, và bạn cũng có thể dễ dàng làm điều tương tự bằng Rails.

Hãy xem phần giải thích này (chỉ phần đầu tiên) về [API là gì](http://money.howstuffworks.com/business-communications/how-to-leverage-an-api-for-conferencing1.htm) từ howstuffworks.

Không phải API nào cũng dựa trên web. Nhiều API sử dụng cùng định dạng HTTP nhưng thực sự chỉ được thiết kế để truyền dữ liệu giữa các service. Trên thực tế, đó là cách các thành phần của Rails được kết nối với nhau -- chúng dùng HTTP để giao tiếp.

Sau này một chút, bạn sẽ có cơ hội tự xây dựng API, và Rails khiến việc đó rất dễ dàng. Không có gì thần kỳ cả -- bạn chỉ cần nói với controller rằng bạn muốn phản hồi các request do server khác gửi đến thay vì (hoặc bên cạnh) các request web thông thường, rồi chỉ định chính xác dữ liệu bạn muốn trả về (vì có lẽ nó sẽ không phải là một view HTML).

Nhưng chúng ta đang đi hơi xa ở đây... điểm chính là bạn sẽ thấy “API” xuất hiện rất nhiều lần, và nó hoàn toàn không đáng sợ; nó chỉ có nghĩa là cách hai ứng dụng giao tiếp với nhau.

### Cookie

Bạn đã nghe nói về cookie. Về cơ bản, cookie là cách để website ghi nhớ bạn là ai từ request này sang request khác. Hãy nhớ -- mỗi request HTTP hoàn toàn độc lập với các request khác. Nghĩa là khi bạn vào trang Home của một website rồi nhấp vào liên kết đến trang About, web server sẽ xem bạn như một người dùng hoàn toàn mới.

...Trừ khi website đã cấp cho bạn cookie (điều gần như chắc chắn đã xảy ra). Cookie là những mẩu dữ liệu nhỏ mà trình duyệt gửi đến website mỗi lần bạn request đến đó. Từ góc nhìn của web server, cookie cho phép server nhận ra bạn chính là người đã thực hiện bất kỳ request nào trong chuỗi các request trước đó. Nó duy trì *trạng thái* của session.

Hãy xem [allaboutcookies.org](http://www.allaboutcookies.org/) và đọc ba phần đầu để biết thêm thông tin về cookie.

[Bài viết trên Wikipedia về cookie](http://en.wikipedia.org/wiki/HTTP_cookie) cũng có thể hữu ích.

Hãy truy cập một website bạn thường dùng, mở developer tools và tìm các cookie. Trong Chrome, bạn có thể làm vậy bằng cách nhấp vào tab “Application”, sau đó chọn “cookies” ở menu ngoài cùng bên trái. Bạn sẽ thấy chúng dưới dạng các cặp name-value. Thường sẽ có một biến như “user_session” hoặc “token” chứa một chuỗi ký tự không thể hiểu nổi.

### Session

Cookie quan trọng vì chúng cho phép bạn có một “session” liên tục trong suốt quá trình tương tác với website. Điều đó có nghĩa là bạn chỉ cần đăng nhập một lần thay vì đăng nhập trong từng request (điều mà có lẽ bạn từng trải qua với một website bị lỗi nào đó vào cuối những năm 90).

Trình duyệt gửi toàn bộ cookie mà một website cụ thể đã thiết lập cùng với request thông thường, và server dùng các chuỗi đó để xác định bạn là người dùng nào, bạn đã đăng nhập hay chưa, cài đặt của bạn là gì (chẳng hạn bạn đã thiết lập tùy chọn hiển thị ra sao), v.v. Đây cũng là lý do khi bạn xóa cookie khỏi lịch sử trình duyệt, mọi thứ dường như bị xóa sạch và trở về mặc định.

Đó cũng là cách một số quảng cáo dường như bám theo bạn từ website này sang website khác -- một tên gọi khác của chúng là “tracking cookie”.

### Xác thực

Ở phía server, bạn sẽ tương tác khá nhiều với cookie và biến session. Như đã đề cập ở trên, một trong những công dụng chính của chúng là xác định người dùng là ai, hay “xác thực”. Về cơ bản, bạn sẽ lấy cookie mà người dùng gửi cho mình, dùng nó để tìm người dùng đó trong cơ sở dữ liệu, rồi (nếu người dùng tồn tại) hiển thị trang web được tùy chỉnh cho người dùng ấy.

Về lý thuyết, việc này khá đơn giản, nhưng một số vấn đề bảo mật có thể trở nên phức tạp, nên may mắn là có những người tốt bụng đã tạo ra một [gem tiện dụng có tên Devise](https://github.com/heartcombo/devise) để xử lý toàn bộ việc này cho bạn. Trong chương trình học này (ở phần sau), bạn sẽ tự tạo hệ thống xác thực của riêng mình trước khi học cách dùng Devise để đảm nhiệm phần việc nặng nhọc.

### Phân quyền

Phân quyền là khái niệm đi đôi với xác thực... Xác thực cho phép bạn xác định người dùng là ai, còn ý tưởng đằng sau phân quyền là bạn có thể giới hạn những gì người đó được xem dựa trên cấp quyền của họ. Trường hợp phổ biến nhất thực ra là sự khác biệt giữa một người dùng ngẫu nhiên chưa đăng nhập và một người đã đăng nhập. Một trường hợp phổ biến khác là sự khác biệt giữa người dùng thông thường của website và người dùng admin có các đặc quyền riêng.

Ở phía server, cuối cùng bạn sẽ viết (hoặc sử dụng) các method giới hạn quyền truy cập vào một số loại action nhất định dựa trên người dùng hiện tại là ai (hoặc liệu người gửi request có đăng nhập hay không). Một lần nữa, Devise sẽ giúp bạn việc này bằng cách cung cấp sẵn một số helper method (chẳng hạn để kiểm tra có người dùng nào đang đăng nhập hay kiểm tra người dùng hiện tại là ai).

### Kết luận

Chúng ta sẽ tìm hiểu sâu hơn về những nội dung này sau, nhưng việc hiểu chúng trong bối cảnh những gì đã nói về cách request được tạo ra là rất hữu ích, vì chúng bổ sung một vài lớp lên các request HTTP vốn trước đây độc lập với nhau. Hệ thống xác thực cho phép bạn thiết lập các session để duy trì trạng thái của người dùng (chẳng hạn trạng thái đã đăng nhập) qua nhiều request, đồng thời giúp bạn xác định người dùng có được phép thực hiện một việc nào đó hay không.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- [HTTP được giải thích bởi David Malan của Harvard](http://www.youtube.com/watch?v=8KuO4r5CHjM)
- [Kiến thức cơ bản về HTTP Request/Response](http://justahelp.blogspot.com/2013/09/http-requestresponse-basics.html) của Pralay Roy
