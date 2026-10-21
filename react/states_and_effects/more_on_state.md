### Giới thiệu

Bài học này sẽ Xây dựng upon the previous bài học on state và introduce further concepts và techniques to manage state. Hãy  dive in.

### Tổng quan bài học

Phần này chứa a general tổng quan of topics that you sẽ tìm hiểu Trong bài học này.

- How to structure state.
- How state updates.
- tìm hiểu về controlled các component.

### How to structure state

Managing và structuring state effectively is by far one of the most crucial parts of building your ứng dụng. Nếu không done correctly, it có thể become a source of bugs và headaches.

The assignment items go qua the topic thoroughly, nhưng as a general rule of thumb: don't put các giá trị in state that có thể be calculated sử dụng existing các giá trị, state, và/hoặc props.

#### state nên không be mutated

Mutating state is a no-go area in React as it leads to unpredictable results. Primitives are already immutable, nhưng if you are sử dụng reference-type các giá trị, i.e., arrays và objects, never mutate them. According to the React documentation, we nên treat state as if it was *immutable*. To change state, we nên always sử dụng the `setState` hàm, which in the case of the ví dụ below is the `setPerson` hàm.

```jsx
function Person() {
  const [person, setPerson] = useState({ name: "John", age: 100 });

  // BAD - Don't do this!
  const handleIncreaseAge = () => {
    // mutating the current state object
    person.age = person.age + 1;
    setPerson(person);
  };

  // GOOD - Do this!
  const handleIncreaseAge = () => {
    // copy the existing person object into a new object
    // while updating the age property
    const newPerson = { ...person, age: person.age + 1 };
    setPerson(newPerson);
  };

  return (
    <>
      <h1>{person.name}</h1>
      <h2>{person.age}</h2>
      <button onClick={handleIncreaseAge}>Increase age</button>
    </>
  );
}
```

<div class="lesson-note lesson-note--tip" markdown="1">

#### Objects và arrays in state

In the bên trên ví dụ, notice cách we *tạo* a mới object và then copy the existing state các giá trị vào the mới object while providing a mới giá trị for `age`.
That is vì if we don't provide a mới object to `setState` it is không guaranteed to re-render the page. Do đó, we nên always provide a mới Object for `setState` to trigger a re-render. `setState` uses [Object.is()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) to determine if the previous state is the giống nhau.

As for nested objects và arrays, state có thể get tricky fast since you sẽ have to copy the nested items as well. Be careful Khi sử dụng them.

</div>

### How state updates

state updates are asynchronous. điều gì this implies is that whenever you call the `setState` hàm, React sẽ apply the update in the **tiếp theo** component render. This concept có thể take a while to wrap your head around. với a lot of practice, you'll get the hang of it in no time.

Remember, state variables aren't reactive; the component is. This có thể be understood by the fact that calling `setState` re-renders the entire component Thay vào đó of just changing the state variable on the fly.

```jsx
function Person() {
  const [person, setPerson] = useState({ name: "John", age: 100 });

  const handleIncreaseAge = () => {
    console.log("in handleIncreaseAge (before setPerson call): ", person);
    setPerson({ ...person, age: person.age + 1 });
    // we've called setPerson, surely person has updated?
    console.log("in handleIncreaseAge (after setPerson call): ", person);
  };

  // this console.log runs every time the component renders
  // what do you think this will print?
  console.log("during render: ", person);

  return (
    <>
      <h1>{person.name}</h1>
      <h2>{person.age}</h2>
      <button onClick={handleIncreaseAge}>Increase age</button>
    </>
  );
}
```

These are the logs:

![browser console of the bên trên mã snippet](https://cdn.statically.io/gh/TheOdinProject/curriculum/103edd69831b1f0e946258009fe36a462c70c163/react/states_and_effects/more_on_state/imgs/00.png)

Uh-oh, điều gì is happening? Hãy  break it down (ignore the double `console.log`s for the render case; this is covered in the upcoming lessons).

1. The component renders for the đầu tiên time. The `person` state variable is initialized to `{ name: 'John', age: 100 }`. The "during render" `console.log` prints the state variable.
1. The button is clicked invoking `handleIncreaseAge`. Interestingly, the `console.log` Trước khi và Sau khi the `setPerson` call prints the giống nhau giá trị.
1. The component re-renders. The `person` state variable is updated to `{ name: 'John', age: 101 }`.

The `person` state stays the giống nhau throughout the current render of the component. This is điều gì "state as a snapshot" refers to. The `setState` call triggers a component re-render, và the `person` state is updated to the mới giá trị.

<div class="lesson-note lesson-note--warning" markdown="1">

#### The unexpected infinite loop

The following sẽ cause an infinite loop, nhưng React có thể, bên dưới certain circumstances, detect infinite re-renders và throw an lỗi. Can you guess tại sao React sẽ throw an lỗi ở đây, cảnh báo you về too many re-renders? Drop by in the [TOP Discord server](https://discord.gg/fbFCkYabZB), tell us tại sao, và score a brownie point!

```jsx
function Component() {
  const [count, setCount] = useState(0);

  setCount(count + 1);

  return <h1>{count}</h1>;
}
```

</div>

#### state updater functions

A trick question. Hãy  look at another implementation of `handleIncreaseAge`; điều gì do you think it does?

```jsx
const handleIncreaseAge = () => {
  setPerson({ ...person, age: person.age + 1 });
  setPerson({ ...person, age: person.age + 1 });
};
```

Surely, it sẽ increase the age by 2? Nope. The bên trên mã is saying to React:

> Hey, replace the current render's `person` với an increase in age by 1. Then, replace the current render's `person` với an increase in age by 1.

Notice the word "replace". Khi you pass in the giá trị to the `setState` hàm, React sẽ replace the current state với the giá trị you passed in. You might be wondering, điều gì if I want to update the state multiple times sử dụng the latest state? This is nơi the state updater hàm comes in.

```jsx
const handleIncreaseAge = () => {
  setPerson((prevPerson) => ({ ...prevPerson, age: prevPerson.age + 1 }));
  setPerson((prevPerson) => ({ ...prevPerson, age: prevPerson.age + 1 }));
};
```

Khi a callback is passed to the `setState` hàm, it ensures that the latest state is passed in as an argument to the callback.

sử dụng an updater is không always necessary. Nếu you want to change the state sử dụng your previous state và you prefer consistency over verbosity, then you might consider sử dụng an updater.

<div class="lesson-note" markdown=1>

#### React batches state updates

ở đó are two `setPerson` calls in the bên trên ví dụ, và từ điều gì we've đã học so far, a `setState` call triggers a component re-render. So, the component nên re-render twice, right? You sẽ say yes, nhưng React is smart. Wherever possible, React batches the state updates. ở đây, the component chỉ re-renders Sau khi. We'd encourage you to sử dụng `console.log`s to verify this.

</div>

### Controlled các component

ở đó are native HTML các phần tử that maintain their own internal state. The `input` phần tử is a great ví dụ. You type vào an `input` và it updates its own giá trị on every keystroke. For many sử dụng-cases, you sẽ like to *control* the giá trị of the `input` phần tử, i.e., set its giá trị yourself. This is nơi controlled các component come in.

```jsx
function CustomInput() {
  const [value, setValue] = useState("");

  return (
    <input
      type="text"
      value={value}
      onChange={(event) => setValue(event.target.value)}
    />
  );
}
```

Thay vào đó of letting the `input` maintain its own state, we define our own state sử dụng the `useState` hook. We then set the `value` prop of the `input` to the state variable và update the state variable on every `onChange` event. Now, every time the user types something in the input, React sẽ ensure you have the latest comment, Ôn lại, hoặc post (whatever the user was typing) in `value`.

This pattern is extremely useful wherever you cần thêm control over user input, such as live validation hoặc anything else that may require you to access the latest state giá trị từ these inputs. Contrarily, yes, the `input` phần tử có thể be left uncontrolled và access its giá trị qua some khác phương thức. You don't cần to worry về it yet, as it sẽ be covered later on in the course. For bây giờ, control your các component!

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. đọc the following articles từ the React documentation:

   - [state as a Snapshot](https://react.dev/learn/state-as-a-snapshot)
   - [Choosing the state Structure](https://react.dev/learn/choosing-the-state-structure)
   - [Sharing state giữa các component](https://react.dev/learn/sharing-state-between-components)

1. Update the `Person` component we've been sử dụng bên trên.
   - Thêm two separate input fields for the đầu tiên name và the last name. Either of these nên be able to update the full name in the `h1` phần tử với every keystroke.
   - ở đó are many ways Bạn có thể do this. Keep in mind điều gì you've đã học Trong bài học này while coding it out.

</div>
