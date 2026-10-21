### Giới thiệu

Chúng ta đã đề cập đến một số kiến thức cơ bản về kiểm thử React. Bây giờ là lúc đi sâu hơn.

Trong bài học này, chúng ta sẽ tìm hiểu về việc giả lập (mocking). Hơn nữa, chúng ta sẽ thảo luận về một thành phần React từ trang web này ([theodinproject.com](https://theodinproject.com)) và hiểu cách các bài kiểm tra React được viết trong một ứng dụng thực tế.

### Tổng quan bài học

Phần này chứa tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thực hiện các bản giả lập trong bối cảnh kiểm thử React.

### Chế giễu là gì?

Nếu bạn đã theo dõi các bài học của chúng tôi cho đến nay, khái niệm về việc giả lập đã được giới thiệu trong một phần trước và bạn thậm chí có thể đã tích hợp các giả lập trong [dự án Battleship](https://www.theodinproject.com/lessons/javascript-battleship) của mình. Hãy cùng xem cách các giả lập sẽ giúp trong việc kiểm thử các thành phần React.

#### Kiểm tra các trình xử lý callback

Các callback rất phổ biến. Mọi hình thức tương tác của người dùng đều liên quan đến callback. Đôi khi chúng được truyền vào như các props để thay đổi trạng thái của component cha. Hãy xem xét component nút bấm này:

```jsx
// CustomButton.jsx

const CustomButton = ({ onClick }) => {
  return (
    <button onClick={onClick}>Click me</button> 
  );
};

export default CustomButton;
```

Chẳng có gì phức tạp. `CustomButton` là một component với một prop được truyền vào. Chúng tôi quan tâm tới prop `onClick`. Chúng tôi không biết hàm này làm gì. Chúng tôi không biết hàm này sẽ ảnh hưởng thế nào tới ứng dụng. Tất cả những gì chúng tôi biết là nó phải được gọi khi người dùng nhấn vào nút. Hãy thử nghiệm nó.

Hãy chú ý cách chúng ta chế nhạo và kiểm tra hàm `onClick`:

```jsx
// CustomButton.test.jsx

import { vi, describe, it, expect } from 'vitest'
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import CustomButton from "./CustomButton";

describe("CustomButton", () => {
  it("should render a button with the text 'Click me'", () => {
    render(<CustomButton onClick={() => {}} />);

    const button = screen.getByRole("button", { name: "Click me" });

    expect(button).toBeInTheDocument();
  });
  
  it("should call the onClick function when clicked", async () => {
    const onClick = vi.fn();
    const user = userEvent.setup()
    render(<CustomButton onClick={onClick} />);

    const button = screen.getByRole("button", { name: "Click me" });

    await user.click(button);

    expect(onClick).toHaveBeenCalled();
  });

  it("should not call the onClick function when it isn't clicked", () => {
    const onClick = vi.fn();
    render(<CustomButton onClick={onClick} />);

    expect(onClick).not.toHaveBeenCalled();
  });
});
```

Ba bài kiểm tra và chúng ta sẽ hoàn thành thành phần này. Bạn nên đã quen thuộc với cách hoạt động của bài kiểm tra đầu tiên. Hãy dành một chút thời gian để tìm hiểu chức năng nào đến từ gói nào.

Đối với bài kiểm tra thứ hai và thứ ba, chúng tôi giả lập trình xử lý `onClick` bằng một trong các hàm của Vitest, `vi.fn()`. Sau đó, chúng tôi xác nhận rằng nó được gọi/không được gọi khi nút được nhấn hoặc không.

Bạn cũng có thể thiết lập các mock của mình trong một khối `beforeEach` thay vì trong mỗi khối test. Điều này có thể phù hợp với một số tình huống. Tuy nhiên, để dễ đọc hơn, nên thực hiện tất cả các thiết lập trong cùng một khối test. Làm như vậy sẽ loại bỏ nhu cầu tìm kiếm toàn bộ tệp để lấy ngữ cảnh, giúp việc xem xét các thay đổi trong tương lai dễ dàng hơn. Điều này cũng giảm khả năng rò rỉ gây ra vấn đề trong toàn bộ bộ test. Trừ khi tệp test của bạn đặc biệt dài và việc chuẩn bị test chiếm hàng chục dòng, nên thiết lập các mock trong mỗi khối test; nếu không, bạn có thể sử dụng `beforeEach`.

Được khuyến nghị gọi `userEvent.setup()` trước khi render component. Không nên gọi các hàm render và `userEvent` bên ngoài chính bài kiểm tra, (ví dụ, trong một khối `beforeEach`). Nếu bạn thấy mình lặp lại cùng một mã trong nhiều bài kiểm tra, phương pháp khuyến nghị để rút ngắn mỗi bài kiểm tra là [viết một hàm setup](https://testing-library.com/docs/user-event/intro/#writing-tests-with-userevent).

#### Giả lập các thành phần con

Bạn có thể đã nghe qua khái niệm mô phỏng các module. Trong React, khi cây component trở nên lớn, các bài kiểm tra có thể trở nên phức tạp. Đặc biệt đối với các component ở mức cao hơn trong cây. Đó là lý do tại sao chúng ta mô phỏng các component con. Đây không phải là điều bạn thường gặp, tuy nhiên, việc hiểu được khái niệm này là có lợi trong trường hợp bạn có thể cần nó trong các thử nghiệm của riêng bạn.

### Kiểm thử React trong thế giới thực

Nếu bạn đang đăng nhập trên trang web này ([theodinproject.com](https://theodinproject.com)), có lẽ bạn đã từng thấy danh sách gửi dự án dưới mỗi dự án. Những thành phần đó được viết bằng React và được kiểm tra với React Testing Library. Chúng đã bị loại bỏ, nhưng vẫn là những ví dụ tuyệt vời. Điều này sẽ rất thú vị. Nhiệm vụ của bạn là:

Đọc và cố gắng hiểu component [submissions-list.jsx](https://github.com/TheOdinProject/theodinproject/blob/0886578d5b27a967e6bba2b31f212efe284d9413/app/javascript/components/project-submissions/components/submissions-list.jsx). Không sao nếu bạn không hiểu tất cả mọi thứ. Và tin tốt là chúng ta không cần phải hiểu hết để theo kịp bài học này!

#### danh-sách-bài-nộp.jsx

Chúng ta bắt đầu bằng cách nhập một loạt thứ giống như bất kỳ thành phần React nào khác. Có thể có một vài thứ hơi lạ; chúng ta không cần lo lắng quá nhiều về chúng. Nếu chúng ta nhìn lướt qua các props, có một vài hàm ở đó, có thể là các bộ xử lý sự kiện. Hãy xem thành phần này render những gì.

1. Nếu có `userSubmission`, nó sẽ hiển thị thành phần `Submission`.
1. Nếu `hasSubmissions` đúng, sắp xếp các bài nộp và hiển thị chúng với `Submission`. Ngược lại, một tiêu đề nói "Chưa có bài nộp nào, hãy là người đầu tiên!"
1. Nếu `allSubmissionsPath` đúng, nó sẽ hiển thị một thẻ `<p>`.

Chỉ cần xem qua mã, nó sẽ cho chúng ta một số ý tưởng về những gì cần kiểm tra. Sẽ rất hữu ích nếu bạn dành vài giây để lập sơ đồ những bài kiểm tra mà chúng ta có thể cần cho `SubmissionsList`.

Xem qua tệp kiểm thử của nó, [submissions-list.test.jsx](https://github.com/TheOdinProject/theodinproject/blob/0886578d5b27a967e6bba2b31f212efe284d9413/app/javascript/components/project-submissions/components/__tests__/submissions-list.test.jsx). Một lần nữa, đừng lo nếu tất cả mọi thứ chưa hiểu được, chúng ta sẽ bàn kỹ về nó ngay sau.

<div class="lesson-note" markdown="1">

#### Thư viện Kiểm thử React test ID

Trong khi bộ kiểm tra trên sử dụng `data-test-id` để xác định các thành phần con được giả lập, cần nhớ rằng React Testing Library mặc định sử dụng `data-testid`.

</div>

#### submissions-list.test.jsx

Chúng tôi nhận thấy có hai thành phần con của `SubmissionsList`. Một trong số đó đến từ một gói có tên `react-flip-move`. Mã bên ngoài. Chúng tôi sẽ giả lập nó.

Hãy chú ý cách chúng tôi chế giễu thành phần `Submission`:

```jsx
jest.mock('../submission', () => ({ submission, isDashboardView }) => (
  <>
    <div data-test-id="submission">{submission.id}</div>
    <div data-test-id="dashboard">{isDashboardView.toString()}</div>
  </>
));
```

Chúng tôi chỉ hiển thị mức tối thiểu cần thiết để xác nhận tính hợp lệ của thành phần mà chúng tôi đang kiểm tra. Tiếp theo, chúng tôi thiết lập các props với dữ liệu giả và các hàm giả lập.

<div class="lesson-note" markdown="1">

#### Lưu ý - Các mock của Vitest

Bài kiểm tra ở đây sử dụng `jest.mock()`. Nếu bạn đã làm theo và thiết lập bài kiểm tra của mình bằng Vitest, bạn có thể sử dụng `vi.mock()` thay thế. Xem [`vi.mock()` API](https://vitest.dev/api/vi.html#vi-mock).

</div>

Hãy chuyển sang khẳng định đầu tiên của chúng ta. Đừng quá lo lắng về `ProjectSubmissionContext.Provider`. Trong bối cảnh của bài kiểm tra này, mục đích của nó là đóng vai trò như một đường dẫn để truyền prop `allSubmissionsPath`. Chúng tôi đã xác định ba điểm quan tâm mà chúng tôi muốn kiểm tra. Chúng tôi chia chúng thành ba bộ kiểm tra để dễ đọc bằng cách sử dụng `describe`.

Trong bộ kiểm thử đầu tiên, chúng tôi thực hiện một số khẳng định nếu người dùng có một bản nộp và sau đó một số khẳng định nếu người dùng không có. Các bộ kiểm thử khác theo một mô hình tương tự.

#### Khám phá thêm

Hãy thoải mái lướt qua các thành phần khác và các bài kiểm tra của chúng. Bạn sẽ thấy các hàm giả lập. Bạn sẽ thấy một số hàm chưa từng thấy như `act`. Bạn sẽ thấy các hàm render tùy chỉnh. Không sao nếu bạn không hiểu hết, mục tiêu là làm quen.

Điều quan trọng khác cần lưu ý là hầu hết các bài kiểm tra đều theo một mô hình nhất định về cách chúng được viết. Chúng theo mô hình [Arrange-Act-Assert](http://wiki.c2.com/?ArrangeActAssert). Nên sớm hay muộn cũng nên áp dụng mô hình này để làm cho các bài kiểm tra của bạn dễ đọc hơn và cuối cùng là tốt hơn.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết này về [giả lập các component con](https://medium.com/@taylormclean15/jest-testing-mocking-child-components-to-make-your-unit-tests-more-concise-18691ef6a0c2). Nó bao quát chi tiết về cách thức giả lập các component con. (Bạn có thể cần đăng ký để đọc)
1. [Hướng dẫn về Testing React Apps của Academind](https://academind.com/tutorials/testing-react-apps) này là một tổng quan tuyệt vời về những gì bạn đã học. API `userEvent` trong hướng dẫn này là đồng bộ, không giống như API bất đồng bộ hiện tại của chúng ta. Tuy nhiên, bạn vẫn có thể theo dõi bằng cách sử dụng kiến thức trước đây của mình và bạn vẫn có thể sử dụng Vite và Vitest như trước.

</div>
