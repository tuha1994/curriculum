### Giới thiệu

Có một số chủ đề mà chúng ta chưa có dịp tìm hiểu, nhưng sẽ rất hữu ích nếu bạn biết đến chúng. Trong phần này, chúng ta sẽ tìm hiểu về định tuyến nâng cao, layout và phần giới thiệu ngắn về siêu lập trình.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài.

- Tài nguyên số ít.
- Các route lồng nhau.
- Route thành viên và route tập hợp.
- Lồng các layout.
- Siêu lập trình là gì.
- Mẫu thiết kế là gì.

### Định tuyến nâng cao

Đến lúc này, hẳn bạn đã khá quen với phần cốt lõi của định tuyến -- chuyển các request RESTful dùng những HTTP verb quen thuộc thành ánh xạ tới các action cụ thể của controller (dù dùng phương thức `#resources` hay khai báo tường minh bằng phương thức `get`). Đó là 90% công việc bạn sẽ làm với file routes... nhưng 10% còn lại đem đến một số tùy chọn khá thú vị, chẳng hạn chuyển hướng trực tiếp từ file routes, lồng các route vào nhau hoặc phân tích tham số từ request gửi đến.

#### Tài nguyên số ít

Có thể bạn đã gặp trường hợp này nhưng chưa thực sự hiểu. Từ trước đến nay, chúng ta nói về các resource (như "posts" và "users") trong đó có rất nhiều bản ghi. Điều này khá trực quan. Trong file `config/routes.rb`, bạn biểu diễn chúng bằng một dòng như `resources :users`.

Đôi khi cũng có những resource mà về bản chất chỉ nên có một. Ví dụ như dashboard của User, hiển thị các thông tin thú vị dựa trên user đang đăng nhập. Chỉ có một template dashboard, nhưng nó đủ thông minh để hiển thị những nội dung phù hợp với user hiện đang đăng nhập.

Trong trường hợp này, việc hiển thị một "index" của các dashboard không thật sự hợp lý, vì chỉ có một dashboard (nó chỉ thay đổi tùy theo người đăng nhập). Ta cũng có thể nói rằng, với những action khác vốn cần một ID để phân biệt resource đang thao tác (như `#show`), vì chỉ có một resource nên không còn cần tham số `id` nữa.

Dòng trong file routes cho một resource số ít sẽ có dạng:

```ruby
  # in config/routes.rb
  resource :dashboard
```

Lưu ý rằng từ "resource" là số ít và `dashboard` cũng vậy. Điều này khiến nhiều người nhầm khi viết "resource" thay vì "resources" trong lúc thực sự muốn dùng resource số nhiều (vốn phổ biến hơn).

Kết quả `$ rails routes` cho một resource số ít chỉ có 6 route (vì không còn dùng `#index`), và bạn sẽ không còn thấy phần `:id` trong các route, chẳng hạn:

```bash
  edit_dashboard  GET /dashboard/edit(.:format)  dashboards#edit
```

...so với phiên bản số nhiều của cùng route:

```bash
  edit_dashboard  GET /dashboards/:id/edit(.:format)  dashboards#edit
```

### Route lồng nhau

Đôi khi việc một resource nằm bên trong resource khác là hợp lý. Chẳng hạn, danh sách lesson như thế này về mặt logic thuộc về danh sách course -- vì vậy bạn có thể mong đợi một URL dạng `http://example.com/courses/1/lessons/3`. Để tạo kiểu lồng này, trong file routes ta lồng một resource bên trong block được truyền cho resource khác, chẳng hạn:

```ruby
  # config/routes.rb
  TestApp::Application.routes.draw do
    resources :courses do
      resources :lessons
    end
  end
```

Lưu ý rằng phương thức `#resources` lúc này nhận một block, trong đó có một tập hợp các route.

Khi truy cập URL, bạn sẽ phải chỉ định tham số `:id` cho CẢ HAI đối tượng. Kết quả `$ rails routes` cho ví dụ trên sẽ bao gồm một route tương tự:

```ruby
  course_lesson  GET  /courses/:course_id/lessons/:id(.:format)  lessons#show
```

Cũng cần lưu ý rằng request được đưa đến controller của resource nằm sâu nhất, và đó cũng là tham số `:id` được gọi là `:id` (các tham số của resource cha, như trong ví dụ trên, sẽ có tên cụ thể như `:course_id`).

Các view helper cũng được tự động tạo theo cách hợp lý (như bạn có thể thấy trong output của `$ rails routes`). Khi dùng các view helper như `#course_lesson_path`, bạn sẽ cần chỉ định cả hai tham số theo đúng thứ tự, chẳng hạn `course_lesson_path(1,3)`.

Đừng lồng route quá sâu! Nếu sâu hơn một hoặc hai tầng thì có lẽ bạn nên thiết kế khác đi. Thực tế, nhiều khi bạn chỉ thấy một số action của controller được lồng -- chỉ những action thực sự *cần* ID của resource cha để xác định duy nhất resource. Chẳng hạn, bạn có thể lấy một Lesson cụ thể chỉ bằng ID của nó. Nhưng để lấy tất cả lesson được liệt kê bên dưới một Course cụ thể, bạn cần Course ID nên route phải được lồng. Việc tạo lesson cũng tương tự, vì lesson cần có resource cha:

```ruby
  # config/routes.rb
  TestApp::Application.routes.draw do
    resources :courses do
      resources :lessons, :only => [:index, :create]
    end
  end
```

Nếu ban đầu điều này hơi khó hiểu, bạn sẽ nhanh chóng nắm được khi gặp nó trong code của mình. Nếu trong controller bạn thấy mình cần ID của resource cha thì route lẽ ra phải được lồng. Nếu không cần ID của resource cha thì không cần lồng. Đơn giản vậy thôi.

#### Route thành viên và route tập hợp

Đôi khi bạn muốn thêm một route không RESTful khác cho một resource. Nếu muốn thêm route chỉ áp dụng cho một member của resource đó, hãy dùng phương thức `#member`:

```ruby
  # config/routes.rb
  TestApp::Application.routes.draw do
    resources :courses do
      member do
        get "preview"   # Preview a single course
      end
    end
  end
```

Route đó sẽ ánh xạ tới action `courses#preview`. Bạn có thể thêm bao nhiêu route tùy thích.

Nếu muốn thêm route không RESTful cho toàn bộ collection của resource (nên không cần chỉ định thuộc tính `:id`, giống action `index`), bạn dùng phương thức `#collection`:

```ruby
  # config/routes.rb
  TestApp::Application.routes.draw do
    resources :courses do
      member do
        get "preview"  # Preview a single course (requires ID)
      end
      collection do
        get "upcoming"  # Show a list of *all* upcoming courses (no ID needed)
      end
    end
  end
```

Route `upcoming` sẽ ánh xạ tới action `courses#upcoming` nhưng không nhận tham số `:id`.

Nếu thấy phần nào khó hiểu, cứ thử dùng chúng và chạy `$ rails routes` để xem điều gì đang diễn ra phía sau.

#### Chuyển hướng và route wildcard

Có thể bạn muốn cung cấp một URL thuận tiện cho người dùng nhưng ánh xạ trực tiếp nó tới một URL khác mà bạn đang dùng. Hãy dùng redirect:

```ruby
  # config/routes.rb
  TestApp::Application.routes.draw do
    get 'courses/:course_name' => redirect('/courses/%{course_name}/lessons'), :as => "course"
  end
```

Chỉ vậy mà đã có khá nhiều điều thú vị. Nguyên tắc cơ bản là dùng phương thức `#redirect` để chuyển một route tới route khác. Nếu route của bạn đơn giản, cách này rất thẳng thắn. Nhưng nếu muốn truyền cả các tham số ban đầu, bạn cần thực hiện thêm một chút bằng cách bắt tham số bên trong `%{here}`. Hãy lưu ý dấu nháy đơn bao quanh mọi thứ.

Trong ví dụ trên, chúng ta cũng đổi tên route cho tiện bằng cách dùng bí danh với tham số `:as`. Nhờ đó, ta có thể dùng tên này trong các phương thức như helper `#_path`. Một lần nữa, hãy thử chạy `$ rails routes` và tự đặt câu hỏi về kết quả.

### Controller, model và việc giữ tính RESTful

Bên cạnh các chủ đề định tuyến nâng cao, việc suy nghĩ về những controller Rails không nhất thiết có ActiveRecord model riêng để làm việc cùng cũng hữu ích. Giả sử ứng dụng cần cho phép một `lesson` có các `images` đi kèm. Việc này có vẻ đơn giản, nên ta có thể cập nhật model:

```ruby
# app/models/lesson.rb
class Lesson < ApplicationRecord
  # other stuff
  has_many_attached :images
end
```

Sau đó, ta suy nghĩ về cách quản lý các image này ở phía route và controller. Ban đầu, ta có thể nghĩ đến cách như sau:

```ruby
# config/routes.rb
resources :lessons do
  member do
    patch :attach_image
    delete :remove_image
  end
end
```

Tiếp theo, ta có các method tương ứng trong `LessonsController` để xử lý image. Cách này đủ dùng, nhưng khi nhìn controller Rails như một khái niệm độc lập, ta có thể chọn triển khai tính năng theo cách khác. Hãy xem cách tiếp cận thứ hai:

```ruby
# config/routes.rb
resources :lessons do
  resources :images, only: [:create, :delete]
end
```

Cùng với một controller mới là `Lessons::ImagesController`, có dạng:

```ruby
# app/controllers/lessons/images_controller.rb
module Lessons
  class ImagesController < ApplicationController
    def create
      # logic to attach images to a course
    end

    def destroy
      # logic to remove an image image from a course
    end
  end
end
```

Điều chúng ta đã làm là khiến cách triển khai mang tính RESTful hơn, vì không còn action tùy chỉnh không RESTful nào. Thay vào đó, ta có hẳn một controller mới (RESTful). Controller này không gắn với model riêng để xử lý các action, mà làm việc trên model `Lesson`. Không chỉ vậy, nhờ controller mới, ta có thể bám sát các action REST để mô tả việc đang làm: *tạo* một image attachment mới hoặc *hủy* một image của lesson.

Thực ra, nếu suy nghĩ kỹ, điều này đã được thể hiện ngầm trong cách thử ban đầu: `attach_image` và `remove_image` đều theo mẫu `<action>_<noun>`, có thể là dấu hiệu cho thấy ta nên dùng một resource khác. Ở đây, chúng ta cũng dùng một controller lồng để cho developer tiếp theo biết ý định của mình. Khi giữ các action trong phạm vi RESTful, việc mở rộng ứng dụng sẽ trở nên dễ dàng. Giữ mọi thứ RESTful cũng giúp hiểu chuyện gì đang diễn ra mà không cần đào quá sâu vào code.

Khi có thể suy nghĩ vượt ra ngoài sự gắn kết controller/model trong Rails, nhiều khả năng sẽ mở ra. Thậm chí bạn có thể có một controller gắn với một thuộc tính cụ thể của model!

Để xem thêm thông tin và ví dụ, bạn nên đọc bài nói chuyện xuất sắc của Derek Prior có tên "In Relentless Pursuit of REST" trong phần tài nguyên bổ sung.

### Layout nâng cao: Lồng layout và truyền thông tin

Trong bài về Views, chúng ta đã tìm hiểu khá kỹ về view layout, nhưng còn một chủ đề khác: render nhiều layout cho một trang. Cách này cho phép bạn tạo các section riêng mà vẫn tái sử dụng nhiều phần style cần nhất quán trên toàn site (ví dụ footer). Chẳng hạn, các trang user có thể cần style khác với trang chủ. Ý nghĩ đầu tiên có thể là dùng một stylesheet khác cho từng layout, nhưng hãy nhớ rằng Asset Pipeline của Rails dù sao cũng gộp tất cả stylesheet lại.

Một cách tốt hơn là yêu cầu layout thực hiện một số việc (bất kỳ việc gì layout thường làm), rồi render *một* layout khác bằng phương thức `render :template => "your_layout.html.erb"`. Bạn có thể hình dung layout này dùng layout kia gần giống như một view dùng view partial.

Bạn cũng có thể truyền thông tin từ layout đầu tiên sang layout mà nó render bằng phương thức `#content_for`. Nhờ đó, bạn có thể tạo logic trong layout chính phụ thuộc vào nội dung do các file layout riêng lẻ truyền vào... khả năng là vô tận.

Ví dụ, bạn có thể có một file layout riêng cho các trang tĩnh là `app/views/layouts/static_pages.html.erb`. Theo mặc định, file này sẽ được render (nếu tồn tại) cho các view tạo từ StaticPagesController (một controller mặc định của Rails). Tuy nhiên, giả sử bạn muốn các trang tĩnh trông gần giống toàn bộ site nhưng không muốn navbar xuất hiện ở phía trên.

Trong trường hợp này, bạn sẽ yêu cầu layout `static_pages.html.erb` gọi layout `application.html.erb`, đồng thời truyền cho nó một số CSS đặc biệt bằng phương thức `#content_for`, chẳng hạn:

```erb
  # app/views/layouts/static_pages.html.erb

  <% content_for :stylesheets do %>
    #navbar {display: none}
  <% end %>
  <%= render :template => "layouts/application" %>
```

Sau đó, layout `application.html.erb` cần được thiết lập để nhận và sử dụng nội dung đó, chẳng hạn bằng cách thêm dòng `#yield` này:

```erb
  # app/views/layouts/application.html.erb
  ...
  <head>
    ...
    <style><%= yield :stylesheets %></style>
  </head>
  ...
  render :template => "static_pages.html.erb"
  ...
```

Khi bạn `#yield` tới một content block cụ thể, trong trường hợp này là `:stylesheets`, nó sẽ đưa đoạn code bên trong block của `content_for` vào vị trí mà method `#yield` xuất hiện. Vì vậy, trong ví dụ trên, ta đã thêm một số style CSS vào application layout bằng cách trước tiên render layout `static_pages.html.erb` đặc biệt, rồi truyền style sang layout `application.html.erb` chính bằng `#content_for`. Kết quả sẽ là:

```erb
  # app/views/layouts/application.html.erb
  ...
  <head>
    ...
    <style> #navbar {display: none} </style>
  </head>
  ...
```

Mẹo này hữu ích cho nhiều việc hơn là chỉ truyền thông tin stylesheet... bất cứ khi nào muốn một section của site trông khác đi nhưng không muốn thiết kế lại hoàn toàn bằng một layout mới, bạn có thể cân nhắc lồng các layout và truyền thông tin từ layout này sang layout khác.

### Siêu lập trình trong Rails

"Siêu lập trình" là gì? Đây là một khái niệm tuyệt vời và hữu ích, được sử dụng khắp nơi trong Rails, và bạn cũng có thể tự mình áp dụng. Về cơ bản, đó là ý tưởng rằng ứng dụng hoặc script có thể tạo function, method hoặc class ngay trong lúc chạy, đồng thời cũng có thể gọi chúng một cách động. Đây là một trong những điểm mạnh của ngôn ngữ thông dịch như Ruby... gần như đã được tích hợp sẵn vào ngôn ngữ. Ở đây chúng ta chỉ lướt qua bề mặt, nhưng chắc chắn bạn nên tìm hiểu sâu hơn khi đã thấy thoải mái với những phần cốt lõi của Rails.

Một ví dụ về siêu lập trình trong Rails là các route helper. Khi ứng dụng Rails khởi động lần đầu, nó nạp file `config/routes.rb`, trong đó có thể có dòng `get "home" => "static_pages#home"` để người dùng gõ `http://www.yoursite.com/home` và quay về trang chủ. Sau đó Rails tạo cho bạn một vài method, trong đó có helper `home_path` và `home_url`. Đó là một phần của siêu lập trình!

Tuy nhiên, ví dụ routes này chưa thật sự công bằng, vì bạn đã viết file `routes.rb` và có lẽ đã hard-code nhiều lời gọi method `#home_path` hoặc `#home_url` dựa trên những gì bạn biết sẽ có ở đó. Còn những tình huống động hơn, khi bạn không biết trước method sẽ được gọi là gì thì sao?

Ruby cung cấp method `#send` để giải quyết vấn đề. Nếu muốn chạy một method trên một object, chỉ cần *gửi* cho object đó method cùng mọi argument bạn muốn. Một ví dụ đơn giản có thể thực hiện trên command line là `1+2`:

```bash
  > 1 + 2
  => 3
  > 1.send(:+, 2)
  => 3
```

Trong tình huống thông thường, không có lý do gì để dùng method `#send`, nhưng nếu không biết mình sẽ cần gọi method nào thì nó thật sự rất hữu ích. Chỉ cần truyền cho nó tên method dưới dạng symbol mà bạn muốn chạy trên object đó, Ruby sẽ tự tìm method ấy.

Nhưng làm thế nào để định nghĩa một method mới ngay trong lúc chạy? Trong trường hợp này, bạn có thể dùng method `#define_method`, nhận symbol của method muốn định nghĩa và một block đại diện cho chính method đó. Các ví dụ sau được lấy từ [hướng dẫn siêu lập trình này của ruby-metaprogramming.rubylearning.com](https://web.archive.org/web/20200801134147/http://ruby-metaprogramming.rubylearning.com/html/ruby_metaprogramming_2.html):

```ruby
  class Rubyist

    define_method :hello do |my_arg|
      my_arg
    end
  end

  obj = Rubyist.new
  puts(obj.hello('Matz')) # => Matz
```

Một công cụ rất mạnh khác là method `#method_missing`. Chắc chắn bạn từng thấy lỗi đại loại như "Này, bạn vừa thử gọi một method không tồn tại!" và stack trace có thể chạy qua thứ gì đó tên là `method_missing`. Rất có thể bạn đã gõ sai tên method.

Về cơ bản, `#method_missing` là một method của class `BasicObject` trong Ruby, được mọi object trong Ruby kế thừa, và nó được gọi bất cứ khi nào bạn cố chạy một method thực sự không tồn tại. Nó cũng nhận tất cả argument bạn đã cố truyền cùng mọi block đi kèm. Điều đó có nghĩa là bạn có thể tự override `#method_missing` cho một object cụ thể và dùng thông tin vừa được gọi, chẳng hạn in ra tên method bạn định gọi cùng các argument của nó:

```ruby
  class Rubyist

    def method_missing(m, *args, &block)
      str = "Called #{m} with #{args.inspect}"

      if block_given?
        puts str + " and also a block: #{block}"
      else
        puts str
      end
    end
  end
```

```ruby
  > Rubyist.new.anything
  "Called anything with []"
  => nil

  > Rubyist.new.anything(3, 4) { "something" }
  "Called anything with [3, 4] and also a block: #<Proc:0x007fa0261d2ae0@(irb):38>"
  => nil
```

Siêu lập trình thật sự rất thú vị và có vô số cách sử dụng. Bạn không cần thành thạo nó để học Rails, nên chỉ nên đào sâu sau khi đã thoải mái với Rails, nhưng chắc chắn nó sẽ hữu ích trong thực tế. Có đủ loại mẹo, mẫu và thủ thuật siêu lập trình, nhưng việc tìm hiểu chúng nằm ngoài phạm vi của khóa học này.

Đây là một ví dụ hay về [dùng siêu lập trình để DRY hóa code](http://rails-bestpractices.com/posts/2010/07/24/dry-metaprogramming/).

Nếu thật sự tò mò, hãy xem [Metaprogramming Ruby](http://www.amazon.com/Metaprogramming-Ruby-Program-Like-Pros/dp/1934356476) của Paolo Perrotta.

### Mẫu thiết kế

Mẫu thiết kế có danh tiếng khá trái chiều trong giới phát triển phần mềm. Một mặt, chúng đại diện cho "best practices" về cách viết code để vượt qua một tình huống nhất định (không phải code cụ thể, chỉ là một khuôn mẫu để giải quyết vấn đề). Mặt khác, chúng đôi khi áp đặt một cách không cần thiết. Hãy xem [bài viết Wikipedia về Design Patterns](http://en.wikipedia.org/wiki/Software_design_pattern) để có cái nhìn tổng quan. Khóa học này sẽ không đi vào từng mẫu cụ thể.

[Bài viết Wikipedia về SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) cung cấp phần tổng quan tốt cùng các liên kết hữu ích về thiết kế phần mềm SOLID. Nếu muốn viết code tốt, bạn cần biết từng nguyên tắc mà các chữ cái đại diện (diễn giải):

- [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) (Một class chỉ nên có một trách nhiệm duy nhất)
- [Open/Closed Principle](http://en.wikipedia.org/wiki/Open/closed_principle) (các thực thể trong code nên mở cho việc mở rộng nhưng đóng với việc sửa đổi)
- [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle) (thay một object bằng một subtype của nó không nên làm hỏng bất cứ điều gì)
- [Interface Segregation Principle](http://en.wikipedia.org/wiki/Interface_segregation_principle) (viết nhiều interface dành riêng cho client tốt hơn một interface khổng lồ dùng chung... hãy nghĩ đến API)
- [Dependency Inversion Principle](http://en.wikipedia.org/wiki/Dependency_inversion_principle) (thay vì cấu trúc cấp cao phụ thuộc vào cấu trúc cấp thấp, hãy để chúng phụ thuộc vào abstraction)

May mắn là Rails đã làm khá tốt việc tuân theo các nguyên tắc này, nên chỉ qua quá trình sử dụng Rails bạn có lẽ đã hình thành được một số thói quen tốt. Tuy vậy, hãy dành chút thời gian đọc về từng nguyên tắc (kể cả những nguyên tắc có tên nghe lạ) vì chúng khá cốt lõi đối với toàn bộ ngành kỹ nghệ phần mềm (và là câu hỏi phỏng vấn rất hay).

Có một cuốn sách hữu ích về anti-pattern, giúp bạn dọn dẹp code bằng cách nhận diện các dấu hiệu code có vấn đề, mang tên [Rails Antipatterns](http://www.amazon.com/Rails-AntiPatterns-Refactoring-Addison-Wesley-Professional/dp/0321604814/) của Tammer Saleh và Chad Pytel.

### I18n: Quốc tế hóa

[Quốc tế hóa và bản địa hóa](http://en.wikipedia.org/wiki/Internationalization_and_localization) là quá trình điều chỉnh ứng dụng để phù hợp với các khu vực địa lý và/hoặc ngôn ngữ cụ thể. Phần này nằm ngoài phạm vi của chúng ta, nhưng nếu quan tâm, hãy xem [bài hướng dẫn quốc tế hóa trên SitePoint](http://www.sitepoint.com/go-global-rails-i18n/), theo gợi ý của K. Bates.

### Kết luận

Trong bài học này, chúng ta đã đề cập đến một số khái niệm khá ngẫu nhiên và phức tạp nhưng hữu ích, ít nhất là để làm quen, ngay cả khi bạn không dùng chúng hằng ngày. Kinh nghiệm mới là chìa khóa thật sự ở đây -- trong quá trình xây dựng những sản phẩm tuyệt vời, bạn sẽ gặp nhu cầu sử dụng tất cả những thứ vừa học và chúng có thể giúp bạn tiết kiệm rất nhiều thời gian cũng như độ phức tạp trong code.

Những nguyên tắc tổng quát hơn như thiết kế SOLID và siêu lập trình sẽ hữu ích bất kể bạn tiếp tục gắn bó với Ruby và Rails hay chuyển sang những công nghệ tốt hơn, sáng sủa hơn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc lướt phần 2.6 về namespacing trong [Rails Guide về Routing](http://guides.rubyonrails.org/routing.html#controller-namespaces-and-routing).
  1. Đọc các phần 2.7-3.7 của cùng tài liệu để tìm hiểu về route lồng nhau, route thành viên, route tập hợp và nhiều nội dung khác.
  1. Đọc các phần 3.8-3.15 của cùng tài liệu để tìm hiểu nhiều chủ đề định tuyến nâng cao, bao gồm giới hạn input cho route và chuyển hướng.
  1. Đọc lướt chương 4 của cùng tài liệu. Một số nội dung chúng ta đã thấy, nhưng phần lớn nhằm giúp bạn hình dung những gì có thể làm được. Khi cần, có lẽ bạn sẽ lại Google để tìm đến đó.
  1. Đọc phần 3.5 về [Rails Guide về Layout](http://guides.rubyonrails.org/layouts_and_rendering.html#using-nested-layouts) để xem cách truyền thông tin giữa view file và layout file, bao gồm cả CSS style. Hãy thật sự dành thời gian để hiểu điều gì đang xảy ra trong ví dụ đó.
  1. Nếu quan tâm, hãy xem qua [Ruby metaprogramming](https://web.archive.org/web/20210514184321/http://ruby-metaprogramming.rubylearning.com/). Đây không phải kiến thức thiết yếu để xây dựng các ứng dụng Rails ban đầu, nhưng bạn chắc chắn sẽ gặp nó thường xuyên hơn trong "thực tế".
  1. Đọc lướt [bài thuyết trình Slideshare về các nguyên tắc SOLID](http://www.slideshare.net/jcfischer/solid-ruby-solid-rails) hoặc [tải trực tiếp bản PDF](https://github.com/MichaelMahlberg/RailsWayCon2010/blob/master/railswaycon10_SOLID_ruby.pdf) nếu thích.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- Một câu hỏi trên Stack Overflow về [tìm hiểu resource số ít trong Rails](http://stackoverflow.com/questions/6629142/having-problem-understanding-singular-resource-in-rails)
- [In Relentless Pursuit of REST của Derek Prior](https://www.youtube.com/watch?v=HctYHe-YjnE)
- [Video của Yehuda Katz về Rails Security](http://youtu.be/2Ex8EEv-WPs)
- Lời giải đầu tiên trong câu hỏi Stack Overflow này trình bày một cách hay để [làm việc với nhiều layout, dùng class để kích hoạt các style CSS khác nhau](http://stackoverflow.com/questions/4208380/confused-on-advanced-rails-layout-nesting).
- [Ruby Metaprogramming](https://web.archive.org/web/20200801134147/http://ruby-metaprogramming.rubylearning.com/html/ruby_metaprogramming_2.html)
- [Bài viết trên Stack Overflow về mẫu thiết kế trong Rails (2010)](http://stackoverflow.com/questions/2522065/design-patterns-in-rails)
- [Phần giải thích dài hơn về các nguyên tắc SOLID](https://www.youtube.com/watch?v=8STtzjyDTTQ)