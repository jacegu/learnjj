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

    The best way to understand this is to read through [`jj rebase --help`](https://martinvonz.github.io/jj/latest/cli-reference/#jj-rebase).
    The important bits here are:
    - When not specifying `-b`, `-s` or `-r`, which defines the _what_ to rebase, the default value is `-b @`, which is the current branch.
    - In the context of rebase, the _branch_ of a commit is, the commit iself, its descendants, and all the ancestors it doesn't have in common with the destination commit.
    </details>


### Branching

#### Creating branches

Coming from a Git workflow, branching is just how everything starts. In `jj` there is no such things as branches. You can have revisions that share a parent, which effectively creates a branch. To create a new branch you just need to create a new revision specifying what the parent is, using:
```
jj new <PARENT-REVISION>
```

<details>
<summary>🤨 How does this even work?</summary>

##### What's a branch, anyway?

A branch happens when you have 2 commits with the same immediate ancestor.
```
 B C
 |/
 A
```

In Jujutsu, when you create a new commit you provide the parent you want for it. If you don't specify anything the ancestor will always be your working copy. 

```
jj new
```

Provided this is your current status (`B` is your working copy):
```
@B
 |
 A
```

When you `jj new`, you will end up with:
```
@C
 |
 B
 |
 A
```

However, you can provide the parent you want:

```
jj new A
```

Then you would end up with a branch:

```
B @C
|/
A
```
</details>

#### Naming branches

Every revision has an identifier, that is not different in `jj`. When you want to give a human-friendly name to a revision you use [bookmarks](https://martinvonz.github.io/jj/latest/bookmarks/). This is how you name your branches in Jujutsu.

> [!IMPORTANT]  
> The most important difference to know right off the bat is that, where Git will advance the branch reference as you add commits to it, Jujutsu will not. You boomark will stay wherever you defined it. You are responsible for moving it to the revision you want it to point to _explicitly_. 

To bookmark a revision (you can also read this as _giving a giving a revision a name_), you do:
```
jj boomark set <NAME>
```
<details>
<summary>🤨 How does this even work?</summary>

By default, this will set the bookmark to your working copy. You can also provide an specific revision when doing it. Note that if the revision you are targeting is not a child of the revision the bookmark currently points to, you will have to provide the extra parameter `-B` or `--allow-backwards`. 

Note that if the bookmark already exists, what we are doing here is moving the bookmark to a different revision.

See [`jj bookmark set --help`](https://martinvonz.github.io/jj/latest/cli-reference/#jj-bookmark-set) for more details.
</details> 


### Committing


### Pushing

Pushing changes to an origin will require two things:

1. Updating the bookmark to the revision that you want to push to the remote (remember: `jj` won't advance bookmarks as you add changes). Assuming that you want to push all the changes in your working copy:
    ```
    jj boomark set the-boomark-name
    ```
    <details>
    <summary>🤨 How does this even work?</summary>
    
    What you are doing here is updating a bookmark to point to your working copy. Alternatively, you could specify a revision for your bookmark by running `jj bookmark set the-bookmark-name -r <REVISION>`. See [`jj bookmark --help`](https://martinvonz.github.io/jj/latest/cli-reference/#jj-bookmark) for more information on this.
    </details>
2. Push a-la-git.
    ```
    jj git push -b the-bookmark-name
    ```
    If you don't specify the bookmark, you will push all the bookmarks in the ancestors of your working copy. 

    <details>
    <summary>🤨 How does this even work?</summary>
    
    To better understand how this works see [`jj git push --help`](https://martinvonz.github.io/jj/latest/cli-reference/#jj-git-push).
    </details>






### Rewriting history



## Advanced stuff

### Signing commits
Follow: https://github.com/martinvonz/jj/blob/main/docs/config.md#commit-signing

### Revset language
See: https://martinvonz.github.io/jj/latest/revsets/
