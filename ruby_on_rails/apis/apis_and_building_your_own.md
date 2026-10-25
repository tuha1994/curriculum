### Giới thiệu

Làm việc với API vừa tuyệt vời vừa gây nản lòng. Một mặt, việc kết nối với những ứng dụng khác có thể mở rộng đáng kể phạm vi tiếp cận và "độ ngầu" của ứng dụng bạn. Mặt khác, công việc này đòi hỏi phải đọc rất nhiều tài liệu, tìm hiểu các chiến lược xác thực, rồi phân tích những thông báo lỗi tệ hại (hoặc hoàn toàn không có thông báo lỗi).

Nói rộng hơn, nếu bạn vẫn chưa rõ API (Application Programming Interface - giao diện lập trình ứng dụng) về cơ bản là gì, [FreeCodeCamp giải thích API là gì](https://www.freecodecamp.org/news/what-is-an-api-in-english-please-b880a3214a82/). [Bài tóm tắt về API của Dave Roos](http://money.howstuffworks.com/business-communications/how-to-leverage-an-api-for-conferencing1.htm) cũng là một cách hay để nắm lại kiến thức.

"API" là một khái niệm vô cùng rộng -- bất cứ khi nào ứng dụng của bạn giao tiếp với một ứng dụng khác, việc đó đều diễn ra thông qua một dạng API nào đó. Các thành phần bên trong chính ứng dụng của bạn, chẳng hạn những phần khác nhau của Rails, cũng giao tiếp với nhau thông qua API... về cơ bản, chúng là những ứng dụng con tương đối độc lập, truyền cho nhau dữ liệu cần thiết để hoàn thành nhiệm vụ riêng. Trong thế giới ứng dụng, mọi thứ đều là API!

Khi xây dựng các ứng dụng có phần giao diện người dùng động hơn (từ những ứng dụng JavaScript một trang phức tạp đến những lời gọi AJAX riêng lẻ đơn giản), chúng sẽ giao tiếp với phần backend Rails thông qua các API do bạn xây dựng... mà thực chất chỉ cần thêm một hoặc hai dòng code để chỉ cho controller cách xuất JSON hoặc XML thay vì HTML.

Trong bài học này, chúng ta sẽ tìm hiểu cách xây dựng API của riêng mình. Ở bài học tiếp theo, chúng ta sẽ tìm hiểu cách kết nối với API của các ứng dụng khác. Các bài học nhằm cung cấp cho bạn điểm khởi đầu tốt để học nội dung này, nhưng tất nhiên không thể bao quát mọi trường hợp. Phần lớn việc làm việc với API là học cách đọc tài liệu của chúng và tìm hiểu chúng yêu cầu điều gì.

### Tổng quan bài học

Phần này đưa ra tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Tạo một action trong Rails Controller có khả năng trả về phản hồi JSON và XML.
- Tạo API cho một model, trong đó ẩn một số thuộc tính nhất định khỏi người dùng API.
- Tạo thông báo lỗi tùy chỉnh để phản hồi các request không hợp lệ.
- Service Oriented Architecture là gì.

### Kiến thức cơ bản về API

Ứng dụng Rails của bạn về cơ bản vốn đã là một API, dù có thể bạn chưa nhìn nhận nó theo cách đó. Trình duyệt web mà người dùng đang chạy cũng là một chương trình, vì vậy nó thực chất gửi một request API đến ứng dụng Rails mỗi khi bạn yêu cầu một trang mới. Chỉ là việc render payload HTML phổ biến đến mức chúng ta tích hợp sẵn nó vào các chương trình phía server như kiểu phản hồi mặc định và xem mọi thứ khác là đặc biệt.

Tuy nhiên, nhiều khi bạn muốn gửi một request mà không cần trải qua toàn bộ phiền phức của việc dùng trình duyệt web. Có thể bạn không quan tâm trang được cấu trúc như thế nào (HTML), mà chỉ muốn lấy thẳng dữ liệu. Giả sử bạn muốn lấy danh sách tất cả người dùng. Bạn có thể truy cập một URL như `http://yourapplication.com/users` mà URL này có lẽ sẽ chạy action `#index` và render danh sách tất cả người dùng của ứng dụng.

Nhưng nếu tất cả những gì bạn muốn chỉ là danh sách người dùng, tại sao phải bận tâm đến toàn bộ phần dư thừa đó? Cách đơn giản nhất là gửi request đến chính URL đó và yêu cầu phản hồi JSON hoặc XML. Nếu thiết lập controller Rails đúng cách, bạn sẽ nhận lại một object mảng JSON chứa toàn bộ người dùng. Tuyệt!

Nguyên tắc tương tự cũng áp dụng khi bạn giao tiếp với API bên ngoài... chẳng hạn bạn muốn lấy các bài đăng gần đây của một người dùng từ X (trước đây có tên là Twitter). Bạn chỉ cần chỉ cho ứng dụng Rails cách giao tiếp với API của Twitter (ví dụ: tự xác thực), gửi request và xử lý nhóm bài đăng được trả về.

### Xây dựng API

Bạn có thể muốn biến toàn bộ ứng dụng Rails thành backend API cho một trang web frontend, hoặc chỉ muốn học cách gửi JSON khi frontend yêu cầu. Phần này không hướng dẫn cách xây dựng một API RESTful đầy đủ tính năng với hệ thống xác thực... đây chỉ là phần giới thiệu nhẹ nhàng về việc coi ứng dụng của bạn như một API.

#### Những điều cơ bản

Nếu muốn ứng dụng Rails trả về JSON thay vì HTML, bạn cần chỉ cho controller làm điều đó. Điều thú vị là cùng một action của controller có thể trả về những thứ khác nhau tùy thuộc việc người dùng đang gửi request thông thường từ trình duyệt hay một lời gọi API từ dòng lệnh. Rails xác định loại request dựa trên phần mở rộng của file được yêu cầu, chẳng hạn `example.xml` hoặc `example.json`.

Bạn có thể xem Rails cho rằng bạn muốn loại file nào bằng cách kiểm tra log của server:

```bash
  Started GET "/posts/new" for 127.0.0.1 at 2013-12-02 15:21:08 -0800
  Processing by PostsController#new as HTML
```

Dòng đầu tiên cho biết URL nào được yêu cầu, còn dòng thứ hai cho biết request đang được chuyển đến đâu và Rails xử lý nó như thế nào. Nếu dùng phần mở rộng `.json`, log sẽ trông như sau:

```bash
  Started GET "/posts.json" for 127.0.0.1 at 2013-12-04 12:02:01 -0800
  Processing by PostsController#index as JSON
```

Nếu đang chạy một ứng dụng mẫu, hãy thử truy cập các URL khác nhau. Nếu controller chưa sẵn sàng xử lý chúng, bạn có thể gặp lỗi, nhưng vẫn sẽ thấy Rails hiểu request của bạn là gì.

#### Render JSON hoặc XML

Sau khi quyết định sẽ phản hồi một request JSON hoặc XML, bạn cần chỉ cho controller render JSON hoặc XML thay vì HTML. Cách thực hiện là dùng method `#respond_to`:

```ruby
  class UsersController < ApplicationController

    def index
      @users = User.all

      respond_to do |format|
        format.html # index.html.erb
        format.xml  { render :xml => @users }
        format.json { render :json => @users }
      end
    end

  end
```

Trong trường hợp này, `#respond_to` truyền cho block một object format, và bạn có thể gắn vào đó lời gọi render phù hợp. Nếu không làm gì, HTML sẽ được render bằng template Rails mặc định như bình thường (trong trường hợp này là `app/views/index.html.erb`).

Hàm `#render` đủ thông minh để biết cách render rất nhiều định dạng. Khi truyền cho nó key `:json`, hàm sẽ gọi `#to_json` trên giá trị, trong trường hợp này là `@users`. Việc đó biến các object Ruby của bạn thành các chuỗi JSON, có thể được truyền đến ứng dụng đã gửi request.

Vậy là bạn đã có một API. Tất nhiên, mọi thứ có thể phức tạp hơn một chút nếu bạn muốn làm những việc khác thường, nhưng đó là những điều cơ bản.

#### Chỉ định các thuộc tính cần trả về

Giả sử bạn muốn đảm bảo không trả về địa chỉ email của người dùng cùng với User object. Khi đó, bạn sẽ muốn điều chỉnh những thuộc tính User được trả về bằng cách thay đổi cách method `#to_json` hoạt động.

Ngày trước, bạn chỉ cần ghi đè phiên bản `#to_json` của mình, nhưng hiện nay không cần làm vậy nữa -- thay vào đó, bạn sẽ ghi đè method `#as_json`. Method `#as_json` được `#to_json` sử dụng, vì vậy việc chỉnh sửa nó sẽ ngầm thay đổi output của `#to_json`, nhưng theo một cách rất cụ thể.

`#to_json` thực hiện hai việc -- nó chạy `#as_json` và nhận lại một hash các thuộc tính cần được render thành JSON. Sau đó, nó thực sự thực hiện việc render thành JSON bằng `ActiveSupport::json.encode`. Vì vậy, bằng cách chỉnh sửa `#as_json`, bạn nhắm chính xác hơn vào phần của method `#to_json` mà mình thực sự muốn thay đổi.

Trong trường hợp này, chúng ta sẽ sửa `#as_json` trong model để chỉ trả về những thuộc tính mong muốn:

```ruby
  # app/models/user.rb
  class User < ActiveRecord::Base

    # Option 1: Purely overriding the #as_json method
    def as_json(_options={})
      { :name => self.name }  # NOT including the email field
    end

    # Option 2: Working with the default #as_json method
    def as_json(options={})
      super({ only: [:name] }.merge(options))
    end

  end
```

Trong controller, chúng ta chỉ cần render JSON như bình thường (ở ví dụ dưới đây, nó sẽ luôn trả về JSON, bất kể request có phải là request HTML hay không):

```ruby
  # app/controllers/users_controller.rb
  class UsersController < ApplicationController

    def index
      render :json => User.all
    end

  end
```

Lưu ý rằng bạn không cần tự gọi `#to_json` khi sử dụng `#render`... nó sẽ làm việc đó thay bạn.

Xem [tài liệu về as_json](https://api.rubyonrails.org/classes/ActiveModel/Serializers/JSON.html#method-i-as_json) để biết thêm chi tiết, chẳng hạn như cách include các association.

#### Render trạng thái không có nội dung hoặc lỗi

Đôi khi bạn chỉ muốn gửi một mã lỗi HTTP mà không có response body. [Rails guides](https://guides.rubyonrails.org/layouts_and_rendering.html#using-head-to-build-header-only-responses) một lần nữa cung cấp một giải pháp trang nhã và rất hữu ích cho vấn đề này.
Đây là một ví dụ (một lần nữa, chúng ta chỉ render lỗi trong mọi trường hợp):

```ruby
  # app/controllers/users_controller.rb
  class UsersController < ApplicationController

    def index
      head :not_found
    end

  end
```

#### Tạo các trang lỗi động

Bạn có thể [tự tạo các trang lỗi](https://web-crunch.com/posts/custom-error-page-ruby-on-rails).

Đôi khi Heroku yêu cầu thêm một số bước để hiển thị đúng các trang lỗi của bạn, như mô tả trong [tài liệu trang lỗi của Heroku](https://devcenter.heroku.com/articles/error-pages). Trước hết, bạn có thể cần xóa các trang tĩnh trong thư mục `app/public`.

#### Bảo mật khi cung cấp API ra bên ngoài

Giả sử bạn chỉ muốn cho phép gọi API nếu người dùng đã đăng nhập. Cơ chế xác thực hiện có của controller cũng sẽ xử lý được việc này -- chỉ cần đảm bảo bạn đã thiết lập đúng `#before_action` (ví dụ: `before_action :require_login`). Trường hợp này có thể xảy ra khi cả người dùng đã đăng nhập và chưa đăng nhập đều xem trang, nhưng mỗi nhóm cần thấy dữ liệu khác nhau. Bạn không muốn người chưa đăng nhập có thể gọi API để lấy dữ liệu nhạy cảm, cũng như không muốn họ truy cập một trang HTML mà họ không được phép xem.

Nếu muốn xử lý request từ một ứng dụng không phải trình duyệt web (ví dụ: dòng lệnh), bạn không thể dựa vào cookie trình duyệt để xác thực. Đó là lý do hầu hết API cấp token tùy chỉnh cho từng người dùng được cấp quyền; token này phải được gửi kèm request như một phần của quá trình xác thực. Chúng ta sẽ nói thêm một chút về token trong bài học tiếp theo.

#### Các bước tiếp theo

Hiện giờ bạn đã có khả năng dùng ứng dụng Rails để cung cấp không chỉ HTML mà bất kỳ định dạng nào bạn muốn. Nếu muốn tiến xa hơn và cho phép các nhà phát triển khác xây dựng trên nền tảng của bạn (để họ gửi các request theo chương trình thay vì đăng nhập với tư cách người dùng), bạn sẽ cần làm cho hệ thống API mạnh mẽ hơn nhiều. Chúng ta không thể trình bày tất cả ở đây, nhưng hãy xem tài liệu sau:

- [Xây dựng Rails API tuyệt vời](http://collectiveidea.com/blog/archives/2013/06/13/building-awesome-rails-apis-part-1/) có nhiều phương pháp hay để phát triển từ một API thử nghiệm thành ứng dụng sẵn sàng cho production.

### Service Oriented Architecture (SOA)

Đây là lúc thích hợp để giới thiệu một khái niệm kiến trúc có tên là "Service Oriented Architecture". Ý tưởng cơ bản là ứng dụng của bạn có thể sẽ chứa nhiều service khác nhau, chẳng hạn xử lý thanh toán, đăng ký người dùng, engine gợi ý, v.v. Thay vì xây dựng tất cả dưới cùng một ứng dụng chính, bạn tách chúng thành những phần hoàn toàn độc lập và cho chúng giao tiếp với nhau bằng các API hướng vào bên trong.

Cách làm này có nhiều ưu điểm. Vì mỗi phần trong ứng dụng không quan tâm các phần khác hoạt động ra sao mà chỉ biết cách yêu cầu dữ liệu thông qua API của chúng, bạn có thể thực hiện những thay đổi lớn đối với code bên trong một service mà phần còn lại của ứng dụng vẫn hoạt động y như cũ. Bạn có thể thay thế hoàn toàn service này bằng service khác và, miễn là nó giao tiếp bằng cùng các method API, việc đó rất dễ dàng. Bạn có thể dùng API bên ngoài cho một phần ứng dụng (như thanh toán) thay vì API nội bộ. Bạn có thể có một ứng dụng PHP giao tiếp với ứng dụng Python, rồi ứng dụng Python giao tiếp với ứng dụng Rails, và không ai bận tâm vì tất cả đều giao tiếp bằng API.

Dù thế nào thì việc cố gắng giữ các phần độc lập của ứng dụng càng độc lập càng tốt thường vẫn là một ý tưởng hay. Việc chính thức hóa điều này dưới tên gọi SOA buộc bạn suy nghĩ chính xác về những method nào muốn cung cấp cho các phần khác của ứng dụng, nhờ đó code cũng được cải thiện. Một lợi ích khác là khi coi mỗi thành phần lớn của ứng dụng là độc lập, bạn có thể cô lập vấn đề dễ hơn nhiều và buộc phải xử lý lỗi theo cách hợp lý.

Dùng kiến trúc SOA cho toàn bộ ứng dụng cũng giống như tách một script Ruby khổng lồ và phức tạp thành những class và method gọn gàng, chỉ khác là ở quy mô rộng hơn.

Một trong những trường hợp nổi tiếng nhất về việc chuyển sang kiến trúc hướng service là Amazon.com. Vào khoảng năm 2002, Jeff Bezos về cơ bản đã ra lệnh rằng mọi nhóm phải chuyển sang SOA, nếu không sẽ bị sa thải. Một bài đăng blog nổi tiếng của một nhân viên Google, vô tình được công khai thay vì chỉ lưu hành nội bộ công ty, đã nói về [sức mạnh SOA của Amazon](https://gist.github.com/chitchcock/1281611). Đây là một bài rất đáng đọc, nhưng những điểm chính trong email của Bezos, được bài đăng trích dẫn, là:

1. Từ nay trở đi, tất cả các nhóm phải cung cấp dữ liệu và chức năng của mình thông qua các service interface.
1. Các nhóm phải giao tiếp với nhau thông qua những interface này.
1. Không được phép có bất kỳ hình thức giao tiếp liên tiến trình nào khác: không liên kết trực tiếp, không đọc trực tiếp kho dữ liệu của nhóm khác, không dùng mô hình bộ nhớ chia sẻ, hoàn toàn không có cửa sau nào. Hình thức giao tiếp duy nhất được phép là các lời gọi service interface qua mạng.
1. Công nghệ họ sử dụng không quan trọng. HTTP, Corba, Pubsub, protocol tùy chỉnh -- không quan trọng. Bezos không quan tâm.
1. Tất cả service interface, không có ngoại lệ, phải được thiết kế ngay từ đầu để có thể cung cấp ra bên ngoài. Nghĩa là nhóm phải lập kế hoạch và thiết kế sao cho có thể cung cấp interface cho các nhà phát triển bên ngoài. Không có ngoại lệ.
1. Bất kỳ ai không làm điều này sẽ bị sa thải.

SOA là một vấn đề lớn. Chắc chắn sẽ có nhiều vấn đề phát sinh khi sử dụng nó -- hãy xem bài viết về [“bài học rút ra” của Amazon](http://apievangelist.com/2012/01/12/the-secret-to-amazons-success-internal-apis/) -- nhưng cuối cùng nó đem lại rất nhiều lợi ích.

Có lẽ bạn sẽ không quá bận tâm đến SOA khi tự xây dựng các ứng dụng "thử nghiệm", nhưng chắc chắn khái niệm này sẽ xuất hiện nếu bạn làm việc tại một công ty công nghệ, và đây là một nguyên tắc đáng để làm quen.

### Kết luận

Chúng ta sẽ tìm hiểu sâu hơn về việc dùng ứng dụng như một API trong khóa học JavaScript. Trong khóa học đó, bạn sẽ xây dựng một số ứng dụng full-stack dùng các lời gọi AJAX bằng JavaScript để cải thiện trải nghiệm người dùng; về cơ bản, chúng sẽ yêu cầu ứng dụng Rails cung cấp dữ liệu XML hoặc JSON thay vì cả một trang HTML. Sau đó, bạn sẽ xây dựng các ứng dụng JavaScript một trang, dựa vào API do ứng dụng Rails cung cấp để lấy toàn bộ dữ liệu cần thiết từ database, nhưng phần còn lại sẽ hoàn toàn do frontend điều khiển.

Cách tốt nhất để thực sự hiểu API là tự xây dựng và kết nối với chúng, vì vậy chúng ta sẽ tập trung vào điều đó trong các project.

Trong bài học tiếp theo, chúng ta sẽ tìm hiểu cách làm việc với API của người khác, qua đó bổ sung đủ loại sức mạnh cho ứng dụng của bạn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc phần 7 của [Rails Guide về Controllers](https://guides.rubyonrails.org/v7.2/action_controller_overview.html#rendering) để tìm hiểu về việc render JSON và XML.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Xem miễn phí [Railscast về việc biến ứng dụng của bạn thành API](http://railscasts.com/episodes/348-the-rails-api-gem)
- Xem miễn phí [Railscast về bảo mật API](http://railscasts.com/episodes/352-securing-an-api)
- Xem miễn phí [Railscast về versioning API](http://railscasts.com/episodes/350-rest-api-versioning)
- [GoRails #162 API đầu tiên của chúng ta](https://www.gorails.com/episodes/our-first-api)
- [Tìm hiểu `to_json` hay `as_json` qua bài viết của Jonathan Julian](http://jonathanjulian.com/2010/04/rails-to_json-or-as_json/) đưa ra các ví dụ cụ thể về việc đào sâu vào method `as_json`.
- [Giải thích về Service Oriented Architecture](https://www.youtube.com/watch?v=7s_S5Hkm7z0)