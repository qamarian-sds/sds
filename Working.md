# How SDS Actually Works

Before reading the SDS specification, it is very important we discuss how SDS actually works.

~~~~
Terms to Note:

"SDS directory" would mean a directory that uses SDS to organize its contents.
"Dir" would mean directory.
~~~~

Every SDS directory shall have a file called ".sds" (case-insensitive). This file would contain
information on how the directory is organized. This file shall be placed in the directory itself.

As of _version 0.1.0 model 1 (v0.1.0-1)_, a typical ".sds" file looks like this:

```
|| This is a single-line comment.
|*
This is a multi-line comment.
*|

Version:0.1.0-1
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

If the directory is yet to undergo any organization, an SDS-supported file manager shall list its
contents the following way:

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
table](https://en.wikipedia.org/wiki/List_of_Unicode_characters). Also note how no distinction was
made between lower-case and upper-case alphabets.

## The After Concept

SDS implements directory organization, by saving info about what file should come after another
file.

Ordinarily, contents of an SDS directory would be listed based on what we discussed under
_Unicode-based Listing_. However, if we want a dir-content to be placed at a location which is not
its default location, some information would have to be recorded in the _.sds_ file, to make this
possible.

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
Version:0.1.0-1
Lead:
Orgrules:[
]
~~~~

would now look like this:

~~~~
Version:0.1.0-1
Lead:
Orgrules:[
  .profile/.prof
]
~~~~

With the help of this simple concept and  _Unicode-based Listing_, we can organize an SDS directory
in any way.

## The Lead Content Concept

Although, the _Unicode-based Listing_ and _The After Concept_ are enough for us to organize an SDS
dir in any way, they however are not enough to facilitate computational efficiency in some cases.

Imagine we having a dir containing the following:

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

To make file _MakeFile_ the first content to be listed, we can manipulate our _.sds_ file into
something like this:

~~~~
Version:0.1.0-1
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

#### A Better Approach

Looking at the approach we took above, to making file _MakeFile_ the first content of our dir, you
can see how inefficent it would be if we are dealing with thousands of dir-contents (i.e. we would
have to record so many orgrules). To surpress this potential inefficiency, we introduced the _Lead
 Content_ concept.

With the _Lead Content_ concept, rather than recording so many orgrules, we can just make file
_MakeFile_ our lead content. This means our _.sds_ would instead be manipulated into this:

~~~~
Version:0.1.0-1
Lead:MakeFile
Orgrules:[
]
~~~~

Rather than this:

~~~~
Version:0.1.0-1
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

#### Capitalizing on The Lead

Now let's assume we want to go a step further, and make file _1.txt_ come after _MakeFile_, we can
further manipulate our _.sds_ file into this:

~~~~
Version:0.1.0-1
Lead:MakeFile
Orgrules:[
  MakeFile/1.txt
]
~~~~

In other words, our dir would now be displayed like this:

~~~~
MakeFile
1.txt
.prof
.profile
.sds
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
_TempLog
about.md
~~~~

__NOTE: Lead content is not restricted to files alone, a sub-dir can also be the lead of an SDS
dir.__

* [Back](https://github.com/qamarian-sds/sds/blob/master/SDSForDev.md)
* [Next](https://github.com/qamarian-sds/sds/blob/master/Spec.md)
