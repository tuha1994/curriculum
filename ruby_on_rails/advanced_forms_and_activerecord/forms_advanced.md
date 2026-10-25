### Giới thiệu

Ở các bài học trước, bạn đã tìm hiểu những kiến thức cơ bản về cách xây dựng biểu mẫu trong HTML và Rails, và với kiến thức đó bạn có thể làm được rất nhiều việc.  Tuy nhiên, cũng có những trường hợp mà việc tạo ra trải nghiệm người dùng tốt đòi hỏi phải xây dựng một biểu mẫu xử lý nhiều thứ cùng lúc (ví dụ: các đối tượng model).  Người dùng chỉ thích nhấp nút gửi một lần, vì vậy bạn nên có khả năng mang lại cho họ trải nghiệm mà họ mong muốn.

Trong phần này, chúng ta sẽ xem xét một số lựa chọn giúp biểu mẫu xử lý nhiều đối tượng model cùng lúc.  Bạn cũng sẽ học cách điền sẵn các đối tượng vào một menu thả xuống.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Làm việc với các đối tượng model trong biểu mẫu.
- Cách xây dựng menu thả xuống trong biểu mẫu.
- Cách tạo biểu mẫu lồng nhau.
- Cách cho phép các tham số lồng nhau.
- Cách xóa các bản ghi thông qua các trường biểu mẫu.

### Điền sẵn các thẻ select bằng collection

Rails cung cấp một vài cách tiện dụng để tạo các menu thả xuống đã chứa sẵn dữ liệu khi biểu mẫu được tải (nếu không thì chúng sẽ không hữu ích lắm).

Giả sử bạn muốn xây dựng biểu mẫu Bài viết mới cho blog, nhưng muốn có thể chọn tác giả từ danh sách người dùng của mình.  Bạn sẽ cần tạo một menu thả xuống gửi ID của người dùng như một phần của hash `params`.  Vì vậy, bạn có thể điền `@users` trong posts controller:

```ruby
  # app/controllers/posts_controller.rb
  ...
  def new
    @users = User.all
    @post = Post.new
  end
  ...
```

Cách làm HTML thuần túy là tạo một loạt thẻ `<option>` (bên trong thẻ `<select>`).  Bạn có thể dễ dàng tạo chúng trong mã ERB bằng cách lặp qua một collection, chẳng hạn nếu bạn muốn chọn một bài viết để xem từ danh sách các bài viết.

```erb
  # app/views/posts/new.html.erb
  ...
  <select name="user_id">
    <%= @users.each do |user| %>
      <option value="<%= user.id %>"><%= user.name %></option>
    <% end %>
  </select>
  ...
```

Đoạn này tạo một danh sách thả xuống với tên của mỗi người dùng làm một lựa chọn.  Action `#create` của bạn sẽ nhận một thuộc tính có tên `user_id`, và bạn có thể dùng nó để gắn một tác giả với bài viết đó.

Nhưng Rails cung cấp những cách ít dài dòng hơn để làm điều tương tự, cụ thể là dùng helper `#select_tag` kết hợp với helper `#options_for_select`.  `#select_tag` sẽ tạo thẻ bao quanh, còn `#options_for_select` cung cấp cho `#select_tag` mảng các lựa chọn mà nó cần.

`#options_for_select` yêu cầu một đầu vào rất cụ thể -- một mảng các mảng cung cấp văn bản cho lựa chọn trong menu thả xuống và giá trị mà lựa chọn đó đại diện.  Vì vậy, `options_for_select([["choice1",1],["choice2",2]])` sẽ tạo một vài thẻ option, mỗi thẻ tương ứng với một lựa chọn.  Điều này rất tiện, vì đó chính xác là thứ mà `#select_tag` yêu cầu cho đối số thứ hai.  Điểm cần lưu ý duy nhất là bạn phải chuyển collection `@users`, vốn chứa đầy đủ các đối tượng User, thành một mảng chỉ có `name` và `value`.  Dùng `#map` là đủ đơn giản:

```ruby
  # app/controllers/posts_controller.rb
  ...
  def new
    @user_options = User.all.map{ |u| [ u.name, u.id ] }
    @post = Post.new
  end
  ...
```

```erb
  # app/views/posts/new.html.erb
  ...
  <%= select_tag(:author_id, options_for_select(@user_options)) %>
  ...
```

Vậy là chỉ cần truyền cho `#select_tag` tên mà nó nên dùng cho giá trị được chọn cùng collection, và nó sẽ xuất ra chính xác kết quả tương tự!

Nếu muốn tránh hoàn toàn việc dùng `options_for_select` và biểu mẫu của bạn được thiết kế để xây dựng một instance của model (ví dụ, một đối tượng Post), chỉ cần dùng helper tổng quát hơn là `#select` trong view:

```erb
  # app/views/posts/new.html.erb
  ...
  <%= select(:post, :author_id, @user_options) %>
  ...
```

Bạn vẫn cần truyền tham số `:post` (cho biết biểu mẫu của bạn đang xây dựng một đối tượng Post) để thẻ `select` có thể đặt đúng `name`... trong trường hợp này, nó sẽ đặt tên cho thẻ là `<select name="post[author_id]" id="post_author_id">`.  Điều đó có nghĩa là (hãy nhớ!) thuộc tính `author_id` sẽ xuất hiện trong `params`, lồng bên trong hash `post`.

Đầu vào `:author_id` của helper `#select` ở trên không chỉ đại diện cho tên của giá trị được chọn (như trong `#select_tag`), mà còn cho biết tên cột mà nó đại diện trong model gốc của bạn (ở đây là model Post).  Ban đầu điều này có thể hơi phiền, vì bạn không thể tùy ý đặt tên cho lựa chọn của mình.

Nếu bạn có một biểu mẫu `#form_with` được phân phạm vi dưới biến `f`, bạn không cần truyền symbol `:post` ở trên (nó lấy thông tin đó từ `f`), nên có thể dùng:

```erb
  # app/views/posts/new.html.erb
  ...
    <%= f.select(:author_id, @user_options) %>
  ...
```

Phải mất một chút thời gian mới đi đến đây, nhưng hy vọng giờ bạn có thể thấy phương pháp này đơn giản đến mức nào khi cần tạo một menu thả xuống có thể rất lớn.

Bạn sẽ dùng các menu thả xuống khá thường xuyên để điền association của một model, ví dụ tác giả của một Post.  Trong phần tiếp theo, chúng ta sẽ nói về cách thực sự tạo cả hai đối tượng model từ một biểu mẫu duy nhất.

*Lưu ý: Nếu bạn từng dùng gem `simple_form`, gem này có cách riêng để xử lý các collection đối tượng, hơi khác với cách trên (được cho là `simpler` hơn).*

### Biểu mẫu lồng nhau

Bạn có một biểu mẫu để tạo một trong các đối tượng User (giả sử là cho ứng dụng bản sao Amazon.com), nhưng cũng muốn biểu mẫu đó tạo một hoặc nhiều đối tượng ShippingAddress (một User có thể có nhiều đối tượng như vậy).  Làm thế nào để một biểu mẫu có thể tạo cả hai, để người dùng không bị mắc kẹt trong việc nhấp một loạt nút gửi?

Đây là một quy trình gồm nhiều phần.  Nó liên quan đến controller, view, model và route của bạn... toàn bộ đội ngũ MVC!  Ý chính là biểu mẫu của bạn sẽ gửi đối tượng chính (ví dụ User) như bình thường, nhưng đồng thời lén gửi thêm một loạt thuộc tính cho các đối tượng khác mà bạn muốn tạo (ví dụ các đối tượng ShippingAddress).  Model của bạn phải được chuẩn bị cho việc này.  Nó sẽ tạo không chỉ đối tượng User ban đầu mà còn cả các đối tượng lồng nhau cùng lúc.

Như bạn có thể hình dung, việc liệt kê đúng tên và tham số là rất quan trọng để toàn bộ phép màu này có thể diễn ra phía sau.

Chúng ta sẽ xem xét khái quát quy trình:

1. Bạn cần chuẩn bị model User để nó biết tạo một hoặc nhiều đối tượng ShippingAddress nếu nhận được thuộc tính của chúng khi tạo một User thông thường.  Việc này được thực hiện bằng cách thêm vào model User một method có tên `#accepts_nested_attributes_for`, method này nhận tên của một association, ví dụ:

   ```ruby
     # app/models/user.rb
     class User < ActiveRecord::Base
       has_many :shipping_addresses
       accepts_nested_attributes_for :shipping_addresses
     end
   ```

1. Đảm bảo bạn đã cho phép `params` bao gồm các thuộc tính lồng nhau bằng cách đưa chúng vào method Strong Parameters của controller một cách phù hợp.  Hãy xem phần đọc thêm để biết ví dụ về cách làm.
1. Xây dựng biểu mẫu trong view.  Dùng method `#fields_for` để tạo một `#form_with` bên trong biểu mẫu `#form_with` hiện có một cách hiệu quả.

Có một vài khía cạnh mới trong quy trình này.  Với `#fields_for`, về cơ bản bạn tạo một biểu mẫu bên trong một biểu mẫu (điều này hợp lý vì phía sau nó thực sự được `#form_with` sử dụng).  Trong ví dụ này, chúng ta có thể tạo ba “biểu mẫu con” cho các đối tượng ShippingAddress bằng cách dùng association, chẳng hạn:

```erb
<%= form_with model: @user do |f| %>
    ...
    <% 3.times do %>
    <%= f.fields_for @user.shipping_addresses.build do |sub_form| %>
        ...
        <%= sub_form.text_field :zip_code %>
        ...
    <% end %>
    <% end %>
    <%= f.submit %>
<% end %>
```

Lưu ý rằng chúng ta cũng có thể (và nên) tạo các đối tượng shipping_address mới trong controller thay vì trong view; ở đây làm vậy chỉ nhằm mục đích minh họa.

Method `#accepts_nested_attributes_for` khá dễ hiểu và phần tài liệu sẽ hữu ích.

Phần đọc thêm sẽ trình bày chi tiết hơn về cách cho phép các tham số lồng nhau.

### Xóa các đối tượng của biểu mẫu lồng nhau

Bạn cũng có thể để biểu mẫu hủy các biểu mẫu lồng nhau bằng cách trước tiên đặt tùy chọn `:allow_destroy` thành `true` cho method `#accepts_nested_attributes_for`, ví dụ `accepts_nested_attributes_for :shipping_addresses, :allow_destroy => true`.  Sau đó, bất cứ khi nào muốn hủy một đối tượng ShippingAddress từ biểu mẫu của User, chỉ cần đưa khóa `_destroy => 1` vào các tham số được gửi cho ShippingAddress đó.

### Quan hệ nhiều-nhiều

Nếu bạn có một quan hệ `has_many :through`, có lẽ bạn sẽ cần tiến thêm một bước bằng cách [chỉ rõ rằng mỗi phía của quan hệ là inverse của phía còn lại](http://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through).

### Tự thiết kế biểu mẫu

Đôi khi, dù Rails cung cấp rất nhiều helper tiện lợi, bạn vẫn muốn làm một việc không theo chuẩn.  Trước hết, bạn nên tự hỏi liệu đây có phải là cách dễ dàng và thẳng thắn nhất để thực hiện hay không.  Nếu nó vượt qua bài kiểm tra về mùi thiết kế, hãy cứ xây dựng biểu mẫu.

Trong quá trình học, thường dễ nhất (và cũng là một thực hành tốt) là bắt đầu với dạng biểu mẫu HTML cơ bản nhất.  Nếu bạn không hiểu chuyện gì đang xảy ra trong HTML cơ bản (và hãy nhớ đưa CSRF token vào), thì bạn sẽ hoàn toàn bế tắc khi cố dùng helper.  Khi đã nắm chắc mọi thứ, hãy dần đưa các helper Rails như `#form_tag` và `#form_with` vào.

Đừng nản lòng nếu bạn gặp những khoảnh khắc thật sự khó hiểu khi xây dựng các biểu mẫu không theo chuẩn.  Cần có một chút kinh nghiệm để cảm thấy thoải mái.  Và nếu mọi thứ trở nên quá rối ren, có thể bạn cần đánh giá lại cách tiếp cận của mình (chính xác thì bạn đang hy vọng đạt được điều gì với biểu mẫu phức tạp đó?) rồi bắt đầu lại.

### Biểu mẫu đơn giản

`simple_form` là một gem của Platformatec, có thể thực sự giúp cuộc sống của bạn dễ dàng hơn (nếu bạn không làm điều gì quá điên rồ).  Nó cung cấp nhiều tính năng thân thiện với người dùng để xây dựng biểu mẫu và hiện được sử dụng rộng rãi.

Bạn có thể tự xem [tài liệu của `simple_form`](https://github.com/plataformatec/simple_form) và bắt đầu sử dụng nó trong các ứng dụng của mình tùy nhu cầu.

### Các vấn đề linh tinh: gửi trống có nghĩa là xóa

Đôi khi, với một bản ghi đã tồn tại, bạn muốn bỏ chọn một menu thả xuống hoặc không chọn bất kỳ checkbox nào, nhưng muốn điều đó biểu thị rằng các trường liên quan thực sự phải được đặt thành `nil`.  Tuy nhiên, thông thường khi gửi biểu mẫu, nó sẽ không chứa trường nào trong số đó và phần back end sẽ không biết rằng bạn thực sự muốn xóa các trường ấy, nên sẽ không có gì xảy ra.  Làm thế nào để xử lý việc này?

Hãy thử tạo một trường ẩn trong biểu mẫu (hoặc biểu mẫu lồng nhau) có cùng tên với các checkbox hoặc menu thả xuống, nhưng chỉ chứa giá trị `""`.  Khi đó, thuộc tính này sẽ luôn xuất hiện trong hash `params`, bất kể chuyện gì xảy ra, và bạn có thể xử lý việc xóa các bản ghi theo cách phù hợp.

Đôi khi [các method helper của Rails sẽ tự chèn trường ẩn cho bạn](https://api.rubyonrails.org/v8.0.0/classes/ActionView/Helpers/FormHelper.html#method-i-checkbox-label-Gotcha), nhưng hãy chắc chắn rằng bạn biết chính xác biểu mẫu của mình thực sự gửi gì (nếu có) khi bỏ chọn tất cả các lựa chọn!

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Đọc Rails Guide về Forms, phần 5, trong đó trình bày [cách điền một biểu mẫu bằng một collection các đối tượng](https://guides.rubyonrails.org/form_helpers.html#collection-related-helpers).
  1. Đọc Rails Guide về Forms nói trên, phần 9, trong đó trình bày [cách tiếp nhận dữ liệu biểu mẫu lồng nhau](http://guides.rubyonrails.org/form_helpers.html#building-complex-forms).
  1. Đọc Rails Guide về Forms nói trên, phần 8, trong đó trình bày [các quy ước tham số cho biểu mẫu lồng nhau](https://guides.rubyonrails.org/form_helpers.html#form-input-naming-conventions-and-params-hash).

</div>

#### Kết luận

Chúng ta đã tìm hiểu hai trường hợp sử dụng phổ biến hơn của các biểu mẫu phức tạp -- điền sẵn các đối tượng vào biểu mẫu và tạo nhiều đối tượng bằng một biểu mẫu duy nhất.  Đến lúc này, ngay cả khi vẫn chưa thật sự thoải mái, bạn cũng nên có đủ công cụ cần thiết để bắt tay tạo một biểu mẫu.  Chúng ta sẽ giúp bạn thực hành trực tiếp trong dự án, đừng lo.

Phần hay nhất là gì?  Đây gần như là những nội dung khái niệm phức tạp nhất khi học Rails.  Thực ra, chúng thậm chí không thực sự đặc thù cho Rails... một khi bạn đã quen với HTML mà biểu mẫu yêu cầu và cách các tham số được gửi đến controller, việc ánh xạ chúng vào đúng quy ước hoặc helper của Rails sẽ là phần dễ dàng.  Vì vậy, mọi thứ bạn học được có thể chuyển giao sang gần như mọi biểu mẫu bạn từng tạo.

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem như tài liệu bổ sung.

- [Tài liệu Simple Form trên GitHub](https://github.com/plataformatec/simple_form)
- [Tài liệu về `accepts_nested_attributes_for`](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html)
- [Một ví dụ khác về biểu mẫu lồng nhau trên Stack Overflow](http://stackoverflow.com/questions/15648396/rails-how-to-manage-nested-attributes-without-using-accepts-nested-attributes?rq=1)
- [Dùng `inverse_of` để làm cho `accepts_nested_attributes_for` hoạt động với các quan hệ `has_many :through`](http://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through)
- [Tìm hiểu về authenticity token của biểu mẫu Rails](http://stackoverflow.com/questions/941594/understand-rails-authenticity-token)
- [Vì sao không nên hardcode secret token của ứng dụng trong production](http://daniel.fone.net.nz/blog/2013/05/20/a-better-way-to-manage-the-rails-secret-token/)
