### Giới thiệu

Các project này sẽ cho bạn cơ hội thực sự xây dựng một số form, trước hết bằng HTML gần như thuần túy, sau đó chuyển sang sử dụng các phương thức helper mà Rails cung cấp.

### Project: Form thủ công và helper

Trong project này, bạn sẽ xây dựng một form theo cách truyền thống, rồi xây dựng lại theo cách của Rails.

### Bài tập

<div class="lesson-content__panel" markdown="1">

#### Thiết lập back end

Trong các bài học sắp tới, bạn sẽ ngày càng thành thạo việc thiết lập app nhanh chóng bằng cách sử dụng gần như cùng một chuỗi bước này (dù mỗi lần chúng tôi sẽ hướng dẫn bạn ít hơn):

1. Xây dựng một app rails mới (có tên là "re-former").
1. Tạo một repo Github mới và kết nối remote với repo git cục bộ của bạn. Đưa các tệp ban đầu vào stage và commit chúng.
1. Sửa tệp README để ghi một điều bạn sẽ nhớ về sau, chẳng hạn như "Đây là một phần của Forms Project trong Ruby on Rails Curriculum của The Odin Project. Xem tại [https://www.theodinproject.com](https://www.theodinproject.com)"
1. Tạo và migrate một model User với `:username`, `:email` và `:password`.
1. Thêm validation về tính hiện diện cho từng trường trong model.
1. Tạo resource `:users` trong tệp routes để các request thực sự có nơi được xử lý. Dùng tùy chọn `only:` để chỉ định các action `:new` và `:create`.
1. Xây dựng một UsersController mới (bằng tay hoặc thông qua generator `$ rails generate controller Users`).
1. Viết các method rỗng cho `#new` và `#create` trong UsersController.
1. Tạo view `#new` tại `app/views/users/new.html.erb`.
1. Khởi động một rails server trong tab khác.
1. Đảm bảo mọi thứ hoạt động bằng cách truy cập `http://localhost:3000/users/new` trong trình duyệt.

#### Form HTML

Form đầu tiên bạn xây dựng sẽ chủ yếu là HTML (bạn còn nhớ những thứ đó chứ?). Hãy xây dựng nó trong view New tại `app/views/users/new.html.erb`. Mục tiêu là tạo một form gần như giống hệt form bạn nhận được khi dùng helper của Rails, để bạn thấy nó được thực hiện bên dưới như thế nào.

1. Xây dựng một form để tạo user mới. Nếu bạn đã quên hoàn toàn cách form hoạt động, hãy xem [trang W3Schools về form](https://www.w3schools.com/tags/tag_form.asp). Chỉ định các thuộc tính `method` và `action` trong thẻ `<form>` (dùng `$ rails routes` để xem method HTTP và path nào được mong đợi dựa trên resource bạn đã tạo). Đồng thời thêm thuộc tính `accept-charset="UTF-8"`, vì Rails tự nhiên thêm thuộc tính này vào các form của nó để chỉ định bảng mã ký tự Unicode.

1. Tạo các thẻ input phù hợp cho các trường của user (email, username và password). Dùng input password phù hợp cho "password". Nhớ chỉ định thuộc tính `name` cho các input này. Tạo các thẻ label tương ứng với từng trường.

1. Để đảm bảo an toàn CSRF với Rails 7, Turbo được bật mặc định trong các app mới. Turbo chặn việc gửi form và tạo một request XHR từng phần thay vì request HTTP tiêu chuẩn kèm việc tải lại toàn bộ trang. Để hiểu rõ hơn về cơ chế Rails bảo vệ khỏi [cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery), hãy tạm dừng ngắn để vô hiệu hóa Turbo cho form này bằng cách đặt thuộc tính dữ liệu `data-turbo=false`.
Trong tab network của dev tools, hãy so sánh loại request khi có và không có thuộc tính `data-turbo=false` để xác nhận mọi thứ hoạt động như mong đợi.

1. Gửi form và xem output của server. Request sẽ bị chặn trước khi tới controller và server sẽ đưa ra lỗi CSRF `ActionController::InvalidAuthenticityToken  (Can't verify CSRF token authenticity.)`.

    Đó là vì theo mặc định Rails tự động bảo vệ bạn khỏi [cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery) và yêu cầu bạn xác minh rằng form thực sự được gửi từ một trang do bạn tạo ra. Để làm vậy, nó tạo ra một ["authenticity token"](http://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf) trông như một chuỗi vô nghĩa nhưng giúp Rails đối chiếu form với session và application của bạn.

   Vì vậy, nếu muốn tự tạo một form được Rails xử lý, bạn cũng cần cung cấp token theo một cách nào đó. May mắn là Rails cung cấp cho bạn một method có tên `form_authenticity_token` để làm việc đó

   ```erb
   <input
   type="hidden"
   name="authenticity_token"
   value="<%= form_authenticity_token %>"
   >
   ```

   Bây giờ bạn sẽ thấy token trong output của server:

   ```bash
   ...
   Parameters: {"utf8"=>"✓", "authenticity_token"=>"jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=", "email"=>"foo@bar.com", "commit"=>"Submit Form"}
   ```

1. Tuy nhiên, nếu xem output của server, bạn sẽ thấy sau log về các parameter đã nhận thì hầu như không có gì xảy ra. Log sẽ cho biết response đã hoàn tất với status 204 (không có nội dung). Quả thực, nếu xem tab network trong inspector, bạn có thể thấy request đã được gửi nhưng response trả về là `204 No Content`.
1. Như vậy hoàn toàn ổn, vì điều đó có nghĩa là chúng ta đã đi qua action `#create` rỗng trong controller (và không chỉ định bước tiếp theo cần làm gì). Hãy xem output của server. Nó sẽ bao gồm các parameter đã gửi, tương tự như sau:

   ```bash
   Started POST "/users" for 127.0.0.1 at 2013-12-12 13:04:19 -0800
   Processing by UsersController#create as TURBO_STREAM
   Parameters: {"authenticity_token"=>"WUaJBOpLhFo3Mt2vlEmPQ93zMv53sDk6WFzZ2YJJQ0M=", "username"=>"foobar", "email"=>"foo@bar.com", "password"=>"[FILTERED]"}
   ```

Trông rất giống những gì bạn thường thấy khi Rails tự thực hiện việc này, đúng không?

#### Thiết lập controller

1. Mở UsersController và hoàn thiện action `#create` để nhận các parameter đó rồi tạo một User mới từ chúng. Nếu lưu user thành công, bạn nên redirect trở lại form New User (form sẽ trống); nếu không, nó nên render lại form `:new` (nhưng thông tin đã nhập vẫn còn). Bạn có thể dùng đoạn tương tự như sau:

   ```ruby
   # app/controllers/users_controller.rb
   def create
     @user = User.new(username: params[:username], email: params[:email], password: params[:password])

     if @user.save
       redirect_to new_user_path
     else
       render :new, status: :unprocessable_entity
     end
   end
   ```

1. Hãy kiểm tra thử -- bây giờ bạn đã có thể tạo user bằng form chưa? Nếu có, bạn sẽ thấy một lệnh INSERT SQL trong log của server.
1. Chúng ta vẫn chưa xong... cách này trông quá dài và khó khăn để xây dựng user với tất cả các lệnh gọi `params` đó. Sẽ dễ hơn nhiều nếu chỉ cần dùng một hash chứa các thuộc tính của user, để có thể viết như `User.new(user_params)`. Hãy xây dựng nó... form của chúng ta cần gửi một hash thuộc tính được dùng để tạo user, giống như method `form_with` của Rails. Hãy nhớ rằng method đó gửi một trường `user` ở cấp cao nhất, trường này thực sự trỏ tới một hash các giá trị. Tuy vậy, việc này khá dễ -- chỉ cần thay đổi nhẹ thuộc tính `name`. Lồng ba trường User vào trong thuộc tính biến bằng dấu ngoặc vuông trong tên của chúng, ví dụ `name="user[email]"`.
1. Gửi lại. Bây giờ các parameter của user phải được lồng dưới key `"user"` như sau:

   ```bash
   Parameters: {"authenticity_token" => "WUaJBOpLhFo3Mt2vlEmPQ93zMv53sDk6WFzZ2YJJQ0M=", "user" =>{ "username" => "foobar", "email" => "foo@bar.com", "password" => "[FILTERED]" } }
   ```

1. Bạn sẽ gặp một số lỗi vì controller bây giờ cần thay đổi. Nhưng hãy nhớ rằng chúng ta không còn được phép gọi trực tiếp `params[:user]`, vì nó sẽ trả về một hash và các tính năng bảo mật của Rails ngăn chúng ta làm vậy nếu chưa xác thực nó trước.
1. Mở controller và comment dòng trong action `#create` nơi bạn khởi tạo User `::new` (chúng ta sẽ dùng lại nó sau).
1. Triển khai một private method ở cuối có tên `user_params`, method này sẽ `expect` các trường phù hợp (xem lại [bài học Controllers](/lessons/ruby-on-rails-controllers) nếu cần).
1. Thêm một dòng User `::new` mới sử dụng method allow params vừa tạo.
1. Bây giờ hãy gửi form. Nó sẽ hoạt động tuyệt vời (sau khi bạn tự sửa các lỗi đánh máy)!

#### Form theo phong cách Rails với #form_with

Bây giờ chúng ta sẽ bắt đầu chuyển form thành một form Rails hoàn chỉnh bằng `#form_with`, helper này sẽ dùng các object model để xây dựng form.

1. Sửa action `#new` trong controller để khởi tạo một object `User` trống và lưu nó vào một instance variable có tên `@user`.
1. Comment toàn bộ form HTML. Bạn nên lưu lại form để dùng sau nếu bị mắc kẹt.
1. Chuyển thẻ `<form>` sang dùng helper `#form_with` và `@user` từ controller. Điểm hay là bạn không còn cần authentication token nữa, vì method `form_with` sẽ tự động chèn token đó cho bạn.
1. Tạo các trường input bằng cách gọi các method tương ứng trên form object mới. Xem [tài liệu Action View Form Builder](https://api.rubyonrails.org/v8.0.1/classes/ActionView/Helpers/FormBuilder.html) để biết danh sách tất cả input method bạn có thể dùng với `#form_with`.
1. Thử các tùy chọn của method `#input` -- thêm placeholder mặc định (chẳng hạn "<example@example.com>" cho trường email), cho nó tạo một label khác với mặc định (chẳng hạn "Tên user của bạn"), và thử bắt đầu với một giá trị đã được điền sẵn. Có thể bạn sẽ cần Google một số phần, nhưng hãy xem [tài liệu API Rails về `#form_with`](https://api.rubyonrails.org/v6.1.1/classes/ActionView/Helpers/FormHelper.html#method-i-form_with)
1. Kiểm tra thử.

#### Bật lại Turbo

Ở trên, chúng ta đã yêu cầu tắt Turbo để phục vụ bài tập.

1. Bật lại việc gửi form bằng Turbo bằng cách xóa thuộc tính `data-turbo=false` khỏi thẻ form, sau đó cũng xóa input ẩn chứa thẻ CSRF token rồi gửi form.

   Không còn lỗi CSRF nữa sao?!

1. Form giờ được gửi bằng Turbo, vậy mà Rails vẫn bảo vệ bạn bằng cách xác minh CSRF token. Token này đến từ đâu? Hãy kiểm tra inspector và template `application.html.erb`. Bạn có tìm thấy một CSRF token luôn có sẵn không? Hãy xóa cả token này khỏi `application.html.erb`, rồi xác minh rằng server trả về lỗi CSRF.

1. Khôi phục thẻ CSRF token ở cả hai vị trí và tiếp tục.

#### Chỉnh sửa

1. Cập nhật routes và controller để xử lý việc chỉnh sửa một user hiện có. Bạn sẽ cần controller tìm một user dựa trên ID `params` được gửi lên.
1. Tạo view Edit tại `app/views/users/edit.html.erb` và copy/paste form từ view New. Form HTML của bạn (vẫn đang được comment) sẽ không hoạt động -- nó sẽ gửi form bằng request POST trong khi bạn cần request PATCH (PUT) (hãy nhớ `$ rails routes`?). Đây là một cách sửa đơn giản, bạn có thể nhận ra khi thử chỉnh sửa user bằng form `#form_with` (form này đủ thông minh để biết bạn đang chỉnh sửa user hay tạo user mới).
1. Dùng "view source" trên form do `#form_with` tạo ra trong view Edit, đặc biệt chú ý các trường ẩn ở đầu, nằm bên trong `<form>`. Thấy chưa?
1. Sửa phần đầu view form để hiển thị danh sách các thông báo lỗi được gắn với model object thất bại khi nó không vượt qua validation. Hãy nhớ các method `#errors` và `#full_messages`.
1. Lưu project này vào Git và upload lên GitHub.

</div>
