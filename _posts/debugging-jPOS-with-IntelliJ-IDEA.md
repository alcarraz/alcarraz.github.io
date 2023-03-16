# Debugging jPOS with IntelliJ IDEA

Today I'm going to share with you how I debug jPOS based applications that use the jPOS recommended build system.

In the first section I'll tell you why you may need to configure extra properties to debug a jPOS application, but if you just want the TL;DR; version, just jump to ...

## Why you may need a specific jPOS debug tutorial

See, jPOS has a recommended way to build the applications, and so, if you build your software the jPOS way, you may find yourself wondering how to debug it easily.

Of course, you can debug a program in any way you like, you could just use `System.out.println()`, or in jPOS better use a logger (`logger.debug()`). But at least for me, and I guess for you too if you are reading this, nothing beats the possibility of setting breakpoints and watchpoints in real time, and if you can hotswap your code, even better right?.

Even with these characteristics there may be a bunch of options on how to debug a program, you can even hot swap to a JVM that is running remotely in another computer. Don't tell anyone, but I've seen people doing this to find bugs that they could not find the way to reproduce anywhere else, in production instances.

Two things warrant this post:
1. For many java programs you have a main class with a `main` method, and you just hit the debug button, but jPOS is better used as a framework rather than as a library. So you don't have your own main class on which to hit the debug button, unless you have your main class calling `Q2` to start jPOS, but unless you have to do that for some other reason I don't recommend that approach. Or for another reason you are using jPOS as a library.
2. If you use the jPOS recommended way to build it, you end up having the `deploy` directory (where jPOS loads its components from) deep inside the gradle's `target` directory.

So you need to configure two things, I will show you step by step how to do it if you don't know:

1. Set `org.jpos.q2.Q2` as the main class.
2. Set the correct working directory.


## Setup for tutorial
The first thing you need to debug a jPOS application is a jPOS application to debug. As an example, we will start from the jPOS template repository and then add some components, and a bit of code as an example to debug.

We are going to use a jPOS single module template. For multimodule projects things can be easily extrapolated, but let me know if you would like to see one tutorial about that, and I'll make one in the future.


1. Clone the [jPOS template repository](https://github.com/jpos/jPOS-template):
    ```shell
    git clone https://github.com/jpos/jPOS-template.git
    ```
2. Open IntelliJ and go to File -> 
