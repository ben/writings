Travel through time with Git

# Rewrite Your History

Of all the things git can do, perhaps one of the most feared is the ability to rewrite history. For a user of any other source control system, where the history is permanent and immutable, this can induce vertigo and a sense of existential dread.

So take a breath, make a cup of tea, and let's figure this thing out.

## Wait, why do I want this?

If you've spent much time working on version-controlled software, you've probably been in at least one of these situations:

* Working on a branch, but ended up with two separate features.
* Forgot to delete a file, and had a two-line bug fix spread across 3 commits.
* Made a new commit that only undoes some changes from a previous commit, and suffered ridicule from it.

Rebase can help you solve these problems, or avoid them entirely. It's much like using a word processor instead of a typewriter - you can edit your story and fix problems before you print it.

## It's really not that scary

Let's get this out of the way early: nothing is ever truly lost in a Git repository. History is built out of commits (which are immutable) and refs or branches (which change all the time). Almost every invocation of rebase will move a branch around, but the underlying commits are still in the repository.

Try this in any repository you've been working in:

	$ git reflog my_feature

You'll see a listing of every commit that branch has pointed to.  There's an entry in the log for every commit you've ever made on your machine. It'll look something like this:

	8672898 my_feature@{0}: rebase finished: refs/heads/my_feature onto <sha>
	7b47989 my_feature@{1}: commit: Fixed #7294
	c6cb71b my_feature@{2}: clone: from http://url.to/origin/repo

The lines are in reverse date order; newest at the top. The first line shows the most recent change: a rebase. Undoing the rebase is usually as simple as this:

	$ git reset --hard my_feature@{1}

Also keep in mind that all of these operations **only affect the repository on your machine.** None of the changes rebase is making are shared with anybody else until you decide to share them.

## A Trivial Example

* diagrams for simple rebase

## WTF is it doing?

* series of cherry-picks

## Conflicts

* what to do with 

## Interactivity

* script git runs to make new history

## A Non-trivial Example

* Split a branch into two

## Publicity

* diagram showing which commits are safe to change
* what to do if you rewrite shared history

## Fin

* wrap up
* references