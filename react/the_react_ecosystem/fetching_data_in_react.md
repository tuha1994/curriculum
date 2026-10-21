### Giới thiệu

Cho đến thời điểm này, chúng ta vẫn dùng React để xây dựng các ứng dụng client-side có giao diện tương tác. Nhưng nếu muốn lấy dữ liệu từ internet thì sao? Để tạo ứng dụng web hoàn chỉnh, ta cần cách lấy dữ liệu từ nguồn bên ngoài rồi render dữ liệu đó một cách linh hoạt.

Trong bài học này, chúng ta sẽ tìm hiểu việc lấy dữ liệu trong React: từ cách gọi API, quản lý state của component đến xử lý thao tác bất đồng bộ bằng hàm `fetch` của JavaScript. Bạn đã từng lấy dữ liệu trong các dự án trước, nên một phần nội dung có thể quen thuộc. Ôn lại cũng không thừa!

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thực hiện các yêu cầu lấy dữ liệu trong các component React.
- Bắt lỗi và xử lý.
- Nâng cao yêu cầu lên hệ thống phân cấp component.

### Yêu cầu lấy dữ liệu cơ bản

Trước khi đi sâu vào việc lấy dữ liệu trong React, hãy cùng ôn nhanh cách dùng Fetch API để lấy dữ liệu từ server.

```javascript
const image = document.querySelector("img");
fetch("https://picsum.photos/v2/list")
  .then((response) => response.json())
  .then((response) => {
    image.src = response[0].download_url;
  })
  .catch((error) => console.error(error));
```

Chúng tôi đang đưa ra yêu cầu tới Picsum API để truy xuất một hình ảnh và sau đó đặt URL đó thành src của phần tử `<img>`.

### Sử dụng lấy dữ liệu trong các component React

Bây giờ, hãy xem cách kết hợp `fetch` vào một component React, tương tự ví dụ trước. Một trường hợp phổ biến là lấy dữ liệu từ API khi component được mount để có thể render dữ liệu lên màn hình.

Khi component cần gửi request trong quá trình render, thông thường ta nên đặt lệnh `fetch` bên trong một effect.

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

`useState` cho phép thêm state `imageURL`, còn `useEffect` cho phép thực hiện side effect. Trong trường hợp này, side effect là lấy dữ liệu từ API bên ngoài. Vì chỉ cần lấy dữ liệu một lần khi component mount, ta truyền vào một dependency array rỗng.

### Xử lý lỗi

Làm việc qua mạng vốn không đáng tin cậy. API bạn đang gửi yêu cầu có thể không hoạt động, có thể có sự cố kết nối mạng hoặc phản hồi bạn nhận được có thể có lỗi. Rất nhiều điều có thể xảy ra sai sót và nếu bạn không chuẩn bị trước cho các lỗi, trang web của bạn có thể bị hỏng hoặc có vẻ không phản hồi với người dùng.

Để mô phỏng lỗi mạng, hãy cuộn lên đoạn mã trước đó và thay đổi `fetch` URL thành một cái gì đó ngẫu nhiên. Sau khi làm mới cửa sổ trình duyệt của bạn, trang sẽ vẫn là một màn hình trắng trống mà không cung cấp cho người dùng bất kỳ dấu hiệu nào cho thấy trang đã tải xong hoặc đã xảy ra lỗi.

Để khắc phục, ta cần kiểm tra *một điều gì đó* trước khi component `Image` trả về JSX. Ta sẽ gọi giá trị đó là `error`.

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

Để đặt `error` này, chúng ta sẽ thêm nó vào trạng thái của component.

```jsx
const [imageURL, setImageURL] = useState(null);
const [error, setError] = useState(null);
```

Và cuối cùng, để gán `error` một giá trị khi yêu cầu không thành công, chúng ta sẽ thêm một điều kiện để kiểm tra trạng thái phản hồi và đặt nó ở vị trí dòng console.error của chúng ta.

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

#### Xử lý lỗi phản hồi

Hãy lưu ý cách chúng tôi cũng xử lý các lỗi trong khối `then`? Điều này là do bản thân yêu cầu `fetch` có thể không thất bại mà hoàn thành thành công và mang lại phản hồi. Tuy nhiên, phản hồi nhận được có thể không như những gì ứng dụng của chúng tôi mong đợi. Để xử lý trường hợp này, chúng tôi kiểm tra mã trạng thái phản hồi.

</div>

Bây giờ, khi URL xấu được thông qua hoặc API trả về phản hồi không mong muốn, trang sẽ chuyển tiếp thông tin đó đến người dùng.

#### Trạng thái tải

Tương tự như cách chúng ta thêm giá trị lỗi ở trạng thái để kiểm tra lỗi, chúng ta cũng có thể thêm giá trị đang tải để kiểm tra xem yêu cầu có được giải quyết hay không.

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

### Sử dụng hook tùy chỉnh

Chúng ta có thể tách hoàn toàn logic lấy dữ liệu thành một hook tùy chỉnh. Điều này sẽ cho phép chúng ta làm cho logic có thể tái sử dụng và kiểm tra dễ dàng.

Trong bài [Giới thiệu về state](https://www.theodinproject.com/lessons/node-path-react-new-introduction-to-state), ta đã biết hook React chỉ là một hàm cho phép sử dụng các tính năng của React (như state và effect). Hook phải được đặt tên bắt đầu bằng `use` rồi đến một chữ cái viết hoa, chẳng hạn `useState` hoặc `useEffect`. Nếu đặt `useEffect` trong hàm trợ giúp thông thường như `getImageURL`, React sẽ báo lỗi vì hook chỉ được gọi ở cấp cao nhất của component hoặc trong một hook khác. Vì vậy, ta có thể đổi hàm trợ giúp thành custom hook bằng cách tuân theo quy tắc đặt tên — `useImageURL`.

Đây là cách chúng tôi sẽ làm điều đó cho ví dụ của chúng tôi:

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

Nếu chúng ta cần lấy dữ liệu hình ảnh trong các component khác nhau, thay vì viết lại tất cả logic lấy dữ liệu đó, chúng ta có thể gọi `useImageURL`.

### Quản lý nhiều yêu cầu lấy dữ liệu

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Sử dụng độ trễ nhân tạo

Trong phần này, ví dụ mã có thêm một `delay` nhân tạo bằng hàm `setTimeout`. Như bạn có thể đoán, mục đích là giúp bạn theo dõi các khái niệm cơ bản về lấy dữ liệu. Sau khi học xong phần này, bạn nên xóa các độ trễ rồi tự thử nghiệm lại để củng cố kiến thức.

</div>

Trong một ứng dụng web thực tế, bạn thường phải gửi nhiều request và cần tổ chức chúng cẩn thận. Một vấn đề phổ biến với người mới học React là *request waterfall* — các request nối tiếp nhau. Hãy xem ví dụ trong [repo react-examples](https://github.com/TheOdinProject/react-examples) (hãy fork và clone repo nếu bạn chưa làm), lần này ở thư mục `fetching-data/`. `cd` vào đó, chạy `npm install`, rồi khởi động dev server bằng `npm run dev`.

Chúng tôi có hai component thực hiện yêu cầu lấy dữ liệu: `Profile` và component con của nó `Bio`. Trong cả hai tệp đó, bạn sẽ thấy một component và một phiên bản khác của nó được nhận xét bên dưới - chúng tôi sẽ sớm quay lại những tệp này. Các yêu cầu trong `Profile` và `Bio` đều được kích hoạt bên trong các component tương ứng của chúng. Nhìn bề ngoài, điều này có vẻ như là sự phân tách các mối quan tâm được tổ chức tốt, nhưng trong trường hợp này, nó phải trả giá bằng hiệu suất.

Lưu ý rằng bất cứ khi nào bạn tải trang, `Bio` sẽ mất thêm một giây để render? Cả hai yêu cầu lấy dữ liệu của họ đều phải mất 1000 mili giây để giải quyết, vậy chuyện gì đang xảy ra vậy? Trong React, component không được render cho đến khi nó thực sự được gọi. Nếu JSX có logic điều kiện, các nhánh sai sẽ không bao giờ render cho đến khi chúng trở thành đúng. `Bio` phải đợi yêu cầu bên trong `Profile` được giải quyết trước khi bắt đầu render, điều đó có nghĩa là yêu cầu bên trong `Bio` không được gửi.

Nếu chúng tôi loại bỏ điều kiện đoản mạch chờ `imageURL`, thì `Bio` sẽ gửi yêu cầu ngay lập tức, nhưng điều đó có nghĩa là chúng tôi sẽ phải bỏ màn hình tải. Thay vì ảnh hưởng đến thiết kế, chúng ta có thể nâng yêu cầu lên cây component và chuyển phản hồi của nó dưới dạng chỗ dựa cho `Bio`.

Để thấy điều này hoạt động, trong cả hai component `Profile` và `Bio`, hãy nhận xét chúng rồi bỏ ghi chú các phiên bản thay thế bên dưới chúng.

Bây giờ chúng tôi có cả hai yêu cầu được kích hoạt ngay khi `Profile` render. Yêu cầu `imageURL` được xử lý 2 giây trước yêu cầu `bioText` và div chứa `<Bio />` của chúng tôi render. Khi `bioText` được giải quyết, một bản cập nhật sẽ được thực hiện ở trạng thái kích hoạt render lại trong `<Bio />`, thêm mô tả văn bản đó vào trang.

### Thư viện lấy dữ liệu

Chúng ta mới chỉ bắt đầu chạm đến bề mặt của việc lấy dữ liệu ở frontend. Giữ cho dữ liệu frontend luôn đồng bộ với server là một bài toán khó, và việc quản lý state "bất đồng bộ" ngày càng phức tạp khi thêm tính năng.

Qua bài học này, bạn đã thấy việc lấy dữ liệu phức tạp thế nào. Để có trải nghiệm người dùng tốt, mỗi request cần *tối thiểu* ba state: `data`, `loading` và `error`. Dù một số thư viện có thể hỗ trợ việc lấy dữ liệu và nhiều việc khác, bạn vẫn nên dùng cách lấy dữ liệu bằng React thuần cho mọi dự án trong khóa học này. Những gì học được sẽ rất hữu ích.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [Phương pháp lấy dữ liệu API hiện đại](https://blog.logrocket.com/modern-api-data-fetching-methods-react/) cho đến phần Axios để ôn lại những nội dung đã học trong bài.
1. Đọc [Cách lấy dữ liệu trong React có lưu ý đến hiệu suất](https://www.developerway.com/posts/how-to-fetch-data-in-react) để tìm hiểu thêm về cách xử lý hiệu quả các request trong component React.

</div>
