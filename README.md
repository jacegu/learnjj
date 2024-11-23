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

By default `jj` shows quite a short log. If you want the full picture you have to:
```
jj log -r 'all()'
```
Then you can further trim this with the number of revisions you want to see (20 in this example):
```
jj log -r 'all()' -n 20
```

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
