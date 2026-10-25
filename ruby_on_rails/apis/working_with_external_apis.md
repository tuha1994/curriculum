### Giới thiệu

Việc biến ứng dụng của bạn thành một API khá dễ, nhưng còn việc lấy dữ liệu từ những API khác trên internet thì sao? Đây thường là phần khó hơn một chút, vì mỗi API đều khác nhau và bạn phải tự xác thực với chúng.

Hầu hết các service phổ biến đều cung cấp API để developer có thể tương tác với chúng (họ rất thích nếu bạn giúp có thêm người sử dụng platform của họ). Facebook, X, Instagram, Flickr, Dropbox, AirBnB... tất cả đều có API. Chỉ cần Google "companyX API docs" là bạn sẽ đến được khu vực dành cho developer của họ.

Khi mở tài liệu của một API, đôi khi bạn có thể thấy hơi choáng ngợp vì phần lớn nội dung dường như mặc định rằng bạn đã biết mình đang làm gì, nhưng chắc chắn có tài liệu tốt hơn những tài liệu khác. Ngoài ra, gần như mọi API đều có một số thành phần chung. Bạn càng làm việc với API nhiều (và càng nắm được flow xác thực), chúng càng trở nên dễ hơn. Chẳng bao lâu nữa, bạn sẽ tạo được các mashup.

Bài học này sẽ trình bày một số bước tổng quát thường gặp ở các API và tổng quan ở mức cao về một số phương thức xác thực với API như Omniauth. Hãy cố gắng nắm được càng nhiều kiến thức khái niệm càng tốt, đồng thời sử dụng tài liệu do từng gem hoặc API cung cấp để hỗ trợ việc triển khai. Nếu bạn tìm được những tài nguyên học miễn phí hay giải thích về API, bạn có thể [đề xuất tài nguyên thông qua một GitHub issue trong curriculum repo của chúng ta](https://github.com/TheOdinProject/curriculum/issues/new/choose)!

### Tổng quan bài học

Phần này trình bày tổng quan về những chủ đề bạn sẽ học trong bài học này.

- Cách thiết lập app để sử dụng API.
- Cách việc sử dụng API thường được kiểm soát.
- RESTful API và cách chúng có thể giúp cuộc sống của bạn dễ dàng hơn.
- OAuth là gì.
- OmniAuth là gì.
- SDK là gì.

### Những bước đầu tiên

Bạn sẽ cần đăng ký bản thân và ứng dụng của mình với API provider. Bằng cách này, họ có thể theo dõi và giám sát việc sử dụng của bạn. Thông thường, họ có một mức sử dụng miễn phí và sẽ tính phí nếu bạn vượt quá giới hạn, dù giới hạn đó được tính theo tổng số MB đã tải xuống, số request hay số transaction (chẳng hạn). Thông thường, ngay cả khi bạn là user của service, bạn vẫn phải truy cập developer portal của họ riêng biệt.

Bạn sẽ cần lấy một **API key** từ API provider, thường là bước cuối của quá trình đăng ký. Key này sẽ nhận diện duy nhất ứng dụng của bạn với API provider và bạn sẽ gửi nó kèm theo mọi request. Nó hơi giống việc một user trên site của bạn tự động gửi session cookie cùng mỗi request để bạn biết họ đã đăng nhập.

Thông thường, bạn cũng sẽ nhận được một "secret key" hoặc một code có tên tương tự. Trong khi API key thường là thông tin công khai, secret key là một lớp bảo mật bổ sung cần thiết cho những API an toàn hơn; bạn sẽ dùng nó để tạo một cryptographic token xác thực mọi request bạn thực hiện. Bạn phải đảm bảo không đưa secret key này vào Git repo! Hãy dùng gem `figaro` hoặc environment variable để đặt key trên server thay vì hard code nó.

### Giới hạn API và security token

Hầu hết API yêu cầu các loại "security clearance" khác nhau cho những request khác nhau:

1. Bạn thường có thể thực hiện những request vô hại, chẳng hạn yêu cầu các bài đăng từ API của X (trước đây có tên là Twitter), bằng các GET request đơn giản và không cần xác thực. Bạn có thể thực hiện chúng từ bất kỳ command line nào hoặc một ứng dụng như [Postman](http://getpostman.com). Những loại API request này thường bị giới hạn rate rất nghiêm ngặt để khuyến khích bạn đăng ký.
1. Lớp tiếp theo là các request có kèm API key. Đây vẫn thường là những thao tác khá vô hại (chẳng hạn lấy dữ liệu công khai) và bị giới hạn theo các pricing tier của API.
1. Những request nhạy cảm hơn, chẳng hạn yêu cầu dữ liệu cụ thể của user hoặc gửi/thay đổi/xóa dữ liệu, có thể sẽ yêu cầu bạn sử dụng quy trình xác thực có secret token. Chúng ta sẽ đề cập những điều cơ bản của việc này trong project. Rate dành cho các request này phụ thuộc vào pricing tier của API.
1. Nhiều khi, bạn thực sự muốn thực hiện request thay mặt cho một user. Ví dụ, để hiển thị cho user một dashboard gồm tất cả bài đăng của họ và các bài đăng trên Facebook, bạn sẽ phải yêu cầu X cung cấp bài đăng của user đó và Facebook cũng cung cấp bài đăng của user đó. Việc này có thể tạo ra RẤT NHIỀU request trên một lượng user lớn, nhưng may mắn là bạn có thể thực hiện chúng thay mặt user bằng cách xin phép user. Chúng ta cũng sẽ đề cập việc này sau, nhưng về cơ bản, bạn đưa user đến site của API provider để đăng nhập, sau đó API provider sẽ cấp cho bạn một token riêng của user để sử dụng cho các request thay mặt họ về sau. Rate cho cách này thường có lợi hơn vì được tính trong một bucket riêng cho từng user. Chúng ta thường dùng giao thức OAuth cho việc này, như mô tả bên dưới.

### Phiên bản

Các công ty công nghệ, đặc biệt những công ty đã hoạt động lâu năm, thường có nhiều phiên bản API. Thông thường, họ xây dựng một phiên bản nháp đầu tiên khi còn là startup rồi sau đó phải xây dựng một phiên bản thực sự, nhưng vẫn phải hỗ trợ phiên bản ban đầu vì có rất nhiều ứng dụng khác có thể đang sử dụng nó.

Hãy chọn phiên bản mới nhất, trừ khi bạn có lý do thuyết phục để không làm vậy (thường tài liệu sẽ có một phần riêng cho từng phiên bản). Hãy chú ý bạn đang đọc tài liệu của phiên bản nào... không gì khó chịu bằng việc nhận ra mình đã xem tài liệu API v1 thay vì tài liệu API v2.

### RESTful API

Cũng như routes của bạn lý tưởng nhất nên được thiết lập theo cách RESTful, API cũng vậy. Ngày nay, hầu hết API đều là RESTful, nghĩa là các request sử dụng nhóm HTTP method thông thường (GET/POST/PUT/PATCH/DELETE) sẽ truy xuất resource như mong đợi. Điểm hay của việc thiết lập routes trong app theo kiểu RESTful là API của bạn cũng sẽ hoạt động theo cách tương tự.

Làm việc với RESTful API ít nhất giúp giảm đáng kể sự đau đầu trong quá trình này, vì bạn thường có thể đoán được mình nên làm gì, rồi vào tài liệu để tìm hiểu chính xác cách thực hiện và kết quả sẽ được trả về dưới định dạng nào. Ví dụ, cũng như bạn mong đợi GET request đến route `/users` trong app sẽ hiển thị một trang liệt kê tất cả user, GET request đến RESTful API với cùng route đó có lẽ sẽ trả về một object JSON hoặc XML chứa tất cả user (hoặc ít nhất là một phần dữ liệu được phân trang).

### OAuth và đăng nhập qua API

Bạn hẳn đã thấy những site có trang chủ cho phép đăng nhập bằng Facebook, X hoặc LinkedIn. Sau khi đăng nhập bằng một trong các service bên thứ ba đó, bạn gần như ngay lập tức có một account mới trên site nhưng không cần cung cấp cho site một password mới.

OAuth thường đứng sau cơ chế này. Nó giải quyết vấn đề rất thực tế về quyền riêng tư dữ liệu và kiểm soát truy cập của user. Ý tưởng là user không cần cung cấp password cho ứng dụng tuyệt vời của bạn để bạn có thể thực hiện request thay mặt họ. Vì vậy, OAuth cho phép Facebook đóng vai trò trung gian khi làm việc với user.

Cơ chế này có hai trường hợp sử dụng tuyệt vời -- đăng nhập user bằng một site bên ngoài như Facebook và thực hiện request hoặc gửi dữ liệu thay mặt User.

Trong trường hợp đầu tiên, bạn không bao giờ biết password Facebook của user. User được chuyển hướng đến website Facebook để đăng nhập, tại đó họ cho phép bạn sử dụng một phần dữ liệu của họ (những dữ liệu này sẽ phải được bạn cấu hình khi thiết lập quy trình). Sau đó Facebook gọi ngược lại cho bạn (bằng cách gửi request đến một URL do bạn chọn cùng dữ liệu phù hợp) và cung cấp một access token đặc biệt cho user đó. Bạn có thể dùng token này để lấy thêm bất kỳ dữ liệu nào mà user đã cho phép bạn truy cập (chẳng hạn tên và địa chỉ email). Bây giờ bạn có thể tạo một user mới trong database bằng dữ liệu đó, và mọi người tương đối hài lòng.

Từ góc nhìn của user, họ chỉ cần nhớ password Facebook để đăng nhập vào site của bạn, nên việc này rất thuận tiện. Họ cũng không cần tin tưởng bạn với password của họ. Miễn là họ tin Facebook, họ có thể sử dụng site của bạn. Ngoài ra, nếu site của bạn làm những điều không tốt với dữ liệu của họ (chẳng hạn khi SocialCam từng spam dữ dội lên timeline Facebook của mọi người trong một thời gian), họ có thể yêu cầu Facebook ngừng cho phép bạn sử dụng dữ liệu của họ, và Facebook sẽ tắt quyền truy cập của token đã cấp cho bạn.

Từ góc nhìn của Facebook, họ giữ phần lớn quyền sở hữu user và len sâu hơn vào đời sống của mọi người (dù theo hướng tốt hay xấu).

Khi đã có user token, bạn được phép thực hiện request hoặc gửi dữ liệu dựa trên bất kỳ permission nào mà Facebook (và user) đã cấp cho bạn. Việc đó có thể bao gồm hỏi danh sách bạn bè của user, đăng lên timeline của họ hoặc "like" một thứ gì đó. Bạn chỉ cần đưa token vào quy trình xác thực của request.

#### Các hạn chế

Bên cạnh việc giới hạn rate cho API, mọi site đều có điều khoản sử dụng. Điều khoản này thường quy định bạn được phép làm gì với dữ liệu thu thập được. Chẳng hạn, họ có thể sẽ không cho phép bạn lưu một lượng lớn dữ liệu của họ vào database riêng (harvesting). Họ cũng có thể không cho phép bạn làm điều gì đó mang tính SPAM hoặc gây ảnh hưởng xấu đến trải nghiệm của user. Hãy dùng lẽ thường làm chỉ dẫn ban đầu, nhưng hãy đọc tài liệu TOU để biết chi tiết. Về cơ bản, nếu việc bạn làm gây ảnh hưởng đến trải nghiệm của user hoặc lấy đi dữ liệu quý giá của họ, bạn không được phép làm.

Cần lưu ý rằng bất cứ khi nào phát triển bằng API của người khác, bạn đều đặt mình vào thế phụ thuộc họ. Nếu bạn sử dụng API đến mức phải trả tiền cho dữ liệu, họ sẽ rất vui. Nếu bạn trở nên quá thành công và cạnh tranh với họ, họ sẽ nắm điểm yếu của bạn rồi ngắt quyền truy cập hoặc đòi mức phí cắt cổ. Chỉ cần nhận thức rõ những rủi ro đó.

#### Những điều cơ bản về OAuth 2.0

OAuth 2.0 thực sự khá phức tạp, nên chúng ta sẽ chỉ trình bày quy trình cơ bản. Bạn chưa cần quan tâm chính xác nó hoạt động phía sau hậu trường như thế nào cho đến khi bắt đầu phát triển nghiêm túc với nó hoặc thử tạo API của riêng mình có sử dụng nó. Tuy nhiên, bạn **sẽ** cần hiểu các bước liên quan đến trải nghiệm của user, vì user chính là lý do bạn xây dựng ứng dụng.

Về cơ bản (vẫn lấy Facebook làm ví dụ):

1. User cố truy cập một page trên app của bạn và bạn yêu cầu user đăng nhập
1. User chọn tùy chọn "Login With Facebook"
1. User được chuyển hướng đến một page Facebook yêu cầu họ xem lại những permission bạn xin và yêu cầu họ đăng nhập. URI sẽ chứa các parameter cho Facebook biết ứng dụng của bạn là ứng dụng nào và có thể cả URI mà Facebook cần gửi response đến (hoặc có thể bạn đã chỉ định URI này khi đăng ký API với họ).
1. User thấy bạn có vẻ là một ứng dụng thú vị nên cho phép bạn xem địa chỉ email và đăng lên timeline của họ. User đăng nhập vào account Facebook. Facebook tạo một authorization code rồi gửi code đó về callback URI của app bạn.
1. User chờ trong khi app của bạn lấy authorization code đó và dùng nó để yêu cầu Facebook cung cấp phần dữ liệu thực sự cần thiết. Facebook xác nhận app của bạn chính là app mà user đã cấp quyền, sau đó POST về cho bạn một authentication token duy nhất của user (có thể hết hạn sau 90 ngày) cùng mọi dữ liệu bạn đã yêu cầu từ đầu (chẳng hạn địa chỉ email).
1. Bạn lưu unique token của user vào database và dùng nó cùng với application key(s) để thực hiện các request tiếp theo thay mặt user.

Hãy xem [tổng quan ngắn gọn về OAuth 2.0](http://tutorials.jenkov.com/oauth2/overview.html) này. Sau đó đọc [phần giải thích đầy đủ hơn về OAuth 2.0 từ tutsplus.com](https://code.tutsplus.com/articles/oauth-20-the-good-the-bad-the-ugly--net-33216).

#### Triển khai OAuth 2.0 trong Rails -- Hãy dùng OmniAuth!

Nghe có vẻ vô cùng phức tạp! Chắc hẳn đã có ai đó tạo một gem cho việc này...

May mắn là có rồi. Thực ra là nhiều người cùng tạo. Có một gem OAuth tổng quát tên là `omniauth` ([tài liệu Omniauth](https://github.com/intridea/omniauth)) và một gem riêng cung cấp authentication strategy cụ thể cho mỗi API lớn ([danh sách các strategy của Omniauth](https://github.com/intridea/omniauth/wiki/List-of-Strategies)). Sau khi làm quen với một strategy, bạn sẽ thấy thoải mái với tất cả strategy còn lại. Cũng đáng lưu ý rằng nếu bạn cài đặt và sử dụng [Devise](https://github.com/heartcombo/devise) (chẳng hạn để xử lý user model), nó đã tích hợp sẵn hỗ trợ cho Omniauth! Họ có tài liệu về [cách tích hợp Devise với Omniauth](https://github.com/heartcombo/devise/wiki/OmniAuth%3A-Overview).

Học cách sử dụng Omniauth thông qua thực hành dễ hơn nhiều so với việc đọc một loạt bullet point. Bạn sẽ có cơ hội triển khai nó trong final project, nơi bạn có thể đặt câu hỏi nếu cần.

### SDK

Ngoài hoặc thay cho việc truy cập API, nhiều công ty cung cấp SDK (software development kit). Thông thường, đây là các thư viện JavaScript chứa mọi code cần thiết để truy cập API của họ. Điều này hữu ích vì bạn có thể truy cập API bằng các JavaScript method thay vì phải thực hiện những màn nhào lộn ở backend của mình. Tuy nhiên, nhược điểm là code base của bạn sẽ lớn hơn và bạn bị buộc phải sử dụng convention của họ cho mọi thứ.

Chúng ta sẽ không trình bày SDK một cách cụ thể trong khóa học này, nhưng việc học chúng bằng cách đọc tài liệu hoàn toàn nằm trong khả năng của bạn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Xem Railscast này về [việc dùng Omniauth để cho phép đăng nhập bằng X (trước đây có tên là Twitter)](http://railscasts.com/episodes/241-simple-omniauth-revised).
  1. Đọc tài liệu [Omniauth](https://github.com/intridea/omniauth)
  1. Chọn một API mà bạn thực sự muốn tương tác, hoặc một web product mà bạn sử dụng gần như mỗi ngày (ví dụ Google, Facebook, Instagram...). Google để tìm tài liệu của API đó, chẳng hạn tìm "instagram api documentation", rồi đọc qua. Có tài liệu tốt hơn những tài liệu khác, nhưng chúng sẽ là nguồn giúp bạn hiểu những method nào có thể gọi, chúng trả về gì, cách đăng ký ứng dụng để nhận API key và nhiều thông tin hữu ích khác.

</div>

#### Kết luận

API là nền tảng để xây dựng các web application phong phú, đồng thời cũng rất thú vị -- ứng dụng của bạn có cảm giác "thực" hơn nhiều khi bạn có thể cho phép user đăng nhập bằng Facebook hoặc hiển thị thông tin từ nhiều nguồn khác nhau trên web. Nếu bạn đang xây dựng một startup, trải nghiệm user được cải thiện đủ nhiều để bạn có thể sẽ thấy tỷ lệ chuyển đổi tăng lên.

Làm việc với external API có thể vô cùng đáng giá vì bạn tận dụng được những functionality mà người khác đã dành rất nhiều thời gian để hoàn thiện, nhưng cũng vô cùng bực bội vì mỗi API đều khác nhau và bạn phải phụ thuộc vào các gem thực hiện những việc phía sau hậu trường mà bạn không hoàn toàn chắc chắn. Theo thời gian, bạn sẽ bắt đầu nhận ra nhiều điểm tương đồng giữa các API, và quy trình làm việc với chúng sẽ ngày càng trở nên quen thuộc. Để giúp bạn đạt được điều đó, project tiếp theo sẽ cho bạn làm việc với một API, và final project cũng sẽ yêu cầu bạn triển khai đăng nhập qua API.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Xem bài đăng Stack Overflow này về [việc tương tác với API bên thứ ba](http://stackoverflow.com/questions/6228870/interfacing-with-a-third-party-api-in-rails-opeing-urls-and-parsing-xml-json) để tham khảo các mẹo.
- [RailsConf 2016 - Từ Zero đến API Hero: Sử dụng API như một chuyên gia, của Cecy Correa](https://www.youtube.com/watch?v=Af5HDgvGuXk)
- Xem bài viết Medium này về [tích hợp API bên thứ ba với Rails 5](https://revs.runtime-revolution.com/integrating-a-third-party-api-with-rails-5-134f960ddbba).
- Xem bài viết Medium khác về [tạo một Rails app cơ bản sử dụng OMDB API](https://medium.com/food4fluctuations/using-an-api-in-rails-for-noobs-5e02edb0e56b), một cơ sở dữ liệu phim mã nguồn mở.