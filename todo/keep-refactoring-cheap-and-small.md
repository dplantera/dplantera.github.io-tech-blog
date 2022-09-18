
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

Hence, we need to lower the barrier for refactorings during feature development.

#### Be nice; A lot of changes in a PR increases the fear of missing something important

If the code base allows us to make changes without changing too much anything else with it, then we are able to
refactor[^1] side by side with a feature. Changes are problematic because with each change the
level of fear something will break increases, exponentially for me. I certainly would ___not___ risk ___unnecessarily___
that a PR[^2] becomes too big because of a lot of renaming noise. It would be too easy to miss something important if
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

___

[^1]: Refactoring includes for me renaming stuff, splitting up stuff, reimplementing stuff, deleting stuff.... actually
touching anything that was there before except those changes directly related to the new feature.

[^2]: PR means Pull-Request (I apologize if the context was not clear enough)