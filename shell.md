# Useful Shell Commands

## File sizes

When using `bash`, the following creates a list of files with their file sizes, and sort in ascending order of size.

```bash
du --human-readable -d1 | sort --human-numeric-sort
```
(Source: somewhere on stackoverflow. Will update when I find it again)

## LaTeX

Based on [this script](https://gist.github.com/djsutherland/266983#file-latex-clean-sh) by [Danica J. Sutherland](https://gist.github.com/djsutherland) which removes the temporary files created in the process of (pdf)LaTeX compilation, I made a similar one with minor modifications. I only added a few more extensions to remove and used the verbose option `-v` with the `rm` command to output which files are being delted to the standard output. 
```shell
exts="aux bbl blg brf idx ilg ind lof log lol lot nav out snm tdo toc synctex.gz fdb_latexmk fls"

for x in "${@:-.}"; do
    arg=$(echo ${x:-.} | perl -pe 's/\.(tex|pdf)$//')

    if [[ -d "$arg" ]]; then
        for ext in $exts; do
             rm -vf "$arg"/*.$ext
        done
    else
        for ext in $exts; do
             rm -vf "$arg".$ext
        done
    fi
done

```
