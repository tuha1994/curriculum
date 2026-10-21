### Giới thiệu

Bạn có thể nghĩ phần lớn công việc của một lập trình viên là viết mã. Tuy nhiên, trên thực tế, chúng ta dành rất nhiều thời gian để *đọc* mã. Đó có thể là mã do thành viên khác trong nhóm viết, mã do những người không còn ở trong nhóm viết, hoặc thậm chí là mã bạn đã tự viết từ hai tuần trước nhưng giờ không còn nhớ rõ.

Đừng nghĩ rằng bạn phải lập tức thành thạo những nguyên tắc này. Ai cũng đôi lúc viết mã lộn xộn, kể cả các lập trình viên chuyên nghiệp. Mục tiêu ở đây là đưa ra một số hướng dẫn giúp bạn dần cải thiện khả năng đọc hiểu mã. Bạn càng viết nhiều, mã của bạn sẽ càng tốt hơn, cả về khả năng đọc hiểu lẫn những khía cạnh khác.

Hãy thử các ý tưởng này và tìm cách đưa chúng vào cách suy nghĩ khi viết mã, nhưng đừng tự trách vì mình chưa viết được mã tao nhã, rõ ràng đến từng chi tiết. Hãy tập trung vào sự tiến bộ từng bước thay vì sự hoàn hảo.

Vậy là xong phần mở đầu, hãy bắt đầu!

### Tổng quan bài học

Phần này khái quát các chủ đề bạn sẽ học trong bài.

- Biết phân biệt mã khó đọc với mã dễ đọc.
- Áp dụng các nguyên tắc lập trình để làm mã sạch hơn.
- Viết chú thích tốt.

### Mã sạch là gì?

Hãy xem các ví dụ sau:

Ví dụ A:

```javascript
const x= function (z){
   const w = "Hello ";
return w +  z

 }

x("John");
```

Ví dụ B:

```javascript
const generateUserGreeting = function (name) {
  const greeting = "Hello ";
  return greeting + name;
};

generateUserGreeting("John");
```

Bạn thấy ví dụ nào dễ đọc hơn? Rõ ràng ngay lập tức là ví dụ sau có ý nghĩa hơn. Điều đáng ngạc nhiên là cả hai hàm đều thực hiện chính xác cùng một nhiệm vụ (theo chính xác cùng một cách!), và cả hai đều là mã hợp lệ. Nhưng tại sao ví dụ thứ hai lại dễ đọc hơn nhiều?

Trong ví dụ đầu tiên, các biến chỉ có một chữ cái được sử dụng, còn thụt lề và khoảng cách thì không nhất quán. Kết quả là một đoạn mã khó hiểu và lộn xộn.

Hãy tưởng tượng bạn đang cộng tác trong một dự án với người đã viết hàm đầu tiên. Bạn sẽ mất bao lâu để giải mã xem nó đang làm gì rồi tiếp tục công việc? Hoặc có thể chính bạn đã viết nó từ lâu và hoàn toàn quên mất nó tồn tại. Trong cả hai tình huống, cuối cùng bạn vẫn sẽ hiểu chuyện gì đang diễn ra, nhưng quá trình đó sẽ chẳng thú vị gì.

Ví dụ B thể hiện mã sạch hơn. Có thể bạn chưa biết từng phần làm gì, nhưng việc đoán xem điều gì đang xảy ra dễ hơn nhiều vì tên hàm và biến rất rõ ràng. Cách thụt lề và khoảng cách tuân theo một khuôn mẫu nhất quán, hợp lý.

Bạn có thể dùng ký tự đơn làm tên biến trong ngữ cảnh của vòng lặp hoặc hàm callback, nhưng nên tránh dùng chúng ở những nơi khác.

#### Về camelCase

camelCase là quy ước đặt tên cho phép viết nhiều từ liền nhau mà không có khoảng trắng hay dấu câu. Trong camelCase, khi tên biến gồm nhiều từ như ví dụ `setTimeout` của chúng ta, từ đầu tiên được viết hoàn toàn bằng chữ thường, còn chữ cái đầu của từ thứ hai (và mọi từ tiếp theo) được viết hoa.

Trong suốt bài học này, phần lớn biến và hàm sẽ được đặt tên theo camelCase. Không phải ngôn ngữ nào cũng dùng quy ước này, nhưng nó rất phổ biến trong JavaScript nên đây là một quy ước tốt để noi theo.

<div class="lesson-note" markdown="1">

#### Quy ước chỉ là quy ước

Bài học này đưa ra một số ví dụ về cách làm cho mã gọn gàng hơn, nhưng trên thực tế, mỗi tổ chức sẽ có những cách tiếp cận cụ thể khác nhau; một số cách có thể hơi khác với ví dụ trong bài. Không có gì là tuyệt đối.

Điều quan trọng nhất là tất cả những cách tiếp cận đó đều phục vụ cùng một mục đích: cải thiện khả năng đọc hiểu và bảo trì mã. Cho đến khi cần tuân theo một bộ quy ước cụ thể, việc chọn một quy ước nào đó và sử dụng nó nhất quán là hợp lý.

</div>

### Đặt tên hàm và biến

Trong ví dụ đầu tiên, chúng ta đã đề cập đến tầm quan trọng của việc đặt tên *có ý nghĩa*. Hãy phân tích kỹ hơn xem một tên biến hoặc tên hàm tốt cần có gì.

#### Tên tốt phải mang tính mô tả

Trong ví dụ tốt, chúng ta có một biến `greeting`, được nối với tham số `name`. Hàm có tên `generateUserGreeting` và thực hiện đúng điều mà tên gọi gợi ý. Gọn gàng, sạch sẽ và dễ hiểu.

Bây giờ, hãy thử hình dung một cuộc trò chuyện với ai đó về ví dụ tệ. Hàm tên là `x`, với các biến như `z` và `w`. Thật không ổn chút nào.

#### Sử dụng từ vựng nhất quán

Các biến cùng loại lý tưởng nhất nên tuân theo một hệ thống đặt tên nhất quán. Hãy xem các ví dụ sau từ một trò chơi:

```javascript
// Consistent naming
function getPlayerScore();
function getPlayerName();
function getPlayerTag();
```

Tất cả đều tuân theo cùng một hệ thống đặt tên kiểu “get một thứ gì đó”. Bây giờ hãy xem ví dụ sau:

```javascript
// Inconsistent naming
function getUserScore();
function fetchPlayerName();
function retrievePlayer1Tag();
```

Trong ví dụ không nhất quán, ba động từ khác nhau được dùng cho các hàm. Dù chúng có ý nghĩa gần giống nhau, thoạt nhìn bạn có thể cho rằng việc dùng các động từ khác nhau là có lý do cụ thể (chẳng hạn, trong một số ngữ cảnh, “get” có thể *không hoàn toàn* giống “fetch”). Ngoài ra, `User`, `Player` và `Player1` khác nhau ở điểm nào? Nếu không có khác biệt thì tốt nhất nên dùng cùng một tên, chẳng hạn `Player`. Sự nhất quán tạo ra khả năng dự đoán.

Tên biến tốt nhất nên bắt đầu bằng danh từ hoặc tính từ (tức là một cụm danh từ), vì chúng thường đại diện cho “sự vật”, dù đó là chuỗi, số hay thứ gì khác. Hàm đại diện cho hành động nên lý tưởng là bắt đầu bằng một động từ.

```javascript
// Preferable
const numberOfThings = 10;
const myName = "Thor";
const selected = true;

// Not preferable (these start with verbs, could be confused for functions)
const getCount = 10;
const showNorseGods = ["Odin", "Thor", "Loki"];

// Preferable
function getCount() {
  return numberOfThings;
}

// Not preferable (myName doesn't represent some kind of action)
function myName() {
  return "Thor";
}
```

### Dùng những tên có thể tìm kiếm và dễ hiểu ngay

Đôi khi chúng ta dễ bị cám dỗ dùng “giá trị ma thuật”, tức các giá trị cụ thể như số hoặc chuỗi đứng riêng. Hãy xem lại một ví dụ:

```javascript
setTimeout(stopTimer, 3600000);
```

Vấn đề rất rõ ràng. Con số ma thuật `3600000` có nghĩa là gì, và bộ hẹn giờ này sẽ đếm trong bao lâu trước khi thực thi `stopTimer`? Ngay cả khi biết JavaScript biểu diễn thời gian bằng mili giây, có lẽ bạn vẫn phải dùng máy tính hoặc Google để tính xem nó tương đương bao nhiêu giây hay phút.

Bây giờ, hãy làm cho mã này có ý nghĩa hơn bằng cách đưa vào một biến mang tính mô tả:

```javascript
const ONE_HOUR = 3600000; // Can even write as 60 * 60 * 1000;

setTimeout(stopTimer, ONE_HOUR);
```

Tốt hơn nhiều, đúng không? Biến được khai báo bằng một tên mô tả rõ ràng, và bạn không cần thực hiện phép tính nào khi đọc mã.

Bạn có thể thắc mắc tại sao biến này lại được khai báo bằng chữ in hoa toàn bộ trong khi trước đó chúng ta khuyến nghị camelCase. Đây là quy ước dùng khi lập trình viên hoàn toàn chắc chắn rằng biến đó *thực sự* là một hằng số, đặc biệt khi nó đại diện cho một khái niệm như một khoảng thời gian cụ thể. Chúng ta biết số mili giây trong một giờ sẽ không bao giờ thay đổi, nên dùng như vậy là phù hợp. Hãy nhớ rằng đây chỉ là một quy ước. Không phải ai cũng nhất thiết làm mọi việc giống nhau.

### Thụt lề và độ dài dòng

Bây giờ là lúc chuyển sang những chủ đề “gây tranh cãi” hơn (có một câu đùa về [cuộc chiến giữa các lập trình viên thụt lề bằng dấu cách và bằng tab](https://www.reddit.com/r/programming/comments/p1j1c/tabs_vs_spaces_vs_both/)).

Điều thực sự quan trọng là *tính nhất quán*. Hãy chọn một cách thụt lề rồi kiên trì dùng nó. Nhiều hướng dẫn phong cách JavaScript đề xuất các lựa chọn khác nhau, và không có lựa chọn nào thực sự vượt trội hơn lựa chọn khác. Chúng ta sẽ tìm hiểu kỹ hơn về các hướng dẫn phong cách và công cụ liên quan ở phần sau của chương trình học.

#### Độ dài dòng

Một lần nữa, các hướng dẫn phong cách khác nhau sẽ đưa ra các lựa chọn khác nhau, nhưng gần như *tất cả* đều khuyên giới hạn độ dài mỗi dòng mã.

Nhìn chung, mã sẽ dễ đọc hơn nếu bạn chủ động ngắt những dòng dài hơn khoảng 80 ký tự. Nhiều trình soạn thảo mã có một vạch trên màn hình để báo khi bạn vượt qua ngưỡng này. Khi chủ động ngắt dòng, bạn nên cố gắng ngắt ngay *sau* một toán tử hoặc dấu phẩy.

Có một số cách định dạng các dòng tiếp nối. Ví dụ, bạn có thể viết:

```javascript
// This line is a bit too long
let reallyReallyLongLine = something + somethingElse + anotherThing + howManyTacos + oneMoreReallyLongThing;

// You could format it like this
let reallyReallyLongLine =
  something +
  somethingElse +
  anotherThing +
  howManyTacos +
  oneMoreReallyLongThing;

// Or maybe like this
let anotherReallyReallyLongLine = something + somethingElse + anotherThing +
                                  howManyTacos + oneMoreReallyLongThing;
```

Các cách định dạng khác nhau không nhất thiết đúng hay sai, và mỗi người có thể thích một cách khác nhau. Hãy làm theo cách có ý nghĩa với bạn, đồng thời duy trì sự nhất quán.

### Dấu chấm phẩy

Dấu chấm phẩy *phần lớn* là tùy chọn trong JavaScript vì trình thông dịch JavaScript sẽ tự động chèn chúng nếu bị bỏ qua. Chức năng này *có thể* gặp lỗi trong một số tình huống, dẫn đến lỗi trong mã, vì vậy chúng tôi khuyên bạn nên tập thói quen thêm dấu chấm phẩy.

Dù có dùng hay không thì một lần nữa, điều quan trọng nhất vẫn là tính nhất quán.

### Về chú thích

Chú thích là một công cụ tuyệt vời, nhưng cũng như mọi công cụ tốt, chúng có thể bị sử dụng sai. Đặc biệt khi mới bắt đầu học lập trình, bạn có thể muốn viết chú thích giải thích *mọi thứ* mà mã đang làm. Nhìn chung, đó không phải là một thói quen tốt. Hãy xem một số cạm bẫy thường gặp khi viết chú thích và *vì sao* chúng là cạm bẫy.

#### Đừng dùng chú thích khi bạn nên dùng git

Bạn có thể muốn viết chú thích trong mã để giải thích những thay đổi hoặc phần bổ sung mình đã thực hiện. Ví dụ:

```javascript
/**
 * 2023-01-10: Removed unnecessary code that was causing confusion (RM)
 * 2023-03-05: Simplified the code (JP)
 * 2023-05-15: Removed functions that were causing bugs in production (LI)
 * 2023-06-22: Added a new function to combine values (JR)
 */
```

Vấn đề là bạn đã có sẵn một công cụ để theo dõi thay đổi: git! Việc duy trì những chú thích này sẽ trở thành một gánh nặng, và bạn vẫn chỉ có được bức tranh không đầy đủ về những gì đã xảy ra. Các tệp của bạn cũng sẽ chứa những phần dư thừa không nên có ở đó.

Khi dùng git, toàn bộ thông tin này sẽ được sắp xếp gọn gàng trong kho mã và dễ dàng truy cập bằng `git log`.

Điều tương tự cũng áp dụng cho mã không còn được sử dụng. Nếu sau này cần lại, bạn chỉ việc tìm trong các commit git. Việc tạm thời chú thích một đoạn mã trong lúc thử nghiệm điều gì đó dĩ nhiên không sao, nhưng khi một đoạn mã không còn cần thiết, hãy xóa nó. Đừng để những thứ như sau lảng vảng trong tệp:

```javascript
theFunctionInUse();
// oldFunction();
// evenOlderUselessFunction();
// whyAmIEvenHereImAncient();
```

#### Nói về lý do, không phải cách làm

Lý tưởng nhất, chú thích không nên cung cấp đoạn mã giả lặp lại nội dung mã. Chú thích tốt giải thích *lý do* phía sau một đoạn mã. Đôi khi bạn thậm chí không cần chú thích nào cả!

Giả sử chúng ta có một chuỗi trong đó một phần văn bản nằm giữa cặp dấu ngoặc vuông, và muốn lấy phần văn bản bên trong cặp ngoặc đó.

```javascript
// Function to extract text
function extractText(s) {
  // Return the string starting after the "[" and ending at "]"
  return s.substring(s.indexOf("[") + 1, s.indexOf("]"));
}
```

Các chú thích trên chỉ mô tả những gì chúng ta đã có thể thấy từ chính mã. Những chú thích hữu ích hơn một chút có thể giải thích lý do phía sau đoạn mã.

```javascript
// Extracts text inside square brackets (excluding the brackets)
function extractText(s) {
  return s.substring(s.indexOf("[") + 1, s.indexOf("]"));
}
```

Nhưng thường thì chúng ta có thể để mã tự nói lên ý nghĩa mà không cần chú thích.

```javascript
function extractTextWithinBrackets(text) {
  const bracketTextStart = text.indexOf("[") + 1;
  const bracketTextEnd = text.indexOf("]");
  return text.substring(bracketTextStart, bracketTextEnd);
}
```

Trong ví dụ đầu tiên, các chú thích lặp lại hai lần điều mà mã thực hiện. Nhưng bạn chỉ cần đọc mã là đã hiểu được, nên các chú thích trở nên thừa.

Trong ví dụ thứ hai, chú thích làm rõ mục đích của hàm: lấy phần văn bản giữa các dấu ngoặc vuông trong một chuỗi chứ không chỉ “lấy văn bản”. Điều đó hữu ích, nhưng chúng ta còn có thể làm *tốt hơn nữa*.

Trong ví dụ cuối, hoàn toàn không cần chú thích. Việc dùng tên hàm và biến mang tính mô tả đã loại bỏ nhu cầu phải giải thích thêm. Khá hay, phải không?

**Điều này không có nghĩa là mã tốt thì không nên có chú thích.** Hãy xem một ví dụ trong đó chú thích phục vụ một mục đích hữu ích:

```javascript
function calculateBMI(height, weight) {
  // The formula for BMI is weight in kilograms divided by height in meters squared
  const heightInMeters = height / 100;
  const bmi = weight / (heightInMeters * heightInMeters);
  return bmi;
}
```

Chú thích này giúp người đọc nhớ lại cách tính BMI bằng ngôn ngữ thông thường, nhờ đó hiểu vì sao cần đổi chiều cao và phép tính tiếp theo đang làm gì. Cách đặt tên đã khá ổn, nhưng chú thích vẫn bổ sung thêm sự rõ ràng.

Trong nhiều tình huống, những chú thích được đặt đúng chỗ là vô giá. Chúng có thể giải thích vì sao một đoạn mã khó hiểu lại cần thiết, hoặc trình bày bức tranh lớn hơn về lý do một hàm cụ thể *đặc biệt* quan trọng khi được gọi ở đây chứ không phải ở nơi khác. Bài viết được liên kết trong phần bài tập sẽ phân tích sâu hơn về điều này.

### Kết luận

Giờ đây, sau khi đã tìm hiểu những ý tưởng này, chúng ta nên quay lại lời nhắc ở phần đầu. Đừng cố viết mã sạch hoàn hảo; điều đó chỉ khiến bạn nản lòng. Viết mã “spaghetti” là điều không thể tránh khỏi; đôi lúc ai cũng làm vậy. Chỉ cần ghi nhớ những ý tưởng này, rồi theo thời gian và sự kiên nhẫn, mã của bạn sẽ dần sạch hơn.

Học cách viết mã sạch là một quá trình không ngừng cải thiện. Quá trình đó sẽ tiếp tục ngay cả sau khi bạn *hoàn thành* The Odin Project. Bài học này nhằm làm phần nhập môn và điểm khởi đầu cho hành trình đó.

> Mã hay đến từ kinh nghiệm. Kinh nghiệm đến từ những đoạn mã chưa hay.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [10 nguyên tắc để giữ mã lập trình sạch](https://onextrapixel.com/10-principles-for-keeping-your-programming-code-clean/) để tìm hiểu một số mẹo hữu ích về mã sạch.
1. Để hiểu rõ hơn về cách viết chú thích tốt, hãy đọc về [các chú thích cho biết vì sao mã hoạt động](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/) cũng như [viết mã không cần chú thích](https://blog.codinghorror.com/coding-without-comments/).

</div>