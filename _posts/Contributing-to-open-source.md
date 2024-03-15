---
layout: post
title: 10 Easy ways to contribute to open source projects, with real life examples.
# noinspection SpellCheckingInspection
coverimage: ../img/opensource-contributing.png
---

[//]: # (![]&#40;../img/opensource-contributing.png&#41;)

You can contribute to an open source project you like in many ways, and some of them are rather easy.
While you do that,
you will be gaining knowledge that will let you learn more about it and contribute even more advanced stuff.
On top of that, you are getting involved, getting known by the project staff, and eventually by its users.

In this article, you will find 10 easy ways to contribute to open source projects.
You will find real world examples for most of them,
including the process and motivations that lead to them, so you can see how easy it could be to contribute.
As you will see, you only need to be willing and pay attention.   


## 1. Documentation Improvements
Review and update project documentation, fix typos, clarify instructions, or add examples to make it more accessible and user-friendly.

When you browse your favorite open source project documentation, you will most probably find things that you don't understand at first, instructions that don't work out of the box, etc. You can help a lot just by saying how things would have been easier for you, provide clear alternatives, explaining things the way you would have liked they were explained to you.

In [this case](https://github.com/jpos/jPOS/pull/467) I documented a feature that wasn't in the documentation, all I had to do is traverse the source files, extract the examples and add some explanations. It's an awesome way to learn and understand the code.

## 2. Bug Reporting. <a id="bug-reporting" /> 
When you use the project's software, you will probably find bugs. Report them on the project's issue tracker. Include clear steps to reproduce the problems and any relevant details.

Some months ago, while working on a gradle project I created using the recommended steps,
I found out that the gradle bash completion was not working for projects created using those steps.
In [this issue](https://github.com/gradle/gradle-completion/issues/137) I reported that,
specifying how to reproduce the problem.

Specifying a clear way to reproduce some bug often takes some work, and it may be tedious,
but it is not so difficult, you just need to be patient.
Be sure you can reproduce the problem with those instructions starting from scratch,
and if possible validate they don't depend on the peculiarities of your system, and if they do, be specific about them. 


## 3. Issue Triage. <a name="issue-triage"></a>  
If you are out of ideas, you can always help the project maintainers by triaging existing issues. Go to the project issue tracker and verify reported issues, add any additional information you think it may help, and vote for prioritization. 

[Here](https://github.com/jpos/jPOS/issues/535#issuecomment-1539188264) I triaged an issue from a developer eager to contribute, which is awesome, but the question belonged to another place. If you traverse the issues of your favorite open source projects, it's most likely you will find some of these. Otherwise, just subscribe to receive notifications in your inbox, and sooner or later you will find some way to help.

## 4. Translations.
If you are a native speaker of a supported language, you most probably will find quirks in some places; it is usually easy to find where to contribute the necessary edit, if not, just ask for directions to the maintainer. If you are feeling bold, you could even start the translation to your language if it's not yet supported, this helps make the software accessible to a broader audience.

Translation errors are common when the same sentence has two different meanings depending on context.
One common case is verbs that can be also nouns.
At least in spanish translations is common to find labels like `Test class` incorrectly translated. 
Earlier this year I was configuring a gnome extension
I use to interact with the phone from my laptop and viceversa through [KDE connect](https://kdeconnect.kde.org/),
(it's called [GSConnect](https://extensions.gnome.org/extension/1319/gsconnect/), and it's awesome BTW)
when I found this common error for the label `Share Notifications`, so I went to the project page on GitHub,
searched for the spanish string, checked the message ID was only used with that meaning, 
and made this [pull request](https://github.com/GSConnect/gnome-shell-extension-gsconnect/pull/1561#event-8434291116) to fix it,
easy enough isn't it?

## 5. User Support
Join as many support channels as you can.
Help other users by answering questions on forums, mailing lists, stack overflow, or community chat channels.
Share your knowledge, help troubleshoot problems, and guide users through the project's features.
You will learn a lot, just by trying to help,
on top of that, being helpful will do wonders for your reputation within the community.

Just the other day I received an email (remember what I said about subscribing in [Issue triage](#issue-triage)?) with [this question](https://groups.google.com/g/jpos-users/c/ZHTk7mDKLy4) in the jPOS users mailing list,
and tried to help, as you could see there, you don't even have to be right in the first answer for being helpful.

## 6. Testing and Quality Assurance.
Help improve the project's quality by testing new features,
verifying bug fixes, performing regression testing and suggesting better ways to do things on others' contributions.
Report your findings and provide feedback to ensure the software functions as expected.
Remember to subscribe to the project's repository for notifications on pull requests and issues.

 Sadly, I don't have a concrete example of something I found doing this,
 but it is as easy as trying the new features or run the tests in your environment,
 and if you find some issue while at it, or enhancement proposal, do as in [Bug reporting](#bug-reporting) or [Feature suggestions](#feature-suggestions).

## 7. Code Reviews.

Participate in code reviews by examining and providing feedback on proposed changes or pull requests. Offer suggestions for improvement, identify potential issues, and help maintain code quality.

Following open source projects is a great way to learn, not only about the project itself, but about the stuff it uses.

Sometimes you don't have specific use cases, but you're interested in the potential applications of some technology.
I'm particularly interested in the java project loom (virtual threads),
but I still don't use the necessary jdk version in any of my projects, I'm waiting for the next LTS jdk to be available.

That's why
when I saw [a group of commits](https://github.com/jpos/jPOS/compare/cfb71f683e00...6f1d870205d6) in the jPOS Slack channel for repository notifications
mentioning virtual threads, I clicked right away to learn more and gain insights into their implementation.

While I was there, I saw some part of the code of [one of the commits](https://github.com/jpos/jPOS/commit/2fb4fac06f76b16ebef95a699ba1a96271024f2c) that felt weird and went investigating.
And in fact,
there was an issue
that I mentioned in a comment to [the respective commit](https://github.com/jpos/jPOS/commit/2fb4fac06f76b16ebef95a699ba1a96271024f2c).
That comment [triggered a fix](https://github.com/jpos/jPOS/commit/77b7bb68c887237eb9ef54e54bfc48ae941470be) from the author,
which is the project founder.


So, you not only can learn just by trying to help, but it also works the other way around!


## 8. Feature Suggestions. <a id="feature-suggestions" />

Many times,
you will find that the software would be a lot easier to use
if only it had some features you miss or things that could be better from a usage perspective,
propose them to the project.
Discuss the ideas with the community, provide use cases, and outline potential implementation approaches.
Try to use the proper channels recommended by the project maintainers for that.
If it's hard to find out the proper channels, you have an enhancement proposal for [the documentation](#1-documentation-improvements) right there!

Sometimes you just need a little modification in a framework or library to make your developer life a little easier.
One of many [jPOS](https://www.jpos.org) awesome features is the ability to load properties from many files,
but you don't specify the complete file path, just the name without the extension, it's called an environment actually,
you can give it a directory to where to look for the properties files, and you can even load many of those.
What you can't do is to give it a list of directories, kind of system path,
so you'd need a little hack to load properties files from different paths.
That is why
I proposed that feature in [this GitHub discussion of the project](https://github.com/jpos/jPOS/discussions/525).
This is not a success story yet,
but at some point I intend to provide an implementation of the feature and hope for the approval.

You can use those properties to replace some parts of the configuration files with their values.
Another problem that bothers me is that property substitution isn't supported everywhere.
For some kind of configurations,
each component must implement the property substitution by calling a method that does that,
if you don't control the source of a component not implementing this feature, for instance because it is a built-in component,
and you don't want to fork the project only for that.
You just can't use that feature with that component.
So I went out and wrote a [feature request](https://github.com/jpos/jPOS/discussions/546) for supporting it in more places,
with a proof of concept implementation and usage example.

## 9. Design and UI/UX Contributions.

Contribute to the project's design and user experience by suggesting visual improvements, creating mockups, or providing UI/UX feedback.

Since I don't do much of UI/UX, I couldn't find a personal story to share here,
but if you are a more visual person, then you most certainly can suggest visual improvements.
Also, UX is not only about visual aspects, you can propose workflow enhancements as well.

## 10. Spreading the Word.

If you like the project, who better than you for talking about all the goodness it has. Help promote the project by writing blog posts, sharing your success stories, or presenting at conferences or meetups. Spread awareness and encourage others to use and contribute to the open source project.

Well, in this same post, I have spread the word about some of the projects I like and use,
it's not a perfect example since it was not the main subject of it, but who says we can't do many things at once?

So, if you are still here, thank you for reading,
and I hope you feel more comfortable about contributing, and that you will be doing so soon enough.

If you want to reach out, just head over to [<img src="../img/linkedin.svg" width="16" style="vertical-align:middle">/andresalcarraz](https://linkedin.com/in/andresalcarraz) or [<img src="../img/twitter.svg" width="16" style="vertical-align:middle">/andresalcarraz](https://twitter.com/andresalcarraz).

