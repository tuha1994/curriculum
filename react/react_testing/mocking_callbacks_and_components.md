### Giới thiệu

Chúng ta đã đề cập đến một số vấn đề cơ bản về thử nghiệm React. Bây giờ là lúc để lặn sâu hơn.

Trong bài học này chúng ta sẽ tìm hiểu về chế nhạo. Hơn nữa, chúng ta sẽ thảo luận về thành phần React từ ([theodinproject.com](https://theodinproject.com)) và hiểu cách viết bài kiểm tra React trong ứng dụng thực tế.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thực hiện mô phỏng trong bối cảnh thử nghiệm React.

### Chế nhạo là gì?

Nếu bạn đã theo dõi các bài học của chúng tôi cho đến giờ thì khái niệm chế nhạo đã được giới thiệu trong phần trước và bạn thậm chí có thể đã kết hợp các mô phỏng trong [Dự án tàu chiến](https://www.theodinproject.com/lessons/javascript-battleship). Hãy xem mô phỏng sẽ giúp ích như thế nào trong việc thử nghiệm các thành phần React.

#### Kiểm tra trình xử lý gọi lại

Cuộc gọi lại có mặt khắp nơi. Mọi con đường tương tác của người dùng đều liên quan đến các cuộc gọi lại. Đôi khi chúng được chuyển vào dưới dạng đạo cụ để thay đổi trạng thái của thành phần gốc. Hãy xem xét thành phần nút này:

```jsx
// CustomButton.jsx

const CustomButton = ({ onClick }) => {
  return (
    <button onClick={onClick}>Click me</button> 
  );
};

export default CustomButton;
```

Không có gì lạ mắt. `CustomButton` là một thành phần có prop được truyền vào. Chúng tôi quan tâm đến prop `onClick`. Chúng tôi không biết chức năng này làm gì. Chúng tôi không biết chức năng này sẽ ảnh hưởng đến ứng dụng như thế nào. Tất cả những gì chúng tôi biết là nó phải được gọi khi người dùng nhấp vào nút. Hãy kiểm tra nó.

Lưu ý cách chúng tôi mô phỏng và kiểm tra hàm `onClick`:

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

Ba bài kiểm tra và chúng tôi đã hoàn thành thành phần này. Bạn hẳn đã quen với cách hoạt động của bài kiểm tra đầu tiên. Hãy dành chút thời gian để tìm hiểu chức năng nào đến từ gói nào.

Đối với thử nghiệm thứ hai và thứ ba, chúng tôi thử nghiệm trình xử lý `onClick` bằng cách sử dụng một trong các hàm của Vitest, `vi.fn()`. Sau đó, chúng tôi khẳng định rằng nó được gọi called/not khi nút được nhấp vào hay không.

Bạn cũng có thể thiết lập mô hình của mình trong khối `beforeEach` thay vì trong mọi khối kiểm tra. Điều này có thể phù hợp với một số tình huống. Tuy nhiên, để dễ đọc hơn, nên thực hiện tất cả các thiết lập trong cùng một khối kiểm tra.  Làm như vậy sẽ loại bỏ nhu cầu tìm kiếm ngữ cảnh trong toàn bộ tệp, giúp việc xem lại các thay đổi trong tương lai trở nên dễ dàng hơn. Điều này cũng làm giảm nguy cơ rò rỉ gây ra sự cố trong toàn bộ bộ thử nghiệm. Trừ khi tệp kiểm tra của bạn đặc biệt dài và phần chuẩn bị kiểm tra chiếm hàng chục dòng, bạn nên thiết lập các mô hình của mình trong mỗi khối kiểm tra; nếu không, bạn có thể sử dụng `beforeEach`.

Bạn nên gọi `userEvent.setup()` trước khi hiển thị thành phần. Bạn không nên gọi các hàm hiển thị và hàm `userEvent` bên ngoài bản thử nghiệm (ví dụ: trong khối `beforeEach`). Nếu bạn thấy mình lặp lại cùng một mã trong nhiều lần kiểm tra, phương pháp được đề xuất để rút ngắn mỗi lần kiểm tra là [viết hàm thiết lập](https://testing-library.com/docs/user-event/intro/#writing-tests-with-userevent).

#### Nhạo báng các thành phần con

Bạn có thể đã gặp khái niệm mô-đun mô phỏng. Trong React, khi cây thành phần trở nên lớn, các bài kiểm tra có thể trở nên phức tạp. Đặc biệt là đối với các thành phần cao hơn trên cây. Đó là lý do tại sao chúng tôi mô phỏng các thành phần con. Đây không phải là thứ bạn sẽ gặp thường xuyên, tuy nhiên, sẽ rất hữu ích nếu bạn biết khái niệm này trong trường hợp bạn có thể cần nó trong mục đích thử nghiệm của riêng mình.

### React thử nghiệm trong thế giới thực

Nếu bạn đã đăng nhập vào đây ([theodinproject.com](https://theodinproject.com)), có thể bạn đã từng xem qua danh sách các dự án đã nộp trong mỗi dự án. Các thành phần đó được viết bằng React và được thử nghiệm với Thư viện thử nghiệm React. Chúng đã bị loại bỏ kể từ đó, nhưng chúng vẫn đóng vai trò là những ví dụ tuyệt vời. Sẽ rất vui đây. Nhiệm vụ của bạn là:

Đọc và cố gắng hiểu nội dung [bài nộp-list.jsx](https://github.com/TheOdinProject/theodinproject/blob/0886578d5b27a967e6bba2b31f212efe284d9413/app/javascript/components/project-submissions/components/submissions-list.jsx) thành phần. Sẽ không sao nếu bạn không hiểu mọi thứ. Và tin tốt là chúng ta không cần phải hiểu hết để theo kịp bài học này!

#### bài nộp-list.jsx

Chúng tôi bắt đầu bằng cách nhập một loạt nội dung giống như bất kỳ thành phần React phù hợp nào khác. Có thể có một số thứ không quen thuộc ở đó; chúng ta không phải lo lắng quá nhiều về họ. Nếu chúng ta nhìn qua các props, chúng ta sẽ thấy có một số hàm trong đó, có lẽ là các hàm xử lý sự kiện. Hãy xem thành phần này hiển thị những gì.

1. Nếu có `userSubmission`, nó sẽ hiển thị thành phần `Submission`.
1. Nếu `hasSubmissions` là đúng, hãy sắp xếp nội dung gửi và hiển thị chúng bằng `Submission`. Nếu không, tiêu đề có nội dung "Chưa có bài gửi nào, hãy là người đầu tiên!"
1. Nếu `allSubmissionsPath` là đúng, nó sẽ hiển thị thẻ `<p>`.

Chỉ cần xem qua mã, nó sẽ cho chúng ta một số ý tưởng về những gì cần kiểm tra. Sẽ rất bổ ích nếu bạn dành vài giây để vạch ra những bài kiểm tra mà chúng tôi có thể cần cho `SubmissionsList`.

Đi qua tập tin thử nghiệm của nó,[bài nộp-list.test.jsx](https://github.com/TheOdinProject/theodinproject/blob/0886578d5b27a967e6bba2b31f212efe284d9413/app/javascript/components/project-submissions/components/__tests__/submissions-list.test.jsx). Một lần nữa, đừng lo lắng nếu tất cả những điều đó không có ý nghĩa gì, chúng ta sẽ xem xét nó ngay sau đây.

<div class="lesson-note" markdown="1">

#### React Thư viện kiểm tra ID kiểm tra

Mặc dù bộ thử nghiệm ở trên sử dụng `data-test-id` để xác định các thành phần con bị mô phỏng, nhưng phải nhớ rằng Thư viện thử nghiệm React thay vào đó sử dụng `data-testid` theo mặc định.

</div>

#### bài nộp-list.test.jsx

Chúng tôi nhận thấy có hai thành phần con của `SubmissionsList`. Một trong số đó là từ gói có tên `react-flip-move`. Mã bên ngoài. Chúng ta sẽ chế nhạo nó.

Lưu ý cách chúng tôi mô phỏng thành phần `Submission`:

```jsx
jest.mock('../submission', () => ({ submission, isDashboardView }) => (
  <>
    <div data-test-id="submission">{submission.id}</div>
    <div data-test-id="dashboard">{isDashboardView.toString()}</div>
  </>
));
```

Chúng tôi chỉ hiển thị mức tối thiểu để nhận ra tính hợp lệ của thành phần mà chúng tôi đang thử nghiệm. Tiếp theo, chúng tôi thiết lập đạo cụ của mình với dữ liệu giả và các hàm mô phỏng.

<div class="lesson-note" markdown="1">

#### Lưu ý - Vitest mocks

Bài kiểm tra ở đây sử dụng `jest.mock()`. Nếu bạn làm theo và thiết lập bài kiểm tra của mình bằng Vitest, bạn có thể sử dụng `vi.mock()` để thay thế. Nhìn thấy [`vi.mock()` API](https://vitest.dev/api/vi.html#vi-mock).

</div>

Hãy chuyển sang khẳng định đầu tiên của chúng ta. Đừng lo lắng quá nhiều về `ProjectSubmissionContext.Provider`. Trong bối cảnh của thử nghiệm này, mục đích của nó là hoạt động như một lộ trình để vượt qua trong prop `allSubmissionsPath`. Chúng tôi đã xác định được ba điểm quan tâm mà chúng tôi muốn kiểm tra. Chúng tôi chia chúng thành ba bộ thử nghiệm nhằm mục đích dễ đọc bằng cách sử dụng `describe`.

Trong bộ đầu tiên, chúng tôi đưa ra một số xác nhận nếu người dùng có nội dung gửi và sau đó là một số xác nhận nếu người dùng không gửi. Các dãy khác cũng theo mô hình tương tự.

#### Khám phá thêm

Hãy thoải mái lướt qua các thành phần khác và các bài kiểm tra của chúng. Bạn sẽ thấy các chức năng bị mô phỏng. Bạn sẽ thấy một số chức năng chưa được nhìn thấy như `act`. Bạn sẽ thấy các chức năng kết xuất tùy chỉnh. Không hiểu hết cũng không sao, mục tiêu là làm quen.

Điều quan trọng khác cần lưu ý là hầu hết tất cả các bài kiểm tra đều tuân theo một khuôn mẫu nhất định về cách chúng được viết. Họ làm theo [Sắp xếp-Hành động-Khẳng định](http://wiki.c2.com/?ArrangeActAssert) mẫu. Bạn nên áp dụng mô hình này sớm hay muộn để làm cho bài kiểm tra của bạn dễ đọc hơn và cuối cùng là tốt hơn.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc bài viết này về [chế nhạo các thành phần con](https://medium.com/@taylormclean15/jest-testing-mocking-child-components-to-make-your-unit-tests-more-concise-18691ef6a0c2). Nó bao gồm rộng rãi "cách" chế nhạo các thành phần con. (Bạn có thể cần phải đăng ký để đọc)
1. Cái này [hướng dẫn về Thử nghiệm ứng dụng React của Academind](https://academind.com/tutorials/testing-react-apps) là một cái nhìn tổng quan tuyệt vời về những gì bạn đã học được. `userEvent` API trong hướng dẫn này là đồng bộ, không giống như bất đồng bộ hiện tại của chúng tôi. Tuy nhiên, bạn có thể tiếp tục sử dụng kiến ​​thức trước đây của mình và vẫn có thể sử dụng Vite và Vitest như trước.

</div>
