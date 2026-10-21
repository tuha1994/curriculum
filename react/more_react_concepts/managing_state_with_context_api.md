### Giới thiệu

Trong suốt các bài học, chúng ta đã học cách quản lý trạng thái cũng như truyền dữ liệu và đạo cụ giữa các thành phần. Tuy nhiên, khi ứng dụng của chúng tôi tăng kích thước, quá trình quản lý trạng thái và truyền dữ liệu xung quanh này không chỉ trở nên lặp đi lặp lại và bất tiện mà còn khó quản lý. trong [React Bài học về bộ định tuyến](https://www.theodinproject.com/lessons/node-path-react-new-react-router#outlets-and-state), chúng tôi đã đề cập đến cách chúng tôi có thể sử dụng ngữ cảnh đầu ra để truyền dữ liệu từ thành phần gốc thông qua `<Outlet />` đến thành phần con. Bài học này sẽ đề cập đến cách chúng ta có thể sử dụng Context API để thực hiện những việc tương tự bên ngoài các tình huống đầu ra.

### Tổng quan bài học

Phần này chứa thông tin tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Sử dụng Context API để truyền trạng thái và dữ liệu sâu vào các thành phần React.
- Ứng dụng trong thế giới thực của Context API.
- Những hạn chế của việc sử dụng Context API.

### Tại sao chúng ta cần Context API

Context API trong React là một tính năng cho phép bạn quản lý trạng thái chung của ứng dụng mà không cần truyền dữ liệu qua nhiều cấp độ thành phần bằng cách sử dụng đạo cụ. Nó cung cấp cách chia sẻ dữ liệu và chức năng giữa các thành phần khác nhau, bất kể chúng nằm ở đâu trong cây thành phần.

Để đưa ra một ví dụ cụ thể hơn, hãy quay lại một dự án mà chúng ta đã xây dựng trước đó. Trong dự án Giỏ hàng, bạn có thể có Trang chi tiết sản phẩm có thể trông giống hoặc không giống cửa hàng rất tuyệt vời này:

![Bố cục thô của trang sản phẩm trên trang web mua sắm](https://cdn.statically.io/gh/TheOdinProject/curriculum/e6a43abb8ad6836796159e0ab6160045293e2a27/react/more-react-concepts/managing-state-with-context-api/imgs/00.png)

Bạn có một bộ đếm trong tiêu đề hiển thị số lượng mặt hàng hiện có trong giỏ hàng. Bạn có thể thêm sản phẩm vào giỏ hàng thông qua nút "Thêm vào giỏ hàng".

Đối với các ví dụ bên dưới, chúng tôi sẽ bỏ qua việc định tuyến mà chúng tôi đã thực hiện trong dự án và các thành phần khác không cần thiết cho các ví dụ của chúng tôi. Chúng ta sẽ chỉ xem xét đoạn mã có liên quan đến bài học này.

Như chúng ta đã biết, nếu chúng ta muốn trạng thái tồn tại và được chia sẻ giữa các thành phần, điều chúng ta muốn làm là [Nâng trạng thái lên](https://react.dev/learn/sharing-state-between-components) tới một ancestor/parent. chung

Mã ứng dụng của chúng tôi có thể trông như thế này:

```jsx
import { useState } from "react";
// other imports for Header and ProductDetail

export default function App() {
  const [cartItems, setCartItems] = useState([
    /* List of Items in Cart */
  ]);
  const products = /* some custom hook that fetches products and returns the fetched products */

  const addToCart = (product) => {
    // add to cart logic (this adds to cartItems)
  };

  return (
    <>
      <Header cartItemsCount={cartItems.length} />
      <ProductDetail addToCart={addToCart} products={products} />
    </>
  );
}
```

Hãy tập trung vào các thành phần `Header` và `ProductDetail`.

Tiêu đề của chúng tôi có thể trông như thế này:

```jsx
// import for Link

function Links({ cartItemsCount }) {
  return (
    <ul>
      {/* Links */}
      <li>
        <Link to="Link to the cart">
          <span>Cart</span>
          <div className="cart-icon">{cartItemsCount}</div>
        </Link>
      </li>
    </ul>
  );
}

export default function Header({ cartItemsCount }) {
  return (
    <header>
      {/* Other header elements */}
      <nav>
        <Links cartItemsCount={cartItemsCount} />
      </nav>
    </header>
  );
}
```

Thành phần `ProductDetail` của chúng tôi:

```jsx
export default function ProductDetail({ products, addToCart }) {
  const product = products.find(/* Logic to find the specific product */);

  return (
    <div>
      {/* Image of the product */}
      <div>
        {/* elements that align with the design */}
        <button type="button" onClick={() => addToCart(product)}>
          Add to Cart
        </button>
      </div>
    </div>
  );
}
```

Tuyệt vời. Đây là một mẫu rất phổ biến, chúng ta chuyển data/functions giữa các thành phần, không có gì quá phức tạp. Nhưng có rất nhiều thứ cần quản lý vi mô, rất nhiều thành phần mà bạn cần chuyển đạo cụ tới và thậm chí còn có một chút thao tác khoan chống đỡ đang diễn ra, trong đó `cartItemsCount` được truyền từ `App` -> `Header` -> `Links`.

Cho đến nay, những gì chúng ta đã làm trong thành phần `App` là:

- Truyền `products` và `addToCart` xuống thành phần `ProductDetail`
- Truyền `cartItemsCount` xuống thành phần `Header` và thành phần `Links` là một phần của `Header`

Đây là một ứng dụng rất cơ bản, nhưng hãy tưởng tượng ứng dụng này phát triển về quy mô khi có nhiều tính năng hơn được thêm vào, chẳng hạn như trong Trang chi tiết sản phẩm. Sẽ có nhiều thành phần hơn để lồng vào nhau và có thể có nhiều đạo cụ lồng nhau hơn. Điều gì sẽ xảy ra nếu chúng ta có thành phần `Cart` và thành phần `ProductListing`? Điều gì sẽ xảy ra nếu chúng ta thêm nhiều chức năng khác ngoài `addToCart`? Nó có thể lặp đi lặp lại và phức tạp bởi vì, như chúng ta đều biết, càng nhiều tính năng thì càng phức tạp. Điều này đặc biệt thường xuyên xảy ra trong các khung dựa trên thành phần vì chúng ta có xu hướng tạo các thành phần riêng biệt có thể tái sử dụng thay vì các phần tử nội tuyến, từ đó tạo ra nhiều sự lồng ghép và chuyển giao các đạo cụ hơn.

### Thực hiện Context API

Để đơn giản hóa ứng dụng và giảm độ phức tạp, chúng ta có thể triển khai Context API. Có ba yếu tố chính trong API này mà chúng ta cần hiểu:

1. `createContext` - ​​Điều này "tạo ra bối cảnh" Duh... Nhưng đúng vậy, đó là cách chúng ta có thể tạo ra bối cảnh. Nó nhận bất kỳ giá trị nào, có thể là số, chuỗi hoặc đối tượng, có thể được gọi là*giá trị mặc định*của ngữ cảnh và trả về một đối tượng ngữ cảnh có thể được sử dụng để truyền dữ liệu tới các thành phần
1. `useContext` - ​​Hook này được sử dụng để tiêu thụ dữ liệu từ một đối tượng bối cảnh được tạo bởi `createContext`. Chúng ta có thể sử dụng hook này bên trong thành phần của mình để lấy dữ liệu mà chúng ta cần. Móc này chấp nhận đối tượng bối cảnh làm đối số
1. `ContextObject` - ​​Đối tượng bối cảnh là một thành phần chấp nhận một prop có tên `value`, là giá trị bối cảnh sẽ được truyền xuống các thành phần cho dù chúng được lồng sâu đến đâu. Nói cách khác, một cách để "cung cấp" giá trị ngữ cảnh cho các thành phần này. Trước React 19, `ContextObject.Provider` được sử dụng thay vì `ContextObject`, vì vậy bất cứ khi nào bạn thấy `Provider`, ý chúng tôi là `ContextObject`

Chúng ta có thể bắt đầu bằng cách sử dụng hàm `createContext` có thể được nhập từ mô-đun `react`.

```javascript
import { createContext } from "react";
```

Như chúng tôi đã mô tả trước đó, khi bạn tạo một ngữ cảnh mới bằng cách sử dụng `createContext`, bạn có thể chỉ định giá trị ban đầu cho ngữ cảnh. Đây được gọi là giá trị mặc định của bối cảnh. Giá trị mặc định có thể là bất kỳ loại giá trị nào - một chuỗi, số, boolean hoặc thậm chí là một đối tượng hoặc mảng. Xin lưu ý rằng giá trị mặc định này không thay đổi, nó là giá trị tĩnh.

Trong trường hợp của chúng tôi, chúng tôi sẽ sử dụng một đối tượng có chứa hình dạng sau:

```javascript
const ShopContext = createContext({
  products: [],
  cartItems: [],
  addToCart: () => {},
});
```

Trong ví dụ này, giá trị mặc định của chúng tôi là một đối tượng có ba thuộc tính - `products`, `cartItems` và `addToCart`. `products` và `cartItems` lần lượt là các mảng sẽ chứa sản phẩm và mặt hàng trong giỏ hàng của người dùng. `addToCart` là một chức năng cho phép chúng ta thêm sản phẩm vào giỏ hàng.

Đối tượng mà chúng tôi đã xác định này là không cần thiết. Tất nhiên chúng ta cũng có thể làm điều này và nó sẽ không thành vấn đề gì cả:

```javascript
const ShopContext = createContext(null);
```

Tuy nhiên, lý do tại sao chúng ta thêm đối tượng là để ngay cả khi bằng cách nào đó chúng ta sử dụng ngữ cảnh bên trong một thành phần không được lồng bên trong Provider, bởi vì chúng ta đã đặt một*giá trị mặc định*, ứng dụng của chúng tôi sẽ không bị hỏng (Điều này cũng tốt cho việc thử nghiệm vì chúng tôi không cần bọc một thành phần trong Provider để nhận giá trị) và cũng tận dụng các tính năng của IDE như tự động hoàn thành khi chúng tôi có một đối tượng làm giá trị. Khi chúng tôi sử dụng bối cảnh này trong các thành phần của mình, chúng tôi sẽ có thể truy cập các thuộc tính trực tiếp từ ngữ cảnh. Về cơ bản nó là một phần thưởng! Bạn muốn đặt giá trị mặc định cho đối tượng hay chỉ `null` là tùy thuộc vào bạn vì dù sao thì chúng ta cũng sẽ ghi đè giá trị mặc định này.

Vậy chúng ta sử dụng bối cảnh này như thế nào? Và đó là bằng cách sử dụng đối tượng Context làm thành phần và lồng các thành phần con bên trong nó. Trong ví dụ này, chúng tôi sẽ loại bỏ hoàn toàn các đạo cụ.

```jsx
import { useState, createContext } from "react";
// other imports for Header and ProductDetail

export const ShopContext = createContext({
  products: [],
  cartItems: [],
  addToCart: () => {},
});

export default function App() {
  const [cartItems, setCartItems] = useState([
    /* List of Items in Cart */
  ]);
  const products = /* some custom hook that fetches products and returns the fetched products */

  const addToCart = (product) => {
    // add to cart logic (this adds to cartItems)
  };

  return (
    /* We are going to pass the things that we want to inject to these components using the value prop */
    /* This value prop will overwrite the default value */
    <ShopContext value={{ cartItems, products, addToCart }}>
      <Header />
      <ProductDetail />
    </ShopContext>
  );
}
```

Tuyệt vời! Bây giờ, hãy thử xem lại thành phần `Header` của chúng ta, chúng ta cũng sẽ xóa tất cả các props mà chúng ta đã xác định trước đó và để truy xuất dữ liệu, chúng ta sẽ sử dụng hook `useContext` có thể được nhập trong mô-đun `react`.

```jsx
import { useContext } from "react";
// import for ShopContext
// import for Link

function Links() {
  const { cartItems } = useContext(ShopContext); // We must pass the ShopContext object itself as an argument

  return (
    <ul>
      {/* Other links */}
      <li>
        <Link to="Link to the cart">
          <span>Cart</span>
          <div className="cart-icon">{cartItems.length}</div>
        </Link>
      </li>
    </ul>
  );
}

export default function Header() {
  return (
    <header>
      {/* Other header elements */}
      <nav>
        <Links />
      </nav>
    </header>
  );
}
```

Chúng tôi đã loại bỏ hoàn toàn vấn đề về khoan chống đỡ và chúng tôi có thể lấy `cartItems` một cách thuận tiện ngay trong chính thành phần `Links` vì chúng tôi đã biết rằng cho dù thành phần đó có được lồng sâu đến đâu, chúng tôi vẫn có thể lấy dữ liệu miễn là*nó được lồng bên trong Provider*.

Hãy thay đổi thành phần `ProductDetail` để thực hiện tương tự:

```jsx
import { useContext } from "react";
// import for ShopContext

export default function ProductDetail() {
  const { products, addToCart } = useContext(ShopContext);
  const product = products.find(/* Logic to find the specific product */);

  return (
    <div>
      {/* Image of the product */}
      <div>
        {/* elements that align with the design */}
        <button type="button" onClick={() => addToCart(product)}>
          Add to Cart
        </button>
      </div>
    </div>
  );
}
```

<a name="context-api-benefits"></a>
Trong thành phần `Header`, chúng ta đã sử dụng `useContext()` để truy cập `cartItems` từ `ShopContext`. Tương tự, trong thành phần `ProductDetail`, chúng ta có thể sử dụng hàm `products` và `addToCart`.

Với cách triển khai này, chúng ta không còn cần phải truyền props từ thành phần `App` sang các thành phần `Header` và `ProductDetail` nữa. Các thành phần `Header` và `ProductDetail` có thể truy cập trạng thái `cartItems` và hàm `addToCart` từ `ShopContext`.

Nhìn chung, việc triển khai Context API đã cho phép truyền dữ liệu qua nhiều thành phần một cách hiệu quả hơn, rõ ràng hơn và hợp lý hơn. Bằng cách sử dụng hàm `createContext()` và hook `useContext()`, chúng ta có thể dễ dàng truyền trạng thái và hàm cho các thành phần con mà không cần phải khoan prop. Nó cũng tập trung hơn vì chúng tôi lưu giữ dữ liệu của mình ở một vị trí duy nhất. Tất cả những điều này làm cho mã của chúng ta dễ lý giải hơn.

### Hạn chế của việc sử dụng Context API

Mặc dù Context API có thể là một công cụ mạnh mẽ để quản lý trạng thái trong các ứng dụng React lớn hơn, nhưng nó cũng có một số nhược điểm mà bạn cần lưu ý:

1. Điều này có thể dẫn đến các vấn đề về hiệu suất: Khi bạn cập nhật trạng thái trong một ngữ cảnh, điều này có thể khiến tất cả các thành phần đang sử dụng ngữ cảnh đó hiển thị lại, ngay cả khi trạng thái mà chúng đang sử dụng không thay đổi. Điều này có thể dẫn đến các vấn đề về hiệu suất, đặc biệt nếu bạn có nhiều thành phần đang sử dụng cùng một ngữ cảnh.
1. Nó có thể làm cho mã của bạn khó theo dõi hơn: Với Context API, việc truy cập trạng thái từ bất kỳ thành phần nào trong ứng dụng của bạn sẽ dễ dàng hơn. Tuy nhiên, điều này cũng có thể làm cho mã của bạn khó theo dõi hơn, đặc biệt nếu bạn có nhiều thành phần lồng nhau đang sử dụng cùng một ngữ cảnh. Điều quan trọng là giữ cho mã của bạn được tổ chức và có cấu trúc tốt để tránh nhầm lẫn.

### Giải pháp tiềm năng

1. Sử dụng nhiều ngữ cảnh nhỏ hơn thay vì một ngữ cảnh lớn. Thay vì sử dụng một ngữ cảnh lớn duy nhất để quản lý tất cả các trạng thái ứng dụng của bạn, hãy cân nhắc sử dụng nhiều ngữ cảnh nhỏ hơn để quản lý các phần trạng thái liên quan. Điều này có thể giúp giảm số lượng thành phần đang sử dụng bối cảnh và giảm thiểu việc hiển thị lại không cần thiết.
1. Đôi khi Context API thậm chí có thể không phải là giải pháp tốt nhất cho những vấn đề mà chúng ta muốn giải quyết. Hãy nhìn vào [React Thành phần thành phần](https://www.robinwieruch.de/react-component-composition/) bài viết của Robin Wieruch.
1. Bạn có thể dựa vào các hệ thống quản lý trạng thái bên ngoài như [Zustand](https://github.com/pmndrs/zustand) Và [Redux](https://redux.js.org/). Chúng có rất nhiều tối ưu hóa được tích hợp sẵn và có nhiều tính năng. Thật không may, chúng có một lộ trình học tập và chúng tôi khuyên bạn nên bám sát Context API trong phần còn lại của khóa học này vì nó vẫn đáng tin cậy cho phần lớn các dự án mà chúng tôi sắp xây dựng.

### Phân công

<div class="lesson-content__panel" markdown="1">

1. Tài liệu React cung cấp nhiều ví dụ hấp dẫn hơn và các khả năng tối ưu hóa có thể có cho Context API. Bạn có thể kiểm tra nó bằng cách đi qua [tài liệu về truyền dữ liệu sâu với Context](https://react.dev/learn/passing-data-deeply-with-context). Hãy chắc chắn để thử từng ví dụ!
1. Đọc bài viết ngắn [Khoan chống đỡ](https://kentcdodds.com/blog/prop-drilling) của Kent C. Dodds. Đây là một bài viết tuyệt vời để hiểu thêm về máy khoan chống đỡ, nó có các ví dụ dễ hiểu.

</div>
