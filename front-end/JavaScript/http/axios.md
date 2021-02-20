# Axios Library

reference: [Axios Github](https://github.com/axios/axios)

The third part library axios makes fetching API much easier.
Let's take a look at how we can use it.

Btw, in our case, we are using CDN (`<script src="https://unpkg.com/axios/dist/axios.min.js"></script>`) for this tutorial. Because this `script` tag was included in the HTML file, we don't need to import axios package into our JS file.
Also, axios supports Promise API.

## Example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
   (...)
    <script src="https://unpkg.com/axios/dist/axios.min.js" defer></script>
    <script src="assets/scripts/app.js" defer></script>
  </head>
  <body>
    <template id="single-post">
      <li class="post-item">
        <h2></h2>
        <p></p>
        <button>DELETE</button>
      </li>
    </template>
    <section id="new-post">
      <form>
        <div class="form-control">
          <label for="title">Title</label>
          <input type="text" id="title" name="title" />
        </div>
        <div class="form-control">
          <label for="content">Content</label>
          <textarea rows="3" id="content" name="body"></textarea>
        </div>
        <button type="submit">ADD</button>
      </form>
    </section>
    <section id="available-posts">
      <button>FETCH POSTS</button>
      <ul class="posts"></ul>
    </section>
  </body>
</html>
```

```javascript
const listElement = document.querySelector(".posts");
const postTemplate = document.getElementById("single-post");
const form = document.querySelector("#new-post form");
const fetchButton = document.querySelector("#available-posts button");
const postList = document.querySelector("ul");

const URL = "https://jsonplaceholder.typicode.com/posts";

async function fetchPosts() {
  try {
    const { status, data } = await axios.get(URL);

    if (status === 200) {
      const listOfPosts = data;
      for (const post of listOfPosts) {
        const postEl = document.importNode(postTemplate.content, true);
        postEl.querySelector("h2").textContent = post.title.toUpperCase();
        postEl.querySelector("p").textContent = post.body;
        postEl.querySelector("li").id = post.id;
        listElement.append(postEl);
      }
    }
  } catch (error) {
    console.log(error.response);
  }
}

async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title: title,
    body: content,
    userId: userId,
  };

  const formData = new FormData(form);
  // formData.append('title', title);
  // formData.append('body', content);
  formData.append("userId", userId);

  const response = await axios.post(URL, formData);

  console.log(response);
}

fetchButton.addEventListener("click", fetchPosts);
form.addEventListener("submit", (event) => {
  event.preventDefault();
  const enteredTitle = event.currentTarget.querySelector("#title").value;
  const enteredContent = event.currentTarget.querySelector("#content").value;

  createPost(enteredTitle, enteredContent);
});

postList.addEventListener("click", (event) => {
  if (event.target.tagName === "BUTTON") {
    const postId = event.target.closest("li").id;
    axios.delete(`${URL}/${postId}`);
  }
});

```

It's pretty straight forward. Call axios and define which method you'd like to use - get / post / delete ... This is not the only way to call such methods thought. Refer to their DOCS for further methods of using axios.