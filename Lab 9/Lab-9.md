Librerias

    library(dplyr)

    ## Warning: package 'dplyr' was built under R version 4.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(scales)

    ## Warning: package 'scales' was built under R version 4.3.2

Cargar datos

    titanic_data <- read.csv("titanic_MD.csv")
    titanic_real <- read.csv("titanic.csv")

1.  Reporte detallado de missing data para todas las columnas. (5%)

<!-- -->

    # Reporte detallado de missing values
    missing_data_report <- sapply(titanic_data, function(x) sum(is.na(x)))
    print(missing_data_report)

    ## PassengerId    Survived      Pclass        Name         Sex         Age 
    ##           0           0           0           0           0          25 
    ##       SibSp       Parch      Ticket        Fare       Cabin    Embarked 
    ##           3          12           0           8           0           0

    # Calcular el porcentaje de missing values en cada columna
    missing_percentage <- (missing_data_report / nrow(titanic_data)) * 100
    # Mostrar el porcentaje de missing values
    print(missing_percentage)

    ## PassengerId    Survived      Pclass        Name         Sex         Age 
    ##    0.000000    0.000000    0.000000    0.000000    0.000000   13.661202 
    ##       SibSp       Parch      Ticket        Fare       Cabin    Embarked 
    ##    1.639344    6.557377    0.000000    4.371585    0.000000    0.000000

    # Un solo data frame
    missing_summary <- data.frame(
      Column = names(missing_data_report),
      Missing_Count = missing_data_report,
      Missing_Percentage = missing_percentage
    )

    print(missing_summary)

    ##                  Column Missing_Count Missing_Percentage
    ## PassengerId PassengerId             0           0.000000
    ## Survived       Survived             0           0.000000
    ## Pclass           Pclass             0           0.000000
    ## Name               Name             0           0.000000
    ## Sex                 Sex             0           0.000000
    ## Age                 Age            25          13.661202
    ## SibSp             SibSp             3           1.639344
    ## Parch             Parch            12           6.557377
    ## Ticket           Ticket             0           0.000000
    ## Fare               Fare             8           4.371585
    ## Cabin             Cabin             0           0.000000
    ## Embarked       Embarked             0           0.000000

1.  Reporte de qué filas están completas (

<!-- -->

    # Identificar las filas completas
    complete_rows <- complete.cases(titanic_data)

    # Crear un reporte con las filas completas
    complete_rows_indices <- which(complete_rows)  # Índices de las filas completas
    complete_rows_report <- titanic_data[complete_rows, ]  # Subconjunto de filas completas

    # Número de filas completas
    num_complete_rows <- nrow(complete_rows_report)

    # Mostrar el reporte
    print(paste("Número de filas completas:", num_complete_rows))

    ## [1] "Número de filas completas: 141"

    print("Índices de filas completas:")

    ## [1] "Índices de filas completas:"

    print(complete_rows_indices)

    ##   [1]   1   2   3   6   8  10  11  12  16  17  18  19  20  22  23  25  26  27
    ##  [19]  28  29  30  32  33  34  35  36  37  39  41  44  46  47  49  50  51  53
    ##  [37]  54  55  56  57  58  59  60  61  62  64  65  67  68  69  70  73  74  75
    ##  [55]  76  77  78  79  80  81  82  84  85  86  87  88  90  91  92  93  94  95
    ##  [73]  96  97  98  99 100 101 102 105 106 107 109 110 112 113 114 115 116 117
    ##  [91] 118 119 120 121 123 124 125 126 128 129 131 132 133 134 135 136 137 138
    ## [109] 139 140 141 142 144 146 147 148 149 151 152 153 154 155 156 157 158 159
    ## [127] 160 161 162 166 168 169 170 171 172 173 174 175 177 178 182

1.  Utilizar los siguientes métodos para cada columna que contiene
    missing values: (50%)

<!-- -->

1.  Imputación general (media, moda y mediana)

<!-- -->

    # Función para calcular la moda
    get_mode <- function(x) {
      ux <- unique(na.omit(x))
      ux[which.max(tabulate(match(x, ux)))]
    }

    # Imputación con media
    titanic_data$Age_mean <- titanic_data$Age
    titanic_data$Age_mean[is.na(titanic_data$Age_mean)] <- mean(titanic_data$Age, na.rm = TRUE)

    # Imputación con mediana
    titanic_data$Age_median <- titanic_data$Age
    titanic_data$Age_median[is.na(titanic_data$Age_median)] <- median(titanic_data$Age, na.rm = TRUE)

    # Imputación con moda
    titanic_data$Embarked_mode <- titanic_data$Embarked
    titanic_data$Embarked_mode[is.na(titanic_data$Embarked_mode)] <- get_mode(titanic_data$Embarked)

    # Reporte
    print("Columnas imputadas con media, mediana y moda:")

    ## [1] "Columnas imputadas con media, mediana y moda:"

    print(head(titanic_data))

    ##   PassengerId Survived Pclass
    ## 1           2        1      1
    ## 2           4        1      1
    ## 3           7        0      1
    ## 4          11        1      3
    ## 5          12        1      1
    ## 6          22        1      2
    ##                                                  Name    Sex Age SibSp Parch
    ## 1 Cumings, Mrs. John Bradley (Florence Briggs Thayer)      ?  38     1     0
    ## 2        Futrelle, Mrs. Jacques Heath (Lily May Peel) female  35     1     0
    ## 3                             McCarthy, Mr. Timothy J   male  54     0     0
    ## 4                     Sandstrom, Miss. Marguerite Rut female  NA     1    NA
    ## 5                            Bonnell, Miss. Elizabeth female  58    NA     0
    ## 6                               Beesley, Mr. Lawrence   male  34     0     0
    ##     Ticket    Fare Cabin Embarked Age_mean Age_median Embarked_mode
    ## 1 PC 17599 71.2833   C85        C 38.00000       38.0             C
    ## 2   113803 53.1000  C123        S 35.00000       35.0             S
    ## 3    17463 51.8625   E46        S 54.00000       54.0             S
    ## 4  PP 9549 16.7000    G6        S 35.69253       35.5             S
    ## 5   113783 26.5500  C103        S 58.00000       58.0             S
    ## 6   248698 13.0000   D56        S 34.00000       34.0             S

1.  Modelo de regresión lineal

<!-- -->

    # Seleccionar filas con Age no faltante para entrenar el modelo
    train_data <- titanic_data[!is.na(titanic_data$Age), ]

    # Ajustar modelo de regresión lineal
    age_model <- lm(Age ~ Pclass + Sex + SibSp + Parch + Fare, data = train_data)

    # Predecir valores de Age faltantes
    missing_age_indices <- which(is.na(titanic_data$Age))
    titanic_data$Age_predicted <- titanic_data$Age
    titanic_data$Age_predicted[missing_age_indices] <- predict(age_model, titanic_data[missing_age_indices, ])

    # Reporte
    print("Columnas imputadas con modelo de regresión lineal:")

    ## [1] "Columnas imputadas con modelo de regresión lineal:"

    print(head(titanic_data))

    ##   PassengerId Survived Pclass
    ## 1           2        1      1
    ## 2           4        1      1
    ## 3           7        0      1
    ## 4          11        1      3
    ## 5          12        1      1
    ## 6          22        1      2
    ##                                                  Name    Sex Age SibSp Parch
    ## 1 Cumings, Mrs. John Bradley (Florence Briggs Thayer)      ?  38     1     0
    ## 2        Futrelle, Mrs. Jacques Heath (Lily May Peel) female  35     1     0
    ## 3                             McCarthy, Mr. Timothy J   male  54     0     0
    ## 4                     Sandstrom, Miss. Marguerite Rut female  NA     1    NA
    ## 5                            Bonnell, Miss. Elizabeth female  58    NA     0
    ## 6                               Beesley, Mr. Lawrence   male  34     0     0
    ##     Ticket    Fare Cabin Embarked Age_mean Age_median Embarked_mode
    ## 1 PC 17599 71.2833   C85        C 38.00000       38.0             C
    ## 2   113803 53.1000  C123        S 35.00000       35.0             S
    ## 3    17463 51.8625   E46        S 54.00000       54.0             S
    ## 4  PP 9549 16.7000    G6        S 35.69253       35.5             S
    ## 5   113783 26.5500  C103        S 58.00000       58.0             S
    ## 6   248698 13.0000   D56        S 34.00000       34.0             S
    ##   Age_predicted
    ## 1            38
    ## 2            35
    ## 3            54
    ## 4            NA
    ## 5            58
    ## 6            34

1.  Outliers: Uno de los dos métodos vistos en clase (Standard deviation
    approach o Percentile approach)

Metodo 1: desviacion estandar

    # Detectar outliers en Fare usando desviación estándar
    mean_fare <- mean(titanic_data$Fare, na.rm = TRUE)
    sd_fare <- sd(titanic_data$Fare, na.rm = TRUE)

    # Identificar outliers
    outliers_sd <- titanic_data$Fare > (mean_fare + 3 * sd_fare) | titanic_data$Fare < (mean_fare - 3 * sd_fare)

    # Sustituir outliers por la mediana
    titanic_data$Fare_outlier_handled <- titanic_data$Fare
    titanic_data$Fare_outlier_handled[outliers_sd] <- median(titanic_data$Fare, na.rm = TRUE)

    # Reporte
    print("Outliers manejados con método de desviación estándar:")

    ## [1] "Outliers manejados con método de desviación estándar:"

    print(head(titanic_data))

    ##   PassengerId Survived Pclass
    ## 1           2        1      1
    ## 2           4        1      1
    ## 3           7        0      1
    ## 4          11        1      3
    ## 5          12        1      1
    ## 6          22        1      2
    ##                                                  Name    Sex Age SibSp Parch
    ## 1 Cumings, Mrs. John Bradley (Florence Briggs Thayer)      ?  38     1     0
    ## 2        Futrelle, Mrs. Jacques Heath (Lily May Peel) female  35     1     0
    ## 3                             McCarthy, Mr. Timothy J   male  54     0     0
    ## 4                     Sandstrom, Miss. Marguerite Rut female  NA     1    NA
    ## 5                            Bonnell, Miss. Elizabeth female  58    NA     0
    ## 6                               Beesley, Mr. Lawrence   male  34     0     0
    ##     Ticket    Fare Cabin Embarked Age_mean Age_median Embarked_mode
    ## 1 PC 17599 71.2833   C85        C 38.00000       38.0             C
    ## 2   113803 53.1000  C123        S 35.00000       35.0             S
    ## 3    17463 51.8625   E46        S 54.00000       54.0             S
    ## 4  PP 9549 16.7000    G6        S 35.69253       35.5             S
    ## 5   113783 26.5500  C103        S 58.00000       58.0             S
    ## 6   248698 13.0000   D56        S 34.00000       34.0             S
    ##   Age_predicted Fare_outlier_handled
    ## 1            38              71.2833
    ## 2            35              53.1000
    ## 3            54              51.8625
    ## 4            NA              16.7000
    ## 5            58              26.5500
    ## 6            34              13.0000

Método 2: Percentiles

    # Detectar límites del percentil 1 y 99
    percentile_1 <- quantile(titanic_data$Fare, 0.01, na.rm = TRUE)
    percentile_99 <- quantile(titanic_data$Fare, 0.99, na.rm = TRUE)

    # Identificar outliers
    outliers_percentile <- titanic_data$Fare < percentile_1 | titanic_data$Fare > percentile_99

    # Sustituir outliers por la mediana
    titanic_data$Fare_outlier_handled_percentile <- titanic_data$Fare
    titanic_data$Fare_outlier_handled_percentile[outliers_percentile] <- median(titanic_data$Fare, na.rm = TRUE)

    # Reporte
    print("Outliers manejados con método de percentiles:")

    ## [1] "Outliers manejados con método de percentiles:"

    print(head(titanic_data))

    ##   PassengerId Survived Pclass
    ## 1           2        1      1
    ## 2           4        1      1
    ## 3           7        0      1
    ## 4          11        1      3
    ## 5          12        1      1
    ## 6          22        1      2
    ##                                                  Name    Sex Age SibSp Parch
    ## 1 Cumings, Mrs. John Bradley (Florence Briggs Thayer)      ?  38     1     0
    ## 2        Futrelle, Mrs. Jacques Heath (Lily May Peel) female  35     1     0
    ## 3                             McCarthy, Mr. Timothy J   male  54     0     0
    ## 4                     Sandstrom, Miss. Marguerite Rut female  NA     1    NA
    ## 5                            Bonnell, Miss. Elizabeth female  58    NA     0
    ## 6                               Beesley, Mr. Lawrence   male  34     0     0
    ##     Ticket    Fare Cabin Embarked Age_mean Age_median Embarked_mode
    ## 1 PC 17599 71.2833   C85        C 38.00000       38.0             C
    ## 2   113803 53.1000  C123        S 35.00000       35.0             S
    ## 3    17463 51.8625   E46        S 54.00000       54.0             S
    ## 4  PP 9549 16.7000    G6        S 35.69253       35.5             S
    ## 5   113783 26.5500  C103        S 58.00000       58.0             S
    ## 6   248698 13.0000   D56        S 34.00000       34.0             S
    ##   Age_predicted Fare_outlier_handled Fare_outlier_handled_percentile
    ## 1            38              71.2833                         71.2833
    ## 2            35              53.1000                         53.1000
    ## 3            54              51.8625                         51.8625
    ## 4            NA              16.7000                         16.7000
    ## 5            58              26.5500                         26.5500
    ## 6            34              13.0000                         13.0000

1.  Al comparar los métodos del inciso 4 contra “titanic.csv”, ¿Qué
    método (para cada columna) se acerca más a la realidad y por qué?
    (20%)

<!-- -->

    # Inicializar un reporte
    results <- data.frame(
      Column = character(),
      Method = character(),
      MAE = numeric(),
      stringsAsFactors = FALSE
    )

    #Para cada columna imputada, calcular métricas como el error absoluto medio (MAE), error cuadrático medio (MSE) o el porcentaje de aciertos para categorías.
    #MAE: es una medida de la diferencia entre dos variables continuas

    # Función para calcular MAE
    calculate_mae <- function(original, imputed) {
      mean(abs(original - imputed), na.rm = TRUE)
    }

    # Comparación para Age
    methods_age <- c("Age_mean", "Age_median", "Age_predicted")
    for (method in methods_age) {
      mae <- calculate_mae(titanic_real$Age, titanic_data[[method]])
      results <- rbind(results, data.frame(Column = "Age", Method = method, MAE = mae))
    }

    # Comparación para Embarked (Categorical Accuracy)
    embarked_mode <- titanic_data$Embarked_mode
    accuracy_embarked <- mean(titanic_real$Embarked == embarked_mode, na.rm = TRUE)
    results <- rbind(results, data.frame(Column = "Embarked", Method = "Mode", MAE = 1 - accuracy_embarked))

    # Comparación para Fare (Outliers con SD y Percentiles)
    fare_methods <- c("Fare_outlier_handled", "Fare_outlier_handled_percentile")
    for (method in fare_methods) {
      mae <- calculate_mae(titanic_real$Fare, titanic_data[[method]])
      results <- rbind(results, data.frame(Column = "Fare", Method = method, MAE = mae))
    }

    # Mostrar resultados
    print("Resultados de comparación entre métodos de imputación:")

    ## [1] "Resultados de comparación entre métodos de imputación:"

    print(results)

    ##     Column                          Method        MAE
    ## 1      Age                        Age_mean 1.58973992
    ## 2      Age                      Age_median 1.59289617
    ## 3      Age                   Age_predicted 1.18439922
    ## 4 Embarked                            Mode 0.06557377
    ## 5     Fare            Fare_outlier_handled 5.20457143
    ## 6     Fare Fare_outlier_handled_percentile 5.85519086

Utilizando la misma tabla de “titanic\_MD.csv” en R realice lo
siguiente: 1. Luego del pre-procesamiento de la data con Missing Values,
normalice las columnas numéricas por los métodos: (50%)

1.  Standarization
2.  MinMaxScaling
3.  MaxAbsScaler

<!-- -->

    library(scales)
    # Seleccionar las columnas para normalizar
    numeric_columns <- c("Age", "Fare", "SibSp", "Parch")

    # Filtrar los valores faltantes
    titanic_data_clean <- titanic_data[complete.cases(titanic_data[numeric_columns]), ]


    # Standardization (Z-score normalization)
    titanic_data_clean$Age_standardized <- scale(titanic_data_clean$Age)
    titanic_data_clean$Fare_standardized <- scale(titanic_data_clean$Fare)
    titanic_data_clean$SibSp_standardized <- scale(titanic_data_clean$SibSp)
    titanic_data_clean$Parch_standardized <- scale(titanic_data_clean$Parch)

    print("Standardization (Z-score) results:")

    ## [1] "Standardization (Z-score) results:"

    head(titanic_data_clean[c("Age_standardized", "Fare_standardized", "SibSp_standardized", "Parch_standardized")])

    ##    Age_standardized Fare_standardized SibSp_standardized Parch_standardized
    ## 1        0.16957262        -0.1317692          0.9178876         -0.6288754
    ## 2       -0.02346581        -0.3620045          0.9178876         -0.6288754
    ## 3        1.19911092        -0.3776736         -0.7413707         -0.6288754
    ## 6       -0.08781195        -0.8697471         -0.7413707         -0.6288754
    ## 8       -1.05300410         2.2957303          4.2364041          1.9045942
    ## 10       1.90691849        -0.2495769         -0.7413707          0.6378594

    # MinMax Scaling
    titanic_data_clean$Age_minmax <- rescale(titanic_data_clean$Age)
    titanic_data_clean$Fare_minmax <- rescale(titanic_data_clean$Fare)
    titanic_data_clean$SibSp_minmax <- rescale(titanic_data_clean$SibSp)
    titanic_data_clean$Parch_minmax <- rescale(titanic_data_clean$Parch)

    print("MinMax Scaling results:")

    ## [1] "MinMax Scaling results:"

    head(titanic_data_clean[c("Age_minmax", "Fare_minmax", "SibSp_minmax", "Parch_minmax")])

    ##    Age_minmax Fare_minmax SibSp_minmax Parch_minmax
    ## 1   0.5291096  0.13913574    0.3333333         0.00
    ## 2   0.4863014  0.10364430    0.3333333         0.00
    ## 3   0.7574201  0.10122886    0.0000000         0.00
    ## 6   0.4720320  0.02537431    0.0000000         0.00
    ## 8   0.2579909  0.51334181    1.0000000         0.50
    ## 10  0.9143836  0.12097534    0.0000000         0.25

    # MaxAbs Scaler (Escalado basado en el valor absoluto máximo)
    titanic_data_clean$Age_maxabs <- titanic_data_clean$Age / max(abs(titanic_data_clean$Age), na.rm = TRUE)
    titanic_data_clean$Fare_maxabs <- titanic_data_clean$Fare / max(abs(titanic_data_clean$Fare), na.rm = TRUE)
    titanic_data_clean$SibSp_maxabs <- titanic_data_clean$SibSp / max(abs(titanic_data_clean$SibSp), na.rm = TRUE)
    titanic_data_clean$Parch_maxabs <- titanic_data_clean$Parch / max(abs(titanic_data_clean$Parch), na.rm = TRUE)

    print("MaxAbs Scaling results:")

    ## [1] "MaxAbs Scaling results:"

    head(titanic_data_clean[c("Age_maxabs", "Fare_maxabs", "SibSp_maxabs", "Parch_maxabs")])

    ##    Age_maxabs Fare_maxabs SibSp_maxabs Parch_maxabs
    ## 1   0.5352113  0.13913574    0.3333333         0.00
    ## 2   0.4929577  0.10364430    0.3333333         0.00
    ## 3   0.7605634  0.10122886    0.0000000         0.00
    ## 6   0.4788732  0.02537431    0.0000000         0.00
    ## 8   0.2676056  0.51334181    1.0000000         0.50
    ## 10  0.9154930  0.12097534    0.0000000         0.25

1.  Compare los estadísticos que considere más importantes para su
    conclusión y compare contra la data completa de “titanic.csv”
    (deberán de normalizar también). (50%)

<!-- -->

    # Limpiar los datos completos (sin valores faltantes) en las columnas de interés
    numeric_columns <- c("Age", "Fare", "SibSp", "Parch")

    titanic_data_clean <- titanic_data_clean[complete.cases(titanic_data_clean[numeric_columns]), ]
    titanic_real <- titanic_real[complete.cases(titanic_real[numeric_columns]), ]


    # Normalización en el dataset completo
    # a. Standardization
    titanic_real$Age_standardized <- scale(titanic_real$Age)
    titanic_real$Fare_standardized <- scale(titanic_real$Fare)
    titanic_real$SibSp_standardized <- scale(titanic_real$SibSp)
    titanic_real$Parch_standardized <- scale(titanic_real$Parch)

    # b. MinMax Scaling
    titanic_real$Age_minmax <- rescale(titanic_real$Age)
    titanic_real$Fare_minmax <- rescale(titanic_real$Fare)
    titanic_real$SibSp_minmax <- rescale(titanic_real$SibSp)
    titanic_real$Parch_minmax <- rescale(titanic_real$Parch)

    # c. MaxAbs Scaler
    titanic_real$Age_maxabs <- titanic_real$Age / max(abs(titanic_real$Age), na.rm = TRUE)
    titanic_real$Fare_maxabs <- titanic_real$Fare / max(abs(titanic_real$Fare), na.rm = TRUE)
    titanic_real$SibSp_maxabs <- titanic_real$SibSp / max(abs(titanic_real$SibSp), na.rm = TRUE)
    titanic_real$Parch_maxabs <- titanic_real$Parch / max(abs(titanic_real$Parch), na.rm = TRUE)

    # Calcular estadísticos descriptivos para las columnas normalizadas en ambos datasets
    # Dataset limpio con valores faltantes imputados
    stats_clean <- titanic_data_clean %>%
      select(Age_standardized, Fare_standardized, SibSp_standardized, Parch_standardized,
             Age_minmax, Fare_minmax, SibSp_minmax, Parch_minmax,
             Age_maxabs, Fare_maxabs, SibSp_maxabs, Parch_maxabs) %>%
      summarise_all(list(
        min = min,
        max = max,
        mean = mean,
        sd = sd
      ))

    # Dataset completo
    stats_full <- titanic_real %>%
      select(Age_standardized, Fare_standardized, SibSp_standardized, Parch_standardized,
             Age_minmax, Fare_minmax, SibSp_minmax, Parch_minmax,
             Age_maxabs, Fare_maxabs, SibSp_maxabs, Parch_maxabs) %>%
      summarise_all(list(
        min = min,
        max = max,
        mean = mean,
        sd = sd
      ))

    # Mostrar los resultados comparativos
    print("Estadísticos descriptivos del dataset con valores imputados:")

    ## [1] "Estadísticos descriptivos del dataset con valores imputados:"

    print(stats_clean)

    ##   Age_standardized_min Fare_standardized_min SibSp_standardized_min
    ## 1            -2.216382             -1.034352             -0.7413707
    ##   Parch_standardized_min Age_minmax_min Fare_minmax_min SibSp_minmax_min
    ## 1             -0.6288754              0               0                0
    ##   Parch_minmax_min Age_maxabs_min Fare_maxabs_min SibSp_maxabs_min
    ## 1                0     0.01295775               0                0
    ##   Parch_maxabs_min Age_standardized_max Fare_standardized_max
    ## 1                0             2.292995              5.452714
    ##   SibSp_standardized_max Parch_standardized_max Age_minmax_max Fare_minmax_max
    ## 1               4.236404               4.438064              1               1
    ##   SibSp_minmax_max Parch_minmax_max Age_maxabs_max Fare_maxabs_max
    ## 1                1                1              1               1
    ##   SibSp_maxabs_max Parch_maxabs_max Age_standardized_mean
    ## 1                1                1         -7.270152e-17
    ##   Fare_standardized_mean SibSp_standardized_mean Parch_standardized_mean
    ## 1          -6.481068e-17            7.874845e-17           -2.522654e-17
    ##   Age_minmax_mean Fare_minmax_mean SibSp_minmax_mean Parch_minmax_mean
    ## 1       0.4915051        0.1594483         0.1489362         0.1241135
    ##   Age_maxabs_mean Fare_maxabs_mean SibSp_maxabs_mean Parch_maxabs_mean
    ## 1       0.4980941        0.1594483         0.1489362         0.1241135
    ##   Age_standardized_sd Fare_standardized_sd SibSp_standardized_sd
    ## 1                   1                    1                     1
    ##   Parch_standardized_sd Age_minmax_sd Fare_minmax_sd SibSp_minmax_sd
    ## 1                     1     0.2217601      0.1541529        0.200893
    ##   Parch_minmax_sd Age_maxabs_sd Fare_maxabs_sd SibSp_maxabs_sd Parch_maxabs_sd
    ## 1       0.1973578     0.2188866      0.1541529        0.200893       0.1973578

    print("Estadísticos descriptivos del dataset completo:")

    ## [1] "Estadísticos descriptivos del dataset completo:"

    print(stats_full)

    ##   Age_standardized_min Fare_standardized_min SibSp_standardized_min
    ## 1            -2.221601             -1.030579             -0.7210661
    ##   Parch_standardized_min Age_minmax_min Fare_minmax_min SibSp_minmax_min
    ## 1             -0.6300014              0               0                0
    ##   Parch_minmax_min Age_maxabs_min Fare_maxabs_min SibSp_maxabs_min
    ## 1                0         0.0115               0                0
    ##   Parch_maxabs_min Age_standardized_max Fare_standardized_max
    ## 1                0             2.833416              5.679882
    ##   SibSp_standardized_max Parch_standardized_max Age_minmax_max Fare_minmax_max
    ## 1               3.936172                 4.6707              1               1
    ##   SibSp_minmax_max Parch_minmax_max Age_maxabs_max Fare_maxabs_max
    ## 1                1                1              1               1
    ##   SibSp_maxabs_max Parch_maxabs_max Age_standardized_mean
    ## 1                1                1         -1.873122e-16
    ##   Fare_standardized_mean SibSp_standardized_mean Parch_standardized_mean
    ## 1          -9.183015e-17           -3.873268e-17            5.580975e-17
    ##   Age_minmax_mean Fare_minmax_mean SibSp_minmax_mean Parch_minmax_mean
    ## 1       0.4394844         0.153578          0.154827         0.1188525
    ##   Age_maxabs_mean Fare_maxabs_mean SibSp_maxabs_mean Parch_maxabs_mean
    ## 1       0.4459303         0.153578          0.154827         0.1188525
    ##   Age_standardized_sd Fare_standardized_sd SibSp_standardized_sd
    ## 1                   1                    1                     1
    ##   Parch_standardized_sd Age_minmax_sd Fare_minmax_sd SibSp_minmax_sd
    ## 1                     1     0.1978233      0.1490211       0.2147195
    ##   Parch_minmax_sd Age_maxabs_sd Fare_maxabs_sd SibSp_maxabs_sd Parch_maxabs_sd
    ## 1       0.1886543     0.1955483      0.1490211       0.2147195       0.1886543
