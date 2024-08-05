Lab 1 inciso 2 md
================
Ximena

## Introduccion

En este documento, se utilizará la funcion `lapply` para encontrar la
moda de cada vector en una lista de vectores aleatorios. La moda es el
valor que aparece con mayor frecuencia en un conjunto de datos.

## Crear una lista de vectores aleatorios

``` r
vectores <- list(
  sample(1:10, 15, replace = TRUE),
  sample(1:20, 20, replace = TRUE),
  sample(1:30, 25, replace = TRUE)
)
```

## Ver los vectores generados

``` r
vectores
```

    ## [[1]]
    ##  [1] 2 3 2 4 3 4 3 6 8 9 9 3 6 4 3
    ## 
    ## [[2]]
    ##  [1]  4 11 10  9 17  6  3 13  2  3  2 17  5  8 10 20 20 13  2 16
    ## 
    ## [[3]]
    ##  [1]  4 19  3 14  7  7  8 24 23 28 10 29 20 13  8 11 19  8 28 10 15  8 20 13  7

## Funcion de moda

``` r
# Funcion para encontrar la moda de un vector
find_mode <- function(x) {
  ux <- unique(x)
  ux[which.max(tabulate(match(x, ux)))]
}

# Aplicar la funcion find_mode a los vectores
modas <- lapply(vectores, find_mode)

# Ver resultados
modas
```

    ## [[1]]
    ## [1] 3
    ## 
    ## [[2]]
    ## [1] 2
    ## 
    ## [[3]]
    ## [1] 8
