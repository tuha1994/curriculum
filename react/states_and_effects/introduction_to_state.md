### Giới thiệu

Any exciting ứng dụng you Xây dựng is likely to change over the time the user is exploring it. The changes could be as cơ bản as toggling a dropdown menu hoặc as phức tạp as fetching dữ liệu từ an API. React provides primitives to manipulate the state of your apps, thêm specifically các component, to Tạo them dynamic. Trong bài học này, chúng ta sẽ tìm hiểu cách to sử dụng state in React.

đọc  [state là gì? by Academind](https://academind.com/tutorials/what-is-state) Trước khi we get started.

### Tổng quan bài học

Phần này chứa a general tổng quan of topics that you sẽ tìm hiểu Trong bài học này.

- điều gì state in React is.
- How to sử dụng state in React.
- điều gì happens in React Khi state is changed.

### state là gì in React?

We viết các component in React, A LOT of các component, và many times chúng ta muốn them to undergo visual changes as a result of user hoặc computer interactions. For this purpose, a component cần to "remember" things về itself. This is nơi state comes in. **state is a component's memory.**

Hãy  take a look at cách chúng ta có thể sử dụng state in our mã. Go to our [react-các ví dụ repo](https://github.com/TheOdinProject/react-examples) then fork và clone it. Sau khi you've cloned it, `cd` vào the `state/` directory, nơi Bạn có thể then run `npm install` to get everything set up, then `npm run dev` to start the dev server.

Sau khi opened, you'll see we have an app that changes its background color based on the clicked button. Take your time reading the mã as we'll jump right in và look at the mã within `src/App.jsx`.

#### The useState hook

The `useState` hook is a built-in hook (we'll talk về hooks later) in React that cho phép you to define state in a functional component. It nhận an initial giá trị as a parameter và trả về an array với two các phần tử that chúng ta có thể destructure to get:

1. The current state giá trị
1. A hàm to update the state giá trị

state definition với `useState` commonly follows this pattern:

```javascript
const [stateValue, setStateValue] = useState(initialValue);

// adapted for our use case:
const [backgroundColor, setBackgroundColor] = useState(COLORS[0]);
```

Even không có much knowledge of React, Bạn có thể, to some extent, hiểu điều gì's going on. The `backgroundColor` state is defined với the hook. Then on every button, we set up a *click* event handler that calls the `setBackgroundColor` hàm với the corresponding giá trị. Then, magically the mới color is applied to the background.

Adding thêm state variables nên be as easy as adding thêm `useState` calls.

Great, Bạn có thể bây giờ sử dụng state! But, điều gì's going on bên dưới the hood?

### How does state work in React?

Hãy  hit you với some theory.

In React, Khi a component's state hoặc props change, React runs your component hàm again từ the beginning to figure out điều gì nên be displayed based on the freshly-set state và props. All the calculated changes are then applied to the DOM (committed).
That is, the entire component is recreated, in a sense, nhưng this time the latest state giá trị sẽ be returned từ `useState`. This process is được gọi là rerendering. Rerendering is a key feature of React that enables it to efficiently update the user interface in response to changes in the underlying dữ liệu.

<div class="lesson-note" markdown="1">

#### React reconciliation algorithm

The process of rerendering generates a mới virtual DOM (Document Object Model) tree. The virtual DOM is a lightweight representation of the actual DOM that React uses to keep track of the current state of the UI. React then compares the mới virtual DOM tree to the previous one và calculates the minimal set of changes needed to update the actual DOM. This is the reconciliation algorithm.

</div>

#### Explaining rerendering với the ví dụ bên trên

Whenever `setBackgroundColor` is được gọi là, our `App` component is rerendered. Essentially, the component is recreated điều đó có nghĩa là the `onButtonClick` hàm và our `div` và `button`s are recreated as well. You might wonder, shouldn't the `backgroundColor` state be recreated as well? Well, React nhận the responsibility of keeping track of the latest state và providing it to the component. The initial state giá trị is chỉ được sử dụng for the component's đầu tiên render và is ignored on subsequent renders.

![mã illustration of rerendering](https://cdn.statically.io/gh/TheOdinProject/curriculum/abf5962ab36c951bab907a9952d09400651eab10/react/states_and_effects/introduction_to_state/imgs/00.png)

điều gì we covered in this section is just a brief tổng quan of a portion of React internals. You may be curious to tìm hiểu thêm, nhưng be advised, this is a deep rabbit hole. điều gì we covered bên trên is enough to get you qua the React course.

### Hooks

Hooks are functions that let you sử dụng React features. All hooks are recognizable by the `use` prefix. Ví dụ, `useState` is a hook. We'll sử dụng thêm of these as we get further vào the course. For bây giờ, Hãy nhớ rằng hooks have rules that we cần to abide by:

1. Hooks có thể chỉ be được gọi là từ the top level of a functional component.
1. Hooks có thể't be được gọi là từ bên trong loops hoặc conditions.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. đọc the following articles từ the React docs:
    1. [State: A Component's Memory](https://react.dev/learn/state-a-components-memory)
    1. [Render and Commit](https://react.dev/learn/render-and-commit)
1. đọc this [article on React Reconciliation Algorithm](https://www.geeksforgeeks.org/reactjs-reconciliation/) for a great explanation.
1. Head back to the state ví dụ app từ our `react-examples` repo earlier in the bài học. Thêm a mới state variable to keep track of the number of times the background color has been changed. Display the number of times the background color has been changed on the page.

</div>
