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

- Cambiar las variables Sexo y Embarque por variables binarias y numéricas respectivamente

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

- Agregar la dataset las columnas dummies para Sexo y Embarque sin tomar en cuenta "intercept"

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

## 3.- Crear un modelo de regresión logística

- Crear un dataframe llamado "train" con el 80% (1047 registros) de datos para entrenamiento 

```r
# Crear un dataframe que sea un subconjunto de datos de otro Dataframe donde n es el número de registros
df1 <- df0[1:n,]

```
- Crear un dataframe llamado test con el resto de registros

```r
## Donde z es el número total de registros del df0
df1 <- df0[n+1:z,]

```

- Instalar y activar la libreria "caret"

```r
install.packages("caret")
library(caret)

```

- Crear la función sigmoide

```r
sigmoide = function(x){1/(1+exp(-x))}

```

- Generar un dataset de valores que vayan en un rango de -5 a 5 con saltos de 0.01

```r
x <- seq(-5,5,0.01)

``` 

- Dibujar una función sigmoide con los valores generados en la variable x en color rojo (red)

```r
plot(x, sigmoide(x), col="red")

```

- Crear un modelo de Regresión logística llamado RL_titanic donde se prediga la variable Survived (Predecir si una persona sobrevive o no en base a sus características).

La predicción debe estar en función de las variables:
Pclass + Sex + Age + SibSp + Parch + Fare + Embarked_C + Embarked_Q + Embarked_S

Debe utilizar el dataframe llamado train el cual contiene datos para el entrenamiento

Finalmente como la predicción es si sobrevive o muere, usamos la familia binomial



```r
RL_titanic  <- glm(Survived~ Pclass + Sex + Age + SibSp + Parch + Fare + Embarked_C + Embarked_Q + Embarked_S, data = train, family = binomial())

``` 

- Crear un dataframe de predicciones llamado "predictions" usando el dataframe con datos de prueba llamado "test"

```r
predictions <- predict(RL_titanic, test)

```

- Graficar los valores de predicción en color azul usando la función sigmoide

```r
plot(predictions, sigmoide(predictions), col="blue")

```

- Redondear los valores de predicción SI X>0.5 => 1 ELSE 0

```r
mod_pred <- ifelse(predictions > 0.5,1 , 0)

```

- Crear una matrix de confusión, esto para poder comparar los verdaderos positivos con falsos positivos y viceversa. Ya que nuestro resultado es 1 o 0, la matriz de confusión será de 2x2.

NOTA: Antes de poder crear la matriz de confusión es necesario

1.- Instalar y habilitar el paquete "e1071"

2.- Que los parámetros de la matriz de confusión sean tratados como factores

```r
install.packages("e1071")
library(e1071)
mod_pred <-factor(mod_pred)
test$Survived <- factor(test$Survived)
confusionMatrix(mod_pred, test$Survived)

```

MATRIZ DE CONFUSION

Los valores que se predicen 0 y en realidad son 0 = 166

Los valores que se predicen 0 y en realidad son 1 = 8

Los valores que se predicen 1 y en realidad son 0 = 1

Los valores que se predice 1 y en realidad son 1 = 87

La eficacia del modelo es de 96% => Accuracy : 0.9656  


```
Confusion Matrix and Statistics

          Reference
Prediction   0   1
         0 166   8
         1   1  87
                                          
               Accuracy : 0.9656          
                 95% CI : (0.9358, 0.9842)
    No Information Rate : 0.6374          
    P-Value [Acc > NIR] : <2e-16          
                                          
                  Kappa : 0.9245          
                                          
 Mcnemar's Test P-Value : 0.0455          
                                          
            Sensitivity : 0.9940          
            Specificity : 0.9158          
         Pos Pred Value : 0.9540          
         Neg Pred Value : 0.9886          
             Prevalence : 0.6374          
         Detection Rate : 0.6336          
   Detection Prevalence : 0.6641          
      Balanced Accuracy : 0.9549          
                                          
       'Positive' Class : 0               
                                       
``` 

## Predecir si sobrevivo o no

Intentaremos preguntar al modelo si Yo sobreviviría o no en el hundimiento del Titanic.

Para ello vamos a crear un dataframe con una fila colocando valores correspondientes a nosotros.

En mi caso creare vectores con mis datos:

- IdPasajero=1000
- Edad=35
- Familia en el Titanic=No
- Número de hijos=0
- Ticket = TICKET0
- Costo del ticket=250
- Cabina=A01
- Embarque=Queenstown
- Sexo=Masculino
- Clase=1

```r 
vPassengerId=c(1000)
vAge = c(35)
vSibSp = c(0)
vParch = c(0)
vTicket = c("TICKET0")
vFare = c(250)
vCabin = c("A01")
vEmbarked_C = c(0)
vEmbarked_Q = c(1)
vEmbarked_S = c(0)
vSex = c(1)
vPclass = c(1)
yotest <- data.frame(vPassengerId,vAge,vSibSp,vParch,vTicket,vFare,vCabin,vEmbarked_C,vEmbarked_Q,vEmbarked_S,vSex,vPclass)
library(plyr)
yo <- rename(yotest, c(
  "vPassengerId"="PassengerId",
  "vAge"="Age",
  "vSibSp"="SibSp",
  "vParch"="Parch",
  "vTicket"="Ticket",
  "vFare"="Fare",
  "vCabin"="Cabin",
  "vEmbarked_C"="Embarked_C",
  "vEmbarked_Q"="Embarked_Q",
  "vEmbarked_S"="Embarked_S",
  "vSex"="Sex",
  "vPclass"="Pclass"
  ))
vivo <- predict(RL_titanic,yo)
View(vivo)

``` 




