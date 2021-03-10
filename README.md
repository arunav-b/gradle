# Gradle
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
