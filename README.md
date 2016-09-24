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

The code between the brackets is a closure. A closure is a code block that can be assigned to a variable or passed to a method.

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

### Logging options
Let's look at some of the options in more detail. The options --quiet (or -q ),
--debug (or -d ), --info (or -i ), --stacktrace (or -s ), and --full-stacktrace
(or -S ) control the amount of output we see when we execute tasks.

The options --stacktrace and --full-stracktrace can be combined with the other
logging options.
```bash
gradle --info helloWorld
```

### Changing the biuld file and directory
We created our build file with the name build.gradle . This is the default name
for a build file. Gradle will look for a file with this name in the current directory, to execute the build. But we can change this with the command-line options
--build-file (or -b ) and --project-dir (or -p ).
```bash
gradle --project-dir hello-world -q hW
```

### Running tasks without execution

With the option `--dry-run` (or `-m` ), we can run all the tasks without really
executing them. When we use the dry run option, we can see which tasks are executed, so we get an insight into which tasks are involved in a certain build scenario.

And we don't have to worry if the tasks are actually executed. Gradle builds up a Directed Acyclic Graph (DAG) with all the tasks before any task is executed. The DAG is built so that tasks will be executed in order of dependencies and so that a task is executed only once.
```bash
gradle --dry-run helloWorld
```

### Gradle daemon

The command-line option, `--daemon` , starts a new Java process that will have all Gradle classes and libraries already loaded, and then we execute the build. The next time when we run Gradle with the --daemon option, only the build is executed, because the JVM, with the required Gradle classes and libraries, is already running.

```bash
gradle --daemon helloWorld
```

We use the command-line option `--no-daemon` to run a Gradle
build without utilizing the daemon:

```bash
gradle --no-daemon hW
```

To stop the daemon process, we use the command-line option `--stop`:
```bash
gradle --stop
```

To always use the `--daemon` command-line option, without typing it every time we run the gradle command, we can create an alias.
```bash
alias gradled='gradle --daemon'
```

Instead of using the `--daemon` command-line option, we can use the Java system
property org.gradle.daemon to enable the daemon. We can add this property
to environment variable `GRADLE_OPTS`, so that it is always used when we run
a Gradle build.
```bash
export GRADLE_OPTS="-Dorg.gradle.daemon=true"
```

### Profiling

`--profile` option records the time that certain tasks take to complete. 
The data is saved in an HTML file in the directory `build/reports/profile`.

### Gradle user interface

To start the GUI, we invoke the following command:

```
gradle --gui
```

#### Task Tree

The Task Tree tab shows projects and tasks found in our build project. We can
execute a task by double-clicking on the task name.

#### Favorites

The Favorites tab stores tasks we want to execute regularly. We can add a task by right-clicking on the task in the Task Tree tab and selecting the Add To Favorites menu option.

#### Command Line

On the Command Line tab, we can enter any Gradle command we normally would
enter on the command prompt.

#### Setup

The last tab is the Setup tab. Here, we can change the project directory, which
is set by default to the current directory.

In the GUI, we can select the logging level from the Log Level
drop-down box with the different log levels. We can choose one of `Debug`, `Info`, `Lifecyle`, and `Error` as the log levels. The Error log level only shows errors and is the least verbose, while Debug is the most verbose log level. The Lifecyle log level is the default log level.

Finally, we can define a different way to start Gradle for the build, with the
option Use `Custom Gradle Executor`. For example, we can define a different batch or script file with extra setup information to run the build process.

## Creating Gradle Build Scripts

### A simple build script

```gradle
project.description = "Simple Project"

task simple << {
  println 'Running simple task for project ' + project.description
  println project
  println project.name
}
```

A couple of interesting things happen with this small build script. Gradle reads
the script file and creates a `Project` object. The build script configures the 
`Project` object, and finally the set of tasks to be executed is determined and 
executed.

The `Project` object has several properties and methods, and it is available in our build scripts. We can use the variable name `projec` to reference the `Project` object, but we can also leave out this variable name to reference properties nd methods of the `Project` object. Gradle will automatically try to map roperties and methods in the build script to the `Project` object.

### Defining tasks

A project has one or more tasks to execute some actions, so a task is made up of actions. These actions are executed when the task is executed. Gradle supports several ways to add actions to our tasks.

```gradle
task first {
    doFirst {
        println 'Running first'
    }
}

task second {
    doLast { Task task ->
        println "Running ${task.name}"
    }
}

task third << { taskObject ->
    println 'Running ' + taskObject.name
}
```

We can use the doFirst and doLast methods to add actions to our task, and we can use the left shift operator (<<) as a synonym for the doLast method. With the doLast method or the left shift operator (<<) we add actions at the end of the list of actions for the task. With the doFirst method we can add actions to the beginning of the list of actions.

Maybe it is a good time to look more closely at closures, because they are an important part of Groovy and are used throughout Gradle build scripts. Closures are basically reusable pieces of code that can be assigned to a variable or passed to a method. A closure is defined by enclosing the piece of code between curly brackets ({...}). We can pass one or more parameters to closures. If the closure has only one argument, an implicit parameter, it, can be used to reference the parameter value. We could have written the second task as follows, and the result would still be the same:

```gradle
task second {
    doLast {
        // Using implicit 'it' closure parameter
        println "Running ${it.name}"  
    }
}
```

We can also define a name for the parameter and use that name in the code. This is what we did for the tasks second and third, wherein we named the closure parameter task and taskObject respectively. The resulting code is more readable if we define the parameter name explicitly in our closure:

```gradle
task second {
    doLast { Task task ->
        // Using explicit name 'task' as closure parameter
        println "Running ${task.name}"  
    }
}
```

Gradle often has more than one way of defining something, as we will see throughout the book. Besides using closures to add actions to a task, we can also use a more verbose way by passing an implementation class of the org.gradle.api.Action interface. The Action interface has one method: execute. This method is invoked when the task is executed. The following piece of code shows a reimplementation of the first task in our build script:

```gradle
task fourth {
  doFourth(
    new Action() {
      void execute(task) {
          println "Running ${task.name}"
      }
    }
  )
}
```

#### Interface Task

```groovy
public interface Task
extends Comparable<Task>, ExtensionAware
```

A Task represents a single atomic piece of work for a build, such as compiling classes or generating javadoc.

Each task belongs to a Project. You can use the various methods on TaskContainer to create and lookup task instances. For example, TaskContainer.create(String) creates an empty task with the given name. You can also use the task keyword in your build file:

```gradle
task myTask
task myTask { configure closure }
task myType << { task action }
task myTask(type: SomeType)
task myTask(type: SomeType) { configure closure }
```

Each task has a name, which can be used to refer to the task within its owning project, and a fully qualified path, which is unique across all tasks in all projects. The path is the concatenation of the owning project's path and the task's name. Path elements are separated using the ":" character.

#### Task Actions

A `Task` is made up of a sequence of `Action` objects. When the task is executed, each of the actions is executed in turn, by calling `Action.execute(T)`. You can add actions to a task by calling `doFirst(Action)` or `doLast(Action)`.

Groovy closures can also be used to provide a task action. When the action is executed, the closure is called with the `task` as parameter. You can add action closures to a task by calling `doFirst(groovy.lang.Closure)` or `doLast(groovy.lang.Closure)` or using the left-shift `<<` operator.

There are 2 special exceptions which a task action can throw to abort execution and continue without failing the build. A task action can abort execution of the action and continue to the next action of the task by throwing a `StopActionException`. A task action can abort execution of the task and continue to the next task by throwing a `StopExecutionException`. Using these exceptions allows you to have precondition actions which skip execution of the task, or part of the task, if not true.

