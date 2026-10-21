### Giới thiệu

Chúng ta đã học cách kiểm thử các ứng dụng JavaScript thuần túy của mình trong một phần trước. Kiểm thử là một công cụ mạnh mẽ cho phép chúng ta viết mã dễ bảo trì và linh hoạt. Nếu bạn đã theo dõi các bài học của chúng tôi, cho đến nay chúng ta đã sử dụng framework [Jest](https://jestjs.io/). Tuy nhiên, vì chúng ta đang sử dụng [Vite](https://vitejs.dev/) nên chúng ta sẽ chuyển sang [Vitest](https://vitest.dev/) làm trình chạy kiểm thử của mình vì nó tích hợp tốt với Vite. Chúng ta sẽ thêm nhiều khả năng hơn cho các bài kiểm thử của mình bằng cách sử dụng [React Testing library (RTL)](https://testing-library.com/docs/react-testing-library/intro/).

### Tổng quan bài học

Phần này chứa tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Tại sao kiểm thử giao diện người dùng lại có giá trị.
- Cách thiết lập môi trường kiểm thử React.
- Cách kiểm tra các yếu tố giao diện người dùng.
- Hiểu về các bài kiểm tra snapshot.

### Kiểm thử giao diện người dùng

Đến bây giờ, bạn sẽ đã có một số kinh nghiệm với TDD và ý tưởng về kiểm thử nói chung. Bạn có thể nhớ trong dự án Battleship, bạn sẽ đã viết các bài kiểm thử giới hạn cho trò chơi Battleship cơ bản, nhưng không hề liên quan đến DOM. Bạn có thể đã thực hiện các thay đổi không liên quan đến giao diện người dùng, khiến các bài kiểm thử thất bại, rồi sau đó giải quyết những gì cần thiết để có các bài kiểm thử phù hợp và vượt qua.

Mặc dù vậy, bạn có thể đã gặp phải lỗi trong giao diện người dùng (UI) vì các bài kiểm tra của bạn chỉ liên quan đến logic cơ bản. Bạn có thể có một trò chơi Battleship hoạt động tốt, nhưng điều đó không có nghĩa là UI thực sự hiển thị những gì bạn muốn hoặc cho phép người dùng tương tác với trò chơi như mong đợi. Và hãy thành thật, mỗi khi bạn thay đổi bất kỳ phần nào trong mã của mình (dù liên quan đến UI hay không), bạn sẽ không nhớ kiểm tra lại mọi khía cạnh liên quan đến UI để chắc chắn mọi thứ vẫn hoạt động như bạn mong đợi. Tức là, điều này không khác gì trước đây, chúng ta chỉ đang liên quan đến UI bây giờ.

Các bài kiểm tra giao diện người dùng (UI) mang lại cho chúng ta sự tự tin hơn rằng các trang web của chúng ta chứa đúng nội dung dự định và hoạt động theo ý muốn của chúng ta, đồng thời thông báo cho chúng ta khi có thứ gì đó không còn đáp ứng yêu cầu của chúng ta. Có thể bạn quyết định thay đổi cấu trúc chính xác của một trạng thái, chẳng hạn như chuyển từ một đối tượng đơn giản sang một mảng, nhưng bây giờ danh sách lại không chứa văn bản mà bạn dự định. Hoặc có thể bạn thêm một điều kiện nào đó vào một thành phần và giờ đây, đột nhiên, chỉ có ba mục tiêu thả cuối cùng cho các thẻ kéo và thả của bạn không còn là mục tiêu hợp lệ. Khi các trang web của bạn trở nên phức tạp hơn, giá trị của các bài kiểm tra tốt (cả UI và không UI) sẽ chỉ tăng lên.

### Thiết lập môi trường kiểm thử React

Theo dõi [hướng dẫn của Robin Wieruch về cách thiết lập Vitest với RTL](https://www.robinwieruch.de/vitest-react-testing-library/). Khi bạn đã hoàn tất việc thiết lập, hãy quay lại đây gặp nhau.

<div class="lesson-note" markdown="1">

#### Biến toàn cục Vitest và ESLint

Ngay cả khi bạn đặt `globals: true` trong `vite.config.js` như trong hướng dẫn cài đặt, ESLint vẫn sẽ cảnh báo bạn, vì nó sẽ không nhận diện được các biến toàn cục này nếu không có một số cấu hình bổ sung trong tệp `eslint.config.js` của bạn. Giải pháp đơn giản nhất là nhập rõ ràng các biến toàn cục mà bạn cần. Trong trường hợp này, bạn có thể bỏ `globals: true` khỏi `vite.config.js`.

</div>

Hú, thật là nhiều bước chuẩn bị. Nhưng vẫn còn một gói nhỏ nữa cần cài đặt trước khi chúng ta có thể bắt đầu:

```bash
npm install @testing-library/user-event --save-dev
```

Bây giờ chúng ta đã có tất cả những gì cần thiết, hãy cùng điểm qua nhanh về chức năng của một số gói đó. Chúng ta sẽ tập trung chủ yếu vào các gói `@testing-library`.

- `@testing-library/react` sẽ cung cấp cho chúng ta quyền truy cập vào các chức năng hữu ích như `render` mà chúng ta sẽ trình bày sau.

- `@testing-library/jest-dom` bao gồm một số bộ kiểm tra tùy chỉnh tiện lợi (các hàm assertive) như `toBeInTheDocument` và nhiều hơn nữa. (danh sách đầy đủ trên [github của jest-dom](https://github.com/testing-library/jest-dom)). Jest đã có rất nhiều bộ kiểm tra nên gói này không bắt buộc phải sử dụng.

- `@testing-library/user-event` cung cấp API `userEvent` mô phỏng các tương tác của người dùng với trang web.

### Truy vấn đầu tiên của chúng tôi

Đầu tiên, chúng ta sẽ kết xuất (render) thành phần sử dụng `render` (bạn có thể đọc tất cả về những gì hàm này có thể làm trong [tài liệu API React Testing Library về `render`](https://testing-library.com/docs/react-testing-library/api/#render)). Chúng tôi nói "kết xuất", nhưng bạn sẽ không thấy bất kỳ màn hình nào. Trong bài viết thiết lập RTL vừa rồi, một trong những gói được cài đặt là `jsdom`, gói này mô phỏng DOM (bao gồm các sự kiện) trong bộ nhớ mà không thực sự hiển thị bất cứ thứ gì giống như trong trình duyệt, và điều đó cho phép chúng ta phân tích nội dung của nó cho các bài kiểm tra của mình. Điều này khác và ít phức tạp hơn nhiều so với các thư viện khác thực sự chạy môi trường trình duyệt thực để thực hiện các bài kiểm tra end-to-end phức tạp hơn - điều đó không nằm trong phạm vi ở đây.

```jsx
// App.jsx

const App = () => <h1>Our First Test</h1>;

export default App;
```

```jsx
// App.test.jsx

import { describe, it, expect } from "vitest";
import { render, screen } from "@testing-library/react";
import App from "./App";

describe("App component", () => {
  it("renders correct heading", () => {
    render(<App />);
    // using regex with the i flag allows simpler case-insensitive comparison
    expect(screen.getByRole("heading").textContent).toMatch(/our first test/i);
  });
});
```

Thực thi `npm test App.test.jsx` trên terminal và xem bài kiểm tra vượt qua. `getByRole` chỉ là một trong hàng chục phương thức truy vấn mà chúng ta có thể đã sử dụng. Về cơ bản, các truy vấn được phân loại thành ba loại: `getBy`, `queryBy` và `findBy`. Hãy xem qua [trang tài liệu React Testing Library về các truy vấn](https://testing-library.com/docs/queries/about/). Hãy chú ý đặc biệt đến các phần "Các loại truy vấn" và "Ưu tiên".

Như đã nêu trong tài liệu của React Testing Library, các phương thức `ByRole` là các phương thức được ưa chuộng để truy vấn, đặc biệt khi kết hợp với tùy chọn `name`. Ví dụ, chúng ta có thể cải thiện độ cụ thể của truy vấn trên như sau: `getByRole("heading", { name: "Our First Test" })`. Các truy vấn được thực hiện thông qua `ByRole` đảm bảo rằng giao diện người dùng của chúng ta có thể truy cập được cho tất cả mọi người bất kể họ sử dụng chế độ nào để điều hướng trang web (ví dụ: chuột hoặc công nghệ hỗ trợ).

### Mô phỏng các sự kiện của người dùng

Có nhiều cách mà người dùng có thể tương tác với một trang web. Mặc dù phản hồi và tương tác trực tiếp của người dùng là không thể thay thế, chúng ta vẫn có thể xây dựng một số độ tin cậy cho các thành phần thông qua các bài kiểm tra. Đây là một nút bấm thay đổi tiêu đề của Ứng dụng:

```jsx
// App.jsx

import { useState } from "react";

const App = () => {
  const [heading, setHeading] = useState("Magnificent Monkeys");

  const clickHandler = () => {
    setHeading("Radical Rhinos");
  };

  return (
    <>
      <button type="button" onClick={clickHandler}>
        Click Me
      </button>
      <h1>{heading}</h1>
    </>
  );
};

export default App;
```

Hãy thử xem nút có hoạt động như mong đợi không. Trong bộ kiểm tra này, chúng ta sẽ sử dụng một tiện ích riêng để truy vấn các phần tử giao diện người dùng của mình. React Testing Library cung cấp đối tượng `screen`, đối tượng này có tất cả các phương thức để thực hiện truy vấn.

```jsx
// App.test.jsx

import { describe, it, expect } from "vitest";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import App from "./App";

describe("App component", () => {
  it("renders magnificent monkeys", () => {
    // since screen does not have the container property, we'll destructure render to obtain a container for this test
    const { container } = render(<App />);
    expect(container).toMatchSnapshot();
  });

  it("renders radical rhinos after button click", async () => {
    const user = userEvent.setup();

    render(<App />);
    const button = screen.getByRole("button", { name: "Click Me" });

    await user.click(button);

    expect(screen.getByRole("heading").textContent).toMatch(/radical rhinos/i);
  });
});
```

Các bài kiểm tra tự nói lên điều của chúng. Trong bài kiểm tra đầu tiên, chúng tôi sử dụng các ảnh chụp nhanh để kiểm tra xem tất cả các nút có hiển thị như chúng tôi mong đợi hay không. Trong bài kiểm tra thứ hai, chúng tôi mô phỏng một sự kiện nhấp chuột. Sau đó, chúng tôi kiểm tra xem tiêu đề có thay đổi hay không. `toMatch` là một trong nhiều xác nhận mà chúng tôi có thể đã thực hiện. Lưu ý rằng hàm callback cho bài kiểm tra thứ hai là một `async`, vì chúng tôi cần điều này để `await user.click()`.

Cũng quan trọng để lưu ý rằng sau mỗi bài kiểm tra, React Testing Library sẽ gỡ bỏ các thành phần đã được render. Đó là lý do tại sao chúng ta render cho mỗi bài kiểm tra. Đối với nhiều bài kiểm tra cho một thành phần, một hàm `setup` tùy chỉnh có thể sẽ rất hữu ích.

### Snapshot là gì?

Kiểm tra ảnh chụp nhanh chỉ là so sánh thành phần được hiển thị của chúng ta với một tệp ảnh chụp nhanh liên quan. Ví dụ, tệp ảnh chụp nhanh được tạo tự động sau khi chúng ta chạy bài kiểm tra *"renders magnificent monkeys"* là:

```jsx
// Vitest Snapshot v1, https://vitest.dev/guide/snapshot.html

exports[`App component > renders magnificent monkeys 1`] = `
<div>
  <button
    type="button"
  >
    Click Me
  </button>
  <h1>
    Magnificent Monkeys
  </h1>
</div>
`;
```

Đây là một biểu diễn HTML của thành phần `App`. Và nó sẽ được so sánh với `App` trong các khẳng định chụp nhanh trong tương lai. Nếu `App` thay đổi dù chỉ một chút, bài kiểm tra sẽ thất bại.

Các bài kiểm tra snapshot nhanh và dễ viết. Một phép khẳng định giúp chúng ta không phải viết nhiều dòng mã. Ví dụ, với một `toMatchSnapshot`, chúng ta không cần phải khẳng định sự tồn tại của nút và tiêu đề. Chúng cũng ngăn không cho những thay đổi không mong muốn len lỏi vào mã của chúng ta. Đọc tất cả về những gì có thể đạt được với snapshots trong [tài liệu snapshot của Vitest](https://vitest.dev/guide/snapshot.html).

Các bản chụp nhanh có vẻ là điều tốt nhất đã xảy ra với chúng tôi trong quá trình thử nghiệm cho đến nay. Nhưng chúng tôi buộc phải tự hỏi, *chính xác* chúng tôi đang thử nghiệm điều gì? Điều gì đang được xác thực? Nếu một bản chụp nhanh vượt qua, nó truyền đạt điều gì về sự đúng đắn của thành phần?

Các bài kiểm tra Snapshot có thể gây ra kết quả dương tính giả. Vì chúng ta không thể xác nhận tính hợp lệ của thành phần từ một bài kiểm tra snapshot, một lỗi có thể sẽ không được phát hiện. Việc phụ thuộc quá nhiều vào các snapshot có thể khiến các nhà phát triển tự tin hơn về mã của họ so với mức họ nên có.

Vấn đề khác với snapshot là kết quả âm tính giả. Ngay cả những thay đổi nhỏ nhất cũng khiến bài kiểm tra thất bại. Sửa dấu câu? Snapshot sẽ thất bại. Thay một thẻ HTML bằng thẻ có ngữ nghĩa hơn? Snapshot sẽ thất bại. Điều này có thể khiến chúng ta mất niềm tin vào bộ bài kiểm tra hoàn toàn. Snapshot không phải vốn xấu; chúng thực sự có mục đích. Nhưng hiểu khi nào nên chụp snapshot và khi nào không nên chụp snapshot là điều có lợi.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Tập trung vào các khái niệm

Mặc dù một số bài viết sử dụng Jest và thư viện kiểm thử Enzyme, nhưng các khái niệm nên có thể chuyển giao được.

</div>

1. [Chi tiết triển khai kiểm thử](https://kentcdodds.com/blog/testing-implementation-details) của Kent C. Dodds cho chúng ta thấy cách chúng ta có thể giảm các kết quả kiểm thử sai và các kiểm thử không linh hoạt bằng cách tránh kiểm thử việc triển khai của các thứ.
1. Hãy xem qua tất cả các phương thức truy vấn có sẵn trên [trang cheatsheet của React Testing Library](https://testing-library.com/docs/dom-testing-library/cheatsheet/). Không cần phải sử dụng tất cả, nhưng tối ưu là sử dụng một phương thức cụ thể cho một truy vấn cụ thể. Nếu không có phương thức truy vấn nào đủ, có thể sử dụng test id. Tìm hiểu về test id trên [tài liệu test id của React Testing Library](https://testing-library.com/docs/queries/bytestid/).
1. Đọc [tài liệu API userEvent](https://testing-library.com/docs/user-event/intro) để hiểu cách thực hiện mô phỏng người dùng.
1. Bài viết này về [Ưu và nhược điểm của các bài kiểm tra Snapshot](https://tsh.io/blog/pros-and-cons-of-jest-snapshot-tests/) đi sâu vào các lợi ích và bất lợi của việc kiểm tra snapshot. Và bài viết này, [Kiểm tra Snapshot: Lợi ích và Nhược điểm](https://www.sitepen.com/blog/snapshot-testing-benefits-and-drawbacks), giải thích rất tốt về việc kiểm tra snapshot dùng để làm gì trong lập trình nói chung.

</div>
