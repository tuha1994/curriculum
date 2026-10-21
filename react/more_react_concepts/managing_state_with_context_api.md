### Giới thiệu

Trong suốt các bài học, chúng ta đã học cách quản lý trạng thái và truyền dữ liệu cùng props giữa các thành phần. Tuy nhiên, khi ứng dụng của chúng ta phát triển về kích thước, quá trình quản lý trạng thái và truyền dữ liệu này không chỉ trở nên lặp đi lặp lại và bất tiện, mà còn khó quản lý. Trong [bài học React Router](https://www.theodinproject.com/lessons/node-path-react-new-react-router#outlets-and-state), chúng ta đã đề cập cách chúng ta có thể sử dụng outlet context để truyền dữ liệu từ một thành phần cha qua một `<Outlet />` đến một thành phần con. Bài học này sẽ đề cập cách chúng ta có thể sử dụng Context API để làm những việc tương tự ngoài các tình huống outlet.

### Tổng quan bài học

Phần này chứa tổng quan chung về các chủ đề mà bạn sẽ học trong bài học này.

- Sử dụng Context API để truyền trạng thái và dữ liệu sâu vào các thành phần React.
- Ứng dụng thực tế của API Context.
- Những hạn chế của việc sử dụng Context API.

### Tại sao chúng ta cần Context API

Context API trong React là một tính năng cho phép bạn quản lý trạng thái toàn cục của ứng dụng mà không cần phải truyền dữ liệu qua nhiều cấp của các thành phần thông qua props. Nó cung cấp một cách để chia sẻ dữ liệu và chức năng giữa các thành phần khác nhau, bất kể chúng nằm ở đâu trong cây thành phần.

Để đưa ra một ví dụ cụ thể hơn, hãy quay lại một dự án mà chúng ta đã từng xây dựng trước đó. Trong dự án Giỏ Hàng, bạn có thể có một Trang Chi Tiết Sản Phẩm mà có thể giống hoặc không giống với cửa hàng tuyệt vời này:

![Bố cục sơ bộ của trang sản phẩm trên website mua sắm](https://cdn.statically.io/gh/TheOdinProject/curriculum/e6a43abb8ad6836796159e0ab6160045293e2a27/react/more-react-concepts/managing-state-with-context-api/imgs/00.png)

Bạn có một bộ đếm trong phần tiêu đề hiển thị số lượng mặt hàng hiện có trong giỏ hàng. Bạn có thể thêm mặt hàng vào giỏ hàng thông qua nút "Thêm vào giỏ hàng".

Đối với các ví dụ bên dưới, chúng ta sẽ bỏ qua việc định tuyến mà chúng ta đã thực hiện trong dự án và các thành phần khác không cần thiết cho các ví dụ của chúng ta. Chúng ta sẽ chỉ xem xét mã liên quan đến bài học này.

Như chúng ta đã biết, nếu chúng ta muốn trạng thái tồn tại và được chia sẻ giữa các thành phần, điều chúng ta muốn làm là [Nâng trạng thái lên](https://react.dev/learn/sharing-state-between-components) lên một tổ tiên/cha chung.

Mã ứng dụng của chúng ta có thể trông như sau:

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

Đầu trang của chúng tôi có thể trông như thế này:

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

Tuyệt. Đây là một mô hình rất phổ biến, chúng ta truyền dữ liệu/hàm giữa các thành phần, không có gì quá phức tạp. Nhưng có rất nhiều thứ cần quản lý chi tiết, có nhiều thành phần mà bạn cần truyền props tới, và thậm chí còn có một chút prop drilling đang xảy ra, nơi `cartItemsCount` được truyền từ `App` -> `Header` -> `Links`.

Cho đến nay, những gì chúng ta đã làm trong thành phần `App` là:

- Truyền `products` và `addToCart` xuống thành phần `ProductDetail`
- Truyền `cartItemsCount` xuống thành phần `Header` và thành phần `Links` là một phần của `Header`

Đây là một ứng dụng rất cơ bản, nhưng hãy tưởng tượng ứng dụng phát triển về kích thước khi nhiều tính năng được thêm vào, chẳng hạn như trong Trang Chi Tiết Sản Phẩm. Sẽ có nhiều thành phần hơn cần lồng nhau, và có khả năng nhiều props lồng nhau hơn. Điều gì sẽ xảy ra nếu chúng ta có một thành phần `Cart` và một thành phần `ProductListing`? Điều gì sẽ xảy ra nếu chúng ta thêm nhiều chức năng khác ngoài chỉ `addToCart`? Nó có thể trở nên lặp đi lặp lại và phức tạp vì, như chúng ta đều biết, nhiều tính năng đồng nghĩa với nhiều độ phức tạp hơn. Điều này đặc biệt phổ biến trong các framework dựa trên thành phần vì chúng ta có xu hướng tạo các thành phần tái sử dụng riêng biệt thay vì chèn trực tiếp các phần tử, điều này dẫn đến việc lồng nhau nhiều hơn và truyền props nhiều hơn.

### Triển khai Context API

Để đơn giản hóa ứng dụng của chúng ta và giảm độ phức tạp, chúng ta có thể triển khai Context API. Có ba yếu tố chính trong API này mà chúng ta cần hiểu:

1. `createContext` - Điều này "tạo ra ngữ cảnh" Duh... Nhưng vâng, đó là cách chúng ta có thể tạo ra ngữ cảnh. Nó nhận bất kỳ giá trị nào, có thể là số, chuỗi hoặc đối tượng, mà có thể được gọi là *giá trị mặc định* của ngữ cảnh, và trả về một đối tượng ngữ cảnh có thể được sử dụng để truyền dữ liệu xuống các thành phần
1. `useContext` - Hook này được sử dụng để tiêu thụ dữ liệu từ một đối tượng context được tạo bởi `createContext`. Chúng ta có thể sử dụng hook này bên trong component của mình để lấy dữ liệu mà chúng ta cần. Hook này nhận đối tượng context làm một đối số
1. `ContextObject` - Đối tượng context là một thành phần nhận một prop gọi là `value`, đó là giá trị context sẽ được truyền xuống các thành phần bất kể chúng được lồng sâu như thế nào. Nói cách khác, đây là một cách để "cung cấp" giá trị context cho các thành phần này. Trước React 19, `ContextObject.Provider` được sử dụng thay cho `ContextObject`, vì vậy bất cứ nơi nào bạn thấy `Provider` chúng tôi có nghĩa là `ContextObject`

Chúng ta có thể bắt đầu bằng cách sử dụng hàm `createContext` có thể được nhập từ mô-đun `react`.

```javascript
import { createContext } from "react";
```

Như chúng tôi đã mô tả trước đó, khi bạn tạo một ngữ cảnh mới bằng cách sử dụng `createContext`, bạn có thể xác định một giá trị ban đầu cho ngữ cảnh. Đây được gọi là giá trị mặc định của ngữ cảnh. Giá trị mặc định có thể là bất kỳ loại giá trị nào - một chuỗi, số, boolean, hoặc thậm chí là một đối tượng hoặc mảng. Lưu ý rằng giá trị mặc định này không thay đổi, nó là tĩnh.

Trong trường hợp của chúng tôi, chúng tôi sẽ sử dụng một đối tượng có chứa hình dạng sau:

```javascript
const ShopContext = createContext({
  products: [],
  cartItems: [],
  addToCart: () => {},
});
```

Trong ví dụ này, giá trị mặc định của chúng tôi là một đối tượng với ba thuộc tính - `products`, `cartItems` và `addToCart`. `products` và `cartItems` là các mảng sẽ chứa sản phẩm và các mặt hàng trong giỏ hàng của người dùng, tương ứng. `addToCart` là một hàm sẽ cho phép chúng tôi thêm các mặt hàng vào giỏ hàng.

Đối tượng mà chúng ta đã định nghĩa không cần thiết. Tất nhiên chúng ta cũng có thể làm điều này và điều đó sẽ không thành vấn đề chút nào:

```javascript
const ShopContext = createContext(null);
```

Tuy nhiên, lý do mà chúng tôi thêm đối tượng này là để ngay cả khi chúng ta vô tình sử dụng context bên trong một component không được bao bọc trong Provider, vì chúng tôi đã đặt giá trị *default value*, ứng dụng của chúng ta sẽ không bị hỏng (Điều này cũng hữu ích cho việc thử nghiệm vì chúng ta không cần phải bao một component trong Provider để lấy giá trị) và cũng tận dụng các tính năng của IDE như tự động điền khi chúng ta có một đối tượng làm giá trị. Khi chúng ta sử dụng context này trong các component của mình, chúng ta sẽ có thể truy cập trực tiếp các thuộc tính từ context. Về cơ bản đó là một lợi ích thêm! Bạn có thể quyết định có muốn đặt giá trị mặc định là đối tượng hay chỉ `null` vì dù sao chúng ta cũng sẽ ghi đè giá trị mặc định này.

Vậy chúng ta sử dụng ngữ cảnh này như thế nào? Và điều đó được thực hiện bằng cách sử dụng đối tượng Context như một thành phần và lồng các thành phần con bên trong nó. Trong ví dụ này, chúng ta sẽ loại bỏ hoàn toàn các props.

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

Tuyệt! Bây giờ hãy thử nhìn lại thành phần `Header` của chúng ta, chúng ta cũng sẽ loại bỏ tất cả các props mà chúng ta đã định nghĩa trước đó, và để lấy dữ liệu, chúng ta sẽ sử dụng hook `useContext` có thể được nhập trong module `react`.

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

Chúng tôi đã hoàn toàn loại bỏ vấn đề khoan prop, và chúng tôi có thể thuận tiện lấy `cartItems` trực tiếp trong chính thành phần `Links` vì chúng tôi đã biết rằng dù thành phần có lồng sâu đến đâu, chúng tôi vẫn có thể lấy dữ liệu miễn là *nó được lồng bên trong Provider*.

Hãy thay đổi thành phần `ProductDetail` của chúng ta để làm điều tương tự:

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
Trong thành phần `Header`, chúng tôi đã sử dụng `useContext()` để truy cập `cartItems` từ `ShopContext`. Tương tự, trong thành phần `ProductDetail`, chúng tôi có thể sử dụng các chức năng `products` và `addToCart`.

Với cách triển khai này, chúng ta không còn cần truyền props từ component `App` xuống các component `Header` và `ProductDetail` nữa. Các component `Header` và `ProductDetail` có thể truy cập trạng thái `cartItems` và hàm `addToCart` từ `ShopContext`.

Tổng thể, việc triển khai Context API đã cho phép một cách hiệu quả, gọn gàng và tinh gọn hơn trong việc truyền dữ liệu giữa nhiều component. Bằng cách sử dụng hàm `createContext()` và hook `useContext()`, chúng ta có thể dễ dàng truyền trạng thái và các hàm xuống các component con mà không cần phải truyền prop nhiều cấp. Nó cũng tập trung hơn vì chúng ta giữ dữ liệu ở một vị trí duy nhất. Tất cả những điều này khiến mã của chúng ta dễ hiểu hơn.

### Những hạn chế của việc sử dụng Context API

Mặc dù Context API có thể là một công cụ mạnh mẽ để quản lý trạng thái trong các ứng dụng React lớn hơn, nhưng nó cũng có một số nhược điểm mà bạn nên biết:

1. Nó có thể dẫn đến các vấn đề về hiệu suất: Khi bạn cập nhật trạng thái trong một ngữ cảnh, điều này có thể khiến tất cả các thành phần đang sử dụng ngữ cảnh đó phải render lại, ngay cả khi trạng thái mà chúng đang sử dụng không thay đổi. Điều này có thể dẫn đến các vấn đề về hiệu suất, đặc biệt nếu bạn có nhiều thành phần đang sử dụng cùng một ngữ cảnh.
1. Nó có thể làm cho mã của bạn khó theo dõi hơn: Với Context API, việc truy cập trạng thái từ bất kỳ thành phần nào trong ứng dụng của bạn trở nên dễ dàng hơn. Tuy nhiên, điều này cũng có thể làm cho mã của bạn khó theo dõi hơn, đặc biệt nếu bạn có nhiều thành phần lồng nhau đang sử dụng cùng một context. Việc giữ cho mã của bạn được tổ chức và có cấu trúc tốt là rất quan trọng để tránh nhầm lẫn.

### Các giải pháp tiềm năng

1. Sử dụng nhiều ngữ cảnh nhỏ thay vì một ngữ cảnh lớn duy nhất. Thay vì sử dụng một ngữ cảnh lớn duy nhất để quản lý tất cả trạng thái của ứng dụng, hãy xem xét việc sử dụng nhiều ngữ cảnh nhỏ hơn để quản lý các phần trạng thái liên quan. Điều này có thể giúp giảm số lượng thành phần đang sử dụng ngữ cảnh và giảm thiểu việc render lại không cần thiết.
1. Đôi khi Context API thậm chí có thể không phải là giải pháp tốt nhất cho những vấn đề mà chúng ta muốn xử lý. Hãy xem bài viết [React Component Composition](https://www.robinwieruch.de/react-component-composition/) của Robin Wieruch.
1. Bạn có thể dựa vào các hệ thống quản lý trạng thái bên ngoài như [Zustand](https://github.com/pmndrs/zustand) và [Redux](https://redux.js.org/). Chúng có nhiều tối ưu hóa tích hợp sẵn và có nhiều tính năng. Thật không may, chúng có một đường cong học tập, và chúng tôi khuyên bạn nên gắn bó với Context API cho phần còn lại của khóa học này vì nó vẫn đáng tin cậy cho phần lớn các dự án mà chúng ta sẽ xây dựng.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Tài liệu React cung cấp các ví dụ sinh động hơn và các khả năng tối ưu hóa cho Context API. Bạn có thể xem qua bằng cách truy cập [tài liệu về việc truyền dữ liệu sâu với Context](https://react.dev/learn/passing-data-deeply-with-context) của họ. Hãy chắc chắn thử từng ví dụ!
1. Đọc bài viết ngắn [Prop Drilling](https://kentcdodds.com/blog/prop-drilling) của Kent C. Dodds. Đây là một bài viết tuyệt vời để hiểu thêm về prop drilling, nó có những ví dụ dễ hiểu.

</div>
