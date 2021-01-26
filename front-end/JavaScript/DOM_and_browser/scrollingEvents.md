# Scrolling Events Tips

### Width & Height
`.innerWidth` and `.innerHeight` attribute returns the viewport width/height including the size of a rendered scroll bar (if any), or zero if there is no viewport.
[reference](https://www.w3.org/TR/cssom-view/#dom-window-innerwidth)

### ClientWidth & ClientHeight
- If the element has no associated CSS layout box or if the CSS layout box is inline, return zero
- If the element is the root element and the element’s node document is not in quirks mode, or if the element is the HTML body element and the element’s node document is in quirks mode, return the viewport width excluding the size of a rendered scroll bar (if any)
- Return the width of the padding edge excluding the width/height of any rendered scrollbar between the padding edge and the border edge, ignoring any transforms that apply to the element and its ancestors

### scrollTo()
I didn't know until now that you can pass in a behavior to the scroll event.
```javascript
$0.scrollTo({top: 150, behavior: 'smooth'})
// it scrolls 150px from the top, smoothly.
// $0 is for dev tools
```
