# The SDS Specification

**Note:**

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

SDS organizes dirs based on three things: Unicode, The Lead Content Concept, and The After Concept.

## Unicode

Ordinarily, the contents of an SDS directory (SDSD), shall be sorted based on their Unicode code
point.

Assuming we have a directory containing the following files:

```
01-07-2019.txt 01-07-19.txt 01.txt -x.txt .profile MakeFile _TempLog
```

If an SDS directory is yet to be organized, an SDS-supported file manager 