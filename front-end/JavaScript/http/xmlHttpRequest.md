# XML Http Request

##### Let's practice using [jsonplaceholder API](https://jsonplaceholder.typicode.com/)

> Btw, All browser supports `XMLHttpRequest`

### Sample

```html
 <template id='single-post'>
    <li class="post-item">
        <h2></h2>
        <p></p>
        <button>DELETE</button>
    </li>
</template>

<ul class="posts"></ul>
```

```javascript
const listElement = document.querySelector(".posts");
const postTemplate = document.getElementById("single-post");

const xhr = new XMLHttpRequest();

xhr.open("GET", "https://jsonplaceholder.typicode.com/posts"); // 1st - http method, 2nd - url to be sent

xhr.responseType = "json"; // gets the reponse data in JSON type and converts into obj

xhr.onload = function () {
  const listOfPostsInJSON = JSON.parse(xhr.response); // response data in JSON format and parses to a JS object
  const listOfPosts = xhr.response; // with xhr.responseType = 'json' - don't need to manually convert JSON into JS object

  for (const post of listOfPosts) {
    const postEl = document.importNode(postTemplate.content, true); // clones the node of postTemplate and clones it deep(clones the descendents)
    postEl.querySelector("h2").textContent = post.title;
    postEl.querySelector("p").textContent = post.body;

    listElement.append(postEl);
  }
};

xhr.send(); // sends the request
```

`xhr.open()` receives two arguments: 1 - the http method, 2 - the url to be sent.
We could we `addEventListener` on load event, but not all browsers support using `xhr.addEvn...`, so it's better off using `xhr.onload` and assign a function to it.
Also, note that `listOfPostsInJSON` is parsed into a JS object since we get the data in JSON format. Well, keep in mind that JSON(JavaScript Object Notation) is just a string that contains data in the format of "object". If we don't want to manually convert JSON into an object, we can just use `xhr.responseType = "json"` and that gets the response data in JSON type and converts into an object.

### GET

Let's first promisify Http requests.

```javascript
const listElement = document.querySelector(".posts");
const postTemplate = document.getElementById("single-post");

const URL = "https://jsonplaceholder.typicode.com/posts";

function sentHttpRequest(method, url) {
  const promise = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open(method, url);

    xhr.responseType = "json";

    xhr.onload = function () {
      resolve(xhr.response);
    };

    xhr.send();
  });

  return promise;
}

// Using Promise
function fetchPosts() {
  sentHttpRequest("GET", URL).then((response) => {
    const listOfPosts = response;

    for (const post of listOfPosts) {
      const postEl = document.importNode(postTemplate.content, true);
      postEl.querySelector("h2").textContent = post.title;
      postEl.querySelector("p").textContent = post.body;
      listElement.append(postEl);
    }
  });
}

// Using Async/Await
async function fetchPosts2() {
  const responseData = await sentHttpRequest("GET", URL);

  for (const post of responseData) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector("h2").textContent = post.title;
    postEl.querySelector("p").textContent = post.body;
    listElement.append(postEl);
  }
}
```

By doing the above, it'll put the acquired data into `<h2>` and `<p>` that lives inside `<li>` which will then get appended to the `<ul>`.

### Post

```javascript
const listElement = document.querySelector(".posts");
const postTemplate = document.getElementById("single-post");

const URL = "https://jsonplaceholder.typicode.com/posts";

function sentHttpRequest(method, url, data) { // accepts 3rd arg: data
  const promise = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open(method, url);

    xhr.responseType = "json";

    xhr.onload = function () {
      resolve(xhr.response);
    };

    xhr.send(JSON.stringify(data)); // needs to be JSON format to send the data to the server
  });

  return promise;
}

async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title,
    body: content,
    userId,
  };

  sentHttpRequest("POST", URL, post);
}

fetchPosts2();
createPost("DUMMY", "This is really cool!");
```

Keep in mind that it's just `jsonplaceholder API` specific that we pass in the `const post = {...}` as it varies on how the server wants to get the data. But, we typically post data in JSON format so we need to convert the object into JSON by using `JSON.stringify()`. 

### Delete

When clicking on the delete button, it should be able to reach in and get the desired element.

```javascript
const postList = document.querySelector("ul"); // gets the ul tag

async function fetchPosts2() {
  const responseData = await sentHttpRequest("GET", URL);

  for (const post of responseData) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector("h2").textContent = post.title;
    postEl.querySelector("p").textContent = post.body;
    postEl.querySelector("li").id = post.id; // <- creates an id attribute and adds the id data to it
    listElement.append(postEl);
  }
}

postList.addEventListener("click", (event) => {
  // event delegation - only targets the button clicked on <ul>
  if (event.target.tagName === "BUTTON") {
    const postId = event.target.closest("li").id; // id that lives on <li>
    sentHttpRequest("DELETE", `${URL}/${postId}`);
  }
});
```

Will delete the desired post by passing in the id.

### Error Handling

There may be times when we would need to handle error. Cases could vary from internet being down to server being dead.

```javascript
const URL = "https://jsonplaceholder.typicode.com/blahblahblah";

(...)
xhr.onload = function () {
    resolve(xhr.response);
};

xhr.onerror = function () {
    console.log(xhr.response);
    console.log(xhr.status);
};
(...)
```

Keep in mind that the `xhr.onerror` will handle when there's an issue from the server, won't really catch if we pass in an invalid url like above. This is due to how the server sends back a status code which can be from 200 ~ 404 ish. This is pretty much handled in `xhr.onload`. If we want to test the error handling on the front side, we can do the following.

```javascript
function sentHttpRequest(method, url, data) {
    (...)
    xhr.onload = function () {
        if (xhr.status >= 200 && xhr.status < 300) {
        // success
        resolve(xhr.response);
        } else {
        // fail
        reject(new Error("Something went wrong!"));
        }
    };

    // only works when error from the server
    xhr.onerror = function () {
        reject(new Error("Failed to send request!"));
    };
    (...)
}

async function fetchPosts2() {
  try {
    const responseData = await sentHttpRequest("GET", URL);
    for (const post of responseData) {
      const postEl = document.importNode(postTemplate.content, true);
      postEl.querySelector("h2").textContent = post.title;
      postEl.querySelector("p").textContent = post.body;
      postEl.querySelector("li").id = post.id;
      listElement.append(postEl);
    }
  } catch (error) {
    alert(error);
  }
}
```

We check for the status code within `onload` and see if it is successful by resolving or reject if something happens. This is where non-success status code gets handled.
We can also wrap our business logic in `try()catch()` and catch error.

### Note,

If we need to pass in a header when sending a request to the API in scenarios where the API requires it,

```javascript
xhr.setRequestHeader("Content-Type", "application/json");
```

You can add multiople headers by calling it multiple times. Just keep in mind that once the header is added, you can't delete it.