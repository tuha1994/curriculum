### Giới thiệu

Certain các component in React cần to interact với things outside themselves. These things có thể be anything từ querying dữ liệu từ a server to finding/changing the position of the component on the webpage hoặc even sending some dữ liệu to a server Khi necessary. This interaction với the *outside world* is được gọi là a side-effect.

While we are already familiar với kết xuất mã và adding event handlers, it is không enough for tất cả uses, like Khi you want to connect to your server và fetch messages to show to a user. các effect let you run some mã to synchronize your component as necessary, on kết xuất hoặc a reactive/state giá trị change rather than on a particular event.

Similar to cách we have the `useState` hook, React offers us a handy `useEffect` hook to sử dụng các effect in our các component.

### Tổng quan bài học

Phần này chứa a general tổng quan of topics that you sẽ tìm hiểu Trong bài học này.

- hiểu điều gì các effect are in React.
- tìm hiểu cách to sử dụng các effect in React các ứng dụng.
- Explore khác parts of the useEffect hook.
- Identify Khi to sử dụng an effect in React.

### sử dụng effect saves the day

#### The useEffect hook

Let us take a component in question. chúng ta muốn to Tạo a `Clock` component that shows cách many seconds have passed since the user has loaded the webpage. To update it every second, chúng ta có thể sử dụng our nifty `setInterval` hàm to add one to the `counter` state variable, every second. Hãy  try putting it in the body of our component.

```jsx
import { useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  setInterval(() => {
    setCounter(count => count + 1)
  }, 1000);

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

Alas, we see our counter going berserk. This happens vì the `setInterval` hàm is being được gọi là không Sau khi, nhưng at every state render.

Khi our component đầu tiên renders, it calls our initial `setInterval` hàm. That interval updates the state every second, triggering the component to re-render. But every re-render calls `setInterval` again, which triggers thêm frequent state updates, which mỗi spawn mới intervals, và everything quickly spirals out of control.

This is nơi the `useEffect` hook swoops in to save us. chúng ta có thể wrap this calculation bên trong a `useEffect` hook to move it outside the kết xuất calculation. It accepts a callback hàm với tất cả the calculations.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);
  })

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

But, it still keeps growing too fast! This is nơi another argument of the `useEffect` comes in: the dependency array.

#### The dependency array

By default, `useEffect` hook runs on every render. Since setting state tears the component down, we still get multiple setter calls on every render, which doesn't help us.

Fortunately, the second argument accepts an array of dependencies allowing the hook to re-render **chỉ Khi those dependencies are changed**. So if you have a state variable và want to have some side-effect occur any time the state changes, Bạn có thể sử dụng this hook và mention the state variable in the dependency array.

We pass an empty array in this ví dụ vì we do không want the `useEffect` hook to run anytime khác than the initial component render.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);
  }, [])

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

<div class="lesson-note" markdown="1">

#### Managing useEffect dependencies

Usually, you do không cần to add dependencies to your `useEffect` hook manually. Your linter nên let you know về the dependencies it expects. Letting the linter show các lỗi và fixing them Thay vào đó of suppressing them is usually the best idea. On a general Lưu ý, the following block does a good job of summing this point up.

```jsx
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

</div>

#### The clean-up hàm

Oh, it's không going berserk anymore! We still have an issue với the counter updating twice every second though. That có thể be understood as a [behavior caused by the React StrictMode](https://react.dev/reference/react/StrictMode#strictmode). It is supposed to help us catch bugs, so điều gì is that bug ở đây?

với `StrictMode`, the `App` component is mounted, unmounted, then mounted again. This behavior of `StrictMode` is chỉ in the development environment. Notice that every time the `useEffect` hook runs, a mới `setInterval` is được sử dụng.  Khi the component is unmounted the đầu tiên time, `setInterval` is không stopped, it keeps incrementing. This unnecessary behavior có thể be prevented by clearing the interval Khi the component is unmounted và that is nơi the third phần of our `useEffect` hook comes in - the cleanup hàm.

Bạn có thể return a hàm từ the callback in the `useEffect` hook, which sẽ be executed mỗi time Trước khi the tiếp theo effect is run, và one final time Khi the component is unmounted. In this case, let us clean up the interval với a cleanup hàm.

```jsx
import { useEffect, useState } from "react";

export default function Clock() {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const key = setInterval(() => {
      setCounter(count => count + 1)
    }, 1000);

    return () => {
      clearInterval(key);
    };
  }, [])

  return (
    <p>{counter} seconds have passed.</p>
  );
}
```

Phew! It Cuối cùng works! As the counter keeps merrily ticking on, let us sum up điều gì we know về the `useEffect` hook.

```jsx
useEffect(
  () => {
    // execute side effect
    return () => {
      // cleanup function on unmounting or re-running effect
    }
  },
  // optional dependency array
  [/* 0 or more entries */]
)
```

### But do we cần the effect?

`useEffect` is a mechanism outside the concepts that React usually applies, allowing you to sync your component với various external systems like a server, API, hoặc browser DOM. The single question that Bạn có thể ask yourself Trước khi you sử dụng an effect is if ở đó are any such external systems that cần to be synced với, apart từ props hoặc state. Unnecessary `useEffect` hooks are mã-smell, lỗi-prone, và cause unnecessary performance issues.

Let us address a few cases nơi `useEffect` does không cần to be được sử dụng.

- You do không cần to sử dụng an effect if you are chỉ calculating something based on the state during kết xuất. For a change in a component, due to a change in the props, Bạn có thể calculate và set it during kết xuất.

    ```jsx
    import { useState } from "react";

    export default function AdditionDisplay() {
      const [number1, setNumber1] = useState(0);
      const [number2, setNumber2] = useState(0);

      // This is all unnecessary.

      // const [sum, setSum] = useState(0);
      // useEffect(() => {
      //   setSum(number1 + number2);
      // }, [number1, number2]);

      const sum = number1 + number2;

      return (
        <p>{number1} + {number2} = {sum}</p>
      );
    }
    ```

- You do không cần các effect for events. mã that runs Khi a component is **displayed** nên be in các effect, the rest nên be in events.

    ```jsx
    import { useState } from "react";

    export default function App() {
      const [input, setInput] = useState("");

      const handleInput = (e) => {
        setInput(e.target.value);
      };

      // You should avoid direct manipulation when not necessary

      // useEffect(() => {
      //   document.getElementById("name").addEventListener("change", handleInput);
      //   return () => {
      //     document.getElementById("name").removeEventListener("change", handleInput);
      //   }
      // });

      return (
        <>
          {/* <input id="name" /> */}

          <input onChange={handleInput} value={input} />
          <p>{ input }</p>
        </>
      );
    }
    ```

- You do không cần an effect to reset the state based on a condition most of the time. You have đã học về keys in React. Just like sử dụng a key on a list's item, adding one to a component, based on the state on which it nên be reset creates a unique version of that component for mỗi change in the giá trị of the state.

- Nếu you are having issues với managing your state và want to sử dụng an effect to update the state of a parent hoặc some khác non-child component, consider **lifting the state**. As we know, in React, the state flows in one direction, generally down the DOM. So the parents know of the dữ liệu Trước khi passing it to the children. Nếu multiple children are required to Tạo sử dụng of a single state, it nên be moved up to the parent that has tất cả of the các component that cần it, Thay vào đó of sử dụng escape hatches like an effect.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Bài học này từ the React docs talks về the [lifecycle of a component](https://react.dev/learn/lifecycle-of-reactive-effects), the khác stages at which kết xuất nhận place, và the role of `useEffect` in it.
1. thêm các ví dụ từ the React docs về Khi [you might không cần an effect](https://react.dev/learn/you-might-not-need-an-effect).
1. Yet another article that explains a common mistake that beginners Tạo, [the infinite useEffect loop](https://dmitripavlutin.com/react-useeffect-infinite-loop).

</div>
