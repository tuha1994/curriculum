### Giới thiệu

Mã nguồn. Mã được viết để máy của chúng ta thực thi nhưng quan trọng hơn là nó được viết để chúng ta, con người, hiểu được.

Bây giờ bạn đã thấy một số mã, đặc biệt nếu bạn siêng năng và dành thời gian thực hành đọc mã bằng cách khám phá các dự án được viết
bởi các đồng nghiệp của bạn đang trải qua Dự án Odin. Một số thì tốt, một số thì tệ nhưng có lẽ phần lớn chỉ là khó hiểu.
Tất nhiên, điều đó hoàn toàn ổn - tất cả các bạn vẫn đang học.

Nhưng các chuyên gia sẽ làm gì để đảm bảo mã của họ tốt nhất có thể:
có thể đọc được, có thể dự đoán và hoạt động được không? Họ cố gắng đưa ra những phương pháp hay nhất. Làm thế nào để họ thực thi chúng?

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Linting và định dạng là gì.
- RuboCop lấy các quy tắc khác nhau từ đâu và cách người ta có thể nhóm chúng lại.
- Cách cài đặt và cấu hình RuboCop.
- Cách RuboCop tương tác với RubyLSP và VSCode.
- Độ phức tạp chu kỳ và số liệu ABC là gì.

### Lại đến giờ nói về quy ước

Bạn đã được tiếp xúc với một số quy ước trong HTML, CSS, JS và Ruby.
Đây là lý do tại sao JS có các biến được viết là `likeThis` trong khi của Ruby là `like_this`, tại sao tên hàm là động từ và tên biến là danh từ. Tại sao một số ngôn ngữ thích vòng lặp trong khi những ngôn ngữ khác thích phương pháp lặp Tại sao một số tính năng
hiện diện trong các ngôn ngữ không được khuyến khích - hãy nhớ `var`, `eval()` hoặc `$global_variable`s?

Còn *rất nhiều* những điều này liên quan đến các tính năng của Ruby (hoặc một ngôn ngữ khác!) mà có thể bạn chưa từng nghe tới. Tin tốt là có những hướng dẫn về phong cách:
một bộ tiêu chuẩn để viết và tổ chức mã. Tin xấu? Đầu của bạn sẽ cần lưu trữ toàn bộ bộ tiêu chuẩn đó và có thể nhớ lại chúng bất cứ khi nào làm việc với mã.

### Mã của bạn cũng có những “ngăn” riêng

Hãy dùng thêm một bộ công cụ khác: linter và formatter. Chúng sẽ làm công việc ghi nhớ thay bạn - chúng sẽ chỉ ra rằng có điều gì đó không ổn. Có lẽ chúng có thể tự động sửa
một số vấn đề hoặc ít nhất là cung cấp gợi ý hoặc tài liệu tham khảo về hướng dẫn văn phong có thể giúp bạn viết mã sạch hơn.

Định dạng là làm cho mã của bạn trông gọn gàng và ngăn nắp mà không thay đổi hành vi của mã - hãy nghĩ đến việc thụt lề và các khoảng cách khác nhau, vì vậy hãy tạo phong cách. Linting nghĩa là làm cho mã của bạn dễ lý giải hơn - điều này có thể thay đổi cách mã của bạn hoạt động, chẳng hạn như bằng cách buộc bạn sử dụng `#each` thay vì vòng lặp `for`. Vì tất cả những điều đó, những người theo chủ nghĩa Ruby có một đồng minh đắc lực:

RuboCop. Một gem được hoàn thiện kỹ lưỡng sẽ làm cho mã của bạn tỏa sáng!

Trước khi bạn và RuboCop kiểm tra dự án Mật mã Caesar của bạn, hãy nói thêm một chút về Hướng dẫn về phong cách Ruby, bắt đầu với cách nó tự giới thiệu:
> Hướng dẫn về phong cách Ruby này đề xuất các phương pháp hay nhất để các lập trình viên Ruby trong thế giới thực có thể viết mã mà các lập trình viên Ruby trong thế giới thực khác có thể duy trì. Một hướng dẫn về phong cách phản ánh việc sử dụng trong thế giới thực sẽ được sử dụng, trong khi một hướng dẫn về phong cách bám sát một lý tưởng đã bị mọi người từ chối, nó được cho là sẽ giúp tránh rủi ro không được sử dụng chút nào - cho dù nó có tốt đến đâu. (...)
Các nguyên tắc được cung cấp ở đây nhằm mục đích cải thiện khả năng đọc mã và làm cho nó nhất quán trên phạm vi rộng của mã Ruby.

Như bạn có thể thấy, đó là những *đề xuất* được đưa ra theo cách sử dụng trong thế giới thực nhằm cải thiện khả năng đọc và tính nhất quán của mã Ruby. Cuối cùng, nó giống như sự khác biệt giữa cách bạn gửi tin nhắn trực tuyến và cách bạn viết một bài luận. Một là giao tiếp nhanh chóng, hai là giao tiếp rõ ràng.

Các quy ước khác nhau có thể được nhóm lại dựa trên những khía cạnh nào trong mã của bạn mà chúng xử lý. Để bắt đầu, hãy sử dụng một số bộ phận của RuboCop - Style, Lint và Metrics. Có lẽ bạn đã đoán trước rằng RuboCop không hoạt động một mình - nó thực sự có cả một “đội ngũ” phía sau. Các Cop khác nhau quan tâm đến việc đảm bảo rằng một số quy tắc cụ thể không bị vi phạm. Hãy để các bộ phận tự lên tiếng:

> Style Cops kiểm tra tính nhất quán về kiểu dáng trong mã của bạn. Nhiều trong số chúng dựa trên Ruby Style Guide.
>
> Lint Cops kiểm tra những điểm mơ hồ và lỗi có thể xảy ra trong mã của bạn.
>
> Metrics Cop xử lý các thuộc tính của mã nguồn có thể đo được, chẳng hạn như độ dài lớp, độ dài phương thức, v.v.

Bạn sẽ sớm có cơ hội tìm hiểu chi tiết về Hướng dẫn phong cách Ruby và đối với Cảnh sát - họ sẽ đảm bảo làm cho mình được biết đến. Trên lưu ý đó, hãy bắt đầu cuộc điều tra, thám tử!

### Phần nơi bạn tìm hiểu tất cả về tội phạm mã

Như đã đề cập trước đó, RuboCop là một viên Đá quý và dự án mà chúng tôi muốn điều tra là Caesar Cipher. Quay lại và cài đặt RuboCop cục bộ (như trong, sử dụng Bundler) rồi chạy `bundle exec rubocop` trong thiết bị đầu cuối của bạn. Việc chạy nó như thế này sẽ đảm bảo rằng phiên bản cục bộ của RuboCop được sử dụng và nó sẽ kiểm tra tất cả các tệp trong thư mục làm việc hiện tại *và* các thư mục con của nó. Tóm lại: tất cả mọi thứ.

Ối! Chà, ít nhất là đối với tôi - có lẽ bạn là một người hiểu biết và có phong cách Ruby bẩm sinh, trong trường hợp đó, đây là một số ví dụ đầu ra:

```bash
Inspecting 2 files
CC

Offenses:

Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:1:1: C: [Correctable] Style/FrozenStringLiteralComment: Missing frozen string literal comment.
def caesar_cipher(string, shift_factor)
^
caesars_cipher.rb:3:5: C: [Correctable] Layout/EmptyLineAfterGuardClause: Add empty line after guard clause.
    return string if shift_factor.remainder(26) == 0
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:3:22: C: [Correctable] Style/NumericPredicate: Use shift_factor.remainder(26).zero? instead of shift_factor.remainder(26) == 0.
    return string if shift_factor.remainder(26) == 0
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:15:5: C: [Correctable] Style/RedundantReturn: Redundant return detected.
    return character
    ^^^^^^
caesars_cipher.rb:16:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
  when "z"
       ^^^

2 files inspected, 19 offenses detected, 19 offenses autocorrectable

# Duplicate offenses in the same file were truncated.
```

Hãy phá vỡ điều này.
Đầu ra bắt đầu bằng việc cho biết có bao nhiêu tệp cần được kiểm tra:

```bash
Inspecting 2 files
```

Sau đó, một chuỗi chữ in hoa có phần bí ẩn xuất hiện: `CC`:

```bash
CC
```

Mỗi chữ cái tương ứng với một tập tin. Vì vậy, cả hai tệp được RuboCop kiểm tra đều báo cáo "quy ước" là vấn đề nghiêm trọng nhất của chúng.
Các chữ cái khác mà bạn có thể muốn biết là `W` dành cho cảnh báo và `F` dành cho trường hợp tử vong.
Cuối cùng, đầu ra đến hiện trường vụ án - hành vi phạm tội:

```bash
Offenses:

Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
```

Kết quả đầu ra giống dấu vết ngăn xếp một chút: bạn nhận được tệp, dòng và cột, sau đó là chữ cái mức độ nghiêm trọng. Sau đó bạn nhận được thông tin về việc liệu
RuboCop có thể tự khắc phục sự cố. Chúng tôi sẽ sớm quay lại vấn đề này. Hơn nữa, bạn tìm hiểu về Phòng/Cảnh sát - trong trường hợp này, bạn đang làm việc với StringLiterals Cop từ phòng Phong cách.
Rõ ràng, các trợ lý điều khiển học của chúng tôi đủ lịch sự để tự giới thiệu bản thân, vì vậy bạn sẽ được thông báo có điều gì sai. Cuối cùng, bạn sẽ được thông báo trực tiếp phần mã vi phạm là gì và ở đâu.
Ở đây, RuboCop đã chỉ ra rằng chuỗi này rất có thể được trích dẫn đơn vì chuỗi đó không sử dụng bất kỳ thứ gì đi kèm với chuỗi trích dẫn kép.

Bạn có thể hỏi tại sao một viên Đá quý nền tảng như Bundler lại có thể gặp rắc rối với pháp luật. Câu trả lời là RuboCop có khả năng tùy biến cao để đáp ứng nhiều tiêu chuẩn mà các lập trình viên
có thể có. Hãy nhớ: điều quan trọng trong việc tìm lỗi mã nguồn và định dạng là đảm bảo mọi người trong nhóm đều tuân theo các quy tắc giống nhau để mã dễ đọc hơn.

Trước khi giải phóng cơn giận dữ tự động của RuboCop đối với mã của mình, bạn có thể muốn làm quen với một lá cờ tiện lợi: `-S`. Điều này sẽ cung cấp một liên kết đến Hướng dẫn về Phong cách Ruby
điều đó đề cập đến lý do căn bản của hành vi phạm tội, nếu Cảnh sát có liên kết như vậy khi sử dụng `bundle exec rubocop -S`:

```bash
Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols. (https://rubystyle.guide#consistent-string-literals)
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
```

Tiện lợi nhỉ?

Nhưng tôi nghe thấy bạn - bạn ở đây để hành động. Vì vậy, hãy sử dụng cờ `-a`, một hành động! (Trên thực tế, nó là viết tắt của tự động sửa lỗi nhưng điều đó không thú vị bằng) `bundle exec rubocop -a`, bắt đầu!

```bash
Inspecting 2 files
.C

Offenses:

caesars_cipher.rb:1:1: C: [Correctable] Style/FrozenStringLiteralComment: Missing frozen string literal comment.
def caesar_cipher(string, shift_factor)
^
caesars_cipher.rb:3:5: C: [Corrected] Layout/EmptyLineAfterGuardClause: Add empty line after guard clause.
    return string if shift_factor.remainder(26) == 0
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:3:22: C: [Correctable] Style/NumericPredicate: Use shift_factor.remainder(26).zero? instead of shift_factor.remainder(26) == 0.
    return string if shift_factor.remainder(26) == 0
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:15:5: C: [Corrected] Style/RedundantReturn: Redundant return detected.
    return character
    ^^^^^^
caesars_cipher.rb:16:8: C: [Corrected] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
  when "z"
       ^^^

2 files inspected, 16 offenses detected, 14 offenses corrected, 2 more offenses can be corrected with rubocop -A
# Duplicate offenses were truncated.
```

Bạn có thấy `.` đó không? Điều đó có nghĩa là tập tin đầu tiên bây giờ đã ổn và đẹp đẽ! RuboCop đã không sửa một số lỗi vi phạm và đó là do `-a` dành cho tính năng tự động sửa lỗi an toàn.
Nếu bạn muốn xử lý các vi phạm `[Correctable]`, bạn nên sử dụng `-A` làm kết quả gợi ý hữu ích. Điều này là do thực tế là một số Cảnh sát an toàn, một số không an toàn.

Cảnh sát an toàn hứa rằng họ sẽ không có kết quả dương tính giả và tính năng tự động sửa lỗi của họ sẽ không thay đổi ngữ nghĩa của mã và nó sẽ hoàn toàn tương đương với những gì bạn đã viết.

Từ đó, có thể suy ra rằng Cảnh sát không an toàn có kết quả dương tính giả hoặc thay đổi một chút ngữ nghĩa của mã. Đặc điểm đầu tiên có nghĩa là chúng cho bạn biết có điều gì đó không ổn trong khi thực tế thì mọi thứ đều ổn - ví dụ: bạn có một phương thức có cùng tên với một phương thức trong thư viện chuẩn trong đối tượng của bạn và RuboCop coi nó như thể đó là phương thức thư viện chuẩn.

Điều sau có nghĩa là mặc dù mã của bạn và mã được đề xuất có cùng một đầu ra, chúng có thể thay đổi cách thức (chứ không phải cái gì) đạt được đầu ra hoặc tạo ra tác dụng phụ thậm chí có thể phá vỡ mã của bạn.

### Bạn là kẻ độc tài mã

Do hệ sinh thái của Ruby, RuboCop được xây dựng với khả năng cấu hình mở rộng - cả về việc không sử dụng một số phần và về mặt bổ sung vào nó. Mọi Cảnh sát đều có thể bị tắt, đôi khi Cảnh sát đưa ra các quy tắc thay thế như ưu tiên dấu ngoặc đơn hoặc dấu ngoặc kép cho Chuỗi, bạn có thể tắt Cảnh sát trên cơ sở mỗi tệp và hơn thế nữa.

Vì [RuboCop có thể mở rộng](https://docs.rubocop.org/rubocop/latest/plugins.html), nên tồn tại các bộ phận khác mà bạn có thể sử dụng - như Hiệu suất hoặc RSpec. Bạn thậm chí có thể viết Cop của riêng bạn! Quá trình thêm tiện ích mở rộng rất dễ dàng. Giả sử bạn muốn thêm Đá quý `rubocop-performance` vào dự án của mình. Trước tiên bạn cài đặt Gem cục bộ từ dòng lệnh:

```bash
gem install rubocop-performance
```

Tiếp theo, bạn phải yêu cầu Bundler đưa Đá quý vào dự án của bạn bằng cách thêm dòng sau vào GemFile của bạn (Những viên đá quý không bắt buộc để ứng dụng chạy thường được gắn cờ `require: false`):

```ruby
gem 'rubocop-performance', require: false
```

Bằng cách này, Gem sẽ được cài đặt bình thường, nhưng để `bundle exec` chạy mã để sử dụng nó, nó cần phải rõ ràng là `require`d ở bất cứ nơi nào bạn cần.

Tiếp theo, bạn cần yêu cầu RuboCop tải tiện ích mở rộng mới của bạn. Bạn có thể thực hiện việc này bằng cách chỉnh sửa tệp `.rubocop.yml`, đây là tệp cấu hình cho RuboCop và nó nằm trong thư mục gốc của dự án của bạn. Ở đó, bạn sẽ thay đổi các giá trị mặc định của RuboCop theo sở thích của bạn (hoặc rất có thể là của nhóm bạn). Để tạo tệp `.rubocop.yml`, hãy chạy lệnh này từ thiết bị đầu cuối trong thư mục dự án của bạn:

```bash
bundle exec rubocop --init
```

Nó sẽ không có gì trong đó ngoài một bình luận mô tả mục đích của nó. Bây giờ, tất cả những gì bạn phải làm là sao chép/dán các dòng sau vào tệp `.rubocop.yml` để `rubocop` biết sử dụng tiện ích mở rộng bất cứ khi nào bạn chạy `bundle exec rubocop`:

```yaml
plugins:
  - rubocop-performance
```

RuboCop vẫn đang được phát triển nên sẽ có những thay đổi và bổ sung. Cảnh sát mới tham gia khu vực bầu cử và chúng không được bật theo mặc định - nếu bạn muốn bật chúng theo mặc định thay vì xem qua tất cả chúng và tự mình quyết định, bạn có thể sử dụng:

```yaml
AllCops:
  NewCops: enable
```

trong `.rubocop.yml` của bạn để kích hoạt tất cả Cảnh sát mới.

### Nguồn bị hỏng

Có lẽ bạn không quan tâm đến việc điều chỉnh RuboCop theo ý thích của mình, đặc biệt khi bạn mới bắt đầu và hoàn toàn không biết điều gì là tốt và điều gì là xấu. Đó sẽ là cách tiếp cận đúng - đừng lo lắng về điều đó ngay bây giờ và chỉ cần sử dụng các giá trị mặc định của RuboCop.

Một trong những bộ phận mà bạn có thể muốn bỏ qua là Số liệu. Nó có thể sẽ là kẻ thù tồi tệ nhất của bạn khi bắt đầu bằng việc viết mã lớn hơn, hướng đối tượng hơn. Nhưng việc trở thành kẻ thù tồi tệ nhất đó có lý do chính đáng: nó cố gắng giúp bạn viết mã tốt hơn. Sẽ không sao nếu bạn không phải lúc nào cũng thỏa mãn được RuboCop nhưng việc trốn tránh sự hướng dẫn của nó trong quá trình học là một điều ngu ngốc.

Phải nói rằng, việc nhìn thấy những hành vi phạm tội đó tái diễn ở một nơi mà bạn đã chấp nhận việc không tuân thủ sẽ khiến bạn mất tập trung. Vì không muốn tắt hoàn toàn các Cảnh sát đó nên bạn có thể sử dụng nhận xét nội tuyến để tắt những điều khiến bạn khó chịu:

```ruby
# rubocop: disable Metrics/AbcSize
def some_convoluted_method(it_really, is_convoluted)
    # with convoluted code...
end
# rubocop: enable Metrics/AbcSize
```

Điều này sẽ vô hiệu hóa Cảnh sát `AbcSize` khỏi bộ phận `Metrics` giữa các dòng nhận xét đó. Hãy nhớ: đừng sử dụng điều này để tránh phải làm việc với mã của bạn, hãy sử dụng nó bất cứ khi nào thích hợp - thực sự có thể xảy ra trường hợp một số phương pháp phải phức tạp đến mức đó hoặc ít nhất đó là niềm tin hiện tại.

Một số quy tắc tùy ý hơn rất nhiều - bộ phận Phong cách sẽ là cơ sở chính cho những tranh luận mạnh mẽ về những thứ không thực sự quan trọng - như trích dẫn kép tất cả các chuỗi so với việc phân biệt giữa chuỗi đơn giản và nội suy chuỗi. Có lẽ bạn có cảm xúc mạnh mẽ về các trích dẫn, vì vậy hãy giúp bạn bằng cách chỉ cho bạn cách hiển thị chúng cho RuboCop.

Bắt đầu bằng cách tạo tệp `.rubocop.yml` bằng lệnh `bundle exec rubocop --init` hoặc thủ công bằng cách sử dụng cảm ứng hoặc nano (nano sẽ mở trình soạn thảo văn bản ngay lập tức). Đừng quên rằng nó phải là một dotfile, nghĩa là nó cần phải có dấu chấm trước tên của nó. Bây giờ, bạn cần tìm ra quy tắc nào bạn muốn thay đổi hoặc vô hiệu hóa. Để biết các tùy chọn có thể, hãy luôn tham khảo tài liệu - không phải mọi Cảnh sát chỉ là bật/tắt đơn giản, có thể có nhiều tùy chọn hơn. Ví dụ: chúng tôi sẽ thay đổi các quy tắc liên quan đến chuỗi, chuỗi ký tự cố định và chúng tôi sẽ kích hoạt NewCops.

```yaml
# This is .rubocop.yml in ~/
AllCops:
  NewCops: enable

Style/StringLiterals:
  EnforcedStyle: double_quotes

Style/FrozenStringLiteralComment:
  EnforcedStyle: never
```

Việc đặt `.rubocop.yml` trong `~` không phải ngẫu nhiên - nếu RuboCop không thể tìm thấy tệp cấu hình ở bất kỳ đâu trong dự án, nó sẽ tìm tệp đó ở một vài nơi khác, một trong số đó là thư mục chính của bạn. Tệp cấu hình này sẽ giúp mọi dự án không có cấu hình riêng sẽ tuân theo các quy tắc này - NewCops được bật, tất cả các chuỗi ký tự đều được trích dẫn kép và không cho phép nhận xét kỳ diệu bật hoặc tắt các ký tự chuỗi cố định - điều cuối cùng này sẽ có ý nghĩa sau khi bạn làm việc với RuboCop một thời gian.

Nhưng điều gì sẽ xảy ra với các dự án của bạn muốn sử dụng *một số* cấu hình chung nhưng không phải tất cả? Nhập: `inherit_from:`. Bằng cách thêm một dòng có `inherit_from: ~/.rubocop.yml` vào `.rubocop.yml` cục bộ của bạn, nó sẽ sử dụng các quy tắc tương tự như được xác định ở đó. Sau đó bạn có thể ghi đè lên chúng cục bộ. Điều gọn gàng hơn? Bạn có thể có các `.rubocop.yml` dành riêng cho thư mục kế thừa từ tệp cấu hình cụ thể của dự án của bạn chỉ để đảm bảo mọi tệp trong thư mục đó đều tuân theo hoặc không tuân theo một số quy tắc. Hãy xem một ví dụ:

```yaml
# This is .rubocop.yml in ~/my-cool-project/

inherit_from: ~/.rubocop.yml

Style/StringLiterals:
  EnforcedStyle: single_quotes

Style/FrozenStringLiteralComment:
  EnforcedStyle: always
```

Và bây giờ bạn quay lại trích dẫn đơn và luôn có một nhận xét kỳ diệu liên quan đến chuỗi ký tự cố định. Lưu ý: nếu bạn dựa vào cấu hình toàn cầu như vậy, bạn có thể quên đưa nó vào dự án của mình. Sẽ rất hợp lý nếu những người sẽ thực hiện dự án tuân theo các quy tắc mà nó đã được tạo ra trong đầu.

Tuy nhiên, mặc định là hoàn toàn tốt. Không cần phải mày mò bất cứ điều gì. Theo thời gian, bạn sẽ thấy những quy tắc nào đưa ra cho mình và khi nào việc phá bỏ chúng là hợp lý. Trên thực tế, có một cuộc nói chuyện thú vị về điều đó trong bài tập nên nếu bạn chưa bị thuyết phục, hãy tạm dừng ngựa một lát!

Vì vậy, hãy cố gắng hết sức để giải quyết RuboCop nhưng chấp nhận rằng mã của bạn sẽ không hoàn hảo. Không sao đâu. Hãy chống lại sự cám dỗ để thoát khỏi những điều mặc định. Bạn vẫn đang học - chỉ cần nỗ lực một cách trung thực để làm cho mã của bạn tốt hơn. Có lẽ việc đọc code của người khác sẽ chỉ đường cho bạn, vì vậy hãy luôn nhớ dành chút thời gian để đọc code sau khi hoàn thành một dự án!

### Số liệu sẽ vô ích nếu không được hiểu rõ

Khuyến nghị của chúng tôi là bám sát bộ phận Số liệu yêu cầu chúng tôi giúp giải thích các khái niệm khó hiểu hơn được sử dụng ở đó: số liệu ABC, độ phức tạp chu kỳ và độ phức tạp được nhận thức.

Các chữ cái trong ABC không phải là ngẫu nhiên, chúng là viết tắt của **A**ssignment, **B**ranches và **C**conditionals. Bài tập liên quan đến việc thiết lập hoặc thay đổi một biến, các nhánh có thể gây nhầm lẫn, đề cập đến các lệnh gọi phương thức và điều kiện đều là các câu lệnh và so sánh có điều kiện khác nhau thông thường như `==` hoặc `<=`.

Tác giả của ABC cho biết nó đo kích thước phần mềm và nó được tạo ra với câu trích dẫn: "khắc phục nhược điểm của dòng mã và các biện pháp tương tự". Đúng, đã có lúc độ dài mã chứ không phải độ phức tạp của nó mới là thước đo cho một phần mềm tốt.

Bên cạnh việc thông báo cho bạn về việc vượt quá giá trị được phép cho số liệu, RuboCop cũng sẽ cung cấp cho bạn tổng điểm ABC và các phần cấu thành của nó:

```bash
C: Metrics/AbcSize: Assignment Branch Condition size for testing is too high. \[\<1, 18, 0\> 18.03/17\]
```

Trong trường hợp này, có một bài tập, mười tám nhánh và không có điều kiện, sau khi sử dụng công thức tính điểm, kết quả là 18,03 trong khi điểm cho phép là 17.

Một cách để giải thích điểm cụ thể này là nói rằng phương pháp này phụ thuộc rất nhiều vào các phương pháp khác để thực hiện điều gì đó với dữ liệu. Có lẽ quá trình này có thể được chia thành các bước hoặc tồn tại một số lỗi thiết kế đòi hỏi chúng ta phải thao tác quá nhiều dữ liệu ở một nơi này.

Độ phức tạp theo chu kỳ tương tự như thước đo có điều kiện trong ABC. Nó nhằm mục đích cung cấp cái nhìn sâu sắc về độ phức tạp của chương trình dựa trên số lượng đường dẫn mà chương trình (phương pháp) có thể đi qua. Như bạn có thể tưởng tượng, điều này đề cập đến các câu lệnh luồng điều khiển như câu lệnh if, vòng lặp và toán tử logic như `&&` hoặc `||`.

Trong ngữ cảnh Ruby, thay vì các vòng lặp, rất có thể bạn sẽ sử dụng các phương thức như `#each` để lặp lại các bộ sưu tập của mình - điều đó cũng được tính. Mỗi khi việc thực thi mã có thể đi theo đường dẫn này hoặc đường dẫn khác, một đường dẫn sẽ được thêm vào điểm.

Sự phức tạp được cảm nhận rất giống với độ phức tạp theo chu kỳ. Nó cố gắng đo lường mức độ khó đọc mã của con người và điểm khác biệt với độ phức tạp theo chu kỳ là nó sử dụng trọng số cho một số câu lệnh luồng điều khiển và đếm cả `if` và `else` thay vì chỉ câu lệnh if làm một đường dẫn phân nhánh.

### Sẽ thật tuyệt nếu có tất cả những thứ này trong VSC phải không?

Nó chắc chắn sẽ như vậy! Nhờ Ruby-LSP, RuboCop được tích hợp với Visual Studio Code. Tất cả những gì bạn cần là một Gemfile được thiết lập với RuboCop trong dự án của bạn. Nhờ đó, RuboCop được chạy liên tục trong khi bạn viết mã, cung cấp cho bạn phản hồi khi đang di chuyển:

![Dự án Cipher của Caesar trong VSC có nhiều vấn đề](https://cdn.statically.io/gh/TheOdinProject/curriculum/270b3d2430621d1d768234d53588054dc4bdda13/ruby/object_oriented_programming_basics/linting_and_rubocop/imgs/rubocopinvsc.png)

Bạn đã có danh sách các vấn đề được gạch chân và định dạng gọn gàng được liệt kê trong tab `Problems`. Hãy nhớ rằng tab `Problems` có tính tương tác - hãy nhớ sử dụng nó!

Khi bạn di chuột qua đoạn mã được gạch chân, bạn sẽ được thông báo về hành vi vi phạm và được cung cấp các liên kết đến tài liệu RuboCop - *không phải* hướng dẫn về Ruby Style - đồng thời cũng được cung cấp các phím tắt đến `View Problems` và `Quickfix`:

![VSC bật lên sau khi bạn di chuột vào một hành vi phạm tội](https://cdn.statically.io/gh/TheOdinProject/curriculum/270b3d2430621d1d768234d53588054dc4bdda13/ruby/object_oriented_programming_basics/linting_and_rubocop/imgs/rubohover.png)

Các bản sửa lỗi nhanh có khá nhiều `rubocop -a` nhưng chỉ giới hạn ở một dòng cụ thể. Hãy lưu ý rằng từ đây bạn cũng có thể vô hiệu hóa và kích hoạt Cảnh sát đứng sau hành vi phạm tội:

![Cửa sổ bật lên sửa lỗi nhanh của VSC](https://cdn.statically.io/gh/TheOdinProject/curriculum/270b3d2430621d1d768234d53588054dc4bdda13/ruby/object_oriented_programming_basics/linting_and_rubocop/imgs/quickfixrubocop.png)

### Để viết code tốt trước tiên bạn cần phải viết nhiều code xấu

Bạn có thể thắc mắc tại sao khi cài đặt Ruby bạn lại không được thông báo về Rubocop. Nếu viết mã sạch là mục tiêu, tại sao không bắt đầu với một trình định dạng và một trình nói dối? Tại sao không đi ngay vào phần hướng dẫn về phong cách?

Lý do cho điều này có rất nhiều, nhưng một số trong số đó sẽ là:

- Nếu bạn chỉ tìm hiểu về hướng dẫn phong cách, bạn sẽ không biết nó đề cập đến điều gì hoặc việc bạn thiếu kinh nghiệm với Ruby sẽ khiến bạn không hiểu được những ưu và nhược điểm.
- Nếu RuboCop đồng hành cùng bạn trong suốt hành trình này, bạn sẽ không bao giờ có thể phạm phải tất cả những sai lầm đó và đánh giá cao những gì nó làm được.
- Ngoài ra, một lần nữa bạn sẽ gặp phải những điều mà bạn không hề biết.
- Bạn còn phải lo nhiều thứ nữa: Bundler, Ruby LSP, RuboCop. Vì điều này, bạn sẽ chìm đắm trong cấu hình không phù hợp, chưa kể đến tất cả các vấn đề tiềm ẩn mà bạn không được trang bị để giải quyết vào thời điểm đó.

Hy vọng điều đó sẽ làm sáng tỏ điều này. Đừng quên: RuboCop và hướng dẫn về phong cách luôn sẵn sàng giúp *bạn* viết mã rõ ràng hơn, có thể dự đoán được. Các tiêu chuẩn có thể khác nhau giữa các nhóm, một số kỳ vọng mà RuboCop đặt ra cho bạn dường như không thể thực hiện được và bạn có thể cảm thấy một số quy tắc quá hạn chế.

Theo thời gian, bạn sẽ tuân thủ các quy tắc tốt hơn nhưng có lẽ quan trọng hơn là bạn sẽ hiểu *tại sao* và *khi nào* phá vỡ chúng. Còn bây giờ, hãy tận hưởng chuyến đi trên vai những người khổng lồ.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Tìm hiểu thêm [về xơ vải là gì](https://en.wikipedia.org/wiki/Lint_(software)).
1. Làm quen với [phần giới thiệu về Ruby Style Guide](https://rubystyle.guide/) và đọc lướt qua một chút.
1. Xem [RuboCop là gì](https://docs.rubocop.org/rubocop/index.html) và khám phá tài liệu của nó.
1. [Xem "Rules" của Sandi Metz](https://www.youtube.com/watch?v=npOGOmkxuio) để có khám phá thú vị về cái gì, tại sao và như thế nào của các quy tắc nói chung cũng như trong bối cảnh Ruby.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Nó không bắt buộc, vì vậy hãy xem xét nó bổ sung.

- Nếu bạn tò mò về lịch sử của RuboCop và cách thức hoạt động của nó thì [Bài nói chuyện về RuboCop của Bozhidar Batsov](https://www.youtube.com/watch?v=nrHjVCuVsGA) chắc chắn sẽ là một buổi nghe thỏa mãn.
- [Trò chuyện về Cải thiện trải nghiệm phát triển với máy chủ ngôn ngữ của Vincis Stock](https://www.youtube.com/watch?v=kEfXPTm1aCI) rất phù hợp cho những người muốn biết thêm về Giao thức máy chủ ngôn ngữ, đặc biệt là giao thức Ruby.
- [Bài viết của Honeybadger về Rubocop](https://www.honeybadger.io/blog/linting-formatting-ruby/) có thể mang đến cho bạn một góc nhìn khác và giới thiệu một số tính năng chưa được đề cập trong bài.
