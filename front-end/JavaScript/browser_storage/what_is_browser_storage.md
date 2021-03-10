# Browser Storage

In times when we need to store some data, we use the storage that the browser supports. There are different types of storages in browsers: localStorage, sessionStorage, cookies and indexDB.
They store temporary "convenience data".
Of course if we need to store essential & persistent data, we use server-side database.
We can only access the data when the user visits the page by using JS.

## Differences between all the storages

- `localStorage` and `sessionStorage`: a simple key-value store. Almost like an object in JS. We often use `localStorage` to manage user preferences or basic user-data. Also, you can only access via JS. Not the place for storing complex data though.

- `Cookies`: a simple key-value store with some config options. It's also used to manage user preferences or basic user-data. Can be cleared by the user and via Js as well. This is versatile just like `localStorage` or `sessionStorage`, but it is sent to server without going HTTP requests, which means `cookies` can be read by the server. It is also bad for complex data.

- `IndexDB`: is relatively sophisticated, client-side database. It's good for managing complex data. It can be cleared by the user and via JS. It's a bit clunky to use.

So, never rely on these storages since users can always clear and erase all the data. Use it to improve the user experience.

## localStorage

accessing localStorage is by calling it,
```javascript
localStorage.setItem(); // sets item in the storage
localStorage.getItem(); // gets item from the stroage
```
localStorage belongs to the `window` object, but we can omit `window` object key.
There are more things you can do with the `localStorage` object.

Keep in mind that setting data in the `localStorage`, it only accepts in `string` type.
If we have like an object that needs to be stored in the `localStorage`, we use `JSON.stringify()`. Use `JSON.parse()` method to convert from JSON to what we can read.

```javascript
const storeBtn = document.getElementById('store-btn');
const retBtn = document.getElementById('retrieve-btn');

const userInfo = {
  name: 'David',
  age: 31,
  occupation: 'web dev',
};

storeBtn.addEventListener('click', () => {
  localStorage.setItem('user', JSON.stringify(userInfo));
});

retBtn.addEventListener('click', () => {
  const userInfo = JSON.parse(localStorage.getItem('user'));
  if (userInfo) {
    console.log('user: ', userInfo);
  }
});
```
Data inside `localStorage` is NEVER cleared unless it's cleared by the user or via JS.

## sessionStorage

```javascript
const storeBtn = document.getElementById('store-btn');
const retBtn = document.getElementById('retrieve-btn');

const userInfo = {
  name: 'David',
  age: 31,
  occupation: 'web dev',
};

storeBtn.addEventListener('click', () => {
  sessionStorage.setItem('user', JSON.stringify(userInfo));
});

retBtn.addEventListener('click', () => {
  const userInfo = JSON.parse(sessionStorage.getItem('user'));
  if (userInfo) {
    console.log('user: ', userInfo);
  }
});
```

We can go back to our dev tools and open to `Application`. Visit the sessionStorage tab and click on the storeBtn. We can see that the data was successfully stored in the `sessionStorage`.
However, if we close the browser and open up the same page again, it's gone unlike the `localStorage`. That's the difference.
Data inside `sessionStorage` lives as long as the page is opened in the browser even if you reload the page.

## cookies

Cookies get attached to an outgoing `http` requests. The server needs to be "prepared" to do something with the cookies. It's not really a problem, but doesn't really benefit if not prepared.
Cookies are only available when the webpage is being served with real server.

#### Let's get started

```javascript
const userId = 'hmm';
document.cookie = `userId=${userId}`;
console.log(document.cookie); // userId=hmm

const myName = 'DK';
document.cookies = `name=${myName}`;
console.log(document.cookie); // userId=hmm, name=DK
```

When setting cookies, it will not override the existing cookies. Only when you assign a new key.

#### Working with cookies

When getting an item from cookies, we get the whole stored cookies.
Let's take a look at an example.

```javascript
storeCookieBtn.addEventListener('click', () => {
  const userId = 'user123';
  const user = {
    name: 'DK',
    age: 30,
  };
  document.cookie = `userId=${userId}`;
  document.cookie = `user=${JSON.stringify(user)}`;
});

retCookieBtn.addEventListener('click', () => {
  const cookieData = document.cookie.split(';');
  const data = cookieData.map((cookie) => {
    return cookie.trim();
  });
  console.log(data); // ["userId=user123", "user={"name":"DK","age":30}"]
  console.log(data[1].split('=')[1]); // {"name":"DK","age":30}
});
```

Keep in mind that it's not really a good idea to access the values by using index, but rather it's better to use methods like `includes()`.

Notice how we use `split()` method to split by `;`. When we get cookies, it returns the whole stored cookies. Then we use `trim()` to trim out the white spaces in front of each seperated strings in an array.
Well, it's super clunky to work with cookies, but what are the advantages of using it?
You can set them to expire or you can also send them to a server with a request.

If you don't set an expiration to a cookie, it expires when the session expires (when you close the entire browser). Well, it's really up to the browser to delete when it wants to pretty much. The user also can delete the cookies too.

Let's set up the expiration date.

```javascript
(...)
document.cookie = `userId=${userId}; max-age=4`;
(...)
```

Put a `;` at the end of the key-value and add `max-age=4` where it pretty much stands for maximum age will be 4 seconds.
If we do so, it will expire after 4 seconds.
An alternative will be to use `expires=` which is useful when have a set date.

Just like I mentioned above, it's not really a good idea to access cookies by index. When a previous cookie expires and a new one is created, the order of the cookies change. Therefore, it's better to use method like `includes()` to see if such value is included in that array.