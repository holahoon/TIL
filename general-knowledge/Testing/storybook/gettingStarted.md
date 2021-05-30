# Getting started with Storybook with React

[reference](https://storybook.js.org/docs/react/get-started/install)

### Installation

```bash
$ npx sb init
```

### Run

Starts in development mode

```bash
$ npm run storybook
$ yarn storybook
```

The landing page will be [localhost:6006](http://localhost:6006/?path=/story/example-introduction--page) which opens up the `src/stories/introduction.stories.mdx` file.

### Start

As developers, it is our job to write stories each of UI components in our React application.

Let's go ahead and get started.
Go ahead and delete the `src/stories` folder and create a new folder path - `src/components/Button` and `src/components/Input`

In each directory, create `Button.js`, `Button.css` and `Button.stories.js` and same for Input with the name `Input`.

```jsx
// --- [ Button.js ] ---
import './Button.css';

export default function Button(props) {
  const { variant = 'primary', children, ...rest } = props;
  return (
    <div>
      <button className={`button ${variant}`} {...rest}>
        {children}
      </button>
    </div>
  );
}

// --- [ Input.js ] ---
import './Input.css';

export default function Input(props) {
  const { size = 'medium', ...rest } = props;

  return (
    <div>
      <input className={`input ${size}`} type='text' {...rest} />
    </div>
  );
}

```

```css
/* --- [ Button.css ] --- */
.button {
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border-radius: 4px;
  cursor: pointer;
}

.primary {
  background-color: #008cba;
}
.secondary {
  background-color: #e7e7e7;
  color: black;
}
.success {
  background-color: #4caf50;
}
.danger {
  background-color: #f44336;
}

/* --- [ Input.css ] --- */
.input {
  display: block;
  width: 400px;
  padding-left: 1rem;
  padding-right: 1rem;
  border-radius: 0.25rem;
  border: 1px solid inherit;
  background-color: white;
}

.small {
  height: 2rem;
  font-size: 0.875rem;
}
.medium {
  height: 2.5rem;
  font-size: 1rem;
}
.large {
  height: 3rem;
  font-size: 1.25rem;
}
```

```javascript
// --- [ Button.stories.js ] ---
import React from 'react';
import Button from './Button';

// component story format
export default {
  title: 'form/control/Button', // mandatory and should be unique
  component: Button,
};

export const Primary = () => <Button variant='primary'>Primary</Button>;
export const Secondary = () => <Button variant='secondary'>Secondary</Button>;
export const Success = () => <Button variant='success'>Success</Button>;
export const Danger = () => <Button variant='danger'>Danger</Button>;

// --- [ Input.stories.js ] ---
import Input from './Input';

export default {
  title: 'form/Input',
  component: Input,
};

export const Small = () => <Input size='small' placeholder='Small size' />;
export const Medium = () => <Input size='medium' placeholder='Medium size' />;
export const Large = () => <Input size='large' placeholder='Large size' />;

Small.storyName = 'Small Input';

```

Now, if we run `yarn story` or `npm run story`, it will open up a `localhost:6006` and you'll see on the left it having `FORM / control / Input`.

The `title: form/control/Button` pretty much creates a path.
Within that path, it'll have those `Primary`, `Secondary`, etc... components rendered.

One thing to keep in mind is that it will create the components in the order it is called. If in need of sorting them alphabetically, visit the [doc](https://storybook.js.org/docs/react/writing-stories/naming-components-and-hierarchy) and where it says "Sorting stories", just copy/paste it into the `storybook/preview.js` file
