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
### Default tasks

Show the available tasks for a project.
```bash
gradle -q tasks
```

Show the general help information.
```bash
gradle -q help
```

See the properties of a project.
```bash
gradle -q properties
```

Show dependencies (if any) for a project.
```bash
gradle -q dependencies
```

Display sub-projects for a project.
```bash
gradle -q projects
```

### Task name abbreviation

With task name abbreviation, we don't have to type the complete task name on
the command line. We only have to type enough of the name to make it unique 
within the build.

```bash
gradle -q hello
```

We can also abbreviate each word in a camel case task name.
```bash
gradle -q hW
```

### Executing multiple tasks

To execute multiple tasks we only have to add each task name to the command line.

```bash
gradle -q hW tasks
```

