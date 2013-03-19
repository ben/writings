Travel through time with Git

# Rewrite Your History

Of all the things git can do, perhaps one of the most feared is the ability to rewrite history. For a user of any other source control system, where the history is permanent and immutable, this can induce vertigo and a sense of existential dread.

So take a breath, make a cup of tea, and let's figure this thing out.

## Wait, why do I want this?

If you've spent much time working on version-controlled software, you've probably been in at least one of these situations:

* Working on a branch, but ended up with two separate features.
* Forgot to delete a file, and had a two-line bug fix spread across 3 commits.
* Made a new commit that only undoes some changes from a previous commit, and suffered ridicule from it.
* Started a bugfix that later turned into a deploy-it-yesterday hotfix.

Rebase can help you solve these problems, or avoid them entirely. It's much like using a word processor instead of a typewriter - you can edit your story and fix problems before you print it.

## A trivial example

It's easiest to think about this by seeing it in action. In this example, the `master` branch (which is shared with the whole team) and your own personal `experiment` branch have diverged somewhat.

![](trivial-1a.png)

Rebase allows you to merge the `experiment` branch without the use of a merge commit. Let's remake this into a form where a fast-forward merge is possible. Here are the command lines to make that happen:

```sh
$ git checkout experiment
$ git rebase master
```

![](trivial-1b.png)

The `c` and `d` commits have been re-made in such a way that the same changes are applied on top of `f`, instead of the old `b` commit where they were. 

## A slightly less trivial example

Let's say you were working on a bug fix, but it turns out to be more urgent than you at first thought. Here's what your history looks like right now:

![](trivial-2a.png)

You started your fix on the `development` branch, where all new feature work goes. Meanwhile, the `deployed` branch has had a couple of small fixes applied to it. Now you discover that your fix corrects a critical issue, and you want to deploy it to production, but without all the half-finished features in `b` and `c`. Rebase to the rescue!

```sh
$ git checkout fix
$ git rebase --onto deployed development fix
```

![](trivial-2b.png)

This takes everything between `development` and `fix` and replays the changes starting at `deployed`.

## WTF is this thing doing?

* series of cherry-picks

## Conflicts

* what to do with 

## Interactivity

* script git runs to make new history

## A Non-trivial Example

* Split a branch into two

## It's really not that scary

The biggest worry people have when they learn about this feature is that they'll screw up. Relax; it's going to be fine.

Nothing is ever truly lost in a Git repository. History is built out of commits (which are immutable) and refs or branches (which change all the time). Almost every invocation of rebase will move a branch around, but the underlying commits are still in the repository.

Try this in any repository you've been working in:

	$ git reflog my_feature

You'll see a listing of every commit that branch has pointed to.  There's an entry in the log for every commit you've ever made on your machine. It'll look something like this:

	8672898 my_feature@{0}: rebase finished: refs/heads/my_feature onto <sha>
	7b47989 my_feature@{1}: commit: Fixed #7294
	c6cb71b my_feature@{2}: clone: from http://url.to/origin/repo

The lines are in reverse date order; newest at the top. The first line shows the most recent change: a rebase. Undoing the rebase is usually as simple as this:

	$ git reset --hard my_feature@{1}

Also keep in mind that all of these operations **only affect the repository on your machine.** None of the changes rebase is making are shared with anybody else until you decide to share them.

## Publicity

* diagram showing which commits are safe to change
* what to do if you rewrite shared history

## Fin

* wrap up
* references