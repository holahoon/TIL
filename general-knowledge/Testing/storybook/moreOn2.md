# More on Storybook

## Addons

When looking at storybook localhost, there's a section on the bottom where you can dynamically change/configure the properties of the component.
The `Controls` button allows you to dynamically control the component.
This can be done using `argTypes` passing into the default configuration.

```javascript
// [ Button.stories.js ]
(...)
export default {
  title: 'form/control/Button', // mandatory and should be unique
  component: Button,
  // args: {
  //   children: 'Button',
  // },
  // decorators: [(story) => <Center>{story()}</Center>], // each el in the array is a function that auto receives the story as its argument
  argTypes: {
    variant: { options: ['primary', 'secondary', 'success', 'danger'], control: 'radio' },
    children: { control: 'text' },
  },
};


export const Primary = () => <Button variant='primary'>Jinho is babo</Button>;
export const Secondary = () => <Button variant='secondary'>Secondary</Button>;
export const Success = () => <Button variant='success'>Success</Button>;
export const Danger = () => <Button variant='danger'>Danger</Button>;

// args
const Template = (args) => <Button {...args} />;

export const PrimaryA = Template.bind({});
PrimaryA.args = {
  variant: 'primary',
  children: 'Primary Args',
  onClick: { action: 'clicked' },
};

export const LongPrimaryA = Template.bind({});
LongPrimaryA.args = {
  ...PrimaryA,
  // children: 'Long Primary Args',
};

export const SecondaryA = Template.bind({});
SecondaryA.args = {
  variant: 'secondary',
  // children: 'Secondary Args',
};

// Addons - with actions
export const ActionPrimaryA = Template.bind({});
ActionPrimaryA.args = {
  variant: 'primary',
  children: 'Action Primary A',
};
```

If we check the `Button` component, we can now use radio button to toggle between the variants and change the `children` text.
Make sure that the name `variant` in the exported template matches the `argTypes`.

The `onClick` event can be checked in the `Actions` panel.

## Console Addon

We first need to install storybook console addon

```bash
$ yarn add -D @storybook/addon-console
```

Then, import `@storybook/addon-console` in the storybook `preview.js` file.

```javascript
// [ .storybook/preview.js ]
import '@storybook/addon-console';
```

Let's go to `Input.stories.js` file

```javascript
// [ Input.stories.js ]
import Input from './Input';

export default {
  title: 'form/Input',
  component: Input,
};
(...)

// console addon
export const Log = () => (
  <Input
    size='medium'
    placeholder='Medium Sized input'
    onChange={(e) => console.log(e.target.value)}
  />
);
```

As we type anything in the input, it will console log the value.

If we want to see where the component is from, we need to configure `preview.js` file again.

```javascript
// [ .storybook/preview.js ]
import { addDecorator } from '@storybook/react';
import { withConsole } from '@storybook/addon-console';

export const parameters = {
  actions: { argTypesRegex: '^on[A-Z].*' },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
  options: {
    storySort: (a, b) =>
      a[1].kind === b[1].kind ? 0 : a[1].id.localeCompare(b[1].id, undefined, { numeric: true }),
  },
};

addDecorator((storyFn, context) => withConsole()(storyFn)(context)); // <--
```

If we now type values in the input, we can see that the console was logged from `form/Input/Log: "asdfas"`.
