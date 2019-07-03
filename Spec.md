# The SDS Specification

### The .SDS File

As of _v0.1.0-1_ (version 0.1.0 model 1), a _.sds_ file can only be _UTF-8_ formatted, and can only
use the _Unix type_ of newline character `\n`.

There are three data that can be present in a _.sds_ file:

1. the version of SDS in use by the dir
2. the name of the lead content of the dir
3. the orgrules (organization rules) of the dir

#### Example

~~~~
Version:0.1.0-1
Lead:fileX.ext
Orgrules: [
  fileX.ext/subDirX
]
~~~~

_Note how each data started on a new line. Two or more data can not be on the same line. Also, data
of a _.sds_ file may come in any order, but it's always recommended that the version data is always
the first data to appear in a _.sds_ file._

#### The Version Data

The version data is a data that must be present in a valid _.sds_ file. The value of this data shall
be the version and model no of SDS used to organize the dir. As stated above, it is recommended that
this data should always be the first data in a _.sds_ file. Example:

~~~~
Version:0.1.0-1
~~~~

If a _.sds_ file does not have this data, or the value of this data is not set, or the value of this
data is invalid, the SDS specification does not specify what should happen.

#### The Lead Content Data

The lead content data tells which file or sub-directory should first be listed, when listing an SDS
dir's contents. The value of this data shall be the name of the file or sub-directory to be made
the lead content.

If an SDS dir does not specify a specific dir-content should be its lead content, this data can be
left out of the _.sds_ file or recorded like this:

~~~~
Lead:
~~~~

_Spaces (this is what is meant by space _" "_, not _"\n"_ and the likes) may appear after the colon
`:`._

#### Orgrules

An orgrule (Organization Rule) is an infomation specifically describing where a dir-content should
be placed, in the dir listing. An SDS directory may have an infinite number of orgrules. If an SDS
dir has no orgrule, this data may be left out of the _.sds_ file, or recorded like this:

~~~~
Orgrules:
~~~~

_Spaces (this is what is meant by space _" "_, not _"\n"_ and the likes) may appear after the colon
`:`._

or like this:

~~~~
Orgrules:[
]
~~~~

_Spaces (this is what is meant by space _" "_, not _"\n"_ and the likes) may appear after `[`._

Orgrules of an SDS dir should be placed in between `[` and `]`. Each orgrule must be on a new line.

###### Example

~~~~
Orgrules:[
  someDir/someFile.xt
someFile.xt/anotherFile.xt
]
~~~~

_Spaces (this is what is meant by space _" "_, not _"\n"_ and the likes) may appear before and after
an orgrule. However, the standard is that two spaces should come before an orgrule, and no spcae
should come after._

###### Orgrule Illegal Chars

Owing to the fact that the `/` character and the `\n` character are used for orgrules syntax, a file
or sub-dir is not allowed to have them in its name.

__Examples of invalid orgrules (due to presence of `/` in file name or sub-dir name):__

~~~~
someFile.ext/some/Dir
someFile/ext/some.Dir
some.Dir/someFile/ext
some/Dir/someFile.ext
~~~~

The SDS standard however does not place limit on how long the name of a file or sub-directory can
be.

* [Back](https://github.com/qamarian-sds/sds/blob/master/Working.md)
* [Home](https://github.com/qamarian-sds/sds)

#### Comments

The SDS specification allows comments to be placed in _.sds_ files. `||` can be used for single-line
comment, while `|* ... *|` can be used for multi-line comment.

~~~~
|| This is a single line comment.

|* This is a
multi-line
comment. *|
~~~~

_Note that a comment shall always start on a new line. Two different comments shall not appear on the
same line. Also, a comment shall not appear on the same line as any .sds file data._

###### Examples of Invalid Comments

~~~~
|* Comment A *| || Comment B

Version:0.1.0-1 || Comment C
~~~~
