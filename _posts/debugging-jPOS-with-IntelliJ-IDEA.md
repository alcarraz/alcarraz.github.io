# Debugging jPOS with IntelliJ IDEA

Today I'm going to share with you how I debug jPOS based applications that use the jPOS recommended build system.

In the first section I'll tell you why you may need to configure extra properties to debug a jPOS application, but if you just want the TL;DR; version, just jump to the [**Create the debug configuration** section](#create-the-debug-configuration).

## Why you may need a specific jPOS debug tutorial

Of course, you can debug a program in any way you like, you could just use `System.out.println()`, or in jPOS better use a logger (`logger.debug()`). However, at least for me, and I guess for you too if you are reading this, nothing beats the possibility of setting breakpoints and watchpoints in real time, and if you can hotswap your code, even better right?.

Even with these characteristics there may be a bunch of options on how to debug a program, you can even hot swap to a JVM that is running remotely in another computer. Don't tell anyone, but I've seen people doing this to find bugs that they could not find the way to reproduce anywhere else, in production instances.

You just need to know 2 things about jPOS to set any IDE to debug it:

1. For many java programs you have a main class with a `main` method, and you just hit the debug button, but jPOS is better used as a framework rather than as a library. So you don't have your own main class on which to hit the debug button, unless you have your main class calling `Q2` to start jPOS, but unless you have to do that for some other reason I don't recommend that approach. Or for another reason you are using jPOS as a library.
2. If you use the jPOS recommended way to build it, you end up having the `deploy` directory (where jPOS loads its components from) deep inside the gradle's `build` directory.

So, wrapping up, you just need to configure three things, I will show you step by step how to do it in IntelliJ IDEA if you don't know:

1. Set `org.jpos.q2.Q2` as the main class.
2. Set the correct working directory.
3. Build the project before run/debug, to reflect any modifications in the build directory. 


## Setup for tutorial
The first thing you need to debug a jPOS application is a jPOS application to debug. As an example, we will start from the jPOS template repository and try to debug one of its components.

We are going to use a jPOS single module template. For multimodule projects things can be easily extrapolated, but let me know if you would like to see one tutorial about that, and I'll make one in the future.

1. Clone the [jPOS template repository](https://github.com/jpos/jPOS-template):
    ```shell
    git clone https://github.com/jpos/jPOS-template.git
    ```
   You should have this directory structure:
   ```
   jPOS-template
   ├── bin
   │   ├── q2
   │   ├── start
   │   └── stop
   ├── build.gradle
   ├── COPYRIGHT
   ├── gradle
   │   └── wrapper
   │       ├── gradle-wrapper.jar
   │       └── gradle-wrapper.properties
   ├── gradlew
   ├── gradlew.bat
   ├── jpos-app.gradle
   ├── LICENSE
   ├── README.md
   └── src
       └── dist
           ├── bin
           │   ├── bsh
           │   ├── q2
           │   ├── q2.bat
           │   ├── start
           │   └── stop
           ├── deploy                  <1>
           │   ├── 00_logger.xml       <2>
           │   └── 99_sysmon.xml       <3>
           └── log
               └── q2.log
   
   ```
   <1>
   : Deploy directory
   
   <2>
   : A deployment file to define the default logger.
   
   <3>
   : A deployment file that shows relevant system information every hour.

2. Open IntelliJ and go to **File** -> **Open**, navigate to the `jPOS-template` directory just created, and click OK. 
 
The recommended way to build this application for distribution or for local execution is to execute:
```shell
./gradlew installApp
```
If you do that, you will have the following directory structure under the `build` directory:
```
build
├── install
│   └── jPOS-template                           <1>
│       ├── bin
│       │   ├── bsh
│       │   ├── q2                              <2>
│       │   ├── q2.bat
│       │   ├── start
│       │   └── stop
│       ├── deploy                              <3>
│       │   ├── 00_logger.xml
│       │   └── 99_sysmon.xml
│       ├── jPOS-template-2.1.8-SNAPSHOT.jar    <4>
│       ├── lib
│       │   ├── *.jar
│       └── log
│           └── q2.log
.
.
.
```
<1>
: Directory whose content will be distributed.

<2>
: Script that launches the application, we want to replicate this in the run/debug configuration.

<3>
: Copy of the `deploy` directory with the components to be deployed. This is a copy of the base `deploy` directory, but with filters applied, in this case will be identical, but it may have some modifications, and that is why you need to ensure this one is the one that is used when you launch your jPOS application.

<4>
: jar with the project's code, in this case just contains some metadata, because we didn't write any code of our own.

## Create the debug configuration.

