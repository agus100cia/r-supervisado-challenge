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

- Eliminar la columna Cabin

```r
dataframe$COLUMN <-NULL

## Ejm: Se tiene un dataframe llamado df y se desea eliminar la columna edad
df$edad<-NULL

``` 






