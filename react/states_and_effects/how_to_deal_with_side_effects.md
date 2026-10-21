### Giới thiệu

Một số thành phần nhất định trong React cần tương tác với những thứ bên ngoài chúng. Những việc này có thể là bất cứ điều gì từ việc truy vấn dữ liệu từ máy chủ đến việc tìm kiếm hoặc thay đổi vị trí của thành phần trên trang web, hoặc thậm chí gửi một số dữ liệu đến máy chủ khi cần thiết. Sự tương tác này với *thế giới bên ngoài* được gọi là tác dụng phụ.

Mặc dù chúng ta đã quen với việc hiển thị mã và thêm trình xử lý sự kiện nhưng điều đó vẫn chưa đủ cho mọi mục đích sử dụng, chẳng hạn như khi bạn muốn kết nối với máy chủ của mình và tìm nạp thông báo để hiển thị cho người dùng. Các hiệu ứng cho phép bạn chạy một số mã để đồng bộ hóa thành phần của mình khi cần thiết, khi kết xuất hoặc thay đổi giá trị reactive/state thay vì trên một sự kiện cụ thể.

Tương tự như cách chúng ta có hook `useState`, React cung cấp cho chúng ta một hook `useEffect` tiện dụng để sử dụng các hiệu ứng trong các thành phần của chúng ta.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Hiểu những hiệu ứng trong React.
- Tìm hiểu cách sử dụng hiệu ứng trong ứng dụng React.
- Khám phá các phần khác nhau của hook useEffect.
- Xác định thời điểm sử dụng hiệu ứng trong React.

### Sử dụng hiệu ứng tiết kiệm trong ngày

#### Cái móc useEffect

Hãy để chúng tôi lấy một thành phần trong câu hỏi. Chúng tôi muốn tạo thành phần `Clock` hiển thị số giây đã trôi qua kể từ khi người dùng tải trang web. Để cập nhật nó mỗi giây, chúng ta có thể sử dụng hàm `setInterval` tiện lợi của mình để thêm một biến vào biến trạng thái `counter` mỗi giây. Hãy thử đặt nó vào phần thân của thành phần của chúng ta.

```jsx
import { useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  setInterval(() => {
    setCounter(count => count + 1)
  }, 1000);

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

Than ôi, chúng ta thấy quầy của mình đang trở nên điên cuồng. Điều này xảy ra vì hàm `setInterval` không được gọi một lần mà ở mọi trạng thái hiển thị.

Khi thành phần của chúng tôi hiển thị lần đầu tiên, nó sẽ gọi hàm `setInterval` ban đầu của chúng tôi. Khoảng thời gian đó cập nhật trạng thái mỗi giây, kích hoạt thành phần hiển thị lại. Nhưng mỗi lần kết xuất lại lại gọi `setInterval`, điều này kích hoạt các cập nhật trạng thái thường xuyên hơn, mỗi lần xuất hiện các khoảng thời gian mới và mọi thứ nhanh chóng vượt khỏi tầm kiểm soát.

Đây là lúc cái móc `useEffect` lao vào để cứu chúng ta. Chúng ta có thể gói phép tính này bên trong một móc `useEffect` để di chuyển nó ra ngoài phép tính kết xuất. Nó chấp nhận chức năng gọi lại với tất cả các phép tính.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);
  })

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

Tuy nhiên, nó vẫn tiếp tục phát triển quá nhanh! Đây là nơi xuất hiện một đối số khác của `useEffect`: mảng phụ thuộc.

#### Mảng phụ thuộc

Theo mặc định, hook `useEffect` chạy trên mọi kết xuất. Vì trạng thái cài đặt làm hỏng thành phần nên chúng tôi vẫn nhận được nhiều lệnh gọi setter trên mỗi lần kết xuất, điều này không giúp ích gì cho chúng tôi.

May mắn thay, đối số thứ hai chấp nhận một loạt các phụ thuộc cho phép hook hiển thị lại **chỉ khi những phụ thuộc đó được thay đổi**. Vì vậy, nếu bạn có một biến trạng thái và muốn xảy ra một số tác dụng phụ bất cứ khi nào trạng thái thay đổi, bạn có thể sử dụng hook này và đề cập đến biến trạng thái trong mảng phụ thuộc.

Chúng tôi chuyển một mảng trống trong ví dụ này vì chúng tôi không muốn hook `useEffect` chạy bất cứ lúc nào ngoài kết xuất thành phần ban đầu.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);
  }, [])

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

<div class="lesson-note" markdown="1">

#### Quản lý các phần phụ thuộc useEffect

Thông thường, bạn không cần thêm phần phụ thuộc vào hook `useEffect` của mình theo cách thủ công. Kẻ nói dối của bạn sẽ cho bạn biết về sự phụ thuộc mà nó mong đợi. Để kẻ nói dối hiển thị lỗi và sửa chúng thay vì chặn chúng thường là ý tưởng tốt nhất. Nói chung, khối sau đây thực hiện tốt việc tổng hợp điểm này.

```jsx
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

</div>

#### Chức năng dọn dẹp

Ồ, nó không còn điên cuồng nữa! Tuy nhiên, chúng tôi vẫn gặp sự cố với bộ đếm cập nhật hai lần mỗi giây. Đó có thể được hiểu là một [hành vi gây ra bởi React StrictMode](https://react.dev/reference/react/StrictMode#strictmode). Nó được cho là giúp chúng ta bắt lỗi, vậy lỗi đó ở đây là gì?

Với `StrictMode`, thành phần `App` được gắn kết, ngắt kết nối rồi gắn lại. Hành vi này của `StrictMode` chỉ có trong môi trường phát triển. Lưu ý rằng mỗi khi `useEffect` hook chạy, một `setInterval` mới sẽ được sử dụng.  Khi thành phần được ngắt kết nối lần đầu tiên, `setInterval` không bị dừng mà tiếp tục tăng. Hành vi không cần thiết này có thể được ngăn chặn bằng cách xóa khoảng thời gian khi thành phần chưa được kết nối và đó là lúc phần thứ ba của hook `useEffect` của chúng ta xuất hiện - chức năng dọn dẹp.

Bạn có thể trả về một hàm từ lệnh gọi lại trong hook `useEffect`, hàm này sẽ được thực thi mỗi lần trước khi hiệu ứng tiếp theo được chạy và lần cuối cùng khi thành phần này được ngắt kết nối. Trong trường hợp này, chúng ta hãy dọn dẹp khoảng thời gian bằng chức năng dọn dẹp.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const key = setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);

    return () => {
      clearInterval(key);
    };
  }, [])

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

Phù! Cuối cùng nó cũng hoạt động! Khi bộ đếm tiếp tục tích tắc vui vẻ, chúng ta hãy tóm tắt những gì chúng ta biết về hook `useEffect`.

```jsx
useEffect(
  () => {
    // execute side effect
    return () => {
      // cleanup function on unmounting or re-running effect
    }
  },
  // optional dependency array
  [/* 0 or more entries */]
)
```

### Nhưng chúng ta có cần hiệu ứng không?

`useEffect` là một cơ chế nằm ngoài các khái niệm mà React thường áp dụng, cho phép bạn đồng bộ hóa thành phần của mình với nhiều hệ thống bên ngoài khác nhau như máy chủ, API hoặc trình duyệt DOM. Câu hỏi duy nhất mà bạn có thể tự hỏi mình trước khi sử dụng hiệu ứng là liệu có bất kỳ hệ thống bên ngoài nào cần được đồng bộ hóa, ngoài đạo cụ hoặc trạng thái hay không. Các móc `useEffect` không cần thiết có mùi mã, dễ bị lỗi và gây ra các vấn đề về hiệu suất không cần thiết.

Hãy để chúng tôi giải quyết một số trường hợp trong đó `useEffect` không cần sử dụng.

- Bạn không cần sử dụng hiệu ứng nếu bạn chỉ tính toán thứ gì đó dựa trên trạng thái trong quá trình kết xuất. Đối với sự thay đổi của một thành phần, do thay đổi trong props, bạn có thể tính toán và thiết lập nó trong quá trình kết xuất.

    ```jsx
    import { useState } from "react";

    export default function AdditionDisplay() {
      const [number1, setNumber1] = useState(0);
      const [number2, setNumber2] = useState(0);

      // This is all unnecessary.

      // const [sum, setSum] = useState(0);
      // useEffect(() => {
      //   setSum(number1 + number2);
      // }, [number1, number2]);

      const sum = number1 + number2;

      return (
        <p>{number1} + {number2} = {sum}</p>
      );
    }
    ```

- Bạn không cần hiệu ứng cho các sự kiện. Mã chạy khi một thành phần được **hiển thị** nên có hiệu ứng, phần còn lại nên có trong các sự kiện.

    ```jsx
    import { useState } from "react";

    export default function App() {
      const [input, setInput] = useState("");

      const handleInput = (e) => {
        setInput(e.target.value);
      };

      // You should avoid direct manipulation when not necessary

      // useEffect(() => {
      //   document.getElementById("name").addEventListener("change", handleInput);
      //   return () => {
      //     document.getElementById("name").removeEventListener("change", handleInput);
      //   }
      // });

      return (
        <>
          {/* <input id="name" /> */}

          <input onChange={handleInput} value={input} />
          <p>{ input }</p>
        </>
      );
    }
    ```

- Bạn không cần hiệu ứng để đặt lại trạng thái dựa trên một điều kiện trong hầu hết mọi trường hợp. Bạn đã tìm hiểu về các khóa trong React. Cũng giống như việc sử dụng một khóa trên một mục trong danh sách, việc thêm một khóa vào một thành phần, dựa trên trạng thái mà nó cần được đặt lại sẽ tạo ra một phiên bản duy nhất của thành phần đó cho mỗi thay đổi về giá trị của trạng thái.

- Nếu bạn gặp vấn đề với việc quản lý trạng thái của mình và muốn sử dụng hiệu ứng để cập nhật trạng thái của thành phần cha mẹ hoặc một số thành phần không phải con khác, hãy xem xét **nâng cao trạng thái**. Như chúng ta đã biết, trong React, trạng thái diễn ra theo một hướng, thường là xuống DOM. Vì vậy, cha mẹ biết dữ liệu trước khi truyền cho con cái. Nếu nhiều thành phần con được yêu cầu sử dụng một trạng thái duy nhất thì trạng thái đó nên được chuyển lên trạng thái gốc có tất cả các thành phần cần đến nó, thay vì sử dụng các cửa thoát hiểm như một hiệu ứng.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bài học này từ tài liệu React nói về [vòng đời của một thành phần](https://react.dev/learn/lifecycle-of-reactive-effects), các giai đoạn khác nhau mà quá trình kết xuất diễn ra và vai trò của `useEffect` trong đó.
1. Các ví dụ khác từ tài liệu React về thời điểm [bạn có thể không cần Hiệu ứng](https://react.dev/learn/you-might-not-need-an-effect).
1. Một bài viết khác giải thích một lỗi phổ biến mà người mới bắt đầu mắc phải,[vòng lặp useEffect vô hạn](https://dmitripavlutin.com/react-useeffect-infinite-loop).

</div>
