### Giới thiệu

Làm cách nào chúng ta có thể thực hiện các thao tác DOM mà chúng ta thường làm trong vanilla JavaScript? Thậm chí có thể thực hiện được React vì tính chất khai báo của nó không? Làm thế nào về tối ưu hóa hiệu suất? Như chúng ta đã biết, khi một trạng thái bị thay đổi, React sẽ cố gắng kết xuất lại thành phần, điều đó có nghĩa là nó sẽ hủy tất cả các biến cục bộ không được kiểm soát bởi React và thực thi lại chúng. Mô hình tinh thần này cũng hoạt động cho một hàm thông thường mà bạn tạo, vì một thành phần chỉ là một hàm. Vì trường hợp này xảy ra, liệu nó có làm chậm ứng dụng không nếu chúng ta thực hiện lại một phép tính rất tốn kém sau mỗi lần kết xuất lại?

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- `useRef` hook và các trường hợp sử dụng của nó.
- Ghi nhớ và cách sử dụng `useCallback` và `useMemo`.
- Ghi nhớ tự động với Trình biên dịch React.

### Cái móc useRef

Hook `useRef` cho phép bạn quản lý một giá trị không cần thiết để hiển thị. Chúng là một giải pháp thay thế cho trạng thái, vì khi bạn muốn một thành phần “ghi nhớ” một số thông tin nhưng bạn không muốn thông tin đó kích hoạt các kết xuất mới, bạn có thể sử dụng hook này.

Chúng thường được sử dụng khi thực hiện các hành động bắt buộc hoặc truy cập các phần tử cụ thể được hiển thị trong DOM. Người giới thiệu cũng có thể*kiên trì*giá trị trong suốt vòng đời của thành phần, nghĩa là giá trị của tham chiếu sẽ không bị hủy mỗi khi thành phần hiển thị lại. Điều này rất hữu ích khi bạn muốn lưu trữ một giá trị mà bạn muốn duy trì trong suốt vòng đời của thành phần mà không cần lưu trữ nó ở trạng thái.

#### DOM thao túng

Khi xây dựng ứng dụng web, đôi khi bạn cần kiểm soát trực tiếp hơn đối với các thành phần cụ thể trong DOM. Móc `useRef` được giải cứu bằng cách cung cấp cách truy cập và tương tác với các phần tử đó.

Hãy tưởng tượng một nút trên trang web và bạn muốn tập trung vào nút đó khi tải trang. Bạn có thể đạt được điều này bằng cách sử dụng hook `useRef`. Đây là cách nó hoạt động:

```jsx
import { useRef, useEffect } from "react";

function ButtonComponent() {
  const buttonRef = useRef(null);

  useEffect(() => {
    buttonRef.current.focus();
  }, []);

  return <button ref={buttonRef}>Click Me!</button>;
}
```

Việc thực hiện rất đơn giản:

1. Chúng tôi đã nhập `useRef` và `useEffect` trong mô-đun `react`.
1. Chúng ta gọi `useRef` với đối số là `null`, trả về một đối tượng có thuộc tính được gọi là `current` có giá trị ban đầu được đặt thành đối số được truyền (trong trường hợp này là `null`). Đối số này—giống như đối số được truyền cho `useState`—bị bỏ qua trong các lần hiển thị tiếp theo.
1. Đã tạo một `useEffect` để được thực thi một lần trên mount của thành phần sẽ cố gắng gọi phương thức `focus` của thành phần nút.
1. Chúng tôi đã đính kèm `buttonRef` vào thuộc tính `ref` của thành phần nút. Điều này thiết lập kết nối giữa `buttonRef` và nút trong DOM.

Bất cứ khi nào trang web của bạn tải, nó sẽ tự động tập trung vào thành phần nút. Bạn có thể hỏi, làm sao nó có phương thức `focus` khi giá trị ban đầu là `null`? Bây giờ bạn nên biết rằng `rendering` và `painting of the screen` xuất hiện trước khi React chạy `useEffect`. Nó đã thiết lập kết nối giữa ref và nút trước khi hiệu ứng được thực thi.

Ngoài ra, hãy nhớ rằng hook `useRef` không chỉ giới hạn ở việc lấy nét phần tử. Nó có thể được dùng cho nhiều tình huống thao tác DOM khác, chẳng hạn như cuộn đến một vị trí cụ thể, đo kích thước phần tử, kích hoạt hiệu ứng chuyển động và về cơ bản là *bất kỳ* thao tác DOM nào bạn từng thực hiện với JavaScript thuần. Khả năng là vô tận! Ví dụ: chúng ta có thể thay đổi `useEffect` trong đoạn mã trên để thực hiện như sau. Thay đổi văn bản của nút, rồi sau 2 giây đổi văn bản trở lại. Bạn **không nên** làm điều này; hãy chỉ dùng `useRef` cho các thao tác DOM không phá hủy, nhưng đây vẫn là một ví dụ:

```jsx
useEffect(() => {
  buttonRef.current.focus();
  buttonRef.current.textContent = "Hey, I'm different!";
  let timeout = setTimeout(() => {
    buttonRef.current.textContent = "Click Me!";
  }, 2000);

  return () => {
    clearTimeout(timeout);
  };
}, []);
```

Điều thú vị ở đây là nó sẽ **không bao giờ** kích hoạt kết xuất lại thành phần!

Một câu hỏi khác có thể xuất hiện trong đầu bạn là "Tại sao không sử dụng `querySelector` hoặc các phương pháp thao tác DOM khác mà chúng tôi đã thực hiện trước đây trong vanilla JavaScript?" Việc tự mình xử lý DOM sẽ làm mất đi mục đích của việc sử dụng React và bất cứ khi nào có thể, chúng ta nên để React*làm*đến chính DOM.

Chúng ta cũng có thể thấy rằng nó tương tự như hook `useState` ở chỗ nó có thể lưu trữ một số giá trị. Sự khác biệt chính là `useRef` tạo ra một tham chiếu có thể thay đổi, cho phép bạn cập nhật giá trị của nó mà không cần kích hoạt kết xuất lại. Tuy nhiên, `useState` quản lý trạng thái bất biến, kích hoạt kết xuất lại khi được cập nhật.

### Cái móc useMemo

Trong tất cả các ví dụ, chúng tôi khuyên bạn nên sử dụng [Thành phần hồ sơ](https://react.dev/reference/react/Profiler) được cung cấp trong mô-đun `react`. Nếu bạn muốn một giải pháp thay thế mang tính tương tác hơn, hãy sử dụng `Profiler` trong [React Công cụ dành cho nhà phát triển](https://react.dev/learn/react-developer-tools) để đo hiệu suất kết xuất. Lưu ý rằng đôi khi bạn không cần phải tối ưu hóa bất cứ điều gì vì mọi thứ đã nhanh như thế nào rồi. Như câu nói nổi tiếng trong phát triển phần mềm:

> Tối ưu hóa sớm là gốc rễ của mọi tội lỗi - Nghệ thuật lập trình máy tính của Donald Knuth

Hook `useMemo` cung cấp một cách để thêm cơ chế ghi nhớ vào các thành phần của chúng ta. Nó được dùng để tối ưu hóa các phép tính tốn kém hoặc phức tạp: kết quả của một lần gọi hàm được lưu lại để dùng về sau mà không cần tính toán lại. Tuy nhiên, giá trị được ghi nhớ chỉ được tính toán lại *chỉ khi* các dependency của hook `useMemo` thay đổi. Các tham số của hook này cũng giống như hook `useEffect` mà bạn đã biết. Hook nhận hai đối số: một callback `calculateValue` và một mảng `dependencies`.

#### Ghi nhớ những phép tính đắt tiền

Trong Dự án Giỏ hàng trước đó, bạn có một số logic để tính tổng giá của các sản phẩm được thêm vào giỏ hàng. Bạn có thể có hoặc không có thành phần `Cart` hoạt động như một ngăn kéo, nơi người dùng có thể mở giỏ hàng mỗi khi họ nhấp vào nút `Add to Cart` hoặc biểu tượng Giỏ hàng trong tiêu đề.

Một ví dụ về thành phần `Cart`:

```jsx
function Cart({ products }) {
  const totalPrice = products.reduce(
    (total, product) => total + product.price * product.quantity,
    0
  );

  return (
    <div>
      {/* Some other content in the cart */}
      {/* Products to display */}
      <p>
        Total Price: <strong>${totalPrice}</strong>
      </p>
      {/* Some button to checkout */}
    </div>
  );
}
```

Trong thành phần `Cart`, chúng ta có tổng giá của sản phẩm được tính trực tiếp bên trong thành phần. Mỗi khi thành phần được hiển thị hoặc cập nhật, quá trình tính toán sẽ được thực hiện lại từ đầu! Điều đó nghe có vẻ không ổn... Điều gì sẽ xảy ra nếu người dùng đã thêm hàng trăm nghìn sản phẩm vào giỏ hàng? Sau đó, nó sẽ dẫn đến trải nghiệm người dùng chậm chạp.

Phương thức `reduce` lặp lại từng sản phẩm và thực hiện phép nhân và phép cộng cho mọi mặt hàng trong giỏ hàng. Hoạt động này trở thành*ngày càng*tốn thời gian khi số lượng sản phẩm tăng lên.

Bây giờ hãy tưởng tượng một người dùng thường xuyên opens/closes giỏ hàng. Mỗi khi ngăn kéo được mở, thành phần `Cart` sẽ được hiển thị, thực thi mọi thứ bên trong thành phần đó. Điều này dẫn đến việc tính toán lại cùng một giá trị không cần thiết ngay cả khi nội dung của giỏ hàng không thay đổi.

Hãy xem cách chúng ta có thể sử dụng `useMemo` để giải quyết vấn đề này:

```jsx
import { useMemo } from "react";

function Cart({ products }) {
  const totalPrice = useMemo(() => {
    return products.reduce(
      (total, product) => total + product.price * product.quantity,
      0
    );
  }, [products]);

  return (
    <div>
      {/* Some other content in the cart */}
      {/* Products to display */}
      <p>
        Total Price: <strong>${totalPrice}</strong>
      </p>
      {/* Some button to checkout */}
    </div>
  );
}
```

Trong ví dụ trên, chúng ta có thể dễ dàng ghi nhớ giá trị được tính bằng cách gói nó trong `useMemo`, vì cú pháp khá giống với `useEffect` và gần như hoạt động giống nhau. `useMemo` sẽ chạy lệnh gọi lại trên mount. Trong những lần kết xuất lại tiếp theo, nó sẽ chỉ chạy lại lệnh gọi lại nếu phần phụ thuộc thay đổi, nếu không, nó sẽ trả về giá trị được lưu trong bộ nhớ đệm bất cứ khi nào nó được chạy lần cuối. Trong trường hợp của chúng tôi, bất cứ khi nào `products` prop thay đổi.

Bằng cách này, bất cứ khi nào người dùng opens/closes giỏ hàng nhiều lần, nó sẽ không tính toán lại `totalPrice` và sử dụng giá trị được lưu trong bộ nhớ đệm miễn là `products` không thay đổi.

#### Kiểm tra đẳng thức tham chiếu

Trong ví dụ này, chúng tôi sẽ sử dụng thành phần `Profiler` trong mô-đun `react` để đo hiệu suất của thành phần. Chúng tôi cũng sẽ giới thiệu `memo`.

Bạn không cần phải bắt đầu đăng ký React cho việc này vì chúng tôi đã hỗ trợ bạn. Một lát sau, chúng ta sẽ chia sẻ một ví dụ tương tác, nhưng bây giờ, hãy suy nghĩ kỹ về mã mà bạn nghĩ sẽ xảy ra, điều gì có thể xảy ra, v.v. Đây cũng có thể là một bài tập tuyệt vời trong việc đọc mã và hình dung cách nó hoạt động.

Xin lưu ý rằng đây chỉ là một ví dụ rất cơ bản. Bạn sẽ gặp phải rất nhiều trường hợp chuyển giá trị sang các thành phần khác dưới dạng đạo cụ, những thành phần rất nặng để kết xuất.

```jsx
import { useState } from "react";

const ButtonComponent = ({ children, onClick }) => {
  let i = 0;
  let j = 0;
  const ITERATION_COUNT = 10_000;
  while (i < ITERATION_COUNT) {
    while (j < ITERATION_COUNT) {
      j += 1;
    }
    i += 1;
    j = 0;
  }

  return (
    <button type="button" onClick={onClick}>
      {children}
    </button>
  );
};

function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount((prevState) => prevState + 1);
  };

  return (
    <div>
      <h1>{count}</h1>
      <ButtonComponent onClick={handleClick}>Click me!</ButtonComponent>
    </div>
  );
}
```

Bạn có thể muốn có một thành phần nút riêng biệt nơi bạn có thể xử lý kiểu dáng và những thứ khác trong đó. Vì vậy, chúng tôi đã tạo một thành phần có tên `ButtonComponent` làm ví dụ. Thành phần này có các đạo cụ `children` và `onClick`.

Chúng ta có thể thấy rằng trình xử lý nhấp chuột được xác định trong thành phần `Counter` và chúng ta đã chuyển nó tới prop `onClick` của `ButtonComponent`.

Chúng ta biết rằng một thành phần sẽ hiển thị bất cứ khi nào trạng thái thay đổi hoặc giá trị prop thay đổi. Bất cứ điều gì bên trong không được kiểm soát bởi React đều bị hủy và thực thi lại. Hàm, biến, v.v. Do đó, hàm `handleClick` được tạo lại mỗi lần và prop `onClick` của `ButtonComponent` cũng thay đổi. Được rồi, vậy `useMemo` có thể giúp gì ở đây?

Chúng ta đã biết mình có thể ghi nhớ một giá trị bằng cách sử dụng `useMemo`, phải không? Sau đó, chúng ta chỉ cần lưu trữ tham chiếu hàm và sử dụng một mảng phụ thuộc trống để nó không thay đổi.

Hãy tạo một hàm mới và đặt tên là `memoizedHandleClick`:

```jsx
const memoizedHandleClick = useMemo(() => handleClick, []);
```

Chúng ta không cần tạo một hàm mới mà đây chỉ là để kiểm tra hai hàm này. Bạn cũng có thể trực tiếp thực hiện các thao tác sau:

```jsx
// Syntax might be weird, but just remember that `useMemo` can take any value, and a function is also just a value `() => setCount((prevState) => prevState + 1)`

const handleClick = useMemo(
  // first arrow function is useMemo's callback
  // second arrow function is our function that will be called later, this one is going to be the cached value and what's going to be stored in `handleClick`
  () => () => setCount((prevState) => prevState + 1),
  []
);
```

Tuyệt vời, `useMemo` sẽ giúp chúng ta ở đây phải không? Nó không thể hiển thị lại `ButtonComponent` nữa đúng không? Không, nó vẫn sẽ kết xuất lại vì bất cứ khi nào `state` của một thành phần thay đổi, nó cũng sẽ kết xuất lại các thành phần con của nó, điều này cũng có thể nói khác - một thành phần sẽ tự kết xuất lại nếu thành phần gốc của nó kết xuất lại. Có cách nào để khắc phục điều này? Vâng, có! React ở một trong những APIs của nó cung cấp [chức năng bao bọc bản ghi nhớ](https://react.dev/reference/react/memo) cho phép bạn bỏ qua việc hiển thị lại một thành phần khi các props của nó không thay đổi (có, ngay cả khi thành phần gốc hiển thị lại). Chúng ta có thể sử dụng `memo` này và bọc `ButtonComponent` trong đó.

```jsx
import { useState, memo } from "react";

const ButtonComponent = memo(({ children, onClick }) => {
  let i = 0;
  let j = 0;
  const ITERATION_COUNT = 10_000;
  while (i < ITERATION_COUNT) {
    while (j < ITERATION_COUNT) {
      j += 1;
    }
    i += 1;
    j = 0;
  }

  return (
    <button type="button" onClick={onClick}>
      {children}
    </button>
  );
});
```

Việc gói thành phần bằng `memo` sẽ ngăn chặn quá trình cập nhật đi xuống được kích hoạt phía trên thành phần. Vì vậy, thành phần này sẽ chỉ hiển thị lại khi `props` của nó thay đổi hoặc nếu `state` của chính nó thay đổi.

Với tất cả những gì đã nói và làm, hãy truy cập [repo ví dụ phản ứng](https://github.com/TheOdinProject/react-examples)(phân nhánh và sao chép nó nếu bạn chưa làm vậy), `cd` vào thư mục `memoization/` và chạy `npm install` rồi `npm run dev`. Hãy dành chút thời gian để chơi đùa với mã. Cuộc thí nghiệm! Kiểm tra mọi thứ và phá vỡ mọi thứ!

Đây là một số tình huống có thể xảy ra:

1. Nếu bạn đã vượt qua `handleClick` và `ButtonComponent` có `memo`. Nó vẫn sẽ kết xuất lại. Kiểm tra đẳng thức tham chiếu không thành công (prop trước đó là*không bằng*đến chỗ dựa hiện tại).
1. Nếu bạn đã vượt qua `memoizedHandleClick` và `ButtonComponent` có `memo`. Nó sẽ không hiển thị lại. Đã vượt qua kiểm tra đẳng thức tham chiếu (prop trước đó là*bình đẳng*đến chỗ dựa hiện tại).

Điều này hoạt động với tất cả các giá trị sẽ được chuyển dưới dạng chỗ dựa. Bạn có thể thấy nó được sử dụng thường xuyên với Context API:

```jsx
const value = useMemo(
  () => ({ someState, someFunction }),
  [someState, someFunction]
);

return <Context value={value}>{children}</Context>;
```

### Cái móc useCallback

Hook `useCallback` cung cấp một cách khác để ghi nhớ một giá trị, không chỉ **bất kì** giá trị như `useMemo`. Nó chỉ có thể ghi nhớ một chức năng. Bạn có thấy đoạn mã trước mà chúng tôi ghi nhớ tham chiếu hàm với `useMemo` không?

```jsx
const handleClick = useMemo(
  () => () => setCount((prevState) => prevState + 1),
  []
);
// or
const memoizedHandleClick = useMemo(() => handleClick, []);
```

Với `useCallback` thì chúng ta không cần phải làm điều đó. Nó được thiết kế đặc biệt cho các chức năng:

```jsx
import { useCallback } from "react";

// Inside a component
// Without useCallback
const handleClick = () => setCount((prevState) => prevState + 1);
// With useCallback
const handleClick = useCallback(
  () => setCount((prevState) => prevState + 1),
  []
);
// or
const memoizedHandleClick = useCallback(handleClick, []);
```

Đúng, chỉ có một chức năng mũi tên và nó dễ đọc hơn. Không có gì bổ sung cho `useCallback` ngoài việc nó chỉ ghi nhớ các chức năng. Vì vậy, sự khác biệt chính giữa `useMemo` và `useCallback` chỉ là loại giá trị mà nó trả về.

Vậy chúng ta nên sử dụng cái nào? Sử dụng `useMemo` cho*bất kì*các loại giá trị và sử dụng `useCallback` cụ thể cho các hàm. Vào cuối ngày, cả hai đều làm những việc tương tự với một sự khác biệt nhỏ, vì vậy hãy sử dụng bất cứ thứ gì bạn thích.

### React Trình biên dịch

Hiện tại, chúng tôi cũng có Trình biên dịch React, đây là một công cụ xây dựng tương đối mới có thể tự động tối ưu hóa ứng dụng React của chúng tôi. Nó sẽ phân tích mã của chúng tôi và ghi nhớ các thành phần và hook thích hợp, chủ yếu tập trung vào việc cải thiện hiệu suất cập nhật (kết xuất lại các thành phần hiện có). Điều này có nghĩa là trong hầu hết các trường hợp, chúng ta*không cần phải làm vậy*ghi nhớ thủ công các thành phần hoặc móc.

Vì vậy, nếu React Trình biên dịch tự động ghi nhớ các hook và các thành phần của chúng ta, chúng ta có cần biết về `React.memo` hoặc các hook như `useMemo` và `useCallback` không? Câu trả lời ngắn gọn - có. Việc hiểu về ghi nhớ thủ công giúp chúng tôi hiểu rõ hơn những gì Trình biên dịch React thực hiện, chưa kể rằng bạn gần như chắc chắn sẽ gặp phải các kỹ thuật ghi nhớ thủ công trong các cơ sở mã hiện có. Cuối cùng, có thể có những trường hợp chúng ta cần kiểm soát nhiều hơn những gì được ghi nhớ và so sánh với những gì Trình biên dịch React sẽ tự động thực hiện, chẳng hạn như đảm bảo phần phụ thuộc `useEffect` được ghi nhớ để đảm bảo nó không thay đổi một cách không cần thiết và kích hoạt hiệu ứng.

### Phần kết luận

Phù, đây là một bài học dài. Tham chiếu và ghi nhớ là những khái niệm khó nắm bắt nhưng chúng tôi chắc chắn rằng bạn sẽ hiểu chúng khi thực hành. Các tài liệu tham khảo đặc biệt thực sự hữu ích cho một số trường hợp sử dụng. Đối với tính năng ghi nhớ tự động, nó có thể giúp tối ưu hóa ứng dụng của chúng tôi mà không cần viết thêm bất kỳ mã nào. Việc ghi nhớ thủ công cũng có thể cần thiết trong một số trường hợp nhất định, nhưng bạn chỉ nên sử dụng nó khi thực sự cần. Những chủ đề này cũng tạo nên những câu hỏi phỏng vấn hay, vì vậy hãy đảm bảo bạn biết sự khác biệt giữa `useMemo` và `useCallback`!

### Phân công

<div class="lesson-content__panel" markdown="1">

1. bài viết [Khi nào nên useMemo và useCallback](https://kentcdodds.com/blog/usememo-and-usecallback) của Kent C. Dodds giới thiệu thêm nhiều ví dụ về thời điểm nên sử dụng `useMemo` và `useCallback` và khi nào bạn không nên bận tâm đến việc sử dụng chúng.
1. Chúng ta mới chỉ tìm hiểu cách triển khai cơ bản của hook `useRef`. Để biết thêm ví dụ về cách sử dụng nó và lý do chúng ta nên cảnh giác khi sử dụng hook (xem thêm trên các liên kết họ cung cấp trong hướng dẫn), hãy xem hướng dẫn tương tác của tài liệu React để biết [móc useRef](https://react.dev/reference/react/useRef).
1. Để tìm hiểu sâu hơn về cách truy cập và sửa đổi các nút DOM một cách an toàn, hãy xem tài liệu React trên [Thao tác DOM với Ref](https://react.dev/learn/manipulating-the-dom-with-refs).
1. Như chúng ta đã tìm hiểu, hook `useRef` còn có những cách sử dụng khác ngoài những gì chúng ta đã đề cập chủ yếu là Thao tác DOM. Tìm hiểu thêm về các trường hợp sử dụng của nó trong bài viết tuyệt vời này của Dan Abramov [Tạo setInterval Khai báo bằng React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/).
1. Đi qua [React Tài liệu biên dịch](https://react.dev/learn/react-compiler) để tìm hiểu thêm một chút về nó cũng như tìm hiểu cách cài đặt và định cấu hình nó trong các dự án của bạn.

</div>
