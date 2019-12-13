# Singleton Pattern

## Definition

A way of creating a **single object** that is **shared among different resources** throughout your application without having to recreate that object or lose any of the information inside of it. The state of that object, variables and methods, are shared among different other objects.

One source of information. There is only ever a single type of this object created. This is why it's called a singleton. Your application will only and can only have one type of this object instantiated at a time.

All places that use this object, share that single instance of the object.

**Singletons are essentially global** to your entire application

## Limitations

It can be incredibly **hard to test**. You can run into to what is called the **race condition** where changing things inside of that singleton can cause data to get overwritten or not read correctly because all try to access the exact same information at the same exact time.

Some people say you should never use this singleton design pattern in any of your applications. But there are several neat uses for the singleton design pattern.


## Example of not Using Singleton

**fancyLogger.js**
```
export default class FancyLogger {
  constructor(){
    this.logs = []
  }

  log(message){
    this.logs.push(message)
    console.log(`FANCY: ${message}`)
  }

  printLogCount(){
    console.log(`${this.logs.length} Logs`)
  }
}
```

**firstUse.js**
```
import FancyLogger from './fancyLogger.js'
const logger = new FancyLogger()

export default function logFirstImplementation(){
  logger.printLogCount()
  logger.log('First File')
  logger.printLogCount()
}
```

**secondUse.js**
```
import FancyLogger from './fancyLogger.js'
const logger = new FancyLogger()

export default function logFirstImplementation(){
  logger.printLogCount()
  logger.log('Second File')
  logger.printLogCount()
}
```

**index.js**
```
import logFirstImplementation from './firstUse.js'
import logSecondImplementation from './secondUse.js'

logFirstImplementation()
logSecondImplementation()
```

Output:
```
/*
  0 Logs
  FANCY: First File
  1 Logs
  0 Logs
  FANCY: Second File
  1 Logs
*/
```
Logger goes back to 0 because in each of the files we are creating a new Instance. We don't have the info from the previous logger.

Great Use case.

## Refact to Singleton Pattern
We don't export the class, we want to use a single instance of this class.

**fancyLogger.js**
```
class FancyLogger {
  constructor(){
    if(FancyLogger.instance == null){  // instance is a variable we are creating
      this.logs= []
      FancyLogger.instance = this
    } 
    return FancyLogger.instance  // we want to return that instance the second time
  }

  log(message){
    this.logs.push(message)
    console.log(`FANCY: ${message}`)
  }

  printLogCount(){
    console.log(`${this.logs.length} Logs`)
  }
}

const logger = new FancyLogger()
Object.freeze(logger)  // prevent of change in any way!
export default logger
```

that logger class cannot have any new methods or variables added on to it or removed from it.

**We are importing an instance instead of the Class**

**firstUse.js**
```
import logger from './fancyLogger.js'

export default function logFirstImplementation(){
  logger.printLogCount()
  logger.log('First File')
  logger.printLogCount()
}
```

**secondUse.js**
```
import logger from './fancyLogger.js'

export default function logFirstImplementation(){
  logger.printLogCount()
  logger.log('Second File')
  logger.printLogCount()
}
```

**index.js**
```
import logFirstImplementation from './firstUse.js'
import logSecondImplementation from './secondUse.js'

logFirstImplementation()
logSecondImplementation()
```

Output: 
```
/*
  0 Logs
  FANCY: First File
  1 Logs
  1 Logs
  FANCY: Second File
  2 Logs
*/
```
Now it's working as intended because we are using a single Instance.