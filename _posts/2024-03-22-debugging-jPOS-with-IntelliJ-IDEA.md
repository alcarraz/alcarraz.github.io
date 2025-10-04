---
title: "Three easy ways to debug jPOS with IntelliJ IDEA."
date: 2024-03-22
---
# Three easy ways to debug jPOS with IntelliJ IDEA.

Today I'm going to share with you how I debug jPOS based applications that follow the jPOS app conventions.

In the first section, I'll tell you why you may need to configure extra properties to debug a jPOS application,
but if you just want the TL;DR; version,
just jump to [**Create the debug configuration** section](#create-the-debug-configuration).

## Why you may need a specific jPOS debug tutorial

Of course, you can debug a program in any way you like, you could just use `System.out.println()`, or in jPOS better use a logger (`logger.debug()`). However, at least for me, and I guess for you too if you are reading this, nothing beats the possibility of setting breakpoints and watchpoints in real time, and if you can hotswap your code, even better right?.

Even with these characteristics, there may be a bunch of options on how to debug a program,
you can even hot swap to a JVM that is running remotely on another computer.
Don't tell anyone,
but I've seen people doing this in production to find bugs that they could not find the way to reproduce anywhere else.

All you need to know is two things about jPOS to set any IDE to debug it:

1. For many java programs you have a main class with a `main` method, and you hit the debug button, but jPOS is better used as a framework rather than as a library. So you don't have your own main class on which to hit the debug button, unless you have your main class calling `Q2` to start jPOS for some reason.
2. If you follow jPOS app conventions, you end up having the `deploy` directory (where jPOS loads its components from) deep inside the gradle's `build` directory.

So, wrapping up, you only need to configure three things,
I will show you step by step how to do it in IntelliJ IDEA if you don't know:

1. Set `org.jpos.q2.Q2` as the main class.
2. Set the correct working directory.
3. Build the project before run/debug, to reflect any modifications in the build directory. 


## Setup for tutorial
The first thing you need to debug a jPOS application is a jPOS application to debug. As an example, we will start from the jPOS template repository and try to debug one of its components.

We are going to use a jPOS single module template.
For multimodule projects, things can be easily extrapolated,
but let me know if you would like to see one tutorial about that, and I'll consider making one in the future.

1. Clone the [jPOS template repository](https://github.com/jpos/jPOS-template):
    ```shell
    git clone https://github.com/jpos/jPOS-template.git
    ```
   You should have this directory structure:
   ```dirtree
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

2. Open IntelliJ and go to **File** → **Open**, navigate to the `jPOS-template` directory just created, and click OK. 

The recommended way to build this application for distribution or for local execution is to execute:
```shell
./gradlew installApp
```
If you do that, you will have the following directory structure under the `build` directory:
```dirtree
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
│       ├── lib                                 <5> 
│       │   ├── *.jar               
│       └── log
│           └── q2.log
.
.
.
```
<1>
: The directory whose content will be distributed.

<2>
: Script that launches the application, we want to replicate this in the run/debug configuration.

<3>
: Copy of the `deploy` directory with the components to be deployed. This is a copy of the base `deploy` directory, but with filters applied, in this case will be identical, but it may have some modifications, and that is why you need to ensure this one is the one used when you launch your jPOS application.

<4>
: Jar file with the project's code, in this case, it just contains some metadata because we didn't write any code of our own.

<5>
: Dependencies jars.

## Create the debug configuration.
<img src="/img/new-run-debug-configuration.png" alt="new run/debug configuration" style="width: 20em;float: right; margin-left: 10px; " /> 
To create a new debug configuration you need to perform these 3 steps, then, follow the steps for the debug way of your choice, or all three if you like.

Each way has its own pros and cons, the best for you will depend on your taste and/or the project structure.

1. Open the project in IntelliJ IDEA.
2. In the **Run** menu, select edit configurations.
3. Click on the plus sign, **Add new...**, or **Add new run configuration**.

### Gradle debug configuration.
The easiest way to debug a jPOS application that follows the jPOS app conventions is
to execute the gradle `run` task in debug mode.
This way has a downside, you don't get to pass parameters to the application, if you need to, go to the next section.

1. Open the Gradle view, for instance, by the *View* menu → *Tool Windows* → *Gradle.*
2. Expand `jPOS-template` -> `Tasks` -> `other`
3. Right-click on `run` and select `Debug jPOS template [run]`.

That's it, this will launch the application in debug mode, and create the debug configuration `jPOS-template [sourceJar]`.


### Java application debug configuration.
This is the configuration more complicated to set up, but is also always work, or at least the one covering most scenarios.

<img src="/img/app-debug-configuration.png" alt="app debug configuration" style="width: 20em;float: right; margin-left: 10px; "/>

1. Select **Application** in the *Add New Configuration* dropdown.
2. In **Name**, enter something you like, for example, `jPOS debug`.
3. In the **-cp** combo box, select `jPOS-template.main`
4. In **Main class**, enter `org.jpos.q2.Q2`
5. In **Working directory** enter `build/install/jPOS-template`
6. Click on **Modify options** and mark the **Add before launch task** option.
7. Click the plus sign next to **Build**: ![before launch task](/img/before-launch.png)
8. Select **Run Gradle task**, this is so that the `install` directory is updated with modifications made on the `dist` directory, before every run. 
     1. In **Gradle project**, click on the folder button (![folder icon](/img/folder-icon.png)), and select **jPOS-template**, i.e., the name of the Gradle project. 
     2. In **Tasks**, enter `installApp`
     3. Click the **OK** button 
   
9. Click the **OK** button. 
 
### Java application from `src/dist` directory, my favorite, when possible.
The previous two options work for almost any jPOS project, but I prefer this third one, when possible.
Between jvm support for hot swap of code,
and the ability of jPOS for hot deployment, I find
limiting having to restart the program every time I want to modify a deployment file.
If you don't want that with the two previous ways of debugging jPOS, you have two options:

1. Modify the deployment file directly in the `build/install/jPOS-template/deploy` directory.
2. Modify the deployment file in the `src/dist/deploy` directory and the copy it to the `build/install/jPOS-template/deploy` directory.

With the first option, you take the risk of losing your changes with a future deployment,
if you forget what you are doing, it happened to me more than once.
The second option looks like too much work for every little modification.

So, what I try to do,
is that the code is directly runnable from `src/dist` and use the `Environment` feature instead of
(or together with) token replacement to achieve that,
I can write something about that in the future if you ask me nicely :).
This also has its own downsides, like if you modify the XML and jPOS doesn't like it;
it could move it out of the way by appending the `.BAD` suffix,
if this is worst for you, use the other ways to debug.
Of course, if you are working with a legacy application,
or in an application for which you don't control the structure, which uses token replacement, this isn't an option. 

1. Select **Application** in the *Add New Configuration* dropdown.
2. In **Name**, enter something you like, for example, `jPOS debug`.
3. In the **-cp** combo box, select `jPOS-template.main`
4. In **Main class**, enter `org.jpos.q2.Q2`
5. In **Working directory** enter `src/dist`
6. Click the **OK** button.

Easy-peasy, isn't it?


## Test the debug configuration.

To test the debug configuration, we are going to set a breakpoint in jPOS code, and validate the program stops there.

1. Open the `SystemMonitor` class, by clicking on the **Navigate** menu, and selecting the **Class** item. Start typing `SystemMonitor` until you can select `org.jpos.q2.qbean.SystemMonitor`.
2. Set a breakpoint in the `startService` method.
3.  Start the run configuration in debug mode by clicking on the green bug button, make sure the just created run configuration is selected in the run combo: ![start debug](/img/start-debug.png)

The debugger should stop at that method, and that’s it. Happy debugging.
 
If you'd like me to cover other jPOS related stuff, let me know, you can contact me at [<img src="/img/linkedin.svg" width="16" style="vertical-align:middle">/andresalcarraz](https://linkedin.com/in/andresalcarraz) or [<img src="/img/twitter.svg" width="16" style="vertical-align:middle">/andresalcarraz](https://twitter.com/andresalcarraz).

Head over to [jPOS tutorials](http://www.jpos.org/tutorials) for awesome tutorials from which you can experiment (and debug).

Join the [jPOS slack](http://jpos.slack.com/) or [jPOS users mailing list](http://groups-beta.google.com/group/jpos-users),
for asking questions or learn from others' questions.
You can also check the [Stack Overflow jPOS tag](https://stackoverflow.com/questions/tagged/jpos). 
