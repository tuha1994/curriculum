### Giới thiệu

Có thể bạn đang thắc mắc các lập trình viên cấu trúc dự án của họ như thế nào. Họ chỉ đặt toàn bộ mã của mình vào một tệp duy nhất phải không? Nếu không, làm sao họ có thể sử dụng mã trên nhiều tệp? Có những quy tắc được thống nhất không? Các mẫu phổ biến? Cách làm việc của Ruby là gì?

Trong bài học này, chúng tôi sẽ chỉ cho bạn cách cấu trúc các dự án Ruby của bạn để giúp chúng có thể quản lý được và giúp bạn và những người khác dễ dàng điều hướng mã bạn đã viết.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Lợi ích của việc chia mã của bạn thành các tệp riêng biệt.
- Cách sử dụng mã từ một tập tin riêng biệt.
- Không gian tên và phạm vi của tệp `require`d.
- Phụ thuộc phát triển là gì.
- Đá quý là gì.
- Bundler là gì và tại sao nó hữu ích.
- `bundle init` làm gì và `Gemfile` là gì.
- Cách tận dụng tối đa tiện ích mở rộng VSCode của `Ruby LSP`.

### Nhầm lẫn, quy ước, tiện lợi

Hãy nhớ lại cách cấu trúc các dự án bạn đã thực hiện trong Foundations. Tất cả HTML, CSS và JS đều tồn tại trong các tệp riêng biệt và dường như là "một" trong trình duyệt nhờ liên kết CSS và JS trong (các) tệp HTML.

Việc sắp xếp các dự án của bạn thành các tệp khác nhau có nhiều lợi ích thiết thực. Điều quan trọng nhất trong số đó là làm cho mã của bạn trở nên mô-đun hơn, giúp điều chỉnh và hiểu mã dễ dàng hơn khi nó trở nên phức tạp hơn.

Hãy nhớ câu nói cổ về cách giữ cho môi trường vật chất của bạn được ngăn nắp—“Một nơi cho mọi thứ và mọi thứ ở đúng vị trí của nó”. Nguyên tắc tương tự cũng áp dụng cho các dự án phần mềm.

Đối với các dự án Ruby, nguyên tắc chung là:

- Một lớp cho mỗi tập tin. Mỗi khi bạn tạo một lớp mới, bạn nên tạo một tệp mới để chứa nó.
- Đó là quy ước để đặt tất cả các tệp Ruby của bạn vào một thư mục lib. Ví dụ:

  ```text
  project_name
  ├── lib
  │   └── lovely_file_of_yours.rb
  └── main.rb
  ```

### Sử dụng nhiều tập tin

Nếu bạn muốn chia mã của mình thành nhiều tệp, trước tiên bạn cần biết cách đảm bảo mã từ một tệp có thể được sử dụng trong một tệp khác. Hãy xem xét cấu trúc tập tin này:

```text
├── lib
│   ├── sort
│   │   ├── bogo_sort.rb
│   │   ├── bubble_sort.rb
│   │   └── merge_sort.rb
│   └── sort.rb
└── main.rb
```

Có hai cách chính để làm điều đó: `require_relative` và `require`.

#### require_relative

Giả sử trong thiết bị đầu cuối của bạn, bạn hiện đang ở thư mục gốc của dự án (thư mục chứa `main.rb`). `main.rb` và `lib/sort.rb` có nội dung sau:

```ruby
# main.rb
require_relative 'lib/sort'
```

```ruby
# lib/sort.rb
require_relative 'sort/bubble_sort'
require_relative 'sort/bogo_sort'
require_relative 'sort/merge_sort'
```

Hãy bắt đầu với cách các tài liệu xác định chức năng của nó:

> `require_relative(string) → true or false`
>
> Ruby cố tải thư viện có tên `string` tương ứng với thư mục chứa tệp yêu cầu. Nếu tệp không tồn tại, `LoadError` sẽ được nâng lên. Trả về `true` nếu tệp đã được tải và `false` nếu tệp đã được tải trước đó.

Phần quan trọng ở đây là *liên quan đến thư mục chứa tệp yêu cầu*. Điều này có nghĩa là bất kể bạn thực thi mã từ đâu, `require_relative` sẽ tìm kiếm tệp được chỉ định từ quan điểm của tệp mà nó đã được ghi vào. Vì vậy, `main.rb` chỉ cần truy cập `lib` để tìm `sort` (`.rb` là ẩn) và `sort.rb` sẽ truy cập `sort` để tìm ba loại khác nhau đó. Đủ đơn giản phải không?

#### require

`require` phức tạp hơn. Hãy lấy *một số* tài liệu ở đây:

> Nếu đối tượng địa lý là đường dẫn tuyệt đối (ví dụ: bắt đầu bằng `'/'`), đối tượng địa lý sẽ được tải trực tiếp bằng đường dẫn tuyệt đối. Nếu đối tượng địa lý là đường dẫn tương đối rõ ràng (ví dụ: bắt đầu bằng `'./'` hoặc `'../'`), đối tượng địa lý sẽ được tải bằng đường dẫn tương đối từ thư mục hiện tại. Nếu không, tính năng này sẽ được tìm kiếm trong thư mục thư viện được liệt kê trong `$LOAD_PATH`.

Bit đường dẫn tuyệt đối có vẻ tự giải thích. Khi bạn sử dụng đường dẫn tương đối, sự khác biệt giữa việc sử dụng `require` và `require_relative` là `require` giải quyết nó theo quan điểm của thư mục nơi bạn đang chạy mã. Hãy thay đổi ví dụ của chúng ta, một lần nữa hãy tưởng tượng thiết bị đầu cuối của bạn hiện nằm trong thư mục gốc của dự án có `main.rb`:

```ruby
# main.rb
require 'lib/sort'
```

```ruby
# lib/sort.rb
require 'sort/bubble_sort'
require 'sort/bogo_sort'
require 'sort/merge_sort'
```

À. Tất nhiên—có lỗi—nó không thể tìm thấy `lib/sort`! Đó không phải là những đường dẫn tương đối... `require_relative` ưa thích và nó ngầm cho rằng các đường dẫn là tương đối!

```ruby
# main.rb
require './lib/sort'
```

```ruby
# lib/sort.rb
require './sort/bubble_sort'
require './sort/bogo_sort'
require './sort/merge_sort'
```

Bây giờ nó báo không tìm thấy `./sort/bubble_sort`! Điều này là do nó không tìm kiếm nó từ quan điểm của `sort.rb` mà từ quan điểm của `main.rb`.

Còn phần `$LOAD_PATH` thì sao?

```ruby
# main.rb
require 'csv'

require_relative 'lib/sort'
```

`require 'csv'` sẽ tìm kiếm `csv.rb` trong biến toàn cục `$LOAD_PATH` của Ruby, theo mặc định chứa thư viện chuẩn Ruby. Có những phần mở rộng tệp khác mà nó có thể tìm kiếm, nhưng điều này không quan trọng tại thời điểm này—chỉ cần nhớ rằng `require` tìm kiếm một số phần mở rộng như `.rb` mà không cần phải khai báo chúng một cách rõ ràng. Ngoài ra, nếu nó không tìm thấy tệp đó trong `$LOAD_PATH`, nó sẽ xem qua các gem đã cài đặt (sẽ nói thêm về chúng sau) để xem liệu tệp có ở đó hay không.

Cả hai cách tiếp cận đó (`require` và `require_relative`) đều sẽ thực thi tệp, cho phép bạn sử dụng nội dung của chúng. Nếu bạn cố gắng yêu cầu thứ gì đó lần thứ hai, sẽ không có gì xảy ra và những `require` đó sẽ trả về `false`.

Quy ước là `require_relative` được sử dụng cho mã của riêng bạn trong khi `require` được sử dụng cho những thứ bên ngoài mã đó, chẳng hạn như đá quý mà ứng dụng của bạn phụ thuộc vào.

Lợi ích của phương pháp này là bạn không cần phải giữ tất cả mã cho một phần ứng dụng của mình trong một tệp. Giả sử đây là cấu trúc tệp của bạn:

```text
├── lib
│   ├── flight.rb
│   ├── hotel.rb
│   └── airport.rb
└── main.rb
```

Và nội dung tập tin của bạn:

```ruby
# lib/airport.rb
class Airport
  def introduce
    puts "I'm at the airport!"
  end
end
```

```ruby
# lib/flight.rb
class Flight
  def introduce
     puts "I'm on the flight!"
  end
end
```

```ruby
# lib/hotel.rb
class Hotel
  def introduce
     puts "I'm at the hotel!"
  end
end
```

```ruby
# main.rb
require_relative 'lib/airport'
require_relative 'lib/flight'
require_relative 'lib/hotel'

Airport.new.introduce
#=> I'm at the airport!

Flight.new.introduce
#=> I'm on the flight!

Hotel.new.introduce
#=> I'm at the hotel!
```

Vì vậy, thay vì xác định cả hai lớp `Flight` và `Hotel` bên trong `airport.rb`, chúng ta có thể thực hiện điều đó trong các tệp riêng biệt. Theo thông lệ, bạn sẽ yêu cầu tất cả các tệp trong tệp trên cùng của mình, như `main.rb` ở đây. Điều này cho phép mọi người chỉ cần nắm giữ `main.rb` và họ nhận được toàn bộ mã của bạn khi họ cần. Tùy theo nhu cầu mà họ sẽ sử dụng cách tải file đó phù hợp.

Một điều khác cần lưu ý là các biến cục bộ không được tải, vì vậy nếu `airport.rb` của bạn có biến cục bộ `coolest_airports`, việc cố truy cập biến cục bộ đó trong `main.rb` sẽ gây ra lỗi. Tuy nhiên, các hằng số sẽ được tải nên bạn có thể truy cập chúng.

#### Không gian tên

Điều quan trọng cần ghi nhớ là tất cả mã được yêu cầu đều được đặt trong cùng một không gian tên. Điều này có nghĩa là nếu bạn có cùng tên cho các phương thức, mô-đun, lớp, v.v., chúng sẽ được thêm vào cùng nhau theo thứ tự được yêu cầu. Ví dụ: giả sử bạn và bạn của bạn đã sử dụng cùng một tên phương thức và bạn đang cố gắng sử dụng mã của họ và của bạn (để đơn giản, giả sử tất cả các tệp đều nằm trong cùng một thư mục):

```ruby
# not_so_green.rb
def food_opinion(food)
  "#{food} is awesome!"
end
```

```ruby
# scheals.rb
def food_opinion(food)
  "#{food} is awful!"
end
```

```ruby
# main.rb
require_relative 'not_so_green'
require_relative 'scheals'

puts food_opinion('Cereal')
#=> Cereal is awful!
```

Vì `food_opinion` được xác định hai lần nên định nghĩa cuối cùng sẽ thắng. Để đảm bảo mã không bị ghi đè, Rubyist bọc mã của họ trong các mô-đun mang lại cho họ những lợi ích của không gian tên:

```ruby
# not_so_green.rb
module NotSoGreen
  def self.food_opinion(food)
    "#{food} is awesome!"
  end
end
```

```ruby
# scheals.rb
module Scheals
  def self.food_opinion(food)
    "#{food} is awful!"
  end
end
```

```ruby
# main.rb
require_relative 'not_so_green'
require_relative 'scheals'

puts NotSoGreen.food_opinion('Cereal')
#=> Cereal is awesome!
puts Scheals.food_opinion('Marmite')
#=> Marmite is awful!
puts food_opinion('Cereal')
#=> Errors out—there's no longer a free floating food_opinion method to use.
```

### Đá quý và bạn

Bây giờ bạn đã biết cách làm việc với các tệp của riêng mình, đã đến lúc học cách làm việc với các tệp của người khác.

Đá quý là các gói chứa các thư viện tiện ích Ruby mà ai đó đã viết—về cơ bản là một số mã. Một số gem đó là một phần của thư viện chuẩn Ruby, nhưng hầu hết đều yêu cầu cài đặt độc lập.

Nếu bạn sử dụng một viên ngọc thì bạn gọi viên ngọc đó là một phần phụ thuộc—mã của bạn phụ thuộc vào viên ngọc đó để hoạt động bình thường. Một số phần phụ thuộc chỉ được sử dụng trong các ngữ cảnh cụ thể; ví dụ: bạn có thể có một bộ đá quý chỉ được sử dụng trong môi trường phát triển hoặc thử nghiệm.

Nhiều loại đá quý phụ thuộc vào các loại đá quý khác và đôi khi, phiên bản mà chúng phụ thuộc vào khác nhau. Bạn *có thể* tự mình quản lý đá quý và các phần phụ thuộc của chúng, trải qua quá trình cài đặt, cập nhật chúng và giải quyết mọi xung đột có thể phát sinh, nhưng những người theo chủ nghĩa Ruby có công cụ cho việc đó!

RubyGems đã là một phần của thư viện chuẩn Ruby kể từ phiên bản 1.9 và được sử dụng để đưa những viên ngọc tuyệt vời đó vào máy tính của bạn. Bạn có nhớ thông tin về `require` khi xem xét các viên ngọc đã cài đặt để có thể tìm thấy tệp bạn đang tìm kiếm không? Đó là công việc của RubyGems. Một phần thú vị khác về nó? Bản thân nó là một viên ngọc quý! Hãy thử xem.

Tạo một tệp Ruby mới `main.rb` trong thư mục có tên `colorful`:

```ruby
require 'colorize'

puts 'Red goes faster!'.colorize(:red)

puts "I'm blue da ba dee da ba di!".colorize(:blue)

puts "It ain't easy bein' green...".colorize(:green)
```

Bạn có thể muốn xem tất cả các màu đó, vì vậy hãy chạy tệp của bạn với `ruby main.rb` để xem chúng... hay đúng hơn là `LoadError`. Đúng rồi—bạn cần cài đặt viên ngọc đó trước! Làm điều đó với `gem install colorize` và bạn sẽ thấy RubyGems hoạt động. Hệ thống của bạn hiện có quyền truy cập vào đá quý `Colorize`!

Đợi đã, *hệ thống của bạn*—còn những người khác muốn sử dụng mã của bạn thì sao? Vâng, họ cũng sẽ cần `gem install`—không có vấn đề gì lớn.

Nhưng nếu bạn có hàng chục viên đá quý thì sao? Làm thế nào để bạn đảm bảo rằng các phiên bản bạn sử dụng là phiên bản giống với phiên bản mà người khác tải xuống? Điều này nghe có vẻ khá tẻ nhạt. Nhập: Bundler. Đó là một viên ngọc khác, một phần của RubyGems, nhưng được phát hành độc lập.

Bundler cho phép bạn khai báo những loại đá quý mà dự án của bạn cần — tùy theo phiên bản của chúng. Đối với những người khác, Bundler cho phép họ lấy tuyên bố đó, một tệp đơn giản có tên `Gemfile` và sử dụng nó để cài đặt những viên ngọc đó trong `bundle install` nhanh chóng.

Vì các lượt cài đặt đá quý mang tính toàn cầu nên bạn cần một cách để chỉ chạy những phiên bản đá quý cụ thể được khai báo trong `Gemfile`. Bạn có thể làm điều đó bằng cách sử dụng `bundle exec`, sau đó là lệnh bạn muốn thực thi—rất có thể là `bundle exec ruby foo.rb`.

Hãy đảm bảo rằng bất kỳ ai muốn sử dụng tập lệnh của chúng tôi đều có thể làm như vậy:

```bash
bundle init # creates a default Gemfile in the current working directory
bundle add colorize # adds the colorize gem to the Gemfile and runs bundle install
```

Điều này đã tạo ra hai tệp: `Gemfile` và `Gemfile.lock`. Chúng ta hãy nhìn vào cả hai:

```ruby
# Gemfile

# frozen_string_literal: true

source "https://rubygems.org"

# gem "rails"

gem "colorize", "~> 1.1"
```

```ruby
# Gemfile.lock
GEM
  remote: https://rubygems.org/
  specs:
    colorize (1.1.0)

PLATFORMS
  ruby
  x86_64-linux # This might be different for you if you're using a different CPU and OS.

DEPENDENCIES
  colorize (~> 1.1)

BUNDLED WITH
   2.5.4
```

Không có nhiều thứ trong số đó, nhưng như bạn có thể thấy, `Gemfile` có thông tin về nơi lấy đá quý và loại đá quý nào được yêu cầu.

`"~> 1.1"` là một ràng buộc về phiên bản, đặc biệt là một ràng buộc bi quan. Nó dựa vào phiên bản ngữ nghĩa.

1. Số đầu tiên là phiên bản **chính**.
1. Phiên bản thứ hai là phiên bản **nhỏ**.
1. Số thứ ba, nếu có, là số **bản vá**.

Các phiên bản chính có thể làm hỏng nhiều thứ so với các phiên bản trước—ví dụ: thay đổi tên phương thức. Các phiên bản nhỏ có thể thêm và thay đổi mọi thứ nhưng không thể phá vỡ bất cứ điều gì. Các bản vá xảy ra khi bạn giới thiệu các bản sửa lỗi không làm hỏng bất kỳ thứ gì.

Vì vậy, nếu những người đứng sau viên ngọc duy trì nó phù hợp với phiên bản ngữ nghĩa, thì bạn có thể dựa vào ràng buộc bi quan này là không bao giờ để dự án của bạn có phiên bản đá quý có khả năng phá vỡ ứng dụng của bạn—nó tương đương với `gem "colorize", ">= 1.1", "< 2.0"`.

`Gemfile.lock` có thông tin về môi trường cuối cùng có thể chạy ứng dụng của bạn. Bundler sẽ sử dụng nó để cài đặt các phiên bản đá quý tương tự ngay cả khi `Gemfile` có khả năng cho phép cài đặt các phiên bản mới hơn.

### .ruby-phiên bản

Có một điều quan trọng khác cần cung cấp cho những người sẽ chạy mã của bạn: phiên bản Ruby mục tiêu của dự án của bạn. Bạn có thể thực hiện việc này một cách dễ dàng bằng cách chạy `rbenv local 3.2.2` vì nó tạo tệp `.ruby-version` với phiên bản được khai báo—3.2.2. Kiểm tra lệnh bằng cách sử dụng một trong các phiên bản Ruby đã cài đặt rbenv của bạn. Chạy `rbenv versions` để xem danh sách các phiên bản Ruby bạn đã cài đặt.

Nhiều công cụ khác nhận ra điều này để tìm ra phiên bản Ruby mà dự án của bạn đang chạy—ví dụ: `rbenv` sẽ không còn sử dụng phiên bản `global` Ruby nữa và tiện ích mở rộng `Ruby LSP` VSCode cũng sẽ thay đổi hành vi của nó.

### Ruby LSP trong VSCode

Trước đó trong chương trình giảng dạy, bạn đã được hướng dẫn chọn tùy chọn `Don't show again` khi `Ruby LSP` thông báo cho bạn về việc không tìm thấy tệp khóa—bạn cũng có thể đã thấy các lỗi liên quan đến `RuboCop`. Trong bài học tiếp theo, chúng ta sẽ xem xét `RuboCop` và cách nó sẽ xuất hiện trong `Gemfile` của dự án của bạn.

Sau đó, bạn sẽ tận hưởng tất cả lợi ích của việc sử dụng `Ruby LSP` và tích hợp `RuboCop` của nó, đồng thời các dự án của bạn sẽ được thiết lập như một chuyên gia thực thụ.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc [Yêu cầu hiểu biết của Eric Mathison và Những người bạn trong Ruby](https://github.com/ericmathison/articles/blob/master/understanding-require-in-ruby.md).
1. Hãy xem [Thông tin cơ bản về RubyGems](https://guides.rubygems.org/rubygems-basics/).
1. Đọc lướt qua [Hướng dẫn mẫu từ RubyGems](https://guides.rubygems.org/patterns/), chú ý kỹ hơn đến [`Semantic versioning`](https://guides.rubygems.org/versioning/), [`Declaring dependencies`](https://guides.rubygems.org/gemfile-and-gemspec/#:~:text=Declaring%20dependencies) và [`Loading code`](https://guides.rubygems.org/patterns/#:~:text=name%20you%20like.-,Loading%20code,-At%20its%20core).
1. Đọc lướt qua [Bundler's Bắt đầu](https://guides.rubygems.org/getting_started/) và [Câu hỏi thường gặp](https://guides.rubygems.org/faq/).
1. Kiểm tra [README của phần mở rộng Ruby LSP VSCode] (https://github.com/Shopify/ruby-lsp/tree/main/vscode#readme).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- [Nghe một trong những người bảo trì của Bundler nói về việc quản lý các phần phụ thuộc trong Ruby](https://www.youtube.com/watch?v=j2V-A8vvLP0) hoặc cách khác là [đọc bài nói chuyện](https://andre.arko.net/2015/04/28/how-does-bundler-work-anyway/).
- [Thông tin thêm về phiên bản ngữ nghĩa—có lẽ bây giờ các cam kết thông thường sẽ có ý nghĩa hơn với bạn!](https://semver.org)
- [Nếu bạn muốn có một cách tiếp cận bài học khác, bài viết của Connor Stack có thể hữu ích](https://medium.com/@connorstack/understanding-ruby-load-require-gems-bundler-and-rails-autoloading-from-the-bottom-up-3b422902ca0).
