Taller 05
================
dacarras
Thu Apr 20, 2023

------------------------------------------------------------------------

# Instrucciones

En este taller, vamos a resolver diferentes ejercicios, de modo que
tengan ejemplos de codigos para resolver la tarea 05. En la tarea 05, se
emplean datos de un ensayo clínico de pacientes con anorexia nerviosa y
se evala la efectividad de un tratamiento. Vamos a ocupar datos
similares, de un “delayed control” donde la evaluación del éxito
terapeutico es sobre puntajes de estress post traumatico (i.e., *post
traumatic stress disorder* o PTSD). Se emplean datos de personas que
padecen PSTD, producto del conflicto de la guerra civil en Sierra Leona
(ver Mughal et al., 2015).

El diseño consiste en un quasi experimento, en que se comparan pacientes
con y sin tratamiento. En este caso, se asume que los pacientes “no
atendidos aún”, debieran ser similares a los pacientes tratados, y por
tanto conforman a un grupo de control valido. Este tipo de diseños no
constituye a un experimento puro; pero dado el contexto de post-guerra
civil, es muy difícil implementar un experimento puro.

Uno de los argumentos que los investigadores presentan, es que al menos,
para la mayoria de las variables, que no son la respuesta de interés,
como los sociodemográficos, y otras covariables, los datos recogidos
indican que hay similitud entre el grupo tratado, y quienes aun no han
sido tratados. Esta es una limitación importante del estudio.

No obstante, para los fines que queremos ilustrar en este taller, estos
datos presentan una estructura muy similar a los que vamos a emplear en
el tarea 05. Esto es, contamos con una variable de respuesta, contamos
con una variable categórica que tiene dos valores (tratados, y no
tratados). A continuación, tenemos un libro de códigos breve:


    |variable |descripción                       |col_type 
    |:--------|:---------------------------------|:--------
    |id_i     |id (unique case id)               |dbl      
    |sex      |Sex (1 = male, 0 = female)        |dbl      
    |grp      |Groups (c = control, t = treated) |chr      
    |ptsd     |post traumatic stress disorder    |dbl      
    |ifor     |Intergroup forgiveness            |dbl      
    |anxi     |Anxiety                           |dbl      
    |outb     |Outgroup blame                    |dbl      

Las variables `ptsd`, `ifor`, `anxi`, y `outb` son variables que
provienen de escalas de multiples items, que tienen espacios de
repsuesta de 1 a 5. Estas variables alojan puntajes promedio en sobre
estos items, e indican mayor presencia del atributo a mayor.

Convencionalmente, se espera que el proceso terapeútico promueva que los
tratados, presenten menores niveles de

- Estres post traumático (`ptsd`)

- Perdón integrupal (`ifor`)

- Anxiedad intergrupal (`anxi`)

- Sentimientos de culpar a los otros (`outb`)

- El archivo que contiene los datos que vamos a emplear se llama:

<!-- -->


    ptsd_data_scores.csv

**Nota**: Los datos que usaremos son datos reales, y las resultados que
podemos producir de forma parcial, se pueden revisar en Mughal et al
(2015).

## Referencias

Mughal, U., Carrasco, D., Brown, R., & Ayers, S. (2015). Rehabilitating
civilian victims of war through psychosocial intervention in Sierra
Leone. Journal of Applied Social Psychology, 45(11), 593–601.
<https://doi.org/10.1111/jasp.12322>

------------------------------------------------------------------------

# Tarea 05

## Ejercicio 1. Abrir los datos.

- Abra los datos `ptsd_data_scores.csv` usando la función `read.csv()`.
  Use un objeto llamado `data_model` para guardar los datos.

``` r
# Instrucciones: Pegue o escriba los códigos utilizados en las siguientes 
#                líneas [no coloque el signo gato antes de su respuesta]
#                Una vez terminado su código, borre estos comentarios.

data_model <- read.csv('ptsd_data_scores.csv')
```

## Ejercicio 2. Vista previa de a los datos.

- **¿Cuántas variables y cuántos casos posee la base de datos
  original?**
- Indique su respuesta bajo el código.

``` r
# Instrucciones: Escriba aqui un comando para obtener la 
#                cantidad de variables, y de casos observados
#                de la base de datos empleada. Se sugiere emplear
#                dplyr::glimpse()


dplyr::glimpse(data_model)
```

    ## Rows: 100
    ## Columns: 7
    ## $ id_i [3m[38;5;246m<int>[39m[23m 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19…
    ## $ sex  [3m[38;5;246m<int>[39m[23m 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1,…
    ## $ grp  [3m[38;5;246m<chr>[39m[23m "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", …
    ## $ ptsd [3m[38;5;246m<dbl>[39m[23m 4.466667, 3.733333, 2.666667, 4.466667, 4.266667, 4.600000, 3.000…
    ## $ ifor [3m[38;5;246m<dbl>[39m[23m 3.5, 4.0, 1.0, 4.0, 4.5, 3.0, 3.0, 3.0, 3.5, 5.0, 3.5, 1.0, 4.0, …
    ## $ anxi [3m[38;5;246m<dbl>[39m[23m 4.000000, 2.000000, 2.166667, 2.000000, 3.000000, 5.000000, 1.666…
    ## $ outb [3m[38;5;246m<dbl>[39m[23m 5.0, 5.0, 1.0, 5.0, 4.5, 1.0, 2.5, 5.0, 3.5, 5.0, 1.0, 5.0, 4.0, …

- Respuesta
  - Casos: 100
  - Variables: 7

## Ejercicio 3. Generar muestra aleatoria

Como en la tarea 1, buscamos resultados únicos para cada estudiante.
Para eso te pedimos generar una muestra de datos única usando tu RUT.
Solo debes cambiar el valor de `set.seed()` para obtener una muestra
diferente. Recuerda que **todos los ejercicios siguientes**
**necesitan** **usar estos datos generados**.

``` r
# [ no necisitamos ilustrar como generar una base de datos aleatoria]
```

## Ejercicio 4. Crear una variable dummy, y una variable de *deviation*.

Para evaluar el efecto del tratamiento, necesitamos una variable *dummy*
con dos valores: cero y uno. Además, vamos a crear una variable de
*deviation coding*. Estas son las dos formas más populares de crear
variables numericas, que representan a variables categoricas de dos
valores, y que nos permiten realizar comparaciones entre grupos
empleando modelos de regresión.

``` r
# -----------------------------------------------
# frecuencia por grupo
# -----------------------------------------------

dplyr::count(data_model, grp)
```

    ##   grp  n
    ## 1   c 50
    ## 2   t 50

``` r
# -----------------------------------------------
# recodificación de variables
# -----------------------------------------------

data_model <- data_model %>%
              mutate(dummy = case_when(
              grp == 'c' ~ 0,
              grp == 't' ~ 1
              )) %>%
              mutate(dev = case_when(
              grp == 'c' ~ -1,
              grp == 't' ~  1
              )) %>%
              dplyr::glimpse()
```

    ## Rows: 100
    ## Columns: 9
    ## $ id_i  [3m[38;5;246m<int>[39m[23m 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 1…
    ## $ sex   [3m[38;5;246m<int>[39m[23m 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 1…
    ## $ grp   [3m[38;5;246m<chr>[39m[23m "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c", "c",…
    ## $ ptsd  [3m[38;5;246m<dbl>[39m[23m 4.466667, 3.733333, 2.666667, 4.466667, 4.266667, 4.600000, 3.00…
    ## $ ifor  [3m[38;5;246m<dbl>[39m[23m 3.5, 4.0, 1.0, 4.0, 4.5, 3.0, 3.0, 3.0, 3.5, 5.0, 3.5, 1.0, 4.0,…
    ## $ anxi  [3m[38;5;246m<dbl>[39m[23m 4.000000, 2.000000, 2.166667, 2.000000, 3.000000, 5.000000, 1.66…
    ## $ outb  [3m[38;5;246m<dbl>[39m[23m 5.0, 5.0, 1.0, 5.0, 4.5, 1.0, 2.5, 5.0, 3.5, 5.0, 1.0, 5.0, 4.0,…
    ## $ dummy [3m[38;5;246m<dbl>[39m[23m 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ dev   [3m[38;5;246m<dbl>[39m[23m -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, …

## Ejercicio 5. Revisión de variable dummy

Después de crear la variable dummy y la variable deviation, vamos a
crear una tabla para revisar que la forma de recodificación cumpla con
lo que esperamos.

``` r
# -----------------------------------------------
# frecuencia por grupo, y recodificación
# -----------------------------------------------

dplyr::count(data_model, grp, dummy, dev)
```

    ##   grp dummy dev  n
    ## 1   c     0  -1 50
    ## 2   t     1   1 50

``` r
# -----------------------------------------------
# tabla de contigencia
# -----------------------------------------------

xtabs(~ grp + dummy, data = data_model)
```

    ##    dummy
    ## grp  0  1
    ##   c 50  0
    ##   t  0 50

``` r
xtabs(~ grp + dev, data = data_model)
```

    ##    dev
    ## grp -1  1
    ##   c 50  0
    ##   t  0 50

## Ejercicio 6. Evaluación del tratamiento en más de una variable de respuesta.

En este estudio, los investigadores emplearon hasta 4 variables de
respuesta diferentes. Vamos a ajustar regresiones para cada una de
ellas. Vamos a emplear la variable dummy primero. Esta nos permite
estimar la diferencia de la variable de respuesta, entre el grupo
control, y el grupo de los tratados. En este caso, la pendiente asociada
a la variable dummy, es la diferencia entre grupos.

``` r
# -----------------------------------------------
# post traumatic stress disorder
# -----------------------------------------------
lm(ptsd ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***
    ## dummy       -1.29867    0.13383  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

``` r
# -----------------------------------------------
# intergroup forgiveness
# -----------------------------------------------
lm(ifor ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ifor ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ##  -2.07  -1.01  -0.01   0.93   1.99 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   3.0700     0.1658  18.513   <2e-16 ***
    ## dummy        -0.0600     0.2345  -0.256    0.799    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.173 on 98 degrees of freedom
    ## Multiple R-squared:  0.0006675,  Adjusted R-squared:  -0.00953 
    ## F-statistic: 0.06545 on 1 and 98 DF,  p-value: 0.7986

``` r
# -----------------------------------------------
# intergroup anxiety
# -----------------------------------------------
lm(anxi ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = anxi ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.51667 -1.05833 -0.01667  0.81667  2.42000 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   3.5167     0.1564  22.490  < 2e-16 ***
    ## dummy        -0.9367     0.2211  -4.236 5.15e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.106 on 98 degrees of freedom
    ## Multiple R-squared:  0.1547, Adjusted R-squared:  0.1461 
    ## F-statistic: 17.94 on 1 and 98 DF,  p-value: 5.147e-05

``` r
# -----------------------------------------------
# outgroup blame
# -----------------------------------------------
lm(outb ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = outb ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ##  -2.66  -1.16   0.09   1.09   2.59 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   3.6600     0.1863  19.646  < 2e-16 ***
    ## dummy        -1.2500     0.2635  -4.744 7.12e-06 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.317 on 98 degrees of freedom
    ## Multiple R-squared:  0.1868, Adjusted R-squared:  0.1785 
    ## F-statistic: 22.51 on 1 and 98 DF,  p-value: 7.117e-06

- **Preguntas**
  - ¿En qué variables de respuesta observa diferencias entre los grupos,
    y en cuales no?

## Ejercicio 7. Modelo nulo e información que nos entrega.

De acuerdo a lo revisado en el capítulo 8 de Vik (2014), el modelo nulo
en una regresión es importante para comparar medias entre grupos. Este
modelo sin predictores muestra la variabilidad total de la variable de
respuesta.

Vamos a calcular el modelo nulo sobre la variable de respuesta `pstd`, y
vamos a extraer información descriptiva sobre este modelo.

1)  ¿Cuál es el promedio de peso en los pacientes evaluados? Indica la
    cifra y en qué parte de la salida de la regresión la encontraste,
    copiando la línea del output con este resultado.

2)  ¿Cuál es la desviación estándar de la variable de respuesta? Indica
    la cifra y en qué parte de la salida de la regresión la encontraste,
    copiando la línea del output con este resultado.

``` r
# -----------------------------------------------
# post traumatic stress disorder, null model
# -----------------------------------------------

lm(ptsd ~ 1 , data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1, data = data_model)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.02800 -0.49467  0.00533  0.72200  1.50533 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.29467    0.09323   35.34   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9323 on 99 degrees of freedom

``` r
# -----------------------------------------------
# descriptivos
# -----------------------------------------------

data_model %>%
summarize(
mean = mean(ptsd, na.rm = TRUE),
sd = sd(ptsd, na.rm = TRUE),
) %>%
knitr::kable(., digits = 4)
```

|   mean |     sd |
|-------:|-------:|
| 3.2947 | 0.9323 |

- Resultados

  a.1) Promedio observado:

<!-- -->

    3.94400

a.2) Copia de la línea donde se encuentra el promedio:

    (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***

b.1) Desviación estándar

    0.6692

a.2) Copia de la línea donde se encuentra el promedio:

    Residual standard error: 0.6692 on 98 degrees of freedom

## Ejercicio 7. Ajuste el modelo aumentado

Vik (2014) llama modelo aumentado al que incluye covariables de interés.
Vamos a ajustar el modelo con la covariable de tratamiento, empleando la
versión en dummy coding.

``` r
# -----------------------------------------------
# post traumatic stress disorder, augmented model
# -----------------------------------------------

lm(ptsd ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***
    ## dummy       -1.29867    0.13383  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

## Ejercicio 7.1. Ajuste el modelo aumentado con deviation coding.

Vamos a ajustar el modelo con deviation coding, para ilustrar que
signfica esta parametrización.

``` r
# -----------------------------------------------
# post traumatic stress disorder, null model
# -----------------------------------------------

lm(ptsd ~ 1 + dev, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dev, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.29467    0.06692  49.235  < 2e-16 ***
    ## dev         -0.64933    0.06692  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

``` r
# -----------------------------------------------
# descriptivos
# -----------------------------------------------

data_model %>%
summarize(
mean = mean(ptsd, na.rm = TRUE),
sd = sd(ptsd, na.rm = TRUE),
) %>%
knitr::kable(., digits = 5)
```

|    mean |      sd |
|--------:|--------:|
| 3.29467 | 0.93228 |

``` r
# -----------------------------------------------
# descriptivos por grupo
# -----------------------------------------------

data_model %>%
group_by(grp) %>%
summarize(
mean = mean(ptsd, na.rm = TRUE),
sd = sd(ptsd, na.rm = TRUE),
) %>%
knitr::kable(., digits = 5)
```

| grp |    mean |      sd |
|:----|--------:|--------:|
| c   | 3.94400 | 0.61314 |
| t   | 2.64533 | 0.72085 |

``` r
# -----------------------------------------------
# comparacion de amobos
# -----------------------------------------------

model_dum <- lm(ptsd ~ 1 + dummy, data = data_model)
model_dev <- lm(ptsd ~ 1 + dev, data = data_model)

#------------------------------------------------
# display estimates
#------------------------------------------------

texreg::screenreg(
    list(model_dum, model_dev),
    star.symbol = "*", 
    center = TRUE, 
    doctype = FALSE,
    dcolumn = TRUE, 
    booktabs = TRUE,
    single.row = FALSE
    )
```

    ## 
    ## ===================================
    ##              Model 1     Model 2   
    ## -----------------------------------
    ## (Intercept)    3.94 ***    3.29 ***
    ##               (0.09)      (0.07)   
    ## dummy         -1.30 ***            
    ##               (0.13)               
    ## dev                       -0.65 ***
    ##                           (0.07)   
    ## -----------------------------------
    ## R^2            0.49        0.49    
    ## Adj. R^2       0.48        0.48    
    ## Num. obs.    100         100       
    ## ===================================
    ## *** p < 0.001; ** p < 0.01; * p < 0.05

- **Preguntas**
  - ¿Por qué el intercepto de este modelo, se parece tanto a la media de
    la muestra?
  - ¿Por qué el coeficiente de este modelo, es casi la mitad del primer
    modelo con dummy coding?
  - Representemos la recta de regresión en un diagrama para ver que
    significa cada término.

## Ejercicio 8. Efecto esperado.

¿Cuales son los puntajes de PTSD esperados de los pacientes tratados?

``` r
# -----------------------------------------------
# deviation coding
# -----------------------------------------------

lm(ptsd ~ 1 + dev, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dev, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.29467    0.06692  49.235  < 2e-16 ***
    ## dev         -0.64933    0.06692  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

``` r
# -----------------------------------------------
# dummy coding
# -----------------------------------------------

lm(ptsd ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***
    ## dummy       -1.29867    0.13383  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

- Resultado
  - con deviation coding: intercepto + pendiente = 3.29467 -0.64933 =
    2.64
  - con dummy coding: intercepto + pendiente = 3.94400 - 1.2986 = 2.64

**Nota**: en ambos casos, si quiero obtener la media del grupo tratado,
me basta con sumar al intercepto la pendiente.

## Ejercicio 9. Tamaño de efecto

¿Cuál es el tamaño de efecto observado, en términos de la métrica de la
variable de respuesta?

- Resultado
  - b1 = -1.29866 (pendiente del modelo con dummy coding).

**Nota**: Sólo la pendiente del modelo con dummy coding me entrega el
tamaño de efecto del modelo. Este decir, la distancia entre el grupo
control, y el grupo tratado. Si quisieramos obtener, la misma cifra, con
el deviation coding, necesitamos multiplicar a la pendiente por dos
(`2 * -0.64933 = -1.29866`)

## Ejercicio 10. Interpretación y descripción de resultados.

Considerando los resultados anteriores, describa los resultados
encontrados respecto a la efectividad del tratamiento. Guiese por medio
del capitlo de Huck (2012, capitulo 16, p367).

``` r
# -----------------------------------------------
# dummy coding
# -----------------------------------------------

lm(ptsd ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***
    ## dummy       -1.29867    0.13383  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

- Resultado
  - El grupo de personas que participa del tratamiento, presenta
    puntajes menores, que el grupo control
    `(b = -1.30, SE = .13, t = -9.7, p < .001)`. Empleamos un modelo de
    regresión para realizar la comparación entre el grupo de tratados, y
    el grupo control (i.e., en lista de espera). Este modelo, con solo
    el predictor que diferencia entre los trataos, y los no tratados, d
    acuenta de hasta un 49% de la variabilidad de los puntajes de PTSD
    (F (1, 98) = 94.16, p \< .001).

## Ejercicio 11. Formalización de hipotesis.

- Comparación de modelos.
  - Nuestro modelo nulo, representa a nuestra hipotesis nula, donde el
    predictor tiene un efecto cero.
  - Nuestro modelo modelo aumentado, representa a nuestra hipotesis
    alternativa.
  - Una prueba de “ANOVA”, permite comparar a ambos podemos modelos, y
    favorecer que nos quedaramos con el modelo aumentado, en contraste
    al modelo nulo.

``` r
# -----------------------------------------------
# ajustar ambos modelos por pasos
# -----------------------------------------------

modelo_nulo <- lm(ptsd ~ 1, data = data_model)
modelo_aumentado <- lm(ptsd ~ 1 + dummy, data = data_model)

# -----------------------------------------------
# comparar ambos modelos
# -----------------------------------------------

anova(modelo_nulo, modelo_aumentado)
```

    ## Analysis of Variance Table
    ## 
    ## Model 1: ptsd ~ 1
    ## Model 2: ptsd ~ 1 + dummy
    ##   Res.Df    RSS Df Sum of Sq     F    Pr(>F)    
    ## 1     99 86.046                                 
    ## 2     98 43.883  1    42.163 94.16 5.324e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# Res.Df    RSS Df Sum of Sq     F    Pr(>F)    
#     99 86.046                                 
#     98 43.883  1    42.163 94.16 5.324e-16 ***


# F = (42.163/1)/(43.883/98) = 94.16

# Nota: F es un ratio. El numerador, es la suma de cuadrado del modelo ajustado,
#       dividido por sus grados de libertad (i.e. df en este output).
#       El denominador, es la suma de cuadrados que el modelo no puede prodcuir (i.e., 
#       le restamos la suma de cuadrados del modelo aumentado, al modelo nulo).
#       Corregimos esta suma de cuadrados, por los grados de libertad.
#       Una vez que tenemos ambas cifras, generamos una fracción (i.e., ratio).
#       Este ratio se encuentra en una métrica de "varianza".
#       Esta nos indica cuantas veces "cabe" la varianza explicada por los efectos
#       fijos del modelo, sobre la varianza no explicada por el model (i.e.
#       varianza residual).


# -----------------------------------------------
# modelo con dummy coding
# -----------------------------------------------

lm(ptsd ~ 1 + dummy, data = data_model) %>%
summary()
```

    ## 
    ## Call:
    ## lm(formula = ptsd ~ 1 + dummy, data = data_model)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.3787 -0.5440  0.1547  0.5547  1.1547 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.94400    0.09463  41.676  < 2e-16 ***
    ## dummy       -1.29867    0.13383  -9.704 5.32e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6692 on 98 degrees of freedom
    ## Multiple R-squared:   0.49,  Adjusted R-squared:  0.4848 
    ## F-statistic: 94.16 on 1 and 98 DF,  p-value: 5.324e-16

``` r
# Nota: la prueba F del modelo con una sola covariable, de forma implicita
#       incluye al modelo nulo. Genera al mismo F, y este es el valor
#       empleado en la prueba de hipotesis. Es un contrastre entre el modelo
#       sin predictores (nulo), con el modelo con los predictores presentes.
#       El F, es un ratio 

# -----------------------------------------------
# tabla de anoval del modelo con una covariable
# -----------------------------------------------

summary(aov(modelo_aumentado))
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## dummy        1  42.16   42.16   94.16 5.32e-16 ***
    ## Residuals   98  43.88    0.45                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# F = (42.163/1)/(43.883/98) = 94.16
```
