# README

This is @jacegu trying jj for the first time.


## Learning sources

### Written
- [The official jj documentatiion](https://martinvonz.github.io/jj/latest/)
- [The tutorial that Steve Klabnk wrote](https://steveklabnik.github.io/jujutsu-tutorial/sharing-code/remotes.html)
- [A random article I found online](https://reasonablypolymorphic.com/blog/jj-strategy/index.html)

### Talks
- [Intro to jj in the GitButler's Bits and Booze podcast](https://www.youtube.com/watch?v=dwyMlLYIrPk)
- [Presentation about jj by its author at Git Merge 2022](https://www.youtube.com/watch?v=bx_LGilOuE4)


## Basic stuff

This will be more or less a journey from Git to jj, and trying to puzzle things together.


## First steps

### Cloning a repo
I actually created this repo from GitHub directly, then I wanted to clone it:

```
jj git clone git@github.com:jacegu/learnjj.git
```

### Seeing the log

I feel blind without this:
```
jj log
```

I actually set the config to jj without modifiers to become just an alias to jj log; And now I just do `jj`.
```
jj config set --user ui.default-command log
```

As I was testing this with a bigger repo (I cloned it using `jj git clone`), I was surprised bythe time it took (8 minutes!!), but also by the fact that the log wasn't showing anything, only the HEAD. In order to see everything I had to run `jj log -r 'all()'`.

There are a couple things that I want to research here:
1. There is a way to customize the structure of the log via templates. I will likely want more or less information in there in the future. For now, I want to try to get used to the default flavor.
2. The -r option I discussed earlier actually has a default, that explains the behavior it has by default. I would like to understand how to tweak it.






### Pulling 

#### How to pull `main` (or `trunk` or _whatever_)

> [!TIP]
> The relevant tip for this can be found [in the jj docs](https://martinvonz.github.io/jj/latest/github/#updating-the-repository).
> There is also a useful reference to a GitHub issue that may change how this is done.

In `jj` there is no equivanet to `git pull`. Instead we need to do 2 things:

1. Fetch the changes:
    ```
    jj git fetch
    ```
    This is the equivalent to `git fetch`. If you are anything like me, you will most likely want to do it for only for the branch you care about
    ```
    jj git fetch -b main
    ```
2. Once you have the updated information about the `main` branch at the origin, you need to rebase your current branch on top of your local `main` "reference" (a.k.a _bookmark_).
    ```
    jj rebase -d main
    ```

    <details>
    <summary>🤨 How does this even work?</summary>
    
    The best way to understand this is to read through `jj rebase --help`.
    The important bits here are:
    - When not specifying `-b`, `-s` or `-r`, which defines the _what_ to rebase, the default value is `-b @`, which is the current branch.
    - In the context of rebase, the _branch_ of a commit is, the commit iself, its descendants, and all the ancestors it doesn't have in common with the destination commit.
    </details>


### Pushing


### Branching

Coming from Git-defined workflow, branching is just how everything starts.


### Committing


### Rewriting history



## Advanced stuff

### Signing commits
Follow: https://github.com/martinvonz/jj/blob/main/docs/config.md#commit-signing

### Revset language
See: https://martinvonz.github.io/jj/latest/revsets/
