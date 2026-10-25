### Giới thiệu

Trong bài học trước, chúng ta đã xác lập tính hữu ích của [phát triển hướng kiểm thử](https://en.wikipedia.org/wiki/Test-driven_development) (TDD) trong việc duy trì mã nguồn và sự minh mẫn của bạn. Trong bài học này, chúng tôi sẽ giới thiệu người bạn thân mới của bạn, framework kiểm thử [RSpec](http://rspec.info/). Đây là một trong những framework kiểm thử phổ biến nhất, đã được tải xuống hơn 700 triệu lần tại thời điểm bài viết này được thực hiện, và đã được chuyển sang sử dụng trong [kiểm thử Rails](https://rubygems.org/gems/rspec-rails).

### Tổng quan bài học

Phần này trình bày tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Biết RSpec là gì.
- Biết cách cài đặt RSpec.
- Hiểu cú pháp RSpec cơ bản: `describe` và `it`.

### Giới thiệu về RSpec

#### RSpec là gì và vì sao lại là RSpec?

Ở mức cơ bản nhất, RSpec là một Ngôn ngữ dành riêng cho miền được viết bằng Ruby, hay nói cách khác, là một ngôn ngữ được chuyên biệt hóa cho một nhiệm vụ cụ thể. Trong trường hợp này, nhiệm vụ đó là kiểm thử mã Ruby. Gem `rspec` được đóng gói cùng mọi thứ bạn cần để bắt đầu, bao gồm năm gem: `rspec`, `rspec-core`, `rspec-expectations`, `rspec-mocks` và `rspec-support`.

Đến đây, có thể bạn đang tự hỏi, *Tại sao lại là RSpec? Chắc hẳn ngoài kia còn có những framework khác*, và bạn nói đúng. Quả thực là có. Trên thực tế, đã có lúc Ruby được tích hợp [Test::Unit](https://github.com/test-unit/test-unit), rồi sau đó là [Minitest](https://github.com/seattlerb/minitest), như một phần của thư viện chuẩn; Minitest vẫn còn tồn tại trong Rails. Nếu bạn có xu hướng bi quan (Xin lỗi, ý chúng tôi là *thực tế*), thì framework kiểm thử [Wrong](https://github.com/sconover/wrong) có thể hợp gu với bạn. Hoặc có lẽ bạn đang đói và muốn một món gì đó đầy đặn hơn; khi ấy, một phần [Bacon](https://github.com/chneukirchen/bacon) ăn kèm có thể là thứ bạn cần. Suy cho cùng, việc bạn chọn framework nào không quan trọng, miễn là bạn rèn luyện kỹ năng kiểm thử. Việc RSpec được sử dụng rộng rãi hơn, đặc biệt trong cộng đồng Rails, chắc chắn là đủ lý do để bạn làm quen với nó, nhưng việc triển khai các bài kiểm thử mới là mục tiêu tối thượng, chứ không phải lựa chọn một framework cụ thể.

Nhưng nói về việc [truyền đạo](https://www.dictionary.com/browse/proselytizing) đủ rồi. Đội mũ bảo hiểm và thắt dây an toàn đi; chúng ta sẽ bắt đầu ngay.

#### Cài đặt RSpec

Mở terminal và nhập `gem install rspec` để cài đặt RSpec. Sau khi hoàn tất, bạn có thể kiểm tra phiên bản RSpec bằng `rspec --version`, lệnh này sẽ xuất ra phiên bản hiện tại của từng gem được đóng gói. Ngoài ra, hãy dành một chút thời gian chạy `rspec --help` và xem qua các tùy chọn khác nhau hiện có.

Cuối cùng, `cd` vào thư mục dự án mà bạn muốn cấu hình để sử dụng với RSpec rồi nhập `rspec --init` nhằm khởi tạo RSpec bên trong dự án. Lệnh này sẽ tạo hai tệp, `.rspec` và `spec/spec_helper.rb`, để dự án của bạn có thể trông như sau:

```bash
project
  |__lib
  |   |__calculator.rb
  |
  |__spec
  |   |__spec_helper.rb
  |
  |__.rspec
```

Vậy là xong. Chỉ với hai bước, bạn đã có thể bắt đầu chạy RSpec. Cũng không khó lắm, phải không?

#### Cú pháp cơ bản

Thử xem cú pháp bằng một bài kiểm thử nhé? Hãy tạo một "dự án" hoàn toàn mới để bắt đầu. Tạo một thư mục mới có tên `ruby_testing`, chuyển vào đó và khởi tạo RSpec.

```bash
mkdir ruby_testing && cd ruby_testing
rspec --init
```

Đúng như dự kiến, kết quả sẽ là:

```bash
  create   .rspec
  create   spec/spec_helper.rb
```

Chạy các bài kiểm thử từ terminal bằng lệnh `rspec`, lệnh này sẽ trả về "No examples found." Điều đó thực sự không nên khiến bạn ngạc nhiên, vì chúng ta vẫn chưa viết bài kiểm thử nào. Nếu bạn vẫn còn sốc... có lẽ hãy nghỉ giải lao một chút, hoặc ghé chào chúng tôi trong [máy chủ Discord của chúng tôi](https://discord.gg/fbFCkYabZB)!

```bash
No examples found.


Finished in 0.00037 seconds (files took 0.21108 seconds to load)
0 examples, 0 failures
```

Hãy thêm bài kiểm thử đầu tiên. Giả sử chúng ta muốn tạo một máy tính có vài phương thức cần kiểm thử. Đúng với tinh thần TDD, chúng ta sẽ viết các bài kiểm thử trước khi viết mã. Thư mục `spec/` là nơi chứa toàn bộ bài kiểm thử của bạn. Dùng `touch` trên dòng lệnh hoặc trình soạn thảo văn bản, tạo `calculator_spec.rb` bên trong thư mục `spec/` và thêm các dòng sau:

```ruby
#spec/calculator_spec.rb

RSpec.describe Calculator do
  describe "#add" do
    it "returns the sum of two numbers" do
      calculator = Calculator.new
      expect(calculator.add(5, 2)).to eql(7)
    end
  end
end
```

Hãy đi qua từng dòng.

Trước hết, `RSpec.describe` là một từ khóa RSpec dùng để định nghĩa một "Nhóm ví dụ", hay một tập hợp các bài kiểm thử. Nó nhận một lớp hoặc một chuỗi làm đối số và được truyền vào một block (`do/end`). Các block `describe` có thể được lồng bên trong `RSpec.describe`, như ở dòng thứ hai của bài kiểm thử trên.

Từ khóa `it` định nghĩa một ví dụ riêng lẻ (hay còn gọi là bài kiểm thử). `it` nhận một đối số chuỗi và cũng được truyền vào một block. Đây là nơi thể hiện những kỳ vọng của chúng ta đối với một phương thức. Trong trường hợp cụ thể này, khi truyền 5 và 2 vào phương thức `#add`, chúng ta kỳ vọng nó trả về 7. Điều này được diễn đạt ngắn gọn trong mệnh đề kỳ vọng, sử dụng một trong các matcher so sánh bằng của [RSpec](http://rspec.info/features/3-12/rspec-expectations/built-in-matchers/equality/), `eql`:

```ruby
  expect(calculator.add(5, 2)).to eql(7)
```

Về cơ bản là vậy. Hãy xem lại một lần nữa, từ đầu:

1. `RSpec.describe` lớp.
1. `describe` nhóm ví dụ của phương thức. Theo quy ước, đối số chuỗi cho các phương thức của đối tượng được viết là `#method`, còn đối số chuỗi cho các phương thức của lớp được viết là `.method`.
1. Viết trường hợp ví dụ/bài kiểm thử bằng `it`.
1. Viết kỳ vọng bằng `expect`. Phương thức `expect` cũng được nối chuỗi với `.to` cho các kỳ vọng khẳng định, hoặc `.to_not`/`.not_to` cho các kỳ vọng phủ định. Chúng tôi ưu tiên `.not_to`. Ngoài ra, hãy giới hạn mỗi trường hợp kiểm thử chỉ có một mệnh đề expect.

#### Mã chạy thành công

Tiếp tục nào. Chạy `rspec` từ thư mục gốc và theo dõi kết quả.

```bash
An error occurred while loading ./spec/calculator_spec.rb.
Failure/Error:
  RSpec.describe Calculator do
    describe "#add" do
      it "returns the sum of two numbers" do
        calculator = Calculator.new
        expect(calculator.add(5, 2)).to eql(7)
      end
    end
  end

NameError:
  uninitialized constant Calculator
# ./spec/calculator_spec.rb:1:in `<top (required)>'
No examples found.


Finished in 0.0004 seconds (files took 0.16461 seconds to load)
0 examples, 0 failures, 1 error occurred outside of examples
```

Vậy bài kiểm thử đầu tiên của chúng ta đã trả về một lỗi. Điều này không có gì bất ngờ. `NameError` về cơ bản đang cho biết RSpec đã tìm một lớp `Calculator` nhưng không thể tìm thấy nó. Vì vậy, hãy tạo lớp đó. Từ thư mục gốc của dự án, tạo một thư mục `lib/`, rồi tạo `calculator.rb` bên trong cùng với lớp của bạn. Chúng ta cũng sẽ bắt đầu phương thức `#add`, nếu không RSpec sẽ đưa ra lỗi tương tự lỗi trước đó khi tìm phương thức này:

```ruby
#lib/calculator.rb

class Calculator
  def add(a,b)
  end
end
```

Cuối cùng, chúng ta cũng phải cho spec biết lớp `Calculator` được định nghĩa ở đâu. Việc này dễ dàng thực hiện bằng `require`:

```ruby
#spec/calculator_spec.rb
require './lib/calculator'  #=> add this

RSpec.describe Calculator do
  #...
end
```

Nếu chạy `rspec` lần này, bạn sẽ nhận được lần thất bại đầu tiên!

```bash
F

Failures:

  1) Calculator#add returns the sum of two numbers
     Failure/Error: expect(calculator.add(5, 2)).to eql(7)

       expected: 7
            got: nil

       (compared using eql?)
     # ./spec/calculator_spec.rb:7:in `block (3 levels) in <top (required)>'

Finished in 0.28565 seconds (files took 0.6273 seconds to load)
1 example, 1 failure

Failed examples:

rspec ./spec/calculator_spec.rb:5 # Calculator#add returns the sum of two numbers

```

Lần thất bại đầu tiên của chúng ta được biểu thị bằng `F` ở đầu kết quả. Xin chúc mừng! Bạn đã bước vào phần "đỏ" của chu kỳ "đỏ-xanh-tái cấu trúc" trong TDD. RSpec cung cấp danh sách tất cả các lỗi, cùng với kết quả kỳ vọng và thực tế của phương thức đang được kiểm thử. Ở cuối kết quả, RSpec cũng trỏ đến dòng của bài kiểm thử thất bại; trong trường hợp này, đó là nơi block `it` của chúng ta bắt đầu.

Đưa phương thức này sang "xanh" sẽ không quá khó. RSpec đã chỉ rõ nguyên nhân thất bại: nó kỳ vọng kết quả là `7` khi chúng ta cung cấp `(5, 2)` làm tham số cho phương thức. Thay vào đó, phương thức trả về `nil`. Tại sao lại như vậy? À, `#add` của chúng ta *có* nhận hai tham số... nhưng chẳng làm gì với chúng cả! Hãy thêm lượng mã tối thiểu cần thiết để bài kiểm thử vượt qua:

```ruby
#lib/calculator.rb

class Calculator
  def add(a,b)
    a + b         #=> add this
  end
end
```

Sau đó, chạy lại bài kiểm thử để nhận được một dấu chấm duy nhất, cho biết bài kiểm thử đã vượt qua:

```bash
.

Finished in 0.0032 seconds (files took 0.14864 seconds to load)
1 example, 0 failures
```

Ở thời điểm này, việc tái cấu trúc là không cần thiết. Phương thức `#add` về cơ bản chỉ là một phương thức một dòng. Tuy nhiên, khi tiến bộ hơn trong quá trình học Ruby, bạn có thể thấy các phương thức của mình trở nên phức tạp hơn và nhận ra rằng mình phải nỗ lực thêm để tuân thủ các [nguyên tắc SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)). Khi thời điểm đó đến, việc sử dụng RSpec cùng chu kỳ "đỏ-xanh-tái cấu trúc" sẽ giúp bạn lập trình tự tin, vì biết rằng các lớp và hành vi của chúng vẫn tiếp tục đáp ứng những kỳ vọng đã xác định.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Đã đến lúc áp dụng những kiến thức mới học vào thực tế. Hãy làm hỏng bài kiểm thử `Calculator` của chúng ta.

1. Hãy triển khai một trường hợp kiểm thử mới cho phương thức `#add`, đã được viết sẵn cho bạn bên dưới. Chạy bài kiểm thử để xem lỗi. Viết lượng mã tối thiểu cần thiết để **cả hai** bài kiểm thử đều vượt qua, sau đó tái cấu trúc nếu cần.

   ```ruby
   #spec/calculator_spec.rb
   
   RSpec.describe Calculator do
     describe "#add" do
       it "returns the sum of two numbers" do
         # removed for brevity
       end
   
       # add this
       it "returns the sum of more than two numbers" do
         calculator = Calculator.new
         expect(calculator.add(2, 5, 7)).to eql(14)
       end
     end
   end
   ```

1. Viết một bài kiểm thử cho một phương thức mới của `Calculator` (`#multiply`, `#subtract` hoặc `#divide`) bằng một block `describe` mới. Bao gồm ít nhất một block `it` với mệnh đề kỳ vọng phù hợp. Làm cho nó vượt qua, rồi tái cấu trúc nếu cần.
1. Trong terminal, hãy thử chạy các bài kiểm thử thất bại hoặc vượt qua bằng `rspec --format documentation`. Có gì khác biệt?
1. RSpec đọc các cấu hình dòng lệnh từ `.rspec`, một trong hai tệp được tạo khi RSpec được khởi tạo trong dự án. Nếu thích kết quả nhận được với `--format documentation`, bạn có thể dùng tệp `.rspec` để lưu cờ đó. Như vậy, bạn sẽ không phải nhập lại cờ này mỗi lần chạy bộ kiểm thử. Mở tệp trong trình soạn thảo văn bản và thêm `--format documentation` vào một dòng mới. Để biết thêm thông tin, bạn có thể đọc thêm về [cấu hình RSpec](http://rspec.info/features/3-12/rspec-core/configuration/).

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, nên hãy xem nó như tài liệu bổ sung.

- Video Youtube này cung cấp một [tổng quan xuất sắc về những nền tảng của RSpec](https://www.youtube.com/watch?v=K6RPMhcRICE), đồng thời giới thiệu ngắn gọn một số khái niệm sẽ được đề cập trong bài học tiếp theo.
- Để có cái nhìn toàn diện hơn về RSpec, hãy đọc [phần RSpec trong cuốn sách "Kiểm thử cho người mới bắt đầu" của Ruby Monsta](http://testing-for-beginners.rubymonstas.org/rspec.html).
- Hãy xem qua [các matcher khác của RSpec](http://rspec.info/features/3-12/rspec-expectations/built-in-matchers/) nếu bạn chưa làm vậy.
- Hãy xem qua [cách tạo kiểu và cú pháp RSpec được BetterSpecs khuyến nghị](http://www.betterspecs.org/) và đọc sáu hướng dẫn đầu tiên.
- [Tờ ghi nhớ RSpec](https://devhints.io/rspec) sẽ giúp bạn không phải Google mỗi cú pháp mới.
- [Bắt đầu với RSpec Phần 1](https://mindovermiles262.medium.com/getting-started-with-rspec-part-1-9418909f5e53): Củng cố các khái niệm này với một lời tự quảng bá không hề giấu giếm từ một cộng tác viên khác của Odin Project.