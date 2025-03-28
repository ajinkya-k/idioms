# LaTeX idioms

## Shell script to remove LaTeX temp files

Based on [this script](https://gist.github.com/djsutherland/266983#file-latex-clean-sh) by [Danica J. Sutherland](https://gist.github.com/djsutherland) which removes the temporary files created in the process of (pdf)LaTeX compilation, I made a similar one with minor modifications. I only added a few more extensions to remove and used the verbose option `-v` with the `rm` command to output which files are being delted to the standard output. 
```bash
exts="aux bbl blg brf idx ilg\
 ind lof log lol lot nav out snm\
 tdo toc synctex.gz fdb_latexmk fls"

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


## Converting TikZ pictures to `.ps` files

The following is based on [this note by Michael Goerz](https://michaelgoerz.net/notes/creating-eps-files-from-tikz.html#using-externalization). 
There are a few steps: 

- In a latex file say `fig-generator.tex` make your `tikzpicture` (call it by any name, here we use `fig-name`) inside a `pgfgraphicnamed` environment as follows:


    ```latex
    \beginpgfgraphicnamed{fig-name}
        \begin{tikzpicture}
            ...
            ;
        \end{tikzpicture}
    \endpgfgraphicnamed
    ```

- Next, we compile the latex to create a `fig-name.dvi` file

    ```shell
    latex --jobname=fig-name fig-generator.tex
    ```

- Now we convert the `dvi` file to an `eps` file:

    ```shell
    dvips -o fig-name.eps fig-name.dvi
    ```

- Sometimes you will need a `ps` file instead of an `eps` file and a simple hack is to just change the file extension using `mv` (if you want to keep the `eps` file use `cp` instead of `mv`)

    ```shell
    mv fig-name.eps fig-name.ps
    ```

- In the `tex` file where you intend to use the `ps` figure add the following in the header: `\usepackage{auto-pst-pdf}`.
You may have to add the `-shell-escape` argument to the `latexmk` command when compiling the `tex` file where you use the figure.
If using VS Code [this stackoverflow thread](https://tex.stackexchange.com/questions/516604/how-to-enable-shell-escape-or-write18-visual-studio-code-latex-workshop) explains how to add shell escape argument to `latexmk`.
