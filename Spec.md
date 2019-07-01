# The SDS Specification

__Note:__

1. An SDS directory is a directory that uses SDS for organization.
2. "Dir" would mean directory.


Every SDS directory shall have a file called ".sds" (case-insensitive). This file would contain
information on how the directory is organized. This file shall be placed in the directory itself. As
of version 0.1.0, a typical ".sds" file looks like this:

```
|*
This file defines the structure of its directory. See https://github.com/qamarian-sds/sds to learn
	more about SDS.
*|

Version:0.1.0
Segregration:da
LeadFile:m.txt
LeadDir:someDir
OrgRules:[
  a.txt/b.txt
  c.txt/z.txt
]
```

SDS organizes dirs based on three things: _Unicode_, _The Lead Content Concept_, and _The After
Concept_.

## Unicode

By default, the contents of an SDS directory (SDSD) shall be based on the position of their
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

```

If the directory is yet to undergo any organization, an SDS-supported file manager shall list it the
following way:

```
.prof
.profile
-x.txt
01-07-19.txt
01-07-2019.txt
01.txt
1.txt
_TempLog
about.md
MakeFile
```

You can see how the contents are listed based on the position of their characters, in the [Unicode table]
(https://en.wikipedia.org/wiki/List_of_Unicode_characters). Also note how no distinction was made
between lower-case and upper-case alphabets.

#### Summary

_In short, the contents of an SDS directory which has not been organized at all, shall be listed based on
the position of their characters (characters of their name), in the Unicode table._
