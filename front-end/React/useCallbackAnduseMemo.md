# React.useCallback vs React.useMemo

#### [reference](https://kentcdodds.com/blog/usememo-and-usecallback)

```jsx
function CandyDispenser() {
  const initialCandies = ['snickers', 'skittles', 'twix', 'milky way']
  const [candies, setCandies] = React.useState(initialCandies)

  const dispense = candy => {
    setCandies(allCandies => allCandies.filter(c => c !== candy))
  }
  return (
    <div>
      <h1>Candy Dispenser</h1>
      <div>
        <div>Available Candy</div>
        {candies.length === 0 ? (
          <button onClick={() => setCandies(initialCandies)}>refill</button>
        ) : (
          <ul>
            {candies.map(candy => (
              <li key={candy}>
                <button onClick={() => dispense(candy)}>grab</button> {candy}
              </li>
            ))}
          </ul>
        )}
      </div>
    </div>
  )
}
```
Above is a simple example that takes iterates through `candies` state which is `['snickers', 'skittles', 'twix', 'milky way']` and filters out using `dispense()` a `candy` that matches the selected candy.

We are now going to wrap the `dispense()` with `React.useCallback` hook as below,
```jsx
const dispense = React.useCallback((candy) => {
   setCandies((allCandies) => allCandies.filter((c) => c !== candy))
}, [])
```

This useCallback allows us to prevent from recreating the `dispense()` function whenever the component rerenders after state updates.
**This may sound like it's a better performance, but it really is NOT**

