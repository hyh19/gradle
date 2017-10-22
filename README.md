# Gradle Practice

https://gradle.org/

https://gradle.org/docs/

https://github.com/gradle/gradle-completion

https://github.com/gradle/gradle

## [Installation](https://gradle.org/install/)

Gradle runs on all major operating systems and requires only a Java JDK or JRE version 7 or higher to be installed. To check, run `java -version`:

```
$ java -version
java version "1.8.0_121"
```

### Install with a package manager

[SDKMAN!](http://sdkman.io/) is a tool for managing parallel versions of multiple Software Development Kits on most Unix-based systems.

```
$ sdk install java 8u144-oracle
$ sdk install gradle 4.2.1
```

## Reference

[API Javadoc](https://docs.gradle.org/current/javadoc/)

[DSL Reference](https://docs.gradle.org/current/dsl/)

## [Gradle User Guide](https://docs.gradle.org/current/userguide/userguide.html)


### [Chapter 4. Using the Gradle Command-Line](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html)

#### [4.1. Executing multiple tasks](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html#sec:executing_multiple_tasks)

```
$ gradle dist test
```

#### [4.2. Excluding tasks](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html#sec:excluding_tasks_from_the_command_line)

```
$ gradle dist -x test
```

#### [4.5. Selecting which build to execute](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html#sec:selecting_build)

Selecting the project using a build file
```
$ gradle -q -b subdir/myproject.gradle hello
using build file 'myproject.gradle' in 'subdir'.
```

Selecting the project using project directory
```
$ gradle -q -p subdir hello
using build file 'build.gradle' in 'subdir'.
```

#### [4.7. Obtaining information about your build](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html#sec:obtaining_information_about_your_build)

##### [4.7.2. Listing tasks](https://docs.gradle.org/current/userguide/tutorial_gradle_command_line.html#sec:listing_tasks)

Obtaining information about tasks
```
$ gradle -q tasks
```

Obtaining more information about tasks
```
$ gradle -q tasks --all
```





