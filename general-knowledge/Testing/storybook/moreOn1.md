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

##
