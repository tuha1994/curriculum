### Giới thiệu

View thực sự là phần đơn giản nhất của cấu trúc MVC -- ở mức cơ bản, nó chỉ là một loạt mã HTML khung, nơi bạn chèn các biến nhận được từ controller rồi gửi tới trình duyệt. Nó chính là "trang web" thực tế của bạn. View thường chứa một vài đoạn code được thiết kế để trình bày đúng các biến đã nhận, chẳng hạn một vòng lặp hiển thị từng bài viết trên blog. View thường được gọi là các view template.

View nằm trong thư mục `app/views/controller_name/action_name.html.erb`, trong đó `controller_name` là tên controller mà view liên kết tới, còn `action_name.html.erb` là method tương ứng bên trong controller vừa được chạy ngay trước khi render view.

Vì vậy, khi controller `Posts` chạy action `#index`, nó sẽ ngầm render view `app/views/posts/index.html.erb` khi hoàn tất. Bạn có thể yêu cầu rõ ràng controller render một view có tên khác bằng cách truyền tên đó làm parameter cho hàm `render` trong controller, nhưng tại sao phải làm vậy? Cấu trúc thư mục và cách đặt tên này giúp bạn (và Rails) luôn biết cần tìm một view ở đâu.

Để dùng một biến instance trong view, chỉ cần gọi nó giống hệt như trong controller: `@user.first_name`, `@posts` hoặc `@some_other_variable`.

Như thường lệ, trong bài này chúng ta sẽ học các nội dung ở mức khái quát trước, sau đó bạn sẽ đọc Rails Guide để hiểu chi tiết hơn cách mọi thứ hoạt động.

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quát về những chủ đề bạn sẽ học trong bài.

- Cách tổ chức view template và layout trong Rails.
- Áp dụng ERB trong các file view.
- Partial và cách tạo, gọi chúng.
- Tạo anchor tag động trong các file view.

### Layout

Điều đầu tiên cần lưu ý là view template có tên mà chúng ta render từ controller thực ra không phải toàn bộ trang web. Nó không chứa các tag `<head>`, khai báo `DOCTYPE` hay một số cấu trúc cơ bản khác xuất hiện trên mọi trang. Chính vì những phần đó có mặt trên tất cả các trang, các nhà sáng tạo Rails đủ thông minh để tách chúng thành một file riêng gọi là "layout". Layout nằm trong thư mục `app/views/layouts`.

Với một ứng dụng Rails hoàn toàn mới, layout `application.html.erb` khá cơ bản. Nó có những tag nền tảng cần có trong mọi trang web (ví dụ `<html>` và `<body>`) cùng một vài đoạn code tải các file JavaScript và CSS mà trang web cần. Bạn nên đặt mọi thứ cần dùng chung cho tất cả trang web vào layout. Thông thường đó là navbar, footer và các đoạn code hiển thị flash message.

Nếu layout về cơ bản chỉ là một lớp vỏ bao quanh từng trang riêng lẻ, vậy trang được chèn vào như thế nào? Điều đó đưa chúng ta trở lại với sự kỳ diệu của method `#yield`, thứ bạn đã thấy khi học về block. View template tại `app/views/posts/index.html.erb` sẽ được chèn vào vị trí có câu lệnh yield. Khi nâng cao hơn, bạn sẽ có thể thử nghiệm thêm với câu lệnh đó.

### Bộ tiền xử lý

Một điều khác mà chắc chắn bạn đã nhận thấy là đoạn mã HTML kỳ lạ nằm bên trong các tag `<%=` và `%>`. Đây là Embedded Ruby (ERB), một cách đặc biệt để thực thi code ruby bên trong HTML. HTML là tĩnh, nên nếu muốn làm những việc động như lặp, dùng câu lệnh `if` hoặc làm việc với biến, bạn cần đưa Ruby vào. ERB (và một ngôn ngữ tương tự khác mà bạn có thể gặp là HAML) làm đúng việc đó.

Các tag này thực thi mọi thứ bên trong chúng chính xác như khi đó là Ruby thông thường. Vì vậy, `<em><%= "I am emphasized" %></em>` sẽ xuất ra một đoạn văn bản được nhấn mạnh như *Tôi được nhấn mạnh* còn `<%= @user.first_name %>` có thể xuất ra `joe`.

Điểm khác nhau giữa `<%` và `<%=` là phiên bản `<%=` thực sự hiển thị bất cứ thứ gì được trả về bên trong các tag ERB. Nếu dùng `<%`, code vẫn được thực thi nhưng dù dòng đó trả về gì, nó cũng không thực sự hiển thị trong HTML template. `<%#` được dùng để comment và sẽ không được thực thi.

Phần lớn tag của bạn sẽ là `<%=` vì bạn thường chỉ xuất ra những phần quan trọng trong các biến instance nhận được từ controller, như ví dụ `<%= @user.first_name %>` ở trên. Bạn dùng `<%` cho những việc thuần túy liên quan tới code như câu lệnh `if` và vòng lặp `each`, khi bạn thực sự KHÔNG muốn hiển thị gì (phần được hiển thị sẽ nằm bên trong vòng lặp).

Nếu thấy khó hiểu, hãy xem ví dụ này: giả sử chúng ta muốn hiển thị tên của tất cả user trong ứng dụng, nhưng chỉ khi user hiện tại đã đăng nhập. Đoạn code có thể trông như sau:

```erb
  <% if current_user.signed_in? %>
    <ul>
      <% @users.each do |user| %>
        <li><%= user.first_name %></li>
      <% end %>
    </ul>
  <% else %>
    <strong>You must sign in!</strong>
  <% end %>
```

Hãy nhớ đóng các câu lệnh và vòng lặp bằng `<% end %>`! (Bạn sẽ quên vài lần đấy.)

Trong đoạn code trên, nếu user đã đăng nhập, kết quả render trên web thực tế sẽ gần giống như sau:

```html
  <ul>
    <li>Bob</li>
    <li>Joe</li>
    <li>Nancy</li>
  </ul>
```

Nếu user chưa đăng nhập, kết quả sẽ ngắn hơn nhiều:

```html
  <strong>You must sign in!</strong>
```

Trong code trên, nếu chúng ta vô tình dùng `<%=` ở dòng vòng lặp, chẳng hạn `<%= @users.each do |user| %>`, code vẫn chạy bình thường, nhưng vì `each` trả về collection ban đầu, chúng ta cũng sẽ thấy một bản dump của biến `@users` trên trang (trông không chuyên nghiệp chút nào). Bạn sẽ gặp việc này vài lần rồi nhanh chóng rút kinh nghiệm.

### Bộ tiền xử lý hoạt động như thế nào?

Điều quan trọng cần biết về việc thực thi code ở trên là tất cả đều diễn ra trên server TRƯỚC KHI file HTML cuối cùng được gửi tới trình duyệt (đây là một phần của Asset Pipeline, sẽ được học trong bài tiếp theo). Đó là vì khi render template trong Rails, trước tiên Rails chạy các "bộ tiền xử lý" như ERB. Rails biết bạn muốn tiền xử lý file vì file có phần mở rộng `.html.erb`.

Rails xử lý các phần mở rộng bổ sung từ ngoài vào trong. Vì vậy, trước tiên nó xử lý file bằng ERB, rồi xem file như HTML thông thường. Điều đó hợp lý vì ERB, theo định nghĩa, xuất ra HTML sạch và hợp lệ như chúng ta đã thấy ở trên.

Bạn cũng sẽ gặp những bộ tiền xử lý khác. File `.css.scss` dùng bộ tiền xử lý SASS và trở thành file CSS thông thường. File `.js.coffee`, dùng bộ tiền xử lý Coffeescript, trở thành JavaScript thông thường sau khi được chạy qua bộ xử lý. Trong cả hai trường hợp, ngôn ngữ tiền xử lý giúp cuộc sống dễ dàng hơn bằng cách cung cấp thêm các công cụ (như vòng lặp và khả năng làm việc với biến), rồi biên dịch ngược về CSS, JavaScript hoặc HTML thuần.

Điểm cốt lõi là có rất nhiều bộ tiền xử lý khác nhau. Chúng thường là các gem đã đi kèm Rails hoặc có thể dễ dàng gắn vào Rails. Rails tự động chạy chúng, nên tất cả những gì bạn cần quan tâm là file có đúng phần mở rộng để báo cho bộ tiền xử lý chạy hay không.

### View partial

Một việc hữu ích khác bạn có thể làm trong Rails là chia view thành các partial. Điều này giúp ích ở nhiều mặt -- code ngắn gọn, dễ đọc hơn, đồng thời cho phép bạn tái sử dụng những mẫu chung. Một ví dụ là form tạo hoặc chỉnh sửa user. Cả action `#new` lẫn `#edit` đều cần render một dạng form cho user, và form đó thường gần như giống hệt nhau. Vì vậy, người ta thường đưa form thành một file mới có tên như `_user_form.html.erb`, rồi gọi file đó từ cả hai view template `new.html.erb` và `edit.html.erb` ở nơi cần dùng.

Nói khái quát hơn, partial chỉ là các file HTML chưa được thiết kế để hoàn chỉnh độc lập nhưng có thể được các file khác dùng chung. Bạn gọi một partial bằng cách viết như sau:

```erb
  # app/views/users/new.html.erb
  <div class="new-user-form">
    <%= render "user_form" %>
  </div>
```

Có một vài điểm cú pháp hơi đặc biệt mà bạn cần chú ý. File view partial được đặt tên với dấu gạch dưới, như `_user_form.html.erb`, nhưng được gọi chỉ bằng phần cốt lõi của tên, ví dụ `user_form` trong đoạn trên.

Nếu tên partial không chỉ rõ thư mục, Rails chỉ tìm trong cùng thư mục với view đã gọi nó, chẳng hạn `app/views/users`. Đôi khi việc dùng chung partial giữa nhiều view template thuộc nhiều controller là hợp lý, nên bạn lưu chúng vào thư mục riêng `app/views/shared`, rồi render bằng code `<%= render "shared/some_partial"%>`.

### Truyền biến local vào partial

Có rất nhiều việc bạn có thể làm với partial và chúng ta sẽ không đi hết tất cả ở đây, nhưng một việc có thể bạn sẽ thường xuyên làm là truyền biến vào partial. Partial có quyền truy cập tất cả biến mà view template gọi nó có, nhưng ĐỪNG phụ thuộc vào điều đó! Nếu partial được một controller khác sử dụng, trong khi controller đó có cấu trúc biến instance khác thì sao? Việc mặc định một biến instance như `@user` luôn tồn tại trong partial là code tệ. Điều đó có nghĩa bạn phải truyền rõ ràng cho partial mọi biến mà bạn muốn nó có quyền truy cập.

Trong ví dụ trên, rất có thể bạn muốn truyền biến `@user` vào partial để code có thể render đúng loại form. `render` chỉ là một method thông thường và cho phép bạn truyền một [options hash](https://stackoverflow.com/questions/18407618/what-are-options-hashes). Một trong các option đó là key `:locals`, chứa các biến bạn muốn truyền vào. Code của bạn có thể đổi thành:

```erb
  <%= render partial: "shared/your_partial", :locals => { :user => user } %>
```

Để dùng biến trong file partial, bạn bỏ `@` rồi gọi nó như một biến thông thường. Lưu ý rằng bạn nên dùng option `:locals` nếu gọi method `render` với key `:partial`.

Có một cách viết tắt của `render` cho phép truyền biến mà không cần dùng option `:locals`:

```erb
  <%= render "shared/your_partial", :user => user %>
```

### Partial ngầm định

Như thường lệ, có một số việc bạn sẽ làm nhiều đến mức Rails cung cấp sẵn cách viết tắt. Một trong số đó là render một model object như User hoặc Post. Nếu muốn có danh sách tất cả user, bạn có thể viết trực tiếp HTML và ERB để hiển thị tên, họ, email... của một user nhiều lần trong file `app/views/users/index.html.erb`, hoặc đặt code đó trong một vòng lặp `each`.

Tuy nhiên, thường tốt nhất là biến User thành partial riêng có tên `_user.html.erb` để có thể tái sử dụng trong các trường hợp khác. Cách gọi cơ bản có thể giống hệt ví dụ trên, như sau:

```erb
  # app/views/index.html.erb
  <h1>Users</h1>
  <ul>
    <% @users.each do |user| %>
      <%= render "user", :locals => {:user => user} %>
    <% end %>
  </ul>
```

Và trong partial:

```erb
  # app/views/_user.html.erb
  <li><%= "#{user.first_name} #{user.last_name}, #{user.email}" %></li>
```

Có thể việc một partial chỉ có một dòng trông hơi lạ, nhưng tin tôi đi, thường nó sẽ không giữ nguyên như vậy lâu đâu, nên bạn nên làm quen với cách này.

Nếu đó là cách cơ bản, vậy cách thần kỳ của Rails là gì? Chỉ cần bảo Rails render trực tiếp object User, chẳng hạn:

```erb
  # app/views/index.html.erb
  <h1>Users</h1>
  <ul>
    <% @users.each do |user| %>
      <%= render user %>     <!-- Lots less code -->
    <% end %>
  </ul>
```

Sau đó Rails tìm file `_user.html.erb` trong thư mục hiện tại và tự động truyền biến `user` vào file đó.

Nếu muốn render cả một nhóm user như chúng ta vừa làm thì sao? Rails cũng làm việc đó theo cách tương tự, giúp bạn không phải tự viết vòng lặp `each` như ở trên. Hãy viết:

```erb
  # app/views/index.html.erb
  <h1>Users</h1>
  <ul>
    <%= render @users %>
  </ul>
```

Trong tình huống này, Rails không chỉ tìm file `_user.html.erb` và truyền cho nó đúng biến `user` để sử dụng, mà còn tự lặp qua toàn bộ user trong collection `@user` của bạn. Khá tiện lợi.

### Các helper method

`render`ing partial không phải method duy nhất bạn có thể gọi từ bên trong view. Rails có rất nhiều helper method tiện dụng để bạn sử dụng trong view. Một vài method phổ biến nhất:

#### #link_to

`link_to` tạo URL cho anchor tag. Thay vì viết:

```erb
  <a href="<%= users_path %>">See All Users</a>
```

Bạn viết:

```erb
  <%= link_to "See All Users", users_path %>
```

Đó là cách làm của Rails. Và hãy nhớ rằng `users_path` tạo một URL tương đối như `/users`, trong khi `users_url` tạo một URL đầy đủ như `http://www.yourapp.com/users`. Trong phần lớn trường hợp, khác biệt này không quan trọng vì trình duyệt xử lý được cả hai, nhưng hãy chắc rằng bạn hiểu sự khác nhau.

### Asset tag

Như bạn có thể đã thấy trong file application layout được nhắc tới ở trên, Rails cung cấp các helper method xuất ra HTML tag để lấy file CSS hoặc JavaScript. Bạn cũng có thể lấy hình ảnh. Chúng được gọi là Asset Tag. Chúng ta sẽ tìm hiểu kỹ hơn về "Asset Pipeline" sau, nhưng về cơ bản các tag này xác định vị trí các file đó dựa trên tên rồi render HTML tag thích hợp.

```erb
  <%= stylesheet_link_tag "your_stylesheet" %>
  <%= javascript_include_tag "your_javascript" %>
  <%= image_tag "happy_cat.jpg" %>
```

Sẽ render ra thứ tương tự như:

```html
  <link href="/assets/your_stylesheet.css" media="all" rel="stylesheet">
  <script src="/assets/your_javascript.js"></script>
  <img src="/assets/happy_cat.jpg">
```

Lưu ý: trong môi trường production, stylesheet và JavaScript của bạn sẽ được gộp vào một file có tên kỳ lạ, vì vậy đừng lo nếu nó có tên như `/assets/application-485ea683b962efeaa58dd8e32925dadf`

### Form

Rails cung cấp nhiều helper khác nhau để giúp bạn tạo form, và chúng ta sẽ tìm hiểu sâu về chúng trong các bài sắp tới.

### Kết luận

Nhìn chung, view tạo nên phần hướng tới người dùng của ứng dụng. Ban đầu có thể hơi khó hình dung cách chọn view nào để render, cần đưa gì vào view đó và dùng partial ra sao, nhưng chỉ cần vài lần làm việc với Rails là bạn sẽ nhanh chóng nắm được các quy ước. View sẽ trở nên quen thuộc tự nhiên.

### Bài tập

Bây giờ bạn đã làm quen với những nội dung khái quát, hãy đọc Rails Guides để tìm hiểu chi tiết hơn. Chương dưới đây thực sự bắt đầu từ controller, nơi bạn cần cho controller biết mình muốn render file view NÀO. Nửa sau của chương đi sâu hơn vào phía view.

<div class="lesson-content__panel" markdown="1">

1. Đọc [chương Layouts và Rendering trong Rails Guide](http://guides.rubyonrails.org/layouts_and_rendering.html), các mục từ 1 đến 3.4. Bạn có thể đọc lướt khi tài liệu bắt đầu trình bày rất nhiều option khác nhau có thể truyền cho một function... biết chúng là gì và tìm chúng ở đâu là tốt, nhưng bạn không cần ghi nhớ tất cả. Thông thường bạn sẽ muốn làm một việc gì đó, tìm kiếm trên Google, rồi thấy một bài đăng Stack Overflow chỉ ra option có thể dùng.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích tới nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ trợ.

- [Bài đăng Stack Overflow về View](https://stackoverflow.com/questions/14429910/an-alternate-explanation-to-rails-layouts-rendering-partials-templates-and-v)
- [Video về mối quan hệ giữa View và Controller](https://www.youtube.com/watch?v=mRJSovhdzWc&ab_channel=GoRails)
- [Video về các tag ERB](https://www.youtube.com/watch?v=na28woOGPUw&ab_channel=NoobandTube) - (video này sẽ yêu cầu bạn tăng âm lượng)
- [Giới thiệu Ruby on Rails từ freeCodeCamp](https://youtu.be/fmyvWz5TUWg) - Nếu muốn tìm hiểu sâu hơn về routing, MVC, CRUD và partial, hãy xem 60 phút đầu của video này; nó tổng kết khá tốt những gì chúng ta đã học. Bạn cũng sẽ được tạo một ứng dụng Rails cơ bản từ đầu nếu muốn luyện tập thêm.