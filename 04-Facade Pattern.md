# Facade Pattern

The entire idea behind the Facade Pattern is to **change code easier in the future**. You create a **Facade between your complex code** **and** your actual **business logic code**.

## Example

A lot of code that is not directly tied to getting a user.

```
function getUsers(){
  return fetch('https://jsonplaceholder.typicode.com/users', {
    method: "GET",
    headers: {"Content-Type": "application/json"}
  }).then(res => res.json())
}

function getUserPosts(userId){
  return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`, {
    method: "GET",
    headers: {"Content-Type": "application/json"}
  }).then(res => res.json())
}

getUsers().then(users => {
  users.forEach(user => {
    getUserPosts(user.id).then(posts => {
      console.log(user.name)
      console.log(posts.length)
    })
  })
})
```

### Refactor to use a Facade

We will create a function that we only pass the information that changes, such as the URL. It cleans up all the duplicate code.


> Creating the query String using Object.entries to convert key value pairs into arrays
```
{ userId: 1 } => [[userId,1]]
```

```
function getFetch(url, params = {}){
  const queryString = Object.entries(params).map(param => {
    return `${param[0]}=${param[1]}`
  }).join('&')
  return fetch(`${url}?${queryString}`, {
    method: "GET",
    headers: {"Content-Type": "application/json"}
  }).then(res => res.json())
}

function getUsers(){
  return getFetch('https://jsonplaceholder.typicode.com/users')
}


function getUserPosts(userId){
  return getFetch('https://jsonplaceholder.typicode.com/posts', {
    userId: userId
  })
}


getUsers().then(users => {
  users.forEach(user => {
    getUserPosts(user.id).then(posts => {
      console.log(user.name)
      console.log(posts.length)
    })
  })
})
```

## Using Axios

One of the really big advantages is when you want to change the implementation.

```
function getFetch(url, params = {}){
  return axios({
    url: url,
    method: "GET",
    params: params
  }).then(res => res.json())
}
```

We still get all the results, but now we actually using axios instead of fetch.