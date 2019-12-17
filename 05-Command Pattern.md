# Command Pattern

Simple code for calculator, not using the command pattern.

```
class Calculator {
  constructor(){
    this.value = 0
  }

  add(valueToAdd){
    this.value = this.value + valueToAdd
  }

  subtract(valueToSubtract){
    this.value = this.value - valueToSubtract
  }

  multiply(valueToMultiply){
    this.value = this.value * valueToMultiply
  }

  divide(valueToDivide){
    this.value = this.value / valueToDivide
  }
}

const calculator = new Calculator()
calculator.add(10)
console.log(calculator.value) // 10
calculator.divide(2)
console.log(calculator.value) // 5
```

## Idea

Take the different operations and encapsulate them into individual commands that have a **perform and undo method**. Essentially, you can do the operation (do the command) and then undo that operation.

Add, subtract, multiply and divide are different commands. We want to take these commands out of our calculator and **make them their own objects**, that have an execute and undo function. We can do and undo all the different commands.

## The Command Pattern

```
class AddCommand {
  constructor(valueToAdd){
    this.valueToAdd = valueToAdd
  }

  execute(currentValue){
    return currentValue + this.valueToAdd
  }

  undo(currentValue){
    return currentValue - this.valueToAdd
  }
}

const addCommand = new AddCommand(10);
const newValue = addCommand.execute(10);
console.log(newValue) // 20
console.log(addCommand.undo(newValue)) // 10
```


### Calculator with Command Pattern

```
class Calculator {
  constructor(){
    this.value = 0
    this.history = []
  }

  executeCommand(command){
    this.value = command.execute(this.value)
    this.history.push(command)
  }

  undo(){
    const command this.history.pop()
    this.value = command.undo(this.value)
  }

  add(valueToAdd){
    this.value = this.value + valueToAdd
  }

  subtract(valueToSubtract){
    this.value = this.value - valueToSubtract
  }

  multiply(valueToMultiply){
    this.value = this.value * valueToMultiply
  }

  divide(valueToDivide){
    this.value = this.value / valueToDivide
  }
}

class AddCommand {
  constructor(valueToAdd){
    this.valueToAdd = valueToAdd
  }

  execute(currentValue){
    return currentValue + this.valueToAdd
  }

  undo(currentValue){
    return currentValue - this.valueToAdd
  }
}

class SubtractCommand {
  constructor(valueToSubtract){
    this.valueToSubtract = valueToSubtract
  }

  execute(currentValue){
    return currentValue - this.valueToSubtract
  }

  undo(currentValue){
    return currentValue + this.valueToSubtract
  }
}

class MultiplyCommand {
  constructor(valueToMultiply){
    this.valueToMultiply = valueToMultiply
  }

  execute(currentValue){
    return currentValue * this.valueToMultiply
  }

  undo(currentValue){
    return currentValue / this.valueToMultiply
  }
}

class DivideCommand {
  constructor(valueToDivide){
    this.valueToDivide = valueToDivide
  }

  execute(currentValue){
    return currentValue / this.valueToDivide
  }

  undo(currentValue){
    return currentValue * this.valueToDivide
  }
}

const calculator = new Calculator()

calculator.executeCommand(new AddCommand(10))
calculator.executeCommand(new MultiplyCommand(2))
console.log(calculator.value) // 20
calculator.undo()
console.log(calculator.value) // 10
```

But this is way more complex and difficult to do the same thing as before.

But in general, commands start simple and then build themselves on them. Like when you close a program you have the options to save, exit and save & exit.


### AddThenMultiplyCommand

The Power of combining different commands

...
```
class AddThenMultiplyCommand {
  constructor(valueToAdd, valueToMultiply){
    this.addCommand = new AddCommand(valueToAdd)
    this.multiplyCommand = new MultiplyCommand(valueToMultiply)
  }

  execute(currentValue){
    const newValue = this.addCommand.execute(currentValue)
    return this.multiplyCommand.execute(newValue)
  }

  undo(currentValue){
    const newValue = this.multiplyCommand.undo(currentValue)
    return this.addCommand.undo(newValue)
  }
}

const calculator = new Calculator()
calculator.executeCommand(new AddThenMultiply(10, 2))
console.log(calculator.value) // 20
calculator.undo() 
console.log(calculator.value) // 0
```

### Text Editors

In **text editors** you have a **button and** a **shortcut** for bolding text. And also an undo shortcut. If you highlight some text and bold it with the button you want the same thing to happen with the keyboard shortcut.