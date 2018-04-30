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

5. El path en donde se encentra el  engine-recomendacion-maki es :

``` PredictionIO-0.12.0-incubating/engines/engine-recomendacion-maki/```

Por default es la ruta al momento de entrar en la consola del container. 

6. Crear app se debe de estar en el path del engine-recomendacion-maki, en la consola tipear :

```$ pio app new app_name``` 

Ejemplo :

```$ pio app new makirecommendation```



7. Ver nombre y key de la app se debe de estar en el path del engine-recomendacion-maki, en la consola tipear :

```$ pio app list```


8. Editar el engine  se debe de estar en el path del engine-recomendacion-maki: 

```$ nano  engine.json```  y remplazar  el  "INVALID_APP"  por   "el_nombre_app"

Ejemplo :

remplazar el  "INVALID_APP"  por   "makirecommendation"


9. Construir el engine se debe de estar en el path del engine-recomendacion-maki, tipear en la consola :

```$ pio build --verbose```



10. Agregar datos al event server

Hay varias maneras de ingestar datos al event server, pero la que se va a utilizar es utilizando NIFI; checar el docker  de DockerNifiMaki.




11. Una vez teniendo datos en el event server entrenar el engine, se debe de estar en el path del engine-recomendacion-maki, tipear :

```$ pio train```


12. Deploy el engine, se debe de estar en el path del engine-recomendacion-maki, tipear :
```$ pio deploy```


13. Checar el status del engine :
Verificar en el navegador :  localhost:8080  ó    ip_del_host:8080


14. Realizar consultas (HTTP Methods):

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

### Referencias:

 * Nos basamos en este Dockerfile [docker-predictionio](https://github.com/steveny2k/docker-predictionio)
* Para mas información checar : http://predictionio.apache.org/templates/recommendation/quickstart/  ,
http://predictionio.apache.org   , https://predictionio.apache.org/deploy/  ,  https://predictionio.apache.org/datacollection/eventapi/  ,  https://predictionio.apache.org/datacollection/eventmodel/

=========================
