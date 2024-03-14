# Gradle

Gradle automates **building**, **testing**, and **deployment** of software from information in build scripts.

![gradle-basic-1](https://github.com/arunav-b/gradle/assets/44658033/3c5fcd99-4c86-4e0b-b85c-deac4f409fce)

## Gradle Basics

- A Gradle **Project** is a piece of software (application or library) that can be built.
  - Single project builds include a single project called root project.
  - Multi-project builds include one root project and any number of subprojects. 
- **Build Scripts** detail to Gradle what steps to take to build a project. Each project can have one or more build scripts.
- **Dependency Management** is an automated technique for declaring and resolving external resources required by a project.
- **Tasks** are a basic unit of work such as compiling code or running test. Each project contains one or more tasks defined inside a build script or a plugin.
- **Plugins** are used to extend Gradle's capability and optionally contribute tasks to a project.

<br>

## Gradle Project structure

```yaml
project
├── gradle                                  # Gradle directory to store wrapper files and more                 
│   ├── libs.versions.toml                  # Gradle version catalog for dependency management
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew                                 # Gradle wrapper scripts
├── gradlew.bat                         
├── settings.gradle(.kts)                   # Gradle settings file to define a root project name and subprojects
├── subproject-a
│   ├── build.gradle(.kts)                  # Gradle build script for the subproject
│   └── src                                 # Source code and/or additional files for the projects
└── subproject-b
    ├── build.gradle(.kts)              
    └── src                             
```

<br>

### Gradle Wrapper
The Wrapper is a script that invokes a declared version of Gradle and is the recommended way to execute a Gradle build. It is found in the project root directory as a gradlew or gradle.bat file.

The Wrapper script invokes a declared version of Gradle, downloading it beforehand if necessary.

![wrapper-workflow](https://github.com/arunav-b/gradle/assets/44658033/8a0efae7-ee93-4592-8a1a-14dc41bec91f)

The Wrapper provides the following benefits:
- Standardizes a project on a given Gradle version.
- Provisions the same Gradle version for different users.
- Provisions the Gradle version for different execution environments (IDEs, CI servers…​).

To run the Wrapper on a Linux or OSX machine:
```gradle
$ ./gradlew build
```

<br>

## Gradle CLI Basics

- Executing Gradle on the command line conforms to the following structure:
```gradle
gradle [taskName...] [--option-name...]
```
- If multiple tasks are required to be specified, we can separate them with a space.
```gradle
gradle [taskName1 taskName2...] [--option-name...]
```
- Options that accept values can be specified with or without = between the option and argument. The use of = is recommended.
```gradle
gradle [...] --console=plain
```
- Options that enable behavior have long-form options with inverses specified with --no-. The following are opposites.
```gradle
gradle [...] --build-cache
gradle [...] --no-build-cache
```
- To execute a task called taskName on the root project, type:
```gradle
$ gradle :taskName
```
- To pass an option to a task, prefix the option name with -- after the task name:
```gradle
$ gradle taskName --exampleOption=exampleValue
```
- In a multi-project build, subproject tasks can be executed with : separating the subproject name and task name. The following are equivalent when run from the root project:
```gradle
$ gradle :subproject:taskName
$ gradle subproject:taskName
```
- We can exclude a task from being executed using the -x or --exclude-task command-line option and providing the name of the task to exclude:
```gradle
$ gradle dist --exclude-task test
```
[Read more](https://docs.gradle.org/current/userguide/command_line_interface.html#command_line_interface)

<br>

## Settings file Basics

- The settings file is the entry point of every Gradle project.
- The primary purpose of the settings file is to add subprojects to your build. Gradle supports single and multi-project builds.
- For single-project builds, the settings file is optional, but for multi-project builds, the settings file is mandatory and declares all subprojects.
- The settings file is a script. It is either a `settings.gradle` file written in Groovy or a `settings.gradle.kts` file in Kotlin.

`settings.gradle.kts`
```gradle
rootProject.name = "root-project"     // Define the project name

include("sub-project-a")              // Sub projects           
include("sub-project-b")
include("sub-project-c")
```
[Read more](https://docs.gradle.org/current/userguide/writing_settings_files.html)

<br>

## Build File Basics

Generally, a build script details build **configuration**, **tasks**, and **plugins**. Every Gradle build comprises at least one build script.

In the build file, two types of dependencies can be added:
1. The libraries and/or plugins on which Gradle and the build script depend.
2. The libraries on which the project sources (i.e., source code) depend.

The build script is either a `build.gradle` file written in Groovy or a `build.gradle.kts` file in Kotlin.

```gradle
plugins {
    id("application")                // Adding a plugin to a build makes additional functionality available
}

application {
    mainClass = "com.example.Main"   // A plugin adds tasks to a project. It also adds properties and methods to a project
}
```

- The `application` plugin facilitates creating an executable JVM application. Applying the Application plugin also implicitly applies the Java plugin. The java plugin adds Java compilation along with testing and bundling capabilities to a project.
- The `application` plugin defines tasks that package and distribute an application, such as the `run` task. The Application plugin provides a way to declare the main class of a Java application, which is required to execute the code.

[Read more](https://docs.gradle.org/current/userguide/writing_build_scripts.html)

<br>

## Dependency Management Basics

- Dependency management is an automated technique for declaring and resolving external resources required by a project.
- Gradle build scripts define the process to build projects that may require external dependencies. Dependencies refer to JARs, plugins, libraries, or source code that support building your project.

### Version Catalog
**Version catalogs** provide a way to centralize your dependency declarations in a `libs.versions.toml` file. The catalog makes sharing dependencies and version configurations between subprojects simple. It also allows teams to enforce versions of libraries and plugins in large projects.

The version catalog typically contains four sections:
- `[versions]` to declare the version numbers that plugins and libraries will reference.
- `[libraries]` to define the libraries used in the build files.
- `[bundles]` to define a set of dependencies.
- `[plugins]` to define plugins.

```gradle
[versions]
androidGradlePlugin = "7.4.1"
mockito = "2.16.0"

[libraries]
google-material = { group = "com.google.android.material", name = "material", version = "1.1.0-alpha05" }
mockito-core = { module = "org.mockito:mockito-core", version.ref = "mockito" }

[plugins]
android-application = { id = "com.android.application", version.ref = "androidGradlePlugin" }
```

The file is located in the gradle directory so that it can be used by Gradle and IDEs automatically. The version catalog should be checked into source control: `gradle/libs.versions.toml`.

### Declaring your dependency

To add a dependency to our project, we need to specify a dependency in the dependencies block of `build.gradle.kts` file. The following `build.gradle.kts` file adds a plugin and two dependencies to the project using the version catalog above:

```gradle
plugins {
   // Applies the Android Gradle plugin to this project, which adds several features that are specific to building Android apps.
   alias(libs.plugins.android.application)  
}

dependencies {
    // Dependency on a remote binary to compile and run the code
    implementation(libs.google.material)    

    // Dependency on a remote binary to compile and run the test code
    testImplementation(libs.mockito.core)   
}
```

Dependencies in Gradle are grouped by configurations.
- The `material` library is added to the `implementation` configuration, which is used for compiling and running production code.
- The `mockito-core` library is added to the `testImplementation` configuration, which is used for compiling and running test code.

We can view dependency tree in the terminal using the following command: 
```gradle
./gradlew :app:dependencies
```

[Read more](https://docs.gradle.org/current/userguide/dependency_management_terminology.html#dependency_management_terminology)

<br>

## Task Basics

A task represents some independent unit of work that a build performs, such as compiling classes, creating a JAR, generating Javadoc, or publishing archives to a repository.
To get the list of tasks we can execute the following command

```gradle
$ ./gradlew tasks
```

### Task Dependency
Many times, a task requires another task to run first. For example, for Gradle to execute the `build` task, the Java code must first be compiled. Thus, the `build` task _depends_ on the `compileJava` task.

[Read more](https://docs.gradle.org/current/userguide/more_about_tasks.html)

<br>

## Plugin Basics

<br>

## Build phases in Gradle:
- Initialization
- Configuration
- Execution
  - doFirst
  - doLast
  - depends On
 
[Read more](https://docs.gradle.org/current/userguide/build_lifecycle.html)
> **Note**
>
> - Plugins extend project
>   - 'Well-known' plugins like java
>   - 'Community' plugins like flyway

## Build Java Project
## Build Kotlin Project
## Testing using Gradle
