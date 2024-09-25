dw-2024-parcial-1
================
Tepi
25/09/2024

# Examen parcial

Indicaciones generales:

- Usted tiene el período de la clase para resolver el examen parcial.

- La entrega del parcial, al igual que las tareas, es por medio de su
  cuenta de github, pegando el link en el portal de MiU.

- Pueden hacer uso del material del curso e internet (stackoverflow,
  etc.). Sin embargo, si encontramos algún indicio de copia, se anulará
  el exámen para los estudiantes involucrados. Por lo tanto, aconsejamos
  no compartir las agregaciones que generen.

## Sección 0: Preguntas de temas vistos en clase (20pts)

- Responda las siguientes preguntas de temas que fueron tocados en
  clase.

1.  ¿Qué es una ufunc y por qué debemos de utilizarlas cuando
    programamos trabajando datos?

Una función universal (ufunc) es una función que opera elemento por
elemento dentro de un array, admitiendo la transmisión de matrices, la
conversión de tipos y varias otras características estándar. Estas
funciones hacen cada elemento de la matriz de entrada se procese de
forma independiente y retornan un array con los resultados.

Este sirve al momento de trabajar con datos ya que no se debe hacer un
ciclo para operar cada uno de los elementos del array, sino que opere
todos al mismo tiempo. Es como usar las funciones apply de R.

2.  Es una técnica en programación numérica que amplía los objetos que
    son de menor dimensión para que sean compatibles con los de mayor
    dimensión. Describa cuál es la técnica y de un breve ejemplo en R.

El broadcasting es una técnica en programación que permite realizar
operaciones entre arrays o matrices de diferentes dimensiones. Lo que
hace es expandir el arreglo más pequeño para que coincida con las
dimensiones del más grande para poder realizar operaciones elemento a
elemento.

``` r
x <- c(1, 2, 3) 
y <- c(4, 5)

print(x)
```

    ## [1] 1 2 3

``` r
print(y)
```

    ## [1] 4 5

``` r
y_matrix <- matrix(y, nrow = length(x), ncol = length(y), byrow = TRUE)
y_matrix
```

    ##      [,1] [,2]
    ## [1,]    4    5
    ## [2,]    4    5
    ## [3,]    4    5

``` r
# broadcasting
x + y_matrix
```

    ##      [,1] [,2]
    ## [1,]    5    6
    ## [2,]    6    7
    ## [3,]    7    8

3.  ¿Qué es el axioma de elegibilidad y por qué es útil al momento de
    hacer análisis de datos?

El axioma de elección dice que, dada una colección de conjuntos no
vacíos, es posible formar un nuevo conjunto seleccionando exactamente un
elemento de cada uno de los conjuntos originales.

La aplicación de este axioma permite tomar partes de diferentes
conjuntos e impide que se haga un sobreajuste a los datos. También
garantiza la existencia de una solución, si se debe operar con un
conjunto infinito, se puede tomar un elemento de cada grupo finito y
operar sobre ese nuevo conjunto.

4.  Cuál es la relación entre la granularidad y la agregación de datos?
    Mencione un breve ejemplo. Luego, explique cuál es la granularidad o
    agregación requerida para poder generar un reporte como el
    siguiente:

| Pais | Usuarios |
|------|----------|
| US   | 1,934    |
| UK   | 2,133    |
| DE   | 1,234    |
| FR   | 4,332    |
| ROW  | 943      |

La granularidad se refiere al nivel de detalle de los datos, es decir,
qué tan específicos son los registros.

La agregación es el proceso de combinar datos de un nivel de
granularidad más bajo a uno más alto.

En una base de datos de ventas en línea. Datos a nivel de cada
transacción individual (granularidad alta), con información como la
fecha, el producto, el cliente y el monto. Si queremos una visión
general de las ventas por país, necesitaríamos agregar los datos de
todas las transacciones de cada país. En este caso, se estaría
reduciendo la granularidad de los datos, pasando de nivel de transacción
a nivel de país.

Para el reporte anterior:

Granularidad requerida: a nivel de país

Se necesitan al menos:

País al que pertenece cada usuario.

Identificador único para cada usuario.

Entonces:

Los datos se agrupan por el campo “País”.

Dentro de cada grupo, se cuenta el número de registros únicos en el
campo “Usuario”.

## Sección I: Preguntas teóricas. (50pts)

- Existen 10 preguntas directas en este Rmarkdown, de las cuales usted
  deberá responder 5. Las 5 a responder estarán determinadas por un
  muestreo aleatorio basado en su número de carné.

- Ingrese su número de carné en `set.seed()` y corra el chunk de R para
  determinar cuáles preguntas debe responder.

``` r
set.seed(20210493) 
v<- 1:10
preguntas <-sort(sample(v, size = 5, replace = FALSE ))

paste0("Mis preguntas a resolver son: ",paste0(preguntas,collapse = ", "))
```

    ## [1] "Mis preguntas a resolver son: 1, 3, 4, 8, 9"

``` r
#Mis preguntas a resolver son: 1, 3, 4, 8, 9
```

### Listado de preguntas teóricas

1.  Para las siguientes sentencias de `base R`, liste su contraparte de
    `dplyr`:
    - `str()` (vusta general dataframe) –\> `glimpse()`
    - `df[,c("a","b")]` (seleccionar columnas en el dataframe) –\>
      `select(df, a, b)`
    - `names(df)[4] <- "new_name"` donde la posición 4 corresponde a la
      variable `old_name` (renombrar columna) –\>
      `rename(df, new_name = old_name)`
    - `df[df$variable == "valor",]`(filtrar valores) –\>
      `filter(df, variable == "valor")`
2.  ¿Por qué en R utilizamos funciones de la familia apply
    (lapply,vapply) en lugar de utilizar ciclos?

Las funciones de la familia apply aplican lo que se necesita a un vector
completo. R es un lenguaje vectorizado, por lo que es mucho más fácil
utilizar las funciones de apply para operar sobre todo el vector de una
vez que utilizar ciclos para aplicar la operación sobre un valor a la
vez.

4.  ¿Cuál es la diferencia entre utilizar `==` y `=` en R?

= se utiliza para declarar una variable, darle un valor a algo, ej. a =
2, en este caso la variable a es 2

== se utiliza para decir que algo es igual que otra cosa, ej. a == 2, en
este caso se está comprobando si el valor de a es igual a 2

8.  Si en un dataframe, a una variable de tipo `factor` le agrego un
    nuevo elemento que *no se encuentra en los niveles existentes*,
    ¿cuál sería el resultado esperado y por qué?
    - El nuevo elemento
    - `NA` Cuando se intenta agregar una nueva variable a un dataframe
      con factor, el nuevo valor se tratará como un valor faltante (NA)
      debido a que los calores y niveles son fijos y se definen desde un
      principio.
9.  En SQL, ¿para qué utilizamos el keyword `HAVING`? HAVING se utiliza
    en las queries que no puede usarse WHERE, que son las que tienen
    operaciones dentro del SELECT.

Extra: ¿Cuántos posibles exámenes de 5 preguntas se pueden realizar
utilizando como banco las diez acá presentadas? (responder con código de
R.)

``` r
#numero total de preguntas
n <- 10
#preguntas por examen
k <- 5

#combinaciones
numero_exámenes <- choose(n, k)
print(numero_exámenes)
```

    ## [1] 252

## Sección II Preguntas prácticas. (30pts)

- Conteste las siguientes preguntas utilizando sus conocimientos de R.
  Adjunte el código que utilizó para llegar a sus conclusiones en un
  chunk del markdown.

A. De los clientes que están en más de un país,¿cuál cree que es el más
rentable y por qué?

B. Estrategia de negocio ha decidido que ya no operará en aquellos
territorios cuyas pérdidas sean “considerables”. Bajo su criterio,
¿cuáles son estos territorios y por qué ya no debemos operar ahí?

### I. Preguntas Prácticas

## A

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 4.3.3

``` r
datos <- readRDS("parcial_anonimo.rds")

#Agrupar por cliente y país, y calcular la suma de ventas y el número de países
clientes_rentables <- datos %>%
  group_by(Cliente) %>%
  summarise(Total_Venta = sum(Venta, na.rm = TRUE),
            Numero_Paises = n_distinct(Pais)) %>%
  filter(Numero_Paises > 1) %>%
  arrange(desc(Total_Venta))

# Visualizar los clientes rentables
print(clientes_rentables)
```

    ## # A tibble: 7 × 3
    ##   Cliente  Total_Venta Numero_Paises
    ##   <chr>          <dbl>         <int>
    ## 1 a17a7558      19818.             2
    ## 2 ff122c3f      15359.             2
    ## 3 c53868a0      13813.             2
    ## 4 044118d4       9436.             2
    ## 5 f676043b       3635.             2
    ## 6 f2aab44e        400.             2
    ## 7 bf1e94e9          0              2

``` r
#rentabilidad de los clientes que operan en más de un país
ggplot(clientes_rentables, aes(x = reorder(Cliente, -Total_Venta), y = Total_Venta)) +
  geom_bar(stat = "identity", fill = "blue") +
  theme_minimal() +
  labs(title = "Clientes en mas de un pais con mas compras",
       x = "Cliente",
       y = "Total Venta") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](dw-2024-examen-parcial-1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#contar clientes unicos
clientes_unicos <- datos %>%
  summarise(total_clientes_unicos = n_distinct(Cliente))
print(clientes_unicos)
```

    ##   total_clientes_unicos
    ## 1                  2147

## B

``` r
###resuelva acá
```
