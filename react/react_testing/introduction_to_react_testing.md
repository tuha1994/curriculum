### Giới thiệu

Chúng ta đã học cách kiểm thử các ứng dụng vanilla JavaScript ở phần trước. Kiểm tra là một công cụ mạnh mẽ cho phép chúng ta viết mã linh hoạt và có thể bảo trì. Nếu bạn đã theo dõi các bài học của chúng tôi thì cho đến giờ chúng tôi đã sử dụng [trò đùa](https://jestjs.io/) khuôn khổ. Tuy nhiên, vì chúng tôi đang sử dụng [Vite](https://vitejs.dev/) chúng ta sẽ chuyển sang [Vitest](https://vitest.dev/) làm người chạy thử nghiệm của chúng tôi vì nó tích hợp độc đáo với Vite. Chúng tôi sẽ bổ sung thêm nhiều khả năng hơn cho các thử nghiệm của mình bằng cách sử dụng [React Thư viện thử nghiệm (RTL)](https://testing-library.com/docs/react-testing-library/intro/).

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Tại sao thử nghiệm UI lại có giá trị.
- Cách thiết lập môi trường thử nghiệm React.
- Cách kiểm tra các phần tử UI.
- Hiểu các bài kiểm tra ảnh chụp nhanh.

### UI thử nghiệm

Đến bây giờ, bạn đã có một số kinh nghiệm với TDD và ý tưởng thử nghiệm nói chung. Bạn có thể nhớ trong dự án Battleship, bạn sẽ có các bài kiểm tra viết giới hạn trong trò chơi Battleship cơ bản nhưng hoàn toàn không liên quan đến DOM. Bạn có thể đã thực hiện các thay đổi không phải UI, khiến các thử nghiệm thất bại, sau đó giải quyết bất cứ điều gì cần thiết để bạn có các thử nghiệm phù hợp và vượt qua.

Mặc dù vậy, bạn có thể đã gặp phải lỗi trong UI vì các thử nghiệm của bạn chỉ liên quan đến logic cơ bản. Bạn có thể có một trò chơi Battleship đang hoạt động nhưng điều đó không có nghĩa là UI thực sự hiển thị những gì bạn dự định cũng như không cho phép người dùng tương tác với trò chơi như dự định. Và thành thật mà nói, mỗi khi bạn thực hiện thay đổi đối với bất kỳ phần nào trong mã của mình (dù có liên quan đến UI hay không), bạn sẽ không nhớ kiểm tra lại mọi khía cạnh UI có liên quan để đảm bảo mọi thứ vẫn hoạt động như bạn mong đợi. tức là, điều này không khác gì trước đây, bây giờ chúng tôi chỉ liên quan đến UI.

Các thử nghiệm UI giúp chúng tôi tin tưởng hơn rằng các trang web của chúng tôi chứa nội dung dự định và hoạt động như chúng tôi muốn, đồng thời thông báo cho chúng tôi khi có nội dung nào đó không còn đáp ứng được yêu cầu của chúng tôi. Có lẽ bạn quyết định thay đổi cấu trúc chính xác của một trạng thái, chẳng hạn như thay đổi từ một đối tượng đơn giản thành một mảng, nhưng bây giờ danh sách không chứa văn bản bạn dự định. Hoặc có thể bạn thêm một điều kiện ở đâu đó trong một thành phần và đột nhiên, chỉ có ba mục tiêu thả cuối cùng cho thẻ kéo và thả của bạn không còn là mục tiêu hợp lệ. Khi trang web của bạn trở nên phức tạp hơn, giá trị của các thử nghiệm tốt (cả UI và non-UI) sẽ chỉ tăng lên.

### Thiết lập môi trường thử nghiệm React

Theo dõi [Hướng dẫn của Robin Wieruch về cách thiết lập Vitest với RTL](https://www.robinwieruch.de/vitest-react-testing-library/). Sau khi thiết lập xong, chúng ta hãy gặp lại nhau ở đây.

<div class="lesson-note" markdown="1">

#### Vitest toàn cầu và ESLint

Ngay cả khi bạn đặt `globals: true` trong `vite.config.js` như trong hướng dẫn thiết lập, ESLint vẫn sẽ la mắng bạn vì nó sẽ không nhận ra những hình cầu này nếu không có một số cấu hình bổ sung trong tệp `eslint.config.js` của bạn. Thay vào đó, giải pháp đơn giản nhất là nhập rõ ràng các hình cầu mà bạn cần. Bạn có thể bỏ `globals: true` khỏi `vite.config.js` trong trường hợp này.

</div>

Phù, đó là rất nhiều thiết lập. Nhưng còn một gói nhỏ nữa cần cài đặt trước khi chúng ta có thể bắt đầu:

```bash
npm install @testing-library/user-event --save-dev
```

Bây giờ chúng ta đã có mọi thứ mình cần, hãy xem nhanh chức năng của một số gói đó. Chúng tôi sẽ chủ yếu tập trung vào các gói `@testing-library`.

- `@testing-library/react` sẽ cung cấp cho chúng ta quyền truy cập vào các chức năng hữu ích như `render` mà chúng ta sẽ trình bày sau.

- `@testing-library/jest-dom` bao gồm một số công cụ so khớp tùy chỉnh tiện dụng (hàm xác định) như `toBeInTheDocument` và hơn thế nữa. (danh sách đầy đủ trên [github của jest-dom](https://github.com/testing-library/jest-dom)). Jest đã có rất nhiều matcher nên gói này không bắt buộc phải sử dụng.

- `@testing-library/user-event` cung cấp `userEvent` API mô phỏng tương tác của người dùng với trang web.

### Truy vấn đầu tiên của chúng tôi

Đầu tiên, chúng ta sẽ kết xuất thành phần bằng cách sử dụng `render` (bạn có thể đọc tất cả về những gì hàm này có thể làm trong [Thư viện thử nghiệm React API tài liệu về `render`](https://testing-library.com/docs/react-testing-library/api/#render)). Chúng tôi nói "kết xuất", nhưng bạn sẽ không thấy bất kỳ màn hình nào. Quay lại bài viết thiết lập RTL vừa rồi, một trong các gói được cài đặt là `jsdom`, mô phỏng DOM (bao gồm các sự kiện) trong bộ nhớ mà không thực sự sắp xếp bất kỳ thứ gì một cách trực quan giống như trong trình duyệt và điều đó cho phép chúng tôi phân tích nội dung của nó cho các thử nghiệm của mình. Điều này khác và ít phức tạp hơn nhiều so với các thư viện khác thực sự chạy môi trường trình duyệt thực cho các thử nghiệm toàn diện phức tạp hơn - điều đó nằm ngoài phạm vi ở đây.

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

Thực thi `npm test App.test.jsx` trên thiết bị đầu cuối và xem vượt qua bài kiểm tra. `getByRole` chỉ là một trong hàng chục phương thức truy vấn mà chúng tôi có thể sử dụng. Về cơ bản, các truy vấn được phân thành ba loại: `getBy`, `queryBy` và `findBy`. Đi qua [trang tài liệu Thư viện thử nghiệm React về các truy vấn](https://testing-library.com/docs/queries/about/). Hãy chú ý hơn đến phần "Loại truy vấn" và "Ưu tiên".

Như đã nêu trong tài liệu Thư viện thử nghiệm React, các phương thức `ByRole` là các phương thức được ưa chuộng để truy vấn, đặc biệt khi được ghép nối với tùy chọn `name`. Ví dụ: chúng ta có thể cải thiện tính đặc hiệu của truy vấn trên như sau: `getByRole("heading", { name: "Our First Test" })`. Các truy vấn được thực hiện thông qua `ByRole` đảm bảo rằng UI của chúng tôi có thể truy cập được đối với mọi người bất kể họ sử dụng chế độ nào để điều hướng trang web (tức là chuột hoặc công nghệ hỗ trợ).

### Mô phỏng sự kiện của người dùng

Có rất nhiều cách người dùng có thể tương tác với một trang web. Mặc dù phản hồi và tương tác trực tiếp của người dùng là không thể thay thế, nhưng chúng tôi vẫn có thể tạo dựng niềm tin vào các thành phần của mình thông qua các thử nghiệm. Đây là nút thay đổi tiêu đề của Ứng dụng:

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

Hãy kiểm tra xem nút có hoạt động như dự định không. Trong bộ thử nghiệm này, chúng tôi sẽ sử dụng một tiện ích riêng để truy vấn các phần tử UI của chúng tôi. Thư viện thử nghiệm React cung cấp đối tượng `screen` có tất cả các phương thức truy vấn.

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

Các bài kiểm tra nói lên điều đó. Trong thử nghiệm đầu tiên, chúng tôi sử dụng ảnh chụp nhanh để kiểm tra xem tất cả các nút có hiển thị như chúng tôi mong đợi hay không. Trong thử nghiệm thứ hai, chúng tôi mô phỏng một sự kiện nhấp chuột. Sau đó chúng tôi kiểm tra xem tiêu đề có thay đổi không. `toMatch` là một trong những khẳng định khác nhau mà lẽ ra chúng tôi có thể đưa ra. Lưu ý rằng hàm gọi lại cho thử nghiệm thứ hai là `async`, vì chúng ta cần hàm này để `await user.click()`.

Điều quan trọng cần lưu ý là sau mỗi lần kiểm tra, Thư viện kiểm tra React sẽ ngắt kết nối các thành phần được kết xuất. Đó là lý do tại sao chúng tôi kết xuất cho mỗi bài kiểm tra. Đối với nhiều thử nghiệm cho một thành phần, hàm `setup` tùy chỉnh có thể tỏ ra hữu ích.

### Ảnh chụp nhanh là gì?

Kiểm tra ảnh chụp nhanh chỉ là so sánh thành phần được kết xuất của chúng tôi với tệp ảnh chụp nhanh được liên kết. Ví dụ: tệp ảnh chụp nhanh được tạo tự động sau khi chúng tôi chạy*"tạo ra những chú khỉ tuyệt đẹp"*bài kiểm tra là:

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

Đó là đại diện HTML của thành phần `App`. Và nó sẽ được so sánh với `App` trong các xác nhận ảnh chụp nhanh trong tương lai. Nếu `App` thay đổi dù chỉ một chút thì thử nghiệm sẽ thất bại.

Bài kiểm tra ảnh chụp nhanh rất nhanh và dễ viết. Một xác nhận giúp chúng ta không phải viết nhiều dòng mã. Ví dụ: với `toMatchSnapshot`, chúng ta không cần phải xác nhận sự tồn tại của nút và tiêu đề. Họ cũng không để những thay đổi bất ngờ len lỏi vào mã của chúng tôi. Đọc tất cả về những gì có thể đạt được với ảnh chụp nhanh trong [Tài liệu chụp nhanh Vitest](https://vitest.dev/guide/snapshot.html).

Ảnh chụp nhanh có vẻ là điều tốt nhất đã xảy ra với chúng tôi khi thử nghiệm cho đến nay. Nhưng chúng ta buộc phải tự hỏi,*Gì* chính xác thì chúng ta đang thử nghiệm phải không? Điều gì đang được xác thực? Nếu một ảnh chụp nhanh trôi qua, nó truyền tải điều gì về tính chính xác của thành phần?

Kiểm tra ảnh chụp nhanh có thể gây ra kết quả dương tính giả. Vì chúng tôi không thể xác định tính hợp lệ của thành phần từ kiểm tra ảnh chụp nhanh nên lỗi có thể không bị phát hiện. Việc phụ thuộc quá nhiều vào ảnh chụp nhanh có thể khiến các nhà phát triển tự tin hơn về mã của họ hơn mức cần thiết.

Vấn đề khác với ảnh chụp nhanh là kết quả âm tính giả. Ngay cả những thay đổi không đáng kể nhất cũng có thể khiến thử nghiệm thất bại. Sửa dấu câu? Ảnh chụp nhanh sẽ thất bại. Thay thế thẻ HTML thành thẻ có ngữ nghĩa hơn? Ảnh chụp nhanh sẽ thất bại. Điều này có thể khiến chúng tôi mất niềm tin hoàn toàn vào bộ thử nghiệm. Ảnh chụp nhanh vốn dĩ không tệ; họ phục vụ một mục đích. Nhưng sẽ rất hữu ích khi hiểu khi nào nên chụp nhanh và khi nào không chụp nhanh.

### Phân công

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Tập trung vào các khái niệm

Mặc dù một số bài viết sử dụng thư viện thử nghiệm Jest và Enzyme, nhưng các khái niệm này vẫn có thể chuyển giao được.

</div>

1. [Chi tiết triển khai thử nghiệm](https://kentcdodds.com/blog/testing-implementation-details) của Kent C. Dodds cho chúng ta thấy cách chúng ta có thể giảm kết quả kiểm tra sai và kiểm tra thiếu linh hoạt bằng cách tránh kiểm tra việc triển khai mọi thứ.
1. Hãy xem qua tất cả các phương pháp truy vấn có sẵn trên [trang bảng tóm tắt của Thư viện thử nghiệm React](https://testing-library.com/docs/dom-testing-library/cheatsheet/). Không cần phải sử dụng tất cả nhưng sẽ tối ưu nếu sử dụng một phương pháp cụ thể cho một truy vấn cụ thể. Nếu không có phương thức truy vấn nào đủ thì sẽ có tùy chọn sử dụng id kiểm tra. Tìm hiểu về id thử nghiệm trên [tài liệu id kiểm tra của Thư viện thử nghiệm React](https://testing-library.com/docs/queries/bytestid/).
1. Đọc [tài liệu userEvent API](https://testing-library.com/docs/user-event/intro) để có được cảm nhận về cách đạt được sự mô phỏng của người dùng.
1. Bài viết này trên [Ưu và nhược điểm của kiểm tra ảnh chụp nhanh](https://tsh.io/blog/pros-and-cons-of-jest-snapshot-tests/) đi sâu về những ưu điểm và nhược điểm của thử nghiệm ảnh chụp nhanh. Và cái này,[Kiểm tra ảnh chụp nhanh: Lợi ích và hạn chế](https://www.sitepen.com/blog/snapshot-testing-benefits-and-drawbacks), thực hiện rất tốt việc giải thích thử nghiệm chụp nhanh là gì đối với lập trình nói chung.

</div>
