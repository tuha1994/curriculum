### Giới thiệu

Một trong những việc mạnh mẽ nhất mà web developer có thể làm là lấy dữ liệu từ máy chủ và hiển thị dữ liệu đó một cách sáng tạo trên trang của mình. Trong nhiều trường hợp, máy chủ chỉ tồn tại cho riêng trang đó. Máy chủ có thể chứa bài blog, dữ liệu người dùng, điểm cao của một trò chơi hoặc bất kỳ thứ gì khác. Trong những trường hợp khác, máy chủ là một dịch vụ mở, cung cấp dữ liệu cho bất kỳ ai muốn sử dụng (ví dụ: dữ liệu thời tiết hoặc giá cổ phiếu). Dù thuộc trường hợp nào, cách truy cập rồi sử dụng dữ liệu về cơ bản vẫn giống nhau.

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Giải thích API là gì.
- Giải thích khái quát cách truy cập API.
- Giải thích cách fetch và trích xuất dữ liệu từ API.
- Giải thích vì sao request API có thể bị trình duyệt chặn và cách khắc phục.

### API

Các máy chủ được tạo ra để cung cấp chức năng và dữ liệu cho mục đích sử dụng bên ngoài (trong website hoặc app) thường thực hiện việc đó thông qua API, hay ['Application Programming Interfaces'](https://www.youtube.com/watch?v=s7wmiS2mSXY).

Có nhiều cách yêu cầu dữ liệu từ API, nhưng về cơ bản tất cả đều làm cùng một việc. Phần lớn API được truy cập thông qua URL, còn chi tiết về cách query các URL này thay đổi tùy theo dịch vụ cụ thể. Ví dụ, [Visual Crossing](https://www.visualcrossing.com/weather-api) có một weather API với nhiều loại dữ liệu bạn có thể yêu cầu. Để lấy thời tiết hiện tại tại một địa điểm cụ thể, bạn có thể truyền tên thành phố vào path của URL như sau:

```text
https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/london
```

Thông tin cụ thể về cách sử dụng mỗi API thường được ghi lại trên website của dịch vụ đó. Hãy xem [Visual Crossing API Documentation](https://www.visualcrossing.com/resources/documentation/weather-api/timeline-weather-api/). Nếu chưa làm, hãy dán URL thời tiết ở trên, cùng với thành phố bạn chọn, vào trình duyệt... (chúng ta sẽ chờ).

Bạn có thể sẽ nhận được lỗi như sau:

```text
No API key or session found. Please verify that your API key parameter is correct.
```

Điều này dẫn chúng ta đến một điểm khác về API. Trong hầu hết trường hợp, bạn sẽ phải tạo tài khoản và yêu cầu một “API key” từ dịch vụ API trước khi thử fetch dữ liệu từ các endpoint của nó (các URL cụ thể được dùng để truy cập một chức năng hoặc dữ liệu cụ thể trong API). Sau khi có API key, thông thường bạn phải đưa nó vào mọi request dữ liệu. Với Visual Crossing, đó là một query string parameter:

```text
https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/london?key=11111111111111111
```

Như bạn có thể hình dung, API key là ngẫu nhiên và duy nhất đối với bạn. Vì vậy, các dịch vụ như Visual Crossing có thể liên kết API key của bạn với các request dữ liệu, bao gồm số lượng và tần suất bạn yêu cầu dữ liệu.

Việc cấp API key giúp dịch vụ API theo dõi tốt hơn hành vi lạm dụng hệ thống và dữ liệu của họ. Ngoài ra, đây cũng có thể là một cách để các dịch vụ đó giảm thiểu và thu hồi chi phí vận hành. Ví dụ, Visual Crossing không chỉ cung cấp tier miễn phí mà còn có nhiều tier trả phí, có thể lên đến 150 USD/tháng! Dù sao thì việc vận hành máy chủ cũng tốn tiền, và API cũng không ngoại lệ. Một request API có thể chỉ tốn một phần nhỏ của một cent, nhưng hãy tưởng tượng bạn dùng API đó để tạo một weather app tuyệt vời được sử dụng trên toàn thế giới... rất dễ có hàng nghìn người truy cập dữ liệu mỗi phút! Chi phí xử lý lượng traffic đó có thể nhanh chóng tăng thành một khoản đáng kể đối với dịch vụ API.

Vì vậy, bạn sẽ thấy hầu hết, nếu không muốn nói là tất cả, dịch vụ API đều cung cấp các tier trả phí, cho phép tạo request thường xuyên hơn hoặc truy cập nhiều thông tin hơn, vốn không có trong các tier thấp hơn. Ví dụ, phiên bản miễn phí của Visual Crossing cho phép 1000 cuộc gọi/request mỗi ngày với lượng thông tin giới hạn (dù đủ cho một pet project). Mặt khác, phiên bản Enterprise cung cấp số lần gọi API không giới hạn mỗi tháng, có dữ liệu Energy, dữ liệu Maritime... đủ loại tính năng ([chi tiết về mức giá của Visual Crossing](https://www.visualcrossing.com/weather-data-editions) nếu bạn tò mò). Nếu app của bạn thành công và cần thêm tính năng, có lẽ bạn sẽ phải trả tiền cho một tài khoản tốt hơn.

Vì API key là **chìa khóa của bạn** để truy cập các dịch vụ và dữ liệu này, bảo vệ chúng là một thói quen quan trọng, đặc biệt khi bạn dùng tier trả phí. Có rất nhiều bot chuyên crawl repository GitHub chỉ để tìm API key bị hardcode hoặc không được bảo vệ, từ đó cho phép những tác nhân xấu truy cập và [sử dụng các dịch vụ cùng dữ liệu mà bạn đã trả tiền](https://web.archive.org/web/20150102022540/http://www.devfactor.net/2014/12/30/2375-amazon-mistake/). Thực tế, những độc giả tinh mắt hơn có thể đã nhận ra một vấn đề trong phần minh họa ở trên: API key nằm ngay trong request URL. Một kẻ nghe lén traffic internet, thậm chí là người đứng nhìn qua vai bạn, cũng không cần nhiều công sức để lấy được API key!

Tuy nhiên, ở thời điểm này trong curriculum, điểm này phần lớn không còn đáng kể. Dù sao chúng ta cũng đang tận dụng quyền truy cập API miễn phí, và phần lớn app của chúng ta chỉ được chính chúng ta cùng những người xem portfolio sử dụng. Hiện tại, hãy ghi nhớ những hạn chế nghiêm trọng của việc dùng API key như minh họa ở trên. Để bảo vệ API key, cần xử lý chúng ở phía server, còn chúng ta hiện chỉ tập trung vào các khái niệm frontend. Nếu bạn theo lộ trình Full Stack JavaScript, backend sẽ được học sau trong curriculum.

Quay lại với Visual Crossing. Hãy [tạo một tài khoản Visual Crossing miễn phí](https://www.visualcrossing.com/sign-up) để lấy API key. Bạn có thể tìm API key trong trang profile tài khoản. Sau khi có key, hãy thử tạo request mới với thành phố bạn chọn và truyền API key vào dưới dạng query string parameters, như ví dụ ở trên. Hy vọng bạn sẽ thấy một response hợp lệ, tương tự như sau:

```JSON
{"queryCost":1,"latitude":51.5064,"longitude":-0.12721,"resolvedAddress":"London, England, United Kingdom","address":"london","timezone":"Europe/London","tzoffset":1.0,"description":"Similar temperatures continuing with a chance of rain tomorrow, Tuesday & Thursday.","days":[{"datetime":"2024-07-06","datetimeEpoch":1720220400,"tempmax":61.4,"tempmin":53.1,"temp":57.8,"feelslikemax":61.4,"feelslikemin":53.1,"feelslike":57.8,"dew":51.3,"humidity":79.7,"precip":0.457,"precipprob":100.0,"precipcover":75.0,"preciptype":["rain"],"snow":0.0,"snowdepth":0.0,"windgust":35.3,"windspeed":21.9,"winddir":262.6,"pressure":1001.8,"cloudcover":70.5,"visibility":8.3,"solarradiation":147.5,"solarenergy":12.9,"uvindex":6.0,"severerisk":10.0,"sunrise":"04:52:02","sunriseEpoch":1720237922,"sunset":"21:18:20","sunsetEpoch":1720297100,"moonphase":0.02,"conditions":"Rain, Partially cloudy","description":"Partly cloudy throughout the day with a chance of rain throughout the day.","icon":"rain","stations":["EGWU","EGLL","D5621","EGLC"]}]}
```

(Phần preview ở trên mới chỉ là phần nổi của tảng băng; response thực tế còn dài hơn rất nhiều!)

<div class="lesson-note" markdown="1">

#### Query builder của Visual Crossing

Tò mò muốn biết cách tạo query? Hãy xem [query builder của Visual Crossing](https://www.visualcrossing.com/weather/weather-data-services)! Công cụ này sẽ giúp bạn xây dựng query với region là input duy nhất. Hãy thử thay đổi location và các tùy chọn query. Thú vị chứ? Output sử dụng tab “Grid”, còn query của chúng ta nằm trong tab “API”, và chúng ta muốn tìm hiểu *cách* query được cấu trúc. Nhấp vào tab “API”, bạn sẽ thấy query đã tạo ra định dạng grid mà bạn vừa xem.

</div>

Chúc mừng bạn đã thực hiện request API đầu tiên!

### Fetch dữ liệu

Vậy làm thế nào để đưa dữ liệu từ API vào mã của chúng ta?

Một vài năm trước, cách chính để truy cập dữ liệu API trong mã là dùng `XMLHttpRequest`. Hàm này vẫn hoạt động trên mọi trình duyệt, nhưng tiếc là không dễ sử dụng. Cú pháp trông như sau:

```javascript
// Just getting XHR is a mess!
if (window.XMLHttpRequest) { // Mozilla, Safari, ...
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) { // IE
  try {
    request = new ActiveXObject('Msxml2.XMLHTTP');
  }
  catch (e) {
    try {
      request = new ActiveXObject('Microsoft.XMLHTTP');
    }
    catch (e) {}
  }
}

// Open, send.
request.open('GET', 'https://url.com/some/url', true);
request.send(null);
```

Ôi. Thật đau đớn.

Các developer, sau khi chịu đựng việc phải viết những thứ đó, bắt đầu tạo các thư viện bên thứ ba để xử lý và giúp việc sử dụng dễ dàng hơn. Một số thư viện phổ biến là [axios](https://github.com/mzabriskie/axios) và [superagent](https://github.com/visionmedia/superagent), mỗi thư viện đều có điểm mạnh và điểm yếu riêng.

Tuy nhiên, gần đây hơn, các trình duyệt web bắt đầu triển khai một hàm native mới để thực hiện HTTP request, và đó là hàm chúng ta sẽ dùng trong thời gian này. Hãy làm quen với fetch:

```javascript
// URL (required), options (optional)
fetch('https://url.com/some/url')
  .then(function(response) {
    // Successful response :)
  })
  .catch(function(err) {
    // Error :(
  });
```

Nếu bạn quên, hãy cuộn lên và xem lại cách dùng XHR để làm cùng việc đó. Trong khi ngưỡng mộ đoạn mã gọn gàng, sạch sẽ này, hãy để ý các hàm `.then()` và `.catch()`. Bạn còn nhớ đó là gì không? (PROMISE!)

Hãy đổi API cho ví dụ này. Chúng ta sẽ đi qua một ví dụ dùng fetch với API [giphy](https://giphy.com/) để hiển thị một GIF ngẫu nhiên trên webpage. API yêu cầu bạn [đăng ký Giphy và lấy một API key miễn phí](https://developers.giphy.com/docs/api/#quick-start-guide).

Giphy có một số method để tìm kiếm và tìm GIF, bạn có thể đọc về chúng trong tài liệu. Hôm nay chúng ta chỉ dùng endpoint 'random' vì đây là endpoint đơn giản nhất cho mục đích của mình. Bạn có thể tìm URL phù hợp trong [tài liệu endpoint `random` của Giphy](https://developers.giphy.com/docs/api/endpoint/random). Tài liệu cho biết URL chính xác là `api.giphy.com/v1/gifs/random` và URL này yêu cầu parameter `api_key`. Bạn cũng có thể cung cấp parameter `tag` để thu hẹp các GIF được trả về. Có những parameter tùy chọn khác bạn có thể đưa vào query. Ví dụ, parameter `rating` cho phép tìm nội dung ở các mức độ nhạy cảm khác nhau. Nếu ghép mọi thứ lại đúng cách (với API key CỦA BẠN), bạn sẽ nhận được kết quả tương tự:

```javascript
'https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats&rating=g'
// of course we're searching for cats
```

Hãy thử URL đó (với API key CỦA BẠN) trong trình duyệt. Nếu mọi việc suôn sẻ, bạn sẽ nhận được một chuỗi dữ liệu tương đối dài và không có lỗi.

### Bắt tay vào làm

Trước mắt, chúng ta sẽ đặt tất cả trong một file HTML duy nhất. Hãy tạo một file với một thẻ image trống và một thẻ script rỗng trong body.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <img src="#">
  <script>
  </script>
</body>
</html>
```

Trong thẻ script, hãy bắt đầu bằng cách chọn phần tử image và gán nó cho một biến để chúng ta có thể thay đổi URL sau khi nhận được URL đó từ Giphy API.

```html
<script>
  const img = document.querySelector('img');
</script>
```

Thêm fetch với URL ở trên cũng tương đối dễ:

```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats&rating=g')
    .then(function(response) {
      console.log(response.json());
    });
</script>
```

Bây giờ bạn có thể mở file HTML trong trình duyệt. Dù không thấy gì trên trang, bạn *sẽ* thấy một nội dung được log trong console. Phần khó nhất của toàn bộ quy trình này là giải mã cách lấy dữ liệu mong muốn từ response của máy chủ. Trong trường hợp này, kiểm tra console trình duyệt sẽ cho thấy giá trị trả về là *một* Promise khác... để lấy dữ liệu cần thiết, chúng ta cần thêm một hàm `.then()`.

```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats&rating=g')
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      console.log(response);
    });
</script>
```

Bây giờ chúng ta có một đối tượng JavaScript. Nếu kiểm tra đủ kỹ, bạn sẽ thấy dữ liệu cần (URL của một image) được lồng khá sâu bên trong đối tượng:

![response](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/javascript/async-apis/APIs/imgs/00.png)

Để lấy dữ liệu cần thiết, chúng ta phải đi qua từng lớp của đối tượng cho đến khi tìm thấy thứ mình muốn!

```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats&rating=g')
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      console.log(response.data.images.original.url);
    });
</script>
```

Chạy file bây giờ sẽ log URL của image. Tất cả việc còn lại là đặt source của image trên trang thành URL chúng ta vừa truy cập:

```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats&rating=g')
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      img.src = response.data.images.original.url;
    });
</script>
```

Nếu mọi việc suôn sẻ, mỗi lần refresh bạn sẽ thấy một image được chọn ngẫu nhiên trên trang!

Dù chúng ta đang đưa API key này ra frontend, bạn không nên làm vậy với bất kỳ key nào không miễn phí. Key được dùng ở client-side được xem là thông tin công khai, vì vậy phải cẩn thận với các key nhạy cảm và không miễn phí. Cách xử lý key mà không đưa chúng ra frontend sẽ được dạy ở các phần sau nếu bạn chưa học điều đó trong khóa Ruby.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Xem [danh sách Public APIs](https://github.com/n0shake/Public-APIs) này và thỏa sức tưởng tượng.
1. Mở rộng project nhỏ ở đây bằng cách thêm một button để fetch image mới mà không refresh trang.
1. Thêm một search box để người dùng tìm GIF cụ thể. Bạn cũng nên tìm hiểu cách thêm `.catch()` để quản lý một số lỗi (ví dụ: URL không hợp lệ). Hãy nhớ rằng nếu API response, kể cả với nội dung như `404 Not Found` hoặc một response có status không thuộc nhóm 2XX, thì đó vẫn là một response hợp lệ và `fetch` vì thế sẽ không throw lỗi, nghĩa là `.catch()` không chạy. Tuy vậy, nó vẫn có thể chạy nếu đoạn JavaScript tiếp theo của bạn throw lỗi, chẳng hạn cố truy cập một property của `undefined`, hoặc thậm chí nếu bạn tự throw một lỗi.

   Nếu muốn xử lý có điều kiện những tình huống API không trả về response mong muốn (ví dụ: `404 Not Found` hoặc tương tự), bạn sẽ phải tự làm việc đó trong `.then()`. Hãy xem [tài liệu của MDN về đối tượng Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) để biết một số property hữu ích.

</div>