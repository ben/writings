> Tagline

> # Git is Simple

> Sometimes you actually do need to know how it works.

The way we use most computer systems is through a metaphor.

When you're using a word processor, you don't think about the byte-by-byte representation of each character, or the control sequences that determine which words are italic and which are bold-face.
The software abstracts that away, presenting a clean metaphor – inked characters on a sheet of paper.
You write your words, decide on their font and style, and when you're ready for your words to be on *actual* paper, you print.

When you're adjusting the exposure of a photo, you don't want think about the mathematical acrobatics necessary to change the R, G, and B bytes of every pixel *just so*, or the resampling algorithm needed to adjust the size or rotation.
The metaphor is something like a darkroom, and you adjust exposure and brightness, paint away red-eye, and erase the red wine stain on the wedding dress.

And with most version control systems, you don't want to know how the data is stored and retrieved, or how the bytes are ordered during a network transmission.
You just want to write your code, and every once in a while save a snapshot to someplace safe, so the metaphor is defined on that level.
The underlying data model is complicated, and you rarely (if ever) need to know what it is, because the UI is pretty effective at abstracting those details away.

With Git, the story is a little different.
The metaphor it presents is a directed acyclic graph (DAG) of commit nodes, which is to say there *is* no metaphor – the DAG actually is the data model.
If you try to re-use the metaphor you learned from another system, you're going to run into trouble.

The good news is that this data model is easy to understand, and that figuring it out will make you better at using Git.


## Objects

Just about everything in a git repository is either an **object**  or a **ref**.

Objects are how Git stores content.
They're stored in `.git/objects`, which is sometimes called the *object database* or *ODB*.
Objects in the ODB are immutable; once you create one, you can't change it.
This is because Git uses an object's SHA-1 hash to identify and find it, and if you were to change the content of an object, its hash would change.

Objects come in four flavors: blobs, trees, commits, and tag annotations.
**Blobs** are chunks of data that Git doesn't interpret with any kind of structure, and it's how Git stores your files.
Objects are actually pretty easy to inspect:

```
# Print the object's type
$ git cat-file -t d7abd6
blob

# Print the first 5 lines of the object's content
$ git cat-file -p d7abd6 | head -n 5
<!DOCTYPE html>
<!--[if IEMobile 7 ]><html class="no-js iem7"><![endif]-->
<!--[if lt IE 9]><html class="no-js lte-ie8"><![endif]-->
<!--[if (gt IE 8)|(gt IEMobile 7)|!(IEMobile)|!(IE)]><!--><html class="no-js" lang="en"><!--<![endif]-->
<head>
```

Note that there's no filename here.
Git expects file renames to be fairly common, and if the filename were embedded with the content, you'd have to keep lots of copies of that object where the only difference is the filename.

You use `git cat-file` because Git has optimized the storage of objects.
They're gzip-compressed, and sometimes strings lots of them together into big pack-files, so if you look in `.git/objects`, you may not see anything you recognize as an object.

The second type of object is called a **tree**, and it's how Git stores directory structures.

```
$ git cat-file -t 8f5b65
tree

$ git cat-file -p 8f5b65 | head -n 5
100644 blob 08b8e3400a81a79aeb42878171449b773ab493c0	after_footer.html
100644 blob 11517b315de6d7bc7550cc74ae413f1e6dafce19	archive_post.html
100644 blob 8ad5afd4581caa7458658325aeec9f8de875b988	article.html
040000 tree 5c2166adaa57c909182a45b995dfb750c22c8810	asides
040000 tree 52deb7c58d46aa09208c0b863fbecee81a2e3dad	custom
```

Only blobs are unstructured; Git expects a fairly specific format for all the others.
Each line of a tree object contains the file's permission flags, what type it is (`blob`s are files, `tree`s are subdirectories), the SHA-1 hash of the object, and a filename.
So the tree type is responsible for the names and locations of things, and the blob type is responsible for their contents.

The third type of object is a **commit**.
This is how Git represents a snapshot in history.

```
 $ git cat-file -t e365b1
commit

 $ git cat-file -p e365b1
tree 58c796e7717809c2ca2217fc5424fdebdbc121b1
parent d4291dfddfae86cfacec789133861098cebc67d4
author Ben Straub <bs@github.com> 1380719530 -0700
committer Ben Straub <bs@github.com> 1380719530 -0700

Fix typo, remove false statement
```

A commit has exactly one tree reference, which is the root directory of the commit.
It has zero or more parents, which are references to other commits, and it has some metadata about the commit – who made it, when it was made, and what it's about.

There's just one more type of object, and it's not used very often.
It's called a **tag annotation**, and it's used to make a tag with comments.

```
$ git cat-file -t 849a5e34a
tag

$ git cat-file -p 849a5e34a
object a65fedf39aefe402d3bb6e24df4d4f5fe4547750
type commit
tag hard_tag
tagger Ben Straub <bs@github.com> Fri May 11 11:47:58 2012 -0700

Tag on tag
```

I'll say more about how these work later; for now, just note the object SHA that's stored in there.

That's it!
You can count the number of object types on one hand!
See how simple this is?

## Refs

References (or *refs*) are nothing more than pointers to objects or other refs.
If a ref points to an object, it's called a *direct ref*; if it points to another ref, it's called a *symbolic ref*.

Most refs are direct.
To confirm this, check the contents of anything under `.git/refs/heads`; they're all plain-text files whose content is the SHA hash of the commit they point to.

```
$ cat .git/refs/heads/master
2b67270f960563c55dd6c66495517bccc4f7fb17
```

Git also keeps around a few refs for specific purposes.
The most commonly useful one is `HEAD`, which usually points to the branch you have checked out:

```
$ cat HEAD
ref: refs/heads/master
```


## Three Trees

Knowing these things can make some of Git's commands make a lot more sense.

***TODO***


## Tying it all together

***TODO***
