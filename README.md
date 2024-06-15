## Usage

### Build index

```
nsync build-index [directory] [--only <wildcards list>]
```

Creates an index file containing the path, size, and checksum of all files under [directory]. This
index file is later used to check files for changes or corruption, and for fast syncs.

[directory] defaults to the current directory if not specified.

If `--only` is present, limits the indexing to the files matching a wildcard in the list.

```
Implementation hints:
- if previous index:
  - create backup copy of previous index
  - show summary of detected changes: no. files new, modified deleted
  - create detected changes report file
- if no previous index:
  - show summary: no. indexed files
```

### Check integrity

```
nsync verify-against-index [directory] [--only <wildcards list>]
```

Recomputes the checksums of all files in [directory] and compares them against the checksums stored
in the index file. Useful for detecting bit rot and corruption, as well as intentional changes that
have not yet been reflected in the index file.

Detected changes are stored to a file which can be used to examine them again, or to apply them to
the index file.

[directory] defaults to the current directory if not specified.

If `--only` is present, limits the checking to the files matching a wildcard in the list.

```
Implementation hints:
- fail when index file is missing
- output summary of detected changes: no. files new, modified, deleted
- create detected changes file
```

### Update index

```
nsync commit-changes-to-index [directory]
```

Uses the changes file produced by `nsync verify-against-index` to update the index file. Use this
when you've determined that all changes detected by `verify-against-index` are intentional and not
a sign of data corruption.

[directory] defaults to the current directory if not specified.

```
Implementation hints:
- fail when index file is missing
- fail when changes file is missing
- output summary of changes: no. entries added, modified, deleted
- store detailed list of changes to a report file
```

### Fast sync

```
nsync fast <source dir> <destination dir> [--only <wildcards list>] [--dry-run]
```

Uses the index files in both source and destination directories to quickly determine which files
from the source need to be synced to the destination, and syncs them. Expects that both source and
destination directories have up-to-date index files. Updates the index in the destination.

If `--only` is present, limits the sync to the files matching a wildcard in the list.

If `--dry-run` is present, doesn't change anything in destination, only shows a list of all changes
that would be made without `--dry-run`.

```
Implementation hints:
- fail when index file is missing in source or dest dir
- output summary of changes: no. files created, modified, deleted
- update dest index
- store detailed list of changes to a report file
```

### Full sync

```
nsync full <source dir> <destination dir> [--only <wildcards list>] [--dry-run]
```

Copies all files from source to destination and creates or updates the index file at the
destination.

If `--only` is present, limits the sync to the files matching a wildcard in the list.

If `--dry-run` is present, doesn't change anything in destination, only shows a list of all changes
that would be made without `--dry-run`.

```
Implementation hints:
- create/update dest index
- output summary of changes: no. files created, modified, deleted
- store detailed list of changes to a report file
```
