### Giới thiệu

Trong bài học trước, chúng ta tập trung vào việc đo độ phức tạp xét từ góc độ thời gian. Chúng ta đã tìm hiểu những cách khác nhau để đo độ phức tạp của thuật toán và lý do Big O là cách được ưu tiên. Chúng ta cũng đã xem một số ví dụ về cách áp dụng cách đo này vào độ phức tạp thời gian của một thuật toán.

Trong bài học này, chúng ta sẽ tập trung vào độ phức tạp không gian và xem cách những ký hiệu đã học có thể được dùng để đo việc thay đổi đầu vào của thuật toán ảnh hưởng như thế nào đến lượng bộ nhớ mà thuật toán sử dụng.

Khi nói về bộ nhớ, chúng ta đang nói đến bộ nhớ chính, tức bộ nhớ làm việc mà hệ thống cung cấp để thực thi các thuật toán. Bạn có thể đọc thêm về chủ đề này trong [bài viết về bộ nhớ chính của GeeksforGeeks](https://www.geeksforgeeks.org/primary-memory/).

### Tổng quan bài học

Phần này cung cấp cái nhìn tổng quan về các chủ đề bạn sẽ học trong bài.

- Độ phức tạp không gian là gì.
- Tầm quan trọng của độ phức tạp không gian.
- Cách đo độ phức tạp không gian.
- Phân tích không gian phụ trợ.

### Độ phức tạp không gian là gì?

Có thể xem độ phức tạp không gian là tổng không gian mà một thuật toán sử dụng xét tương quan với kích thước đầu vào. Nhớ lại bài học trước, bạn sẽ thấy rằng chúng ta không đánh giá hiệu quả của một thuật toán dựa trên cách nó hoạt động trong một lần chạy cụ thể. Thay vào đó, chúng ta muốn biết hiệu quả thay đổi như thế nào khi kích thước đầu vào thay đổi.

Việc đo độ phức tạp không gian tính đến không gian mà đầu vào của thuật toán sử dụng và không gian phụ trợ. Không gian phụ trợ là phần không gian thêm mà thuật toán sử dụng. Nó có thể là những thứ như các biến tạm thời được tạo ra trong quá trình thực thi thuật toán. Chúng không tạo ra ảnh hưởng lâu dài đến không gian bộ nhớ, nhưng vẫn cần được tính đến trong lúc thuật toán chạy. Vì vậy, bạn có thể xem độ phức tạp không gian là tổng lượng bộ nhớ làm việc mà thuật toán cần.

### Tầm quan trọng của việc xét độ phức tạp không gian

Nếu tự tìm hiểu về hiệu quả của thuật toán, bạn có thể dễ dàng cho rằng độ phức tạp không gian không quá quan trọng. Hầu hết các bài viết bạn đọc về chủ đề này đều dành toàn bộ, hoặc phần lớn, nội dung để nói về độ phức tạp thời gian; nếu có nhắc đến độ phức tạp không gian thì cũng gần như chỉ xem đó là một chú thích.

Cũng có thể nói rằng hầu hết các thuật toán bạn viết sẽ xử lý những kích thước đầu vào khá dễ quản lý, vì vậy bộ nhớ thường không trở thành vấn đề. Gần như chắc chắn bạn sẽ gặp tình trạng chương trình chạy chậm trước khi gặp vấn đề do bộ nhớ bị sử dụng hết.

Mặt khác, phần cứng của bạn thường chỉ có một lượng bộ nhớ cố định. Trong hầu hết tình huống, bạn không thể dễ dàng mở rộng nó ngay trong lúc chương trình đang chạy. Khi vấn đề nằm ở thời gian, bạn chỉ cần cho chương trình chạy lâu hơn và cuối cùng nó sẽ trả về kết quả. Với không gian thì không thể làm như vậy.

Xét tổng thể, có lẽ bạn sẽ gặp nhiều bài toán mà thời gian thực thi quan trọng hơn lượng không gian sử dụng. Tuy nhiên, hiểu cách đo độ phức tạp không gian sẽ giúp bạn sẵn sàng xử lý khi gặp tình huống bị giới hạn về bộ nhớ.

### Đo độ phức tạp không gian

Tin tốt là chúng ta đo độ phức tạp không gian hoàn toàn giống cách đo độ phức tạp thời gian. Bạn đã học về Big O trong bài trước, nên đã biết cách đo hiệu quả của mã. Điểm khác biệt là bạn cần nghĩ về cách thuật toán sử dụng bộ nhớ thay vì thời gian.

Điều đầu tiên cần biết là, cũng như với độ phức tạp thời gian, chúng ta đo độ phức tạp không gian bằng cách xét tất cả các bước, bao gồm cả các hằng số, rồi loại bỏ các hằng số khi biểu diễn thuật toán bằng ký hiệu Big O. Chẳng hạn, ta có thể có một thuật toán sử dụng bộ nhớ theo Độ phức tạp tuyến tính khi đầu vào thay đổi, đồng thời tạo ra 3 biến tạm thời. Khi đó, ta có thể xem độ phức tạp của thuật toán là O(N) + 3 biến phụ trợ sử dụng bộ nhớ. Vì 3 biến đó không thay đổi dù kích thước đầu vào là bao nhiêu, chúng ta không cần quan tâm đến chúng khi xét độ phức tạp không gian của thuật toán. Do đó, ta nói độ phức tạp không gian là O(N). Điều này hẳn đã quen thuộc với bạn từ bài học về độ phức tạp thời gian.

Nhắc lại, những ký hiệu Big O phổ biến nhất là:

- O(1) - Độ phức tạp hằng số
- O(log N) - Độ phức tạp logarit
- O(N) - Độ phức tạp tuyến tính
- O(N log N) - Độ phức tạp N x log N
- O(n&#178;) - Độ phức tạp bậc hai
- O(n&#179;) - Độ phức tạp bậc ba
- O(2&#8319;) - Độ phức tạp lũy thừa
- O(N!) - Độ phức tạp giai thừa

Hãy cùng xem qua một số ví dụ. Chúng ta sẽ không đi qua mọi độ phức tạp có thể có, vì phần lớn chúng không áp dụng cho những cấu trúc dữ liệu mà bạn sẽ quen thuộc và sử dụng nhiều nhất. Chúng ta sẽ tập trung vào những loại phổ biến nhất.

#### O(1) - độ phức tạp hằng số

Hãy xem ví dụ sau.

```ruby
def multiply(num1, num2)
  num1 * num2
end
```

Ở đây, hy vọng bạn có thể thấy rõ rằng bất kể ta truyền đối số nào vào lời gọi hàm, chỉ một giá trị duy nhất được tạo ra (tích của các số). Giá trị đó không thay đổi. Vì vậy, ta có thể xem không gian mà đoạn mã này sử dụng luôn là O(1).

#### O(N) - độ phức tạp tuyến tính

Hầu hết các cấu trúc dữ liệu bạn gặp sẽ có độ phức tạp không gian là O(N). Điều này hợp lý: khi bạn tăng số lượng phần tử trong cấu trúc dữ liệu, không gian mà cấu trúc dữ liệu đó chiếm dụng cũng tăng tuyến tính.

```ruby
def sum_arr(arr)
  copy_arr = arr.dup
  sum = 0
  copy_arr.each do |number|
    sum += number
  end
  return sum
end
```

Chúng ta viết đoạn mã này dài dòng hơn một chút so với cách thường viết trong Ruby để làm cho ý tưởng rõ ràng hơn. Ở đây, ta có một phương thức nhận vào một mảng. Bên trong phương thức có hai biến: một biến tên là `sum`, và biến còn lại là `copy_arr`, chứa một bản sao của mảng được truyền vào. Sau đó, ta có một vòng lặp `each` duyệt qua mảng. Lượng không gian thuật toán sử dụng phụ thuộc vào mảng được truyền vào. Mảng có thể chứa 3 phần tử hoặc 300 phần tử. Khi không biết độ dài của mảng, ta gọi độ dài đó là N, nên ta có N + 1 biến tên là `sum`. Ta biết rằng Big O loại bỏ các hằng số, vì vậy còn lại N, hay ký hiệu Big O là O(N).

Tại sao chúng ta lại tạo một bản sao của mảng? Điều đó sẽ được thảo luận ở phần sau.

Độ phức tạp này cũng được giữ nguyên bất kể cấu trúc dữ liệu là gì:

```ruby
def sum_hash_values(hash)
  copy_hash = hash.dup
  sum = 0
  copy_hash.each do |_, value|
    sum += value
  end
  return sum
end
```

Ở đây, khi kích thước của hash tăng, không gian mà nó sử dụng cũng tăng theo tuyến tính.

#### Phân tích không gian phụ trợ

Một trong những điểm thường gây nhầm lẫn khi xét độ phức tạp không gian là việc thế nào được xem là sử dụng không gian trong ngữ cảnh của một thuật toán. Trong ví dụ trước, chúng ta viết các phương thức sao chép đối số là một mảng và một hash. Chúng ta làm vậy để mọi thứ rõ ràng hơn. Nhưng nếu chúng ta viết phương thức như sau thì sao?

```ruby
def sum_arr(arr)
  sum = 0
  arr.each do |number|
    sum += number
  end
  return sum
end
```

Khi một cấu trúc dữ liệu được truyền vào dưới dạng đối số, đặc biệt trong những ngôn ngữ truyền mảng bằng tham chiếu thay vì bằng giá trị, có thể khó xác định liệu phương thức đó có tính không gian mà cấu trúc dữ liệu sử dụng vào độ phức tạp không gian hay không. Ta vẫn có thể nói thuật toán này cần O(N) không gian, vì tính cả kích thước đầu vào. Hoặc ta có thể cho rằng việc tạo mảng đầu vào thuộc về một thủ tục bên ngoài nào đó đã cấp phát nó trước khi gọi hàm này. Khi đó, thuật toán chỉ chiếm O(1) không gian, vì mọi biến còn lại đều là hằng số. Cách tiếp cận sau được gọi là "phân tích không gian phụ trợ".

Theo cách tiếp cận này, chúng ta chỉ xét phần không gian *thêm* mà thuật toán sử dụng, nghĩa là không tính đầu vào. Cách này có thể hữu ích trong từng tình huống cụ thể để mô tả những thuật toán sử dụng không gian thêm theo những cách khác nhau. Hãy xem một ví dụ ngắn:

```ruby
def square_nums_in_place(arr)
  0.upto(arr.length) do |idx|
    arr[idx] = arr[idx] * arr[idx]
  end

  return arr
end

def square_nums_new_arr(arr)
  # note that `Array#map` makes a *new* array
  arr.map { |number| number * number }
end
```

Ở đây, ta có hai phương thức nhận vào một mảng các số và trả về một mảng trong đó mỗi số đã được bình phương. Phương thức đầu tiên thay đổi trực tiếp mảng đầu vào, trong khi phương thức thứ hai tạo một mảng mới với mỗi số được bình phương. Theo cách phân tích không gian truyền thống, cả hai phương thức đều sử dụng O(N) không gian. Nhưng trong tình huống này, cách phân tích đó không cho chúng ta biết nhiều về sự khác biệt rất rõ ràng trong cách hai thuật toán sử dụng bộ nhớ. Sẽ tốt hơn nếu dùng phân tích không gian phụ trợ và nói rằng hàm ở trên sử dụng O(1) không gian thêm, còn hàm ở dưới sử dụng O(N).

#### Những độ phức tạp khác

Như đã nói, nhiều cấu trúc dữ liệu có độ phức tạp không gian O(N), vì vậy bạn sẽ không viết nhiều thuật toán có độ phức tạp không gian khác biệt.

Tuy vậy, bạn sẽ bắt gặp một số hàm đệ quy và thuật toán sắp xếp có độ phức tạp không gian khác, dù thông thường bạn cũng không có nhiều lý do để xét đến những loại khác.

Trong bài học trước, một trong các bài tập là đường dẫn đến [bảng tra cứu Big-O](https://www.bigocheatsheet.com/). Nếu xem lại bảng đó ngay bây giờ, bạn có thể sẽ đánh giá đầy đủ hơn mức độ hữu ích đáng kinh ngạc của nó khi tham khảo độ phức tạp không gian và thời gian. Nếu cuộn xuống phần cấu trúc dữ liệu rồi đến phần thuật toán sắp xếp, bạn sẽ thấy bảng cung cấp độ phức tạp thời gian và không gian. Hãy chú ý xem có bao nhiêu trường hợp là O(N), đặc biệt là ở các cấu trúc dữ liệu. Nhiều thuật toán sắp xếp chỉ có độ phức tạp không gian O(1); đây là điều cần ghi nhớ khi bạn gặp các thuật toán sắp xếp khác nhau trong quá trình học.

Đó là lý do chúng ta sẽ không đi sâu vào các ví dụ về những ký hiệu Big O khác đối với độ phức tạp không gian. Chúng ta sẽ phải nghĩ ra những ví dụ rắc rối, không đại diện cho phần lớn mã mà bạn sẽ viết. Nếu bạn bắt gặp một ví dụ thực tế hay trong chính mã của mình, hãy cho chúng tôi biết; chúng tôi có thể cân nhắc bổ sung ví dụ đó để những người khác cùng tham khảo.

### Tổng kết

Đo độ phức tạp của thuật toán, dù là độ phức tạp thời gian hay không gian, có thể khó. Việc này cần sự luyện tập và cân nhắc. Với phần lớn mã dùng để luyện tập, đây không phải điều bạn nghĩ đến, đặc biệt khi bạn còn đang vất vả làm cho mã chạy được.

Tuy nhiên, một khi mã đã hoạt động và bạn có thể muốn tái cấu trúc nó, chắc chắn bạn nên dành một chút thời gian để cân nhắc xem mã đã hiệu quả nhất có thể chưa. Bạn có đang tạo ra những biến không cần thiết không? Hay thuật toán của bạn đang sử dụng một cấu trúc dữ liệu có độ phức tạp thời gian kém hơn cho mục đích sử dụng chủ yếu, trong khi một cấu trúc dữ liệu khác có thể phù hợp hơn?

Bên cạnh những điều đó, bạn cũng cần cân bằng với khả năng đọc hiểu của mã. Nếu bắt đầu đưa [memoization](https://en.wikipedia.org/wiki/Memoization) vào để làm cho mã hiệu quả hơn, liệu mã có trở nên khó hiểu hơn nhiều không? Sự đánh đổi đó có đáng không? Cuối cùng, bạn cần tự đưa ra quyết định. Lời khuyên của chúng tôi là hãy ưu tiên khả năng đọc hiểu, rồi chỉ tái cấu trúc để tăng hiệu quả nếu hiệu năng thực sự được cải thiện rõ rệt.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc câu trả lời đầu tiên cho câu hỏi [phân tích độ phức tạp không gian trên Stack Exchange](https://cs.stackexchange.com/questions/127933/analyzing-space-complexity-of-passing-data-to-function-by-reference) để tìm hiểu một số cách khác nhau mà không gian có thể được tính.
1. [Bài viết về đệ quy và độ phức tạp không gian](https://dev.to/elmarshall/recursion-and-space-complexity-13gc) này cung cấp thêm một chút bối cảnh về các hàm đệ quy và độ phức tạp không gian của chúng.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy bạn có thể xem nó như tài liệu bổ sung.

- Bài viết [Độ phức tạp không gian trong cấu trúc dữ liệu](https://www.scaler.com/topics/data-structures/space-complexity-in-data-structure/) cung cấp cái nhìn tổng quan toàn diện hơn về độ phức tạp không gian trong cấu trúc dữ liệu và thuật toán.
- Video về [Độ phức tạp không gian và những điểm dễ nhầm lẫn của Big O](https://www.youtube.com/watch?v=rHM3zWgnPVA).