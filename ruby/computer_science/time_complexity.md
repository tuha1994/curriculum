### Giới thiệu

Đến thời điểm này, bạn đã viết khá nhiều mã và hy vọng đã chuyển từ việc chỉ cố gắng viết mã hoạt động được sang quan tâm đến khả năng đọc hiểu và bảo trì mã. Bạn có thể dành thời gian suy nghĩ về cách tạo ra những lớp trừu tượng cần thiết để mã vẫn dễ làm việc ngay cả khi các yêu cầu đối với nó ngày càng mở rộng.

Khả năng đọc hiểu và khả năng bảo trì mã cực kỳ quan trọng. Sau cùng, có lẽ bạn sẽ dành nhiều thời gian, nếu không muốn nói là nhiều hơn, để đọc mã so với viết mã. Bạn cần bảo đảm rằng các tính năng mới có thể được tích hợp một cách dễ dàng.

Tuy nhiên, khi viết mã còn có một yếu tố khác cũng quan trọng không kém. Đó là hiệu năng! Bạn cần hiểu mã mình viết sẽ hoạt động như thế nào. Bạn cũng cần hiểu những lựa chọn của mình ảnh hưởng đến hiệu năng ra sao, để có thể chọn đúng cấu trúc dữ liệu và thuật toán cho yêu cầu của mình.

Trong lập trình, có hai cách để đo lường hiệu quả của mã. Chúng ta có thể đo độ phức tạp thời gian hoặc độ phức tạp không gian.

Trong bài học này, chúng ta sẽ giới thiệu những khái niệm cốt lõi liên quan đến việc đo lường hiệu quả về thời gian của mã bạn viết.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Cách đo lường hiệu quả của một thuật toán.
- Big O là gì.
- Những ký hiệu Big O nào được dùng để đo lường hiệu quả của một thuật toán.
- Những cách khác để đo lường hiệu quả của một thuật toán.
- Cần làm gì khi hai thuật toán có cùng độ phức tạp.

### Những kiến thức cơ bản về hiệu năng

Bước đầu tiên để thành thạo việc viết mã hiệu quả là hiểu cách đo lường hiệu quả đó. Hãy cùng xem một chương trình nhỏ in ra tất cả các số lẻ từ 1 đến 10.

```ruby
def odd_numbers_less_than_ten
  current_number = 1

  while current_number < 10
    if current_number % 2 != 0
      puts current_number
    end

    current_number += 1
  end
end
```

Nếu chạy đoạn mã này trong terminal, bạn sẽ thấy các số `1`, `3`, `5`, `7` và `9` được in ra màn hình. Có lẽ chương trình chỉ mất một phần nhỏ của một giây để chạy. Nếu chạy lại, thời gian có thể giống vậy, hoặc nhanh hơn hay chậm hơn tùy vào những việc khác mà máy tính của bạn đang thực hiện. Nếu chạy trên một máy tính khác, chương trình cũng có thể nhanh hơn hoặc chậm hơn. Vì vậy, điều quan trọng là phải hiểu rằng bạn không bao giờ đo hiệu quả của một thuật toán bằng thời gian nó cần để thực thi.

Vậy chúng ta đo lường bằng cách nào?

Cách đo hiệu quả của mã là đánh giá nó cần bao nhiêu “bước” để hoàn thành. Nếu biết một thuật toán bạn viết cần 5 bước còn một thuật toán khác cần 20 bước để thực hiện cùng một nhiệm vụ, bạn có thể nói rằng thuật toán 5 bước sẽ luôn chạy nhanh hơn thuật toán 20 bước trên cùng một máy tính.

Hãy quay lại phương thức `odd_numbers_less_than_ten`. Thuật toán của chúng ta cần bao nhiêu bước?

1. Gán số 1 cho một biến. Đây là một bước.

1. Chúng ta có một vòng lặp. Trong mỗi lần lặp, ta thực hiện những việc sau:

    1. So sánh `current_number` để kiểm tra xem nó có nhỏ hơn 10 hay không. Đây là 1 bước.
    1. Sau đó kiểm tra xem `current_number` có phải số lẻ hay không. Đây là 1 bước.
    1. Nếu đúng, in nó ra terminal. Đây là 1 bước sau mỗi 2 lần lặp.
    1. Tăng `current_number` lên 1. Đây là 1 bước.

1. Để thoát khỏi vòng lặp, chúng ta cần so sánh `current_number` thêm một lần cuối để xác nhận rằng nó không còn nhỏ hơn 10. Đây là một bước cuối cùng.

Như vậy, mỗi lần lặp có 3 bước và vòng lặp chạy 9 lần, tức là 27 bước. Ngoài ra còn có một bước chỉ được thực hiện trong một nửa số lần lặp, tức là 5 bước. Việc gán giá trị ban đầu cho `current_number` và kiểm tra điều kiện kết thúc vòng lặp mỗi việc là một bước. 27 + 5 + 1 + 1 = 34 bước.

Vì vậy, có thể nói thuật toán của chúng ta cần 34 bước để hoàn thành.

Biết điều này rất hữu ích, nhưng thực tế nó không giúp ích nhiều cho việc so sánh các thuật toán. Để thấy lý do, hãy sửa đổi một chút thuật toán ban đầu để nhận vào một số thay vì đặt giá trị mặc định cố định là 10.

```ruby
def odd_numbers(max_number)
  current_number = 1

  while current_number < max_number
    if current_number % 2 != 0
      puts current_number
    end

    current_number += 1
  end
end
```

Thuật toán này cần bao nhiêu bước?

Có lẽ bạn đã nhận ra câu trả lời là: còn tùy. Nếu đặt `max_number` bằng 10 như trước, số bước là 34, nhưng nếu nhập một số khác thì số bước cũng thay đổi. Không có một con số cụ thể nào có thể dùng để đo hiệu quả của mã, vì số đó thay đổi theo đầu vào bên ngoài.

Điều chúng ta thực sự muốn đo là số bước của thuật toán thay đổi như thế nào khi dữ liệu thay đổi. Điều này giúp chúng ta trả lời câu hỏi liệu mã mình viết có thể mở rộng tốt hay không.

Để làm được vậy, chúng ta cần tìm hiểu một khái niệm mới: các ký hiệu tiệm cận và đặc biệt là Big O.

### Các ký hiệu tiệm cận

Các ký hiệu tiệm cận được dùng để mô tả thời gian chạy của một thuật toán. Vì thời gian chạy của thuật toán có thể khác nhau tùy thuộc vào đầu vào, có một số ký hiệu đo thời gian chạy theo những cách khác nhau. Ba ký hiệu phổ biến nhất là:

- Ký hiệu Big O - biểu diễn cận trên của một thuật toán. Điều này có nghĩa là trường hợp xấu nhất về cách thuật toán sẽ hoạt động.
- Ký hiệu Omega - biểu diễn cận dưới của một thuật toán. Đây là trường hợp tốt nhất.
- Ký hiệu Theta - biểu diễn cả cận trên lẫn cận dưới, từ đó phân tích độ phức tạp trong trường hợp trung bình của một thuật toán.

Big O là ký hiệu bạn thường thấy nhất, vì bạn cần bảo đảm trường hợp xấu nhất của mọi mã mình viết vẫn có thể mở rộng khi đầu vào trong ứng dụng tăng lên.

Cũng cần lưu ý rằng những ký hiệu được trình bày dưới đây cho Big O cũng áp dụng cho ký hiệu Omega và Theta. Điểm khác nhau nằm ở cách chúng đo hiệu quả của thuật toán, và do đó ký hiệu nào nên được áp dụng. Điều này sẽ trở nên rõ ràng hơn khi bạn đọc tiếp.

### Big O là gì?

Big O cung cấp một cách nhất quán để đo hiệu quả của một thuật toán. Nó cho biết thời gian một thuật toán cần để chạy khi đầu vào tăng lên, nhờ đó bạn có thể trực tiếp so sánh hiệu năng của hai thuật toán và chọn thuật toán tốt hơn.

Big O không phải là một đoạn mã mà bạn có thể đưa thuật toán vào để nhận được câu trả lời thuật toán hiệu quả đến đâu. Bạn cần đo số bước thay đổi như thế nào khi dữ liệu tăng lên, rồi dựa vào đó áp dụng một ký hiệu Big O và so sánh với các thuật toán khác. Trong nhiều trường hợp, bạn sẽ sử dụng một cấu trúc dữ liệu có những cách tương tác đã được biết rõ; khi đó, việc đánh giá khả năng mở rộng của nó khi đầu vào thay đổi sẽ dễ dàng hơn.

Trước hết, chúng ta sẽ tóm tắt các ký hiệu Big O, sau đó cung cấp thêm một chút bối cảnh cho từng ký hiệu. Tài liệu đọc thêm sẽ đi sâu hơn vào chi tiết.

#### Ký hiệu Big O

Các ký hiệu Big O phổ biến nhất, được sắp xếp theo tốc độ từ nhanh nhất đến chậm nhất, là:

- O(1) - Độ phức tạp hằng số
- O(log N) - Độ phức tạp logarit
- O(N) - Độ phức tạp tuyến tính
- O(N log N) - Độ phức tạp N x log N
- O(n&#178;) - Độ phức tạp bậc hai
- O(n&#179;) - Độ phức tạp bậc ba
- O(2&#8319;) - Độ phức tạp hàm mũ
- O(N!) - Độ phức tạp giai thừa

#### O(1) - độ phức tạp hằng số

Để hiểu độ phức tạp hằng số, hãy sử dụng một mảng.

```ruby
arr = [1, 2, 3, 4, 5]
```

Nếu muốn tra cứu phần tử ở chỉ mục 2, chúng ta có thể lấy phần tử đó bằng `arr[2]`, kết quả là `3`. Việc này chỉ mất một bước. Nếu tăng gấp đôi kích thước mảng...

```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Chúng ta vẫn có thể truy cập bất kỳ phần tử nào chỉ trong một bước. `arr[7]` cho ta `8` chỉ với một bước. Mảng có thể tiếp tục tăng lên nhưng chúng ta vẫn luôn truy cập được bất kỳ phần tử nào trong một bước. Đó là tính hằng số. Vì vậy, ta có `O(1)`.

Tra cứu một thứ gì đó trong một bước là mức tốt nhất có thể đạt được về độ phức tạp thời gian.

Khi xem dạng cơ bản của Big O, hãy lưu ý một điểm nhỏ dễ gây nhầm lẫn. Có thể lúc nãy bạn đã nghĩ: có thật sự chỉ là một bước không? Về mặt kỹ thuật thì không; trên thực tế, trước tiên máy tính phải tra cứu vị trí của mảng trong bộ nhớ, sau đó từ phần tử đầu tiên của mảng nhảy đến chỉ mục được cung cấp. Như vậy phải mất ít nhất vài bước. Vì thế, viết `O(1 + 2(steps))` cũng không sai. Tuy nhiên, 2 bước đó chỉ là những chi phí vụn vặt. Với một mảng có 10.000 phần tử, số bước vẫn giống như khi mảng chỉ có 2 phần tử. Vì lý do này, Big O không quan tâm đến những con số vụn vặt đó. Chúng không cho biết độ phức tạp tăng như thế nào khi kích thước dữ liệu thay đổi, vì chúng là hằng số và do đó bị loại bỏ trong Big O. Big O chỉ muốn cho chúng ta biết độ phức tạp của thuật toán tương quan với kích thước đầu vào.

Số bước có quan trọng không? Có thể có. Chúng ta sẽ nói về trường hợp đó sau một chút.

#### O(log N) - độ phức tạp logarit

Độ phức tạp logarit cho biết số bước mà thuật toán cần tăng thêm 1 khi dữ liệu tăng gấp đôi. Nếu suy nghĩ kỹ, đây vẫn là hiệu quả khá tốt. Từ 5.000 lên 10.000 phần tử dữ liệu mà chỉ cần thêm một bước có thể mở rộng rất tốt.

Một thuật toán hoạt động như vậy là tìm kiếm nhị phân. Nó chỉ hoạt động trên các mảng đã được sắp xếp, nhưng nếu có một mảng gồm 10 phần tử theo thứ tự:

```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Để kiểm tra xem mảng có chứa số `7` hay không, tìm kiếm nhị phân tính chỉ mục ở giữa bằng công thức:

```ruby
middle_index = (start_index + end_index) / 2
```

Ở đây, `start_index` là đầu mảng (ban đầu là `0`), còn `end_index` là phần tử cuối mảng (ban đầu là `9` đối với mảng 10 phần tử). Phần tử ở giữa, tại chỉ mục `4`, sẽ là `5`.

Vì mảng đã được sắp xếp và `7` lớn hơn `5`, chúng ta có thể loại bỏ `5` cùng mọi phần tử bên trái nó, vì tất cả những số đó đều nhỏ hơn `7`.

```ruby
arr = [-, -, -, -, -, 6, 7, 8, 9, 10]
```

Chỉ trong một bước, chúng ta đã loại bỏ một nửa mảng. Bây giờ, ta tính lại chỉ mục ở giữa bằng các giá trị `start_index` và `end_index` mới. Chỉ mục ở giữa lúc này sẽ là `7`, và số ở chỉ mục đó là `8`.

Vì `7` nhỏ hơn `8`, chúng ta có thể loại bỏ `8` cùng mọi phần tử bên phải nó.

```ruby
arr = [6, 7, -, -, -]
```

Chúng ta lặp lại quá trình này cho đến khi mảng chỉ còn một phần tử. Nếu phần tử đó trùng với số cần tìm, ta đã tìm thấy nó. Nếu không, số đó không có trong mảng.

Bảng dưới đây tóm tắt việc kích thước mảng tăng gấp đôi và số bước, theo ký hiệu Big O, cần thực hiện để còn lại một phần tử nhằm kiểm tra xem nó có khớp với giá trị cần tìm hay không:

| Kích thước | Số bước |
| ---- | ----- |
| 1    | 1     |
| 2    | 2     |
| 4    | 3     |
| 8    | 4     |
| 16   | 5     |
| 32   | 6     |

Khá ấn tượng phải không!

#### O(N) - độ phức tạp tuyến tính

Khái niệm này khá dễ nắm bắt. Độ phức tạp tuyến tính chỉ cho biết rằng khi số lượng phần tử tăng, số bước cũng tăng với đúng cùng một tốc độ. Mỗi lần duyệt qua một mảng là một ví dụ về độ phức tạp tuyến tính. Nếu có một mảng gồm 5 phần tử, chúng ta có thể duyệt qua mọi phần tử trong 5 bước. Mảng gồm 10 phần tử có thể được duyệt trong 10 bước. Nếu gặp một thuật toán có hiệu quả Big O là `O(N)`, bạn biết rằng số bước sẽ tăng tương ứng với số phần tử trong cấu trúc dữ liệu.

#### O(N log N) - độ phức tạp N x log N

Khó có thể nói tên gọi của khái niệm này không phù hợp. Ký hiệu này có nghĩa là chúng ta có một thuật toán ban đầu là `O(log N)`, chẳng hạn ví dụ tìm kiếm nhị phân ở trên, trong đó mảng liên tục bị chia đôi; nhưng với `O(N log N)`, mỗi nửa mảng đó lại được xử lý bởi một thuật toán khác có độ phức tạp `O(N)`.

Một thuật toán như vậy là thuật toán sắp xếp trộn trong bài học trước của chúng ta. :)

#### O(n&#178;) - độ phức tạp bậc hai

Có lẽ trong quá trình học lập trình, bạn đã từng viết mã có độ phức tạp bậc hai. Nó thường xuất hiện khi bạn lặp qua một tập dữ liệu và bên trong mỗi vòng lặp lại lặp qua tập dữ liệu đó một lần nữa.
Ví dụ, nếu mảng có 3 phần tử, các vòng lặp lồng nhau cần 3&#178; = 9 bước con. Chỉ cần thêm một phần tử vào mảng, con số này gần như tăng gấp đôi thành 4&#178; = 16. Thêm phần tử thứ 5, ta có 5&#178; = 25 bước con. Sau đó, tăng gấp đôi kích thước mảng lên 10 phần tử sẽ làm số bước con tăng từ 25 lên 100, tức là cần nhiều công việc gấp 4 lần!
Hy vọng bạn đã thấy được vấn đề đang dẫn đến đâu...

#### O(n&#179;) - độ phức tạp bậc ba

Hãy nghĩ đến các vòng lặp lồng nhau ba tầng. Nếu lặp qua một mảng có n phần tử, thêm 1 phần tử sẽ làm tăng thêm một vòng lặp bên ngoài, một vòng lặp ở giữa và một vòng lặp trong cùng. Khi sử dụng các vòng lặp lồng nhau ba tầng như vậy trên một mảng kích thước n, chúng ta cần tổng cộng n&#179; bước con.
Ví dụ, nếu mảng có 3 phần tử, các vòng lặp lồng nhau ba tầng cần tổng cộng 3&#179; = 27 bước con. Thêm một phần tử sẽ làm con số này tăng hơn gấp đôi thành 4&#179; = 64 bước con. Với 5 phần tử, nhiệm vụ lại gần như tăng gấp đôi, thành 5&#179; = 125 bước con. Tăng gấp đôi kích thước mảng lên 10 phần tử nghĩa là cần tổng cộng 10&#179; = 1000 bước con, gấp 8 lần trước đó! Mảng có 100 phần tử cần tổng cộng 1.000.000 bước con. Thật khủng khiếp!

#### O(2&#8319;) - độ phức tạp hàm mũ

Độ phức tạp hàm mũ có nghĩa là với mỗi phần tử thêm vào kích thước dữ liệu, số bước sẽ tăng gấp đôi so với số bước trước đó. Hãy xem một bảng nhỏ để thấy tình hình có thể nhanh chóng vượt khỏi tầm kiểm soát như thế nào.

| Kích thước | Số bước |
| ---- | ----- |
| 1    | 2     |
| 2    | 4     |
| 3    | 8     |
| 4    | 16    |
| 5    | 32    |
| 6    | 64    |
| 7    | 128   |
| 8    | 256   |
| 9    | 512   |
| 10   | 1024  |

Bạn nên tránh độ phức tạp này nếu có thể, nếu không bạn sẽ không thể xử lý nhiều dữ liệu một cách nhanh chóng.

#### O(N!) - độ phức tạp giai thừa

Giai thừa của một số là tích của tất cả các số từ 1 đến số đó. Ví dụ, giai thừa của 4 (viết là 4!) là 4 \* 3 \* 2 \* 1.

Bạn sẽ gặp độ phức tạp giai thừa nếu cần tính các hoán vị hoặc tổ hợp. Nếu có một mảng và phải tính tất cả các tổ hợp có thể tạo ra từ mảng đó, thì đó là độ phức tạp giai thừa. Nó vẫn có thể xử lý được với số lượng phần tử nhỏ, nhưng mỗi phần tử mới trong tập dữ liệu có thể làm công việc tăng vọt.

Giai thừa của 3 là 6 (3 \* 2 \* 1). Giai thừa của 4 là 24. Còn giai thừa của 10? Là 3.628.800. Như vậy, bạn có thể thấy mọi thứ nhanh chóng vượt khỏi tầm kiểm soát đến mức nào.

### Các lựa chọn thay thế Big O

Nếu Big O cho chúng ta biết trường hợp xấu nhất về khả năng mở rộng của thuật toán, thì có những lựa chọn thay thế nào?

#### Big &#937; (ký hiệu omega)

Ký hiệu Omega cho biết trường hợp tốt nhất của một thuật toán. Để hiểu trường hợp này có thể xảy ra ở đâu, hãy xem một phương thức và thảo luận cách đo độ phức tạp của nó.

```ruby
def find_value(arr)
  arr.each do |item|
    return item if item == 1
  end
end
```

Trong trường hợp xấu nhất (Big O), xảy ra khi phần tử không có trong mảng hoặc là phần tử cuối cùng trong mảng, thuật toán có độ phức tạp tuyến tính `O(N)`. Điều này xảy ra vì trong cả hai trường hợp, mã phải duyệt qua mọi phần tử. Khi kích thước đầu vào tăng gấp đôi, số lần lặp cần thiết trong trường hợp xấu nhất cũng tăng gấp đôi.

Tuy nhiên, trong trường hợp tốt nhất, giá trị chúng ta tìm sẽ là phần tử đầu tiên trong mảng. Khi đó, thuật toán chỉ cần một bước. Độ phức tạp là `Ω(1)`. Đây là độ phức tạp Omega của nó.

Ký hiệu Omega không được xem là hữu ích bằng, vì không chắc phần tử cần tìm thường xuyên là phần tử đầu tiên trong cấu trúc dữ liệu; do đó, ký hiệu này không cho chúng ta biết thuật toán sẽ mở rộng tốt đến đâu.

#### Big-Θ (ký hiệu big-theta)

Trong khi ký hiệu Omega đo trường hợp tốt nhất về hiệu quả của thuật toán và Big O đo trường hợp xấu nhất, Theta cố gắng đưa ra giá trị chính xác hoặc một khoảng hữu ích giữa cận trên và cận dưới đủ hẹp.

Nếu có một đoạn mã lặp qua mọi phần tử trong một mảng, thì kích thước mảng không quan trọng. Thuật toán của chúng ta sẽ luôn chạy trong thời gian `O(N)` ở cả trường hợp tốt nhất và xấu nhất. Khi đó, chúng ta biết hiệu năng chính xác của nó trong mọi trường hợp là `Θ(N)`, và đó là hiệu năng Theta của thuật toán. Với những thuật toán khác, Theta có thể biểu diễn cả cận dưới và cận trên của một thuật toán có độ phức tạp khác nhau. Chúng ta sẽ không đi sâu hơn ở đây, vì Big O là ký hiệu chính được dùng cho độ phức tạp thời gian của thuật toán nói chung.

Đây chỉ là cách giải thích đơn giản nhằm giúp chủ đề dễ tiếp cận hơn. Nếu bạn có thiên hướng về toán học, một tìm kiếm nhanh trên mạng sẽ giúp bạn tìm thấy những lời giải thích chi tiết hơn.

### Vì sao dùng Big O

Sau khi đã tìm hiểu các cách khác nhau để định lượng hiệu quả của một thuật toán, hy vọng bạn đã hiểu rõ vì sao chúng ta chọn trường hợp xấu nhất khi đo hiệu quả của thuật toán đó.

Dùng trường hợp xấu nhất giúp chúng ta bảo đảm thuật toán có thể mở rộng trong mọi kết quả. Nếu viết một thuật toán có thể chạy trong thời gian hằng số, nhưng trong trường hợp xấu nhất cũng có thể chạy trong thời gian tuyến tính, nó chỉ có thể mở rộng khi đầu vào tăng nếu vẫn hoạt động được khi trường hợp xấu nhất xảy ra. Bạn cần tin chắc rằng mã của mình sẽ không bị treo và khiến người dùng bực bội nếu đột nhiên nhận đầu vào gồm một triệu phần tử thay vì 10 phần tử.

### Các thuật toán có cùng độ phức tạp

Nếu viết hai thuật toán có cùng độ phức tạp, điều đó có nghĩa là chúng tốt như nhau để sử dụng sao? Chúng ta sẽ trả lời câu hỏi này bằng hai ví dụ mã, sau đó thảo luận thêm để cố gắng giải đáp.

Ví dụ đầu tiên là đoạn mã chúng ta đã thấy, phương thức `odd_numbers`.

```ruby
def odd_numbers(max_number)
  current_number = 1

  while current_number < max_number
    if current_number % 2 != 0
      puts current_number
    end

    current_number += 1
  end
end
```

Độ phức tạp thời gian của thuật toán này là `O(N)`. Khi kích thước dữ liệu tăng, số bước của thuật toán cũng tăng với cùng tốc độ.

Hãy xem một phiên bản khác:

```ruby
def odd_numbers(max_number)
  current_number = 1

  while current_number < max_number
    if current_number % 2 != 0
      puts current_number
    end

    current_number += 2
  end
end
```

Không có nhiều thay đổi, nhưng lần này chúng ta tăng `current_number` lên 2. Điều này ảnh hưởng thế nào đến thời gian chạy của thuật toán? Với đầu vào là `n`, số bước xấp xỉ bằng một nửa vì mỗi lần lặp tăng thêm 2. Đây là thuật toán `O(N/2)`, nhưng như đã đề cập trước đó, chúng ta không cố tìm thời gian chính xác mà quan tâm đến cách thời gian tăng tương quan với kích thước đầu vào. Big O không quan tâm đến các hằng số, vì chúng không phản ánh cách thuật toán mở rộng khi đầu vào thay đổi; hơn nữa, sẽ không thú vị hoặc dễ dàng gì nếu phải so sánh thuật toán `O(N/2 + 5 N)` với `O(N + 5 / 2N)`. Vì vậy, hiệu quả Big O của cả hai thuật toán đều là `O(N)`. Chúng mở rộng với cùng tốc độ khi đầu vào tăng.

Một cách khác để nhìn nhận là các hằng số cuối cùng sẽ trở nên không đáng kể. Hãy xem các trường hợp sau:

N=1, O(10N) = 10, O(n&#178;) = 1

N = 5, O(10N) = 50, O(n&#178;) = 25

N = 100, O(10N) = 1,000, O(n&#178;) = 10,000 *lớn hơn 10 lần*

N = 1000, O(10N) = 10,000, O(n&#178;) = 1,000,000 *lớn hơn 100 lần*

N = 10000, O(10N) = 100,000, O(n&#178;) = 100,000,000 *lớn hơn 1000 lần*

Do đó, O(10N) nhanh hơn O(n²) khi N bằng 100. Trên thực tế, đôi khi một thuật toán n² nhanh hơn một thuật toán N với một số lượng đầu vào nhỏ, như có thể thấy trong hai ví dụ đầu tiên ở trên.

Hãy nhớ bảo đảm mã bạn viết hiệu quả nhất có thể trong phạm vi độ phức tạp thời gian của nó.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Hướng dẫn về ký hiệu Big-O dành cho Rubyist](https://www.honeybadger.io/blog/a-rubyist-s-guide-to-big-o-notation/). Phần cuối nói về việc áp dụng nội dung này cho các truy vấn cơ sở dữ liệu trong một ứng dụng web; bạn chưa học phần đó, nhưng hãy ghi nhớ khi tiếp tục sang các tài liệu về Rails.
1. [Bảng tra cứu Big-O](https://www.bigocheatsheet.com/) là một tài nguyên tuyệt vời. Bảng này cung cấp biểu đồ độ phức tạp để bạn có thể thấy các thuật toán khác nhau hoạt động như thế nào khi kích thước dữ liệu tăng, đồng thời cho biết độ phức tạp thời gian của những thao tác phổ biến trên cấu trúc dữ liệu cũng như các thuật toán sắp xếp phổ biến.
1. [Hướng dẫn về độ phức tạp thời gian dành cho Ruby Developer](https://www.rubyguides.com/2018/03/time-complexity-for-ruby-developers/) cung cấp thêm góc nhìn về cách áp dụng ký hiệu Big-O cho mã của chính bạn.

</div>

### Tài nguyên bổ sung

Phần này chứa các liên kết hữu ích đến nội dung liên quan. Đây không phải phần bắt buộc, vì vậy hãy xem nó như tài liệu bổ sung.

- Đây không phải là tài nguyên miễn phí, nhưng [Hướng dẫn dễ hiểu về cấu trúc dữ liệu và thuật toán](https://pragprog.com/titles/jwdsal2/a-common-sense-guide-to-data-structures-and-algorithms-second-edition/) đã làm rất tốt việc trình bày các chủ đề này theo cách dễ tiếp cận với những người chưa quen với một số thuật ngữ toán học được sử dụng.

- Trong video [Giới thiệu về ký hiệu Big O và độ phức tạp thời gian](https://www.youtube.com/watch?v=D6xkbGLQesk), tác giả trình bày quy trình từng bước để phân tích và hiểu độ phức tạp thời gian của nhiều thuật toán khác nhau.