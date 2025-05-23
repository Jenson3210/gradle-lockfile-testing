# Gradle Lockfiles testing

This project is primarily intended to test the functionality of gradle lockfiles icw OpenRewrite recipes. 

The project is a multi-module gradle project with the following modules:
- core: consider it a reusable library that ships some dependencies
- service: a spring boot application that uses the core library
- integration-test: a source set that is used to test the functionality of the core library
- transitive: a project that only has a dependency on the core library to test transitive dependency locking
- jackson: a project that only has a dependency on openrewrite module that ships certain stuff like jackson for testing the jacksom bom handling
- constraints: a project that has a dependency on liquibase-core and constraints the internall version of opencsv to a higher one

There will be 2 branches to test the different behaviour of gradle lockfiles:
- `main`: This branch contains lockfiles that are generated by the gradle build system and does STRICT locking. The lockfiles are generated using `./gradlew resolveAndLockAll --write-locks`. Recipes running against this branch will do best-effort of migrating the lockfile.
- `unlocked`: This branch does not contain the lockfiles and is buildable as strict locking is disabled. It also contains the gradle task to generate the lockfiles. This branch will be used to test auto-generating lockfiles in a post-openrewrite commit action to generate the lockfiles.

WORKFLOW:
If you want to add a test for certain functionality, do this on the `unlocked` branch. Afterwards merge the `unlocked` branch into the `main` branch. **DO NOT DELETE THE UNLOCKED BRANCH**.
When you added a dependency or something alike, you can run the `resolveAndLockAll` task to generate the lockfileson main and push that one afterwards. This way we can always test the added functionality on both branches.