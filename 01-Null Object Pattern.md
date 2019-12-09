# Null Object Pattern

Easiest to understand and implement

## Definition

Anytime you have a **null object being returned** and then you access a method on that null object, you are going to get a null error thrown. So you have to check if something is equal to null or not before you proceed.

**Idea**: You **create another object** that you return instead of null that has the exact same signature (same properties and methods) as the object you already being returning from that method. It **will have default values** for all these properties and methods.


## Use Case
User accounts are a common case as many websites have an option to sign in to an account. If you don't and you are a guest, you have certain permission levels where you can only access certain things.

```
class User{
  constructor(id, name){
    this.id = id
    this.name = name
  }

  hasAccess(){
    return this.name === 'Bob'
  }
}

const users = [
  new User(1, 'Bob'),
  new User(2, 'John')
]

function getUser(id){
  return users.find(user => user.id === id)
}

function printUser(id){
  const user = getUser(id)
  
  /* 
    We need to explicitly tell the console.log to print 'Guest' if the user does not have a name.

    This is problematic since we need to remember to always put this every time we use the user's name.

    It is also bad because if we want to print 'Unknown User' instead, we would need to change every place that we put 'Guest' which will most likely be all over the application.
  */

  let name = 'Guest'
  if(user !=null && user.name !=null) name = user.name
  console.log('Hello ' + name)

  /* 
    This will throw an error if we don't first check that the user object has this function available and isn't null.

    This is a lot of extra code to add in every time we want to check user access and could cause bugs that are easy to miss if we forget to do the null checks.
  */

  if(user !=null && user.hasAccess !=null && user.hasAccess()){
    console.log('You have access')
  }else{
    console.log('You are not allowed here')
  }
  
}
```

## Refactor Code 
Remove null checks by using the Null Object Pattern. Return a null object from getUser function instead of just returning null (for getUser(3))


* Create Null Object Class

  * common thing to do is to prefix then name of the class with Null

* Don't pass arguments in NullUser's constructor, but use default properties

```
class User{
  constructor(id, name){
    this.id = id
    this.name = name
  }

  hasAccess(){
    return this.name === 'Bob'
  }
}

class NullUser{
  constructor(){
    this.id = -1
    this.name = 'Guest'
  }

  hasAccess(){
    return false
  }
}

const users = [
  new User(1, 'Bob'),
  new User(2, 'John')
]

// getUser function takes care of the null check itself
function getUser(id){
  const user = users.find(user => user.id === id)
  if (user == null){
    return new NullUser()
  }else{
    return User
  }
}

function printUser(id){
  const user = getUser(id)

  console.log('Hello ' + user.name)

  if(user.hasAccess()){
    console.log('You have access')
  }else{
    console.log('You are not allowed here')
  }
}

```
