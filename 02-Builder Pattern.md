# Builder Pattern

Useful when you need to create **objects** that have many interlinking parts or many **optional and required fields**.


## Example Problem

A User whose only the name is required. The age, phone, and address are optional fields.

```
class Address{
  constructor(zip, street){
    this.zip = zip
    this.street = street
  }
}

class User {
  constructor(name, age, phone, address){
    this.name = name
    this.age = age
    this.phone = phone
    this.address = address
  }
}

const user = new User('Bob')
console.log(user)
/*
User {
  name: "Bob",
  age: undefined,
  phone: undefined,
  address: undefined
}
*/
```

But what if we want to **add an address** but **not a phone number**:

```
const user = new User('Bob', undefined, undefined, new Address('1', 'Main'))
```

This is kind of complex and not self-explanatory (we don't know what undefined is).

## Builder

### Traditional way

Create **methods** so we can **set the optional fields**.

> Returning the builder back will allow us to chain methods together

```
class Address{
  constructor(zip, street){
    this.zip = zip
    this.street = street
  }
}

class User {
  constructor(name){
    this.name = name
  }
}

class UserBuilder {
  constructor(name){
    this.user = new User(name)
  }

  setAge(age){
    this.user.age = age
    return this
  }

  setPhone(phone){
    this.user.phone = phone
    return this
  }

  setAddress(address){
    this.user.address = address
    return this
  }

  build(){
    return this.user
  }
}


let user = new UserBuilder('Bob').build()
console.log(user) // User {name: "Bob"}

let user2 = new UserBuilder('Bobby').setAge(10).setPhone('11111111').build()
console.log(user2) // User {name: "Bobby", age: 10, phone: '11111111'}
```


### JavaScript Focus - Advanced and Cleaner

Optional parameters in JavaScript. 

> If the second parameter is not provided for the constructor, an Empty Object will be the default whose keys are undefined.

```
class Address{
  constructor(zip, street){
    this.zip = zip
    this.street = street
  }
}

class User {
  constructor(name, { age, phone, address } = {}){
    this.name = name
    this.age = age
    this.phone = phone
    this.address = address
  }
}


let user = new User('Bob', {age: 10})
console.log(user)
/*
User {
  name: "Bob",
  age: 10,
  address: undefined,
  phone: undefined
}
*/

let user2 = new User('Bob2', {age: 10, address: new Address('1', 'Main')})
console.log(user)
/*
User {
  name: "Bob",
  age: 10,
  address: Address {
    street: "Main",
    zip: "1"
  },
  phone: undefined
}
*/
```

The great thing about this constructor is that we can use **default values**.

The phone is 1234567890 **if we don't pass**.

```
class User {
  constructor(name, { age, phone = '1234567890', address } = {}){
    this.name = name
    this.age = age
    this.phone = phone
    this.address = address
  }
}

let user2 = new User('Bob2', {age: 10, address: new Address('1', 'Main')})
console.log(user)
/*
User {
  name: "Bob",
  age: 10,
  address: Address {
    street: "Main",
    zip: "1"
  },
  phone: "1234567890"
}
*/
```



Source: [Web Dev Simplified](https://www.youtube.com/watch?v=M7Xi1yO_s8E)