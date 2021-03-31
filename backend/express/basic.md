# Basic ExpressJS

#### [Express DOC](https://expressjs.com/)

Express is a NodeJS framework.
It's a middleware driven framework.

Let's initialize and create node_modules folder
```bash
$ npm init
```

Install express
```bash
$ npm install express --save
```

```javascript
const express = require('express');

const app = express();

// Add middlewares - order matters
app.use((req, res, next) => {
  res.setHeader('Content-Type', 'text/html');
  next(); // "we are not done yet, move on to the next line~"
});

app.use((req, res, next) => {
  res.send('<h1>Hello Dk!</h1>');
});

app.listen(3000);
```

Let's run
```bash
$ node app.js
```

Open up `http://localhost:3000/`

We'll notice that it successfully printed an h1 tag that has Hello DK!
Awesome! But, it's not really extracting any data.

#### Example - simple example with data extraction

```javascript
const express = require('express');
const bodyParser = require('body-parser'); // looks like it's deprecated

const app = express();

// parse request body and add it to the request object
app.use(
//   bodyParser.urlencoded({
//     extended: true,
//   })
  express.urlencoded({
    extended: true,
  })
);

// Add middlewares - order matters
app.use((req, res, next) => {
  res.setHeader('Content-Type', 'text/html');
  next(); // "we are not done yet, move on to the next line~"
});

app.use((req, res, next) => {
  const userName = req.body.username || 'Unkown User';
  res.send(
    `<h1>${userName}</h1><form method="POST" action="/"><input name="username" type="text" /><button type="submit">submit</button></form>`
  );
});

app.listen(3000);
```

The above code pretty much prints a form with an input. Once you input some value in the input field and submit, it will print the value that you have input.

I installed `body-parser` package to parse request body to add it to the request object.
```bash
$ npm install body-parser --save
```

However, it seems like `bodyParser` is deprecated. Looked up and the answer was to just use express
