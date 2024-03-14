# Gradle

Gradle automates **building**, **testing**, and **deployment** of software from information in build scripts.

![gradle-basic-1](https://github.com/arunav-b/gradle/assets/44658033/3c5fcd99-4c86-4e0b-b85c-deac4f409fce)

## Core Concepts

- A Gradle **Project** is a piece of software (application or library) that can be built.
  - Single project builds include a single project called root project.
  - Multi-project builds include one root project and any number of subprojects. 
- **Build Scripts** detail to Gradle what steps to take to build a project. Each project can have one or more build scripts.
- **Dependency Management** is an automated technique for declaring and resolving external resources required by a project.
- **Tasks** are a basic unit of work such as compiling code or running test. Each project contains one or more tasks defined inside a build script or a plugin.
- **Plugins** are used to extend Gradle's capability and optionally contribute tasks to a project.
  
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
 
## Understanding Tasks
- Gradle runs on tasks
- plugin helps executes tasks
- java plugin executes tasks on java folder that is present under src (main for build and tests for test)
- gradle build creates the build folder
- If any of the source file changes the compile task runs and then the jar task runs to package the new files again
- Example of a Task that runs -
  Task: compileJava
### Types of Tasks:
- Build setup tasks
  - init
  - wrapper
- Help tasks
  - dependencies
  - build environment
  - components
  - projects
  - properties
  - help, etc
## Build phases in Gradle:
- Initialization
- Configuration
- Execution
  - doFirst
  - doLast
  - depends On
> **Note**
>
> - Gradle mostly consists of
>   - projects
>   - tasks
> - Build has one or more projects
> - Project has one or more tasks
> - Plugins extend project
>   - 'Well-known' plugins like java
>   - 'Community' plugins like flyway
## Build Java Project
## Build Kotlin Project
## Dependency Management
## Multi-module build
## Testing using Gradle
## Gradle Wrapper
It is used to execute a particular version of gradle
