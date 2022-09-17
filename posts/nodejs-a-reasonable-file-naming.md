---
title: A Reasonable File Naming For Your NodeJs Project
date: 2022-08-28
permalink: blog/2022/08/28/nodejs-a-reasonable-file-naming/
---

One might say that naming a variable can be the hardest thing in programming. But what about the function, file or
folder where the variable lives in? This post will tell you to apply the same naming schema for files and folders and
provides you with reasons why you should do it.

# A Reasonable File Naming For Your NodeJs Project

## TL;DR

```
1) Use always the same naming convention for files and folders (I prefer kebab case because I like kebab)
2) Use index.ts / index.js to declare a folder as a module
```

### Example: Do!

| Before Refactoring                                  | After Refactoring                                                                        | Change |
|:----------------------------------------------------|:-----------------------------------------------------------------------------------------|:------:|
| `some-module-to-refactor`.ts                        | `some-module-to-refactor`: <ul><li>index.ts</li><li>importantDomainFunction.ts</li></ul> |  yes   |
| import { Util } from './some-module-to-refactor.ts' | import { Util } from './some-module-to-refactor.ts'                                      |  `no`  |

### Example: Don't Do!

| Before Refactoring                               | After Refactoring                                                                      | Change |
|:-------------------------------------------------|:---------------------------------------------------------------------------------------|:------:|
| someModuleToRefactor.ts                          | some-module-to-refactor: <ul><li>index.ts</li><li>importantDomainFunction.ts</li></ul> |  yes   |
| import { Util } from './someModuleToRefactor.ts' | import { Util } from './some-module-to-refactor.ts'                                    | `yes`  |

## Introduction

As a programmer we often rely on naming conventions because as long as it works for everyone we are safe to proceed.
That is totally fine for me if the convention works in a sense it won't make me any troubles now or any
time in the future during development, refactoring or debugging[^1].

Working with NodeJs or typescript I can not say there is any single commonly accepted and reasonable way for naming
stuff. There are a lot and most of the conventions I came across have different schemas for naming files and folders.
Since this is a blog I do not go into the hassle for listening and referencing my sources. But I will provide you a
reasonable convention for naming files and folders in NodeJs. If you, dear reader, disagree with any of the presented
conventions
you most certainly disagree with my reasoning. That is totally fine, and I love having a discussion based on hard
reasons why we should do it differently.

## Motivation

I've spent quite a while the past four years looking for a reasonable way to name files and folders within node projects
I've encountered.
Coming from e.g. Java 8 you most certainly have learned how to name files and packages because the compiler would yell
to you if you would not comply.

In NodeJs there is no compiler that would tell you to write your file names like you named your classes. Maybe there are
linter for that, but you need to know the rule to enable it only for someone else to turn it off because he or she has a
different taste.

My point is, I do not want to discuss taste or style, I want to discuss how to avoid issues, enhancing
DX or productivity for everyone by a naming convention.

## Name Modules not Files or Folders

The title of this section already tells you we should name modules not files or folders. In the following I will
elaborate what I mean by naming modules and why this should be considered as a good reason. Yet, I let the naming schema
which you should apply open to your taste.

### Reason 1: Modules are Files and Folders

In NodeJs every file which exports something is considered a module. Every folder which may contain an index.js (
index.ts) is also considered a module.
This means a module may be a file or a folder. Since a file and a folder can be the same thing why should we apply
different naming conventions?

If this argument is not enough for you, allow me to provide more compelling reason for me.

### Reason 2: We do not think in Files or Folders

I keep this reason short because there are so many streams and terminologies for designing and architecture a service
that I don't want to touch with this simple topic of naming conventions.

This is something we easily can test for our self. If we model or explain how our software system is build, do we would
use the terms
files or folders? Are we importing from files in NodeJs?

### Reason 3: Lower Barries for Refactoring

When treating a file and a folder as a module, and they have the same naming pattern, then we can make a folder from a
file without changing any references to that file. Hence, we are able to refactor a file (module) into a folder (module)
to maybe split up concerns and see better what is going on. We also could make it a file again if we like to reduce
complexity and e.g. refactor stuff out into another module.

If you are interested in why I think this is important then please feel invited to keep reading. But be aware, I do not
have any short answer at the moment.

#### No additional changes when refactoring is more than a nice to have

At the first glance having the same name pattern for files and folders could be considered a nice to have, because if
we ___need___ to refactor from a file to a folder we
don't care about re-referencing to a folder.

There are not many reasons why someone would ask you to check out a project and start working on it. I can certainly
say that the number of times someone wants you to refactor something for the sake of readability or decreasing
complexity is quite low up to non-existent.

#### Refactorings must be kept small and cheap, so we can do it while developing features

Maybe I am biased but in an enterprise environment most people care about features (companies like to see value only in
features). The only reason most of the stakeholder would accept a "only-refactoring-task" is if a feature needs it or
feature
development takes longer because the evolution of business-needs/requirements exceeded the evolution of the code base.
The latter includes wasting too much time on maintenance/bugfixes etc.

Hence, we need to lower the barrier for refactorings during feature development. There must be a barrier because
otherwise we always would have perfect maintained code, wouldn't we?.

#### Be nice; A lot of changes in a PR increases the fear of missing something important

If the code base allows us to make changes without changing too much anything else with it, then we are able to
refactor[^2] side by side with a feature. Changes are problematic because with each change the
level of fear something will break increases, exponentially for me. I certainly would ___not___ risk ___unnecessarily___
that a PR[^3] becomes too big because of a lot of renaming noise. It would be too easy to miss something important if
there are more feature unrelated changes in a single PR.

I try to be a nice guy and do not want any of my coworkers to feel fear when I ask for a PR review. Thus, when renaming
a function or a variable to make it a little more expressive I find myself weighing the amount of changes in my commit
related to this renaming with the expected gain in readability.

#### Accept change due to different taste if it is insignificant to you

Most of the time the amount in changes due to renaming are small because there are maybe two or three occurrences. This
is a number I for myself would not raise any discussion in a PR unless it is wrong. Thus, I try avoiding potentially
wasting time on style discussions.

I accept easily that sometimes some wording bothers a dev when working with the code. But if the entire code base
changes this should be something we should have a discussion upfront.

#### Avoiding change leads to missing opportunities to make it better

As stated before, having a lot of changes is something I would weigh against the gain. The problem is, the gain may only
reveal itself, if we go through with the refactoring. We are consciously or unconsciously aware of this conflict as
a refactoring barrier. The energy to go into uncertain refactoring may wary by personality, outside temperature, the
taste of the coffee in the morning or any other factor which prevents us from refactoring. 

Most of the time, the result is that we let it be until the day comes we have time to pick the "only-refactoring-task".

## Conclusion

In ths blog post I tell you in many words that we should use the same naming convention for files and folders.
It is important to me because I do not want anyone to spend time arguing if we should write a folder in `kebab-case` a
file in `camelCase` if it includes a single function or in `PascalCase` if we export a class. We should consider what
convention will not stop us in generating value - Let us just name modules not files nor folders!

---

[^1] You may want to look up why e.g. the placement of the braces is also not only a matter of taste or
style [Dangerous implications of Allman style in JavaScript](https://splunktool.com/dangerous-implications-of-allman-style-in-javascript)

[^2] Refactoring includes for me renaming stuff, splitting up stuff, reimplementing stuff, deleting stuff.... actually
touching anything that was there before except those changes directly related to the new feature.

[^3] PR means Pull-Request (I apologize if the context was not clear enough)
