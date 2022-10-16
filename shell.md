# Useful Shell Commands

## File sizes

When using `bash`, the following creates a list of files with their file sizes, and sort in ascending order of size.

```bash
du --human-readable -d1 | sort --human-numeric-sort
```
(Source: somewhere on stackoverflow. Will update when I find it again)
