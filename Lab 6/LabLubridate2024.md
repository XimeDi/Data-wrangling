## Laboratorio de la libreria Lubridate

    ##cargar librerias

    library(nycflights13)

    ## Warning: package 'nycflights13' was built under R version 4.3.3

    library(lubridate)

    ## Warning: package 'lubridate' was built under R version 4.3.3

    library(dplyr)

    ## Warning: package 'dplyr' was built under R version 4.3.3

## Resuelva las siguientes preguntas:

### Ejercicio 1: Convertir columnas de hora en fecha-hora

Problema: Convierte las columnas dep\_time (hora de salida) y arr\_time
(hora de llegada) en objetos de tipo datetime usando make\_datetime() de
lubridate. Recuerda que estas columnas están en formato militar (HHMM).

Ayuda: Investiga la funcion matematica de modulo de r.

    # convertir las horas de salida y llegada en datetime

    flights_dt <- flights %>%
      mutate(
        dep_hour = dep_time %/% 100,
        dep_min = dep_time %% 100,
        arr_hour = arr_time %/% 100,
        arr_min = arr_time %% 100,
        
        # crear datetime usando make_datetime para salida y llegada
        dep_datetime = make_datetime(year, month, day, dep_hour, dep_min),
        arr_datetime = make_datetime(year, month, day, arr_hour, arr_min)
      ) %>%
      select(dep_time, dep_datetime, arr_time, arr_datetime)

    head(flights_dt)

    ## # A tibble: 6 × 4
    ##   dep_time dep_datetime        arr_time arr_datetime       
    ##      <int> <dttm>                 <int> <dttm>             
    ## 1      517 2013-01-01 05:17:00      830 2013-01-01 08:30:00
    ## 2      533 2013-01-01 05:33:00      850 2013-01-01 08:50:00
    ## 3      542 2013-01-01 05:42:00      923 2013-01-01 09:23:00
    ## 4      544 2013-01-01 05:44:00     1004 2013-01-01 10:04:00
    ## 5      554 2013-01-01 05:54:00      812 2013-01-01 08:12:00
    ## 6      554 2013-01-01 05:54:00      740 2013-01-01 07:40:00

## Ejercicio 2: Duracion del vuelo

Calcula el tiempo de vuelo total en minutos entre las columnas dep\_time
y arr\_time que calculaste en el primer ejercicio.

    # calcular la duracion del vuelo en minutos
    flights_dt <- flights_dt %>%
      mutate(
        # si la hora de llegada es menor que la de salida, suma un dia a arr_datetime
        arr_datetime_adj = if_else(arr_datetime < dep_datetime, 
                                   arr_datetime + days(1), 
                                   arr_datetime),
        
        # calcular la diferencia de tiempo en minutos
        flight_duration = as.numeric(difftime(arr_datetime_adj, dep_datetime, units = "mins"))
      ) %>%
      select(dep_datetime, arr_datetime, arr_datetime_adj, flight_duration)

    head(flights_dt)

    ## # A tibble: 6 × 4
    ##   dep_datetime        arr_datetime        arr_datetime_adj    flight_duration
    ##   <dttm>              <dttm>              <dttm>                        <dbl>
    ## 1 2013-01-01 05:17:00 2013-01-01 08:30:00 2013-01-01 08:30:00             193
    ## 2 2013-01-01 05:33:00 2013-01-01 08:50:00 2013-01-01 08:50:00             197
    ## 3 2013-01-01 05:42:00 2013-01-01 09:23:00 2013-01-01 09:23:00             221
    ## 4 2013-01-01 05:44:00 2013-01-01 10:04:00 2013-01-01 10:04:00             260
    ## 5 2013-01-01 05:54:00 2013-01-01 08:12:00 2013-01-01 08:12:00             138
    ## 6 2013-01-01 05:54:00 2013-01-01 07:40:00 2013-01-01 07:40:00             106

## Ejercicio 3: Extraer componentes de fechas

Extrae el dia de la semana y la hora en que salieron los aviones y
guardalos en las variables `dep_day_of_week` y `dep_hour`.

    # extraer el dia de la semana y la hora de salida
    flights_dt <- flights_dt %>%
      mutate(
        dep_day_of_week = wday(dep_datetime, label = TRUE, abbr = FALSE),  # extraer dia de la semana completo
        dep_hour = hour(dep_datetime)  # extraer la hora de salida
      ) %>%
      select(dep_datetime, dep_day_of_week, dep_hour)

    head(flights_dt)

    ## # A tibble: 6 × 3
    ##   dep_datetime        dep_day_of_week dep_hour
    ##   <dttm>              <ord>              <int>
    ## 1 2013-01-01 05:17:00 martes                 5
    ## 2 2013-01-01 05:33:00 martes                 5
    ## 3 2013-01-01 05:42:00 martes                 5
    ## 4 2013-01-01 05:44:00 martes                 5
    ## 5 2013-01-01 05:54:00 martes                 5
    ## 6 2013-01-01 05:54:00 martes                 5

## Ejercicio 4: Crear nuevas columnas con el día de la semana y la semana del año

Problema: Usando la columna `time_hour`, crea una nueva columna que
indique el día de la semana y otra que indique la semana del año en la
que ocurrió el vuelo.

Ayuda: Investiga la funcion wday de lubridate.

    # crear nuevas columnas con el dia de la semana y la semana del año
    flights_weekday <- flights %>%
      mutate(
        day_of_week = wday(time_hour, label = TRUE, abbr = FALSE),  # dia de la semana completo
        week_of_year = week(time_hour)  # numero de la semana del año
      ) %>%
      select(time_hour, day_of_week, week_of_year)

    head(flights_weekday)

    ## # A tibble: 6 × 3
    ##   time_hour           day_of_week week_of_year
    ##   <dttm>              <ord>              <dbl>
    ## 1 2013-01-01 05:00:00 martes                 1
    ## 2 2013-01-01 05:00:00 martes                 1
    ## 3 2013-01-01 05:00:00 martes                 1
    ## 4 2013-01-01 05:00:00 martes                 1
    ## 5 2013-01-01 06:00:00 martes                 1
    ## 6 2013-01-01 05:00:00 martes                 1

## Ejercicio 5: Encontrar los vuelos que salieron los fines de semana

Problema: Filtra los vuelos que despegaron un sábado o domingo y
devuelve el total de vuelos en fines de semana.

    # filtrar los vuelos que salieron un sabado o domingo
    weekend_flights <- flights %>%
      mutate(day_of_week = wday(time_hour, label = TRUE, abbr = FALSE)) %>%
      filter(day_of_week %in% c("sabado", "domingo"))  # filtrar sabados y domingos

    # contar el numero total de vuelos en fines de semana
    total_weekend_flights <- nrow(weekend_flights)

    # total de vuelos en fines de semana
    total_weekend_flights

    ## [1] 46357
