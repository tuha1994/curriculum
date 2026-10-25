### Giới thiệu

Router là người gác cửa của ứng dụng. Khi một HTTP request đến từ trình duyệt của người dùng, router cần biết action (method) nào của controller cần được chạy. Có nên hiển thị trang web “người dùng mới” không? Hay nên chỉnh sửa một người dùng hiện có bằng dữ liệu được gửi kèm?

Về cơ bản, Router chỉ là một dịch vụ đối sánh. Nó xem xét HTTP verb (GET, POST, PUT, DELETE) và URL được yêu cầu, rồi đối sánh chúng với action tương ứng của controller để chạy. Đây là một chức năng cơ bản nhưng thiết yếu. Nếu không tìm được route phù hợp với request, ứng dụng sẽ phát sinh lỗi.

Một điều hữu ích khác diễn ra khi request đi vào ứng dụng là Rails lấy tất cả parameter đi kèm và cung cấp chúng cho bạn trong một hash đặc biệt gọi là `params`, để sau đó bạn có thể dùng trong controller. Điều này rất hữu ích với những việc như gửi form, vì bạn có thể dùng dữ liệu form đó để tạo hoặc chỉnh sửa object.

Nếu mở file routes trong ứng dụng Rails (nằm tại `config/routes.rb`), bạn sẽ thấy một liên kết đến phần routing của Rails Guides. Tài liệu này giải thích khá rõ cách hoạt động, vì vậy bạn sẽ không dễ bị lạc hướng.

Nhiều khóa học và hướng dẫn thường lướt qua routes, và khi nhìn lại chúng có vẻ khá dễ, nhưng lúc học Rails, bạn rất dễ mắc kẹt ở việc chính xác thì điều gì đang diễn ra. May mắn là việc nhập `$ rails routes` trên dòng lệnh sẽ cho ra danh sách tất cả route có sẵn trong ứng dụng. Trong phần này, chúng ta sẽ tìm hiểu điều thực sự diễn ra với file này.

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài học.

- Cấu hình route root.
- Cấu hình route RESTful cho một resource.
- Cấu hình route tùy chỉnh cho một resource.
- 7 controller action RESTful.
- Cách lấy danh sách tất cả route khả dụng cho ứng dụng Rails hiện tại.
- Các method helper để tạo liên kết điều hướng trên webpage.

### Route gốc

Route quan trọng nhất trong file của bạn là URL root... khi người dùng truy cập `http://supercutekittenphotos.com`? Họ sẽ được đưa đến đâu? Chỉ cần nói cho Rails controller và action nào sẽ xử lý route đó:

```ruby
  root to: "kittens#index"  #kittens controller, index action (method)
```

Hãy nhớ rằng khi nói “action”, chúng ta thực sự muốn nói đến “method bên trong controller được gọi như vậy”; ví dụ, action `index` chỉ là method `index` được định nghĩa trong KittensController*

### Route RESTful

Nếu bạn còn nhớ phần thảo luận trước đây về REST, về cơ bản có bảy loại action chính mà bạn có thể (và nên) thực hiện với một “resource”, tức một object như bài đăng blog hoặc người dùng... một thứ có model cơ sở dữ liệu riêng. Theo phần thảo luận đó, chúng là:

1. GET tất cả bài đăng (hay **“index”** các bài đăng)
1. GET một bài đăng cụ thể (hay **“show”** bài đăng đó)
1. GET trang cho phép bạn tạo bài đăng mới (hay xem trang bài đăng **“new”**)
1. POST dữ liệu bạn vừa điền cho bài đăng mới về server để server có thể tạo bài đăng đó (hay **“create”** bài đăng)
1. GET trang cho phép bạn chỉnh sửa bài đăng hiện có (hay xem trang bài đăng **“edit”**)
1. PUT dữ liệu bạn vừa điền để chỉnh sửa bài đăng về server để server thực sự thực hiện việc cập nhật (hay **“update”** bài đăng)
1. DELETE một bài đăng cụ thể bằng cách gửi delete request đến server (hay **“destroy”** bài đăng)

Các từ được nhấn mạnh tương ứng với những controller action chuẩn của Rails!

Mỗi mục trên đại diện cho một route “RESTful”, vì vậy bạn cần có cách viết chúng trong file Router để các request mà chúng đại diện thực sự được chuyển đến action phù hợp của controller (trong trường hợp này là controller “Posts”). Một cách viết dài dòng sẽ là:

```ruby
  get "/posts", to: "posts#index"
  get "/posts/new", to: "posts#new"
  get "/posts/:id", to: "posts#show"
  post "/posts", to: "posts#create"  # usually a submitted form
  get "/posts/:id/edit", to: "posts#edit"
  put "/posts/:id", to: "posts#update" # usually a submitted form
  delete "/posts/:id", to: "posts#destroy"
```

Về cơ bản, mỗi route này là một Ruby method đối sánh URL và HTTP verb cụ thể với action đúng của controller. Có hai điều cần lưu ý:

1. Điều quan trọng đầu tiên là một số route trong đó gửi đến CÙNG MỘT URL... chúng chỉ dùng các HTTP verb khác nhau, nhờ vậy Rails có thể gửi chúng đến các controller action khác nhau. Đây là điều khiến nhiều người mới bắt đầu nhầm lẫn.
1. Điều thứ hai cần lưu ý là trường “id” được thêm dấu hai chấm ở phía trước... điều đó chỉ nói với Rails rằng “Hãy tìm bất kỳ giá trị nào ở đây và lưu nó dưới dạng ID trong params hash”. Nhờ vậy, bạn có thể gửi GET request cho bài đăng thứ nhất và bài đăng thứ năm đến cùng một route, chỉ khác ID:

```ruby
  /posts/1  # going to the #show action of the PostsController
  /posts/5  # also going to the #show action of the PostsController
```

Bạn có thể truy cập ID đó trực tiếp từ controller bằng cách lấy nó trong params hash, nơi nó được lưu lại.

### Cách Rails viết route RESTful

Rails biết rằng bạn sẽ thường xuyên muốn dùng bảy action đó... vì vậy họ tạo ra một method helper tiện dụng, cho phép bạn làm trong một dòng điều mà chúng ta vừa viết thành bảy dòng trong file resources:

```ruby
  # in config/routes.rb
  ...
  resources :posts
  ...
```

Chỉ vậy thôi. Đó là một Ruby method về cơ bản chỉ xuất ra bảy route mà chúng ta đã nói đến. Không có phép màu nào cả. Bạn sẽ thấy nó xuất hiện rất nhiều; giờ thì bạn đã biết nó làm gì.

### Rails routes và route helper

Với dòng trên trong file routes, các route của chúng ta trông như thế nào? Nếu nhập `$ rails routes` trên dòng lệnh, lệnh này sẽ xuất ra tất cả route mà ứng dụng biết, chẳng hạn:

```bash
  edit_post  GET  /posts/:id/edit(.:format)  posts#edit
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Xem Rails routes

Đầu ra của lệnh `rails routes` có thể bị định dạng sai nếu terminal của bạn đang phóng to hoặc màn hình không đủ rộng để hiển thị thông tin route. Nếu vậy, bạn có thể thử thu nhỏ terminal bằng phím tắt <kbd>Ctrl</kbd> + <kbd>-</kbd>. Bạn cũng có thể dùng lệnh sau để xem danh sách dài và đầy đủ hơn của từng route:

```bash
rails routes --expanded
```

Ngoài ra, nếu server đang chạy, bạn có thể xem routes trong trình duyệt bằng cách truy cập `localhost:3000/rails/info/routes`. Một điểm hay là bạn sẽ có thanh tìm kiếm để tìm trong các route của dự án.

</div>

Bạn có thể thấy HTTP verb và URL đầu vào ở các cột giữa, sau đó là controller action mà chúng ánh xạ tới ở bên phải; tất cả hẳn khá quen thuộc vì bạn vừa viết chúng trong file routes. `(.:format)` chỉ có nghĩa là bạn có thể, nhưng không bắt buộc phải, chỉ định phần mở rộng file như `.doc` ở cuối route... dù sao nó cũng sẽ được lưu trong `params` hash để dùng sau. Nhưng cột ngoài cùng bên trái là gì? Đó là “tên” của route.

Có nhiều tình huống bạn muốn lấy URL của một route cụ thể, chẳng hạn khi muốn hiển thị liên kết điều hướng trên webpage (ĐỪNG hard-code URL, vì bạn sẽ gặp rắc rối khi quyết định thay đổi URL và phải tự vào sửa từng chỗ). Rails cung cấp một method helper tên là `link_to` để tạo liên kết, nhưng bạn cần truyền vào đó đoạn text muốn hiển thị và URL mà liên kết trỏ tới.

```ruby
  link_to "Edit this post", edit_post_path(3) # don't hardcode 3!
```

Ở đây chúng ta đi trước một chút, nhưng trong trường hợp này, đối số thứ hai phải là một path hoặc URL, nên chúng ta dùng path helper method để tạo nó. `edit_post_path(3)` sẽ tạo path `/posts/3/edit`.

Rails tự động tạo các method helper tương ứng với tên của tất cả route. Các method này kết thúc bằng `_path` và `_url`. `path`, như trong `edit_post_path(3)`, chỉ tạo phần path của URL, đủ dùng cho hầu hết ứng dụng. `url` sẽ tạo URL đầy đủ.

Mọi route yêu cầu bạn chỉ định ID hoặc parameter khác cũng sẽ yêu cầu bạn truyền chúng vào method helper (như chúng ta đã làm ở trên với edit). Bạn cũng có thể thêm query string bằng cách bổ sung một parameter:

```ruby
  post_path(3, :referral_link => "/some/path/or/something")
```

Khi đó, parameter `:referral_link` sẽ có sẵn trong `params` hash của controller, bên cạnh tập parameter thông thường.

### Route dẫn đến controller action!

Để nhấn mạnh rằng route tương ứng trực tiếp với controller action, một controller mẫu đáp ứng các route được tạo bởi `resources :posts` ở trên có thể trông như sau:

```ruby
  # in app/controllers/posts_controller.rb
  class PostsController < ApplicationController

    def index
      # code to grab all posts so they can be
      # displayed in the Index view (index.html.erb)
    end

    def show
      # code to grab the proper Post so it can be
      # displayed in the Show view (show.html.erb)
    end

    def new
      # code to create an empty post and send the user
      # to the New view for it (new.html.erb), which will have a
      # form for creating the post
    end

    def create
      # code to create a new post based on the parameters that
      # were submitted with the form (and are now available in the
      # params hash)
    end

    def edit
      # code to find the post we want and send the
      # user to the Edit view for it (edit.html.erb), which has a
      # form for editing the post
    end

    def update
      # code to figure out which post we're trying to update, then
      # actually update the attributes of that post. Once that's
      # done, redirect us to somewhere like the Show page for that
      # post
    end

    def destroy
      # code to find the post we're referring to and
      # destroy it.  Once that's done, redirect us to somewhere fun.
    end
  end
```

Hãy nhớ rằng bạn có thể chạy `$ rails routes` trong thư mục dự án để xem tất cả route cùng controller và action tương ứng.

### Chúng ta không muốn cả bảy route!

Đôi khi bạn không muốn cả bảy route RESTful mà `resources` cung cấp. Rất đơn giản: hãy chỉ định những route muốn dùng bằng `only`, hoặc những route KHÔNG muốn dùng bằng `except`:

```ruby
  resources :posts, only: [:index, :show]
  resources :users, except: [:index]
```

### Route không theo RESTful

Dĩ nhiên, bạn không nhất thiết phải làm mọi thứ theo cách RESTful. Có lẽ bạn nên làm vậy, nhưng đôi khi bạn muốn tự tạo route và ánh xạ nó đến controller action của riêng mình. Chỉ cần làm theo các ví dụ về route RESTful ở phần đầu:

```ruby
  get '/somepath', to: 'somecontroller#someaction'
```

... dĩ nhiên, các comment trong `config/routes.rb` cũng sẽ hữu ích trong trường hợp này.

### Bài tập

Đến đây, có lẽ bạn đã hiểu khá rõ chuyện gì đang diễn ra trong file routes, nhưng cũng có thể còn rất nhiều câu hỏi. Rails Guides sẽ giúp bạn!

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương Routing của Rails Guides](http://guides.rubyonrails.org/routing.html), các phần 1-2.5, 3.1-3.4, 4.6 và 6.1.
1. Xem [Video giải thích tuyệt vời về cách REST và HTTP hoạt động](https://www.youtube.com/watch?v=Q-BpqyOT3a8). Bạn có thể làm theo hướng dẫn bằng `curl https://api.github.com`.

   <div class="lesson-note lesson-note--tip" markdown="1">

   #### Không sử dụng tiện ích mở rộng Postman cho Chrome

   Tiện ích mở rộng Postman cho Chrome xuất hiện trong video đã bị ngừng hỗ trợ. Hãy dùng [ứng dụng Postman](https://www.postman.com/downloads/) bản native hoặc dùng phiên bản web.

   </div>

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu bổ sung.

- [Bài viết Medium về routing trong Rails](https://medium.com/podiihq/understanding-rails-routes-and-restful-design-a192d64cbbb5). Bài viết trình bày nhiều nội dung giống Rails Guides, nhưng với giọng điệu hơi khác mà một số người có thể thấy dễ tiếp thu hơn.