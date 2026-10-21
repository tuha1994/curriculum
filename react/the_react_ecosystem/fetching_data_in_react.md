### Giới thiệu

Cho đến thời điểm này, chúng ta vẫn đang sử dụng React để xây dựng các ứng dụng phía máy khách với giao diện người dùng tương tác, nhưng nếu chúng ta muốn tìm nạp dữ liệu từ internet thì sao? Để tạo các ứng dụng web hoàn chỉnh, chúng ta cần một số cách để lấy dữ liệu từ các nguồn bên ngoài và hiển thị nó một cách linh hoạt.

Trong bài học này, chúng ta sẽ khám phá chi tiết về việc tìm nạp dữ liệu trong React, bắt đầu với những điều cơ bản về thực hiện lệnh gọi API, quản lý trạng thái thành phần và xử lý các hoạt động không đồng bộ bằng cách sử dụng hàm JavaScript của JavaScript. Bạn đã thực hiện tìm nạp dữ liệu trong các dự án trước đó, vì vậy một số nội dung trong bài học sẽ quen thuộc với bạn. Sửa đổi không bị tổn thương!

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Thực hiện các yêu cầu tìm nạp trong các thành phần React.
- Bắt lỗi và xử lý.
- Nâng cao yêu cầu lên hệ thống phân cấp thành phần.

### Yêu cầu tìm nạp cơ bản

Trước khi đi sâu vào chi tiết cụ thể về tìm nạp dữ liệu trong React, hãy xem lại ngắn gọn cách chúng ta có thể sử dụng tìm nạp API để lấy dữ liệu từ máy chủ.

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

### Sử dụng tìm nạp trong các thành phần React

Bây giờ, hãy xem cách chúng ta có thể kết hợp `fetch` vào thành phần React, tương tự như ví dụ trước của chúng ta. Một trường hợp sử dụng phổ biến là tìm nạp dữ liệu từ API khi một thành phần được gắn kết để dữ liệu có thể được hiển thị trên màn hình.

Bất cứ khi nào một thành phần cần thực hiện một yêu cầu khi nó hiển thị, tốt nhất nên bọc `fetch` đó bên trong một hiệu ứng.

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

`useState` cho phép chúng ta thêm trạng thái `imageURL`, trong khi `useEffect` cho phép chúng ta thực hiện các tác dụng phụ. Trong trường hợp này, tác dụng phụ là tìm nạp dữ liệu từ API bên ngoài. Vì chúng ta chỉ cần tìm nạp dữ liệu một lần khi thành phần được gắn kết nên chúng ta chuyển một mảng phụ thuộc trống.

### Xử lý lỗi

Làm việc qua mạng vốn không đáng tin cậy. API bạn đang gửi yêu cầu có thể không hoạt động, có thể có sự cố kết nối mạng hoặc phản hồi bạn nhận được có thể có lỗi. Rất nhiều điều có thể xảy ra sai sót và nếu bạn không chuẩn bị trước cho các lỗi, trang web của bạn có thể bị hỏng hoặc có vẻ không phản hồi với người dùng.

Để mô phỏng lỗi mạng, hãy cuộn lên đoạn mã trước đó và thay đổi `fetch` URL thành một cái gì đó ngẫu nhiên. Sau khi làm mới cửa sổ trình duyệt của bạn, trang sẽ vẫn là một màn hình trắng trống mà không cung cấp cho người dùng bất kỳ dấu hiệu nào cho thấy trang đã tải xong hoặc đã xảy ra lỗi.

Để khắc phục điều này chúng ta cần kiểm tra*thứ gì đó*trước khi thành phần Hình ảnh trả về JSX. Chúng ta sẽ gọi nó là: `error`.

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

Để đặt `error` này, chúng ta sẽ thêm nó vào trạng thái của thành phần.

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

### Sử dụng móc tùy chỉnh

Chúng ta có thể tách hoàn toàn logic tìm nạp thành một móc tùy chỉnh. Điều này sẽ cho phép chúng ta làm cho logic có thể tái sử dụng và kiểm tra dễ dàng.

Nhớ lại trong bài học [Giới thiệu về trạng thái](https://www.theodinproject.com/lessons/node-path-react-new-introduction-to-state), chúng ta đã nói rằng hook React chỉ là một hàm cho phép bạn sử dụng các tính năng của React (như state, effect, v.v.). Các hook tuân theo quy tắc đặt tên: bắt đầu bằng `use` theo sau là một chữ cái viết hoa (ví dụ `useState` hoặc `useEffect`). Nếu cố đặt một hook như `useEffect` bên trong một hàm trợ giúp thông thường như `getImageURL`, React sẽ không chấp nhận vì hook chỉ được gọi ở cấp cao nhất của một thành phần hoặc một hook khác. Do đó, chúng ta có thể biến hàm trợ giúp thành một custom hook bằng cách tuân theo quy tắc đặt tên hook — `useImageURL`.

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

Nếu chúng ta cần tìm nạp hình ảnh trong các thành phần khác nhau, thay vì viết lại tất cả logic tìm nạp đó, chúng ta có thể gọi `useImageURL`.

### Quản lý nhiều yêu cầu tìm nạp

<div class="lesson-note lesson-note--warning" markdown="1" >

#### Sử dụng độ trễ nhân tạo

Trong phần này, chúng tôi sử dụng một ví dụ về mã trong đó chúng tôi đã thêm `delay` nhân tạo bằng hàm `setTimeout`. Như bạn có thể đã đoán bây giờ, điều này nhằm giúp bạn tìm hiểu các kiến ​​thức cơ bản về tìm nạp dữ liệu trong bài học. Chúng tôi khuyên bạn nên loại bỏ những độ trễ này sau khi hoàn thành phần này, sau đó xem lại các ví dụ về mã để củng cố thêm các khái niệm.

</div>

Trong một ứng dụng web có quy mô đầy đủ, bạn thường đưa ra nhiều yêu cầu và bạn cần cẩn thận với cách sắp xếp chúng. Một vấn đề phổ biến mà các nhà phát triển React mới gặp phải khi ứng dụng của họ bắt đầu thực hiện nhiều yêu cầu được gọi là*thác nước yêu cầu*. Hãy xem xét một ví dụ trong [repo ví dụ phản ứng](https://github.com/TheOdinProject/react-examples)(phân nhánh và sao chép nó nếu bạn chưa làm vậy), lần này là trong thư mục `fetching-data/`. `cd` vào đó, chạy `npm install` sau đó khởi động máy chủ dev với `npm run dev`.

Chúng tôi có hai thành phần thực hiện yêu cầu tìm nạp: `Profile` và thành phần con của nó `Bio`. Trong cả hai tệp đó, bạn sẽ thấy một thành phần và một phiên bản khác của nó được nhận xét bên dưới - chúng tôi sẽ sớm quay lại những tệp này. Các yêu cầu trong `Profile` và `Bio` đều được kích hoạt bên trong các thành phần tương ứng của chúng. Nhìn bề ngoài, điều này có vẻ như là sự phân tách các mối quan tâm được tổ chức tốt, nhưng trong trường hợp này, nó phải trả giá bằng hiệu suất.

Lưu ý rằng bất cứ khi nào bạn tải trang, `Bio` sẽ mất thêm một giây để hiển thị? Cả hai yêu cầu tìm nạp của họ đều phải mất 1000 mili giây để giải quyết, vậy chuyện gì đang xảy ra vậy? Trong React, thành phần không được hiển thị cho đến khi nó thực sự được gọi. Nếu JSX có logic điều kiện, các nhánh sai sẽ không bao giờ hiển thị cho đến khi chúng trở thành đúng. `Bio` phải đợi yêu cầu bên trong `Profile` được giải quyết trước khi bắt đầu hiển thị, điều đó có nghĩa là yêu cầu bên trong `Bio` không được gửi.

Nếu chúng tôi loại bỏ điều kiện đoản mạch chờ `imageURL`, thì `Bio` sẽ gửi yêu cầu ngay lập tức, nhưng điều đó có nghĩa là chúng tôi sẽ phải bỏ màn hình tải. Thay vì ảnh hưởng đến thiết kế, chúng ta có thể nâng yêu cầu lên cây thành phần và chuyển phản hồi của nó dưới dạng chỗ dựa cho `Bio`.

Để thấy điều này hoạt động, trong cả hai thành phần `Profile` và `Bio`, hãy nhận xét chúng rồi bỏ ghi chú các phiên bản thay thế bên dưới chúng.

Bây giờ chúng tôi có cả hai yêu cầu được kích hoạt ngay khi `Profile` hiển thị. Yêu cầu `imageURL` được xử lý 2 giây trước yêu cầu `bioText` và div chứa `<Bio />` của chúng tôi hiển thị. Khi `bioText` được giải quyết, một bản cập nhật sẽ được thực hiện ở trạng thái kích hoạt hiển thị lại trong `<Bio />`, thêm mô tả văn bản đó vào trang.

### Thư viện tìm nạp dữ liệu

Chúng tôi chỉ mới bắt đầu tìm hiểu bề mặt của việc tìm nạp dữ liệu trên giao diện người dùng. Luôn cập nhật dữ liệu giao diện người dùng của bạn với máy chủ là một nhiệm vụ đầy thách thức cần hoàn thành. Việc quản lý trạng thái "không đồng bộ" ngày càng trở nên phức tạp với mỗi tính năng được thêm vào.

Bạn đã trải nghiệm sự phức tạp của việc tìm nạp dữ liệu trong bài học này. Mỗi yêu cầu phải có một*tối thiểu*gồm ba trạng thái để đạt được trải nghiệm người dùng tối ưu: `data`, `loading` và `error`. Mặc dù một số thư viện có thể giúp bạn tìm nạp dữ liệu và hơn thế nữa, bạn nên sử dụng tìm nạp dữ liệu vanilla React cho tất cả các dự án trong khóa học này. Những bài học bạn sẽ học được khi làm như vậy sẽ là vô giá.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Đọc [Phương pháp tìm nạp dữ liệu API hiện đại](https://blog.logrocket.com/modern-api-data-fetching-methods-react/) cho đến khi bạn đến phần Axios để có cái nhìn tổng quan ngắn gọn về những gì đã được thảo luận trong bài học này.
1. Đọc [Cách tìm nạp dữ liệu trong React có lưu ý đến hiệu suất](https://www.developerway.com/posts/how-to-fetch-data-in-react) để tìm hiểu thêm về cách xử lý hiệu quả các yêu cầu tìm nạp trong các thành phần React.

</div>
