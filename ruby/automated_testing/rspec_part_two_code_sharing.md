### Giới thiệu

Trong bài học này, chúng ta sẽ khám phá một số công cụ RSpec cung cấp để chia sẻ mã giữa nhiều bài kiểm thử. Cụ thể, chúng ta sẽ tìm hiểu các phương thức helper hook, let và subject. Những phương thức này thường xuyên được dùng trong các bộ kiểm thử RSpec, vì vậy điều quan trọng là biết chúng hoạt động thế nào và khi nào nên dùng.

Một lần nữa, chúng ta sẽ làm việc trên dự án `odin_rspec` đã thiết lập ở bài học trước. Để tận dụng bài học tốt nhất, hãy làm theo tất cả các ví dụ.

### Mục tiêu học tập

- Before hook là gì và nên dùng nó cho giai đoạn nào của bài kiểm thử?
- After hook là gì và nên dùng nó cho giai đoạn nào của bài kiểm thử?
- Biến let dùng để làm gì?
- Subject dùng để làm gì?

### Hook

Chúng ta thường có cùng mã arrange, act và teardown trong nhiều trường hợp kiểm thử. RSpec cung cấp cách chia sẻ mã này bằng các phương thức hook, cho phép chạy mã trước và sau các bài kiểm thử.

#### Before hook

Before hook chạy **trước** khi các bài kiểm thử được thực thi. Chúng ta có thể đặt bất kỳ mã nào vào đó, nhưng cách dùng phù hợp nhất là chia sẻ cùng giai đoạn arrange và, trong một số trường hợp, cùng mã ở giai đoạn act giữa nhiều bài kiểm thử.

Để xem cách này hoạt động, hãy tạo một tệp mới trong thư mục lib của dự án `odin_rspec`, đặt tên là `user.rb`, rồi dán đoạn mã sau vào đó:

~~~ruby
# lib/user.rb

class User
  attr_reader :name, :email, :age

  def initialize(name, email, age)
    @name = name
    @email = email
    @age = age
  end

  def retired?
    age >= 65
  end
end
~~~

Tiếp theo, tạo tệp kiểm thử cho lớp user trong thư mục spec, đặt tên là `user_spec.rb`, rồi viết các bài kiểm thử sau cho lớp `User` để kiểm tra các thuộc tính name, email và age:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do

  describe "name" do
    it "returns the users name" do
      user = User.new("David", "david@example.com", 30)

      expect(user.name).to eq("David")
    end
  end

  describe "email" do
    it "returns the users email" do
      user = User.new("David", "david@example.com", 30)

      expect(user.email).to eq("david@example.com")
    end
  end

  describe "age" do
    it "returns the users age" do
      user = User.new("David", "david@example.com", 30)

      expect(user.age).to eq(30)
    end
  end

end
~~~

Chạy các bài kiểm thử để bảo đảm mọi thứ đã được kết nối đúng. Chúng ta có thể chỉ chạy các bài kiểm thử cho lớp user bằng cách truyền tên tệp kiểm thử cho lệnh `rspec`: `$rspec spec/user_spec.rb` .

Xem lại tệp kiểm thử: chúng ta cần tạo một đối tượng user mới với cùng các thuộc tính trong giai đoạn arrange của tất cả bài kiểm thử. Điều này tạo ra khá nhiều phần trùng lặp giữa các bài kiểm thử.

Để giảm phần trùng lặp này, thay vào đó chúng ta có thể dùng before hook để thực thi mã của giai đoạn arrange và làm cho các bài kiểm thử ngắn gọn hơn nhiều.

Trong trường hợp này, chúng ta có thể thêm một before hook để tạo một instance `User` mới và gán instance đó vào biến instance `@user`, rồi dùng biến này trong từng bài kiểm thử:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do

  before do
    @user = User.new("David", "david@example.com", 30)
  end

  describe "name" do
    it "returns the users name" do
      expect(@user.name).to eq("David")
    end
  end

  describe "email" do
    it "returns the users email" do
      expect(@user.email).to eq("david@example.com")
    end
  end

  describe "age" do
    it "returns the users age" do
      expect(@user.age).to eq(30)
    end
  end

end
~~~

Theo mặc định, before hook chạy *trước* mỗi bài kiểm thử trong cùng một example group. Chúng ta có thể thấy rõ hơn bằng cách thêm một câu lệnh puts vào before hook:

~~~ruby
# spec/user_spec.rb

before do
  puts "running the before hook"
  @user = User.new("David", "david@example.com", 30)
end
~~~

Giờ khi chạy lại các bài kiểm thử, chúng ta sẽ thấy dòng "running the before hook" được in ra màn hình ba lần, một lần cho mỗi bài kiểm thử.

Ngoài ra, chúng ta có thể truyền đối số `:all` cho phương thức before. Khi đó before hook chỉ chạy một lần trước toàn bộ các bài kiểm thử.

Thay before hook bằng đoạn sau rồi chạy các bài kiểm thử. Câu lệnh puts chỉ in một lần trong terminal, chứng minh block before chỉ được thực thi một lần.

~~~ruby
# spec/user_spec.rb

before(:all) do
  puts "running the before hook"
  @user = User.new("David", "david@example.com", 30)
end
~~~

Tuy nhiên, một nguyên tắc thực tế tốt là tránh dùng đối số `:all` và giữ hành vi mặc định của before hook, tức chạy trước từng bài kiểm thử riêng lẻ.

Khi before hook chạy trước mỗi bài kiểm thử, nó bảo đảm mỗi bài bắt đầu với một đối tượng mới để làm việc, đồng thời các thay đổi trạng thái trên đối tượng sẽ không rò rỉ sang bài kiểm thử khác, vốn có thể gây ra những lỗi khó gỡ.

#### After hook

After hook chạy sau khi các bài kiểm thử đã thực thi. Cách dùng phù hợp nhất của chúng là cho giai đoạn teardown, để chúng ta có thể dọn dẹp **sau** các bài kiểm thử và đưa trạng thái hệ thống trở lại như trước khi bài kiểm thử chạy.

Dùng ví dụ counter mà chúng ta đã dùng để minh họa giai đoạn teardown trong bài trước, chúng ta có thể tái cấu trúc các bài kiểm thử counter để dùng after hook cho giai đoạn teardown và đặt lại bộ đếm giữa các bài kiểm thử.

Chúng ta cũng sẽ chuyển mã tạo counter của giai đoạn arrange vào before hook và gán nó cho một biến instance để minh họa cách hai hook phối hợp:

~~~ruby

# spec/counter_spec.rb

require "spec_helper"
require "counter"

RSpec.describe Counter do

  describe "increment!" do

    before do
      @counter = Counter.new
    end

    after do
      @counter.reset!
    end

    it "increments the counter by one" do
      @counter.increment!

      expect(@counter.count).to eq(1)
    end

    it "increments the counter by two" do
      @counter.increment!
      @counter.increment!

      expect(@counter.count).to eq(2)
    end
  end

end
~~~

Chạy các bài kiểm thử counter bằng `$ rspec spec/counter_spec.rb` để bảo đảm mọi thứ vẫn hoạt động.

### Biến Let và Subject

Việc dùng biến instance trong before hook như chúng ta vừa làm có một số nhược điểm lớn, có thể khiến bộ kiểm thử chậm hơn và kém ổn định hơn.

- Before hook chạy cho mọi bài kiểm thử trong cùng một example group. Một số bài có thể không cần biến instance đó.
- Biến instance trả về nil nếu chưa được gán; rất dễ và cũng thường gặp lỗi viết sai tên biến instance, rồi nhận lỗi no method for nil class trong Ruby. Có lẽ bạn đã gặp lỗi này vài lần khi học phần OOP.
- Biến instance có thể rò rỉ giữa các bài kiểm thử, thậm chí giữa các tệp kiểm thử. Điều này có khả năng gây đủ loại rắc rối trong bộ kiểm thử.

May mắn là RSpec cung cấp một cách tạo biến mang tính idiomatic và an toàn hơn để dùng trong nhiều bài kiểm thử: phương thức `let`.

Quay lại ví dụ user dùng để minh họa before hook ở trên. Thay vì dùng biến instance, chúng ta có thể dùng biến `let`. Xóa block before và tạo một biến let ngay bên dưới example group cấp cao nhất. Đồng thời đổi tên các biến instance trong bài kiểm thử từ `@user` thành chỉ `user`:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  let(:user) { User.new("David", "david@example.com", 30) }

  describe "name" do
    it "returns the users name" do
      expect(user.name).to eq("David")
    end
  end

  describe "email" do
    it "returns the users email" do
      expect(user.email).to eq("david@example.com")
    end
  end

  describe "age" do
    it "returns the users age" do
      expect(user.age).to eq(30)
    end
  end

end
~~~

Bảo đảm mọi thứ vẫn đạt bằng cách chạy các bài kiểm thử user với `$ rspec spec/user_spec.rb`.

Phương thức let nhận một đối số symbol, chính là tên biến và cũng là cách chúng ta gọi biến đó trong các trường hợp kiểm thử. Trong mã của chúng ta, tên biến let là `user`.

Nó cũng nhận một block; kết quả của block sẽ được trả về khi chúng ta gọi biến let. Trong mã này, đó là một instance của lớp `User`.

Phương thức let được đánh giá lười, nghĩa là nó không thực thi cho đến khi được gọi trong bài kiểm thử. Có một phiên bản khác của phương thức let: `let!`. Phiên bản này thực thi ngay trước các bài kiểm thử, giống như before hook.

Tuy nhiên, phiên bản biến let này sẽ hữu ích hơn về sau trong khóa học, khi chúng ta viết bài kiểm thử cho ứng dụng Rails và cần tạo các bản ghi database trước bài kiểm thử rồi gán chúng vào một biến để dùng sau.

#### Subject

Như bạn đã thấy nhiều lần trong hướng dẫn này, chúng ta cần khởi tạo instance mới của lớp đang kiểm thử và gán nó cho một biến trong giai đoạn arrange của hầu hết các bài kiểm thử.

Vì đây là một pattern rất phổ biến, RSpec cung cấp một phương thức chuyên biệt hơn chỉ để tạo một instance có thể dùng trong tất cả bài kiểm thử. Đó là phương thức helper `subject`.

Tái cấu trúc ví dụ user trước đó để dùng `subject` thay cho biến `let` cực kỳ đơn giản. Chúng ta chỉ cần thay lời gọi phương thức `let` bằng `subject`:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  subject(:user) { User.new("David", "david@example.com", 30) }

  # omitted for brevity...
end
~~~

Nếu chạy các bài kiểm thử sau khi thay đổi, tất cả vẫn phải đạt.

Subject và biến let có cú pháp rất giống nhau. Cả hai đều nhận đối số tên dưới dạng symbol và đều nhận một block. Về chức năng, chúng gần như hoạt động giống nhau.

Tuy nhiên, có một số khác biệt tinh tế. Đối số tên là tùy chọn; chúng ta có thể bỏ qua nó và dùng *subject không tên*. Nhưng khi đó phải tham chiếu subject bằng lời gọi phương thức `subject`.

Dùng subject có tên khiến bài kiểm thử dễ đọc hơn nhiều và được xem là cách tốt nhất để dùng subject. Sự khác nhau giữa hai cách này được thể hiện dưới đây:

~~~ruby
# Using an unnamed subject
describe "age" do
  subject { User.new("David", "david@example.com", 30) }

  it "returns the users age" do
    expect(subject.age).to eq(30)
  end
end

# Using a named subject
describe "age" do
  subject(:user) { User.new("David", "david@example.com", 30) }

  it "returns the users age" do
    expect(user.age).to eq(30)
  end
end
~~~

Khác biệt chính giữa subject và let nằm ở ý nghĩa của chúng đối với người đọc.

Subject được dùng cho *subject* của tệp kiểm thử, hay nói cách khác là instance của đối tượng đang được kiểm thử. Dùng phương thức `subject` giúp người đọc dễ nhận ra instance được tạo ở đâu.

Ngược lại, biến let có thể dùng cho mọi dữ liệu chung cần được chia sẻ.

#### Vậy khi nào nên dùng let?

Quy tắc ở đây là dùng biến `let` cho mọi thứ ngoài việc tạo đối tượng đang kiểm thử, vì việc đó nên dành cho `subject`.

Một trường hợp thường gặp là dùng biến let cho các đối số truyền vào instance được tạo trong subject.

Quay lại các bài kiểm thử user, hãy tách tất cả đối số chúng ta truyền vào [`User.new`](http://user.new) trong subject thành các biến let:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  subject(:user) { User.new(name, email, age) }

  let(:name) { "David" }
  let(:email) { "david@example.com" }
  let(:age) { 30 }

	# omitted for brevity...

end
~~~

Chạy các bài kiểm thử user bằng `$ rspec spec/user_spec.rb`; tất cả vẫn phải đạt.

Việc này có thể trông như một refactor không cần thiết vì trước đó mã đã hoạt động tốt và thay đổi chỉ có vẻ thêm nhiều dòng hơn. Tuy nhiên, dùng các biến let để đặt tên cho những đối số truyền vào [`User.new`](http://user.new) giúp làm rõ các đối số đại diện cho điều gì và cải thiện khả năng đọc của bài kiểm thử.

Nhưng lý do thú vị hơn nhiều để thực hiện refactor này là nó cho phép chúng ta tạo các instance user với những đối số khác nhau, từ đó kiểm thử nhiều kết quả khác nhau mà các đối số đó tạo ra.

Ví dụ là cách tốt nhất để hiểu rõ điều này. Chúng ta sẽ viết bài kiểm thử cho phương thức `retired?` trước đây chưa được kiểm thử trên lớp user. Nhắc lại, phương thức đó có dạng:

~~~ruby
# lib/user.rb

def retired?
  age >= 65
end
~~~

Có 3 nhánh khả dĩ cần bao phủ bằng các trường hợp kiểm thử:

1. Khi tuổi của user nhỏ hơn 65
2. Khi tuổi của user bằng 65
3. Khi tuổi của user lớn hơn 65

Để viết các trường hợp kiểm thử này, chúng ta sẽ giới thiệu một cú pháp RSpec mới: phương thức `context`:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  subject(:user) { User.new(name, email, age) }

  let(:name) { "David" }
  let(:email) { "david@example.com" }
  let(:age) { 30 }

  # omitted for brevity...

  describe "retired?" do
    context "when the user is 65" do
      it "returns true" do
      end
    end

    context "when the user is older than 65" do
      it "returns true" do
      end
    end

    context "when the user is younger than 65" do
      it "returns false" do
      end
    end
  end

end
~~~

Phương thức `context` chỉ là bí danh của phương thức `describe` mà chúng ta đã khá quen thuộc. Nghĩa là hai phương thức hoạt động giống hệt nhau và có thể dùng thay thế cho nhau. Tuy nhiên, chúng khác nhau ở ý nghĩa đối với người đọc.

`describe` dùng để biểu diễn example group cho những *thứ* như lớp và phương thức. Còn context mô tả và biểu diễn các example group xoay quanh những trạng thái khác nhau, chẳng hạn trạng thái tuổi trong lớp user: khi user 65 tuổi, khi user lớn hơn 65 tuổi, v.v.

Nói cách khác, nó cung cấp *ngữ cảnh* về một số điều kiện trong bài kiểm thử, để chúng ta kiểm thử điều gì xảy ra khi các điều kiện đó được đặt ra. Quy tắc dùng context là khi muốn kiểm thử kết quả mà phương thức tạo ra trong những điều kiện khác nhau.

Điền các expectation cho từng trường hợp kiểm thử:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  subject(:user) { User.new(name, email, age) }

  let(:name) { "David" }
  let(:email) { "david@example.com" }
  let(:age) { 30 }

  # omitted for brevity...

  describe "retired?" do
    context "when the user is 65" do
      it "returns true" do
        expect(user.retired?).to be(true)
      end
    end

    context "when the user is older than 65" do
      it "returns true" do
        expect(user.retired?).to be(true)
      end
    end

    context "when the user is younger than 65" do
      it "returns false" do
        expect(user.retired?).to be(false)
      end
    end
  end

end
~~~

Nếu chạy các bài kiểm thử bằng `$ rspec spec/user_spec.rb`, chúng ta sẽ nhận được hai lỗi:

~~~bash
Failures:

  1) User retired? when the user is 65 returns true
     Failure/Error: expect(user.retired?).to be(true)

       expected true
            got false
     # ./spec/user_spec.rb:32:in `block (4 levels) in <top (required)>'

  2) User retired? when the user is older than 65 returns true
     Failure/Error: expect(user.retired?).to be(true)

       expected true
            got false
     # ./spec/user_spec.rb:38:in `block (4 levels) in <top (required)>'
~~~

Lý do là chúng ta đang mặc định tuổi của user là 30 trong biến let age được định nghĩa ở example group cấp cao nhất `let(:age) { 30 }` . Vì vậy, hiện tại chỉ trường hợp user trẻ hơn 65 là đạt.

Tuy nhiên, chúng ta có thể ghi đè biến let age trong các context để tạo đúng điều kiện, giúp các bài kiểm thử đang thất bại đạt:

~~~ruby
# spec/user_spec.rb

require "spec_helper"
require "user"

RSpec.describe User do
  subject(:user) { User.new(name, email, age) }

  let(:name) { "David" }
  let(:email) { "david@example.com" }
  let(:age) { 30 }

  # omitted for brevity...

  describe "retired?" do
    context "when the user is 65" do
      let(:age) { 65 }

      it "returns true" do
        expect(user.retired?).to be(true)
      end
    end

    context "when the user is older than 65" do
      let(:age) { 66 }

      it "returns true" do
        expect(user.retired?).to be(true)
      end
    end

    context "when the user is younger than 65" do
      it "returns false" do
        expect(user.retired?).to be(false)
      end
    end
  end

end
~~~

Nếu chạy các bài kiểm thử lúc này, mọi thứ sẽ đạt 🎉

Cơ chế hoạt động đơn giản hơn vẻ ngoài. Nếu có nhiều biến let cùng tên trong tệp kiểm thử, trường hợp kiểm thử sẽ dùng biến let được định nghĩa gần nó nhất.

Trong mã của chúng ta, có một biến let age ở example group cấp cao nhất và các biến let khác cũng tên age trong những example group context.

Ví dụ, khi trường hợp kiểm thử user 65 tuổi được thực thi, nó dùng biến let gần nhất. Nói cách khác, nó dùng biến let được định nghĩa trong cùng example group: `let(:age) { 65 }`.

### Kết luận

Hook, let và subject cực kỳ mạnh mẽ. Chúng có thể giúp tạo ra các bài kiểm thử có cấu trúc tốt và dễ hiểu hơn nhiều bằng cách giảm sự trùng lặp và phần nhiễu không cần thiết trong bộ kiểm thử.

Tuy nhiên, rất dễ lạm dụng chúng và tạo ra các tệp kiểm thử buộc người đọc phải liên tục chuyển qua lại giữa hook, let và các bài kiểm thử thực tế để có được toàn cảnh về những gì bài kiểm thử đang làm. Khi đó việc đọc và suy luận về từng trường hợp kiểm thử trở nên phiền phức.

Các công cụ chia sẻ mã này chỉ nên được dùng khi chúng cải thiện khả năng đọc và sự rõ ràng tổng thể của bài kiểm thử. Phần lớn thời gian, để giữ cho mọi thứ rõ ràng, tốt nhất nên đặt mã arrange và teardown ngay trong block kiểm thử sử dụng nó, tương tự cách chúng ta làm với ví dụ todo list ban đầu.

### Bài tập
<div class="lesson-content__panel" markdown="1">
1. TODO: Bài tập về hook trong kho mã kiểm thử
1. TODO: Bài tập về subject trong kho mã kiểm thử
1. TODO: Bài tập về let trong kho mã kiểm thử
</div>

### Tài nguyên bổ sung
Phần này chứa các liên kết hữu ích đến nội dung khác. Đây không phải phần bắt buộc, nên bạn có thể xem như tài liệu bổ sung.

- [https://mixandgo.com/learn/let-vs-instance-variables-in-rspec](https://mixandgo.com/learn/let-vs-instance-variables-in-rspec)
