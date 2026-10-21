### Giới thiệu

Chúng ta đã nhắc đến thuật ngữ **ES6** từ những bài học đầu tiên, nhưng chưa dành thời gian giải thích đúng nghĩa của nó hoặc tìm hiểu những ảnh hưởng của nó trong code của mình.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Hệ thống phát hành của ECMAScript.
- Một số vấn đề tiềm ẩn với việc hỗ trợ các tính năng JavaScript mới hơn trên trình duyệt.
- Babel.

### ECMAScript là gì?

JavaScript chỉ là một ngôn ngữ lập trình tuân theo tiêu chuẩn ECMAScript, được tạo ra bởi Ecma International, và ES6 là một phiên bản JavaScript được phát hành chính thức vào mùa hè năm 2015. Nó bao gồm *nhiều* tính năng mới giúp việc viết JavaScript dễ dàng và rõ ràng hơn. Trong curriculum này, bạn đã học nhiều tính năng mới đó vì, nói cho cùng, ES6 *chính là JavaScript*.

#### Lịch phát hành

Có lẽ bạn cũng đã gặp những bài viết nói về các tính năng trong ES7, ES8, ES2015 hoặc ES2017, v.v. Một phần nguyên nhân gây nhầm lẫn là ngay sau khi ES6 được phát hành, ủy ban đưa ra các quyết định này (được gọi là TC39) đã đổi cách đặt tên từ số phiên bản (ES5, ES6, ES7, v.v.) sang năm phát hành. ES6 còn được gọi là ES2015, và thứ mà một số bài viết gọi là ES7 còn được gọi là ES2016, v.v. Thay vì một phiên bản mới duy nhất chứa rất nhiều tính năng mới (như trường hợp ES6 và các phiên bản trước đó), hiện nay các phiên bản được phát hành hằng năm với một tập hợp bổ sung nhỏ hơn trong mỗi phiên bản.

### Hỗ trợ trình duyệt

*Vấn đề* với việc JavaScript liên tục cập nhật và bổ sung tính năng là đôi khi các trình duyệt cần thời gian để bắt kịp và triển khai những tính năng mới sau khi chúng được phát hành. Một tính năng mới thường cần vài năm để trở nên "được hỗ trợ rộng rãi" (được hỗ trợ trên phần lớn các trình duyệt hiện đại và phiên bản trình duyệt đang được sử dụng). Điều này không may có nghĩa là nếu bạn viết code sử dụng các tính năng hoàn toàn mới, nó **sẽ không chạy** trên những trình duyệt không hỗ trợ tính năng đó.

Với phần lớn chúng ta, đây không phải vấn đề vì gần như chắc chắn bạn đang dùng một trình duyệt mới tự động cập nhật khi có phiên bản mới. Nhưng trong thế giới thực, nếu bạn bán sản phẩm cho khách hàng, bạn không thể kiểm soát người dùng sẽ dùng trình duyệt nào để kết nối với site của mình.

#### Babel

May mắn là *có* một giải pháp cho vấn đề này. [Babel](http://babeljs.io/) là một công cụ nhận code JavaScript hiện đại của bạn và **transpiles** nó thành code mà các trình duyệt cũ hơn có thể hiểu. Nó có thể được thiết lập để transpile nhằm đáp ứng bất kỳ số lượng target nào (ví dụ: phiên bản trình duyệt tối thiểu trên nhiều trình duyệt), trong đó các target cần thiết phụ thuộc vào những yếu tố như mục đích của product, đối tượng mục tiêu, v.v. Một số product có thể chỉ cần hỗ trợ các trình duyệt mới hơn, trong khi những product khác có thể cần nghiêm ngặt hơn nhiều và bảo đảm hỗ trợ các trình duyệt cũ hơn.

Thành thật mà nói, đây *không phải* điều bạn sẽ cần lo lắng trong mọi project bắt đầu thực hiện, vì mục đích của các project trong curriculum là có trải nghiệm thực tế chứ không phải giao sản phẩm ở cấp độ production cho khách hàng. Tuy vậy, hiểu được tình huống này và những điều có thể cần thiết trong thế giới thực vẫn rất hữu ích.

### Assignment

<div class="lesson-content__panel" markdown="1">

1. Để thỏa mãn trí tò mò, hãy xem tất cả [tính năng mới xuất hiện trong ES6 (hay còn gọi là ES2015)](https://github.com/lukehoban/es6features). Dù bạn đã sử dụng nhiều tính năng trong số đó, vẫn có một vài tính năng chúng ta chưa đề cập cụ thể (bạn chưa cần tìm hiểu những tính năng này ngay).
1. Hãy xem nhanh [timeline của các phiên bản ECMAScript khác nhau](https://en.wikipedia.org/wiki/ECMAScript_version_history#14th_Edition_%E2%80%93_ECMAScript_2023), trong đó có các tóm tắt ngắn về những phiên bản hằng năm kể từ ES6. Một lần nữa, có những thứ bạn có thể đã sử dụng và những thứ khác bạn sẽ chưa nhận ra hoặc chưa cần tìm hiểu.

</div>