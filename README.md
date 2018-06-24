#ONFRAUD

**Trabajo Fin de Máster**
*Raul del Aguila*

##Ontologías

Como se puede ver en el repositorio, existen dos ficheros que contienen la ontología OnFraud:
+ La versión `onfraudv8_1rdf` contiene los conceptos sin enlazar con la DBPedia.
+ La versión `onfraudv10_1_rdf_enlazada` es la misma ontología con los conceptos enlazados a la DBPedia.

Si se pretende revisar las ontologías, se recomienda utilizar el editor [Protégé](https://protege.stanford.edu/).

##Documentación de la Ontología:

La documentación generada con [Ontoology](http://ontoology.linkeddata.es) se encuentra dentro de la carpeta "Ontoology". Existe también la posibilidad de generar la documentación con [Protégé](https://protege.stanford.edu/), pero en el desarrollo de OnFraud se ha preferido utilizar la primera plataforma.

##Datos enlazados del Ayuntamiento de Madrid.

Como parte de la elaboración del *dashboard*, se han utilizado los datos enlazados del Ayuntamiento de Madrid. La documentación de cómo fueron generados esos datos enlazados, así como el proyecto Open Refine, pueden encontrarse en la carpeta "Proyecto Open Refine". 

El archivo "README_DatosAnotados" contiene la información relativa a cómo fueron generados esos datos. El esqueleto de RDF generado se puede observar en las siguientes imágenes:

![Alineamiento 1](https://github.com/datalavidaloca/onfraud/blob/master/img/esqueleto-1.png?raw=true)

![Alineamiento 2](https://github.com/datalavidaloca/onfraud/blob/master/img/esqueleto-2.png?raw=true)

##Dashboard
Se ha generado un *dashboard* a modo de prueba de concepto para visualizar y explotar datos abiertos desde una perspectiva de prevención de fraude.

La carpeta "OnFraudDashboard" contiene la aplicación web de Shiny que se ejecutará a modo de dashboard. 

Para replicar la arquitectura definida en la memoria del TFM, es necesario realizar los siguientes pasos:

###Configuración del endpoint



***Inicio de la máquina docker*** 

`docker pull tenforce/virtuoso`

`docker run -p 127.0.0.1:8890:8890 -p 127.0.0.1:1111:1111 -e DBA_PASSWORD="ROOT" -e SPARQL_UPDATE=true -e DEFAULT_GRAPH="http://purl.org/puchase2pay/individuals/" -v //DESKTOP-NCTP077/db:/data -d tenforce/virtuoso`

***Comprobamos que esté corriendo la máquina y apuntamos el container id***
`docker ps`

***Carga de la tripleta*** 
`docker cp Contratos-Menores2.ttl objective_jones:/usr/local/virtuoso-opensource/var/lib/virtuoso/db/dumps`

***Ejecutamos el terminal dentro de nuestro contenedor***
`docker exec -it <container id> bash`

***Entramos en la consola de la BBDD***
`isql-v -U dba -P $DBA_PASSWORD`

***Cargamos el grafo en virtuoso***
`ld_dir('dumps', '*.ttl', 'http://purl.org/purchase2pay/individuals/');`

***Carga y ejecución***
`rdf_loader_run();`
 
###Actualización del fichero config.properties

Actualizamos este fichero con las rutas necesarias. 
IMPORTANTE: la ruta definida en la clave `rdf ` es utilizada por R. En caso de estar ejecutando la aplicación en `Windows` no es necesario especificar doble barra invertida.

###Ejecución

Se pueden cargar los ficheros `app.R`, `funciones.R` y `config.R` junto con el fichero `RDF.jar` y la carpeta `libs` en un servidor `shiny`. No obstante, para comprobar la prueba de concepto se considera más rápido la instalación de `RStudio` y ejecutar el archivo `app.R` con la máquina docker del endpoint de sparql corriendo. 
















