### Giới thiệu

Đến đây, chương trình đã giúp chúng ta xây dựng các ứng dụng một trang. Nhưng những ứng dụng lớn hơn thường có nhiều trang. May mắn là trình duyệt cho phép JavaScript phía máy khách quản lý cách người dùng điều hướng thông qua [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API). Ta có thể tận dụng cơ chế này để định tuyến trong React với sự trợ giúp của các thư viện như React Router.

### Tổng quan bài học

Phần này tóm lược những nội dung bạn sẽ học trong bài.

- Hiểu định tuyến phía máy khách là gì.
- Tìm hiểu cách sử dụng Bộ định tuyến React để định tuyến phía máy khách.
- Khám phá cách tạo đường dẫn lồng nhau và động trong Bộ định tuyến React.
- Khám phá cách thêm tuyến đường 'bắt tất cả' trong Bộ định tuyến React.
- Tìm hiểu cách truyền dữ liệu từ thành phần gốc tới bất kỳ thành phần con nào được hiển thị qua ổ cắm.
- Hiểu cách triển khai các tuyến được bảo vệ trong Bộ định tuyến React.
- Nhận phần giới thiệu về các thành phần thử nghiệm sử dụng Bộ định tuyến React.

### Định tuyến phía máy khách

Định tuyến phía máy khách là kiểu định tuyến trong đó JavaScript xử lý các route của ứng dụng. Nhờ đó, ta có thể xây dựng ứng dụng một trang (SPA) mà không cần tải lại toàn bộ trang khi người dùng điều hướng. Ví dụ, khi người dùng nhấp vào một liên kết trên thanh điều hướng, URL thay đổi và ứng dụng cập nhật phần giao diện tương ứng.

Giả sử bạn đang nấu một ít thịt gà. Muốn nấu ngon và ngon thì bạn phải:

1. Cho gà vào lò nướng và đặt chế độ nấu với thời gian và nhiệt độ thích hợp
1. Đợi đến khi món ăn tỏa ra mùi thơm sảng khoái
1. Bắt đầu nhai!

Điều này là phổ biến đối với tất cả các trang web, bạn thiết lập sẵn những gì bạn muốn (truy cập bất kỳ URL nào, như [https://theodinproject.com/](https://theodinproject.com/)), đợi lò nấu xong (màn hình tải) và tada, hãy thưởng thức món ăn ngon của bạn (trang của bạn đã sẵn sàng để sử dụng). Nhưng nếu bạn quên thêm một số gia vị trước khi nấu thì sao? Bạn phải lặp lại quy trình này một lần nữa:

1. Hãy đứng dậy khỏi chỗ ngồi của bạn
1. Thêm gia vị vào thịt gà
1. Quay trở lại lò nướng, cho gà vào và chuẩn bị hâm nóng lại
1. Đợi nó đẹp và ấm áp
1. Bây giờ bạn có thể ăn nó!

Đây là điểm khác biệt quan trọng: **bạn phải đứng dậy khỏi chỗ ngồi** trong một ứng dụng nhiều trang (MPA), vì trình duyệt tải lại mỗi lần bạn nhấp vào liên kết để điều hướng. Với định tuyến phía máy khách, **bạn không rời khỏi trang hiện tại**—JavaScript chặn các yêu cầu điều hướng và tự cập nhật giao diện thay vì gửi yêu cầu trực tiếp lên máy chủ.

### Một giải pháp phản ứng

Mặc dù định tuyến phía máy khách cho phép các tương tác đẹp hơn, giống như ứng dụng (vì bạn đang kiểm soát các tuyến đường nên bạn có thể tạo các hoạt ảnh CSS lạ mắt khi thay đổi tuyến đường), nhưng rất nhiều cảnh báo có thể bị bỏ qua. Khi trình duyệt tải lại, trình duyệt sẽ thông báo cho trình đọc màn hình về nội dung mới cần đọc, nhưng trong trường hợp định tuyến phía máy khách, bạn sẽ cần thông báo cho trình đọc màn hình về các cập nhật tuyến theo cách thủ công. Tuy nhiên, với sự trợ giúp của một thư viện mạnh mẽ, bạn thường có thể giải quyết những mối lo ngại này!

React Router là thư viện định tuyến tiêu chuẩn cho các ứng dụng React. Bằng cách sử dụng Bộ định tuyến React, chúng ta có thể chỉ định các thành phần React, có thể được hiển thị dựa trên tuyến đường, v.v. Hãy đi sâu vào!

### Thêm bộ định tuyến

Hãy tạo một ứng dụng nhỏ để hiểu cách triển khai bộ định tuyến này. Tạo một dự án React mới và bắt đầu bằng cách thêm một số trang mô phỏng làm ví dụ. Tạo một tệp `Profile.jsx` mới với thành phần sau:

```jsx
const Profile = () => {
  return (
    <div>
      <h1>Hello from profile page!</h1>
      <p>So, how are you?</p>
    </div>
  );
};

export default Profile;
```

Thay thế tệp `App.jsx` bằng một số nội dung cơ bản:

```jsx
const App = () => {
  return (
    <div>
      <h1>Hello from the main page of the app!</h1>
      <p>Here are some examples of links to other pages</p>
      <nav>
        <ul>
          <li>
            <a href="profile">Profile page</a>
          </li>
        </ul>
      </nav>
    </div>
  );
};

export default App;
```

Bây giờ là lúc thêm bộ định tuyến! Có một số cách để xác định lộ trình của ứng dụng, chúng tôi sẽ thực hiện theo cách tiếp cận dựa trên đối tượng.

Hãy để chúng tôi cài đặt gói Bộ định tuyến React:

`npm install react-router`

Thêm phần sau vào `main.jsx`, chúng ta sẽ nói về những gì đang xảy ra sau.

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import App from "./App";
import Profile from "./Profile";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "profile",
    element: <Profile />,
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);
```

Sau khi hoàn tất việc này, hãy tiếp tục và chạy `npm run dev` và kiểm tra cả hai tuyến: tuyến nhà `/` và tuyến hồ sơ `/profile`. Nó hoạt động! Nhưng chuyện gì đang xảy ra ở đây vậy?

1. Chúng tôi nhập `createBrowserRouter` và `RouterProvider` từ Bộ định tuyến React.
1. `createBrowserRouter` được sử dụng để tạo cấu hình cho bộ định tuyến bằng cách truyền các đối số dưới dạng một mảng các tuyến đường.
1. Mảng cấu hình chứa các đối tượng có hai khóa bắt buộc là đường dẫn và phần tử tương ứng sẽ được hiển thị.
1. Sau đó, cấu hình được tạo này sẽ được hiển thị bằng cách chuyển nó tới thành phần `RouterProvider`.

### Yếu tố liên kết

Nhưng bạn có thể nhận thấy, khi chúng ta nhấp vào các liên kết trong thanh điều hướng, trình duyệt sẽ tải lại cho URL tiếp theo thay vì sử dụng Bộ định tuyến React. Đây không phải là những gì đã hứa! Để trợ giúp việc này, React Router xuất một tập tin tùy chỉnh [Thành phần liên kết](https://reactrouter.com/api/components/Link) được sử dụng thay cho thẻ `a` thông thường. Chúng ta có thể thay thế thẻ `a` trong thanh điều hướng bằng phần tử `Link`.

```jsx
import { Link } from "react-router";

const App = () => {
  return (
    <div>
      <h1>Hello from the main page of the app!</h1>
      <p>Here are some examples of links to other pages</p>
      <nav>
        <ul>
          <li>
            <Link to="profile">Profile page</Link>
          </li>
        </ul>
      </nav>
    </div>
  );
};

export default App;
```

Và bây giờ, chúng tôi không tải lại trình duyệt mỗi khi chúng tôi nhấp vào liên kết trên thanh điều hướng!

### Các tuyến đường, cửa hàng và phân khúc động được lồng nhau

Bây giờ, điều gì sẽ xảy ra nếu bạn muốn hiển thị một phần của trang theo cách khác, dựa trên URLs khác? Đây là nơi các tuyến đường lồng nhau phát huy tác dụng! Chúng ta có thể thêm các tuyến được lồng vào nhau như các tuyến con của nhau để đảm bảo rằng tuyến con được hiển thị cùng với tuyến cha. Tạo một vài thành phần, `Popeye.jsx` và `Spinach.jsx`.

```jsx
import { Link } from "react-router";

const Popeye = () => {
  return (
    <>
      <p>Hi, I am Popeye! I love to eat Spinach!</p>
      <Link to="/">Click here to go back</Link>
    </>
  );
};

export default Popeye;
```

```jsx
import { Link } from "react-router";

const Spinach = () => {
  return (
    <>
      <p>Hi, I am Spinach! Popeye loves to eat me!</p>
      <Link to="/">Click here to go back</Link>
    </>
  );
};

export default Spinach;
```

Bây giờ, chúng ta có thể viết lại các tuyến đường như đã cho:

```jsx
import { StrictMode } from "react"
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import App from "./App";
import Profile from "./Profile";
import Spinach from "./Spinach";
import Popeye from "./Popeye";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "profile",
    element: <Profile />,
    children: [
      { path: "spinach", element: <Spinach /> },
      { path: "popeye", element: <Popeye /> },
    ],
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>,
);
```

Điều này cho phép chúng ta kết xuất thành phần con cùng với thành phần cha, thông qua một [Thành phần đầu ra](https://reactrouter.com/api/components/Outlet)! Chúng ta có thể viết lại thành phần Hồ sơ để thêm `Outlet` sẽ được thay thế bằng các hồ sơ khác nhau khi tuyến đường đó được truy cập!

```jsx
import { Outlet } from "react-router";

const Profile = () => {
  return (
    <div>
      <h1>Hello from profile page!</h1>
      <p>So, how are you?</p>
      <hr />
      <h2>The profile visited is here:</h2>
      <Outlet />
    </div>
  );
};

export default Profile;
```

Hãy xem trang `/profile`. Để truy cập các trang `/profile/popeye` hoặc `/profile/spinach`, hãy thêm thủ công `/popeye` hoặc `/spinach` vào cuối URL hiện tại. Thành phần `<Outlet />` được thay thế bằng thành phần con khi đường dẫn của chúng được truy cập.

Nếu bạn muốn hiển thị thứ gì đó làm thành phần mặc định khi không có đường dẫn nào được thêm vào Hồ sơ, bạn có thể thêm tuyến chỉ mục cho các thành phần con!

Tạo thành phần DefaultProfile:

```jsx
const DefaultProfile = () => {
  return <p>Oh, nothing to see here!</p>;
};

export default DefaultProfile;
```

Bây giờ, hãy thêm thuộc tính chỉ mục với DefaultProfile khi còn nhỏ vào tuyến đường `/profile`.

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import App from "./App";
import Profile from "./Profile";
import DefaultProfile from "./DefaultProfile";
import Spinach from "./Spinach";
import Popeye from "./Popeye";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "profile",
    element: <Profile />,
    children: [
      { index: true, element: <DefaultProfile /> },
      { path: "spinach", element: <Spinach /> },
      { path: "popeye", element: <Popeye /> },
    ],
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>,
);
```

Nếu bạn truy cập đường dẫn `/profile` bây giờ, bạn sẽ có thể thấy một số nội dung mặc định trong đó `Outlet` được hiển thị khi đường dẫn chỉ mục được hiển thị!

Nhưng ví dụ này lại mang đến một vấn đề nan giải khác. Đôi khi, chúng ta muốn hiển thị nội dung theo URLs. Điều đó ở đây có nghĩa là chúng ta có thể hiển thị nội dung một cách linh hoạt, từ chính thành phần đó. Rất may, bạn có thể làm như vậy với các phân đoạn động! Thay đổi các tuyến đường như sau:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import App from "./App";
import Profile from "./Profile";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "profile/:name",
    element: <Profile />,
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>,
);
```

Dấu hai chấm (:) biến phần đường dẫn sau nó thành "đoạn động". Các phân đoạn động sẽ khớp với các giá trị động (thay đổi) ở vị trí đó của URL, giống như `name`. Chúng cũng có thể được gọi ngắn gọn là "URL params" hoặc "params". Chúng có thể được sử dụng với sự trợ giúp của móc `useParams`. Do đó chúng ta có thể viết lại thành phần Profile như sau:

```jsx
import { useParams } from "react-router";
import DefaultProfile from "./DefaultProfile";
import Spinach from "./Spinach";
import Popeye from "./Popeye";

const Profile = () => {
  const { name } = useParams();

  return (
    <div>
      <h1>Hello from profile page!</h1>
      <p>So, how are you?</p>
      <hr />
      <h2>The profile visited is here:</h2>
      {name === "popeye" ? (
        <Popeye />
      ) : name === "spinach" ? (
        <Spinach />
      ) : (
        <DefaultProfile />
      )}
    </div>
  );
};

export default Profile;
```

### Xử lý các url xấu

Nhưng than ôi, đường dẫn chỉ mục không còn hoạt động với điều này nữa, vì trong đường dẫn `/profile`, không có thông số nào thực sự được chuyển. Trên thực tế, đường dẫn `/profile` sẽ không có nhiều ý nghĩa nếu không có tên thực, nếu không thì nó sẽ hiển thị hồ sơ của ai, phải không? Vì vậy, ứng dụng hiển thị lỗi! Điều này không thể tốt được, vậy làm cách nào để hiển thị trang mặc định trong trường hợp người dùng truy cập vào đường dẫn sai hoặc không được sử dụng? Bạn có thể chuyển đối số `errorElement` tại đây! Tạo trang "Không tìm thấy" cơ bản:

```jsx
import { Link } from "react-router";

const ErrorPage = () => {
  return (
    <div>
      <h1>Oh no, this route doesn't exist!</h1>
      <Link to="/">
        You can go back to the home page by clicking here, though!
      </Link>
    </div>
  );
};

export default ErrorPage;
```

Thêm `errorElement` vào cấu hình và xác minh rằng nó hiển thị trang lỗi bằng cách đi tới đường dẫn `/profile` hoặc bất kỳ đường dẫn nào chưa được đề cập.

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import App from "./App";
import Profile from "./Profile";
import ErrorPage from "./ErrorPage";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
    errorElement: <ErrorPage />,
  },
  {
    path: "profile/:name",
    element: <Profile />,
  },
]);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>,
);
```

### Tái cấu trúc các tuyến đường

Hãy cấu trúc lại mảng tuyến đường của chúng ta thành tệp riêng. Bằng cách tái cấu trúc, chúng ta có thể nhập các tuyến vào `main.jsx` và tạo bộ định tuyến trình duyệt từ đó, như trong ví dụ trên. Điều thuận tiện ở đây là chúng ta cũng có thể nhập mảng tuyến đường vào bất kỳ tệp thử nghiệm nào, có thể cần sử dụng bộ định tuyến dành riêng cho thử nghiệm thay vì bộ định tuyến trình duyệt (sẽ nói thêm về điều đó sau).

Tạo một tệp `routes.jsx` mới và di chuyển mảng tuyến đường tới nó:

```jsx
import App from "./App";
import Profile from "./Profile";
import ErrorPage from "./ErrorPage";

const routes = [
  {
    path: "/",
    element: <App />,
    errorElement: <ErrorPage />,
  },
  {
    path: "profile/:name",
    element: <Profile />,
  },
];

export default routes;
```

Nhập các tuyến đường vào tệp `main.jsx` của bạn:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router";
import routes from "./routes";

const router = createBrowserRouter(routes);

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>
);
```

Đẹp hơn nhiều!

### Cửa hàng và trạng thái

Như chúng ta đã tìm hiểu trước đó, bạn có thể lồng các tuyến đường dưới dạng tuyến đường con của tuyến đường gốc, cho phép bạn sử dụng `<Outlet />` trong tuyến đường gốc để hiển thị phần tử thích hợp dựa trên phần còn lại của đường dẫn.

Nếu component gốc có dữ liệu, chẳng hạn state, và muốn truyền dữ liệu đó cho component được render bên trong outlet, ta sẽ dùng `context`. Ở đây chúng ta tập trung vào context của outlet; bài sau sẽ nói về context độc lập với outlet.

Outlet có sẵn một prop `context`. Ta có thể truyền vào đó bất kỳ giá trị nào, kể cả mảng hoặc object. Bên trong *bất kỳ* component nào được render trong outlet, kể cả component “cháu”, ta có thể gọi hook `useOutletContext()` để lấy giá trị đã truyền. Nếu truyền mảng hoặc object, ta còn có thể destructure giá trị đó.

Hãy xem React Bộ định tuyến [tài liệu về `useOutletContext`](https://reactrouter.com/api/hooks/useOutletContext) để tìm hiểu thêm về cách chuyển ngữ cảnh qua một lối thoát và truy cập ngữ cảnh đó trong các thành phần con.

### Các tuyến đường và điều hướng được bảo vệ

Thông thường, bạn sẽ cần phải quyết định xem có nên hiển thị một tuyến đường nhất định hay không. Một ví dụ là xác thực, trong đó bạn hiển thị các tuyến nhất định dựa trên việc người dùng có đăng nhập hay không. Nếu họ đã đăng nhập, bạn sẽ hiển thị một số thông tin về người dùng như tại đây [Trang bảng điều khiển Dự án Odin](https://www.theodinproject.com/dashboard). Nếu không, họ sẽ được chuyển hướng đến trang đăng nhập (đây có thể là bất kỳ trang nào). Mặc dù có nhiều cách để thực hiện việc này nhưng một trong những cách dễ nhất là tạo cấu hình có điều kiện cho bộ định tuyến.

Bạn sẽ thường gặp nhu cầu định tuyến lại người dùng đến một URL khác theo chương trình. Đây là nơi chúng ta có thể sử dụng [cái móc useNavigate](https://reactrouter.com/api/hooks/useNavigate) cho phép bạn điều hướng đến URLs hoặc thậm chí quay lại lịch sử của người dùng.

### React Kiểm tra bộ định tuyến

Khi kiểm tra các thành phần sử dụng Bộ định tuyến React, điều quan trọng cần nhớ là ứng dụng của bạn không bao giờ hiển thị trực tiếp các thành phần đó. Thay vào đó, nó hiển thị chúng thông qua bộ định tuyến (ví dụ: qua `<RouterProvider>`). Do đó, các thử nghiệm hiển thị các thành phần này cũng cần hiển thị chúng bên trong ngữ cảnh định tuyến - nếu không, các hook như `useNavigate` hoặc `useParams` hoặc thành phần `<Link>` sẽ gây ra lỗi.

Nếu bạn chỉ hiển thị một thành phần có chứa thứ gì đó như `<Link>` (và bạn không kiểm tra điều hướng hoặc dựa vào các tính năng khác của bộ định tuyến), hãy gói nó trong một gói nhẹ [Bộ định tuyến bộ nhớ](https://reactrouter.com/api/declarative-routers/MemoryRouter) có thể là đủ. Nhưng nếu thành phần phụ thuộc vào **hành vi của bộ định tuyến**, như sử dụng ngữ cảnh đầu ra, thông số phù hợp, thành phần lỗi hoặc chuyển hướng, sẽ hợp lý hơn khi hiển thị `<RouterProvider>` giống như ứng dụng của bạn. Vì các bài kiểm tra của bạn không chạy trên trình duyệt nên bạn có thể sử dụng [createMemoryRouter](https://reactrouter.com/api/data-routers/createMemoryRouter) để tạo bộ định tuyến trong các thử nghiệm của bạn với cùng cấu hình tuyến đường từ `routes.jsx`.

### Phần kết luận

Bây giờ bạn đã có đủ kiến ​​thức cơ bản để bắt đầu với định tuyến React. Có rất nhiều tính năng khác của bộ định tuyến phản ứng cực kỳ hữu ích nhưng nằm ngoài phạm vi của bài học này. Chúng tôi tự tin rằng các khái niệm nêu trên sẽ đủ để giúp bạn vượt qua khóa học React. Nếu bạn muốn tìm hiểu thêm, chúng tôi khuyên bạn nên xem xét lịch sử hoặc đối tượng trùng khớp. Chắc chắn hãy đi và kiểm tra các khái niệm nâng cao hơn khi bạn đã quen với những điều cơ bản.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Bài viết này trên [SPA và định tuyến phía khách hàng của Ben Holmes](https://bholmes.dev/blog/spas-clientside-routing/) đi qua rất nhiều khái niệm định tuyến một cách chính xác.
1. Thêm một vài tuyến đường mới vào ứng dụng chúng tôi đã tạo ở trên. Đây là một bài học dày đặc, vì vậy hãy dành chút thời gian để làm quen với những công cụ mới mà bạn đã học. Hãy cân nhắc việc xóa nó hoàn toàn và viết lại nó bằng những gì bạn biết.
1. Duyệt qua [React Tài liệu về bộ định tuyến](https://reactrouter.com/home). Xin nhắc lại, bạn không cần phải đọc hết hay hiểu hết. Chỉ cần duyệt qua các khái niệm chúng ta đã thảo luận ở đây và đọc lại chúng. Xem xét các tính năng khác mà Bộ định tuyến React cung cấp. Đây là một nguồn tài liệu tuyệt vời để tham khảo lại.

</div>
