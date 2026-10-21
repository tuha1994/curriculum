### Giới thiệu

Làm thế nào chúng ta có thể thực hiện các thao tác DOM mà chúng ta thường làm trong JavaScript thuần? Liệu điều này có khả thi trong React vì tính chất khai báo của nó không? Còn về tối ưu hiệu suất thì sao? Như tất cả chúng ta đều biết, khi một trạng thái bị thay đổi, React sẽ cố gắng render lại component, điều này có nghĩa là nó sẽ hủy tất cả các biến cục bộ không được React quản lý và thực thi lại chúng. Mô hình tư duy này cũng áp dụng cho một hàm thông thường mà bạn tạo, vì một component chỉ là một hàm. Vì đây là trường hợp, liệu nó có làm chậm ứng dụng nếu chúng ta có một phép tính rất tốn kém bị thực thi lại với mỗi lần render không?

### Tổng quan bài học

Phần này chứa tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- `useRef` và các trường hợp sử dụng của nó.
- Ghi nhớ kết quả tính toán trước (Memoization) và cách `useCallback` và `useMemo` có thể được sử dụng.
- Ghi nhớ tự động với Trình biên dịch React.

### Hook useRef

Hook `useRef` cho phép bạn quản lý một giá trị không cần thiết cho việc hiển thị. Chúng là một sự thay thế cho state, khi bạn muốn một component “ghi nhớ” một số thông tin, nhưng bạn không muốn thông tin đó kích hoạt các lần render mới, bạn có thể sử dụng hook này.

Chúng thường được sử dụng khi thực hiện các hành động mệnh lệnh hoặc truy cập vào các phần tử cụ thể được hiển thị trong DOM. Refs cũng có thể *duy trì* giá trị trong suốt vòng đời của component, có nghĩa là giá trị của ref sẽ không bị phá hủy mỗi khi component được render lại. Điều này rất hữu ích khi bạn muốn lưu trữ một giá trị mà bạn muốn duy trì trong suốt vòng đời của component mà không cần lưu trữ nó trong state.

#### Thao tác DOM

Khi xây dựng các ứng dụng web, đôi khi bạn cần kiểm soát trực tiếp hơn các phần tử cụ thể trong DOM. Hook `useRef` giúp giải quyết vấn đề này bằng cách cung cấp cách tiếp cận và tương tác với các phần tử đó.

Hãy tưởng tượng một nút trên trang web, và bạn muốn tập trung vào nút đó khi trang tải. Bạn có thể thực hiện điều này bằng cách sử dụng hook `useRef`. Đây là cách nó hoạt động:

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

Việc triển khai rất đơn giản:

1. Chúng tôi đã nhập `useRef` và `useEffect` trong mô-đun `react`.
1. Chúng tôi gọi `useRef` với một đối số `null`, điều này trả về một đối tượng với một thuộc tính có tên là `current` mà giá trị ban đầu được đặt thành đối số được truyền vào (trong trường hợp này là `null`). Đối số này—giống như đối số được truyền cho `useState`—bị bỏ qua trong các lần kết xuất tiếp theo.
1. Đã tạo một `useEffect` để được thực thi một lần trên mount của thành phần sẽ cố gắng gọi phương thức `focus` của phần tử nút.
1. Chúng tôi đã đính kèm `buttonRef` vào thuộc tính `ref` của phần tử nút. Điều này thiết lập kết nối giữa `buttonRef` và nút trong DOM.

Bất cứ khi nào trang web của bạn tải, nó sẽ tự động tập trung vào phần tử nút. Bạn có thể hỏi, làm sao nó có thể có phương thức `focus` khi giá trị ban đầu là `null`? Bạn nên biết rằng `rendering` và `painting of the screen` xuất hiện trước khi React chạy `useEffect`. Nó đã thiết lập kết nối giữa ref và nút trước khi hiệu ứng được thực hiện.

Ngoài ra, hãy nhớ rằng hook `useRef` không chỉ giới hạn cho các phần tử lấy nét. Nó có thể được sử dụng cho nhiều tình huống thao tác DOM khác nhau, chẳng hạn như cuộn đến một vị trí cụ thể, đo kích thước của một phần tử, kích hoạt hoạt ảnh, và cơ bản là *bất kỳ* thao tác DOM nào mà bạn đã làm trước đây với JavaScript thuần. Các khả năng là vô tận! Ví dụ, chúng ta có thể thay đổi `useEffect` trong đoạn mã trên để thực hiện những điều sau. Thay đổi văn bản của nút, và sau 2 giây, thay đổi văn bản trở lại. Bạn **không nên** làm điều này và chỉ sử dụng `useRef` cho các thao tác DOM không phá hủy, nhưng chỉ là một ví dụ:

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

Điều thú vị về việc này là điều này sẽ **không bao giờ** kích hoạt việc render lại component!

Một câu hỏi khác có thể xuất hiện trong đầu bạn là, "Tại sao không chỉ sử dụng `querySelector` hoặc các phương pháp thao tác DOM khác mà chúng ta đã làm trước đây trong JavaScript thuần?" Tự chúng ta thao tác với DOM sẽ làm mất mục đích sử dụng React, và bất cứ khi nào có thể chúng ta nên để React *commit* vào chính DOM.

Chúng ta cũng có thể thấy rằng nó tương tự như hook `useState` ở chỗ nó có thể lưu trữ một số giá trị. Sự khác biệt chính là `useRef` tạo ra một tham chiếu có thể thay đổi, cho phép bạn cập nhật giá trị của nó mà không kích hoạt render lại. Nhưng `useState` quản lý một trạng thái không thể thay đổi, kích hoạt render lại khi được cập nhật.

### Hook useMemo

Trong tất cả các ví dụ, chúng tôi khuyên bạn nên sử dụng [thành phần Profiler](https://react.dev/reference/react/Profiler) được cung cấp trong module `react`. Nếu bạn muốn một lựa chọn tương tác hơn, hãy sử dụng `Profiler` trong [Công cụ Phát triển React](https://react.dev/learn/react-developer-tools) để đo hiệu suất kết xuất. Lưu ý rằng đôi khi bạn không cần tối ưu gì cả vì mọi thứ đã hoạt động rất nhanh. Như câu nói nổi tiếng trong phát triển phần mềm:

> Tối ưu hóa sớm là nguồn gốc của mọi điều ác -- Nghệ thuật lập trình máy tính của Donald Knuth

Hook `useMemo` cung cấp một cách để thêm memoization bên trong các component của chúng ta. Nó được sử dụng để tối ưu hóa các phép tính tốn kém hoặc phức tạp, nơi nó lưu trữ kết quả của một lần gọi hàm và giữ lại để sử dụng sau mà không cần tính toán lại. Tuy nhiên, giá trị đã memo sẽ chỉ được tính toán lại *only* khi các phụ thuộc của hook `useMemo` thay đổi. Và đúng vậy, các tham số của hook này giống với hook `useEffect` mà bạn đã biết. Hook nhận vào hai đối số: một callback `calculateValue` và một mảng `dependencies`.

#### Ghi nhớ các phép tính tốn kém

Trong Dự án Giỏ Hàng trước, bạn có một số logic nơi bạn tính tổng giá của các sản phẩm được thêm vào giỏ. Bạn có thể có hoặc không có một thành phần `Cart` hoạt động như một ngăn kéo, nơi người dùng có thể mở giỏ hàng mỗi khi họ nhấn vào nút `Add to Cart` hoặc biểu tượng Giỏ hàng trong phần đầu trang.

Một ví dụ về một thành phần `Cart`:

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

Trong thành phần `Cart` của chúng tôi, chúng tôi đã tính tổng giá của các sản phẩm trực tiếp bên trong thành phần. Mỗi khi thành phần được hiển thị hoặc cập nhật, phép tính này được thực hiện từ đầu! Điều đó nghe không hay... Nếu người dùng đã thêm hàng trăm nghìn sản phẩm vào giỏ hàng thì sao? Khi đó nó sẽ dẫn đến trải nghiệm người dùng chậm chạp.

Phương pháp `reduce` lặp qua từng sản phẩm và thực hiện phép nhân và cộng cho mỗi mặt hàng trong giỏ. Hoạt động này trở nên *ngày càng* tốn thời gian khi số lượng sản phẩm tăng lên.

Bây giờ hãy tưởng tượng một người dùng thường xuyên mở/đóng giỏ hàng. Mỗi lần ngăn kéo được mở, thành phần `Cart` được kết xuất, thực thi tất cả mọi thứ bên trong thành phần. Điều này dẫn đến việc tính toán lại không cần thiết giá trị cùng một lúc ngay cả khi nội dung giỏ hàng không thay đổi.

Hãy xem chúng ta có thể sử dụng `useMemo` để giải quyết điều này như thế nào:

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

Trong ví dụ trên, chúng ta có thể dễ dàng ghi nhớ giá trị đã tính bằng cách bao nó trong một `useMemo`, vì cú pháp gần như giống với `useEffect` và hầu như hoạt động tương tự. `useMemo` sẽ chạy callback khi component được mount. Trong các lần render tiếp theo, nó chỉ chạy lại callback nếu một phụ thuộc thay đổi, nếu không nó sẽ trả về giá trị đã được lưu trong bộ nhớ cache từ lần cuối cùng nó được chạy. Trong trường hợp của chúng ta, mỗi khi prop `products` thay đổi.

Theo cách này, bất cứ khi nào người dùng mở/đóng giỏ hàng nhiều lần, nó sẽ không tính lại `totalPrice` và sẽ sử dụng giá trị đã lưu trong bộ nhớ đệm miễn là `products` không thay đổi.

#### Kiểm tra bằng tham chiếu

Trong ví dụ này, chúng ta sẽ sử dụng thành phần `Profiler` trong mô-đun `react` để đo hiệu suất của thành phần. Chúng ta cũng sẽ giới thiệu `memo`.

Bạn không cần phải khởi động một ứng dụng React cho việc này vì chúng tôi đã chuẩn bị sẵn cho bạn. Một chút sau, chúng tôi sẽ chia sẻ một ví dụ tương tác, nhưng hiện tại, hãy suy nghĩ về mã và dự đoán những gì bạn nghĩ sẽ xảy ra, những gì có thể xảy ra, v.v. Điều này cũng có thể là một bài tập tuyệt vời để đọc mã và hình dung cách nó hoạt động.

Xin lưu ý rằng đây chỉ là một ví dụ rất cơ bản. Bạn sẽ gặp nhiều việc truyền giá trị cho các thành phần khác dưới dạng props, những thành phần mà việc render rất nặng.

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

Bạn có khả năng sẽ muốn có một thành phần nút riêng mà bạn có thể xử lý các kiểu dáng và những thứ khác trong đó. Vì vậy, chúng tôi đã tạo một thành phần gọi là `ButtonComponent` làm ví dụ. Thành phần này nhận các props `children` và `onClick`.

Chúng ta có thể thấy rằng trình xử lý click được định nghĩa trong thành phần `Counter`, và chúng ta đã truyền nó vào prop `onClick` của `ButtonComponent`.

Chúng ta biết rằng một component sẽ render bất cứ khi nào state thay đổi hoặc prop thay đổi. Bất cứ thứ gì bên trong mà không được React kiểm soát sẽ bị hủy và thực thi lại. Các hàm, biến, v.v. Do đó, hàm `handleClick` được tạo lại mỗi lần, và prop `onClick` của `ButtonComponent` cũng thay đổi. Được rồi, vậy `useMemo` có thể giúp gì ở đây?

Chúng ta đã biết rằng chúng ta có thể ghi nhớ một giá trị bằng cách sử dụng `useMemo`, đúng không? Sau đó, chúng ta chỉ cần lưu trữ tham chiếu hàm và sử dụng một mảng phụ thuộc trống để nó không thay đổi.

Hãy tạo một hàm mới và đặt tên nó là `memoizedHandleClick`:

```jsx
const memoizedHandleClick = useMemo(() => handleClick, []);
```

Chúng ta không cần tạo một hàm mới, nhưng đây chỉ là để kiểm tra hai hàm này. Bạn cũng có thể thực hiện trực tiếp như sau:

```jsx
// Syntax might be weird, but just remember that `useMemo` can take any value, and a function is also just a value `() => setCount((prevState) => prevState + 1)`

const handleClick = useMemo(
  // first arrow function is useMemo's callback
  // second arrow function is our function that will be called later, this one is going to be the cached value and what's going to be stored in `handleClick`
  () => () => setCount((prevState) => prevState + 1),
  []
);
```

Tuyệt vời, `useMemo` sẽ giúp chúng ta ở đây đúng không? Nó không thể nào render lại `ButtonComponent` nữa đúng không? Không, nó vẫn sẽ render lại vì mỗi khi `state` của một component thay đổi, nó cũng sẽ render lại các component con của nó, điều này cũng có thể được nói khác đi - một component sẽ render lại chính nó nếu component cha của nó render lại. Có cách nào để khắc phục điều này không? Có, có chứ! React trong một trong các API của nó cung cấp [hàm bao bọc memo](https://react.dev/reference/react/memo) cho phép bạn bỏ qua việc render lại một component khi props của nó không thay đổi (vâng, ngay cả khi component cha render lại). Chúng ta có thể sử dụng `memo` và bao bọc `ButtonComponent` trong nó.

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

Việc bao quanh thành phần bằng một `memo` ngăn chặn việc cập nhật xuống dưới được kích hoạt phía trên thành phần. Vì vậy, thành phần này sẽ chỉ render lại khi `props` của nó thay đổi hoặc nếu chính `state` của nó thay đổi.

Với tất cả những gì đã nói và làm, hãy đến với [repo ví dụ react của chúng tôi](https://github.com/TheOdinProject/react-examples) (nhánh và sao chép nó nếu bạn chưa làm), `cd` vào thư mục `memoization/` và chạy `npm install` rồi `npm run dev`. Dành chút thời gian để thử nghiệm với mã. Thử nghiệm đi! Kiểm tra mọi thứ và phá vỡ chúng!

Đây là một số tình huống có thể xảy ra:

1. Nếu bạn đã truyền `handleClick` và `ButtonComponent` có một `memo`. Nó vẫn sẽ render lại. Kiểm tra tương đương tham chiếu thất bại (prop trước đó là * không bằng * với prop hiện tại).
1. Nếu bạn đã truyền `memoizedHandleClick` và `ButtonComponent` có một `memo`. Nó sẽ không được kết xuất lại. Kiểm tra bằng tham chiếu thành công (prop trước đó là *equal* với prop hiện tại).

Điều này hoạt động với tất cả các giá trị sẽ được truyền như một prop. Bạn có thể thấy nó được sử dụng thường xuyên với Context API:

```jsx
const value = useMemo(
  () => ({ someState, someFunction }),
  [someState, someFunction]
);

return <Context value={value}>{children}</Context>;
```

### Hook useCallback

Hook `useCallback` cung cấp một cách khác để ghi nhớ giá trị, không chỉ là **any** giá trị như `useMemo`. Nó chỉ có thể ghi nhớ một hàm. Bạn có thấy đoạn mã trước đó mà chúng tôi có với việc ghi nhớ một tham chiếu hàm bằng `useMemo` không?

```jsx
const handleClick = useMemo(
  () => () => setCount((prevState) => prevState + 1),
  []
);
// or
const memoizedHandleClick = useMemo(() => handleClick, []);
```

Với `useCallback`, chúng ta không cần làm điều đó. Nó được tạo ra đặc biệt cho các hàm:

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

Tuyệt, chỉ có một hàm mũi tên, và nó dễ đọc hơn. Không có gì thêm vào `useCallback` ngoài việc nó chỉ ghi nhớ các hàm. Vì vậy, sự khác biệt chính giữa `useMemo` và `useCallback` chỉ là loại giá trị mà nó trả về.

Vậy chúng ta nên dùng cái nào? Dùng `useMemo` cho *any* các loại giá trị, và dùng `useCallback` đặc biệt cho các hàm. Cuối cùng, cả hai đều làm những việc tương tự với một sự khác biệt nhỏ, nên dùng cái nào bạn thích.

### Trình biên dịch React

Chúng ta hiện cũng có React Compiler, là một công cụ xây dựng thời gian chạy khá mới có thể tự động tối ưu hóa ứng dụng React của chúng ta. Nó sẽ phân tích mã của chúng ta và ghi nhớ các component và hook phù hợp, chủ yếu tập trung vào việc cải thiện hiệu suất cập nhật (render lại các component hiện có). Điều này có nghĩa là trong hầu hết các trường hợp, chúng ta *không phải* tự ghi nhớ các component hoặc hook.

Vậy nếu Trình biên dịch React tự động ghi nhớ (memoize) các hook và component của chúng ta, liệu chúng ta có cần biết về `React.memo` hay các hook như `useMemo` và `useCallback` không? Câu trả lời ngắn gọn - có. Hiểu về việc ghi nhớ thủ công sẽ giúp chúng ta hiểu những gì Trình biên dịch React thực hiện bên trong, chưa kể rằng bạn hầu như chắc chắn sẽ gặp phải các kỹ thuật ghi nhớ thủ công trong các codebase hiện có. Cuối cùng, có thể có những trường hợp chúng ta cần kiểm soát nhiều hơn về những gì được ghi nhớ và cách thức so với những gì Trình biên dịch React sẽ tự động thực hiện, chẳng hạn như đảm bảo một phụ thuộc `useEffect` được ghi nhớ để đảm bảo nó không thay đổi không cần thiết và kích hoạt hiệu ứng.

### Kết luận

Hú, đây là một bài học dài. Refs và memoization là những khái niệm khó nắm bắt, nhưng chúng tôi chắc chắn bạn sẽ hiểu chúng qua thực hành. Refs đặc biệt rất hữu ích cho một số trường hợp sử dụng. Còn về memoization tự động, nó có thể giúp tối ưu hóa ứng dụng của chúng ta mà không cần viết thêm bất kỳ mã nào. Memoization thủ công cũng có thể cần thiết trong một số trường hợp, nhưng bạn chỉ nên sử dụng khi thực sự cần. Những chủ đề này cũng là các câu hỏi phỏng vấn tuyệt vời, vì vậy hãy chắc chắn bạn hiểu sự khác biệt giữa `useMemo` và `useCallback`!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bài viết [Khi nào sử dụng useMemo và useCallback](https://kentcdodds.com/blog/usememo-and-usecallback) của Kent C. Dodds giới thiệu thêm nhiều ví dụ về khi nào sử dụng `useMemo` và `useCallback` và khi nào bạn không nên bận tâm sử dụng chúng.
1. Chúng ta chỉ mới tìm hiểu về một triển khai cơ bản của hook `useRef`. Để biết thêm các ví dụ về cách sử dụng nó và lý do tại sao chúng ta nên cẩn thận khi sử dụng hook này (tìm hiểu thêm trong các liên kết mà họ cung cấp trong hướng dẫn), hãy xem hướng dẫn tương tác trong tài liệu React về [useRef hook](https://react.dev/reference/react/useRef).
1. Để tìm hiểu sâu hơn về cách truy cập và chỉnh sửa các nút DOM một cách an toàn, hãy tham khảo tài liệu React về [Manipulating the DOM with Refs](https://react.dev/learn/manipulating-the-dom-with-refs).
1. Như chúng ta đã học, hook `useRef` còn có những mục đích sử dụng khác ngoài những gì chúng ta đã đề cập chủ yếu là Manipulation DOM. Tìm hiểu thêm về các trường hợp sử dụng của nó trong bài viết tuyệt vời này của Dan Abramov [Making setInterval Declarative with React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/).
1. Xem qua [tài liệu trình biên dịch React](https://react.dev/learn/react-compiler) để tìm hiểu thêm về nó, và để biết cách cài đặt và cấu hình nó trong các dự án của bạn.

</div>
