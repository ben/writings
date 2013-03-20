> Travel through time with Git

> # Rewrite Your History

> Of all the things Git can do, perhaps one of the most feared is the ability to rewrite history.

The amount of control git gives to its users is what sets it apart from other version-control systems.
Users of other systems, where history is permanent and immutable, generally have two reactions upon discovering the `rebase` command: horror that such a thing even exists, and confusion as to why anyone would want to use it.

## Why do I want this?

If you've spent much time working on version-controlled software, you've probably been in at least one of these situations:

* You're working on a branch, but find yourself with two chunks of work that should be separate.
* You forgot to delete a file, and a two-line bug fix got spread across 3 commits.
* You had to make a new commit that *only* undoes some changes introduced in a previous commit. 
* You started working on a bugfix, which later turned into a deploy-it-yesterday hotfix.

Rebase can help you solve these problems, or avoid them entirely.
It's much like using a word processor instead of a typewriter - you can edit your story and fix problems before sending it to the printer.

It's easiest to think about this by seeing it in action.
Let's look at a couple of examples.

## A trivial example

In this example, the `master` branch (which is shared with the whole team) and your own personal `experiment` branch (which only exists on your machine) have diverged somewhat.

![](trivial-1a.png)

*[Diagram key: green blocks are commits. 
The arrows indicate "parenthood", so they point backward in time. The gray blocks are branch refs.]*

Let's suppose that, for whatever reason, you don't want a merge commit.
Rebase allows you to bring in the changes from the `experiment` branch while keeping the history linear.
Here's how to make that happen:

```sh
$ git checkout experiment
$ git rebase master
```

And here's what the history looks like afterward:

![](trivial-1b.png)

It helps to think of commits as **a collection of changes** rather than a collection of fixed versions. 
The `c` and `d` commits have been re-made as `c'` and `d'`, such that their *changes* are applied on top of `f`.

## A slightly less trivial example

Let's say you were working on a bug fix, but it turns out to be more urgent than you first thought.
Here's what your history looks like right now:

![](trivial-2a.png)

You started your fix on the `development` branch, where all new feature work goes.
Meanwhile, the `deployed` branch has had a couple of small fixes applied to it.
Now you discover that your fix corrects a critical issue, and you want to deploy it to production, but without all the half-finished features in `b` and `c`.
Rebase to the rescue!

```sh
$ git rebase --onto deployed development fix
```

The syntax is `--onto <new-base> <old-base> <end>`.
In this case, we want all the changes starting after `development` and ending with `fix` to be replayed on top of `deployed`.
Here's what it looks like afterward:

![](trivial-2b.png)


## Conflicts

At its heart, rebase is a merge operation.
The bad news is that there will occasionally be conflicts.
The good news is that rebase actually makes it easier to deal with them!

When you do a standard `git merge`, you have to resolve all the conflicts at once.
If your branches have been separated for a while, this can be a daunting task; dozens of conflicts, scattered through your codebase, only some of which are related to each other.
It's the kind of situation that make developers run screaming from their keyboards.

Rebase, on the other hand, applies commits one at a time.
If any of them conflict, you get to review them as they're happening, and take corrective action.
Let's take a look at 

* the specific changes that conflicted, a commit message that describes what was changed and why, and the names of the people who made the conflicting changes in the first place  

* what to do with 

## Interactivity

* script git runs to make new history

## A Non-trivial Example

* Split a branch into two

## It's really not that scary

The biggest worry people have when they learn about this feature is that they'll screw up.
Relax; it's going to be fine.
Looking at the diagrams above, you may notice that the original commits aren't gone, they're just harder to see. 

Nothing is ever truly lost in a Git repository.
History is built out of commits (which are immutable) and refs or branches (which change all the time).
Almost every invocation of rebase will move a branch around, but the underlying commits are still in the repository.

Try this in any repository you've been working in:

	$ git reflog my_feature

You'll see a listing of every commit that branch has pointed to.
There's an entry in the log for every commit you've ever made on your machine.
It'll look something like this:

	8672898 my_feature@{0}: rebase finished: refs/heads/my_feature onto <sha>
	7b47989 my_feature@{1}: commit: Fixed #7294
	c6cb71b my_feature@{2}: clone: from http://url.to/origin/repo

The lines are in reverse date order; newest at the top.
The first line shows the most recent change: a rebase.
Undoing the rebase is usually as simple as this:

	$ git reset --hard my_feature@{1}

Also keep in mind that all of these operations **only affect the repository on your machine.**
None of the changes rebase is making are shared with anybody else until you decide to share them.

## Publicity

One simple guideline will save you from worlds of pain: **only change history that has never left your machine.**
Git helps you with this; if your rebase changes history that exists in the origin, it helpfully warns you:

	$ git push
	To https://url.to/origin/repo
	 ! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'url.to/origin/repo'

A simple `git push -f` will get around this, but the warning should be enough to keep people from making mistakes.

There are some situations where you *do* want to change history that exists elsewhere, though.
One example is the removal of sensitive information from a repository.
Let's walk through an example.
Here's some simple history

![](public-a.png)

Let's say that the `c` commit contains a server password, and simply adding a new commit that deletes that line won't do, you want it to have never existed in the repository.
So you do an interactive rebase, and remove the offending commit:

![](public-b.png)

You make sure the new history is pushed to origin:

	$ git push -f origin master

And you send an email to your team, explaining what happened, and what they should do to adjust.
Your teammate Jill has some work that was based off of the old `master`:

![](public-c.png)

Here's what it looks like after a `git fetch`:

![](public-d.png)

So she has to rebase her work onto the new head of the history:

	$ git rebase --onto origin/master master~ master

![](public-e.png)

Your teammate Bill has it easier; his `master` doesn't have any extra commits on it:

![](public-f.png)

So all he has to do is change his `master` branch to point to `origin/master`:

	$ git reset --hard origin/master

![](public-g.png)

## Fin

* wrap up
* references