# Understanding Getters and Setters

아주 조금 배워봤다. 예제를 보자.

```javascript
const inputTitle = document.getElementById("title").value; // input value

const newMovie = {
    info: {
        // setter
        set title(val) {
            if (val.trim() === '') {
                this._title = 'DEFAULT TITLE';
                return; // exit out
            }
            this._title = val;
        }
        // getter
        get title() {
            return this._title;
        }
    },
    getFormattedTitle() {
        return this.info.title.toUpperCase();
    }
}

newMovie.info.title = inputTitle; // setter runs
const getTitle = newMovie.info.title; // getter runs
```

`setter` sets a value on `_title` (underscore is just an indicator that it's an inner title reference(?)).
`getter` gets the value that lives on `_title`.
the above `val.trim() ...` validation just checks to see if the `val` is empty which will set the `_title` as `'DEFAULT TITLE'`;

> Keep in mind that you CANNOT use getter without setter.