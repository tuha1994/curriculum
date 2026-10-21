### Giới thiệu

All the các component so far have been functional in style và syntax. This is common bây giờ, nhưng you sẽ see a khác `class` based syntax too. Trong bài học này, we explore cách a class-based component is written và cách concepts like props và state are được sử dụng in one.

### Tổng quan bài học

Phần này chứa a general tổng quan of topics that you sẽ tìm hiểu Trong bài học này.

- How class các component are written.
- How to sử dụng props và state in class các component.
- Uses of the `this` keyword in class các component.

### Historical React component patterns

In your previous lessons, you have already been introduced to functional các component, và the cơ bản patterns in which các component get written nowadays. Tuy nhiên, React các component did không look this way Khi React was introduced.

Nếu you look vào any older React codebase, you'll notice a lot of classes. These are known as component dựa trên class. Prior to February 2019, functional các component were cũng được gọi là state-ít hơn, as ở đó was no way to manage state in them. This was changed Khi hooks were introduced, leading to ít hơn verbose và 'neater' các component.

In your career, chances are, you sẽ be dealing với legacy mã, so ở đó sẽ be days nơi you sẽ be dealing với class các component. Hãy  peek vào the intricacies of a class-based component, và cách they work.

### Building a class component

As we already know về functional các component, let us Xây dựng a class-based component từ a functional one. Usually, you sẽ want to divide the contents of a component, like the one we sử dụng, vào smaller, reusable các component, nhưng for the purposes of this exercise, chúng ta sẽ stick to everything in one component.

Firstly, go và fork then clone our [react-các ví dụ repo](https://github.com/TheOdinProject/react-examples) if you have không already done so. `cd` vào the `class-components/` directory, install dependencies với `npm install` then open the dev server với `npm run dev`. You'll see we have two versions of the giống nhau feature - one made với a functional component, the khác với a class component. Hãy  start by looking at the functional component bên trong `src/components/FunctionalInput.jsx`. It's a solid chunk of mã so take a while, sip some water, và đọc it a couple of times.

#### The start of a class-based component

You'll see in `src/components/ClassInput.jsx` that we have a completed class-based version of it. Hãy  take a few steps back và walk qua step by step cách it was recreated từ the functional version. The đầu tiên thing it nên have is, *drumroll*, a class! But it không thể be just another class, it sẽ cần to have certain properties that qualifies it as a React component. React provides us với tất cả those properties on a class được gọi là `Component`, và chúng ta có thể viết our các component by extending the given class, as shown below:

```jsx
import { Component } from "react";

class ClassInput extends Component {
  // Some code goes here
}

export default ClassInput;
```

#### The sử dụng of a constructor và props

A class is generally incomplete không có a constructor, so Hãy  add one.

The props passed vào this component are passed to the class's `constructor`. This, along với the `super` phương thức, cho phép you to sử dụng the props in the context of `this`, which, in *this* case, refers to the component. Nếu you’re really curious về điều gì `super` actually does, Kiểm tra out the [MDN docs on the `super` keyword](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super).

Nếu your component doesn't have any props, it is fine to leave the `constructor` và the `super` với no arguments.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);
  }
  // Some more code goes here
}

export default ClassInput;
```

#### How Bạn có thể render JSX

Now that the props có thể be accessed bên trong of the class component, the tiếp theo issue is to find a way to render the JSX.

Well, Bạn có thể do that by returning your JSX từ a `render` phương thức! Bạn có thể sử dụng the props that you declared in the constructor too!

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);
  }
  // Some more code goes here

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
       {/* The input field to enter Todos */}
        <form>
          <label htmlFor="task-entry">Enter a task: </label>
          <input type="text" id="task-entry" name="task-entry" />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        {/* The list of all the Todos, displayed */}
        <ul></ul>
      </section>
    );
  }
}

export default ClassInput;
```

Notice cách the props get provided by `this`, unlike the functional component that we saw, initially?

#### How to sử dụng state và manage context

tiếp theo comes the state. In a class-based component, the state gets initialized as a phần of the constructor.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [],
      inputVal: "",
    };
  }
  // Some more code goes here

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
        <form>
          <label htmlFor="task-entry">Enter a task: </label>
          <input type="text" id="task-entry" name="task-entry" />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        <ul></ul>
      </section>
    );
  }
}

export default ClassInput;
```

The pre-defined `setState` phương thức có thể be được sử dụng to set it again! Remember, state phải không be mutated, so a mới state phải be set, every time.

Now, it is time to finish it off by adding tất cả the functionality! It is nearly the giống nhau, except for a single difference. Whenever a phương thức is declared, you phải `bind` the `this` of the phương thức to that of the class in order to work với it, as by default, the các phương thức in a class are không bound to it. Usually, you do this bên trong the constructor và không at runtime [in the render phương thức]. Alternatively, if you define your các phương thức sử dụng the arrow hàm syntax, `this` is automatically bound to the class instance, và Bạn có thể skip binding it in the constructor.

```jsx
import { Component } from "react";

class ClassInput extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [],
      inputVal: "",
    };

    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleInputChange(e) {
    this.setState((state) => ({
      ...state,
      inputVal: e.target.value,
    }));
  }

  handleSubmit(e) {
    e.preventDefault();
    this.setState((state) => ({
      todos: state.todos.concat(state.inputVal),
      inputVal: "",
    }));
  }

  render() {
    return (
      <section>
        <h3>{this.props.name}</h3>
        <form onSubmit={this.handleSubmit}>
          <label htmlFor="task-entry">Enter a task: </label>
          <input
            type="text"
            id="task-entry"
            name="task-entry"
            value={this.state.inputVal}
            onChange={this.handleInputChange}
          />
          <button type="submit">Submit</button>
        </form>
        <h4>All the tasks!</h4>
        <ul>
          {this.state.todos.map((todo) => (
            <li key={todo}>{todo}</li>
          ))}
        </ul>
      </section>
    );
  }
}

export default ClassInput;
```

And ở đó we go, we have successfully made a class-based component as easy as that!

### Bài tập

<div class="lesson-content__panel" markdown="1">

Since we have a completed class-based component, Hãy  add additional functionality to it.

1. Implement a delete button for mỗi task. The delete button nên Xóa that specific task từ the state array, thus deleting the task itself! Styling isn't a priority at this moment, nhưng the button tag nên be styled by default.
1. Implement a mới class component, `Count`, that displays the number of todos at any given time. render it somewhere within the `ClassInput` component.
1. Implement an edit button for mỗi task. It nên replace the todo với an input field, và change the button itself to 'Resubmit', so the edits có thể be saved. This is a comparatively harder task, kudos for Khi you finish it!

</div>
