# Document related scripts

## PDF without active content

Sometimes online forms reqiure a PDF that does not have active content.
The following snippet taken from a [stackoverflow answer](https://tex.stackexchange.com/a/481609/240442) generates a PDF that has no active contents using GhostScript (`gs` command on macOS and other UNIX equivalents)

```bash
gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=pdf-inactive.pdf -dBATCH input-pdf.pdf
```