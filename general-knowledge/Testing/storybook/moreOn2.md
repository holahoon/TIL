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
