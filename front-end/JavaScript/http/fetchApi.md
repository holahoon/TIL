# Fetch API

#### Refer to the example in [Xml Http Request](./xmlHttpRequest.md) note.

> Btw, not all supports `XMLHttpRequest`, especially IE11 XD

This was built to be more modern and be less weird-ish...

The `fetch()` method is promise-based which means that we don't need to promisfy it.

```javascript
const URL = "https://jsonplaceholder.typicode.com/posts";

function sentHttpRequest(url) {
  return fetch(url).then((response) => {
    return response.json();
  });
}

async function fetchPosts2() {
  const responseData = await sentHttpRequest("GET", URL);

  for (const post of responseData) {
    const postEl = document.importNode(postTemplate.content, true);
    postEl.querySelector("h2").textContent = post.title;
    postEl.querySelector("p").textContent = post.body;
    postEl.querySelector("li").id = post.id;
    listElement.append(postEl);
  }
}
```

Note that `fetch` does not hold the response body that's ready to be used. Therefore, we need to access the response. The way is to call `response.json()` to convert from "screened unparsed response" body into a "snapshot parsed" body.
There are other methods like `response.blob()` for downloading a file or `response.text()` to return plain text.
Basically, the returned data is not JSON data, but it's a screened data.

Important to know that unlike the XMLHttpRequest, it works a bit differently.

### GET / POST / DELETE / PUT
```javascript
function sentHttpRequest(method, url, data) {
  return fetch(url, {
    method: method,
    body: JSON.stringify(data), // also, this can be binary data, JSON, form data...
    headers: {
      "Content-Type": "application/json", // my request has json data
    },
  }).then((response) => {
    return response.json();
  });
}
```

`method` and `body` - It accepts an object in second parameter with some key: value pairs. The `method` is the method in which you want to do - GET/POST/etc... The `body` is where you want to send your data, but in JSON format like above. Of course there are also other properties.

`headers` - There are times when working with some APIs, you need to describe which type of data you are sending. Or other APIs might need some extra authentication data.
Headers are basically metadata that you can attach to an outgoing request.
The example above is telling the API, "hey my request has json data".
This is *optional* if the API doesn't require it.

