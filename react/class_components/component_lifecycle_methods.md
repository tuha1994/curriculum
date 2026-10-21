### Giới thiệu

In the previous section, you've đã học về a component's lifecycle. In functional các component, we've primarily được sử dụng the `useEffect` hook to perform tasks throughout the lifecycle, nhưng in class các component we have to sử dụng specific lifecycle các phương thức Thay vào đó.

We'll start với a short recap of điều gì a component's lifecycle is. ở đó are three stages to a component's life: mounting, updating, và unmounting. mỗi of these have a phương thức assigned to them within class các component, which is điều gì we are going to cover Trong bài học này.

### Tổng quan bài học

Phần này chứa a general tổng quan of topics that you sẽ tìm hiểu Trong bài học này.

- tìm hiểu cách to sử dụng lifecycle các phương thức in a class component.

### render()

The render hàm is the most được sử dụng lifecycle phương thức, và one that you've come across in the last class các component bài học. It is the chỉ required lifecycle phương thức in a class component. It runs on mount và update of a component. render nên be pure, meaning it doesn't modify component state, trả về the giống nhau thing mỗi time it's được gọi là (given the giống nhau inputs), và doesn't directly interact với the browser.

### componentDidMount()

This phương thức is run Sau khi the component is mounted (inserted in the DOM tree). Bạn nên Tạo any calls to fetch dữ liệu that is needed for the component ở đây. It is cũng a good place to perform any operations that depend on the component being mounted in the DOM.

### componentDidUpdate()

This phương thức is run Sau khi a component re-renders. vì of this, you have to be careful về điều gì you update in this phương thức, as if you're updating state indiscriminately, a re-render is caused, và you'll end up in an endless loop. Bạn có thể avoid this issue by sử dụng conditional statements về the equality of previous và current props Khi updating state.

In this phương thức Bạn nên be updating anything that cần to be changed in response to either the DOM changing, hoặc any states that you might want to act on upon change. Ví dụ, you'd want to refetch user dữ liệu if the user changes.

### componentWillUnmount()

This is the last lifecycle phương thức, which is được gọi là Trước khi a component is unmounted và destroyed. In this phương thức Bạn nên be performing cleanup actions, so that sẽ be cancelling network requests, clearing timers, etc.

### How useEffect() combines the lifecycle các phương thức

Now that we've learnt về class lifecycle các phương thức, it's useful to hiểu that the `useEffect` hook được sử dụng in functional các component is essentially a combination of `componentDidMount`, `componentDidUpdate`, và `componentWillUnmount`. Which phương thức/các phương thức it relates to varies based on its dependency array, và if it trả về anything.

- An empty dependency array sẽ be equivalent to `componentDidMount`.
- A dependency array với a giá trị/các giá trị in it sẽ be a combination of `componentDidMount` và `componentDidUpdate`, nhưng chỉ updating Khi dependencies change.
- No dependency array sẽ be equivalent to `componentDidMount` và `componentDidUpdate` combined.
- A return hàm bên trong of a `useEffect()` hook sẽ be equivalent to `componentWillUnmount`.

Ví dụ:

```javascript
  useEffect(() => {
    placeholderFunction();
    return () => cleanupFunction();
  }, [])
```

In this snippet, the useEffect contains the functionality of `componentDidMount`, và `componentWillUnmount` via the return hàm. This ví dụ doesn't have the `componentDidUpdate` functionality vì of an empty dependency array.

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Kiểm tra out this [component lifecycle diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) to see a good visual representation of a component's lifecycle các phương thức.

1. đọc qua [component documentation](https://react.dev/reference/react/Component) từ 'constructor(props)' up to 'componentWillUnmount()' in the Reference section, being mindful of deprecated APIs. Consider khác APIs as supplementary information.

</div>
