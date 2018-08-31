# DockerMakiPrediction

docker-prediction-maki
======================

Ejecuta MAKI PredictionIO en Docker

1. Clona este repositorio

2. Construye la imagen del dockerfile 
    
    ```Bash
    $ docker build -t bigdatamx/predictionmaki .
    ```
    
3. Ejecuta el contenedor

    ```Bash
    $ docker run -p 9000:9000 -p 8000:8000 -p 7070:7070 --name predictionmaki -it bigdatamx/predictionmaki /bin/bash
    ```

4. Dentro del contenedor ejecuta el siguiente comando para levantar los servicios

    ```Bash
    $ pio-start-all
    ```
   
   Verifica que los servicios esten arriba

    ```Bash
    $ jps -l
    ```

5.  Probar en el navegador si el servicio de Event Server se encuentra levantado, poner la siguiente URL: 
http://localhost:7070/  ó  http://{tu_ip}:7070/, y debe de mostrar un mensaje como : {"status":"alive"}; de otro modo 
si no esta levantado colocar en la consola del prediction el siguiente comando:
 pio eventserver


6. El path en donde se encentra el  engine-recomendacion-maki es :

``` PredictionIO-0.12.0-incubating/engines/engine-recomendacion-maki/```

Por default es la ruta al momento de entrar en la consola del container. 

7. Crear app se debe de estar en el path del engine-recomendacion-maki, en la consola tipear :

```$ pio app new app_name``` 

Ejemplo :

```$ pio app new makirecommendation```



8. Ver nombre y key de la app se debe de estar en el path del engine-recomendacion-maki, en la consola tipear :

```$ pio app list```


9. Editar el engine  se debe de estar en el path del engine-recomendacion-maki: 

```$ nano  engine.json```  y remplazar  el  "INVALID_APP"  por   "el_nombre_app"

Ejemplo :

remplazar el  "INVALID_APP"  por   "makirecommendation"


10. Construir el engine se debe de estar en el path del engine-recomendacion-maki, tipear en la consola :

```$ pio build --verbose```



11. Agregar datos al event server

Hay varias maneras de ingestar datos al event server, pero la que se va a utilizar es utilizando NIFI; checar el docker  de DockerNifiMaki.




12. Una vez teniendo datos en el event server entrenar el engine, se debe de estar en el path del engine-recomendacion-maki, tipear :

```$ pio train```


13. Deploy el engine, se debe de estar en el path del engine-recomendacion-maki, tipear :
```$ pio deploy```


14. Checar el status del engine :
Verificar en el navegador :  localhost:8080  ó    ip_del_host:8080


15. Realizar consultas (HTTP Methods):

Se recomienda utilizar Postman para realizar las peticiones HTTP:

https://www.getpostman.com/apps

* realizar consultas al event server:

Peticion get ,ejemplo obtener todos los datos alamcenados en el event server de una app  :

Por default son 20 los que muestra:
http://localhost:7070/events.json?accessKey=$APP_KEY


tilizando limit te muestra mas.
http://localhost:7070/events.json?accessKey=NN9gQssNi0v33LG6I5Vfd7h2qICq0HDn27jSB-wWcOnFnrVjUqunNevkbbrMl4oX&limit=500



* realizar peticiones y obtener recomendaciones es de la siguente manera al engine:

Peticion post, ejemplo obtener una  recomendacion de un user :

curl -H "Content-Type: application/json" \
-d '{ "user": "16727", "num": 4 }' http://localhost:8000/queries.json




 La consola esta disponible en el puertos 9000



16. Comandos :


La interacción con Apache PredictionIO se realiza a través de la interfaz de línea de comando. Sigue el formato de:

pio <command> [options] <args>...

Puede ejecutar pio help para ver una lista de todos los comandos disponibles y pio help <command>para ver detalles del comando.

Los comandos de Apache PredictionIO se pueden separar en las siguientes tres categorías.


Comandos generales:
pio help  Muestra resumen de uso. pio help <command>para leer acerca de un subcomando específico.

pio version Muestra la versión del PredictionIO instalado.

pio status Muestra la ruta de instalación y el estado de ejecución del sistema PredictionIO y sus dependencias


Comandos del servidor de eventos:
pio eventserver   Inicie el servidor de eventos.

pio app   Administrar las aplicaciones que usa el servidor de eventos.

pio app data-delete <name>   borra todos los datos asociados con la aplicación.

pio app delete <name>    borra la aplicación y sus datos.

--ip <value>  IP para enlazar a. Predeterminado para localhost.

--port <value>  Puerto al que enlazar Predeterminado a 7070

pio accesskey    Administra las claves de acceso a la aplicación.


Comandos del motor :
Los comandos del motor deben ejecutarse desde el directorio que contiene el proyecto del motor. --debugy las --verbosebanderas proporcionarán mensajes informativos de depuración y de terceros.

pio build  Construye el motor en el directorio actual.

pio train  Inicia un entrenamiento usando un motor.

pio deploy  Implementar un motor como servidor del motor.

pio batchpredict  Procesa predicciones en bloque usando un motor.

Para deploy& batchpredict, si --engine-instance-idno se especifica, usará la última instancia capacitada.





### Referencias:

 * Nos basamos en este Dockerfile [docker-predictionio](https://github.com/steveny2k/docker-predictionio)
* Para mas información checar : http://predictionio.apache.org/templates/recommendation/quickstart/  ,
http://predictionio.apache.org   , https://predictionio.apache.org/deploy/  ,  https://predictionio.apache.org/datacollection/eventapi/  ,  https://predictionio.apache.org/datacollection/eventmodel/
https://predictionio.apache.org/cli/

=========================
