# Gradle Startup
Learn gradle basics.

## Getting Started

### hello-world

Edit the gradle script file build.gradle:

```gradle
task helloWorld << {
  println 'Hello world.'
}
```

With this code we define a helloWorld task. println is a Groovy method to print
text to the console and is basically a shorthand version of the Java method
System.out.println.

The code between the brackets is a closure. A closure is a code block that can be
assigned to a variable or passed to a method.

The << syntax is, technically speaking, operator shorthand for the method
leftShift() , which actually means "add to".

Run the script,

```bash
gradle helloWorld
gradle --quiet helloWorld
```
