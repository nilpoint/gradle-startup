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
for a build file. Gradle will look for a file with this name in the current directory,
to execute the build. But we can change this with the command-line options
--build-file (or -b ) and --project-dir (or -p ).
```bash
gradle --project-dir hello-world -q hW
```

### Running tasks without execution

With the option `--dry-run` (or `-m` ), we can run all the tasks without really
executing them. When we use the dry run option, we can see which tasks are executed,
so we get an insight into which tasks are involved in a certain build scenario.
And we don't have to worry if the tasks are actually executed. Gradle builds up a
Directed Acyclic Graph (DAG) with all the tasks before any task is executed. The DAG
is built so that tasks will be executed in order of dependencies and so that a task
is executed only once.
```bash
gradle --dry-run helloWorld
```

### Gradle daemon

The command-line option, `--daemon` , starts a new Java process
that will have all Gradle classes and libraries already loaded, and then we execute
the build. The next time when we run Gradle with the --daemon option, only the
build is executed, because the JVM, with the required Gradle classes and libraries,
is already running.

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

To always use the `--daemon` command-line option, without typing it every time we
run the gradle command, we can create an alias.
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

`--profile` option records the time that certain tasks take to complete. The data is saved in an HTML file in the directory `build/reports/profile`.

### Gradle user interface

To start the GUI, we invoke the following command:

```
gradle --gui
```

#### Task Tree

The Task Tree tab shows projects and tasks found in our build project. We can
execute a task by double-clicking on the task name.

#### Favorites

The Favorites tab stores tasks we want to execute regularly. We can add a task by
right-clicking on the task in the Task Tree tab and selecting the Add To Favorites
menu option.

#### Command Line

On the Command Line tab, we can enter any Gradle command we normally would
enter on the command prompt.

#### Setup

The last tab is the Setup tab. Here, we can change the project directory, which
is set by default to the current directory.

In the GUI, we can select the logging level from the Log Level
drop-down box with the different log levels. We can choose one of `Debug`, `Info`,
`Lifecyle`, and `Error` as the log levels. The Error log level only shows errors and
is the least verbose, while Debug is the most verbose log level. The Lifecyle
log level is the default log level.

Finally, we can define a different way to start Gradle for the build, with the
option Use `Custom Gradle Executor`. For example, we can define a different batch
or script file with extra setup information to run the build process.
