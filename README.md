# Predicción usando R en el Titanic

Bienvenido al taller de predicción supervisada con R usando el famoso dataset del Titanic. Este ejercicio suele ser muy conocido entre la comunidad de científicos de datos, por lo tanto encontrarás mucha información en el internet. Sin embargo el objetivo de este taller, es que pongas en práctica los conocimientos de R para el tratamiento de datos y luego en la ejecución del modelo en sí.

## 1.- Analizando el dataset

Usaremos un archivo CSV con datos de los pasajeros del Titanic.

Archivo: https://github.com/agus100cia/r-supervisado-challenge/blob/master/titanicdf.csv

COLUMNAS:

- PassengerId: secuencial
- Survived: Indica si el pasajero sobrevivió o no. 1) Sobrevivió  2) Murió
- Pclass: Número de la clase en la que el pasajero viajó.
- Name: Nombre del pasajero
- Sex: Sexo del pasajero
- Age: Edad del pasajero
- SibSp: Indica si tenía familiares en el Titanic al momento del hundimiento
- Parch: Número de hijos del pasajero
- Ticket: Número de ticket
- Fare: Tarifa del boleto 
- Cabin: Cabina donde se alojó el pasajero
- Embarked: Lugar de embarque S:Southampton, C:Cambridge, Q:Queenstown


## 2.- Depurar el dataset.

Ya que vamos a utilizar una regresión logística para la predicción, necesitamos tener únicamente valores numéricos, por lo tanto debemos eliminar las columnas que no aportan o en su defecto cambiar los valores de las columnas por sus equivalentes en números.

PROCESO:

- Eliminar las columnas Cabin y Ticket

```r
# Eliminar una columna
dataframe$COLUMN <-NULL

## Ejm: Se tiene un dataframe llamado df y se desea eliminar la columna edad
df$edad<-NULL

``` 

- En los casos donde el pasajero tenga el valor de edad igual a nulo, colocar el valor de 29 (media de la edad)

```r
# Estadisticas de cada columna de un dataframe
summary(df)
# Media de una columna del dataframe
mean(df$columna)

#Colocar un valor fijo cuando los valores de una columna son nulos
df$columnx[is.na(df$columny)]<-value

# Ejm: Colocar el valor de 5 en la columna altura de un dataframe llamado datos, si y solo si su valor original sea NULO (NULL)
datos$altura[is.na(datos$altura)]<-5

```

- Eliminar todos los valores nulos que resten dentro del dataframe

```r
## Eliminar todos los valores nulos de un dataset
df_sin_nulos = na.omit(dataframe)

```

- Cambiar las variables Sexo y Cabina por variables binarias y numéricas respectivamente

```r
## Usar las variables Dummies (Convierte las opciones de una columna en valores booleanos)
Col_ = factor(dataframe$Columna)
dummies_Col = model.matrix(~Col_)

## Ejemplo: Se tiene una columna de Sexo
 ----------           -------------
|Sexo      |         |Masculino    |
|----------|         |-------------| 
|Masculino |   ==>   | 1           |
|Femenino  |         | 0           |
 ----------           -------------

Sex_ = factor(df$Sexo)
dummies_Sex = model.matrix(~Sex_)

```

- Agregar la dataset las columnas dummies para Sexo y Cabina sin tomar en cuenta "intercept"

```r
# Agregar una columna Y al dataframe df
df <- data.frame(df, df$Y)

```

- Reemplazar los valores de la columna Sex por los números generados en dummies
```r
## Reemplazar los valores de la columna X por los de la columna Y
df$X <- df$Y

```

- Crear un archivo CSV llamado titanic_clean.csv con los datos del dataframe limpio

```r
# Escribir un csv con datos de un dataframe (Se escribe en la carpeta configurada con setwd)
write.csv(df, "archivo.csv")

```

