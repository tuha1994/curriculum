### Giới thiệu

Khi người dùng gửi một yêu cầu đến ứng dụng, controller là phần của MVC tiếp nhận yêu cầu đó và phản hồi bằng tệp view `.html.erb`. Trong quá trình xây dựng các ứng dụng của mình, có thể bạn đã tạo những view có các tính năng và bố cục tương tự nhau. Sẽ thế nào nếu có cách chỉ thay đổi những phần khác nhau trong view mà không phải mất công cập nhật nội dung vốn không có ý định thay đổi ngay từ đầu? Nhờ vậy, chúng ta có thể giảm thời gian tải đối với người dùng và lượng dữ liệu ứng dụng phải gửi đi.

Đó là lúc khái niệm **ứng dụng một trang** xuất hiện. Phần này sẽ trình bày cách sử dụng một framework có tên **Turbo** để triển khai hành vi của ứng dụng một trang trong chính ứng dụng Rails của chúng ta. Phần này đề cập đến khá nhiều công cụ, nên có thể lần đầu đọc bạn chưa hiểu hết. Không sao cả; hãy xem bài học này như một tài liệu tham khảo để quay lại khi bạn bắt đầu sử dụng các công cụ được đề cập và hình dung rõ hơn điều gì đang diễn ra trong view của mình.

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- SPA là gì.
- Hotwire là gì.
- Turbo và bốn thành phần của nó.
- Khi nào nên sử dụng Turbo Frame.
- Cách dùng Turbo Stream để cập nhật các trang.
- Hotwire Native được dùng để làm gì.

### Ứng dụng một trang (SPA)

Trước khi tìm hiểu Turbo, điều quan trọng là phải hiểu khái niệm ứng dụng một trang, hay SPA. SPA là một
cách triển khai ứng dụng web chỉ tải một tài liệu web duy nhất. Thay vì liên tục thay tài liệu này bằng một tài liệu hoàn toàn mới sau mỗi yêu cầu, website sẽ tự động viết lại trang hiện tại bằng thông tin mới để trang tải nhanh hơn và mang lại cảm giác giống một ứng dụng gốc hơn. Trang không bao giờ được tải lại. Hãy nghĩ đến những website như Gmail hoặc Facebook, nơi bạn thấy email mới hoặc bài đăng mới trên dòng thời gian mà không cần làm mới trang.

#### Tạo ứng dụng một trang trong Rails

Có nhiều framework JavaScript hỗ trợ lập trình viên triển khai chức năng SPA. Có thể bạn đã nghe đến một số framework như AngularJS hoặc ReactJS. Tuy nhiên, Rails có giải pháp riêng để tạo trải nghiệm SPA mà không cần viết JavaScript. Giải pháp này là một tập hợp các thư viện nằm dưới cái ô Hotwire.

#### Hotwire

Trong Rails 7, mọi ứng dụng mới đều mặc định bao gồm Hotwire. Thực ra Hotwire là thuật ngữ chung cho ba framework khác nhau. Ba framework đó là:

1. Turbo
1. Stimulus
1. Hotwire Native

Bài học bạn đang đọc tập trung hoàn toàn vào Turbo. Chúng ta sẽ học Stimulus trong một bài khác. Trong phạm vi khóa học này, bạn không cần lo về Hotwire Native; chỉ cần nhớ tên của nó vì thỉnh thoảng bạn sẽ thấy nó được nhắc đến.

### Turbo

Turbo là trung tâm của hệ sinh thái Hotwire. Mục tiêu của Turbo là dùng bốn kỹ thuật khác nhau để tạo trải nghiệm SPA nhanh nhạy mà không cần viết JavaScript.

Sau đây là phần tóm tắt nhanh về cả bốn kỹ thuật Turbo. Khi tiếp tục đọc bài, chúng ta sẽ tìm hiểu từng phần sâu hơn.

1. **Turbo Drive**: Chúng ta đã học phần này trong một bài trước.
1. **Turbo Frames**: Giống Turbo Drive, Turbo Frames cũng hỗ trợ điều hướng nhanh, nhưng chỉ áp dụng cho các phần được định trước của trang. Thay vì yêu cầu toàn bộ trang, bạn có thể định nghĩa một vùng HTML là Turbo Frame và chỉ thay thế nội dung bên trong vùng đó.
1. **Turbo Streams**: Turbo Stream phân phối các thay đổi của trang để lập tức chèn, cập nhật hoặc xóa một vùng của trang web. Ví dụ, người dùng tạo một bài đăng mới và bài đăng đó lập tức được chèn lên đầu danh sách bài đăng mà không cần tải lại hay chuyển hướng.
1. **Hotwire Native**: Hotwire Native là kỹ thuật cho phép lập trình viên đạt được các chuyển cảnh kiểu Turbo tương tự trong ứng dụng di động cho iOS hoặc Android.

### Turbo Frames

#### Tạo một frame

Hãy tưởng tượng một tờ giấy bị khoét một lỗ vuông nhỏ. Bạn có thể thay đổi những gì nhìn thấy qua lỗ bằng cách đổi một tờ giấy khác ở phía sau, nhưng phần còn lại của tờ giấy vẫn luôn giống nhau. Đó chính là ý tưởng của Turbo Frames. Turbo Frames cho phép chúng ta định trước một phần của trang để thay thế trong lúc xử lý yêu cầu. Mọi liên kết hoặc form bên trong frame sẽ tạo một yêu cầu đặc biệt, kết quả là chỉ frame được thay đổi. Một trang cũng có thể có nhiều Turbo Frame.

Một frame được xác định bằng cách bọc một vùng bên trong phần tử `<turbo-frame>`. Rails có một helper đặc biệt cho việc này là `<%= turbo_frame_tag %>`.

Một Turbo Frame cơ bản, sử dụng các helper của Rails, có thể trông như sau:

```erb
# with a block
<%= turbo_frame_tag "article" do %>
  Some content
<% end %>

# without a block (typically this is used as a placeholder element to be filled later)
<%= turbo_frame_tag "article" %>
```

sẽ tạo ra:

```html
# with a block
<turbo-frame id="article">
  Some content
</turbo-frame>

# without a block
<turbo-frame id="article"></turbo-frame>
```

Lưu ý rằng các frame có một ID. ID là cách Turbo nhận diện frame để biết frame nào là frame nào.
Với helper Turbo Frame, bạn có thể thay ID bằng một biến. Ví dụ:

```erb
<% @articles.each do |article| %>
  <%= turbo_frame_tag article do %>
    <%= article.title %>
  <% end %>
<% end %>
```

Ví dụ trên sẽ tạo một turbo frame cho mỗi bài viết. Mỗi frame có một id duy nhất như `article_1` hoặc `article_2`, và tất cả những gì chúng ta phải đưa vào là biến article.

#### Kết nối với các frame khác

Bây giờ chúng ta đã có frame đầu tiên và có thể thay nội dung của nó bằng một liên kết yêu cầu nội dung frame mới. Tất cả những gì cần làm là đặt một liên kết bên trong Turbo Frame, trong đó view được yêu cầu *cũng* chứa một Turbo frame có **cùng ID**.

Hãy thay view `/show` bằng view `/edit` của một bài viết:

```erb
# views/articles/show.html.erb

...
<%= turbo_frame_tag @article do %>
  Content for our article!
  <%= link_to "Edit Article", edit_article_path(@article) %>
<% end %>
...
```

```erb
# views/articles/edit.html.erb

...
<%= turbo_frame_tag @article do %>
  Form to edit the article
  <%= link_to "Return to Article", @article %>
<% end %>
...
```

Chỉ vậy là đủ! Turbo sẽ nhận ra URL đích, tức trang `/show` hoặc `/edit`, có một Turbo Frame tương ứng và thay vùng frame bằng nội dung từ frame của trang mới. Cũng cần lưu ý rằng cách này hoạt động với form. Trong controller, nếu action `update` chứa `redirect_to @article`, Turbo Frame sẽ được cập nhật khi chúng ta gửi form, giống như khi nhấp vào một liên kết.

Bây giờ các frame tương ứng đã có thể thay nội dung cho nhau, vậy nội dung nằm bên ngoài frame thì sao? Mọi thứ bên ngoài frame không thay đổi. Nếu đi từ `/show` đến `/edit`, nội dung bên ngoài frame vẫn là nội dung của trang `/show`, và chúng ta cũng không nhận được nội dung nào nằm ngoài frame `/edit`. Chúng ta không điều hướng đến trang mới; chỉ yêu cầu HTML mới từ một route khác rồi chèn nó vào trang hiện tại. `url` hiện tại cũng không thay đổi. Chúng ta vẫn ở path `/show`, và nếu làm mới trang, vẫn sẽ thấy view `/show`. (Lưu ý rằng có thể thay đổi hành vi mặc định này bằng cách dùng `data-turbo-action` của Turbo Drive để tiến lịch sử trình duyệt và cập nhật `url` hiện tại.)

#### Thoát khỏi một Turbo Frame

Đôi khi bạn có thể muốn một liên kết bên trong Turbo Frame hoạt động như điều hướng trang bình thường. Để làm vậy, hãy thêm `data-turbo-frame="_top"` vào phần tử. Ví dụ với helper liên kết của Rails:

```erb
<%= link_to "Return to Article", @article, data: { turbo_frame: "_top" } %>
```

#### Nhắm đến một Turbo Frame từ bên ngoài

Chúng ta cũng có thể làm ngược lại: biến một liên kết nằm bên ngoài Turbo Frame hoạt động như thể nó nằm bên trong Frame và cập nhật frame đó. Lần này, chúng ta đặt thuộc tính dữ liệu `turbo-frame` trỏ đến ID của frame cụ thể. Giả sử chúng ta muốn chỉ định một turbo frame để hiển thị danh sách bài đăng hoặc danh sách hình ảnh:

```erb
<%= link_to "Show Posts", posts_path, data: { turbo_frame: "list-region" } %>
<%= link_to "Show Images", images_path, data: { turbo_frame: "list-region" } %>

<%= turbo_frame_tag "list-region" %>
```

Nhấp vào một trong hai liên kết trên sẽ gửi yêu cầu đến path tương ứng và trả về nội dung bên trong frame `"list-region"`.

#### Src và tải chậm

Frame có thể được cung cấp thuộc tính `src`. Khi thuộc tính này được cung cấp, frame sẽ được điền sau lần tải trang ban đầu bằng cách gửi một yêu cầu riêng đến path tương ứng. Chúng ta cũng có thể bọc nội dung giữ chỗ bên trong frame `src` này để tạo một [khung giữ chỗ dạng skeleton](https://uxdesign.cc/what-you-should-know-about-skeleton-screens-a820c45a571a).

Ví dụ:

```erb
...
<%= turbo_frame_tag "Articles", src: articles_path do %>
  <div>
    I am a placeholder! After the request to articles_path is finished,
    I will be replaced with the content inside of that page's turbo frame
    with the id of "Articles".
  </div>
<% end %>
```

Chúng ta cũng có thể **tải chậm** các frame. Frame tải chậm chỉ lấy nội dung khi nó trở nên hiển thị trên trang. Để frame tải chậm, chúng ta thêm `loading=lazy`. Frame có `loading=lazy` *bắt buộc* phải có thêm thuộc tính `src:` để lấy nội dung; nếu không, nó sẽ không làm gì cả. Dùng lại ví dụ trước:

```erb
...
<%= turbo_frame_tag "Articles", src: articles_path, loading: "lazy" do %>
  <div>
    I am a placeholder! I will be replaced when a user scrolls down to see me on the page!
  </div>
<% end %>
```

### Turbo Stream

Giờ chúng ta đã biết cách thiết lập view để dùng Turbo Frames, nhưng còn nội dung do người dùng thay đổi thì sao? Chúng ta không thể đặt một Turbo Frame quanh thứ chưa tồn tại. Đó là lúc Turbo Stream phát huy tác dụng. Turbo Streams gửi các thay đổi của trang dưới dạng HTML được bọc trong các phần tử `<turbo-stream>`. Turbo Streams chỉ định một action cần thực hiện và ID của phần tử DOM đích sẽ được cập nhật bằng action đó. Ví dụ, một Turbo Stream có `action="replace"` và `target="body"` sẽ thay thế phần tử HTML có `id="body"` bằng phần tử mới được gửi qua Turbo Stream. Các stream này có thể được gửi để phản hồi một yêu cầu trực tiếp từ trình duyệt hoặc được broadcast qua kết nối websocket. Bài học này chỉ tập trung vào cách triển khai bằng yêu cầu trình duyệt. Turbo Streams có thể dùng 7 action khác nhau:

- Append
- Prepend
- Before
- After
- Replace
- Update
- Remove

Turbo Streams được phân phối thông qua controller. Cũng như người dùng gửi yêu cầu `html` và nhận các tệp `view.html.erb`, người dùng có thể nhận các tệp `view.turbo_stream.erb`. Đây không phải các tệp view độc lập như bạn vẫn biết; chúng chỉ chứa vài dòng và là cách gửi cho người dùng một phản hồi Stream thay vì một trang mới.

#### Turbo Stream đầu tiên

Giả sử chúng ta đã xây dựng một website nơi người dùng có thể tạo bài đăng. Bằng cách thêm một `turbo_frame` có thuộc tính `src:` trỏ đến `new_post_path`, đồng thời bọc view `new` của bài đăng trong một `turbo_frame_tag` có `id` tương ứng, chúng ta có thể đưa form của action `new` lên cùng trang với feed `index`. Nó có thể trông như sau.

View `index` của chúng ta:

```erb
# views/posts/index.html.erb

<%= turbo_frame_tag "new_post", src: new_post_path %>
<div id="posts">
  <%= render @posts %>
</div>
```

Trang `new` cùng form của nó:

```erb
# views/posts/new.html.erb

<%= turbo_frame_tag "new_post" do %>
  <%= form_with model: @post do |form| %>
    <%= form.label :body %>
    <%= form.text_area :body %>
    <%= form.submit %>
  <% end %>
<% end %>
```

Partial `_post` mà mọi bài đăng sẽ được render cùng:

```erb
# views/posts/_post.html.erb

<div>
  <%= post.body %>
</div>
```

Controller của chúng ta:

```ruby
# controllers/posts_controller.rb

class PostsController < ApplicationController
  def index
    @posts = Post.all
  end

  def new
    @post = Post.new
  end

  def create
    @post = Post.new(post_params)
    if @post.save
      redirect_to posts_path
    else
      render :new
    end
  end

  private

  def post_params
    params.expect(post: [:body])
  end
end
```

Kết quả là ban đầu chỉ trang `posts#index` được tải, *sau đó* một yêu cầu khác mới được gửi đến action `posts#new` của controller để chèn form! Cách này cũng giúp code của chúng ta DRY, vì nếu thay vào đó chọn thêm form bằng một partial trên trang index, chúng ta vẫn phải lặp lại action `posts#new` trong action `posts#index`.

Tuy nhiên, nếu ngay lúc này người dùng gửi một bài đăng, điều kỳ lạ sẽ xảy ra! Cửa sổ form biến mất nhưng không có lỗi nào. Bài đăng đã đi đâu? Khi làm mới trang, chúng ta thấy bài đăng đã được gửi. Chuyện xảy ra là Rails không biết chúng ta muốn một Turbo Stream nên chưa thể tự động thêm bài đăng. Thay vào đó, nó cố chuyển hướng đến chính trang hiện tại. **Turbo Drive** chặn việc này và tải một phiên bản được lưu trong bộ nhớ đệm của trang, phiên bản đó không có nội dung form, nên form biến mất.

Tất cả chuyện này trông đáng sợ như thể có gì đó sai, nhưng không sao cả. Chúng ta chỉ cần thiết lập Turbo Stream, rồi các thành phần sẽ lại phối hợp với nhau.

#### Turbo Stream trong controller

Trước hết, chúng ta cho controller biết rằng muốn chấp nhận định dạng Turbo Stream. Cách làm giống như khi chấp nhận một định dạng chẳng hạn JSON. Action create của chúng ta giờ có thể trông như sau:

```ruby
# controllers/posts_controller.rb
...
  def create
    @post = Post.new(post_params)

    respond_to do |format|
      if @post.save
        format.turbo_stream
      else
        format.html { render :new, status: :unprocessable_entity }
      end
    end
  end
```

Điều gì đang diễn ra ở đây? Block `respond_to do |format|` là cách chúng ta cho controller biết phải xử lý nhiều hơn chỉ các yêu cầu định dạng `html`. Nếu bài đăng được lưu, chúng ta muốn định dạng phản hồi là `create.turbo_stream.erb`. Nếu không lưu được, chúng ta muốn render tệp `new.html.erb`. Hãy chú ý rằng điểm khác nhau giữa các định dạng nằm ở phần mở rộng tệp sau tên và trước `.erb`.

Chúng ta đưa mã lỗi `422 Unprocessable Entity` vào phản hồi khi bài đăng chưa được lưu, để Turbo Drive biết phải xử lý thế nào và cửa sổ form không biến mất nữa.

Giờ tất cả những gì cần làm là tạo tệp `create.turbo_stream.erb` để phản hồi.

#### Template Turbo Stream

Bạn tạo tệp Turbo Stream bên trong thư mục `views`, giống như mọi view khác. Trong ví dụ này, tệp sẽ nằm ở `views/posts/create.turbo_stream.erb`. Tệp view của chúng ta sẽ như sau:

```erb
<%= turbo_stream.append "posts", @post %>
```

Chỉ cần dòng đó! Dòng lệnh này tạo một gói Turbo Stream với action `append`. Đích của action là `"posts"`. Đây là `<div>` có `id="posts"`, chứa tất cả bài đăng. Turbo Stream sẽ tìm `<div>` đó và nối `@post` hoàn toàn mới vào cuối. `@post` vừa tạo thậm chí sẽ dùng partial `_post.html.erb` để có bố cục phù hợp như các bài đăng khác. Chúng ta có quy ước đặt tên của Rails để cảm ơn điều đó. Nếu bạn không theo quy ước Rails, có thể chỉ định partial cần dùng bên trong template `turbo_stream`.

Bây giờ chúng ta đã thêm phản hồi `format.turbo_stream` vào controller và tệp `create.turbo_stream.erb`, chúng ta có thể tạo bài đăng và nhìn nó được nối vào danh sách.

#### Mẹo bổ sung về Turbo Stream

Bài học này không có đủ thời gian để đi sâu hơn vào cách Turbo Stream hoạt động, nhưng sau đây là một số chủ đề để bạn tự nghiên cứu
nếu muốn tìm hiểu thêm:

1. Bạn có thể nối nhiều action `turbo_stream` trong một tệp `turbo_stream.erb`, chẳng hạn khi muốn nối vào một vùng và
    cập nhật một vùng khác cùng lúc.

1. Bạn *không bắt buộc* phải tạo một tệp bố cục riêng cho các định dạng Stream. Ví dụ, thay vào đó chúng ta có thể đổi dòng trong controller thành:

    ```ruby
    format.turbo_stream { render turbo_stream: turbo_stream.append('posts', @post) }
    ```

    Tuy nhiên, bạn không nên làm vậy với thứ gì phức tạp hoặc có chuỗi action.

1. Có thể bạn đã nhận thấy khi gửi một Post mới, hộp văn bản không được xóa. Bạn cần reset phần tử gửi để nó lại trống. Hotwire có cách giải quyết bằng cách tích hợp Stimulus, một framework JavaScript nhẹ. Tuy nhiên, đừng lo về Stimulus trong ví dụ này; bài học tiếp theo sẽ trình bày cách viết và sử dụng Stimulus Controller.

### Hotwire Native

Mảnh ghép cuối cùng của Turbo là thứ bạn không cần biết nhiều trong phạm vi khóa học này, ngoài việc biết nó tồn tại. Hotwire Native hữu ích để bọc ứng dụng của bạn trong một shell thân thiện với thiết bị di động. Nếu quan tâm đến phát triển di động và cách dùng Turbo với nó, bạn có thể thích xem các bản demo cho điện thoại [iOS](https://github.com/hotwired/turbo-ios/blob/main/Demo/README.md) và [Android](https://github.com/hotwired/turbo-android/blob/main/demo/README.md).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc các phần 1, 2 và 3 trong bài viết [Ứng dụng một trang là gì và tại sao mọi người lại thích chúng đến vậy?](https://www.bloomreach.com/en/blog/what-is-a-single-page-application) của Bloomreach.
1. Xem [Video demo Hotwire](https://www.youtube.com/watch?v=eKY-QES1XQQ)
    - Chúng ta mới chỉ học nội dung đến mốc 5:40, nhưng bạn có thể xem tiếp sau mốc đó để làm quen hơn với những khía cạnh khác của Hotwire sẽ được đề cập trong các bài sắp tới.
    - Video được biên tập như một phần trình diễn rất nhanh. Đừng cố tạm dừng để dùng video này làm hướng dẫn. Hãy thư giãn và xem demo để quan sát trực quan cách Turbo Drive, Frames và Streams phối hợp với nhau.
1. Đọc lướt các phần 1–5 của [Sổ tay Turbo](https://turbo.hotwired.dev/handbook/introduction)
    - Sổ tay này được viết độc lập với backend, nghĩa là code bạn thấy là HTML thuần chứ không phải các tag Rails, nhưng đây vẫn là tài liệu hữu ích để tham khảo cách Turbo hoạt động.
1. Xem nhanh [trang thông tin RubyDoc](https://www.rubydoc.info/gems/turbo-rails/) của gem Turbo-Rails.
    - Tài liệu này trình bày các cú pháp và tag đặc thù Rails mà bạn có thể dùng cho Turbo. Hiện tại bạn chưa cần đọc gì; chỉ cần biết nó tồn tại để có thể quay lại khi cần tìm cách dùng một thành phần Turbo cụ thể trong ứng dụng.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- [Thông tin tham khảo về Turbo](https://turbo.hotwired.dev/reference/drive)
- Hãy xem hướng dẫn này về [Turbo Frames và Turbo Stream](https://www.hotrails.dev/turbo-rails/turbo-frames-and-turbo-streams) để xây dựng trình chỉnh sửa câu trích dẫn.Để bắt đầu ngay, hãy nhập vào dòng lệnh `rails new quote-editor;cd quote-editor;rails g scaffold quote name:string` rồi chạy `rails db:migrate`.
- [Diễn đàn Hotwire chính thức](https://discuss.hotwired.dev/)
- Hãy nhớ rằng bạn có thể dùng công cụ dành cho nhà phát triển của trình duyệt để [theo dõi hoạt động mạng](https://developer.chrome.com/docs/devtools/network/) và xem điều gì đang xảy ra với các yêu cầu và phản hồi Turbo. Nếu có gì đó không hoạt động, hãy kiểm tra xem trình duyệt có nhận được thông báo lỗi Rails hay không. Hãy tìm dòng chữ màu đỏ.
