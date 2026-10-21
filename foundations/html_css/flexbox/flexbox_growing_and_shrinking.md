### Giới thiệu

Hãy xem kỹ hơn điều thực sự xảy ra khi bạn đặt `flex: 1` cho các flex item trong bài học trước.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài học này.

- Bạn sẽ học 3 thuộc tính được định nghĩa bởi dạng viết tắt `flex`, cũng như cách sử dụng từng thuộc tính riêng lẻ.

### Dạng viết tắt flex

Khai báo `flex` thực chất là dạng viết tắt của 3 thuộc tính có thể đặt trên một flex item. Những thuộc tính này ảnh hưởng đến cách flex item tự điều chỉnh kích thước bên trong container. Bạn đã từng thấy một số thuộc tính dạng viết tắt, nhưng chúng ta chưa chính thức định nghĩa chúng.

> Thuộc tính dạng viết tắt là các thuộc tính CSS cho phép bạn đồng thời đặt giá trị cho nhiều thuộc tính CSS khác. Nhờ dùng dạng viết tắt, bạn có thể viết stylesheet ngắn gọn hơn (và thường dễ đọc hơn), tiết kiệm thời gian và công sức.
>
> Nguồn: [Thuộc tính dạng viết tắt trên MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties)

Trong trường hợp này, `flex` thực chất là dạng viết tắt của `flex-grow`, `flex-shrink` và `flex-basis`.

![flex shorthand](https://cdn.statically.io/gh/TheOdinProject/curriculum/0cc6b26bb0c4b94524369d327c97a8fb11e83b6b/foundations/html_css/flexbox/imgs/10.png)

Trong ảnh chụp màn hình trên, `flex: 1` tương đương với: `flex-grow: 1`, `flex-shrink: 1`, `flex-basis: 0`.

#### Flex-grow

`flex-grow` nhận một số duy nhất làm giá trị, và số đó được dùng làm "hệ số tăng trưởng" của flex item. Khi áp dụng `flex: 1` cho mọi div bên trong container, ta yêu cầu mọi div nở ra cùng một mức. Kết quả là mọi div có kích thước hoàn toàn bằng nhau. Nếu thay vào đó chỉ thêm `flex: 2` cho một div, div đó sẽ nở lên gấp 2 lần kích thước của các div còn lại.

Trong ví dụ sau, dạng viết tắt `flex` có các giá trị của `flex-shrink` và `flex-basis` được chỉ định theo giá trị mặc định của chúng.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="YzQqvgK" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/YzQqvgK">
  flex-grow example</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

#### Flex-shrink

`flex-shrink` tương tự `flex-grow`, nhưng đặt "hệ số co lại" của một flex item. `flex-shrink` chỉ được áp dụng khi tổng kích thước của các flex item lớn hơn container cha. Ví dụ, nếu 3 div ở trên có khai báo chiều rộng như `width: 100px`, còn `.flex-container` nhỏ hơn `300px`, các div sẽ phải co lại để vừa với container.

Hệ số co mặc định là `flex-shrink: 1`, nghĩa là mọi item sẽ co đều nhau. Nếu *không* muốn một item co lại, bạn có thể chỉ định `flex-shrink: 0;`. Bạn cũng có thể chỉ định các số lớn hơn để khiến một số item co nhanh hơn bình thường.

Đây là một ví dụ. Lưu ý rằng chúng ta cũng đã thay đổi `flex-basis` vì những lý do sẽ được giải thích ngay sau đây. Nếu thu nhỏ cửa sổ trình duyệt, bạn sẽ thấy `.two` không bao giờ nhỏ hơn chiều rộng 250px đã cho, dù quy tắc `flex-grow` vốn sẽ khiến mọi phần tử có kích thước bằng nhau.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="JjJXZVz" data-editable="true" data-user="TheOdinProjectExamples" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">

  <span>See the Pen <a href="https://codepen.io/TheOdinProjectExamples/pen/JjJXZVz">
  flex-shrink example</a> by TheOdinProject (<a href="https://codepen.io/TheOdinProjectExamples">@TheOdinProjectExamples</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>

</p>

<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

Một hệ quả quan trọng cần lưu ý là khi chỉ định `flex-grow` hoặc `flex-shrink`, flex item không nhất thiết tôn trọng giá trị `width` bạn đưa ra. Trong ví dụ trên, cả 3 div đều có chiều rộng 250px, nhưng khi phần tử cha đủ lớn, chúng sẽ nở ra để lấp đầy nó. Tương tự, khi phần tử cha quá nhỏ, hành vi mặc định là chúng co lại cho vừa. Đây không phải lỗi, nhưng có thể gây khó hiểu nếu bạn không lường trước.

#### Flex-basis

`flex-basis` đặt kích thước ban đầu của một flex item, vì vậy mọi quá trình `flex-grow` hoặc `flex-shrink` đều bắt đầu từ kích thước cơ sở đó. Giá trị mặc định của dạng viết tắt là `flex-basis: 0%`. Ta phải đổi nó thành `auto` trong ví dụ `flex-shrink` vì khi cơ sở là `0`, các item sẽ bỏ qua chiều rộng của mình và mọi thứ co đều nhau. Dùng `auto` làm flex-basis sẽ yêu cầu item kiểm tra xem có khai báo chiều rộng nào không (`width: 250px`).

<div class="lesson-note" markdown="1">

#### Lưu ý quan trọng về flex-basis

Có sự khác nhau giữa giá trị mặc định của `flex-basis` và cách dạng viết tắt `flex` định nghĩa nó khi không cung cấp `flex-basis`. Giá trị mặc định thực sự của `flex-basis` là `auto`, nhưng khi bạn chỉ định `flex: 1` cho một phần tử, nó được hiểu là `flex: 1 1 0`. Nếu *chỉ* muốn điều chỉnh `flex-grow` của một item, bạn có thể đặt trực tiếp thuộc tính đó mà không dùng dạng viết tắt. Hoặc bạn có thể viết đầy đủ dạng viết tắt 3 giá trị là `flex: 1 1 auto`, cũng tương đương với `flex: auto`.

</div>

#### Flex auto là gì?

Nếu để ý, bạn sẽ thấy chúng ta đã nhắc đến dạng viết tắt flex mới `flex: auto` trong lưu ý trước. Tuy nhiên, chúng ta chưa giới thiệu đầy đủ về nó. `flex: auto` là một trong các dạng viết tắt của flex. Khi `auto` được định nghĩa như một từ khóa flex, nó tương đương với các giá trị `flex-grow: 1`, `flex-shrink: 1` và `flex-basis: auto`, hoặc tương đương với `flex: 1 1 auto` khi dùng dạng viết tắt flex. Lưu ý rằng `flex: auto` không phải giá trị mặc định khi dùng dạng viết tắt flex, dù tên là "auto", nên ban đầu có thể hơi gây nhầm lẫn. Bạn sẽ gặp và tìm hiểu thêm về `flex: auto` cùng các trường hợp sử dụng trong phần bài tập.

#### Trong thực tế

Trong thực tế, có lẽ bạn sẽ không dùng các giá trị phức tạp cho `flex-grow`, `flex-shrink` hoặc `flex-basis`. Thông thường, bạn sẽ dùng những khai báo như `flex: 1;` để các div nở đều và `flex-shrink: 0` để giữ cho một số div không bị co.

Bạn *có thể* tạo những bố cục cầu kỳ hơn, trong đó một số cột liên hệ với nhau theo một tỷ lệ cụ thể. Vì vậy, biết rằng bạn có thể dùng các giá trị khác cũng hữu ích, dù những trường hợp đó tương đối hiếm.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [W3C: "7.1.1 Basic Values of 'flex'"](https://www.w3.org/TR/css-flexbox-1/#flex-common) để hiểu các giá trị cơ bản của những dạng viết tắt flex thường dùng.
1. [Tài liệu của MDN về flex](https://developer.mozilla.org/en-US/docs/Web/CSS/flex) tóm tắt toàn bộ các giá trị dạng viết tắt flex, đồng thời giới thiệu một số cú pháp mới chưa được đề cập trong bài viết trước.

</div>