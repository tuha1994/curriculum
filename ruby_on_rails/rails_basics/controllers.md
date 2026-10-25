### Giới thiệu

Nhiệm vụ của controller thực chất là làm người trung gian tối cao. Nó biết mình muốn hỏi Model những câu hỏi nào, nhưng để model đảm nhận toàn bộ phần nặng nhọc trong việc giải quyết các câu hỏi đó. Nó biết muốn render view nào và gửi view đó trở lại trình duyệt, nhưng để chính view lo việc ghép toàn bộ HTML. Đó là lý do nó được gọi là "controller"... đủ thông minh để biết phải làm gì rồi ủy thác mọi phần việc khó. Tất cả những gì nó làm là tập hợp đúng nhóm các biến instance để gửi sang view.

Khi nào controller được sử dụng? Sau khi một HTTP request đi vào ứng dụng và router quyết định ánh xạ request đó tới controller và action nào, Rails đóng gói tất cả tham số đi kèm request rồi chạy method được chỉ định trong controller tương ứng. Khi method đó hoàn tất, Rails sẽ lấy mọi biến instance bạn đã cung cấp trong method của controller rồi chuyển chúng sang file view thích hợp, để chúng được chèn vào template HTML ("View") và cuối cùng gửi trở lại trình duyệt.

Mọi việc khá dễ hiểu. Các controller điển hình khá gọn nhẹ, không có quá nhiều code nhưng vẫn làm được rất nhiều việc với số code đó. Nếu bạn muốn hiển thị tất cả bài viết blog trên trang index của website thì sao? Hãy chạy action `#index` của controller Posts; action này sẽ lấy toàn bộ bài viết rồi gửi chúng sang file view `index.html.erb`, nơi quyết định bạn thực sự muốn hiển thị chúng như thế nào (một danh sách gạch đầu dòng khổng lồ? Những khung trông thật bắt mắt?).

Action `#index` của controller thực tế sẽ trông như sau:

```ruby
  PostsController < ApplicationController
    ...
    def index
      @posts = Post.all
    end
    ...
  end
```

Trong action này, controller yêu cầu model cung cấp một thứ gì đó ("Này, đưa cho tôi tất cả bài viết!"), đóng gói chúng vào biến instance `@posts` để view có thể sử dụng, rồi tự động render view tại `app/views/posts/index.html.erb` (chúng ta sẽ nói về việc này ngay sau đây).

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về những chủ đề bạn sẽ học trong bài.

- Những gì diễn ra bên trong một action controller RESTful.
- Ghi đè việc render ngầm định của file view.
- Những trường hợp nên dùng `redirect_to` thay vì `render` và ngược lại.
- Xử lý và hạn chế các parameter cùng dữ liệu POST được truyền tới controller.
- Flash message và cách hiển thị nó.

### Tên gọi rất quan trọng

Một cách Rails giúp cuộc sống của bạn dễ dàng hơn đôi chút là giả định mọi thứ được đặt tên theo một quy ước nhất định, rồi thực hiện chúng ở phía sau dựa trên những tên đó. Chẳng hạn, controller và action của nó phải được đặt tên giống như tên bạn đã khai báo trong file `routes.rb` khi ánh xạ một loại HTTP request cụ thể tới chúng.

Đầu bên kia của quy trình là việc controller làm gì sau khi hoàn tất. Khi Rails đi tới `end` của action controller đó, nó lấy tất cả biến instance từ controller và gửi sang file view *được đặt tên giống với action của controller* và nằm trong thư mục mang tên controller, chẳng hạn `app/views/posts/index.html.erb`. Đây không phải sự tùy tiện; quy ước này được thiết kế có chủ đích để sau này bạn dễ tìm file hơn. Nếu lưu file ở một thư mục hoặc hệ thống phân cấp khác, bạn sẽ phải chỉ rõ file nào cần được render.

### Render và redirect

Mặc dù Rails sẽ ngầm định render một file view có cùng tên với action của controller, có rất nhiều tình huống bạn muốn ghi đè hành vi đó. Một trường hợp chính là khi bạn thực sự muốn chuyển hướng hoàn toàn người dùng sang một trang mới thay vì render kết quả của action controller hiện tại.

Redirect thường xảy ra sau các action controller đã tiếp nhận thông tin được gửi lên, chẳng hạn khi tạo một Post mới. Không có lý do gì phải có file view `create.html.erb` chỉ để hiển thị một lần sau khi bài viết được tạo... thông thường chúng ta chỉ muốn xem bài viết vừa tạo, nên sẽ redirect tới trang Show của bài viết đó. Điểm khác biệt là ứng dụng của bạn xem redirect như *một HTTP request hoàn toàn mới*... vì vậy request sẽ đi qua router lần nữa, tìm trang Show tương ứng với bài viết đó rồi render trang như bình thường. Điều đó cũng có nghĩa mọi biến instance bạn đặt trong action controller `#create` ban đầu sẽ bị xóa trong quá trình này.

Nếu đó là cách phổ biến để xử lý việc tạo object thành công, vậy khi việc tạo thất bại vì lý do nào đó (chẳng hạn người dùng nhập tiêu đề bài viết quá ngắn) thì sao? Khi đó, bạn chỉ cần render view của một action controller khác, thường là chính action đã tạo form mà bạn vừa gửi đi (tức action `#new`).

Điểm mấu chốt là trang view sẽ nhận các biến instance từ action controller *hiện tại* của bạn. Giả sử bạn thử `#create` một Post và lưu nó vào `@post` nhưng thao tác lưu thất bại. Sau đó bạn render view của action `#new`, và view đó sẽ nhận `@post` mà bạn vừa xử lý trong action `#create`. Điều này rất hữu ích vì bạn không phải xóa sạch form (một trải nghiệm rất khó chịu đối với người dùng) -- bạn chỉ cần xác định những trường bị lỗi rồi yêu cầu người dùng gửi lại. Bây giờ nghe có vẻ hơi trừu tượng, nhưng khi bắt tay xây dựng bạn sẽ nhanh chóng thấy sự khác biệt.

Hãy xem qua code:

```ruby
  # app/controllers/posts_controller.rb
  class PostsController < ApplicationController
    ...
    # Make (but don't save) an empty Post so the form we render
    # knows which fields to use and where to submit the form
    # This action will render app/views/posts/new.html.erb once
    # it's done
    def new
      @post = Post.new
    end

    # We know this will get run once we receive the submitted
    # form from our NEW action above (remember your REST actions??)
    # We'll just use pseudo-code for now to illustrate the point
    def create
      # code here to set up a new @post based on form info
      if @post.save
        # code to set up congratulations message
        redirect_to post_path(@post.id) # go to show page for @post
      else
        # code to set up error message
        render :new, status: :unprocessable_entity
      end
    end
  end
```

Điều cần chú ý là: nếu lưu bài viết mới vào database thành công, chúng ta redirect tới trang show của bài viết đó. Lưu ý rằng một cách viết tắt bạn sẽ thường xuyên gặp là, thay vì viết `redirect_to post_path(@post.id)`, chỉ cần viết `redirect_to @post`, vì Rails biết mọi người làm vậy rất thường xuyên nên cung cấp sẵn cách viết ngắn gọn này. Chúng ta sẽ dùng nó trong ví dụ khi phát triển tiếp.

Điều kiện lỗi trong action `#create` ở trên sẽ render chính form đã được render trong action `#new`, nhưng lần này `@post` sẽ là object Post mà chúng ta đã thử lưu nhưng thất bại, nên nó cũng sẽ chứa một số lỗi. Các lỗi đó có thể được dùng để đánh dấu màu đỏ những trường form gây ra vấn đề.

### Nhiều lần render/redirect

Điều quan trọng cần nhớ là `render` và `redirect_to` KHÔNG dừng ngay action controller như một câu lệnh `return`. Vì vậy, bạn phải hết sức cẩn thận để logic của mình không dẫn tới việc chạy nhiều hơn một trong hai câu lệnh đó. Nếu không, bạn sẽ gặp lỗi. Những lỗi này thường khá dễ gỡ lỗi.

Nếu bạn viết một đoạn như sau:

```ruby
  def show
    @user = User.find(params[:id])
    if @user.is_male?
      render "show-boy"
    end
    render "show-girl"
  end
```

Trong mọi trường hợp người dùng là nam, bạn sẽ gặp lỗi multiple render vì đã yêu cầu Rails render cả "show-boy" lẫn "show-girl".

### Params và strong parameters

Trong ví dụ trên, chúng ta thấy `... code here to set up a new @post based on form info ...`. Được rồi, làm thế nào lấy được thông tin đó? Chúng ta liên tục nói rằng router đóng gói tất cả parameter được gửi cùng HTTP request ban đầu, nhưng truy cập chúng bằng cách nào?

Bằng object `params`! Object `params` là một instance của `ActionController::Parameters`, hoạt động tương tự một Ruby hash thông thường. Nó chứa các parameter của request được lưu dưới dạng các cặp `:key => value`. Vậy làm sao lấy ID của bài viết mà chúng ta đang yêu cầu? `params[:id]`. Bạn có thể truy cập mọi parameter theo cách này nếu chúng có "giá trị vô hướng", chẳng hạn string, number, boolean, `nil`... bất cứ thứ gì "phẳng".

Một số form sẽ gửi mỗi trường dưới dạng một mục vô hướng ở cấp cao nhất trong hash params, chẳng hạn `params[:post_title]` có thể là "My Test Post Title" còn `params[:post_body]` có thể là "Body of post!" v.v.; bạn có thể truy cập chúng mà không gặp vấn đề gì. Bạn có quyền kiểm soát cấu trúc này, như sẽ học trong các bài về form.

#### Tham số mạnh (Strong parameters)

Tuy nhiên, thường bạn muốn gửi các parameter từ trình duyệt đã được đóng gói gọn gàng vào một hash hoặc lồng trong một array. Điều đó có thể khiến cuộc sống dễ dàng hơn nhiều, vì bạn chỉ cần truyền thẳng hash đó vào `Post.new(your_hash_of_attributes_here)`, đúng với thứ mà `Post.new` vốn mong đợi! Chúng ta sẽ chưa đi sâu vào phần này cho tới các bài về Model và Form, nhưng bạn nên biết rằng cấu trúc dữ liệu được gửi từ form phụ thuộc hoàn toàn vào cách bạn thiết lập form (thực chất dựa trên cách bạn đặt tên các trường bằng thuộc tính HTML `name=''`).

Trong ví dụ này, chúng ta giả định `params[:post]` cung cấp một hash gồm các thuộc tính của Post như `{ :title => "My Test Post Title", :body => "this post rocks!", :author_id => "1"}`, đúng là thứ `Post.new` đang mong đợi. Việc parameter được đóng gói trong một hash hay nằm ở cấp cao nhất dưới dạng các thuộc tính riêng lẻ (như `params[:id]`) một lần nữa phụ thuộc vào bạn và cách bạn tạo form. Nhưng hãy nhớ rằng controller thường xử lý dễ hơn khi nhận được một hash thuộc tính Post được đóng gói gọn gàng.

Điểm khác biệt quan trọng giữa các giá trị parameter "vô hướng" như string với những parameter phức tạp hơn như hash và array là Rails 4 đã triển khai một số cơ chế bảo vệ trong controller, gọi là "Strong Parameters". Cơ chế này ngăn người dùng gửi dữ liệu gây hại (chẳng hạn tự động đặt mình làm admin khi tạo tài khoản). Để làm vậy, Rails buộc bạn phải xác nhận rõ ràng rằng mình chấp nhận những phần tử nhất định của một hash hoặc array.

<div class="lesson-note" markdown="1">

#### Whitelist các thuộc tính "an toàn" trong Rails 3

Trước đây, Rails 3 xử lý việc này bằng cách đặt `attr_accessible` trong model để cho phép các thuộc tính, vì vậy bạn có thể sẽ thấy cách này trong rất nhiều bài đăng trên Stack Overflow và các ứng dụng cũ.

</div>

Để cho phép parameter một cách rõ ràng, bạn có thể gọi method `#expect` trên `params`. Thông thường, method này nhận một key trùng với tên model bạn đang làm việc (tức là một `post`), theo sau là một array gồm các thuộc tính được phép. Ví dụ:

```ruby
  def allowed_post_params
    params.expect(post: [:title, :body, :author_id])
  end
```

Method này trả về hash chỉ gồm những params bạn cho phép rõ ràng (ví dụ `{:title => "your title", :body => "your body", :author_id => "1"}` ). Nếu không làm vậy, khi bạn thử truy cập params[:post] sẽ không có gì xuất hiện! Ngoài ra, nếu có trường bổ sung nào được gửi bên trong hash, chúng sẽ bị loại bỏ và không thể truy cập (để bảo vệ bạn).

Điều này có thể hơi bất tiện, nhưng đó là cách Rails bảo vệ bạn khỏi những người dùng xấu. Thông thường, bạn sẽ gom các helper strong parameter này vào một method private riêng ở cuối controller, rồi gọi method đó tại nơi cần lấy những parameter cụ thể.

Vì vậy, action `#create` của chúng ta giờ có thể được viết đầy đủ hơn một chút:

```ruby
  # app/controllers/posts_controller.rb
  class PostsController < ApplicationController
    ...

    # We know this will get run once we've received the submitted
    # form from our new action above (remember your REST actions??)
    def create
      @post = Post.new(allowed_post_params) # see method below
      if @post.save
        # code to set up congratulations message
        redirect_to post_path(@post.id) # go to show page for @post
      else
        # code to set up error message
        render :new, status: :unprocessable_entity
      end
    end

    private  # Best to make helper methods like this one private

    # gives us back just the hash containing the params we need to
    # to create or update a post
    def allowed_post_params
      params.expect(post: [:title, :body, :author_id])
    end
  end
```

<div class="lesson-note lesson-note--warning" markdown="1">

#### Xử lý strong parameters trước Rails 8

Trước Rails 8, strong parameters được xử lý theo cách khác. Thay vì method `#expect`, bạn phải gọi `#require` trên tên key cấp cao nhất, rồi gọi `#permit` với danh sách thuộc tính. Ví dụ:

```ruby
def allowed_post_params
  params.require(:post).permit(:title, :body, :author_id)
end
```

Cách này vẫn hoạt động, nhưng có một vài lỗ hổng bảo mật, là động lực thúc đẩy việc phát triển `#expect`. Tuy nhiên, bạn có thể sẽ gặp cách làm này trong các dự án cũ, bài blog và câu trả lời trên StackOverflow. Chỉ cần biết rằng nó thực hiện cùng chức năng với method `#expect` mới.

</div>

### Flash message

Phần code cuối cùng chúng ta cần viết ở đây là cách thiết lập các thông báo đặc biệt cho người dùng. Rails cung cấp một công cụ tiện lợi để gửi thông báo thành công và lỗi (chẳng hạn thông báo màu xanh lá nhỏ xuất hiện thoáng qua ở đầu ứng dụng để chúc mừng bạn đăng ký thành công), gọi là "flash". Nó hoạt động giống một hash -- bạn có thể gán cho các key của nó những thông báo cụ thể, rồi truy cập chúng nếu muốn hiển thị trong view.

Bạn có thể dùng bất kỳ key nào cho flash, nhưng theo thông lệ, người ta thường chỉ dùng ba key là `:success`, `:error` và `:notice`. Vì vậy, thông báo thành công ở trên có thể được viết là `flash[:success] = "Great! Your post has been created!"`.

Lý do bạn có thể dùng bất kỳ key nào là vì dù sao bạn cũng phải viết một đoạn code trong view để hiển thị flash, nhưng bám theo các key thông dụng vẫn là thực hành tốt. Một mẹo thú vị khác của flash là nó tự động xóa sau khi được sử dụng, nên bạn không phải lo nó hiển thị mỗi lần truy cập trang mới... chỉ dùng một lần. Giống Snapchat.

Tuy nhiên, còn một điểm phân biệt cuối cùng liên quan tới sự khác nhau giữa redirect và render. Hãy nhớ rằng redirect gửi một HTTP request hoàn toàn mới, về cơ bản rời bỏ ứng dụng hiện tại và bắt đầu lại từ đầu. Chúng ta mất toàn bộ dữ liệu... ngoại trừ flash. Flash được thiết kế đặc biệt để đi cùng HTTP request đó, nhờ vậy bạn vẫn có thể truy cập nó khi được redirect sang trang tiếp theo.

Render không đi xa đến vậy -- nó chỉ dùng một file view thuộc luồng bình thường của ứng dụng, và bạn có quyền truy cập mọi biến instance trong file đó. Vì flash là một trường hợp đặc biệt, khi chỉ render view thay vì gửi một request hoàn toàn mới, bạn phải dùng `flash.now` thay cho `flash`. Cách viết sẽ là `flash.now[:error] = "Rats! Fix your mistakes, please."`.

Sự phân biệt giữa `flash` và `flash.now` chỉ giúp Rails biết khi nào cần cung cấp flash cho bạn... nếu dùng `flash` ở nơi lẽ ra phải dùng `flash.now`, bạn sẽ bắt đầu thấy thông báo xuất hiện "trễ một trang", và khi đó nguyên nhân chắc sẽ khá rõ.

Bây giờ, toàn bộ code controller cho action `#create` có thể được viết như sau:

```ruby
  # app/controllers/posts_controller.rb
  class PostsController < ApplicationController
    ...

    # We know this will get run once we've received the submitted
    # form from our new action above (remember your REST actions??)
    def create
      @post = Post.new(allowed_post_params)
      if @post.save
        flash[:success] = "Great! Your post has been created!"
        redirect_to @post # go to show page for @post
      else
        flash.now[:error] = "Rats! Fix your mistakes, please."
        render :new, status: :unprocessable_entity
      end
    end

    private

    def allowed_post_params
      params.expect(post: [:title, :body, :author_id])
    end
  end
```

Như vậy, action đó đã làm khá nhiều việc -- lấy dữ liệu form, tạo một post mới, thử lưu post, thiết lập thông báo thành công và redirect tới post nếu thành công, đồng thời xử lý trường hợp thất bại bằng cách mắng bạn vì sự ngớ ngẩn rồi render lại form. Chỉ 10 dòng Ruby mà làm được cả đống việc. Đó mới là điều khiển thông minh.

### Bài tập

Đây mới chỉ là phần làm quen với Rails controller, nhưng hẳn bạn đã có ý niệm khá rõ về những gì đang diễn ra và những thủ thuật có thể sử dụng.

<div class="lesson-content__panel" markdown="1">

  1. Đọc [chương Controllers trong Rails Guides](http://guides.rubyonrails.org/action_controller_overview.html), các mục 1 - 4.3 và mục 6.2. Chúng ta sẽ học kỹ hơn về session (mục 6.1) sau, nên hiện tại bạn chưa cần lo về phần đó.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- [Video minh họa Controller và Routes](https://vimeo.com/168501163)
- [Rails 3 Rendering và Partials qua YouTube](http://www.youtube.com/watch?v=m-tw2OCHPMI)