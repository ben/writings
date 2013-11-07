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

The good news is that this data model is easy to understand.


## Objects and Refs

Just about everything in a git repository is either an **object** (in `.git/objects`) or a **ref** (in `.git/refs`).

Objects come in four flavors: blobs, trees, commits, and tag annotations.


## Three Trees

***TODO***

## Tying it all together

***TODO***
