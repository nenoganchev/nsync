## Usage

### Verify

```
nsync verify [dir]
```

Traverses `dir`, computing the checksum of each file, compares them against the ones stored in the
current snapshot, reports the result, and produces a new snapshot with the updated checksums if any
changes were found.

If `dir` is not specified, the current directory is used.

The report puts files in one of the following categories:
* "added": The current snapshot doesn't contain a checksum for this file. It will be included in the
  updated snapshot.
* "changed": The file's checksum differs from the one stored in the current snapshot. The updated
  snapshot will contain the new checksum.
* "unchanged": The file's checksum matches the one in the current snapshot.
* "deleted": The current snapshot contains a checksum for this file, but it was not found while
  traversing the directory. The checksum will be removed in the updated snapshot.

Even if changes are detected, this command doesn't modify the current snapshot; it is preserved so
that its contents can be used to examine the changes. A new snapshot is created instead, containing
all detected changes. The following commands make use of this new snapshot:
* `nsync show-changes`, to display the same report as this command, but instantaneously, without the
  need to traverse the directory again and compute checksums.
* `nsync commit-changes`, to replace the original snapshot with the updated one, thus making the
  changes the new official baseline.

###

```
nsync show-changes [dir]
```

Shows a report of the changes betweenk

###

```
nsync commit-changes [dir]
```
