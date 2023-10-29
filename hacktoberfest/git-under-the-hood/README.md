# Git Under the Hood: A Tutorial
Have you ever wondered what happens behind the scenes when you use Git commands? In this tutorial, we'll explore the inner workings of Git by examining its object storage, commit structures, and more. We'll use shell commands to dive deep into `.git` directories to see how Git stores your data.

## Pre-requisites

- Basic understanding of Git commands
- Familiarity with Unix/Linux shell
- Some experience with Vim or another text editor

## Setting Up

For this tutorial, make sure you're inside a Git repository. If you don't have one, you can create a new repo as follows:

```bash
mkdir git-under-the-hood
cd git-under-the-hood
git init
```

## Exploring Object Storage

### Git Objects

Git objects can be categorized primarily into:

- Blob: Storing file data
- Tree: A level in the directory structure
- Commit: The metadata and pointer to a tree for a set of changes

You can find these objects in the `.git/objects` directory.

```bash
ls .git/objects
```

#### Decoding a Commit Object

Let's decode a commit object to see what's inside.

```bash
file .git/objects/01/6f2f36d106637714f0b629c25bdfa1b0f4a969
```

This will output something like:

```
zlib compressed data
```

To uncompress and read the commit object, use:

```bash
cat .git/objects/01/6f2f36d106637714f0b629c25bdfa1b0f4a969 | zlib-flate -uncompress
```

You'll see details like `tree`, `parent`, `author`, and `committer`.

### Tree Object and Blob Object

Tree objects link to blob objects and other tree objects, forming the directory structure. Let's decode one:

```bash
cat .git/objects/f6/9630aa411244b1aeb1a65da62d999c9e8d4f2a | zlib-flate -uncompress | xxd
```

This will show the hexadecimal representation of the blob or tree object. To see the actual file content related to a blob, you can use:

```bash
cat .git/objects/23/007bff4fb70a5ccd2fab9eb5f3c9f2423162b5 | zlib-flate -uncompress
```

## Understanding Commit History as a Linked List

Each commit has a `parent` field which points to the preceding commit. This forms a linked list where you can trace back the entire commit history.

## Verifying Data Integrity

You can check the integrity of an object using `sha1sum`:

```bash
cat .git/objects/23/007bff4fb70a5ccd2fab9eb5f3c9f2423162b5 | zlib-flate -uncompress | sha1sum
```

This should match the original SHA-1 hash, verifying that the object is intact.

## Conclusion

Understanding the inner workings of Git can be incredibly useful for debugging and for getting a deeper understanding of this essential tool. Feel free to explore and manipulate these objects (carefully!) to see how changes affect your Git repository.
