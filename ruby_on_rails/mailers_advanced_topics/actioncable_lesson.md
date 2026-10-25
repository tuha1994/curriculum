### Giới thiệu

Đây là một bài học thực hành cơ bản nhằm chỉ cho bạn cách sử dụng Action Cable để nâng cấp ứng dụng và bổ sung các tính năng thời gian thực. Chúng ta sẽ không giải thích mọi thứ thật chi tiết, vì phần lớn hướng dẫn thiết lập hẳn đã quen thuộc với bạn; tuy nhiên, chúng ta sẽ dành thời gian giải thích kỹ hơn một chút ở những phần riêng về Action Cable.

Ứng dụng chúng ta xây dựng sẽ gồm hai giai đoạn. Ở giai đoạn đầu, đó sẽ là một ứng dụng gửi các tin nhắn chỉ những người đang đăng nhập mới xem được. Sau đó, ở giai đoạn hai, chúng ta sẽ phát triển tiếp để lưu các tin nhắn, nhờ vậy chúng cũng có thể được xem bởi bất kỳ ai đăng nhập trong tương lai.

### Thiết lập

Trước tiên, hãy tạo ứng dụng

```bash
rails new messenger -T
```

Thêm devise vào Gemfile

```bash
bundle add devise
```

`bundle add` vừa thêm gem vào Gemfile vừa chạy bundle install. Điểm hay là nó cũng đặt phiên bản gem thành biến thể mới nhất. Mặt hạn chế là theo mặc định, nó thêm các gem vào cuối Gemfile. Vì vậy hãy dùng lệnh này thận trọng. [Lệnh `bundle add` có một vài tùy chọn bạn có thể sử dụng](https://bundler.io/man/bundle-add.1.html).

Chạy trình cài đặt devise

```bash
rails generate devise:install
```

Mở `config/environments/development.rb` và thêm dòng sau

```ruby
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

Tạo model user và migrate

```bash
rails generate devise user && rails db:migrate
```

Để bắt đầu, chúng ta chỉ cần tạo một vài user trong `db/seeds.rb`

```ruby
User.create(email: 'humblebragger@humblebrag.com', password: 'humblebaby')
User.create(email: 'fitnessgrampacer@test.com', password: 'pacertest')
```

Bạn có thể đặt thông tin user tùy ý. Sau đó chạy tệp seed

```bash
rails db:seed
```

Hãy tạo một ứng dụng một trang cơ bản: user sẽ vào trang chủ, được chuyển hướng đến trang đăng nhập, rồi được đưa trở lại trang chủ. Trước tiên, hãy tạo một controller. Chúng ta sẽ gọi nó là `hangouts`, vì user sẽ tụ tập ở đó mà chẳng làm gì cả.

```bash
rails generate controller hangouts index
```

Đặt trang index làm root trong `routes.rb`

```ruby
root 'hangouts#index'
```

Đồng thời, hãy bảo vệ ứng dụng khỏi việc bị xem trái phép bằng cách thêm đoạn sau vào `app/controllers/application_controller.rb`

```ruby
before_action :authenticate_user!
```

Cuối cùng trong phần thiết lập ban đầu, hãy thêm một chút giao diện với gem Bulma và một thanh điều hướng chỉ có chức năng đăng nhập, đăng xuất. Chúng ta cũng sẽ cài dartsass-rails để Bulma hoạt động phù hợp. (Lưu ý: Nếu bạn nhận được lỗi này - `LoadError: cannot load such file -- sassc`, hãy chạy `rails tmp:clear` rồi thử khởi động lại server.)

```bash
bundle add bulma-rails
./bin/bundle add dartsass-rails
./bin/rails dartsass:install
```

Tiếp theo, để tiết kiệm thời gian, hãy đi tới thư mục `app/assets/stylesheets/` và đổi tên `application.css` thành `application.scss`. Bulma-rails sử dụng scss; dù có những cách khác để xử lý việc này, đây là cách dễ nhất.

Bây giờ mở tệp `application.scss` và thêm đoạn sau ngay cuối tệp

```css
@use "bulma";
```

Cuối cùng, để thêm chức năng cho thanh điều hướng, chúng ta có thể đặt trực tiếp nó vào tệp layout chính. Mở `app/views/layouts/application.html.erb`, rồi thêm đoạn mã sau ngay dưới thẻ mở `<body>` và phía trên dòng `<%= yield %>`.

```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-end">
    <div class="navbar-item">
      <div class="buttons">
        <% if current_user %>
          <%= link_to "Logout", destroy_user_session_path, data: { turbo_method: :delete }, class: 'button is-link' %>
        <% else %>
          <%= link_to "Login", new_user_session_path, class: 'button is-link' %>
        <% end %>
      </div>
    </div>
  </div>
</nav>
```

Đoạn này chủ yếu chỉ sử dụng các kiểu giao diện của Bulma để làm cho ứng dụng trông đẹp hơn.

Bây giờ, nếu khởi động server và mở localhost trong trình duyệt, bạn sẽ cần đăng nhập để truy cập trang chủ và có thể đăng xuất bằng nút trên thanh điều hướng. Nút đăng nhập thực ra khá thừa, vì ngay khi đăng xuất bạn sẽ được đưa đến màn hình đăng nhập do ứng dụng đang được bảo vệ, nhưng cứ để vậy cũng được.

### Action Cable

Bây giờ hãy xử lý việc thiết lập kết nối server. Như đã trình bày trong bài học Action Cable, connection quản lý tất cả channel mà một client đăng ký, đồng thời xử lý việc xác thực và phân quyền.

Các tệp Action Cable không được tạo mặc định khi bạn tạo ứng dụng mới. Trước tiên chúng ta cần tạo một channel; thao tác này cũng sẽ tạo các thư mục và tệp Action Cable cần thiết. Hãy tạo một channel cho message. Trong terminal, hãy nhập

```bash
rails generate channel message
```

Bạn có thể thấy từ kết quả đầu ra rằng nó tạo một số tệp cho chúng ta

```bash
      create  app/channels/application_cable/channel.rb
      create  app/channels/application_cable/connection.rb
      create  app/channels/message_channel.rb
      create  app/javascript/channels/index.js
      create  app/javascript/channels/consumer.js
      create  app/javascript/channels/message_channel.js
```

Thao tác này đã tạo các tệp nền tảng của Action Cable (`connection.rb` và `channel.rb`), cùng với các tệp `message_channel.rb` và `message_channel.js` mà chúng ta sẽ sớm sử dụng.

Bây giờ mở `app/channels/application_cable/connection.rb` để chúng ta có thể cấp quyền cho một connection khi user đăng nhập. Đoạn mã này gần như giống hệt [connection](https://guides.rubyonrails.org/action_cable_overview.html#server-side-components-connections) trong Rails Guides về Action Cable; điểm khác duy nhất là chúng ta có thể sử dụng thông tin user được đặt trên biến môi trường Warden (Devise được xây dựng trên [Warden](https://github.com/wardencommunity/warden/wiki)).

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user

    def connect
      self.current_user = find_verified_user
    end

    private

    def find_verified_user
      if verified_user = env['warden'].user
        verified_user
      else
        reject_unauthorized_connection
      end
    end
  end
end
```

Đoạn mã này dùng để thiết lập kết nối giữa client và server, nhưng sẽ chưa làm gì cho đến khi bạn thiết lập channel. Chính channel sẽ gửi yêu cầu đến server websocket, từ đó bắt đầu quá trình cấp quyền cho connection.

Trong `app/channels/message_channel.rb`, hãy thiết lập stream của chúng ta. Hãy nhớ rằng stream là cách Rails phát broadcast đến các subscriber. Chúng ta sẽ không thao tác trên model, nên không cần `stream_for`. Thay vào đó, chúng ta có thể dùng `stream_from` để chỉ phát stream tới phòng message của mình.

```ruby
class MessageChannel < ApplicationCable::Channel
  def subscribed
    stream_from 'message'
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end
end
```

Bây giờ stream đã được thiết lập, khi refresh bạn sẽ thấy trong log server rằng mình đã kết nối với channel, với dòng `MessageChannel is streaming from message`. Tuyệt.

Việc tiếp theo cần làm là tạo nơi hiển thị message và một form để đăng message trên trang index. Mở `app/views/hangouts/index.html.erb` và thêm đoạn mã sau

```html
<div>
  <div id="message-display">
  </div>
  <div id="message-form">
    <form action="" id="message-form">
      <div class="field has-addons">
        <div class="control">
          <input class="input" id="message-input" type="text">
        </div>
        <div class="control">
          <input type="submit" value="Send Message" class="button is-primary">
        </div>
      </div>
    </form>
  </div>
</div>
```

Chúng ta đã viết một div có id `message-display`, nơi có thể hiển thị các message khi chúng được tạo, rồi thêm một form. Form không có action vì chúng ta không gửi nó đến đâu cả. Chúng ta sẽ cần lấy tham chiếu đến form trong JavaScript; khi form được submit, ta có thể gửi một message đến server websocket. Chúng ta dùng một form HTML cơ bản vì hiện chưa có object hay url để làm việc, nên các helper form của Rails không hữu ích lắm. Sau này, khi tạo Message object để thao tác, chúng ta có thể thay đổi điều này. Tất cả tên class đều là kiểu giao diện của Bulma, nên đừng quá bận tâm đến markup.

Tôi sẽ không quá lo về việc trang phải trông thật đẹp. Nhưng để khu vực hiển thị message có kích thước vừa phải, hãy mở `app/assets/stylesheets/hangouts.scss` và thêm đoạn mã sau.

```css
#message-display {
  height: 80vh;
  overflow: auto
}

#message-input {
  width: 60vw;
}
```

Import tệp này vào `app/assets/stylesheets/application.scss` bằng cách thêm đoạn sau ở cuối tệp đó:

```scss
@import "./hangouts.scss"
```

[Asset Pipeline](https://guides.rubyonrails.org/asset_pipeline.html) sẽ nhận biết những thay đổi này, nhưng nếu không thì:

1. chạy `./bin/dev` để biên dịch lại các asset stylesheet
1. Nếu các thay đổi vẫn chưa được nhận biết, hãy chạy `rails assets:precompile`.

Bây giờ, khi form được submit, chúng ta cần chặn và xử lý nó. Mở `app/javascript/channels/message_channel.js` và xem đoạn mã hiện có.

```javascript

import consumer from "./consumer"

const messageChannel = consumer.subscriptions.create("MessageChannel", {
  connected() {
  },

  disconnected() {
  },

  received(data) {
  }
});
```

Đoạn mã của bạn có thể có một vài comment không quan trọng, nhưng bạn cần xác nhận rằng hằng số `messageChannel` đã được khai báo. Việc đầu tiên cần làm ở đây là đặt một event listener cho thao tác submit của form để lấy giá trị đã nhập. Khi có giá trị đó, chúng ta muốn gửi nó đến server để broadcast cho tất cả subscriber của channel. Đây được gọi là [rebroadcasting](https://guides.rubyonrails.org/action_cable_overview.html#rebroadcasting-a-message). Chúng ta có thể sử dụng `MessageChannel.send` để gửi giá trị trong ô input đến server websocket.

Ngay cuối tệp `message_channel.js`, hãy thêm đoạn mã sau

```javascript
document.addEventListener("turbo:load", () => {
  let form = document.querySelector('#message-form')
  if(form) {
    form.addEventListener('submit', (e) => {
      e.preventDefault()
      let messageInput = document.querySelector('#message-input').value
      if(messageInput == '') return;
      const message = {
        body: messageInput
      }
      messageChannel.send({message: message})
    })
  }
})
```

Trước hết, vì asset JS được tải trong phần head của HTML nên các tệp JavaScript thường được đánh giá trước khi DOM được render. Do đó, trước tiên chúng ta cần đảm bảo DOM đã được tạo. Ta làm việc này bằng event listener `turbo:load`. Tiếp theo, chúng ta kiểm tra xem hiện có đang ở một trang chứa form hay không. Có thể điều này nghe hiển nhiên vì chúng ta chỉ có một trang, nhưng cẩn thận không bao giờ là thừa...

Tiếp theo, nếu đang ở trang có form, chúng ta thêm event listener cho thuộc tính submit của nó. Khi form được submit, trước tiên ta ngăn thao tác submit mặc định. Việc này ngăn trang render lại. Sau đó, ta lấy giá trị từ trường input và kiểm tra xem nó có phải chuỗi rỗng hay không. Nếu rỗng, ta return và không làm gì cả. Nếu không, ta tạo một object JS có key tên là `body`, chứa giá trị của messageInput. Nếu bạn thắc mắc tại sao chúng ta đặt nó trong một object riêng thay vì chỉ gửi `message: messageInput` trong phương thức send, đó là vì thường bạn sẽ submit nhiều hơn một tham số; và khi tạo Message Model, đây cũng tương tự cách nó sẽ được trả về từ controller.

Phần cuối của đoạn mã mới là phần quan trọng. Chúng ta gọi `send` trên `messageChannel` và truyền vào một object có key là `message`, với giá trị là message object đã tạo trước đó. Bạn có thể thấy `messageChannel` được tạo trong dòng `const messageChannel = consumer.subscriptions.create("MessageChannel", {`. Vì vậy nó giữ tham chiếu đến channel được tạo cho message. Khi gọi `send` trên đó, Rails biết phải định tuyến nó tới message_channel ở phía server. Nếu lưu tệp này rồi refresh trình duyệt khi server Rails đang chạy, bạn sẽ có thể submit một message mà không làm trang refresh. Kiểm tra log server, bạn sẽ thấy nội dung tương tự

```bash
Unable to process MessageChannel#receive({"message"=>{"body"=>"hello"}})
```

Điều này xảy ra vì chúng ta chưa có phần xử lý phía server cho message được submit đến server. Phần rebroadcasting trong Rails Guides được liên kết ở trên có trình bày một phương thức `receive` ở phía server để xử lý message đến từ client.

Mở `app/channels/message_channel.rb` và thêm phương thức receive bên dưới phương thức `unsubscribed`

```ruby
def receive(data)
  ActionCable.server.broadcast('message', data)
end
```

Rails xử lý việc định tuyến mọi message đến từ client tới phương thức `receive` trên class đại diện cho channel mà message được gửi qua. Trong phương thức `receive`, chúng ta broadcast message trở lại cho tất cả subscriber của stream `message`. Việc gửi ngược lại có vẻ kỳ lạ, nhưng đó là cách đảm bảo message được tạo trên một client sẽ được gửi đến tất cả client khác đã subscribe channel mà không cần họ refresh trình duyệt. Đây là một trong những lợi ích chính của WebSockets.

Khi dữ liệu được broadcast đến subscriber, nó sẽ quay lại phía client, tới object đang xử lý connection với websocket channel. Trong trường hợp này, đó là `messageChannel` tham chiếu đến connection. Dữ liệu nhận được ở đây, đúng như tên gọi, sẽ đi tới hàm `received` của object messageChannel trong `message_channel.js`. Bên trong `received`, chúng ta cần dùng dữ liệu nhận được để tạo template cho message nhằm hiển thị nó, rồi nối template vào div `#message-display`. Để tạo template, chúng ta có thể dùng JavaScript string template literal. Bên dưới hàm `received`, hãy thêm một hàm mới tên là `template`, hàm này nhận data và trả về một chuỗi HTML. Template có dùng một số kiểu giao diện của Bulma, nên đừng quá bận tâm đến markup thực tế. Nó có thể là bất kỳ HTML nào.

```javascript
const messageChannel = consumer.subscriptions.create("MessageChannel", {
  connected() {
  },

  disconnected() {
  },

  received(data) {
  },

  template(data) {
    return `<article class="message">
              <div class="message-header">
                <p>${data.user.email}</p>
              </div>
              <div class="message-body">
                <p>${data.message.body}</p>
              </div>
            </article>`
  }
});
```

Đừng quên thêm dấu phẩy sau hàm received.

Để hiển thị message trong view, trước tiên chúng ta tìm div có id `message-display` đã tạo trong view index của hangouts. Sau đó, ta có thể tạo template và chèn nó vào document. Việc này sẽ diễn ra trong hàm `received()`, là hàm xử lý các dữ liệu đến từ server websocket.

```javascript
received(data) {
  const messageDisplay = document.querySelector('#message-display')
  messageDisplay.insertAdjacentHTML('beforeend', this.template(data))
}
```

Nếu thử ngay bây giờ, bạn sẽ gặp lỗi. Đó là vì dữ liệu nhận được không biết gì về user; do đó trong template, chỗ viết `${data.user.email}` sẽ không có tham chiếu đến user.

Để sửa, chúng ta cần quay lại phương thức `receive` trong `message_channel.rb`. Hãy nhớ hiện tại nó có dạng

```ruby
def receive(data)
  ActionCable.server.broadcast('message', data)
end
```

Để sửa, chúng ta chỉ cần đặt user vào data. Hãy nhớ data là json đã được chuyển thành hash object trong mã Ruby, và chúng ta có user đã gửi message thông qua `current_user`.

```ruby
def receive(data)
  data['user'] = current_user
  ActionCable.server.broadcast('message', data)
end
```

data sau đó sẽ được chuyển ngược lại thành JSON khi gửi về client để xử lý trong tệp js. Không tệ.

Bây giờ nếu đăng nhập và thử đăng message, bạn sẽ thấy nó được hiển thị gọn gàng. Hãy mở một cửa sổ trình duyệt riêng tư và đăng nhập bằng user còn lại. Đăng một vài message bằng user đó nữa. Điều thú vị ở ứng dụng hiện tại là nó hoạt động như một nơi gặp gỡ bí mật. Chỉ những user đang đăng nhập mới nhìn thấy message khi chúng được đăng. Bất kỳ ai tham gia sau đó sẽ không bao giờ biết trước đó đã nói gì.

Dù vậy khá thú vị, giờ chúng ta sẽ biến ứng dụng thành thứ có lưu các message. Chuẩn bị nhé!

Trước hết, chúng ta cần một message model. Từ template, ta biết nó cần một `body` kiểu `string`, đồng thời cần tham chiếu đến `user` đã tạo nó. Hiện tại chỉ cần vậy.

Trong terminal, nhập

```bash
rails generate model message body:string user:references
```

Sau đó

```bash
rails db:migrate
```

Tiếp theo, chúng ta cần một controller cho các message. Chỉ cần action create vì chúng ta không làm gì khác với chúng

```bash
rails generate controller messages
```

Sau đó trong `routes.rb`, hãy tạo resource

```ruby
resources :messages, only: [:create]
```

Trong user model, thêm phía còn lại của association

```ruby
has_many :messages
```

Giờ chúng ta có thể tạo form bằng các helper của Rails. Trước hết, trong phương thức index của hangouts controller, chúng ta có thể tạo một message object mới cho form sử dụng

```ruby
def index
  @message = Message.new
end
```

Sau đó trong view index của hangouts, chúng ta có thể đổi form sang dùng `form_with`. Bạn không cần thay đổi gì khác, chỉ cần thay form nằm trong div có id `message-form` bằng form sau:

```html
<%= form_with model: @message, local: false do |f| %>
  <div class="field has-addons">
    <div class="control">
      <%= f.text_field :body, id: 'message-input', class: 'input' %>
    </div>
    <div class="control">
      <%= f.submit 'Send Message', class: 'button is-primary' %>
    </div>
  </div>
<% end %>
```

Nếu bây giờ thử submit message, nó vẫn hoạt động. Đó là vì trong tệp `message_channel.js`, chúng ta vẫn có đoạn mã chiếm quyền xử lý thao tác submit và ngăn hành vi mặc định. Bây giờ bạn nên xóa phần mã đó. Mở tệp `message_channel.js` và xóa

```javascript
document.addEventListener("turbo:load", () => {
  let form = document.querySelector('#message-form')
  if(form) {
    form.addEventListener('submit', (e) => {
      e.preventDefault()
      let messageInput = document.querySelector('#message-input').value
      if(messageInput == '') return;
      const message = {
        body: messageInput
      }
      messageChannel.send({message: message})
    })
  }
})
```

Bây giờ submit form có hoạt động trong log server, nhưng rõ ràng chưa có gì xảy ra. Trước khi xử lý việc đó, trước tiên chúng ta cần hiển thị các message hiện có khi tải trang.

Quay lại hangouts controller và thêm dòng sau vào phương thức index

```ruby
@messages = Message.all
```

Sau đó trong view index, hãy lặp qua các message và hiển thị chúng bằng cùng HTML đã dùng trong template. Trong tệp index của hangouts, thêm đoạn mã sau vào div `message-display`.

```html
<% @messages.each do |message| %>
  <article class="message">
    <div class="message-header">
      <%= message.user.email %>
    </div>
    <div class="message-body">
      <%= message.body %>
    </div>
  </article>
<% end %>
```

Xin khen bạn nếu đã nhận ra vấn đề N+1 mà chúng ta vừa tạo ra, do gọi `message.user` cho từng message. Hãy sửa nó trong phương thức index của hangouts controller

```ruby
@messages = Message.includes(:user)
```

Tốt hơn nhiều.

Giờ hãy xử lý phương thức create trong messages controller

```ruby
def create
  @message = current_user.messages.build(message_params)
  @message.save
  ActionCable.server.broadcast('message', @message.as_json(include: :user))
end

private

def message_params
  params.expect(message: [:body])
end
```

Mọi thứ ở đây hẳn đều quen thuộc, ngoại trừ `@message.as_json(include: :user)`. Để gửi dữ liệu đến client thông qua server websocket, dữ liệu phải ở dạng giống json. Nếu chỉ cung cấp `@message`, Rails sẽ tự gọi `as_json` trên object đó. Tuy nhiên, vì muốn đưa cả association user vào, chúng ta cần tự gọi phương thức này.

Chỉ còn một thay đổi nhỏ cuối cùng. Nếu đặt một console.log cho `data` trong hàm received, bạn sẽ thấy bây giờ `body` là thuộc tính trực tiếp của `data`; vì vậy trong hàm template ở `message_channel.js`, chúng ta chỉ cần đổi `${data.message.body}` thành `${data.body}`.

Vậy là xong. Hãy thử gửi message giữa hai user, bạn sẽ thấy message thời gian thực xuất hiện ngay lập tức. Vì giờ message đã được lưu nên sẽ không ai bỏ lỡ những câu chuyện vui trước khi họ tham gia.

### Kết luận

Mục đích ở đây là chỉ cho bạn thấy Action Cable giúp làm việc với WebSockets dễ dàng như thế nào. Bạn cần ít nhất kiến thức JavaScript cơ bản để xử lý mọi việc ở client khi một message được gửi, nhưng ngoài ra bạn có thể thấy công cụ này mạnh mẽ ra sao. Nếu gặp tình huống cần cập nhật cho mọi client đang kết nối mà không phải chờ họ refresh, hãy nghĩ đến WebSockets.

Đây mới chỉ là phần bề mặt của những gì Action Cable có thể làm, nhưng nếu gặp một tình huống mà bạn nghĩ chúng có thể hữu ích, bạn sẽ có thể tìm thấy điều mình cần sau vài lần tìm kiếm.