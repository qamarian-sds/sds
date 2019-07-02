# The SDS Specification

__Note:__

1. An SDS directory is a directory that uses SDS for organizing its files and sub-directories.
2. "Dir" would mean directory.


Every SDS directory shall have a file called ".sds" (case-insensitive). This file would contain
information on how the directory is organized. This file shall be placed in the directory itself. As
of version 0.1.0, a typical ".sds" file looks like this:

```
|| This is a single-line comment.
|*
This is a multi-line comment.
*|


Version:0.1.0
Segregation:da
Lead:m.txt
Orgrules:[
  a.txt/b.txt
  c.txt/z.txt
]
```

SDS organizes dirs based on three things: _Unicode-based Listing_, _The Lead Content Concept_, and
_The After Concept_.

## Unicode-based Listing

By default, the contents of an SDS directory shall be listed based on the position of their
characters in the [Unicode table](https://en.wikipedia.org/wiki/List_of_Unicode_characters).

Assuming we have an SDS directory containing the following files:

```
01-07-2019.txt
01-07-19.txt
01.txt
-x.txt
.profile
MakeFile
_TempLog
about.md
1.txt
.prof
.sds

```

If the directory is yet to undergo any organization, an SDS-supported file manager shall list it the
following way:

```
.prof
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
MakeFile
```

You can see how the contents are listed based on the position of their characters, in the [Unicode
table] (https://en.wikipedia.org/wiki/List_of_Unicode_characters). Also note how no distinction was
made between lower-case and upper-case alphabets.

## The After Concept

SDS implements directory organization, by saving info about what file should come after another
file.

Ordinarily, contents of an SDS directory would be listed based on what we discussed under the
__Unicode-based Listing section__; however, if we want a dir-content to be placed at a location
which is not its default location, some information would have to be recorded in the __".sds"__,
to make this possible.

#### Example

Assuming we have an SDS dir containing the following files:

~~~~
.prof
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
MakeFile
~~~~

To make _.prof_ appear after _.profile_, we have to save an __organization rule__ which would
tell SFMs (SDS File Managers) to implement such a behaviour. That is, our _.sds_ file which was
formerly like this:

~~~~
Version:0.1.0
Segregation:
Lead:
Orgrules:[
]
~~~~

would now look like this

~~~~
Version:0.1.0
Segregation:
Lead:
Orgrules:[
  .profile/.prof
]
~~~~

With the help of this simple concept (in addition to the Unicode-based listing discussed above), we
can organize an SDS directory anyway.

## The Lead File Concept

Although, Unicode-based Listing and The After Concept are enough for us to organize an SDS dir in
any way, they however are not enough to facilitate computational efficiency in some cases.

Imagine we have a dir containing the following:

~~~~
.prof
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
MakeFile
~~~~

To make file _MakeFile_ the first content to be listed, we can manipulate our _.sds_ into something
like this:

~~~~
Version:0.1.0
Segregation:
Lead:
OrgRules:[
  MakeFile/.prof
  .prof/.profile
  .profile/.sds
  .sds/-x.txt
  -x.txt/01-07-19.txt
  01-07-19.txt/01-07-2019.txt
  01-07-2019.txt/01.txt
  01.txt/1.txt
  1.txt/_TempLog
  _TempLog/about.md
]
~~~~

Doing so, would cause our dir to now be listed like this:

~~~~
MakeFile
.prof
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
~~~~

Note, if we only added the organization rule (orgrule) `MakeFile/.prof`, this is how our dir would
have been listed:

~~~~
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
MakeFile
.prof
~~~~

Looking at the approach we took, to making file _MakeFile_ the first content of our dir, you can see
how inefficent it would be if we are dealing with thousands of files and sub-dirs (i.e. we would
have to record so many orgrules). To surpress this potential inefficiency, we introduced the _Lead
File_ concept.

With the Lead File concept, rather than recording so many orgrules, we can just make file _MakeFile_
our lead file. This means our _.sds_ would instead be manipulated into this:

~~~~
Version:0.1.0
Segregation:
Lead:MakeFile
Orgrules:[
]
~~~~

Now let's assume we want to go a step further, and make file _1.txt_ come after _MakeFile_, we can
further manipulate our _.sds_ file into this:

~~~~
Version:0.1.0
Segregation:
Lead:MakeFile
Orgrules:[
  MakeFile/1.txt
]
~~~~

In other words, our dir would now be displayed like this:

~~~~
MakeFile
.sds
.prof
.profile
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
~~~~

__Note that not only a file can be made the lead of a dir, a sub-dir can also be made the lead.__
