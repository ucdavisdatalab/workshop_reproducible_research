# Navigating File Systems

Due to the nature of its stripped down display, the command line interface can
at times feel a bit static. But it offers much of the same functionality that
modern GUIs offer, including the ability to move around your computer. With
GUIs, we tend to navigate with our mouses; on the command line, we'll use our
keyboard. The live workshop session will cover this in depth, but for now it's
important to have a working sense of how your files are structured.

## File Paths

Broadly speaking, your computer is comprised of **files** (chunks of data) and
**directories** (or "folders"). The way your computer organizes this
information is called a **directory structure**. This structure is like a map
of all the places you can navigate to you in your computer. Each file has an
**address** on this map, and there is a **path** that leads to it. When you use
a GUI, your computer does the work of determining which path to follow to find
the file or folder you'd like to open, but performing the same operation with a
CLI requires us to manually specify where we'd like to go.

In a Unix environment, we do this with the following syntax:

```
/this/is/a/path/to/your/file.txt
```

This is called a **file path**. It threads through several different
directories to point directly and specifically at your desired file,
`file.txt`.

**Note:** non-Unix environments on Windows will use `\` instead of `/`. This
isn't as important to know for our workshop, but if you use file paths in other
instances it may factor in to the way you write out a path.

## Path Hierarchies

Importantly, the structure of your computer's directory is **hierarchical**.
Directories nest inside one another. Each `/` in the path above denotes a new
"level" within the directory structure. You thus need to have a sense of which
directories are "above" or "below" the present file path to navigate with the
command line.

The top-most directory in your computer is called the **root**. Unix systems
denote it with the `/` character. It's like the trunk of a tree: every
directory in your computer branches off from it.

A complication arises from the fact that directories can also  branch off from
each other. Whenever you make a folder within another folder -- or a
**subdirectory** -- you've created another branch in the tree, one which is at
the same time a branch of root *and* a branch of whatever directory you're
currently in. For example, this is the structure of the practice exercise we'll
be using during our live session:

```
.                               top of the directory
├── instructions.md             file in the top of the directory
├── instructions.pdf            ""
└── level_1                     first subdirectory
    ├── level_2a                a second subdirectory, below level_1
    │   ├── 2.txt               file in the second subdirectory
    │   └── extra_file.txt      ""
    ├── level_2b                another subdirectory, at the same level as level_2a
    │   └── level_3             third subdirectory, below level_2a and level_2b
    │       └── 1.txt           file in the third subdirectory
    └── wrong_name.txt          file in first subdirectory
```

As you can see, this can get complicated! When you're first getting started
with the command line, it's easy to feel a little lost and forget where you are
in your computer. We'll discuss some strategies to help with this in the live
session, but know for the moment that the `pwd` command will always tell you
where you are relative to the root:

```
$ pwd
/here/is/where/you/are/located/in/your/computer
```

**Note:** on Windows, the root is usually called `C:` or `D:` and is designated
as such in file paths. In Unix environments (like Git Bash), we'd write the
above file path like so:

```
$ pwd
C:/here/is/where/you/are/located/in/your/computer
```

Outside Unix (i.e. for DOS-style environments), the same path would be:

```
$ pwd
C:\here\is\where\you\are\located\in\your\computer
```

The root on Windows machines also refers to the physical drive in which your
files are stored. Because of this, a computer with multiple drives has multiple
roots -- something you'd need to be aware of when specifying your file paths.

## Absolute vs. Relative Paths

Regardless of your operating system, there are two different ways to specify a
file path on the command line. Recall that the Unix-style path above:

```
$ pwd
/here/is/where/you/are/located/in/your/computer
```

...begins with `/`, which, again, is root. When you see a path that begins with
`/`, it's showing you the full, or **absolute** location of a directory or
file. No matter where you are in your computer, no matter how deep into a
series of subdirectories you may be, if you used this absolute path with a
navigation command, you could go straight to the location it indicates.

By contrast, a **relative path** is context-specific. It depends on wherever
you are in your computer. Unix uses some shorthand for this: 

+ `.` denotes the current location in your computer; while 
+ `..` denotes the directory above that location.

Remember seeing these in the previous chapter with `ls -lha`? Your computer
keeps track of its directories by placing these two symbols in every folder you
create. You can thus use this shorthand to avoid having to type out the entire
file path to a file. This is useful if you're far away from `root`, or if, in a
coding project, you are using files that are encapsulated in a specific
directory structure. To continue with the example from above, if you're at
`computer`:

```
/here/is/where/you/are/located/in/your/computer
                                       ^^^^^^^^
```

And you want to get to `located`:

```
/here/is/where/you/are/located/in/your/computer
                       ^^^^^^^
```

You could use an absolute path to get there. To do so, you use the `cd`
("change directory") command:

```
$ cd /here/is/where/you/are/located
```

If we were to represent the logic of this path, it would look like so:

```
root
└── here
│   └── is
│       └── where
│           └── you
│               └── are
└──────────────────>└── located
                        └── in
                            └── your
                                └── computer
```

Alternatively, you could use a relative path:

```
$ cd ../../../
```

...which we could represent like so:

```
root
└── here
    └── is
        └── where
            └── you
                └── are
                    └── located
                    ^   └── in
                    │       └── your
                    │           └── computer
                    └───────────────┘
```

The relative path would take you three directories up from `computer`, which,
as you can see above, is `located` (count the names between backslashes).
That's much shorter, but there's a downside: those `..` sequences are pretty
hard to read without context, and it's easy to get confused. Clearly, there can
sometimes be a trade-off between using absolute and relative paths, and in the
workshop, we'll talk about how to weigh such options (particularly when first
beginning to use the command line).

## Moving Data Around

Beyond helping you navigate, file paths also enable you to move information
around on your computer. Doing so works very much like the above.

To move a file, we use `mv`, which has the following syntax:

```
$ mv [location/of/file] [location/where/you/want/to/move/the/file]
```

If we're in a directory that looks like so:

```
$ ls
file.txt  subdirectory
```

...and we want to move `file.txt` into `subdirectory`, we can use:

```
$ mv ./file.txt ./subdirectory/file.txt
```

Note here that we've used `./` to specify that `file.txt` is in the *current*
directory. Note too that we wrote out the *full filename* of `file.txt` after
`subdirectory`. If you didn't write out the full filename after `subdirectory`
or use `./`, your CLI is usually smart enough to infer what you meant and would
move `file.txt` to `subdirectory`:

```
$ mv file.txt subdirectory
$ cd subdirectory
$ ls
file.txt
```

However, it's better to be as specific as possible so as to ensure you've moved
your file exactly where you want it. Many a file has gotten lost on computers
because of inexact commands.
