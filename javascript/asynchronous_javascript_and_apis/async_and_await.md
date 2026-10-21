### Giới thiệu

Mã bất đồng bộ có thể trở nên khó theo dõi khi có quá nhiều việc diễn ra. `async` và `await` là hai từ khóa có thể giúp mã bất đồng bộ đọc giống mã đồng bộ hơn. Nhờ đó, mã có thể trông gọn gàng hơn mà vẫn giữ được lợi ích của mã bất đồng bộ.

Ví dụ, hai khối mã dưới đây thực hiện chính xác cùng một việc. Cả hai đều lấy thông tin từ máy chủ, xử lý thông tin rồi trả về một promise.

```javascript
function getPersonsInfo(name) {
  return server.getPeople().then(people => {
    return people.find(person => { return person.name === name });
  });
}
```

```javascript
async function getPersonsInfo(name) {
  const people = await server.getPeople();
  const person = people.find(person => { return person.name === name });
  return person;
}
```

Ví dụ thứ hai trông giống hơn với kiểu hàm bạn thường viết. Tuy nhiên, bạn có để ý từ khóa `async` trước khai báo hàm không? Còn từ khóa `await` trước `server.getPeople()` thì sao?

Nếu muốn tự chạy thử các hàm này, hãy dán khối mã sau, đại diện cho một máy chủ, trước các định nghĩa hàm. Cách “máy chủ” này hoạt động không quan trọng và chỉ là một abstraction. Mục tiêu là để bạn thấy cả hai hàm hoạt động hoàn toàn giống nhau và đều trả về một promise.

```javascript
const server = {
  people: [
    {
      name: "Odin",
      age: 20,
    },
    {
      name: "Thor",
      age: 35,
    },
    {
      name: "Freyja",
      age: 29,
    },
  ],

  getPeople() {
    return new Promise((resolve, reject) => {
      // Simulating a delayed network call to the server
      setTimeout(() => {
        resolve(this.people);
      }, 2000);
    });
  },
};
```

### Tổng quan bài học

Phần này cung cấp tổng quan về các chủ đề bạn sẽ học trong bài học này.

- Giải thích cách khai báo một hàm `async`.
- Giải thích từ khóa `async` làm gì.
- Giải thích từ khóa `await` làm gì.
- Giải thích một hàm `async` trả về gì.
- Giải thích điều gì xảy ra khi một lỗi được throw bên trong một hàm `async`.
- Giải thích cách xử lý lỗi bên trong một hàm `async`.

### Từ khóa async

Từ khóa `async` cho engine JavaScript biết rằng bạn đang khai báo một hàm bất đồng bộ. Đây là điều bắt buộc nếu muốn dùng `await` bên trong bất kỳ hàm nào. Khi một hàm được khai báo với `async`, hàm đó tự động trả về một promise; câu lệnh return trong một hàm `async` tương đương với việc resolve một promise. Tương tự, việc throw một lỗi sẽ reject promise.

Điều quan trọng cần hiểu là các hàm `async` chỉ là syntactical sugar cho `promises`.

Từ khóa `async` cũng có thể dùng với mọi cách tạo hàm. Nói cách khác: bạn có thể dùng một hàm `async` ở bất cứ nơi nào có thể dùng một hàm thông thường. Dưới đây là một vài ví dụ có thể không trực quan. Nếu chưa hiểu, hãy quay lại xem chúng sau khi hoàn thành các bài tập.

```javascript
const yourAsyncFunction = async () => {
  // do something asynchronously and return a promise
  return result;
}
```

```javascript
anArray.forEach(async item => {
  // do something asynchronously for each item in 'anArray'
  // one could also use .map here to return an array of promises to use with 'Promise.all()'
});
```

```javascript
server.getPeople().then(async people => {
  people.forEach(person => {
    // do something asynchronously for each person
  });
});
```

### Từ khóa await

`await` thực hiện việc sau: nó bảo JavaScript chờ một thao tác bất đồng bộ hoàn tất trước khi tiếp tục thực thi hàm. Nó giống như một từ khóa “tạm dừng cho đến khi xong”. Từ khóa `await` được dùng để lấy một giá trị từ hàm, tại nơi mà thông thường bạn sẽ dùng `.then()`. Thay vì gọi `.then()` sau hàm bất đồng bộ, bạn gán một biến cho kết quả bằng `await`. Sau đó, bạn có thể dùng kết quả trong mã giống như với mã đồng bộ.

### Xử lý lỗi

Việc xử lý lỗi trong các hàm `async` rất dễ. Promise có method `.catch()` để xử lý promise bị reject, và vì các hàm async chỉ trả về một promise, bạn có thể gọi hàm rồi nối thêm method `.catch()` ở cuối.

```javascript
asyncFunctionCall().catch(err => {
  console.error(err)
});
```

Nhưng còn một cách khác: câu lệnh [try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) mạnh mẽ! Nếu muốn xử lý lỗi trực tiếp bên trong hàm async, bạn có thể dùng `try...catch` với cú pháp `async/await`. Nếu JavaScript throw một lỗi trong khối `try`, mã trong khối `catch` sẽ chạy thay thế (cách này cũng có thể dùng cho mã đồng bộ).

```javascript
async function getPersonsInfo(name) {
  try {
    const people = await server.getPeople();
    const person = people.find(person => { return person.name === name });
    return person;
  } catch (error) {
    // Handle the error any way you'd like
  }
}
```

Cách này có thể trông hơi lộn xộn, nhưng là một cách rất dễ để xử lý lỗi mà không cần nối `.catch()` sau các lần gọi hàm. Cách bạn xử lý lỗi là tùy bạn, và phương pháp nên dùng phụ thuộc vào cách mã được viết. Theo thời gian, bạn sẽ dần cảm nhận được cần làm gì. Các bài tập cũng sẽ giúp bạn hiểu cách xử lý lỗi.

### Thực hành

Bạn còn nhớ project thực hành với Giphy API không? (Nếu không, bạn nên quay lại hoàn thành bài học về API). Chúng ta sẽ chuyển mã dựa trên promise sang mã tương thích với `async/await`. Đây là phần ôn lại mã ban đầu:

```javascript
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats')
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      img.src = response.data.images.original.url;
    })
    .catch(function (error) {
      console.error(error);
    });
</script>
```

Vì `await` không hoạt động ở cấp cao nhất của một script không phải module, chúng ta sẽ phải tạo một hàm `async` bao quanh lần gọi API đến Giphy.

```javascript
<script>
  const img = document.querySelector('img');

  async function getCats() {
    fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats')
      .then(function(response) {
        return response.json();
      })
      .then(function(response) {
        img.src = response.data.images.original.url;
      })
      .catch(function(error) {
        console.error(error);
      });
  }
</script>
```

Bây giờ chúng ta đã có một hàm bất đồng bộ, có thể bắt đầu refactor từ việc dùng promise sang dùng `await`:

```javascript
<script>
  const img = document.querySelector('img');

  async function getCats() {
    const response = await fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats');
    response.json().then(function(response) {
      img.src = response.data.images.original.url;
    }).catch(function(error) {
      console.error(error);
    });
  }
</script>
```

Vì `response` vẫn là cùng đối tượng mà chúng ta đã truyền vào khối `.then()` lúc đầu, chúng ta vẫn cần dùng method `.json()`, method này lại trả về một promise. Vì `.json()` trả về một promise, chúng ta có thể dùng `await` để gán response cho một biến.

```javascript
<script>
  const img = document.querySelector('img');

  async function getCats() {
    const response = await fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats');
    const catData = await response.json();
    img.src = catData.data.images.original.url;
  }
</script>
```

Để xử lý lỗi, thay vì nối method `.catch()`, chúng ta cần một câu lệnh `try...catch`. Chúng ta bọc mã thông thường trong khối `try`, rồi dùng khối `catch` cho phần xử lý lỗi.

```javascript
<script>
  const img = document.querySelector('img');

  async function getCats() {
    try {
      const response = await fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats');
      const catData = await response.json();
      img.src = catData.data.images.original.url;
    } catch (error) {
      console.error(error);
    }
  }
</script>
```

Để dùng hàm này, chúng ta chỉ cần gọi nó bằng `getCats()` trong mã.

```javascript
<script>
  const img = document.querySelector('img');

  async function getCats() {
    try {
      const response = await fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_KEY_HERE&tag=cats');
      const catData = await response.json();
      img.src = catData.data.images.original.url;
    } catch (error) {
      console.error(error);
    }
  }
  getCats();
</script>
```

Mã này sẽ hoạt động chính xác như mã của bài học trước; sau khi refactor, nó chỉ trông hơi khác. `async/await` là những công cụ rất hữu ích để làm gọn mã JavaScript bất đồng bộ. Điều quan trọng cần nhớ là `async/await` chỉ là promise được viết theo một cách khác. Hãy làm các bài tập bên dưới và tìm hiểu sâu hơn về `async/await`.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Đọc [bài viết về Async and Await](https://javascript.info/async-await) để có phần giới thiệu vững chắc về async/await. [Bài viết ví dụ về Async and Await](https://codeburst.io/javascript-es-2017-learn-async-await-by-example-48acc58bad65) cũng có một số ví dụ hay về cách sử dụng.
1. Xem [video về Async and Await của Wes Bos](https://www.youtube.com/watch?v=9YkUCxvaLEk) để có cái nhìn tổng quan về async/await, mục đích của nó và một mẹo đặc biệt.

</div>