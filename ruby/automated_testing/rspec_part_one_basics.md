### Giới thiệu

Trong bài học này, chúng ta sẽ tìm hiểu toàn diện về RSpec, một framework kiểm thử cho Ruby cung cấp các công cụ giúp bạn cấu trúc và viết các bài kiểm thử tự động.

Trong hệ sinh thái Ruby có [một vài lựa chọn khác nhau](https://www.ruby-toolbox.com/categories/testing_frameworks.html) khi nói đến các framework kiểm thử. Tuy nhiên, RSpec phổ biến hơn hẳn; tại thời điểm viết bài, nó đã có hơn nửa tỷ lượt tải xuống kể từ khi được tạo ra.

RSpec được ưa chuộng phần lớn nhờ cú pháp phong phú, giàu tính biểu đạt, giúp việc viết các bài kiểm thử dễ đọc và dễ bảo trì trở nên đơn giản.

Bài học này được thiết kế như một hướng dẫn về tất cả các tính năng RSpec cơ bản mà bạn cần biết để bắt đầu tự viết các bài kiểm thử tự động. Để tận dụng bài học tốt nhất, hãy làm theo tất cả các ví dụ.

### Mục tiêu học tập

- Cài đặt RSpec như thế nào
- Example group là gì và định nghĩa nó ra sao?
- Khối `it` dùng để làm gì?
- Expectation là gì?
- Matcher là gì?
- Ba giai đoạn của một bài kiểm thử là gì?
- Khi nào nên sử dụng giai đoạn teardown?

### Thiết lập dự án và cài đặt RSpec

Để bắt đầu, chúng ta cần tạo một dự án mới, nơi có thể cài đặt RSpec và bắt đầu sử dụng nó.

Mở terminal, tạo một thư mục mới rồi chuyển vào đó:

~~~bash
$ mkdir odin_rspec
$ cd odin_rspec
~~~

Tạo thư mục lib và tệp `todo_list.rb`

~~~bash
$ mkdir lib && touch lib/todo_list.rb
~~~

Mở tệp `todo_list.rb` bằng trình soạn thảo văn bản bạn yêu thích và dán đoạn mã sau vào đó. Đây sẽ là phần mã mà chúng ta viết các bài kiểm thử cho trong suốt phần đầu của bài học này.

~~~ruby
# lib/todo_list.rb

class TodoList

  attr_reader :tasks

  def initialize(name, tasks = [])
    @name = name
    @tasks = tasks
  end

  def name
    "#{@name} (#{remaining_tasks})"
  end

  def add_task(task)
    @tasks << task
  end

  def complete_task(task)
    tasks.delete(task)
  end

  def completed?
    tasks.empty?
  end

  private

  def remaining_tasks
    if tasks.size == 1
      "1 task"
    else
      "#{tasks.size} tasks"
    end
  end

end
~~~

Giờ khi phần mã cần kiểm thử đã sẵn sàng, đã đến lúc cài đặt và thiết lập RSpec. Quay lại terminal, bảo đảm bạn đang ở thư mục gốc của dự án `odin_rspec`, rồi tạo một `Gemfile`

~~~ruby
$ touch Gemfile
~~~

Gem là các thư viện Ruby do những lập trình viên khác viết, cho phép chúng ta dễ dàng thêm chức năng có sẵn vào dự án. Trong trường hợp này, chúng ta muốn thêm RSpec vào dự án.

Dán nội dung sau vào `Gemfile`:

~~~ruby
source 'https://rubygems.org'

gem 'rspec', '3.10'
~~~

Quay lại terminal, chúng ta sẽ chạy bundle install để gem RSpec khả dụng trong dự án:

~~~ruby
$ bundle install
~~~

Tiếp theo, chạy script cài đặt RSpec; script này sẽ tạo cho chúng ta một vài tệp. Chúng ta sẽ giải thích công dụng của chúng ở phần sau.

~~~ruby
$ rspec --init
  create   .rspec
  create   spec/spec_helper.rb
~~~

Mở tệp `.rspec` vừa được tạo; lúc này nó chỉ nên chứa đoạn mã sau:

~~~ruby
--require spec_helper
~~~

Thêm nội dung sau vào tệp rồi lưu lại:

~~~ruby
--require spec_helper
--format documentation
--order rand
~~~

Việc cuối cùng cần làm là tạo tệp chứa các bài kiểm thử cho lớp `TodoList`.

Trong RSpec, các tệp này thường được gọi là tệp *spec* vì tên tệp được nối thêm `_spec.rb`.

~~~ruby
$ touch spec/todo_list_spec.rb
~~~

Cuối cùng, để xác nhận mọi thứ đã được thiết lập đúng, chạy `$ rspec`; kết quả của lệnh này nên bao gồm nội dung sau:

~~~ruby
0 examples, 0 failures
~~~

Vậy là xong, chúng ta đã có một dự án RSpec đang hoạt động. Cấu trúc thư mục cuối cùng của dự án sẽ như sau:

~~~bash
.
├── .rspec
├── Gemfile
├── Gemfile.lock
├── lib
│   └── todo_list.rb
└── spec
    ├── todo_list_spec.rb
    └── spec_helper.rb
~~~

`Gemfile` là nơi chúng ta liệt kê các dependency bên ngoài cần thiết để dự án chạy đúng. Trong dự án này, chúng ta chỉ cần gem RSpec để viết và chạy các bài kiểm thử RSpec.

`Gemfile.lock` được tự động tạo sau khi chúng ta chạy lệnh bundle install ở trên. Tệp này khóa các gem ở những phiên bản cụ thể để dự án luôn sử dụng cùng phiên bản, bất kể được cài đặt trên máy nào.

Hiện tại, đó là tất cả những gì bạn thực sự cần biết về các Gemfile, nhưng chúng ta sẽ thêm một liên kết vào phần tài nguyên bổ sung nếu bạn muốn tìm hiểu thêm.

Thư mục lib là nơi chứa mã nguồn thực thi thực tế của chúng ta. Trong trường hợp này là lớp todo list.

Thư mục spec là nơi đặt tất cả các tệp kiểm thử. Chúng ta không muốn trộn mã kiểm thử với mã nguồn vì mọi thứ sẽ nhanh chóng trở nên rối rắm và khó phân biệt phần kiểm thử với phần mã nguồn. Vì vậy, thông lệ tốt là đặt mã kiểm thử trong các tệp riêng, bên trong một thư mục khác.

Cuối cùng, lệnh `rspec --init` mà chúng ta chạy trước đó đã tạo một vài tệp. Tệp đầu tiên là `.rspec`, nơi chúng ta thêm cấu hình dòng lệnh bổ sung cho lúc chạy lệnh `rspec` để thực thi các bài kiểm thử. Chẳng hạn như định dạng đầu ra RSpec trong terminal và việc có chạy các bài kiểm thử theo thứ tự ngẫu nhiên hay không.

Tệp thứ hai là `spec_helper.rb`, nơi chúng ta thêm các cấu hình cần được chia sẻ giữa nhiều tệp kiểm thử. Trong bài học này, chúng ta sẽ không làm gì thêm với hai tệp đó. Tuy vậy, biết mục đích của chúng vẫn rất hữu ích.

### Cấu trúc một tệp kiểm thử RSpec

Mở tệp `todo_list_spec.rb` đã tạo trước đó. Việc đầu tiên cần làm là require một vài dependency. Dependency đầu tiên là tệp `spec_helper` do RSpec tạo cho chúng ta.

Tệp thứ hai là `todo_list.rb` trong lib, để chúng ta có thể tạo một instance của lớp `TodoList` trong tệp kiểm thử rồi gọi các phương thức của nó và kiểm tra chúng thực hiện điều gì.

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"
~~~

#### Example group

Bây giờ hãy tạo example group cấp cao nhất. Example group có hai mục đích: tổ chức tệp kiểm thử bằng cách nhóm các bài kiểm thử liên quan lại với nhau, và cho phép chúng ta *mô tả* hành vi đang kiểm thử bên trong nhóm đó.

Chúng ta định nghĩa một example group bằng phương thức `describe`. Trong ví dụ sau, chúng ta tạo một example group cho tất cả các bài kiểm thử liên quan đến lớp `TodoList`.

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

end
~~~

Các example group có thể được lồng vào nhau. Điều này cho phép nhóm các bài kiểm thử liên quan một cách chi tiết hơn. Chúng ta sẽ thấy cách hoạt động bằng việc tạo một example group lồng nhau, nơi đặt tất cả bài kiểm thử cho phương thức `#name` của lớp `TodoList`.

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  describe "name" do

  end
end
~~~

Phương thức describe nhận một đối số có thể là tên lớp hoặc một chuỗi. Thông thường, bạn truyền tên lớp đang kiểm thử cho example group ngoài cùng và dùng chuỗi cho các example group lồng nhau.

Sau này chúng ta sẽ tìm hiểu vì sao muốn dùng tên lớp cho một số example group và chuỗi cho những nhóm khác. Hiện tại, điều quan trọng nhất cần ghi nhớ là đối số này được dùng làm nhãn mô tả hành vi sẽ được kiểm thử trong example group.

Trong trường hợp của chúng ta, example group cấp cao nhất mô tả rằng tất cả bài kiểm thử bên trong dành cho lớp `TodoList`.

Example group lồng nhau mô tả rằng tất cả bài kiểm thử bên trong dành cho phương thức `name` của lớp `TodoList`.

Việc cấu trúc tệp kiểm thử và cung cấp các nhãn mô tả như vậy giúp bạn dễ đọc tệp kiểm thử và xác định phần nào đang được kiểm tra.

Phương thức describe cũng nhận một block ( `do..end`). Như đã thấy, các example group lồng nhau có thể nằm trong block này. Nhưng chúng ta cũng có thể đặt các example riêng lẻ trong đó, hay nói cách khác là các bài kiểm thử thực tế.

#### Khối it

Các khối `it` được dùng để định nghĩa những example riêng lẻ bên trong example group. Tương tự phương thức describe, `it` cũng là một phương thức nhận một đối số cho phép chúng ta diễn đạt điều mong đợi xảy ra trong bài kiểm thử đó, chẳng hạn `it "returns the users name"`.

Tuy nhiên, khác với phương thức `describe`, đối số này **luôn phải** là một chuỗi và chúng ta không thể lồng bất cứ thứ gì bên trong khối `it`. Đó là vì các khối `it` được dành riêng cho mã kiểm thử thực tế.

Hãy tạo một vài khối `it` để kiểm thử phương thức `name` của todo list:

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  describe "name" do
    it "returns the name with 0 tasks remaining" do
    end

    it "returns the name with 1 task remaining" do
    end

    it "returns the name with 2 tasks remaining" do
    end
  end
end
~~~

Để thấy toàn bộ phần cấu trúc vừa làm hoạt động, chúng ta cần thực thi các bài kiểm thử. Mở terminal và chạy lệnh `$ rspec` từ thư mục gốc của dự án `odin_rspec`.

Đầu ra trong đoạn mã dưới đây dùng các đối số chúng ta truyền cho example group và các khối `it` để in cấu trúc bài kiểm thử ra terminal.

~~~bash
TodoList
  name
    returns the name with 1 task remaining
    returns the name with 0 tasks remaining
    returns the name with 2 tasks remaining

Finished in 0.00247 seconds (files took 0.1574 seconds to load)
3 examples, 0 failures
~~~

#### Thứ tự kiểm thử

Có thể bạn nhận thấy các bài kiểm thử không hiển thị theo thứ tự chúng ta viết. Khi thiết lập dự án trước đó, chúng ta đã cấu hình để các bài kiểm thử chạy theo thứ tự ngẫu nhiên bằng cách thêm dòng `--order rand` vào tệp `.rspec`.

Chúng ta làm vậy vì mỗi bài kiểm thử phải mang tính tất định, nghĩa là chúng độc lập với nhau và thực thi theo cùng một cách, bất kể thứ tự là gì.

Nếu các bài kiểm thử luôn chạy theo cùng một thứ tự, chúng ta có thể viết các bài kiểm thử phụ thuộc vào thứ tự đó để thành công. Điều này cuối cùng sẽ dẫn đến một bộ kiểm thử rất mong manh và khó bảo trì.

#### Kiểm thử đang chờ

Đầu ra cũng hiển thị ba khối `it` riêng lẻ màu xanh. Trong kiểm thử, màu xanh nghĩa là bài kiểm thử đang đạt. Nhưng điều đó chưa hoàn toàn đúng, vì các example của chúng ta chưa kiểm thử điều gì cả.

Theo mặc định, RSpec coi các khối `it` rỗng là bài kiểm thử đạt. Thay vào đó, hãy đặt chúng ở trạng thái pending để biểu thị rằng chúng vẫn cần được hoàn thành.

Có [một vài cách khác nhau để đặt các khối it ở trạng thái pending trong RSpec](https://relishapp.com/rspec/rspec-core/v/3-10/docs/pending-and-skipped-examples/skip-examples). Nhưng trong phạm vi bài này, chúng ta sẽ thêm chữ x vào đầu mỗi khối it để đặt chúng ở trạng thái pending:

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  describe "name" do
    xit "returns the name with 0 tasks remaining" do
    end

    xit "returns the name with 1 task remaining" do
    end

    xit "returns the name with 2 tasks remaining" do
    end
  end
end
~~~

Chạy lại bộ kiểm thử bằng lệnh `$ rspec`. Lúc này đầu ra sẽ gần giống kết quả terminal sau, trong đó các khối `it` được in màu vàng. Màu vàng báo hiệu một bài kiểm thử đang pending và cần được hoàn thành.

~~~bash
Pending: (Failures listed here are expected and do not affect your suite's status)

  1) TodoList name returns the name with 0 tasks remaining
     # Temporarily skipped with xit
     # ./spec/todo_list_spec.rb:6

  2) TodoList name returns the name with 2 tasks remaining
     # Temporarily skipped with xit
     # ./spec/todo_list_spec.rb:12

  3) TodoList name returns the name with 1 task remaining
     # Temporarily skipped with xit
     # ./spec/todo_list_spec.rb:9

Finished in 0.00179 seconds (files took 0.06131 seconds to load)
3 examples, 0 failures, 3 pending
~~~

### Tạo expectation

Giờ khi cấu trúc cơ bản đã sẵn sàng, đã đến lúc viết mã kiểm thử thực tế. Để kiểm thử một điều gì đó trong RSpec, chúng ta đặt ra các *expectation* về việc mã sẽ làm gì.

Để minh họa, hãy viết bài kiểm thử cho khối `it` đầu tiên. Xóa x khỏi phương thức `it` để nó không còn pending, rồi đặt nội dung sau vào trong block:

~~~ruby
# spec/todo_list_spec.rb

it "returns the name with 0 tasks remaining" do
  todo_list = TodoList.new("My List")

  expect(todo_list.name).to eq("My List (0 tasks)")
end
~~~

Có lẽ chỉ cần đọc mã là bạn đã đoán được tác dụng của nó: nó kỳ vọng tên của todo list vừa tạo bằng `"My List (0 tasks)"`.

Chạy bộ kiểm thử bằng lệnh `$ rspec` trong terminal. Bạn sẽ thấy đầu ra của bài kiểm thử vừa hoàn thành có màu xanh. Điều đó nghĩa là chúng ta vừa viết bài kiểm thử đạt đầu tiên 🎉

~~~bash
TodoList
  name
    returns the name with 2 tasks remaining (PENDING: Temporarily skipped with xit)
    returns the name with 1 task remaining (PENDING: Temporarily skipped with xit)
    returns the name with 0 tasks remaining

Pending: (Failures listed here are expected and do not affect your suite's status)

  1) TodoList name returns the name with 2 tasks remaining
     # Temporarily skipped with xit
     # ./spec/todo_list_spec.rb:13

  2) TodoList name returns the name with 1 task remaining
     # Temporarily skipped with xit
     # ./spec/todo_list_spec.rb:10

Finished in 0.00179 seconds (files took 0.06131 seconds to load)
3 examples, 0 failures, 2 pending

Randomized with seed 3657
~~~

#### Các phần của một expectation

Một expectation trong RSpec gồm hai phần riêng biệt. Phần đầu là giá trị thực tế mà chúng ta muốn đặt expectation lên: `expect(todo_list.name).to`

Phần thứ hai là matcher, được dùng để xác minh giá trị thực tế **khớp** với giá trị kỳ vọng: `eq("My List (0 tasks)")`

Hai phần này kết hợp lại tạo thành một expectation hoàn chỉnh trong RSpec:

~~~ruby
expect(actual_value).to eq(expected_value)
~~~

#### Expectation phủ định

Giả sử bạn muốn kỳ vọng một thứ gì đó không bằng một thứ khác. RSpec có hỗ trợ cho trường hợp này. Bạn có thể dùng expectation `expect(actual_value).not_to`, làm điều ngược lại với `expect(actual_value).to` và sẽ thất bại nếu expectation **có** khớp.

Để xem cách này hoạt động, hãy thêm một expectation phủ định vào test đầu tiên cho phương thức name. Trong expectation này, chúng ta kỳ vọng tên todo list **không bằng** `"Barrys List (0 tasks)"`

~~~ruby
# spec/todo_list_spec.rb

it "returns the name with 0 tasks remaining" do
  todo_list = TodoList.new("My List")

  expect(todo_list.name).to eq("My List (0 tasks)")
  expect(todo_list.name).not_to eq("Barrys List (0 tasks)")
end
~~~

Chạy lại các bài kiểm thử bằng `$ rspec`. Test đầu tiên vẫn phải đạt.

#### Bài kiểm thử thất bại

Cho đến giờ chúng ta đã thấy bài kiểm thử pending và bài kiểm thử đạt, nhưng bài kiểm thử thất bại trông như thế nào? Hãy tìm hiểu. Xóa `x` khỏi phương thức `it` của bài kiểm thử thứ hai đã viết trước đó rồi đặt nội dung sau vào block của nó:

~~~ruby
# spec/todo_list_spec.rb

it "returns the name with 1 task remaining" do
  todo_list = TodoList.new("My List")

  expect(todo_list.name).to eq("My List (1 task)")
end
~~~

Chúng ta kỳ vọng tên todo list hiển thị rằng nó có một task. Chạy kiểm thử để xem expectation có đúng không: `$ rspec`

~~~bash
1) TodoList name returns the name with 1 task remaining
     Failure/Error: expect(todo_list.name).to eq("My List (1 task)")

       expected: "My List (1 task)"
            got: "My List (0 tasks)"

       (compared using ==)
     # ./spec/todo_list_spec.rb:15:in `block (3 levels) in <top (required)>'
~~~

Như bạn có lẽ đã dự đoán, bài kiểm thử này thực sự thất bại. Đầu ra có màu đỏ, biểu thị một bài kiểm thử thất bại.

Đầu ra cũng hiển thị thông tin hữu ích cho biết điều gì đã sai. Có hai phần đặc biệt quan trọng giúp bạn gỡ lỗi nguyên nhân thất bại.

Phần đầu tiên là kết quả cho biết bài kiểm thử kỳ vọng điều gì và thực tế nhận được gì. Thông tin này thường đủ để bạn biết điều gì đã sai và nên bắt đầu sửa từ đâu.

~~~ruby
expected: "My List (1 task)"
got: "My List (0 tasks)"
~~~

Một phần hữu ích khác là số dòng nơi bài kiểm thử thất bại. Trong những tệp kiểm thử lớn và phức tạp hơn, khi kết quả expected và got không cung cấp đủ manh mối về lỗi, thông tin này sẽ là điểm bắt đầu hữu ích để truy tìm nguyên nhân bài kiểm thử thất bại.

~~~ruby
# ./spec/todo_list_spec.rb:16:in `block (3 levels) in <top (required)>'
~~~

Hãy nhanh chóng sửa bài kiểm thử này để nó hoạt động. Chúng ta chỉ cần truyền vào một mảng tasks chứa một task:

~~~ruby
# spec/todo_list_spec.rb

it "returns the name with 1 task remaining" do
  todo_list = TodoList.new("My List", ["task 1"])

  expect(todo_list.name).to eq("My List (1 task)")
end
~~~

Chạy lại các bài kiểm thử bằng `$ rspec`. Lúc này chúng ta sẽ có 2 bài kiểm thử đạt và còn lại một bài pending.

### Matcher

Từ đầu đến giờ, chúng ta đã dùng matcher `eq` trong các expectation. Nhưng RSpec có rất nhiều matcher khác nhau để làm cho bài kiểm thử giàu tính biểu đạt hơn trong đủ loại tình huống.

Ở đây chúng ta sẽ giới thiệu một vài matcher mà bạn có khả năng thấy hữu ích nhất. Tuy nhiên, bạn nên xem qua [tài liệu matcher của RSpec](https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers) để biết những matcher khác đang có sẵn.

#### Matcher Eq

Matcher `eq` là matcher linh hoạt và thường dùng nhất. Có lẽ đây sẽ là matcher bạn dùng nhiều nhất. Nó so sánh giá trị thực tế với giá trị kỳ vọng và trả về true nếu chúng **khớp**. Nếu không, nó trả về false.

Hãy xem matcher này hoạt động một lần nữa bằng cách hoàn thành các bài kiểm thử cho phương thức name của todo list:

~~~ruby
# spec/todo_list_spec.rb

it "returns the name with 2 tasks remaining" do
  todo_list = TodoList.new("My List", ["task 1", "task 2"])

  expect(todo_list.name).to eq("My List (2 tasks)")
end
~~~

Chạy lại toàn bộ bài kiểm thử trong terminal bằng `$ rspec` để thấy tất cả bài kiểm thử phương thức name của todo list đều đạt.

#### Matcher Be

Chúng ta thường cần kiểm thử các phương thức predicate trong lớp, vốn trả về true hoặc false. Chúng ta có thể dùng matcher `eq` để kiểm thử các phương thức này.

Nhưng RSpec có matcher `be` giàu tính biểu đạt hơn, phù hợp với công việc này và giúp bài kiểm thử dễ đọc hơn nhiều: bạn có thể kỳ vọng kết quả *là true* hoặc *là false*, thay vì bằng true hoặc bằng false, vốn không tự nhiên bằng.

Để minh họa matcher `be`, chúng ta sẽ viết bài kiểm thử cho một phương thức khác của `TodoList`, cụ thể là phương thức `completed?`.

Trước hết, chúng ta cần một example group để nhóm các bài kiểm thử cho phương thức `completed?`.

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  # omitted for brevity

  describe "completed?" do

  end
end
~~~

Tiếp theo, chúng ta sẽ viết các trường hợp kiểm thử riêng lẻ cho phương thức `completed?`. Chỉ có hai trường hợp cần quan tâm.

Phương thức trả về gì khi danh sách đã hoàn thành và trả về gì khi danh sách chưa hoàn thành:

~~~ruby
# spec/todo_list_spec.rb

describe "completed?" do
  it "returns true when all tasks are completed" do
  end

  it "returns false when all tasks are not completed" do
  end
end
~~~

Bây giờ hãy xem cách dùng matcher `be` mà chúng ta vừa giới thiệu bằng cách viết expectation cho trường hợp đầu tiên:

~~~ruby
# spec/todo_list_spec.rb

it "returns true when all tasks are completed" do
  todo_list = TodoList.new("My List", [])

  expect(todo_list.completed?).to be(true)
end
~~~

Để hoàn thành các bài kiểm thử cho phương thức `completed?`, chúng ta sẽ viết bài kiểm thử khi todo list chưa hoàn thành:

~~~ruby
# spec/todo_list_spec.rb

it "returns false when all tasks are not completed" do
  todo_list = TodoList.new("My List", ["Incomplete task"])

  expect(todo_list.completed?).to be(false)
end
~~~

Cuối cùng, chạy lại toàn bộ các bài kiểm thử bằng `$ rspec`; mọi thứ vẫn phải đạt.

#### Matcher Include

Matcher include hoạt động tương tự phương thức `include?` mà bạn đã dùng với các collection của Ruby. Nó đạt expectation nếu giá trị thực tế *bao gồm* giá trị kỳ vọng.

Để xem matcher include hoạt động ra sao, chúng ta sẽ viết một số bài kiểm thử cho một phương thức khác của todo list, lần này là phương thức `add_task`.

Viết bài kiểm thử sau và bảo đảm nó hoạt động bằng cách chạy `$ rspec` trong terminal.

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  # omitted for brevity

  describe "add_task" do
    it "adds a task to the list" do
      todo_list = TodoList.new("My List", ["Existing task"])

      todo_list.add_task("New task")

      expect(todo_list.tasks).to include("New task")
    end
  end
end
~~~

Trong bài kiểm thử này, chúng ta thêm một task mới vào todo list vốn đã chứa một task hiện có. Tuy nhiên, chúng ta không quan tâm đến task hiện có. Điều duy nhất cần quan tâm là danh sách *bao gồm* task mới được thêm.

Matcher `include` rất phù hợp với những tình huống như vậy, khi chúng ta chỉ quan tâm đến một phần nhất định của giá trị thực tế.

Có lẽ bạn đã nhận thấy các bài kiểm thử cho phương thức `add_task` trông khá khác so với các bài kiểm thử trước đó. Đây là dấu hiệu tốt để chúng ta xem kỹ hơn các phần khác nhau của một bài kiểm thử riêng lẻ.

### Giải phẫu một bài kiểm thử

Mỗi bài kiểm thử thường gồm ba giai đoạn riêng biệt:

1. **Arrange**
2. **Act**
3. **Assert**

Hãy viết một bài kiểm thử cho phương thức `complete_task` để xem xét chi tiết hơn từng giai đoạn:

~~~ruby
# spec/todo_list_spec.rb

require "spec_helper"
require "todo_list"

RSpec.describe TodoList do

  # omitted for brevity

  describe "complete_task" do
    it "marks a task as completed" do
      # arrange
      todo_list = TodoList.new("My List", ["Incomplete Task"])

      # act
      todo_list.complete_task("Incomplete Task")

      # assert
      expect(todo_list.tasks).not_to include("Incomplete Task")
    end
  end
end
~~~

1. **Arrange** - Trước tiên, chúng ta cần sắp xếp các điều kiện phù hợp trong example để có thể kiểm thử đúng hành vi. Ở giai đoạn arrange của bài kiểm thử `complete_task`, chúng ta khởi tạo một todo list mới với một task chưa hoàn thành, để có thể kiểm thử hành vi hoàn thành trên task này. Giai đoạn này còn thường được gọi là giai đoạn setup.

2. **Act** - Ở giai đoạn thứ hai, chúng ta *hành động*, hay nói cách khác là gọi hành vi đang kiểm thử. Trong giai đoạn act của bài kiểm thử, chúng ta gọi phương thức `complete_task`, truyền vào tên task đã tạo ở giai đoạn arrange để hoàn thành nó.

3. **Assert** - Ở giai đoạn cuối, chúng ta xác nhận hành vi đã làm đúng điều mong đợi. Trong giai đoạn assert, chúng ta đặt expectation rằng todo list không còn bao gồm task vừa được hoàn thành.

#### Phong cách

Một số bài kiểm thử trước đó gộp giai đoạn act và assert. Ví dụ như các bài kiểm thử phương thức `completed?`, trong đó chúng ta vừa act vừa assert ngay trong expectation:

~~~ruby
# spec/todo_list_spec.rb

it "returns true when all tasks are completed" do
  # arrange
  todo_list = TodoList.new("My List", [])

  # act and assert
  expect(todo_list.completed?).to be(true)
end
~~~

Có thể viết lại bài kiểm thử này để các giai đoạn rõ ràng hơn:

~~~ruby
# spec/todo_list_spec.rb

it "returns true when all tasks are completed" do
  # arrange
  todo_list = TodoList.new("My List", [])

  # act
  result = todo_list.completed?

  # assert
  expect(result).to be(true)
end
~~~

Bạn dùng phiên bản nào là tùy bạn. Nhiều người thấy cú pháp ngắn gọn hơn của phiên bản đầu dễ đọc hơn. Một thông lệ phổ biến khác là tách mỗi giai đoạn bằng một dòng trống để dễ nhận ra chúng ngay lập tức.

#### Giai đoạn teardown

Đôi khi cần thêm một giai đoạn để hoàn tác điều đã thực hiện trong giai đoạn arrange hoặc act nhằm thay đổi trạng thái toàn cục trong hệ thống. Giai đoạn này được gọi là teardown.

Để xem giai đoạn này hoạt động ra sao, hãy tạo một tệp mới trong thư mục lib có tên `counter.rb` và dán đoạn mã sau vào đó:

~~~ruby
# lib/counter.rb

$global_counter = 0

class Counter

  def count
    $global_counter
  end

  def increment!
    $global_counter += 1
  end

  def reset!
    $global_counter = 0
  end

end
~~~

Tiếp theo, tạo một tệp kiểm thử mới cho lớp `Counter` trong thư mục spec, đặt tên là `counter_spec.rb`, rồi nhập đoạn mã kiểm thử sau:

~~~ruby
# spec/counter_spec.rb

require "spec_helper"
require "counter"

RSpec.describe Counter do

  describe "increment!" do
    it "increments the counter by one" do
      counter = Counter.new

      counter.increment!

      expect(counter.count).to eq(1)
    end

    it "increments the counter by two" do
      counter = Counter.new

      counter.increment!
      counter.increment!

      expect(counter.count).to eq(2)
    end
  end

end
~~~

Trong tệp kiểm thử, chúng ta có hai bài kiểm thử cho phương thức `increment!`: một bài xác nhận bộ đếm tăng lên một, và bài kia xác nhận bộ đếm tăng lên 2.

Nếu chạy các bài kiểm thử ngay lúc này, chúng ta sẽ thấy một trong hai lỗi, tùy vào thứ tự thực thi. Hãy nhớ rằng theo mặc định RSpec chạy các bài kiểm thử theo thứ tự ngẫu nhiên.

Nếu bài kiểm thử tăng một được chạy trước, bài kiểm thử tăng hai sẽ thất bại vì lý do sau:

~~~ruby
1) Counter increment! increments the counter by two
     Failure/Error: expect(counter.count).to eq(2)

       expected: 2
            got: 3
~~~

Nếu bài kiểm thử tăng hai được chạy trước, bài kiểm thử tăng một sẽ thất bại vì lý do này:

~~~ruby
1) Counter increment! increments the counter by one
     Failure/Error: expect(counter.count).to eq(1)

       expected: 1
            got: 3
~~~

Nếu chạy các bài kiểm thử vài lần, cuối cùng chúng ta sẽ thấy cả hai lỗi này khi thứ tự ngẫu nhiên thay đổi.

Vấn đề là cả hai bài kiểm thử đều kỳ vọng bộ đếm bắt đầu từ 0. Nhưng cả hai cũng đang thay đổi trạng thái hệ thống bằng cách tăng `$global_count`.

Các thay đổi trạng thái này cứ tồn tại và *rò rỉ* giữa hai bài kiểm thử. Điều đó nghĩa là bộ đếm toàn cục chỉ bằng 0 ở bài kiểm thử được thực thi đầu tiên.

Đây là lúc giai đoạn teardown phát huy tác dụng: chúng ta cần dọn dẹp sau mỗi bài kiểm thử bằng cách đặt bộ đếm toàn cục về 0 sau khi bài kiểm thử thực thi xong. Như vậy, các thay đổi trạng thái do một bài kiểm thử tạo ra sẽ không ảnh hưởng đến bài còn lại.

May mắn là lớp counter có phương thức `reset!` tiện dụng để làm việc này. Thêm các giai đoạn teardown sau vào các bài kiểm thử counter:

~~~ruby
# spec/counter_spec.rb

require "spec_helper"
require "counter"

RSpec.describe Counter do

  describe "increment!" do
    it "increments the counter by one" do
      # arrange
      counter = Counter.new

      # act
      counter.increment!

      # assert
      expect(counter.count).to eq(1)

      # teardown
      counter.reset!
    end

    it "increments the counter by two" do
      # arrange
      counter = Counter.new

      # act
      counter.increment!
      counter.increment!

      # assert
      expect(counter.count).to eq(2)

      # teardown
      counter.reset!
    end
  end

end
~~~

Giờ khi chạy bộ kiểm thử, cả hai bài này sẽ luôn đạt, bất kể chúng được thực thi theo thứ tự nào.

Lớp counter này là một ví dụ được dựng lên để minh họa giai đoạn teardown. Trong thực tế, bạn sẽ không phải dùng giai đoạn teardown quá thường xuyên vì thông thường sẽ không làm điều như vậy với biến toàn cục trong một hệ thống thực.

Tuy nhiên, về sau trong chương trình học, bạn sẽ làm việc với những thứ lưu trữ dữ liệu như database và hệ thống tệp. Giai đoạn teardown sẽ hữu ích khi bạn cần dọn dẹp dữ liệu đã lưu giữa các bài kiểm thử.

### Kết luận

Chúng ta đã đi qua khá nhiều nội dung; giờ bạn đã biết tất cả khối xây dựng cơ bản để viết bài kiểm thử bằng RSpec. Trong các bài học sắp tới, chúng ta sẽ tìm hiểu nên kiểm thử những gì.

Nhưng trước đó, ở bài học tiếp theo, chúng ta sẽ khám phá thêm một vài tính năng RSpec cho phép chia sẻ mã giữa các bài kiểm thử.

Một lần nữa, chúng ta sẽ dùng dự án `odin_rspec` đã thiết lập trong bài học này để minh họa các tính năng đó, vì vậy hãy giữ lại dự án.

### Bài tập
<div class="lesson-content__panel" markdown="1">
1. TODO: Bài tập về matcher trong kho mã kiểm thử
</div>

### Tài nguyên bổ sung
Phần này chứa các liên kết hữu ích đến nội dung khác. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [https://www.rubyguides.com/2018/09/ruby-gems-gemfiles-bundler/](https://www.rubyguides.com/2018/09/ruby-gems-gemfiles-bundler/)
