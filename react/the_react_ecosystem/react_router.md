### Giới thiệu

Cho đến thời điểm này trong curriculum, chúng ta đã xây dựng các application một trang. Tuy nhiên, mọi application quy mô lớn hơn sẽ có nhiều page. May mắn là trình duyệt cho phép JavaScript phía máy khách quản lý cách người dùng điều hướng bằng [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API). Chúng ta có thể tận dụng sức mạnh này để quản lý routing trong React với sự trợ giúp của package như React Router.

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài này.

- Hiểu client-side routing là gì.
- Học cách dùng React Router cho client-side routing.
- Tìm hiểu cách tạo path lồng nhau và path động trong React Router.
- Tìm hiểu cách thêm route “catch-all” trong React Router.
- Học cách truyền dữ liệu từ component cha tới các component con được render qua outlet.
- Hiểu cách triển khai protected route trong React Router.
- Làm quen với việc kiểm thử component sử dụng React Router.

### Client-side routing

Client-side routing là kiểu routing trong đó JavaScript đảm nhiệm việc xử lý các route của application. Client-side routing giúp xây dựng single-page application (SPA) mà không refresh khi người dùng điều hướng. Ví dụ, khi người dùng click một phần tử navbar, URL thay đổi và phần hiển thị của page được cập nhật tương ứng ngay phía client.

Hãy tưởng tượng bạn đang nấu gà. Nếu muốn nấu ngon, bạn sẽ phải:

1. Cho gà vào lò và đặt thời gian cùng nhiệt độ phù hợp
1. Chờ đến khi món ăn tỏa ra mùi thơm hấp dẫn
1. Bắt đầu thưởng thức!

Điều này cũng giống với mọi website: bạn cài đặt lò theo thứ mình muốn (truy cập một URL bất kỳ, chẳng hạn [https://theodinproject.com/](https://theodinproject.com/)), chờ lò nấu xong (màn hình loading), rồi thưởng thức món ngon (page đã sẵn sàng để dùng). Nhưng nếu bạn quên cho gia vị trước khi nấu thì sao? Bạn phải lặp lại quy trình:

1. Đứng dậy khỏi ghế
1. Thêm gia vị vào gà
1. Quay lại lò, cho gà vào và cài đặt để hâm nóng
1. Chờ đến khi gà nóng vừa đủ
1. Bây giờ bạn có thể ăn!

Đây là điểm cần nhấn mạnh: **bạn phải đứng dậy khỏi ghế**. Trong một multi-page application (MPA) thông thường, trình duyệt reload mỗi khi bạn click link để điều hướng. Với client-side routing, **bạn không bao giờ rời page hiện tại** — bạn mang lò vi sóng tới bàn để không phải đứng dậy nếu gặp vấn đề “thiếu gia vị”. Các request của link được JavaScript bạn viết chặn lại thay vì gửi thẳng tới server.

### Một giải pháp Reactive

Client-side routing cho phép tương tác đẹp và giống app hơn (vì kiểm soát route nên bạn có thể tạo animation CSS bắt mắt khi đổi route), nhưng dễ bỏ sót nhiều điểm cần lưu ý. Khi trình duyệt reload, nó thông báo cho screen reader về nội dung mới; còn với client-side routing, bạn cần tự thông báo cập nhật route cho screen reader. Tuy nhiên, một thư viện mạnh có thể giúp giải quyết các vấn đề này!

React Router là thư viện routing tiêu chuẩn cho application React. Với React Router, chúng ta có thể chỉ định các component React được render dựa trên route cùng nhiều khả năng khác. Hãy bắt đầu!

### Thêm router

Hãy tạo một app nhỏ để hiểu router này được triển khai thế nào. Tạo một project React mới và bắt đầu bằng cách thêm vài page mô phỏng làm ví dụ. Tạo file `Profile.jsx` mới với component sau:

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

Đồng thời thay nội dung file `App.jsx` bằng nội dung cơ bản sau:

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

Bây giờ là lúc thêm router! Có vài cách định nghĩa route của app; chúng ta sẽ dùng cách tiếp cận dựa trên object.

Hãy cài package React Router:

`npm install react-router`

Thêm nội dung sau vào `main.jsx`; lát nữa chúng ta sẽ giải thích điều đang diễn ra.

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

Sau khi xong, hãy chạy `npm run dev` và kiểm tra cả hai route: home route `/` và profile route `/profile`. Nó hoạt động! Nhưng chuyện gì đang xảy ra?

1. Chúng ta import `createBrowserRouter` và `RouterProvider` từ React Router.
1. `createBrowserRouter` dùng để tạo cấu hình router bằng cách truyền đối số dưới dạng một array route.
1. Array cấu hình chứa các object với hai key bắt buộc là path và element tương ứng cần render.
1. Sau đó cấu hình được tạo này được render bằng cách truyền nó vào component `RouterProvider`.

### Phần tử link

Tuy nhiên, bạn có thể nhận thấy khi click link trong navbar, trình duyệt reload để tới URL tiếp theo thay vì dùng React Router. Đây không phải điều đã hứa! Để giải quyết, React Router cung cấp custom [Link component](https://reactrouter.com/api/components/Link) để dùng thay cho tag `a` thông thường. Chúng ta có thể thay tag `a` trong navbar bằng phần tử `Link`.

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

Bây giờ trình duyệt không còn reload mỗi khi chúng ta click link trên navbar!

### Route lồng nhau, outlet và segment động

Nếu muốn render một phần của page khác nhau tùy URL thì sao? Đây là lúc nested route phát huy tác dụng! Chúng ta có thể lồng route làm child của nhau để đảm bảo child được render cùng parent. Hãy tạo hai component `Popeye.jsx` và `Spinach.jsx`.

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

Bây giờ, chúng ta có thể viết lại route như sau:

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

Điều này cho phép render component con cùng parent thông qua [Outlet component](https://reactrouter.com/api/components/Outlet)! Chúng ta có thể viết lại component Profile để thêm `Outlet`; nó sẽ được thay thế bằng các profile tương ứng khi truy cập route đó!

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

Hãy mở page `/profile`. Để truy cập page `/profile/popeye` hoặc `/profile/spinach`, hãy thêm thủ công `/popeye` hoặc `/spinach` vào cuối URL hiện tại. Component `<Outlet />` được thay bằng component con khi truy cập path tương ứng.

Nếu muốn render một component mặc định khi không thêm path vào Profile, bạn có thể thêm index route vào các child!

Tạo component DefaultProfile:

```jsx
const DefaultProfile = () => {
  return <p>Oh, nothing to see here!</p>;
};

export default DefaultProfile;
```

Bây giờ, thêm property index với DefaultProfile làm child cho route `/profile`.

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

Nếu truy cập path `/profile` lúc này, bạn sẽ thấy nội dung mặc định tại vị trí `Outlet` khi index path được render!

Nhưng ví dụ này đặt ra một vấn đề khác. Đôi khi chúng ta muốn render nội dung theo URL. Trong trường hợp này, nghĩa là component phải có thể render nội dung động từ chính nó. May mắn là dynamic segment cho phép làm vậy! Hãy đổi route thành:

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

Dấu hai chấm (:) biến phần path sau nó thành một “dynamic segment”. Dynamic segment sẽ khớp với các giá trị động (có thể thay đổi) ở vị trí đó trong URL, chẳng hạn `name`. Chúng cũng có thể được gọi là “URL params” hoặc gọi tắt là “params”. Có thể dùng chúng với hook `useParams`. Vì vậy, chúng ta có thể viết lại component Profile như sau:

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

### Xử lý URL không hợp lệ

Nhưng tiếc là index path không còn hoạt động, vì ở path `/profile` thực sự không truyền params nào. Thật ra `/profile` không có nhiều ý nghĩa nếu thiếu một name cụ thể; nếu không thì phải hiển thị profile của ai, đúng không? Vì vậy application hiển thị lỗi! Điều này không ổn, vậy làm sao hiển thị page mặc định khi người dùng truy cập path sai hoặc chưa được dùng? Bạn có thể truyền đối số `errorElement` ở đây! Hãy tạo một page “Not Found” cơ bản:

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

Thêm `errorElement` vào cấu hình và xác minh nó render error page bằng cách truy cập path `/profile` hoặc bất kỳ path chưa được khai báo nào.

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

### Refactor route

Hãy refactor array route thành file riêng. Sau khi refactor, chúng ta có thể import route vào `main.jsx` và tạo browser router từ đó như ví dụ trên. Điều tiện lợi là cũng có thể import array route vào các file test cần dùng test-specific router thay vì browser router (sẽ nói thêm sau).

Tạo file `routes.jsx` mới và chuyển array route vào đó:

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

Import route vào file `main.jsx`:

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

Gọn gàng hơn nhiều!

### Outlet và state

Như đã học, bạn có thể lồng route làm child của route cha, cho phép dùng `<Outlet />` trong parent để render element phù hợp dựa trên phần còn lại của path.

Nếu parent element có dữ liệu, chẳng hạn state, và muốn truyền dữ liệu đó tới mọi component được render bởi outlet, chúng ta cần dùng thứ gọi là `context`. Hiện tại, chúng ta tập trung vào context với outlet; trong bài sau, chúng ta sẽ tìm hiểu thêm cách dùng context không qua outlet.

Outlet có sẵn prop `context`. Chúng ta có thể truyền bất kỳ giá trị nào vào prop này, kể cả array hoặc object. Bên trong *bất kỳ* component nào được render trong outlet đó (kể cả component “cháu”), chúng ta có thể gọi hook `useOutletContext()`, hook này trả về giá trị đã truyền vào prop context. Nếu truyền array hoặc object, thậm chí chúng ta còn có thể destructure nó!

Hãy xem [tài liệu về `useOutletContext`](https://reactrouter.com/api/hooks/useOutletContext) của React Router để tìm hiểu thêm cách truyền context qua outlet và truy cập context đó trong component con.

### Protected route và điều hướng

Thường bạn sẽ cần quyết định một route có được render hay không. Authentication là một ví dụ: render các route nhất định dựa trên việc người dùng đã đăng nhập hay chưa. Nếu đã đăng nhập, bạn hiển thị thông tin người dùng như tại [dashboard page của The Odin Project](https://www.theodinproject.com/dashboard). Nếu không, họ được redirect tới sign-in page (có thể là bất kỳ page nào). Có nhiều cách làm, nhưng một trong những cách dễ nhất là tạo config cho router một cách có điều kiện.

Bạn sẽ thường gặp nhu cầu chuyển người dùng tới URL khác bằng code. Đây là lúc có thể dùng [hook useNavigate](https://reactrouter.com/api/hooks/useNavigate), cho phép điều hướng tới URL hoặc thậm chí quay lại trong history của người dùng.

### Kiểm thử React Router

Khi kiểm thử component dùng React Router, điều quan trọng là nhớ rằng app không bao giờ render trực tiếp các component đó. Thay vào đó, app render chúng thông qua router (ví dụ qua `<RouterProvider>`). Vì vậy, test render các component này cũng cần render chúng bên trong routing context — nếu không, các hook như `useNavigate` hoặc `useParams`, hay component `<Link>`, sẽ gây lỗi.

Nếu chỉ render một component tình cờ chứa `<Link>` (và không kiểm thử navigation hay phụ thuộc tính năng router khác), bọc nó trong [MemoryRouter](https://reactrouter.com/api/declarative-routers/MemoryRouter) nhẹ là đủ. Nhưng nếu component phụ thuộc vào **hành vi router**, chẳng hạn dùng outlet context, matching params, error element hoặc redirect, hợp lý hơn là render `<RouterProvider>` như app. Vì test không chạy trong trình duyệt, bạn có thể dùng [createMemoryRouter](https://reactrouter.com/api/data-routers/createMemoryRouter) để tạo router trong test với cùng cấu hình route từ `routes.jsx`.

### Kết luận

Bây giờ bạn đã có đủ kiến thức cơ bản để bắt đầu routing trong React. react-router còn nhiều tính năng cực kỳ hữu ích nhưng nằm ngoài phạm vi bài này. Chúng tôi tin rằng các khái niệm trên đủ giúp bạn hoàn thành khóa học React. Nếu muốn học thêm, chúng tôi khuyên bạn tìm hiểu history hoặc match object. Khi đã quen với nền tảng, hãy tiếp tục khám phá các khái niệm nâng cao.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bài viết về [SPA và client-side routing của Ben Holmes](https://bholmes.dev/blog/spas-clientside-routing/) trình bày ngắn gọn nhiều khái niệm routing.
1. Thêm một vài route mới vào application đã tạo ở trên. Đây là bài học nhiều nội dung, vì vậy hãy dành thời gian thử nghiệm các công cụ mới. Bạn có thể cân nhắc xóa hoàn toàn application và viết lại bằng những gì đã biết.
1. Đọc lướt qua [tài liệu React Router](https://reactrouter.com/home). Một lần nữa, bạn không cần đọc hết hay hiểu hết. Chỉ cần xem qua và đọc lại các khái niệm đã thảo luận ở đây. Tìm hiểu thêm những tính năng khác React Router cung cấp. Đây là tài liệu rất hữu ích để tham khảo sau này.

</div>
