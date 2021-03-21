# What is NodeJS

Well, we know that JavaScript is a hosted language, which means that a JavaScript engine is needed to read and understand JavaScript.
Chrome as V8 engine that is capable of reading and understanding JavaScript. You can also work with other browser APIs. Well, this is great, but it's only possible only in the browser.
This is where NodeJs comes into play. It takes the JavaScript engine out of the browser and runs outside of the browser. Yes, I'm talking about the V8 engine. Now you have access to local files on filesystem since v8 JS engine is extracted and enriched by extra APIs.

## Getting Started

Make sure to have node installed on your local machine.
Let's create a JS file `file.js`

```javascript
const name = 'DK';
console.log(`Hello, ${name}`);
```

Then run
```bash
node app.js
```

will console log "Hello, DK" in the terminal.

Of course this is just a very basic example.
There are A LOT of Node API examples in the docs - **[DOC](https://nodejs.org/api/)**

### Example - working with files

##### [ file.js ]
```javascript
const fs = require('fs');

fs.readFile('user-data.txt', (err, data) => {
  if (err) {
    console.log('error: ', err);
    return;
  }
  console.log(data.toString());
});

fs.writeFile('user-data.txt', 'user-name=DK', (err) => {
  if (err) console.log('error: ', err);
  else console.log('result: ', 'wrote to file!');
});
```

We first need to import the package `fs` by running `require('fs')`. then we can create a txt file by running `fs.writeFile()` which takes 3 arguments - the name of the file you want to create, the data you want to have inside the file and a callback to handle jobs after. We can also read file like the code above.
Let's run `node file.js`, we will get

```bash
result:  wrote to file!
user-name=DK
```

### Example - working with Http Requests

You can listen or create a response to a server.
This spins up a server on the local that opens up a server.

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  response.write('Hello there! from the server');
  response.end();
});

server.listen(3000); // port 3000
```

If we open up http://localhost:3000, it will show `Hello there! from the server`.

