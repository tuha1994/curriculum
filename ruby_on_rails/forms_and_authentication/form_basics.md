### Giới thiệu

Bạn hẳn đã quen với biểu mẫu, cả với tư cách người dùng Internet thông thường lẫn người viết HTML từng học [khóa Intermediate HTML and CSS](https://www.theodinproject.com/paths/full-stack-ruby-on-rails/courses/intermediate-html-and-css). Nhưng bạn THỰC SỰ hiểu về biểu mẫu đến đâu? Nghe có vẻ lạ, nhưng biểu mẫu có lẽ là phần phức tạp nhất khi học phát triển web. Không nhất thiết vì bản thân mã khó, mà vì bạn thường muốn xây dựng các biểu mẫu có thể làm rất nhiều việc khác nhau cùng lúc.

Cho đến nay, chúng ta vẫn thường suy nghĩ về các Model trong Rails theo kiểu tách riêng từng cái một. Model User. Model Post. Đôi khi các model liên hệ với nhau qua association, chẳng hạn một Post có thể `has_many` đối tượng Comment. Tuy nhiên, phần lớn thời gian chúng ta có xu hướng chỉ tập trung xử lý từng model riêng lẻ.

Bây giờ hãy nghĩ đến một biểu mẫu web để mua vé máy bay. Có lẽ bạn cần nhập tên, địa chỉ, số điện thoại, email, hãng hàng không, số hiệu chuyến bay, ngày bay, số thẻ tín dụng, mã bảo mật thẻ tín dụng, ngày hết hạn thẻ, mã bưu chính của thẻ, cùng một loạt checkbox cho những lựa chọn bổ sung như bảo hiểm chuyến đi. Có quá nhiều model khác nhau được nhúng trong một biểu mẫu! Vậy mà bạn vẫn gửi tất cả chỉ bằng một nút. Trời đất ơi!

Hầu hết biểu mẫu của bạn sẽ không dài hay phức tạp đến vậy, nhưng việc hình dung tất cả những gì bạn có thể (và một ngày nào đó sẽ) làm với chúng vẫn rất hữu ích. Tạo một biểu mẫu cơ bản cực kỳ dễ, nên việc đầu tiên chúng ta sẽ làm là bảo đảm bạn hiểu thật sâu cách tạo biểu mẫu bằng HTML, rồi cách Rails cung cấp các helper để giúp công việc của bạn dễ dàng hơn. Chúng ta sẽ tìm hiểu cách dữ liệu được cấu trúc và gửi đến controller cho đến khi bạn cảm thấy khá thoải mái với việc đó. Sau đó, một bài học khác sẽ chỉ cho bạn cách tận dụng hiểu biết cơ bản này để làm cho biểu mẫu mạnh mẽ hơn.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài này.

- Tạo biểu mẫu trong Rails.
- Cách Rails bảo vệ trước các cuộc tấn công CSRF.
- Cách xử lý các biểu mẫu không vượt qua validation.
- Liên kết biểu mẫu và model bằng helper `form_with`.
- Cách truy cập và sử dụng các giá trị được gửi trong biểu mẫu.

### Biểu mẫu trong HTML

Bước đầu tiên là có thể tạo một biểu mẫu bằng HTML. Bạn còn nhớ nó trông như thế nào không?

```html
<form action="/somepath" method="post">
  <input type="text">
  <!-- other inputs here -->
  <input type="submit" value="Submit This Form">
</form>
```

Có rất nhiều thẻ `input` để bạn lựa chọn, bao gồm `button`, `checkbox`, `date`, `hidden`, `password`, `radio` và nhiều loại khác (xem [danh sách đầy đủ các loại input HTML trên W3Schools](https://www.w3schools.com/html/html_form_input_types.asp)).

### Xem những gì biểu mẫu gửi đi

Nếu muốn xem biểu mẫu đang gửi gì đến ứng dụng Rails, hãy xem phần output được in ra console khi bạn chạy `$ rails server`. Mỗi khi gửi một biểu mẫu rất cơ bản để đăng ký email người dùng, output sẽ có những dòng tương tự như sau:

```bash
  Started POST "/user" for 127.0.0.1 at 2013-11-21 19:10:47 -0800
  Processing by UsersController#create as TURBO_STREAM
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=", "email"=>"foo@bar.com", "commit"=>"Submit Form"}
```

**Lưu ý**: đây là output từ một biểu mẫu có thể được tạo bởi một phương thức helper của Rails, như sẽ giải thích ở phần sau.

Dòng đầu tiên cho biết phương thức HTTP nào đã được dùng và biểu mẫu đã đi đến route nào. Dòng thứ hai cho biết controller và action nào sẽ xử lý biểu mẫu; hãy chú ý action TURBO_STREAM vì đây là hành vi mặc định. Dòng thứ ba chứa mọi thứ sẽ được đưa vào hash `params` để controller sử dụng. Chúng ta sẽ nói về nội dung của hash này trong các phần tiếp theo.

Khi bắt đầu xây dựng biểu mẫu, bạn sẽ thường xuyên xem output này của server. Nó giúp bạn giữ được bình tĩnh vì cho biết chính xác trình duyệt đã gửi gì về ứng dụng, nhờ đó bạn có thể nhận ra liệu đã có một... hiểu lầm nào xảy ra hay chưa.

### Rails hóa biểu mẫu bằng cách đưa dữ liệu vào params

Mỗi input trong số này có cấu trúc hơi khác nhau, nhưng chúng có một số điểm chung. Một điều quan trọng cần lưu ý là thuộc tính `name` bạn có thể gán cho một thẻ input. Trong Rails, thuộc tính này rất quan trọng. Thuộc tính `name` cho Rails biết phải gọi dữ liệu bạn nhập vào trường đó là gì khi tạo hash `params`. Ví dụ:

```html
...
<input type="text" name="description">
...
```

sẽ khiến hash `params` chứa một key tên là `description`, bạn có thể truy cập như bình thường, chẳng hạn `params[:description]`, bên trong controller. Đó cũng là lý do một số input như radio button (trong đó `type="radio"`) dùng thuộc tính `name` để biết radio button nào cần được nhóm với nhau, sao cho khi chọn một nút thì các nút còn lại sẽ bỏ chọn. Thuộc tính `name` quan trọng một cách đáng ngạc nhiên!

Một điều khác chúng ta đã nói trong phần controller là việc lồng dữ liệu. Bạn thường sẽ muốn xếp gọn dữ liệu đã gửi vào một hash thay vì để tất cả ở cấp cao nhất. Điều này hữu ích vì như đã thấy với controller, nó cho phép bạn viết `#create` trên một dòng (sau khi đã cho phép các parameter bằng `#expect`). Khi truy cập `params[:user]`, thực ra bạn nhận được một hash chứa toàn bộ thuộc tính của người dùng, chẳng hạn `{first_name: "foo", last_name: "bar", email: "foo@bar.com"}`. Làm thế nào để biểu mẫu gửi các parameter như vậy? Dễ thôi!

Mọi thứ lại quay về thuộc tính `name` của các input trong biểu mẫu. Chỉ cần dùng dấu ngoặc vuông để lồng dữ liệu như sau:

```html
...
<input type="text" name="user[first_name]">
<input type="text" name="user[last_name]">
<input type="text" name="user[email]">
...
```

Các input này giờ sẽ được chuyển thành một hash lồng nhau dưới key `:user`. Output của server sẽ trở thành:

```bash
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=", "user"=>{"first_name"=>"foo","last_name"=>"bar","email"=>"foo@bar.com"}, "commit"=>"Submit Form"}
```

Các parameter cụ thể trong hash `params` được truy cập giống như mọi hash lồng nhau khác: `params[:user][:email]`.

Đừng quên rằng giờ bạn phải cho phép các params trong controller bằng `#expect`, vì chúng là một hash chứ không chỉ là một chuỗi phẳng. Hãy xem lại phần Controller bên dưới để nhớ lại cách xử lý ở phía controller.

Đây là phần khá thú vị mà bạn sẽ có dịp thực hành trong project.

#### Authenticity token

Có thể bạn đã nhận thấy trong các ví dụ trên rằng Rails luôn gửi đến backend một parameter tên là `"authenticity_token"`, với giá trị là một chuỗi số và chữ có vẻ ngẫu nhiên. Token này là một chuỗi ngẫu nhiên, duy nhất mà Rails đưa vào biểu mẫu để bảo vệ trước các cuộc tấn công Cross-Site Request Forgery (CSRF). Về cơ bản, đây là cách Rails bảo đảm mọi input biểu mẫu nó nhận được đều đến từ *trang web của bạn*. Nếu không có cơ chế này, người dùng xấu có thể tạo các liên kết hoặc nút ở nơi họ kiểm soát (chẳng hạn website riêng hoặc email của họ), rồi lừa người dùng trang web của bạn nhấp vào đó để thực hiện hành động thay mặt họ.

Nếu Rails xử lý dữ liệu biểu mẫu mà parameter không có `authenticity_token`, hoặc giá trị của nó không khớp với điều Rails mong đợi, Rails sẽ phát sinh lỗi thay vì tiếp tục xử lý yêu cầu. Điều này giữ cho trang web của bạn an toàn trước các kiểu tấn công như vậy.

### Form helper: form_with

Rails cố gắng làm cho cuộc sống của bạn dễ dàng nhất có thể, nên đương nhiên nó cung cấp các phương thức helper tự động hóa một số phần lặp đi lặp lại khi tạo biểu mẫu. Điều đó không có nghĩa bạn không cần biết cách tạo biểu mẫu theo cách "cổ điển"... thực tế, khi dùng helper thì việc nắm vững nền tảng biểu mẫu còn QUAN TRỌNG hơn, vì bạn cần thật sự hiểu chuyện gì diễn ra phía sau nếu có gì đó hỏng.

Hãy bắt đầu bằng cách tạo một biểu mẫu dùng helper `form_with`, helper này nhận một block đại diện cho toàn bộ input của biểu mẫu. Nó tự xử lý token bảo mật CSRF đã nói ở trên bằng cách tự động tạo input ẩn cho token, nên bạn không phải làm việc đó. Bạn truyền các argument để cho biết biểu mẫu cần gửi đến path nào (mặc định là trang hiện tại) và dùng method nào. Sau đó có các tag helper tạo những thẻ tương ứng cho bạn, chẳng hạn `text_field_tag` bên dưới. Ở đó, tất cả những gì bạn cần chỉ là chỉ định tên của field khi nó được gửi đi.

```erb
  <%= form_with(url: "/search", method: "get") do %>
    <%= label_tag(:query, "Search for:") %>
    <%= text_field_tag(:query) %>
    <%= submit_tag("Search") %>
  <% end %>
```

tạo ra biểu mẫu:

```html
<form accept-charset="UTF-8" action="/search" method="get">
  <label for="query">Search for:</label>
  <input id="query" name="query" type="text">
  <input
    name="commit"
    type="submit"
    value="Search"
    data-disable-with="Search"
  >
</form>
```

Có tag helper cho tất cả các thẻ chính, và các tùy chọn chúng nhận vào cũng hơi khác nhau. Hãy xem bài đọc được giao để biết thêm chi tiết.

Có một vài điều cần lưu ý khi dùng helper `form_with`.

1. ID của các input trùng với name.
1. Dòng thứ hai kết thúc bằng `as TURBO_STREAM` khi bạn xem output trong console sau khi gửi biểu mẫu. Theo mặc định, tất cả biểu mẫu hiện được gửi bởi Turbo Drive. Điều này có nghĩa một chu kỳ request đầy đủ không diễn ra và trang không tải lại khi biểu mẫu được gửi. Để tắt hành vi này, bạn cần thêm thuộc tính dữ liệu tương ứng đã học trong bài Turbo Drive.

```erb
  <%= form_with(url: "/search", method: "get", data: { turbo: false} ) do %>
    <%= label_tag(:query, "Search for:") %>
    <%= text_field_tag(:query) %>
    <%= submit_tag("Search") %>
  <% end %>
```

### Dùng model với helper form_with

Trong phần lớn trường hợp, bạn sẽ muốn biểu mẫu tác động lên các thuộc tính của một model có sẵn. Chẳng hạn chỉ định title (hoặc bất kỳ field nào khác mà model yêu cầu) cho một Article mới.

Chỉ cần truyền cho `form_with` một đối tượng model, nó sẽ khiến biểu mẫu gửi đến URL của đối tượng đó; ví dụ, `@article` sẽ gửi đến đúng URL để tạo một Article. Hãy nhớ từ bài học về controller rằng action `#new` thường tạo một instance mới (chưa lưu) của đối tượng rồi truyền nó cho view... giờ cuối cùng bạn cũng thấy lý do cần dùng đối tượng đó trong các biểu mẫu `#form_with`!

Theo Rails Guide:

```ruby
  # app/controllers/articles_controller.rb
  def new
    @article = Article.new
  end
```

```erb
  # app/views/articles/new.html.erb
  <%= form_with model: @article do |form| %>
    <%= form.text_field :title %>
    <%= form.submit "Create" %>
  <% end %>
```

Đoạn này sẽ tạo ra HTML sau:

```html
<form action="/articles" method="post">
  <input
    name="authenticity_token"
    type="hidden"
    value="J7CBxfHalt49OSHp27hblqK20c9PgwJ108nDHX/8Cts="
  >
  <input type="text" name="article[title]">
  <input type="submit" value="Create">
</form>
```

Điểm hay nhất của `form_with` là nếu chỉ truyền một đối tượng model như `@article` trong ví dụ trên, Rails sẽ tự kiểm tra xem đối tượng đó đã được lưu chưa. Nếu là đối tượng mới, nó sẽ gửi biểu mẫu đến action `#create`. Nếu đối tượng đã từng được lưu, tức là chúng ta biết mình đang chỉnh sửa một đối tượng hiện có, nó sẽ gửi đối tượng đến action `#update` thay vào đó. Rails làm việc này bằng cách tự động tạo URL đúng khi biểu mẫu được tạo. Thật kỳ diệu!

### Biểu mẫu và validation

Điều gì xảy ra nếu biểu mẫu được gửi nhưng không vượt qua các validation bạn đặt ra? Ví dụ, nếu mật khẩu của người dùng quá ngắn thì sao? Trước hết, bạn đáng lẽ phải có các validation bằng JavaScript làm tuyến phòng thủ đầu tiên, và chúng phải phát hiện được vấn đề đó... nhưng chúng ta sẽ nói về việc này trong một khóa học khác. Dù sao đi nữa, hy vọng controller của bạn đã được thiết lập để render lại biểu mẫu hiện tại.

Có lẽ bạn sẽ muốn hiển thị lỗi để người dùng biết chuyện gì đã xảy ra. Hãy nhớ rằng khi Rails cố gắng validate một đối tượng nhưng thất bại, nó gắn thêm một tập field mới có tên là `errors` vào đối tượng. Bạn có thể xem các lỗi đó bằng cách truy cập `your_object_name.errors`. Những lỗi này có một vài helper tiện dụng để hiển thị đẹp mắt trên trình duyệt -- `#count` và `#full_messages`. Xem đoạn code dưới đây:

```erb
  <% if @post.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>

      <ul>
      <% @post.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
      </ul>
    </div>
  <% end %>
```

Đoạn này sẽ cho người dùng một thông báo cho biết có bao nhiêu lỗi, sau đó là một thông báo cho từng lỗi.

Điểm hay nhất về form helper của Rails... chúng cũng tự động xử lý lỗi! Nếu một biểu mẫu được render cho một đối tượng model cụ thể, chẳng hạn dùng `form_with model: @article` như ví dụ trên, Rails sẽ kiểm tra lỗi và nếu tìm thấy lỗi, nó sẽ tự động bọc một phần tử `<div>` đặc biệt quanh field đó, với class `field_with_errors`, để bạn có thể viết CSS tùy ý nhằm làm field nổi bật. Hay quá!

### Gửi yêu cầu PATCH và DELETE

Biểu mẫu vốn không được thiết kế để xóa đối tượng một cách nguyên bản vì trình duyệt chỉ hỗ trợ yêu cầu GET và POST. Rails cung cấp một cách khắc phục bằng cách chèn một field ẩn có tên "\_method" vào biểu mẫu. Field này cho Rails biết rằng thực ra bạn muốn thực hiện yêu cầu PATCH (còn gọi là PUT) hoặc DELETE (tùy phương thức bạn chỉ định), và có thể trông như `<input name="_method" type="hidden" value="patch">`.

Bạn yêu cầu Rails thêm field này vào biểu mẫu bằng cách truyền cho `form_with` một tùy chọn tên là `:method`, ví dụ:

```ruby
  form_with(url: search_path, method: "patch")
```

### Ôn lại phía controller

Để ôn lại, dưới đây là một thiết lập controller rất cơ bản để xử lý các action `#new` và `#create`.

```ruby
  # app/controllers/users_controller.rb
  ...
  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to @user
    else
      render :new, status: :unprocessable_entity
    end
  end

  def user_params
    params.expect(user: [:first_name, :last_name, :other_stuff])
  end
  ...
```

Tôi muốn đưa đoạn này ra một lần nữa để bạn nhớ lại điều gì đang diễn ra trong vòng đời của biểu mẫu. Có lẽ người dùng đã đi đến path của action `#new`, thường là `http://www.yourapp.com/users/new`. Thao tác đó chạy action `#new`, tạo một đối tượng user mới trong bộ nhớ (nhưng chưa lưu vào database), rồi render view `new.html.erb`. View này có lẽ dùng `form_with model: @user` để mọi việc trở nên dễ dàng hơn cho developer.

Khi biểu mẫu được gửi, action `#create` sẽ xây dựng một đối tượng User mới khác bằng các parameter mà chúng ta chỉ rõ là hợp lệ. Hãy nhớ rằng phương thức tùy chỉnh `#user_params` sẽ trả về hash `params[:user]`, nhờ đó `User.new` có thể xây dựng một instance mới hoàn chỉnh. Nếu instance đó được lưu vào database, mọi việc đều ổn và chúng ta đi đến trang `show.html.erb` của user đó.

Nếu không thể lưu `@user`, chẳng hạn vì `first_name` chứa chữ số, chúng ta sẽ quay thẳng lại việc render view `new.html.erb`, lần này dùng instance `@user` vẫn còn chứa các lỗi. Biểu mẫu của chúng ta nên xử lý các lỗi đó một cách phù hợp bằng cách cho người dùng biết họ đã nhập sai ở đâu.

Hãy chú ý cách chúng ta chỉ định một status trong mệnh đề else:

```ruby
else
  render :new, status: :unprocessable_entity
```

Bạn còn nhớ bài Turbo Drive không? Turbo xử lý những mã status nào? Một redirect, status 4XX hoặc status 5XX. Khi lưu thành công đối tượng đã tạo, bạn có thể thấy chúng ta thực hiện redirect:

```ruby
if @user.save
  redirect_to @user
```

Đây là status redirect 303 mà chúng ta đã nhắc đến trong bài Turbo Drive trước đó. Nếu không redirect, và cũng không muốn lỗi 5XX vì điều đó có nghĩa ứng dụng gặp sự cố ngoài dự kiến, thì chúng ta chỉ còn status 4XX.

Nếu không đưa `status: :unprocessable_entity` vào thì điều gì sẽ được trả về?

Đó sẽ là status 200 OK vì đây là phản hồi được mong đợi đối với một lần gửi biểu mẫu thất bại. Yêu cầu đã hoàn tất theo đúng cách chúng ta dự kiến. Vì vậy, khi xử lý validation biểu mẫu thất bại, bạn cần chỉ định status HTTP 422. Làm vậy sẽ thay đổi status trả về từ `Completed 200 OK in 206ms (Views: 125.5ms | ActiveRecord: 8.1ms | Allocations: 18793)` thành `Completed 422 Unprocessable Entity in 52ms (Views: 44.6ms | ActiveRecord: 0.0ms | Allocations: 6391)`, và như đã biết, Turbo sẽ xử lý phản hồi này rồi cập nhật HTML bằng response được trả về.

Response là gì? Trang mới lại được render dưới dạng HTML. Nhưng lần này, vì đối tượng `@user` có một số lỗi, HTML sẽ bao gồm mọi thông tin bạn đã render cho một đối tượng đang có lỗi.

Nếu bạn gửi biểu mẫu mà không có gì xảy ra, rất có thể bạn đã quên trả về status đúng. Bạn có thể xác nhận điều này trong server log khi gửi biểu mẫu.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc [Rails Guide về Form Helpers](http://guides.rubyonrails.org/form_helpers.html), các mục từ 1 đến 2.4.
  1. Đọc lướt các mục từ 3.1 đến 7 để xem có những gì. Một ngày nào đó bạn sẽ cần đến chúng, và giờ bạn đã biết tìm ở đâu.
  1. Đọc các mục từ 8.1 đến 8.3 để xem giải thích chính thức về cách tạo parameter từ thuộc tính `name`.
  1. Đọc mục 9 trong [Rails Guide về Validations](http://guides.rubyonrails.org/active_record_validations.html#displaying-validation-errors-in-views) để xem nhanh cách hiển thị lỗi.
  1. Đọc lướt phần API Rails chính thức về [helper form_with](https://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html#method-i-form_with) để xem những cách khác nhau có thể dùng tag helper này.

</div>

#### Kết luận

Đến đây, bạn hẳn đã hiểu vững cách biểu mẫu hoạt động nói chung và khá nắm được cách Rails hỗ trợ bạn bằng cách tự tạo biểu mẫu. Trong vài project tiếp theo, bạn sẽ có cơ hội xây dựng rất nhiều biểu mẫu, nên đừng lo nếu mọi thứ vẫn chưa hoàn toàn in sâu trong đầu. Nhìn thấy chúng hoạt động thực tế sẽ giúp bạn hiểu rõ hơn.