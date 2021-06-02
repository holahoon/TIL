# More on Story 1

## Story within story

Let's create a folder called `src/Subscription` and create a story called `Subscription.stories.js`.

```javascript
import { Primary } from '../Button/Button.stories';
import { Large } from '../Input/Input.stories';

export default {
  title: 'form/Subscription',
};

export const PrimarySubscription = () => (
  <>
    <Large />
    <Primary />
  </>
);
```

This will import those two `Primary` and `Large` stories and will see those two in the `Primary Subscription` path in the localhost.

One of the advantage of having a story within a story is that the changes that happens within the component auto-reflects on the story that's wrapping the components.

However one of the disavantage is using args.

## Using arguments

There are times when we just need to use the existing component without having to write more code.

```javascript
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

// create a template
const Template = (args) => <Button {...args} />;

export const PrimaryA = Template.bind({});
PrimaryA.args = {
  variant: 'primary',
  children: 'Primary Args',
};

export const LongPrimaryA = Template.bind({});
LongPrimaryA.args = {
  ...PrimaryA,
  children: 'Long Primary Args',
};

export const SecondaryA = Template.bind({});
SecondaryA.args = {
  variant: 'secondary',
  children: 'Secondary Args',
};
```

We first create a template which is a function that takes in `args`.
We bind the template with an object.
Then, assign its appropriate properties. That should create another `PrimaryA` button component with the same arguments.
We can also "extend" the existing template like above `LongPrimaryA`.

We can also set a default `args`

```javascript
// component story format
export default {
  title: 'form/control/Button', // mandatory and should be unique
  component: Button,
  args: {
    children: 'Button',
  },
};
```

This `args` will set as default and can be overwritten individually.

## Decorators

Decorators are components that wrap a story.
If we want to decorate the rendered components in the storybook, we can use decorators using CSS.

We first create a wrapper components

```javascript
// [ components/Center/Center.js ]
import './Center.css';

export default function Center(props) {
  return <div className='center'>{props.children}</div>;
}
```

```css
/* [ components/Center/Center.css ] */
.center {
  display: flex;
  justify-content: center;
}
```

Then we import that `Center` component and wrap it around the desired components.
In our case the `Button.stories.js`

```javascript
//...
export const Primary = () => (
  <Center>
    <Button variant='primary'>Primary</Button>
  </Center>
);
export const Secondary = () => (
  <Center>
    <Button variant='secondary'>Secondary</Button>
  </Center>
);
export const Success = () => (
  <Center>
    <Button variant='success'>Success</Button>
  </Center>
);
export const Danger = () => (
  <Center>
    <Button variant='danger'>Danger</Button>
  </Center>
);
```

Or we can just set a default,

```javascript
// component story format
export default {
  title: 'form/control/Button', // mandatory and should be unique
  component: Button,
  args: {
    children: 'Button',
  },
  decorators: [(story) => <Center>{story()}</Center>], // each el in the array is a function that auto receives the story as its argument
};
```

This `decorators` is an array of function that automatically receives the story as its argument.
Now, we can remove the `Center` wrapper from the `Button` component and it still works as we expect it to be.
