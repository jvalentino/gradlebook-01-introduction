# 1.      Forward

When I started my first big job (more than two people) as a programmer, I thought I had things figured out. I could code, what else did I need? I realized my inadequacy when tasked with building the binaries for C++ based applications on IRIX. There was a language unto itself I had to code in for building the things, which I had just coded. There had to be an easier way, as others have done this, and if not and I am the first, how I can avoid copying and pasting? Thus, began my journey of finding easier ways to build, test, and deploy software.

When I was introduced to Gradle shortly after its release I was immediately hooked. At that point I had been using a combination of Ant, Maven, and scripting on Continuous Integration servers to engineer reusable means for build, test, and deploy for Java and non-Java systems. Even without the introduction of reusable build components, the builds were either too abstract or too verbose. It had become common in the industry for developers to become build specialists just to deal with these complexities. Gradle had simplified all of that for me.

Following the principle of “convention over configuration”, the simplest build need only maintain the standard directory structure, apply the Java plugin, and you were done. Customization was equally as easy, where at first, I could write whatever code I wanted using Groovy directly in the build file. To make that code reusable, I then could make a library that acts as a Gradle plugin in the same manner as the Java plugin. Reusable build components that are easily shared, and in a technology stack that supports test driven development practices.

# 2.      Ant versus Maven versus Gradle

**Ant**: Apache Ant is a Java library and command-line tool whose mission is to drive processes described in build files as targets and extensions points dependent upon each other (http://ant.apache.org/manual). 

**Maven**: Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information (https://maven.apache.org/). 

**Gradle**: Gradle is an open-source build automation tool focused on flexibility and performance. Gradle build scripts are written using a Groovy or Kotlin DSL ([https://docs.gradle.org/current/userguide/userguide.html](https://docs.gradle.org/4.6/userguide/userguide.html)).

## 2.1      General

·   **Ant** - No conventions and everything is procedural, at least when you declare it that way. You can do whatever you want and, in any order, because Ant is a toolbox.

·   **Maven** - Conventions that you are stuck to because Maven is a framework. Syntax is also limited to declarative.

·   **Gradle** - Conventions from which you can easily deviate, and also the ability to be both procedural and declarative.

## 2.2      Dependency Management

·   **Ant** - None built in, and requires you to use Ivy (http://ant.apache.org/ivy/). Downloading dependencies to use as part of your Ant build also requires some strange looking boilerplate code, so that you define Ant task definitions after having the jar files available.

·   **Maven** - Built in but difficult to customize (http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)

·   **Gradle** - Built in and easy to customize (http://www.gradle.org/docs/current/userguide/dependency_management.html)

## 2.3      Build Life Cycle

·   **Ant** - Made up of targets, which can depend on or call other targets. There is no enforced life cycle, which can lead to chaotic builds.

·   **Maven** - Made up of 3 life cycles, which are made of phases, which are made up of goals. There are 30 phases used to cover the clean, default, and site life cycles. (http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

·   **Gadle** - Made up of three distinct phases: Initialization, Configuration, and Execution. This is significantly simpler than the 30 phases of maven. (http://www.gradle.org/docs/current/userguide/build_lifecycle.html)

## 2.4      Dealing with non-Java

**Ant** - With easy access to the command-line and the ability to do whatever you want, non-Java support is only limited by your imagination. Dealing with programmatic problems in a declarative language leads to complexity, for example when doing modular Flex, a mostly dead language (https://stackoverflow.blog/2017/08/01/flash-dead-technologies-might-next/), builds (http://jvalentino.blogspot.com/2010/03/flex-ant-build-optimized-modules_24.html)

·   **Maven** - If you want to deal with non-Java you are going to need to write a custom plugin. When I used to work with Flex it was much easier to build an Ant macro and put it in a jar versus building a Maven plugin.

·   **Gradle** - It is possible to do the same things you did with Ant, or deal with and possibly write your own Gradle plugin.

# 1.      The Basics

## 1.1      Installing Gradle

**Reference**: https://gradle.org/install/

For the purposes of this writing, I am using Gradle 4.3, though the specific version of Gradle should not matter. The only reason one would need to make Gradle available from the command-line, is for the purpose of generating the Gradle Wrapper. The Wrapper is a script that invokes a declared version of Gradle, downloading it beforehand if necessary (https://docs.gradle.org/current/userguide/gradle_wrapper.html).

The following command can be used to verify that Gradle is successfully installed and available from the command-line:

```bash
$ gradle --version

------------------------------------------------------------
Gradle 4.3
------------------------------------------------------------

Build time:   2017-10-30 15:43:29 UTC
Revision:     c684c202534c4138b51033b52d871939b8d38d72

Groovy:       2.4.12
Ant:          Apache Ant(TM) version 1.9.6 compiled on June 29 2015
JVM:          1.8.0_101 (Oracle Corporation 25.101-b13)
OS:           Mac OS X 10.11.6 x86_64
```

Running the following at the command-line, in any directory, will result in the Gradle Wrapper files bring created:

```bash
$ gradle wrapper

BUILD SUCCESSFUL in 1s
```

These files are the following:

·   **gradle/wrapper/gradle-wrapper.jar** – A command-line application used to handle the mechanics of downloading Gradle if needed.

·   **gradle/wrapper/gradle-wrapper.properties** – Specifies properties, specifically the accessible web location that can be used to download Gradle if it needs to be installed

·   **gradlew** – The linux version of the Gradle wrapper script

·   **graldew.bat** – The Windows version of the Gradle wrapper script

If you store these files with your project, the gradlew script can be used to run the Gradle build, so that Gradle does not have to be installed and available at the command-line.