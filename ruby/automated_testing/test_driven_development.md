### Giới thiệu

Đến giờ, bạn đã học cách kiểm thử code mình viết, nhưng có một quy trình phát triển phổ biến đảo ngược hoàn toàn khái niệm đó. Thay vì viết code, kiểm thử thủ công cho đến khi code hoạt động, rồi viết một test để đảm bảo nó tiếp tục hoạt động; bạn có thể viết test **trước** code, nhờ đó không phải lãng phí *bất kỳ* thời gian nào cho việc kiểm thử thủ công. Phát triển hướng kiểm thử là tên gọi của quy trình phát triển đảo ngược này.

### Mục tiêu học tập
Hãy xem qua các mục tiêu này ngay bây giờ và dùng chúng để định hướng việc học. Đến cuối bài học, bạn sẽ có thể:

- Hiểu phát triển hướng kiểm thử là gì
- Hiểu những ưu điểm của phát triển hướng kiểm thử
- Học cách viết code theo phong cách TDD

### TDD là gì và tại sao nên sử dụng?

Theo định nghĩa đơn giản nhất, Phát triển hướng kiểm thử (TDD) có nghĩa là bạn viết các test cho một method *trước* chính method đó. Khái niệm này có thể *thực sự* kỳ lạ, đặc biệt là khi bạn mới thử làm, nhưng cũng như mọi thứ khác trong lập trình (và trong cuộc sống nói chung), bạn càng làm nhiều thì nó càng trở nên quen thuộc, và bạn sẽ càng làm tốt hơn.

Làm theo Phát triển hướng kiểm thử đặc biệt hữu ích khi bạn vẫn đang học về kiểm thử, vì nó giúp bạn học cách viết code *dễ* kiểm thử ngay từ đầu, thay vì phải viết lại các method để khiến chúng có thể kiểm thử được.

Một lợi ích khác của việc viết code theo phong cách TDD là nó *đảm bảo* code của bạn sẽ được kiểm thử. Khi viết code với ý định kiểm thử sau đó, bạn rất dễ trì hoãn việc kiểm thử lâu đến mức nó không bao giờ xảy ra; còn việc kiểm thử *trước* khi viết phần code liên quan sẽ loại bỏ sự cám dỗ đó. Như đã đề cập trong phần giới thiệu, TDD một dự án có nghĩa là bạn không phải kiểm thử thủ công nhiều, nếu không muốn nói là bất kỳ chức năng nào, và khoảng thời gian lẽ ra dùng để kiểm thử thủ công có thể nhanh chóng cộng dồn trong một dự án lớn. Một lợi ích cuối cùng của cách tiếp cận TDD là nó có thể giúp bạn lập kế hoạch thiết kế tốt hơn và phát hiện lỗi sớm hơn trong quá trình phát triển, vì các test đang *định hướng* việc phát triển của bạn.

Tuy nhiên, cũng cần nói rằng không phải *ai cũng* đồng ý TDD là cách nên làm. Có rất nhiều developer viết test sau khi đã viết code. Khi mới học cách kiểm thử, có lẽ tốt nhất là bạn nên viết test sau code, để có thể tập trung học kiểm thử một cách độc lập. Nhưng khi đã cảm thấy thoải mái hơn với việc kiểm thử, bạn cứ thử áp dụng TDD!

#### Red Green Refactor

Khi lần đầu viết test trong Phát triển hướng kiểm thử, test đang ở trạng thái `red`, vì khi chạy test, nó sẽ thất bại và output sẽ có màu đỏ. Khi bạn viết một method làm test chạy qua, bạn đang ở trạng thái `green`, vì lúc này test đã thành công và output sẽ có màu xanh lá. Sau khi test chạy qua, bạn có thể `refactor` method, dùng test để đảm bảo method vẫn hoạt động như dự định sau khi refactor.

![Sơ đồ mô tả chu trình red-green-refactor](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/00.png)

Chu trình phát triển này được gọi là red-green-refactor, và nó nằm ở trung tâm của quy trình TDD. Khi làm theo TDD, bạn sẽ lặp lại ba bước này cho từng đơn vị công việc cần thiết của dự án cho đến khi hoàn thành!

Một khía cạnh quan trọng của chu trình red-green-refactor nhưng *không xuất hiện* trong tên gọi là code bạn viết để chuyển từ `red` sang `green` phải là lượng code **tối thiểu** cần thiết để test chạy qua. Nếu nhận thấy chức năng mình đang thêm thực tế làm *nhiều hơn* những gì được kiểm thử, đó là dấu hiệu cho thấy method của bạn có thể đang làm quá nhiều việc, hoặc có thể các test chưa kiểm tra đầy đủ chức năng cần thiết.

#### Ví dụ về TDD
Hãy cùng đi qua cách tiếp cận TDD với một class cơ bản.

Bài toán tổng quát là:

Tạo một class `Square` nhận một số nguyên khi khởi tạo để làm độ dài mỗi cạnh, đồng thời có method `#area` và method `#perimeter` lần lượt trả về (đúng như bạn có thể đoán) diện tích và chu vi của hình vuông.

Bắt đầu bằng cách tạo một directory để chứa ví dụ này, sau đó tạo các directory `lib` và `spec` để lần lượt chứa class và các spec. Tiếp theo, tạo các file spec và class. Cuối cùng, mở directory ví dụ trong text editor để bắt đầu theo phong cách TDD!

~~~ bash
mkdir tdd-lesson
cd tdd-lesson
mkdir lib
mkdir spec
touch lib/square.rb
touch spec/square_spec.rb
code .
~~~

Bây giờ, bạn sẽ áp dụng TDD để tạo class này như thế nào? Bạn biết mình cần `describe` một class có tên `Square`, đồng thời cần mô tả method `#area` của nó, nên hãy bắt đầu bằng cách thiết lập tất cả những thứ đó trong file `square_spec.rb`:

~~~rb
require_relative '../lib/square'

describe Square do
  describe "#area" do

  end
end
~~~

Vì `Square` sẽ được khởi tạo với độ dài cạnh, hãy thực hiện việc khởi tạo đó trong một vài block `context` khác nhau để có thể tạo các hình vuông với kích thước khác nhau nhằm kiểm thử method:

~~~rb
require_relative '../lib/square'

describe Square do
  describe "#area" do
    context 'when side length is 4' do
      let(:size_4_square) { described_class.new(4) }
    end

    context 'when side length is 6' do
      let(:size_6_square) { described_class.new(6) }
    end
  end
end
~~~

Tiếp theo, thêm một block `it` vào mỗi context để thực sự kiểm tra method hoạt động như bạn mong đợi:

~~~rb
require_relative '../lib/square'

describe Square do
  describe "#area" do
    context 'when side length is 4' do
      let(:size_4_square) { described_class.new(4) }

      it 'returns 16' do
        expect(size_4_square.area).to eq(16)
      end
    end

    context 'when side length is 6' do
      let(:size_6_square) { described_class.new(6) }

      it 'returns 36' do
        expect(size_6_square.area).to eq(36)
      end
    end
  end
end
~~~

Được rồi, vậy là bạn đã sẵn sàng kiểm thử `#area`. Bây giờ hãy chạy test bằng cách nhập `rspec` vào terminal!

![Ảnh chụp lỗi nhận được khi chạy test suite ở thời điểm này, đó là một NameError: uninitialized constant Square. Phần chữ có màu đỏ.](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/01.png)

Kết quả sẽ giống như lỗi *red* ở trên, vì bạn vẫn chưa triển khai `Square`. Đây là giai đoạn `red` của chu trình red-green-refactor đã đề cập trước đó, vì vậy bước tiếp theo là chuyển sang `green` bằng cách triển khai chức năng được mong đợi! Hãy thực hiện việc đó ngay bây giờ trong file `lib/square.rb`:

~~~rb
class Square
  def initialize(side_length)
    @side_length = side_length
  end

  def area
    @side_length * @side_length
  end
end
~~~

Bây giờ, nếu chạy lại test suite, bạn sẽ nhận được output *green* đẹp mắt cho biết không có lỗi! Hãy thử bằng cách nhập lại `rspec` vào terminal:

![Ảnh chụp cho thấy hai test hiện đã chạy qua. Phần chữ có màu xanh lá.](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/02.png)

Thành công! Bạn đang ở giai đoạn green của chu trình red-green-refactor! Giờ bạn có thể refactor vì biết rằng mình có thể dùng test để đảm bảo chức năng hiện có vẫn được giữ nguyên, chẳng hạn như:

~~~rb
class Square
  def initialize(side_length)
    @side_length = side_length
  end

  def area
    @side_length ** 2
  end
end
~~~

Đúng là gọn hơn một chút! Bạn có thể chạy lại `rspec` để đảm bảo các test vẫn chạy qua:

![Ảnh chụp cho thấy hai test vẫn đang chạy qua. Phần chữ có màu xanh lá.](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/02.png)

Có vẻ như mọi thứ đều ổn! Bây giờ hãy làm method `#perimeter`! Trước tiên, hãy viết test:

~~~rb
require_relative '../lib/square'

describe Square do

  # omitted for brevity

  describe "#perimeter" do
    context 'when side length is 4' do
      let(:size_4_square) { described_class.new(4) }

      it 'returns 16' do
        expect(size_4_square.perimeter).to eq(16)
      end
    end

    context 'when side length is 6' do
      let(:size_6_square) { described_class.new(6) }

      it 'returns 24' do
        expect(size_6_square.perimeter).to eq(24)
      end
    end
  end
end
~~~

Chạy test để đảm bảo nó thất bại như bạn mong đợi:

![ảnh chụp cho thấy hiện có hai test chạy qua và hai test thất bại trong test suite. Lỗi được đưa ra là một NoMethodError: undefined method `perimeter` for #<Square:0x000055f96bad77e8 @side_length=6>. Phần chữ có màu đỏ.](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/03.png)

Bây giờ hãy viết lượng chức năng tối thiểu cần thiết để các test chạy qua:

~~~rb
class Square
  def initialize(side_length)
    @side_length = side_length
  end

  def area
    @side_length ** 2
  end

  def perimeter
    @side_length * 4
  end
end
~~~

Sau đó chạy test để đảm bảo chúng chạy qua:

![Ảnh chụp cho thấy cả bốn test hiện đã chạy qua trong test suite. Phần chữ có màu xanh lá.](https://cdn.statically.io/gh/TheOdinProject/curriculum/ab4656dabe528812c252eb7393452251a744e57b/ruby_programming/automated_testing/test_driven_development/imgs/04.png)

Vì có vẻ như phần này không cần refactor nên chúng ta đã hoàn thành!

### Bài tập

TODO: Bài tập: Hoàn thiện TDD thêm một vài method trong class ví dụ được dùng trong nội dung

TODO: Bài tập: Dùng TDD để phát triển một value object

TODO: Bài tập: Dùng TDD để phát triển một class có collaborator chưa tồn tại bằng mock

### Tài nguyên bổ sung
Phần này chứa các liên kết hữu ích đến nội dung khác. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

Đọc bài ["TDD is Dead. Long Live Testing"](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) của DHH để xem một quan điểm phản đối TDD