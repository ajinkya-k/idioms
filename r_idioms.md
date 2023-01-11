# R idioms

## Parallelization

The `foreach` library doesn't allow side-effects or manipulating shared objects (at least I don't know how to do it).
Suppose we want to generate two matrices of size $m \times n$ using a parallelized loop, here's how to do it:\

First we define a combination function, and then we use it in for each with `.multicombine = TRUE`.

This draws very heavily from a [stack overflow answer](https://stackoverflow.com/a/28354056).

```r
# combination function
comb <- function(...) {
    mapply("rbind", ..., SIMPLIFY = FALSE)
}

# sample usage
result <- foreach(i=1:m, .combine='comb', .multicombine=TRUE) %dopar% {
    m1_row <- rep(i, n)
    m2_row <- rep(i, n)
    names(m1_row) <- paste0("a", as.character(1:n)) # with `rbind` creates a matrix with column names
    list(m1_row, m2_row)
}

result[[1]] #returns a matrix with colnames a1, a2, ...
result[[2]] #returns a matrix with same values but no colnames
```
