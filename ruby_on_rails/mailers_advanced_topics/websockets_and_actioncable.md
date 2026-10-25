### Giới thiệu

Trong một tương tác điển hình giữa client và website, trình duyệt gửi một request đến server; sau khi request hoàn tất, server đóng connection.

Có nhiều lúc bạn muốn giữ connection đó mở để server có thể cập nhật cho client khi có điều gì liên quan đến user. WebSockets giải quyết nhu cầu này, còn Action Cable đưa WebSockets vào Rails theo một cách dễ sử dụng.

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài.

- WebSocket là gì.
- WebSockets có thể giúp bạn giải quyết những loại vấn đề nào.
- Action Cable là gì.

### WebSocket là gì?

Trước khi đi sâu vào các chi tiết riêng của Action Cable, trước tiên chúng ta nên hiểu WebSocket là gì và nó muốn giải quyết vấn đề nào.

WebSocket API được tạo ra để cho phép giao tiếp hai chiều giữa server và client. Một kết nối http điển hình giữa server và client mở rồi đóng khi client yêu cầu một thứ gì đó. Kết nối WebSocket giữ kết nối http hoạt động để server có thể gửi các cập nhật đến client mà không cần có một request cụ thể.

WebSocket quản lý connection giữa server và client, đồng thời gửi và nhận dữ liệu thông qua connection đó.

Để hiểu WebSockets giải quyết vấn đề nào, hãy xem xét một ví dụ về nơi WebSockets có thể hữu ích, rồi xem trước WebSocket API chúng ta đã có những lựa chọn nào.

Hãy tưởng tượng chúng ta đã xây dựng một ứng dụng mạng xã hội mới tuyệt vời, Chewbooka. Đây là nơi chúng ta được sống thật với bản thân và nói mọi chuyện về Chewbacca. Một tính năng then chốt của ứng dụng, giúp tất cả chúng ta trở thành tỷ phú, là khi ai đó tạo một bài đăng ca ngợi những phẩm chất của Peter Mayhew, tất cả user khác đang kết nối với ứng dụng đều nhận được bài đăng đó mà không phải tự refresh để cập nhật.

Chúng ta sẽ giàu to! Nhưng trong thời kỳ trước WebSockets, chúng ta sẽ giải quyết việc này thế nào?

Chúng ta có thể dùng JavaScript để đặt một interval, định kỳ liên hệ với server xem có cập nhật nào không. Nếu có, ta có thể thông báo cho user về bài đăng mới và cập nhật view của họ; nếu không, ta chỉ cần trả về một response rỗng. Cách này vẫn phải mở và đóng một request trên server. Kỹ thuật này được gọi là polling, và là một trong những cách đầu tiên các website thử dùng để đưa cập nhật từ server đến client. Nhược điểm là nó không hiệu quả. Nếu không có cập nhật nào cho client, client vẫn request một bản cập nhật từ server. Trong thời kỳ Internet bị giới hạn dung lượng, đây là điều rất tệ.

Vì không muốn sự kém hiệu quả của việc kiểm tra khi chẳng có cập nhật nào, vậy nếu chúng ta làm như sau thì sao: Cho phép client gửi một http request; nếu không có thông tin mới, thay vì gửi response rỗng rồi đóng connection, ta giữ request đó mở ở phía server. Khi có bài đăng mới, ta có thể gửi response đó đến mọi http request đang được giữ mở; việc này sẽ hoàn tất http request và đóng connection. Sau đó client có thể gửi request mới để mở connection lại. Cách này gọi là long-polling và hiện vẫn được nhiều website sử dụng. Nhược điểm của nó là server phải làm việc rất nhiều để liên tục nhận request và giữ chúng mở trong thời gian không xác định; ngoài ra, nếu thứ tự response quan trọng, bạn có thể gặp vấn đề khi có nhiều cập nhật giữa hai request. Có thể trước đây bạn từng gặp hiện tượng này trên một số website, khi thứ tự cập nhật thay đổi sau mỗi lần refresh trang. Một số phòng chat cũ từng hoạt động như vậy.

Có một số cách tiếp cận khác, chẳng hạn Java applet hoặc Cross Frame Communication, nhưng cuối cùng tất cả đều có những nhược điểm khá lớn. Internet ban đầu không được thiết kế cho những loại request này.

Nếu ứng dụng của chúng ta muốn thành công, nếu muốn mang Chewbooka đến với thế giới, chúng ta cần một thứ khác...

WebSockets được tạo ra để làm việc này. Thời khắc của chúng ta đã đến!

### WebSockets và Action Cable

Trong các phiên bản Rails trước đây, khi WebSockets lần đầu xuất hiện, không có cách dễ dàng nào để tích hợp chúng vào ứng dụng. Nhiều gem đã ra đời để giải quyết vấn đề này, nhưng cuối cùng chúng vẫn hơi chắp vá. Đến Rails 5 thì Action Cable xuất hiện — một cách tích hợp sẵn để hỗ trợ WebSockets theo đúng phong cách Rails. Kể từ đó nó đã phát triển khá nhiều để việc tích hợp liền mạch hơn, và giờ đây việc bắt đầu với WebSockets rất dễ dàng. Chúng ta sẽ trình bày những kiến thức cơ bản ở đây, đủ để tạo nền tảng cho bạn tự sử dụng khi tìm thấy trường hợp phù hợp trong các ứng dụng của mình.

#### Thuật ngữ

Điều đầu tiên cần trình bày là một số thuật ngữ bạn sẽ gặp khi làm việc với Action Cable.

Action Cable sử dụng một server để xử lý các connection. Một user có thể có nhiều connection mở tới cùng một server, chẳng hạn khi họ dùng nhiều tab trình duyệt để truy cập website của bạn. Nhưng để đơn giản, phần lớn mọi người chỉ dùng một tab trình duyệt để truy cập một website, và do đó sẽ duy trì một connection tới server Action Cable. Client của connection đó được gọi là consumer.

Mặc dù một consumer giữ một connection tới cable server, nó có thể subscribe, thông qua connection đó, tới nhiều channel. Một channel nên đại diện cho một đơn vị công việc logic. Khi consumer subscribe một channel, nó được gọi là subscriber, còn connection được gọi là subscription.

Sau khi subscribe, mỗi channel có thể stream những thứ gọi là 'broadcastings' đến các subscriber, chẳng hạn các bài đăng mới trên Chewbooka.

Ở đây có khá nhiều thuật ngữ, hơn nữa chúng được xử lý ở cả phía client lẫn server, nên có rất nhiều điều cần cân nhắc — nhưng Rails đã làm phần lớn công việc nặng cho chúng ta.

Khi đi qua các tầng của Action Cable, sẽ rất hữu ích nếu có một ứng dụng Rails thực tế để tham khảo. Nếu bạn vẫn còn ứng dụng Rails cars đã cài trước đây thì đó là lựa chọn lý tưởng để mở các tệp khi chúng ta nhắc đến, qua đó làm quen với vị trí của chúng. Nếu không, chỉ cần khởi tạo một ứng dụng Rails mới cũng hoàn toàn được.

Chúng ta sẽ tách riêng các vấn đề phía server và phía client để cố gắng giữ mọi thứ đơn giản.

Trước tiên hãy xem các tệp xử lý WebSockets. Các tệp Action Cable không được tạo mặc định khi bạn tạo ứng dụng mới. Chúng được tạo lần đầu khi bạn chạy một channel generator (ví dụ: `rails generate channel room`). Sau đó chúng ta sẽ xem những gì được tạo ra cùng với nó.

### Các vấn đề phía server

#### Các WebSocket connection

Như đã đề cập trước đó, mỗi WebSocket được server chấp nhận sẽ tạo một connection quản lý tất cả channel mà user subscribe. Bản thân connection xử lý việc authentication và authorization. Client của connection object này được gọi là consumer.

Sau khi tạo channel, hãy mở `app/channels/application_cable/connection.rb`

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
  end
end
```

Bạn dùng class trên để cấp quyền cho connection đến. Bạn có thể dùng bất kỳ logic nào muốn để nhận diện duy nhất một user. Nếu dùng devise hoặc một gem tương tự và chỉ muốn các user đã đăng nhập được cấp quyền, bạn có thể dùng nó để xác minh connection đến.

Bạn có thể xem ví dụ về cách làm việc đó trong phần Rails Guides về [Action Cable connections](https://guides.rubyonrails.org/action_cable_overview.html#connection-setup). Ở đó, họ tìm một cookie đã mã hóa chứa user_id để xác minh connection, nên có thể giả định rằng bạn đã đặt cookie theo cách này. Như bạn biết, devise đặt một cookie trong session. Tuy nhiên, vì Action Cable hoạt động trên một server riêng biệt dành cho WebSockets, nó không có quyền truy cập session. Thay vào đó, chúng ta có thể dùng object biến môi trường warden. Lý do là Devise được xây dựng trên [Warden](https://github.com/wardencommunity/warden/wiki) và đặt user trên object middleware warden.

Vì vậy, nếu đang dùng devise, một cách gọn gàng để xác minh connection là dùng đoạn sau trong phương thức `find_verified_user`:

```ruby
def find_verified_user
  if verified_user = env['warden'].user
    verified_user
  else
    reject_unauthorized_connection
  end
end
```

Có rất nhiều cách để cấp quyền cho một connection. Chẳng hạn, bạn có thể muốn nó khả dụng cho tất cả user, kể cả những người chưa đăng nhập. Có rất nhiều lựa chọn, nên nếu từng cần một cách khác, chúng ta sẽ để bạn tự tìm hiểu.

#### Channel

Channel là một đơn vị công việc logic, không khác controller trong ứng dụng Rails quá nhiều. Nó xử lý thông tin đến và đi thông qua một channel cụ thể. Nếu đang có một ứng dụng Rails, bạn có thể xem `app/channels/application_cable/channel.rb`. Đây là channel cha mà mọi channel bạn tạo sẽ kế thừa. Bạn có thể đặt ở đây mọi logic cần được dùng chung giữa nhiều channel thay vì lặp lại code.

#### Subscription

Mỗi channel bạn tạo có thể được một hoặc nhiều client subscribe. Sau đó message có thể được broadcast đến channel này, và channel sẽ chuyển tiếp chúng đến mọi subscriber.

### Các vấn đề phía client

#### Kết nối client-server

Consumer của subscription cũng cần một instance của connection ở phía client. Nhờ vậy, khi server broadcast một message, trình duyệt có thể nhận và xử lý nó. Rails tự tạo phần boilerplate này cho bạn khi tạo channel, các tệp nằm trong `app/javascript/channels`.

#### Kết nối consumer

Tệp này sẽ nằm ở `app/javascript/channels/consumer.js` khi bạn tạo channel

```javascript
import { createConsumer } from "@rails/actioncable"

export default createConsumer()
```

Đoạn mã này tạo một consumer, consumer sẽ cố gắng thiết lập connection tới `/cable` trên server của bạn. Mọi channel bạn tạo sẽ import tệp này và dùng nó để tạo subscription đến server.

### Tạo channel

Bây giờ đã nắm được những điều cơ bản về cách Rails xử lý việc tạo connection WebSocket giữa server và client, hãy chuyển sang cách tạo channel và xem thao tác này tạo ra những tệp nào.

#### Trình tạo channel của Rails

Không có gì đáng ngạc nhiên khi Rails cung cấp cho chúng ta một generator để tạo channel mới. Generator này tạo toàn bộ boilerplate code cần dùng, giúp việc bắt đầu đơn giản hơn.

Lệnh này rất dễ hiểu. Hãy tưởng tượng chúng ta muốn tạo một connection WebSocket cho một phòng chat. Trong terminal, chúng ta sẽ nhập

```bash
rails generate channel room
```

Lệnh này sẽ tạo các tệp sau

```bash
      create  app/channels/application_cable/channel.rb
      create  app/channels/application_cable/connection.rb
      create  app/channels/room_channel.rb
      create  app/javascript/channels/index.js
      create  app/javascript/channels/consumer.js
      create  app/javascript/channels/room_channel.js
```

Bạn có thể thấy nó tạo các tệp nền tảng của Action Cable (`connection.rb` và `channel.rb`) cùng các tệp riêng cho channel. Nếu một trong các tệp này đã tồn tại, chẳng hạn do trước đó đã tạo channel, nó sẽ không ghi đè chúng.

#### Lỗi có thể gặp

Bạn có thể nhận được thông báo lỗi như "You have already activated error_highlight 0.6.0, but your Gemfile requires error_highlight 0.5.1. Prepending 'bundle exec' to your command may solve this. (Gem::LoadError)".
Trong trường hợp đó, hãy xóa Gemfile.lock và chạy

```bash
bundle install
```

, thao tác này sẽ tạo một Gemfile.lock mới. Bây giờ lệnh "rails generate channel room" sẽ tạo đúng các tệp như trên.

### Tương tác client-server

Hãy xem kỹ hơn các tệp room_channel.rb và room_channel.js được generator tạo ra.

Như đã đề cập, generator sẽ tạo một channel trong thư mục `app/channels`. Trong ví dụ của chúng ta, đó là `room_channel.rb`, chứa một đoạn boilerplate code:

```ruby
class RoomChannel < ApplicationCable::Channel
  def subscribed
    # stream_from "some_channel"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end
end
```

#### Stream

Stream là cách Rails phát broadcast đến các subscriber. Để đăng ký một subscription, sau khi connection được thiết lập, phương thức subscribed sẽ được gọi và phương thức này nên thiết lập stream.

Action Cable cung cấp hai tùy chọn stream: `stream_from` và `stream_for`.

Điểm khác nhau là `stream_from` mong đợi một chuỗi để nhận diện stream, còn `stream_for` hoạt động trên một model. Về cơ bản, chúng tạo ra thứ tương tự nhau.

```ruby
def subscribed
  stream_from "room"
end

def subscribed
  room = Room.find_by(name: 'someroom')
  stream_for room
end
```

Vì vậy, bạn sẽ dùng `stream_for` khi có một model cụ thể muốn sử dụng cho stream. Lấy ví dụ từ tình huống phòng chat: bạn có thể muốn subscribe vào phòng chat và broadcast khi một message được tạo trong phòng đó. Dùng `stream_for` cho phép Rails tự xử lý phần nặng của việc thiết lập stream, để chúng ta có thể broadcast bằng room object (điều này sẽ rõ hơn sau một chút khi xem các tùy chọn broadcast).

Bạn cũng có thể truyền các tham số từ client đến server và dùng chúng để tạo stream. Chúng ta sẽ xem cách làm phía client ngay sau đây. Ở phía server, việc này không khác cách bạn xử lý trong controller quá nhiều

```ruby
def subscribed
  stream_from "room_#{params[:room]}"
end

def subscribed
  room = Room.find(params[:id])
  stream_for room
end
```

#### Broadcast

Sau khi thiết lập xong WebSocket server, bạn chỉ cần broadcast tới nó từ nơi phù hợp trong ứng dụng. Thông thường, nơi đó sẽ là một controller.

Giống như stream, có một vài cách để broadcast.

Nếu dùng `stream_for`, bạn có thể gọi trực tiếp `broadcast_to` trên chính room channel object. Khi dùng cách này, bạn sẽ truyền ít nhất hai đối số. Đối số đầu tiên là room object mà ban đầu bạn dùng để thiết lập stream; các đối số còn lại là dữ liệu muốn broadcast. Dữ liệu có thể là một danh sách các cặp key-value hoặc một object phản hồi `to_json`, vì cuối cùng dữ liệu được stream ở định dạng có thể chuyển đổi thành json.

Theo ví dụ trước về việc thiết lập stream trên một room, bạn có thể có một message controller, nơi tạo message cho một room liên kết. Phương thức create có thể trông như sau

```ruby
def create
  @room = Room.find(params[:id])
  @message = @room.messages.build(message_params)
  @message.save

  # You can then broadcast to the room like follows
  RoomChannel.broadcast_to(@room, @message)
end
```

Rails tự động gọi `to_json` trên message object của chúng ta. Cú pháp thay thế sẽ như sau

```ruby
RoomChannel.broadcast_to(@room, name: 'Kevin', dork_status: 'Ultimate')
```

Ở đây Rails gọi `to_json` trên các đối số dưới dạng Hash object.

Cách còn lại để broadcast một stream là broadcast trực tiếp từ ApplicationCable server. Tại sao bạn lại muốn làm vậy? Nếu không thiết lập stream trên một object mà dùng `stream_from`, việc dùng `broadcast_to` trên RoomChannel sẽ tìm một stream có định dạng tên cụ thể. Quay lại ví dụ chat, nếu bạn thiết lập stream như sau

```ruby
stream_from "room"
```

rồi thử gọi

```ruby
RoomChannel.broadcast_to("room", @message)
```

Rails sẽ tìm stream có tên `room:room`. Đây chỉ là cách Action Cable hoạt động. Khi dùng `stream_for room`, thực tế nó tạo stream là `room:<some_room_id>`, nên định dạng sẽ đúng khi sau đó bạn gọi `RoomChannel.broadcast_to(@room, @message)`. Khi ấy nó sẽ tìm stream `room:<the_room_id>`.

Vì vậy, nếu chỉ muốn gọi broadcast trên một stream được thiết lập bằng chuỗi thông qua `stream_from`, bạn có thể viết trong controller

```ruby
ActionCable.server.broadcast 'message', @message
```

Bằng cách này, chúng ta truy cập trực tiếp server và tìm stream có tên tương ứng. Cách tiếp cận này có thể cần một chút thời gian để quen, nhưng lựa chọn của bạn thực sự phụ thuộc vào cách đã thiết lập stream.

#### Subscription phía client

Quay lại channel generator ở trên, tệp còn lại mà nó tạo cho chúng ta là `app/javascript/channels/room_channel.js`. Tệp này khá dễ hiểu, chỉ có vài điều chúng ta cần quan tâm lúc này. Hãy xem tệp mặc định mà generator tạo ra:

```javascript
import consumer from "./consumer"

consumer.subscriptions.create("RoomChannel", {
  connected() {
    // Called when the subscription is ready for use on the server
  },

  disconnected() {
    // Called when the subscription has been terminated by the server
  },

  received(data) {
    // Called when there's incoming data on the websocket for this channel
  }
});
```

Lưu ý rằng nó import consumer từ tệp consumer.js đã nói ở trên. Sau đó nó gọi `subscriptions.create` trên consumer. Chúng ta không thực sự cần đi sâu vào cách hoạt động bên trong; chỉ cần hiểu một vài điểm chính.

Điểm đầu tiên là đối số thứ nhất được truyền dưới dạng chuỗi. Cách này sẽ cố gắng kết nối tới channel `RoomChannel` ở server. Tuy nhiên, nó không nhất thiết phải là chuỗi. Hãy nhớ các tham số đã nói trước đó? Đây là nơi bạn có thể truyền chúng. Thay vì chuỗi, bạn có thể truyền một object. Cặp key-value đầu tiên phải có định dạng `channel: 'ChannelName'`, sau đó có thể truyền bao nhiêu cặp key-value tùy ý; chúng sẽ trở thành các tham số gửi tới server để thiết lập connection. Giả sử muốn gửi id của một room tới RoomChannel. Ta có thể viết dòng đầu tiên như sau

```javascript
consumer.subscriptions.create({channel: 'RoomChannel', room: 1}, {
```

Sau đó, ở phía server, ta có thể truy cập các tham số trong RoomChannel như sau

```ruby
room = Room.find(params[:room])
```

Điều này cho phép bạn thiết lập nhiều stream cho cùng một channel bằng cách cung cấp các tham số khác nhau. Khi nào cách này hữu ích? Một ví dụ là đăng ký vào các phòng chat khác nhau bằng cùng một channel object. Hoặc bạn có thể subscribe tới các notification khác nhau cho các ngôn ngữ lập trình khác nhau. Tất cả phải đi qua cùng NotificationChannel, nhưng bạn chỉ muốn gửi dữ liệu phù hợp đến những người đã subscribe notification tương ứng. Params là cách để làm việc đó.

Đối số thứ hai là một object khác chứa ba hàm được định nghĩa sẵn. `connected()` và `disconnected()` được gọi khi channel lần lượt kết nối và ngắt kết nối với server. `received(data)` là nơi bạn sẽ dành phần lớn thời gian tập trung. Hàm này được gọi mỗi khi có thứ gì đó được broadcast qua stream. Việc bạn làm ở đây phụ thuộc vào ứng dụng, nhưng thường sẽ gồm nối thêm thứ gì đó vào DOM hoặc thông báo cho user về một message mới.

Bạn có thể thêm các hàm khác vào object để hỗ trợ xử lý dữ liệu. Dữ liệu sẽ ở định dạng json.

### Một vài điều cuối cùng

Client có thể gửi một message đến server và server có thể rebroadcast message đó cho những client khác. Nếu quan tâm, bạn có thể đọc phần ngắn trong [Rails Guides](https://guides.rubyonrails.org/action_cable_overview.html#rebroadcasting-a-message).

Nếu muốn gửi các tham số động từ client khi thiết lập server, bạn cần cẩn thận. Bạn chỉ có thể lấy params sau khi DOM đã render, vì vậy thông thường sẽ bọc code trong một event listener `turbo:load`. Do đó, nếu không cẩn thận, bạn có thể subscribe cùng một stream nhiều lần khi điều hướng giữa các trang. Action Cable không kiểm tra xem bạn đã subscribe stream hay chưa trước khi thực hiện lại, vì đây có thể là hành vi có chủ đích trong ứng dụng; họ không thể chỉ cho phép một connection tới một channel. Điều này liên quan đến cách turbolinks hoạt động. Bạn có thể đọc bài đăng Stack Overflow này, nơi những người khác từng gặp [duplicate messages from duplicate stream subscriptions](https://stackoverflow.com/questions/39541259/rails-actioncable-turbolinks-chat-issue-posting-duplicate-messages).

Để kiểm thử và phát triển, Action Cable sử dụng async adapter; nhưng trong Production, mặc định nó tìm một redis server. Bạn cần đảm bảo có một server hoạt động nếu muốn chạy ứng dụng trên Heroku hoặc một dịch vụ khác.

Connection chỉ duy trì hoạt động khi http request vẫn còn nguyên vẹn. Refresh trình duyệt hoặc điều hướng sang trang mới sẽ ngắt connection, sau đó connection sẽ được thiết lập lại khi kết nối lại.

### Kết luận

Action Cable còn nhiều điều khác, nhưng đây vẫn là một trường hợp sử dụng khá đặc thù, nên bạn không cần tìm cách dùng nó trong mọi ứng dụng xây dựng. Chỉ nên đưa WebSockets vào khi thấy một cơ hội thực sự để tăng giá trị cho website.

### Bài tập

<div class="lesson-content__panel" markdown="1">

  1. Hãy chắc chắn bạn đã đọc toàn bộ [Rails Guides về Action Cable](https://guides.rubyonrails.org/action_cable_overview.html). Các tài liệu này còn có một số ví dụ full stack hoàn chỉnh có thể hữu ích.
  1. Làm theo [Basic Messaging App](https://github.com/TheOdinProject/curriculum/blob/main/ruby_on_rails/mailers_advanced_topics/actioncable_lesson.md) mà chúng ta đã viết để bạn trải nghiệm việc đưa Action Cable vào một dự án

</div>