Lab 1 inciso 1 md
================
Ximena

## Carga de librerias

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
library(openxlsx)
```

    ## Warning: package 'openxlsx' was built under R version 4.3.3

``` r
library(writexl)
```

    ## Warning: package 'writexl' was built under R version 4.3.3

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

## Carga de archivos

``` r
leer_excel1 <- read.xlsx('01-2023.xlsx')
leer_excel2 <- read.xlsx('02-2023.xlsx')
leer_excel3 <- read.xlsx('03-2023.xlsx')
leer_excel4 <- read.xlsx('04-2023.xlsx')
leer_excel5 <- read.xlsx('05-2023.xlsx')
leer_excel6 <- read.xlsx('06-2023.xlsx')
leer_excel7 <- read.xlsx('07-2023.xlsx')
leer_excel8 <- read.xlsx('08-2023.xlsx')
leer_excel9 <- read.xlsx('09-2023.xlsx')
leer_excel10 <- read.xlsx('10-2023.xlsx')
leer_excel11 <- read.xlsx('11-2023.xlsx')
```

## Eliminar columnas innecesarias

``` r
leer_excel7 <- leer_excel7[,1:8]
leer_excel8 <- leer_excel8[,1:8]
leer_excel9 <- leer_excel9[,1:8]
leer_excel10 <- leer_excel10[,1:8]
leer_excel11 <- leer_excel11[,1:8]
```

## Crear fechas

``` r
leer_excel1$Fecha <- '01-2023'
leer_excel2$Fecha <- '02-2023'
leer_excel3$Fecha <- '03-2023'
leer_excel4$Fecha <- '04-2023'
leer_excel5$Fecha <- '05-2023'
leer_excel6$Fecha <- '06-2023'
leer_excel7$Fecha <- '07-2023'
leer_excel8$Fecha <- '08-2023'
leer_excel9$Fecha <- '09-2023'
leer_excel10$Fecha <- '10-2023'
leer_excel11$Fecha <- '11-2023'
```

## Unir los dataframes

``` r
df_completo <- bind_rows(leer_excel1, leer_excel2, leer_excel3, leer_excel4, leer_excel5, leer_excel6, leer_excel7, leer_excel8, leer_excel9, leer_excel10, leer_excel11)
```

## Preview dataframe

``` r
head(df_completo)
```

    ##   COD_VIAJE                                       CLIENTE UBICACION CANTIDAD
    ## 1  10000001       EL PINCHE OBELISCO / Despacho a cliente     76002     1200
    ## 2  10000002               TAQUERIA EL CHINITO |||Faltante     76002     1433
    ## 3  10000003      TIENDA LA BENDICION / Despacho a cliente     76002     1857
    ## 4  10000004                           TAQUERIA EL CHINITO     76002      339
    ## 5  10000005 CHICHARRONERIA EL RICO COLESTEROL |||Faltante     76001     1644
    ## 6  10000006                       UBIQUO LABS |||FALTANTE     76001     1827
    ##                          PILOTO      Q CREDITO        UNIDAD   Fecha
    ## 1       Fernando Mariano Berrio 300.00      30 Camion Grande 01-2023
    ## 2        Hector Aragones Frutos 358.25      90 Camion Grande 01-2023
    ## 3          Pedro Alvarez Parejo 464.25      60 Camion Grande 01-2023
    ## 4          Angel Valdez Alegria  84.75      30         Panel 01-2023
    ## 5 Juan Francisco Portillo Gomez 411.00      30 Camion Grande 01-2023
    ## 6             Luis Jaime Urbano 456.75      30 Camion Grande 01-2023

## Exportar el dataframe a Excel

``` r
write_xlsx(df_completo, 'excel_exportado.xlsx')
```
