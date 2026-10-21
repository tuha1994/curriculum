### Giới thiệu

Cho đến thời điểm này, chúng ta đã dùng React để xây dựng các ứng dụng phía máy khách với giao diện người dùng tương tác. Nhưng nếu muốn lấy dữ liệu từ Internet thì sao? Để tạo các ứng dụng web hoàn chỉnh, chúng ta cần một cách lấy dữ liệu từ các nguồn bên ngoài và hiển thị dữ liệu đó một cách động.

Trong bài học này, chúng ta sẽ tìm hiểu mọi khía cạnh của việc lấy dữ liệu trong React, bắt đầu với những điều cơ bản về gọi API, quản lý state của component và xử lý các thao tác bất đồng bộ bằng function `fetch` của JavaScript. Bạn đã từng lấy dữ liệu trong các project trước, nên một phần nội dung trong bài sẽ quen thuộc. Ôn tập cũng không bao giờ thừa!

### Tổng quan bài học

Phần này khái quát những chủ đề bạn sẽ học trong bài này.

- Thực hiện các request fetch trong component React.
- Bắt và xử lý lỗi.
- Đưa request lên cấp cao hơn trong hệ thống phân cấp component.

### Request fetch cơ bản

Trước khi đi sâu vào việc lấy dữ liệu trong React, hãy cùng ôn lại ngắn gọn cách dùng fetch API để lấy dữ liệu từ server.

```javascript
const image = document.querySelector("img");
fetch("https://picsum.photos/v2/list")
  .then((response) => response.json())
  .then((response) => {
    image.src = response[0].download_url;
  })
  .catch((error) => console.error(error));
```

Chúng ta gửi request tới Picsum API để lấy một hình ảnh, sau đó đặt URL của hình ảnh đó vào thuộc tính src của một phần tử `<img>`.

### Dùng fetch trong component React

Bây giờ, hãy xem cách tích hợp `fetch` vào một component React tương tự ví dụ trước. Một trường hợp thường gặp là lấy dữ liệu từ API khi component mount để có thể hiển thị dữ liệu trên màn hình.

Khi component cần thực hiện request trong lúc render, thường tốt nhất là bọc `fetch` đó bên trong một effect.

```jsx
import { useEffect, useState } from "react";

const Image = () => {
  const [imageURL, setImageURL] = useState(null);

  useEffect(() => {
    fetch("https://picsum.photos/v2/list")
    .then((response) => response.json())
    .then((response) => setImageURL(response[0].download_url))
    .catch((error) => console.error(error));
  }, []);

  return (
    imageURL && (
      <>
        <h1>An image</h1>
        <img src={imageURL} alt={"placeholder text"} />
      </>
    )
  );
};

export default Image;
```

`useState` cho phép chúng ta thêm state `imageURL`, còn `useEffect` cho phép thực hiện side effect. Trong trường hợp này, side effect là lấy dữ liệu từ một API bên ngoài. Vì chỉ cần lấy dữ liệu một lần khi component mount, chúng ta truyền vào một dependency array rỗng.

### Xử lý lỗi

Làm việc qua mạng vốn không đáng tin cậy. API mà bạn request có thể đang ngừng hoạt động, kết nối mạng có thể gặp vấn đề hoặc response nhận được có thể chứa lỗi. Có rất nhiều điều có thể xảy ra; nếu không chủ động dự liệu lỗi, website của bạn có thể bị hỏng hoặc có vẻ không phản hồi với người dùng.

Để mô phỏng lỗi mạng, hãy cuộn lên đoạn code trước và đổi URL của `fetch` thành một URL bất kỳ. Sau khi refresh cửa sổ trình duyệt, trang sẽ vẫn là một màn hình trắng trống mà không cho người dùng biết trang đã tải xong hay đã xảy ra lỗi.

Để khắc phục, chúng ta cần kiểm tra *một giá trị nào đó* trước khi component Image trả về JSX. Hãy gọi giá trị đó là: `error`.

```jsx
if (error) return <p>A network error was encountered</p>

return (
  imageURL && (
    <>
      <h1>An image</h1>
      <img src={imageURL} alt={"placeholder text"} />
    </>
  )
);
```

Để thiết lập `error`, chúng ta sẽ thêm nó vào state của component.

```jsx
const [imageURL, setImageURL] = useState(null);
const [error, setError] = useState(null);
```

Cuối cùng, để gán giá trị cho `error` khi request thất bại, chúng ta sẽ thêm điều kiện kiểm tra status của response và thiết lập nó tại vị trí trước đây có dòng console.error.

```jsx
useEffect(() => {
  fetch("https://picsum.photos/v2/list")
    .then((response) => {
      if (response.status >= 400) {
        throw new Error("server error");
      }
      return response.json();
    })
    .then((response) => setImageURL(response[0].download_url))
    .catch((error) => setError(error));
}, []);
```

<div class="lesson-note" markdown="1" >

#### Xử lý lỗi response

Bạn có để ý rằng chúng ta cũng xử lý lỗi trong block `then` không? Đó là vì bản thân request `fetch` có thể không thất bại mà hoàn tất thành công và trả về một response. Tuy nhiên, response nhận được có thể không đúng với điều app mong đợi. Để xử lý trường hợp này, chúng ta kiểm tra status code của response.

</div>

Bây giờ, khi truyền vào URL không hợp lệ hoặc API trả về response không mong đợi, trang sẽ thông báo thông tin đó cho người dùng.

#### State loading

Tương tự như khi thêm giá trị error vào state để kiểm tra lỗi, chúng ta cũng có thể thêm giá trị loading để kiểm tra request đã hoàn tất hay chưa.

```jsx
const Image = () => {
  const [imageURL, setImageURL] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://picsum.photos/v2/list")
      .then((response) => {
        if (response.status >= 400) {
          throw new Error("server error");
        }
        return response.json();
      })
      .then((response) => setImageURL(response[0].download_url))
      .catch((error) => setError(error))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>A network error was encountered</p>;

  return (
    <>
      <h1>An image</h1>
      <img src={imageURL} alt={"placeholder text"} />
    </>
  );
};
```

### Dùng custom hook

Chúng ta có thể tách toàn bộ logic lấy dữ liệu thành một custom hook. Điều này giúp logic có thể tái sử dụng và dễ kiểm thử hơn.

Hãy nhớ trong bài [Introduction to state](https://www.theodinproject.com/lessons/node-path-react-new-introduction-to-state), chúng ta đã nói rằng React hook chỉ là một function cho phép dùng các tính năng của React (như state, effect, v.v.), và chúng tuân theo quy tắc đặt tên: bắt đầu bằng `use` rồi đến một chữ cái viết hoa (ví dụ `useState` hoặc `useEffect`). Nếu thử đặt một hook như `useEffect` bên trong helper function thông thường như `getImageURL`, React sẽ không chấp nhận vì hook chỉ được gọi ở cấp cao nhất của component hoặc hook khác. Vì vậy, chúng ta chỉ cần biến helper function thành custom hook bằng cách tuân theo quy tắc đặt tên hook: `useImageURL`.

Với ví dụ của chúng ta, cách làm như sau:

```jsx
import { useState, useEffect } from "react";

const useImageURL = () => {
  const [imageURL, setImageURL] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://picsum.photos/v2/list")
      .then((response) => {
        if (response.status >= 400) {
          throw new Error("server error");
        }
        return response.json();
      })
      .then((response) => setImageURL(response[0].download_url))
      .catch((error) => setError(error))
      .finally(() => setLoading(false));
  }, []);

  return { imageURL, error, loading };
};

const Image = () => {
  const { imageURL, error, loading } = useImageURL();

  if (loading) return <p>Loading...</p>;
  if (error) return <p>A network error was encountered</p>;

  return (
    <>
      <h1>An image</h1>
      <img src={imageURL} alt={"placeholder text"} />
    </>
  );
};
```

Nếu cần lấy hình ảnh trong các component khác, thay vì viết lại toàn bộ logic lấy dữ liệu, chúng ta có thể gọi `useImageURL`.

### Quản lý nhiều request fetch

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Dùng độ trễ nhân tạo

Trong phần này, chúng ta dùng một ví dụ code có thêm `delay` nhân tạo bằng function `setTimeout`. Có lẽ bạn đã đoán ra, mục đích là giúp bạn theo dõi các kiến thức cơ bản về lấy dữ liệu trong bài. Chúng tôi khuyên bạn xóa các độ trễ này sau khi hoàn thành phần này, rồi thử nghiệm với các ví dụ code để củng cố khái niệm.

</div>

Trong một web app quy mô đầy đủ, bạn thường sẽ thực hiện nhiều hơn một request và cần cẩn thận với cách tổ chức chúng. Một vấn đề phổ biến mà các React developer mới gặp phải khi app bắt đầu thực hiện nhiều request được gọi là *thác request*. Hãy xem ví dụ trong [repo react-examples](https://github.com/TheOdinProject/react-examples) của chúng ta (hãy fork và clone repo nếu bạn chưa làm), lần này trong thư mục `fetching-data/`. `cd` vào đó, chạy `npm install`, sau đó khởi động dev server bằng `npm run dev`.

Chúng ta có hai component thực hiện request fetch: `Profile` và component con `Bio`. Trong cả hai file, bạn sẽ thấy một component và một phiên bản khác của nó được comment bên dưới; chúng ta sẽ quay lại với chúng ngay sau đây. Các request trong `Profile` và `Bio` đều được thực hiện bên trong component tương ứng. Nhìn bề ngoài, đây có vẻ là cách tách biệt mối quan tâm được tổ chức tốt, nhưng trong trường hợp này nó phải trả giá bằng hiệu năng.

Bạn có nhận thấy mỗi khi tải trang, `Bio` mất thêm một giây mới hiển thị không? Cả hai request fetch lẽ ra đều mất 1000ms để hoàn tất, vậy chuyện gì đang xảy ra? Trong React, component không được render cho đến khi thực sự được gọi. Nếu JSX có logic điều kiện, các nhánh false sẽ không bao giờ render cho đến khi trở thành true. `Bio` phải đợi request bên trong `Profile` hoàn tất trước khi bắt đầu render, nghĩa là request bên trong `Bio` chưa được gửi.

Nếu xóa điều kiện short-circuit đang chờ `imageURL`, `Bio` sẽ gửi request ngay lập tức, nhưng như vậy chúng ta phải bỏ màn hình loading. Thay vì thỏa hiệp về thiết kế, chúng ta có thể đưa request lên cao hơn trong cây component và truyền response của nó vào `Bio` dưới dạng prop.

Để xem điều này hoạt động, trong cả component `Profile` và `Bio`, hãy comment chúng lại rồi bỏ comment các phiên bản thay thế bên dưới.

Bây giờ cả hai request đều được thực hiện ngay khi `Profile` render. Request cho `imageURL` hoàn tất sớm hơn request `bioText` 2 giây, và div chứa `<Bio />` được render. Khi `bioText` hoàn tất, state sẽ được cập nhật, từ đó kích hoạt việc render lại `<Bio />` và thêm phần mô tả văn bản đó vào trang.

### Thư viện lấy dữ liệu

Chúng ta mới chỉ bắt đầu tìm hiểu bề mặt của việc lấy dữ liệu ở frontend. Giữ cho dữ liệu frontend luôn cập nhật với server là một nhiệm vụ khó. Quản lý state “async” ngày càng phức tạp khi thêm từng tính năng.

Bạn đã nếm trải sự phức tạp của việc lấy dữ liệu trong bài này. Mỗi request cần có *tối thiểu* ba state để mang lại trải nghiệm người dùng tốt: `data`, `loading` và `error`. Dù một số thư viện có thể hỗ trợ lấy dữ liệu cùng nhiều việc khác, chúng tôi đặc biệt khuyên bạn dùng cách lấy dữ liệu bằng React thuần cho mọi project trong khóa học. Những bài học bạn nhận được khi làm như vậy sẽ vô cùng giá trị.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Modern API data fetching methods](https://blog.logrocket.com/modern-api-data-fetching-methods-react/) cho đến phần Axios để có cái nhìn khái quát về những gì đã được thảo luận trong bài này.
1. Đọc [How to fetch data in React with performance in mind](https://www.developerway.com/posts/how-to-fetch-data-in-react) để tìm hiểu thêm về cách xử lý request fetch hiệu quả trong component React.

</div>
