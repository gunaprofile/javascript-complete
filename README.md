# Javascript Complete

## Working with JavaScript Libraries

### Adding Libraries (Example: lodash)

* Example : https://lodash.com/docs/4.17.15

* Now to use Lodash, we have to add it to our project though, we can't just call this code here.  to have lodash define it, we need to add lodash to our project.

* Refer : https://jquery.com/

### Discovering Libraries

* Refer : https://www.npmjs.com/

### Axios

* Refer : https://www.npmjs.com/package/axios#features

```js
const listElement = document.querySelector('.posts');
const postTemplate = document.getElementById('single-post');
const form = document.querySelector('#new-post form');
const fetchButton = document.querySelector('#available-posts button');
const postList = document.querySelector('ul');

function sendHttpRequest(method, url, data) {
  // const promise = new Promise((resolve, reject) => {
  // const xhr = new XMLHttpRequest();
  // xhr.setRequestHeader('Content-Type', 'application/json');

  //   xhr.open(method, url);

  //   xhr.responseType = 'json';

  //   xhr.onload = function() {
  //     if (xhr.status >= 200 && xhr.status < 300) {
  //       resolve(xhr.response);
  //     } else {
  // xhr.response;
  //       reject(new Error('Something went wrong!'));
  //     }
  //     // const listOfPosts = JSON.parse(xhr.response);
  //   };

  //   xhr.onerror = function() {
  //     reject(new Error('Failed to send request!'));
  //   };

  //   xhr.send(JSON.stringify(data));
  // });

  // return promise;
  return fetch(url, {
    method: method,
    // body: JSON.stringify(data),
    body: data
    // headers: {
    //   'Content-Type': 'application/json'
    // }
  })
    .then(response => {
      if (response.status >= 200 && response.status < 300) {
        return response.json();
      } else {
        return response.json().then(errData => {
          console.log(errData);
          throw new Error('Something went wrong - server-side.');
        });
      }
    })
    .catch(error => {
      console.log(error);
      throw new Error('Something went wrong!');
    });
}

async function fetchPosts() {
  try {
    const response = await axios.get(
      'https://jsonplaceholder.typicode.com/posts'
    );
    const listOfPosts = response.data;
    for (const post of listOfPosts) {
      const postEl = document.importNode(postTemplate.content, true);
      postEl.querySelector('h2').textContent = post.title.toUpperCase();
      postEl.querySelector('p').textContent = post.body;
      postEl.querySelector('li').id = post.id;
      listElement.append(postEl);
    }
  } catch (error) {
    alert(error.message);
    console.log(error.response);
  }
}

async function createPost(title, content) {
  const userId = Math.random();
  const post = {
    title: title,
    body: content,
    userId: userId
  };

  const fd = new FormData(form);
  // fd.append('title', title);
  // fd.append('body', content);
  fd.append('userId', userId);

  const response = await axios.post(
    'https://jsonplaceholder.typicode.com/posts',
    fd
  );
  console.log(response);
}

fetchButton.addEventListener('click', fetchPosts);
form.addEventListener('submit', event => {
  event.preventDefault();
  const enteredTitle = event.currentTarget.querySelector('#title').value;
  const enteredContent = event.currentTarget.querySelector('#content').value;

  createPost(enteredTitle, enteredContent);
});

postList.addEventListener('click', event => {
  if (event.target.tagName === 'BUTTON') {
    const postId = event.target.closest('li').id;
    axios.delete(`https://jsonplaceholder.typicode.com/posts/${postId}`);
  }
});

```
### Third-Party Library Considerations

* a couple of things you should keep in mind when working with libraries, for one keep in mind that for example Lodash here, if you add it like this, includes a lot of functionality which you never use.

* We only use the difference method here and yet we still import all the other features as well, that means that users of our application will download all that code even if we just use one feature. We can see the effect if we reload our page, you see the Lodash package here has 24kb. Now obviously that's not that big but it's way bigger than the rest of our application, so that can be a downside, you're downloading way more than you might need and that could slow down the initial loading time of your application which matters a lot if your application primarily targets people in areas of the world with poor internet connection.

* So that's one thing to keep in mind, you might include a lot of overhead. That being said however, it is worth pointing out that many libraries allow you to import them in different ways, for example Lodash gives you a core build which only comes with some of the features, which is way smaller or in more advanced project setups which we'll also cover later in the course, you even have other capabilities of basically just importing parts of a library and then stripping out the parts which you don't need.

* So you can mitigate this problem but still, it is something to keep in mind, also because not all libraries allow you to be as selective as I just described.

* In addition to that, you also want to make sure you are working with libraries which are secure and well maintained, let's use axios here.

* So having open source is generally good but you want to make sure that the library you're using is also actively maintained, so that if there are bugs, bugs that break your application or security issues which of course also are very important, that they are fixed in time.

* Now an indicator for that for example is to view the last commit, this basically means when the code was edited the last time.

* If this is two years old, then it looks like it's not actively maintained anymore, two days, a week, a month, that is pretty active.You can also check the releases page on Github to find out how often new versions are released and if that is fairly regular, let's say once a year, twice a year, then this also seems to be actively maintained,

