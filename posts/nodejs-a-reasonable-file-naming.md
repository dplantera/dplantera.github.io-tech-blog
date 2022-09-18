---
title: A Reasonable File Naming For Your NodeJs Project
date: 2022-09-18
permalink: blog/2022/08/28/nodejs-a-reasonable-file-naming/
---

One might say that naming a variable can be the hardest thing in programming. But what about the file or
folder where the variable lives in? This post will meet us halfway by providing reasons why we should have the same
naming schema for files and folders.
___

## TL;DR

> 1) Use always the same naming convention for files and folders. I prefer kebab case because I like kebab, but you may
     use whatever you see fit
> 2) If we refactor a file to a folder while keeping the same name, we induce no additional changes (technical reason)

### Do!

| Before Refactoring                                  | After Refactoring                                                                                                   |               Change                |
|:----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------|:-----------------------------------:|
| some-module-to-refactor.ts                          | <span style="color:green">some-module-to-refactor</span>: <ul><li>index.ts</li><li>important-functions.ts</li></ul> |                 yes                 |
| import { Util } from './some-module-to-refactor.ts' | import { Util } from './<span style="color:green">some-module-to-refactor</span>.ts'                                | <span style="color:green">no</span> |

### Don't Do!

| Before Refactoring                               | After Refactoring                                                                                                     |              Change               |
|:-------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------|:---------------------------------:|
| someModuleToRefactor                             | <span style="color:red">some-module-to-refactor</span>: <ul><li>index.ts</li><li>importantDomainFunction.ts</li></ul> |                yes                |
| import { Util } from './someModuleToRefactor.ts' | import { Util } from './<span style="color:red">some-module-to-refactor</span>.ts'                                    | <span style="color:red">no</span> |

___

## Introduction

As a programmer we often rely on naming conventions because as long as it works for everyone we are safe to proceed.
That is totally fine if the convention works in a sense it won't make any troubles now or any
time in the future during development, refactoring or debugging. Hence, a convention must not have a negative impact
from a practical or technical perspective like readability (practical) or in Vanilla JS e.g. the placement of braces (
technical) [^1].

I've worked about four years with NodeJs projects and wondered often if there is a technical reason in how
we name files and folders. Until now, I can not say there actually is any single commonly accepted way for
naming stuff in NodeJs projects. There are a lot and most of them relate to practical reasons like "filename will
express...something"  e.g. "whether there is a class or a function in a file exported". Yet, I haven't found any
technical reason why we should apply a particular naming convention on files or folders.[^2]

Coming from e.g. Java 8 you most certainly have learned that you need to name a file like you've named your class inside
the file because the compiler would yell to you otherwise.
In NodeJs there is no compiler that enforces naming conventions. Maybe there are linter for that, but you need to know
the rule and enable it only for someone else to turn it off because he or she has a different taste.

My point is, I do not want to discuss taste or style, I want to discuss how to avoid issues, enhancing
DX or productivity for everyone by providing a naming convention through technical reasoning.[^3]

## Name Modules not Files or Folders

The title of this section already tells us we should name modules not files or folders. I'm convinced that if we start
naming modules instead of files or folders then we will take a step towards a natural naming convention and style.

In the following I will elaborate what I mean by naming modules and why this should be considered as a good reason. Yet,
I let the naming schema which you should apply open to your taste, for now.

### Reason 1: Modules are Files and Folders

In NodeJs every file which exports something is considered a module. Every folder which may contain an index.js (
index.ts) is also considered a module. This means a module may be a file or a folder.

Since a file and a folder can be the same thing why should we apply different naming conventions?

### Reason 2: We do not think in Files or Folders

I keep this reason short because there are so many streams and terminologies for designing and architecture a service
that I don't want to touch with this simple topic of naming conventions. This is something we easily can test for our
self.

If we model or explain how our software system is build, do we would use the terms files or folders? Are we importing
from files in NodeJs?

### Reason 3: Lower Barries for Refactoring

When treating a file and a folder as a module, and they have the same naming pattern, then we can make a folder from a
file without changing any references to that file.
Hence, we are able to refactor a file (module) into a folder (module) without inducing any other change in the project.

[The absence of additional changes lowers the barrier for refactoring and enables us in return to increase the quality
of a module and with it of the system.](#Technical and practical impact of different naming conventions for files and folders)

## Impact of different naming conventions for files and folders

Reason 3 stated that naming files and folders the same way, will lower barriers for refactoring by reducing the amount
of induced additional change. Let us define additional change as any change which would be a side
effect of the actively applied change like updating file references.
However, it does not state why additional change can become such an issue that we would avoid a refactoring.

We have certainly come across a file which is large enough where we can't see anymore what actually happens, but not
large enough, anyone would have an issue if we extend it with another function. Be honest, we both would just put the
function in the file. And it makes sense in a first iteration to do so, the question is what happens next? Would we
refactor it? What would hold us from it?

There are *barriers for refactoring* and one of it is *additional change*. If we change a filename then in some cases we
change the whole project because of updated file references some refactoring tool applied for us. Most certainly we
would work on a feature or bug when we think of refactoring. If we induce changes across the project which are totally
unrelated to the thing we are actually doing, we consciously or unconsciously tend not to go through with the
refactoring. Of course this would not matter if a refactoring would become necessary to implement or fix something but
maybe raise some reflections why this become necessary.

Yet, refactoring is one of the most important tasks in programming and general speaking in writing. There is no
masterpiece text when an author writes it down the first time. A masterpiece text is developed mostly by
multiple people. Thus, refactoring enables us to achieve masterpieces or at least better and reliable software.

But refactoring is a skill which is, thanks to our tools, cheap and easy to apply but really hard to master. What is
hard to learn is to know when it will be necessary, when it will bring as value or the hardest thing ever: when to stop.
We can not force achieving mastery in refactoring, but we can and should eliminate every barrier which will prevent us
from refactoring. Anything preventing us from refactoring would prevent us from writing better software.

Modularize and split up concerns in a folder to see better what is going on in a file is a refactoring worth to proceed.
This enables us e.g. to refactor out concerns in another module and reduce complexity which again enables us to see
better what's going, refactor out and so on and so forth.

If the file and the folder can have the same name since they are modules then we actually can go from file to folder
back and forth in
multiple commits while only changing the module and never the project.

## Conclusion

In ths blog post I tell you in many words that we should use the same naming convention for files and folders. We do not
think in files and folders, so we should not name them us such, we should name modules. In NodeJs we work with modules
and modules have the semantic feature that they could be exchanged. We should transport this feature when writing our
projects. We should be able to exchange a file module with a folder module without changing anything else in the
project.

Let us not waste time whether we should write a folder in `kebab-case` and a file in `camelCase` or `PascalCase`. Let us
not prevent us from refactoring and from writing better software. Let us just name modules not files nor folders!

---

[^1]: You may want to look up why e.g. the placement of the braces is also not only a matter of taste or
style [Dangerous implications of Allman style in JavaScript](https://splunktool.com/dangerous-implications-of-allman-style-in-javascript)

[^2]: Since this is a blog I do not go into the hassle for listening and referencing my sources. But I will provide you
with reasonable statements we can discuss in a productive way and maybe exchange sources if we see fit. And yes, I was
lazy when writing this blog because I did not want start spending more time with searching the sources I have in mind
than actually writing.

[^3]: If you, dear reader, disagree with the presented convention you most certainly disagree with my reasoning. That is
totally fine, and I love having a discussion based on hard reasons why we should do it differently.

