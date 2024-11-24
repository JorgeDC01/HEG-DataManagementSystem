# Introducción

Este proyecto integra un **Data Warehouse** desarrollado en **Apache Hive**, un **Data Lake** implementado con **ElasticSearch** y **Hadoop**, y una ontología que describe la infraestructura mediante **datos enlazados** en **GraphDB**. La ontología aprovecha el concepto de **Data Space** para facilitar consultas cruzadas entre diversos conjuntos de datos distribuidos en Internet.

El objetivo principal de este proyecto es realizar un **análisis de las reseñas de películas con el fin de identificar las palabras que puedan distinguir entre los grupos de películas, organizados en cuartiles según su puntuación media.**

El proyecto se desarrolla en 3 fases:

1. **Datos estructurados**. Implementación de un data warehouse con Apache Hive + PostgreSQL
2. **Datos no estructurados**. Implementación de un data lake con ElasticSearch + Hadoop
3. **Datos enlazados**. Diseño de una ontología descriptiva de la infraestructura empleada con GraphDB

> El notebook _./main/main.ipynb_ detalla el _pipeline_ creado para la consecución del proyecto.

## Infraestructura

<img src="https://github.com/user-attachments/assets/00a1c782-7edf-4fcc-8b7f-0fa30fbe97ae" width="800" height="400">


# Datos estructurados - Películas de dos fuentes de datos distintas

Este proyecto utiliza dos fuentes de datos estructurados de películas para crear un conjunto de datos enriquecido y consolidado. Los datos se obtienen de la siguiente manera:

1. Primer Dataset - Películas con Puntuación: La primera fuente de datos proviene de un repositorio en GitHub, que contiene información básica de películas, incluyendo sus puntuaciones. Este archivo se encuentra en formato ".csv" y se carga en una base de datos de PostgreSQL para facilitar su almacenamiento y consulta.

2. Segundo Dataset - Películas con Información Extendida de Rotten Tomatoes: La segunda fuente de datos es un archivo en formato ".csv" que proviene de Rotten Tomatoes y contiene información adicional sobre cada película, incluyendo campos como directores, actores, guionistas, restricciones de edad (rating_content), puntuación en rotten tomatoes, entre otros. Este archivo se mantiene en su formato original de almacenamiento ".csv".

La combinación de ambos datasets permite enriquecer la información de las películas al realizar un JOIN entre el dataset básico de GitHub (con puntuación) y el dataset extendido de Rotten Tomatoes. Este proceso facilita que cada película en el conjunto final tenga no solo su puntuación, sino también detalles adicionales como sus directores, autores, actores, restricción de edad, etc.

Para realizar el JOIN, se emplea Apache Hive como intermediario entre el archivo .csv y la base de datos PostgreSQL. Apache Hive es una infraestructura distribuida sobre Hadoop que actúa como un data warehouse, permitiendo gestionar y analizar grandes volúmenes de datos de forma eficiente. Su lenguaje de consulta similar a SQL facilita el acceso, la combinación y la transformación de datos provenientes de distintas fuentes, consolidando así la información de ambos datasets en una única fuente unificada.

Esta integración de datos a través de Hive permite analizar y explorar información detallada sobre las películas, abstrayendose de las distintas formas de almacenamiento internas.

## Fuente de los Datos
- **Origen: Repositorio en Github y Kaggle** 
- **URLs de origen:**
  - [Movies con puntuaciones](https://github.com/melodiromero/movies/blob/main/dataset/movies_limpio.csv)
  - [Movies de Rotten Tomatoes](https://www.kaggle.com/datasets/stefanoleone992/rotten-tomatoes-movies-and-critic-reviews-dataset)
  
## Fecha de Recogida
- **Fecha:** 27-10-2024 / 6-11-2024

## Formato de los Datos
- **Formatos:**
  - Movies con puntuaciones: CSV. Almacenamiento en PostgreSQL
  - Movies-Rotten: CSV. Almacenamiento en formato ".csv"
  
## Licencias de Uso
- **Películas**
  - **Licencia:** [Licencia de Datos](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)
  - **Enlace a la licencia:** [URL de la licencia](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)

- **Puntuaciones**
  - **Licencia:** CC0 1.0 Universal
  - **Enlace a la licencia:** [URL de la licencia](https://creativecommons.org/publicdomain/zero/1.0/)

## Descripción de las variables del conjunto de datos de Movies con puntuaciones

| Nombre de la Variable       | Tipo de Dato | Descripción                                                                                         |
|-----------------------------|--------------|-----------------------------------------------------------------------------------------------------|
| id                          | int64        | ID de la película en la base de datos.                                                             |
| budget                      | int64        | El presupuesto de la película, en dólares.                                                         |
| genres                      | object       | Un diccionario que indica todos los géneros asociados a la película.                               |
| original_language           | object       | Idioma original en el que se grabó la película.                                                    |
| overview                    | object       | Pequeño resumen de la película.                                                                     |
| popularity                  | float64      | Puntaje de popularidad de la película, asignado por TMDB (TheMoviesDataBase).                     |
| release_date                | object       | Fecha de estreno de la película.                                                                    |
| revenue                     | int64        | Recaudación de la película, en dólares.                                                             |
| runtime                     | int64        | Duración de la película, en minutos.                                                                |
| spoken_languages            | object       | Lista con los idiomas que se hablan en la película.                                               |
| status                      | object       | Estado actual de la película (si fue anunciada, si ya se estrenó, etc).                           |
| title                       | object       | Título de la película.                                                                               |
| vote_average                | float64      | Puntaje promedio de reseñas de la película.                                                        |
| vote_count                  | int64        | Número de votos recibidos por la película en TMDB.                                                |
| franquicia                  | object       | Información sobre la franquicia a la que pertenece la película.                                   |
| productoras                 | object       | Lista de productoras asociadas a la película.                                                     |
| release_year                | int64        | Año de estreno de la película.                             |
| retorno                     | float64      | Retorno de la película, generalmente representado como un valor monetario. |

Los datos han sido almacenados en PostgreSQL, un sistema gestor de bases de datos relacional orientado a objetos. Los principales beneficios de PostgreSQL son los siguientes:

1. Simultaneidad: los SGBD tradicionales implementan bloqueos ante el acceso a los registros para evitar conflictos de lectura/escritura. Estos bloqueos aseguran la integridad de los datos, pero pueden generar tiempos de espera o ralentizar el rendimiento en entornos de alta concurrencia. PostgreSQL gestiona la concurrencia mediante un sistema de control de concurrencia mediante múltiples versiones (MVCC). MVCC crea varias versiones de los datos, permitiendo que cada usuario acceda a una versión específica de los datos, según el momento en el que inició la transacción.

2. Flexibilidad en los lenguajes de programación. PostgreSQL destaca por su compatibilidad con varios lenguajes de programación, como Python, JavaScript, o C++.
  
3. _Open Source_: su código fuente es público, accesible y modificable por cualquier persona. Este modelo ha impulsado la creación de una base de datos sólida y flexible que evoluciona en base a las necesidades de sus usuarios.

El tipo de dato de la columna "genres" es un diccionario. Una película puede relacionarse con varios géneros y un género puede contener multitud de películas. Por tanto, se crean 3 tablas que permiten modelar la relación entre películas y géneros. Una primera tabla llamada "movies" almacenan las películas del _dataset_. Se crea una segunda tabla "genres" que contiene los identificadores y nombres para cada género. La tabla intermedia "movie_genres" mapea los géneros de cada película relacionando los identificadores de las películas con los identificadores de los géneros mediante dos claves externas.

<img src="https://github.com/user-attachments/assets/30cfa78f-72e7-4705-b01f-d80a7e8cfa35" width="500" height="400">

## Descripción de las variables del conjunto de datos de Movies-Rotten

| Nombre de la Variable              | Tipo de Dato | Descripción                                                                                    |
|------------------------------------|--------------|------------------------------------------------------------------------------------------------|
| rotten_tomatoes_link               | object       | Enlace único a la película en Rotten Tomatoes.                                                 |
| movie_title                        | object       | Título de la película.                                                                         |
| movie_info                         | object       | Información adicional sobre la película.                                                       |
| critics_consensus                  | object       | Opinión general de los críticos sobre la película.                                             |
| content_rating                     | object       | Clasificación de contenido de la película (ej., PG, R).                                        |
| genres                             | object       | Géneros a los que pertenece la película.                                                       |
| directors                          | object       | Director(es) de la película.                                                                   |
| authors                            | object       | Autor(es) o guionista(s) de la película.                                                       |
| actors                             | object       | Actor(es) principales de la película.                                                          |
| original_release_date              | object       | Fecha de estreno original de la película.                                                      |
| streaming_release_date             | object       | Fecha de estreno en plataformas de streaming.                                                  |
| runtime                            | float64      | Duración de la película en minutos.                                                            |
| production_company                 | object       | Compañía de producción de la película.                                                         |
| tomatometer_status                 | object       | Estado en el Tomatometer (ej., Fresh, Rotten).                                                 |
| tomatometer_rating                 | float64      | Calificación en el Tomatometer (0-100).                                                        |
| tomatometer_count                  | int64        | Número de críticas evaluadas en el Tomatometer.                                                |
| audience_status                    | object       | Estado de la audiencia (ej., Upright, Spilled).                                                |
| audience_rating                    | float64      | Calificación de la audiencia (0-100).                                                          |
| audience_count                     | int64        | Número de reseñas de la audiencia evaluadas.                                                   |
| tomatometer_top_critics_count      | int64        | Número de críticas realizadas por los principales críticos en el Tomatometer.                  |
| tomatometer_fresh_critics_count    | int64        | Número de críticas positivas en el Tomatometer.                                                |
| tomatometer_rotten_critics_count   | int64        | Número de críticas negativas en el Tomatometer.                                                |

Los datos han sido almacenados en su formato origen ".csv". 

## :star: Data Warehouse. Apache Hive

La infraestructura de Hive ha sido configurada usando contenedores en Docker para gestionar un metastore distribuido con PostgreSQL como base de datos de respaldo.

1. PostgreSQL (hive4-postgres): Se emplea como la base de datos principal del metastore de Hive, donde se almacenan los metadatos de las tablas y bases de datos creadas en Hive. Este servicio tiene la siguiente configuración:

- Se establece el nombre de la base de datos metastore_db, con el usuario hive y la contraseña password.
- Expone el puerto 5432 para permitir conexiones al metastore de Hive.
- **Almacenamiento persistente** mediante el volumen hive-db, asegurando que los datos persisten entre reinicios.

2. Metastore de Hive (metastore): Este servicio se conecta a la base de datos PostgreSQL para almacenar y recuperar metadatos, configurado para integrarse con PostgreSQL como sistema de gestión de metadatos. Su configuración incluye:

- Variables de entorno para especificar el controlador de la base de datos (postgres) y los detalles de conexión (URL, usuario y contraseña).
- Uso de los controladores JDBC de PostgreSQL, montados en /opt/hive/lib/, para permitir la conectividad.
- Exposición del puerto 9083, utilizado para la comunicación entre servicios.

3. HiveServer2 (hiveserver2): Proporciona la interfaz para ejecutar consultas en Hive utilizando el protocolo Thrift. Este servicio se conecta al metastore para consultar y gestionar los datos, configurado con:

- El puerto 10000 para consultas Thrift y 10002 para JDBC/ODBC.
- Variables de entorno que apuntan al servicio de metastore, permitiendo la conexión para la ejecución de consultas en Hive.

4. Hue (hue): Interfaz web que permite a los usuarios interactuar de manera sencilla con Hive y su metastore. Hue está configurado para conectarse a - HiveServer2 en el puerto 10000 para ejecutar consultas SQL desde su entorno web.

Red de Docker (gestbd_net): Todos los contenedores están conectados en la red gestbd_net, lo cual permite una comunicación interna y segura entre los servicios.

Para implementar el **data warehouse**, se crean dos tablas en Hive mediante consultas SQL y estárán enlazadas con la tabla de películas de PostgreSQL y el archivo ".csv". Las consultas SQL para la creación de las tablas "movies_linked" y "rotten_movies" son las siguientes:

```sql
CREATE EXTERNAL TABLE movies_linked (
    movie_id INT,
    budget FLOAT,
    original_language STRING,
    overview STRING,
    popularity FLOAT,
    release_date DATE,
    revenue FLOAT,
    runtime FLOAT,
    title STRING,
    vote_average FLOAT,
    vote_count INT,
    release_year INT,
    retorno FLOAT
)
STORED BY 'org.apache.hive.storage.jdbc.JdbcStorageHandler'
TBLPROPERTIES (
  "hive.sql.database.type" = "POSTGRES",
  "hive.sql.jdbc.driver" = "org.postgresql.Driver",
  "hive.sql.jdbc.url" = "jdbc:postgresql://hive4-postgres:5432/postgres",
  "hive.sql.dbcp.username" = "hive",
  "hive.sql.dbcp.password" = "password",
  "hive.sql.table" = "movies"
);

CREATE EXTERNAL TABLE rotten_movies (
  rotten_tomatoes_link STRING,
  movie_title STRING,
  movie_info STRING, 
  critics_consensus INT,
  content_rating STRING,
  genres STRING,
  directors INT,
  authors INT,
  actors INT,
  original_release_date STRING,
  streaming_release_date STRING,
  runtime INT,
  production_company INT,
  tomatometer_status INT,
  tomatometer_rating INT,
  tomatometer_count INT,
  audience_status INT,
  audience_rating INT,
  audience_count INT,
  tomatometer_top_critics_count INT,
  tomatometer_fresh_critics_count INT,
  tomatometer_rotten_critics_count INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/opt/hive/data/warehouse/rotten_movies';

LOAD DATA LOCAL INPATH '/opt/hive/data/warehouse/rotten_tomatoes_movies.csv' INTO TABLE rotten_movies;
```


---

# Datos no estructurados - Reviews

El objetivo del proyecto es recuperar reseñas de películas almacenadas en el Data Warehouse.

Como parte de una práctica inicial, en el directorio _./data/unstructured_ se descarga un dataset de Kaggle que contiene enlaces a reseñas de películas de IMDb. Sobre estos enlaces, se aplica un proceso de web scraping para extraer un total de 48,000 reseñas de películas. Estas reseñas son almacenadas en una base de datos NoSQL y documental: **ElasticSearch**.

Aunque ElasticSearch no es una base de datos tradicional, sino un motor de búsqueda distribuido basado en texto, se utiliza en este proyecto debido a su capacidad para manejar grandes volúmenes de datos no estructurados y realizar búsquedas rápidas y eficientes sobre contenido textual. Su uso permite optimizar la consulta y recuperación de reseñas, lo que facilita el análisis de las mismas en fases posteriores del proyecto.


## Fuente de los Datos
- **Origen: Kaggle. Movie-reviews** 
- **URLs de origen:**
  - [Reviews](https://www.kaggle.com/datasets/jaidalmotra/movies-review)
  
## Fecha de Recogida
- **Fecha:** 28-10-2024

## Formato de los Datos
- **Formato:**
  - WebScraping 
  
## Licencia de Uso
- **Licencia:** MIT
- **Enlace a la licencia:** [URL de la licencia](https://www.mit.edu/~amini/LICENSE.md)

## Descripción de las variables del conjunto de datos de Movies (movies.csv)

| Nombre de la Variable       | Tipo de Dato | Descripción                                                                                         |
|-----------------------------|--------------|-----------------------------------------------------------------------------------------------------|
| id                          | str          | Identificador de la película.                                                                       |
| name                        | str          | Título de la película.                                                                              |
| genres                      | str          | Géneros asociados a la película.                                                                    |
| review_url                  | str          | Enlace (URL) a reseña de la película.                                                               |

## Descripción de las variables del conjunto de datos de Reviews (reviews.csv)

| Nombre de la Variable       | Tipo de Dato | Descripción                                                                                         |
|-----------------------------|--------------|-----------------------------------------------------------------------------------------------------|
| id                          | str          | Identificador de la película.                                                                       |
| name                        | str          | Título de la película.                                                                              |
| review                      | str          | Reseña de la película.                                                                              |

## Proceso de recolección de las reviews
El dataset de Kaggle contiene 17 archivos con formato ".csv", cada uno de ellos contiene un conjunto de películas de un género en concreto.

![image](https://github.com/user-attachments/assets/9c97fca9-e10c-4f9c-9dd9-8b12d6de2fa3)

Cada ".csv" almacena un conjunto de películas con diferentes atributos (name, year, movie_rating ... review_url). Este último atributo es un enlace a una página web en IMDB que contiene 25 reviews de la película correspondiente. La siguiente imagen corresponde a una página web de reviews:


![image](https://github.com/user-attachments/assets/a5631176-9997-4a74-b969-c7313b3ccea9)

Nuestro dataset no estructurado se obtiene a partir de la técnica de ***webscraping*** sobre estas páginas web. Se extraen 28693 reseñas de películas en total. Estas reseñas se insertan en ElasticSearch. 

ElasticSearch es un motor de búsqueda y análisis que permite almacenar, buscar y analizar grandes volúmenes de datos. Además, puede utilizarse como una base de datos NoSQL orientada a documentos, aunque no es un SGBD completo como lo pueden ser PostgreSQL o MySQL porque no ofrece el mismo nivel de consistencia de los datos.


Un ejemplo de documento extraido del dataset de reviews es el siguiente:

```
{
  '_index': 'reviews',
  '_id': 'tt0114369',
  '_source': {
    'name': 'Se7en',
    'review': It is a rarity for a film to be completely unsettling and yet unrelentingly gripping.
    David Fincher's story takes place in a bleak and constantly raining city, never named, 
    where urban decay and sleaze in all forms are rampant. Coming up to his retirement from 
    the police force is Detective Lieutenant Somerset (Morgan Freeman), who is tasked with 
    breaking in his replacement, Detective Sergeant Mills (Brad Pitt), before leaving. 
    Somerset is world-weary...
  }
}
```

El propósito de almacenar las reseñas de películas junto con el nombre de la película es relacionar el conjunto de datos estructurado en PostgreSQL con el conjunto no estructurado de reseñas, de modo que ambas bases de datos se ejecuten en dos contenedores diferentes dentro de una misma red en Docker. De esta forma, se pueden realizar consultas de texto sobre un conjunto de películas o sobre una en específico.


## :star: Data lake. ElasticSearch - Hadoop

En el notebook ./main/main.ipynb, la extracción de las reseñas se lleva a cabo de manera diferente en comparación con el proceso seguido en el directorio ./unstructured. Desde el Data Warehouse, se realiza una consulta para obtener los títulos de las películas, en total, 14,000. Con estos títulos, se realiza una consulta al endpoint de la ontología de Wikidata para obtener datos relacionados con cada película. En concreto, se busca obtener el identificador de cada película en IMDb para generar el enlace que nos permitirá acceder a las reseñas de esa película. Para ello, se ejecuta una consulta SPARQL sobre el endpoint: https://query.wikidata.org/sparql.

La consulta SPARQL ejecutada tiene la siguiente forma:

```
SELECT ?item ?itemLabel ?imdb_id
WHERE {{
  VALUES ?title {{ {titles_str} }}
  ?item rdfs:label ?title.
  ?item wdt:P31 wd:Q11424.  # La entidad debe ser una película
  ?item wdt:P345 ?imdb_id.  # ID de IMDb
  SERVICE wikibase:label {{ bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }}
}}
```
En total, se obtienen los identificadores de IMDb de 11,906 películas. Con estos identificadores, se forman las URLs: https://www.imdb.com/title/{imdb_id}/reviews/_ajax y se realiza web scraping para recuperar un total de 217,000 reseñas, las cuales se almacenarán en una infraestructura de ElasticSearch-Hadoop, formando un Data Lake.

La principal ventaja de usar Spark con ElasticSearch es su capacidad para procesar grandes volúmenes de datos de forma distribuida y escalable. Aunque no se ha configurado un clúster de Spark con ElasticSearch en el entorno actual (que se ejecuta en una sola máquina), en un sistema distribuido esta integración sería clave para acelerar los procesos de preprocesamiento y consultas.

A continuación, se inicia el proceso de inserción de las reseñas en ElasticSearch. Un documento de reseña contiene los siguientes campos:

- imdb_id: identificador de la película en IMDb.
- movie_title: título de la película.
- quartile_vc: cuartil de la puntuación media al que pertenece la reseña.
- quartile_rotten_rating: cuartil de la puntuación de Rotten Tomatoes al que pertenece la reseña.
- review: contenido de la reseña.

Un ejemplo de reseña almacenada en el índice reviews_extracted de ElasticSearch es el siguiente:

```
{'_index': 'reviews_extracted',
 '_id': 1,
 '_source': {'imdb_id': 'tt0112715',
  'movie_title': 'Congo',
  'quartile_vc': 'Q1',
  'quartile_rotten_rating': 'Q1',
  'review': 'ok movie best watched rainy day youre pretty bored fake looking gorillas obviously wellmade
 whereas 1993 film jurassic park familiarized audiences cg dinosaurs fact cgi originally planned grays
 technology yet developed point realistic hair could created smooth skinned dinosaurs possible hairy apes
 would looked inappropriately cartooned therefore animatronics masks puppetry utilized kind damper special
 effects well tim currys bad acting movie could lot better viewable got gorilla puppets tim currys accent pretty
 decent movie thought story good jurassic park michael crichton congo wasnt realistic neither jurassic park like
 show legends hidden temple intelligent gorillas science action plot could sit watch ill give barely 6 stars'}}
```
Cada reseña está asociada a un cuartil. Como se describió al comienzo, la idea es extraer los tópicos más relevantes de cada cuartil, utilizando dos distribuciones de películas clasificadas según dos tipos de puntuaciones. Esto nos permitirá identificar coincidencias entre los tópicos de los cuartiles o determinar si algunos son discriminantes específicos para cada rango de puntuación.


# Datos Enlazados - Ontología descriptiva de la infraestructura - GraphDB
Los datos enlazados tienen una amplia variedad de aplicaciones. Se puede diseñar una ontología que describa nuestros datos y establezca vínculos entre las clases de la ontología y otras ontologías del Data Space mediante equivalencias, con el objetivo de conectar infraestructuras de datos distintas y permitir consultas cruzadas.

Por ejemplo, en el dominio de las películas, podríamos realizar una consulta SPARQL a una ontología que contenga información sobre distribuidoras, productoras, actores o localizaciones de ciudades, lo que permitiría relacionar películas con sus localizaciones geográficas. Esto no solo facilita el acceso y la exploración de los datos, sino que también ofrece una capa de interoperabilidad con otras fuentes de datos externas.

En este proyecto, se ha diseñado una ontología que describe nuestra infraestructura de datos, permitiendo el acceso libre a cualquier usuario interesado en hacer uso de los datos almacenados en el Data Warehouse o en el Data Lake. Además, se proporcionan los endpoints correspondientes para facilitar la interacción con nuestra ontología a través de consultas SPARQL. De esta manera, los usuarios pueden explorar nuestra infraestructura y acceder a los datos de manera flexible.

Es importante destacar que este proyecto ha sido ejecutado en un entorno local y no ha sido desplegado en una red pública. Por lo tanto, aunque la ontología y los datos están disponibles para su consulta en el entorno local, no están accesibles en la web pública.


## Fuente de los Datos
- **Origen:** Estructurado por los miembros del grupo con la herramienta [WebProtégé](https://webprotege.stanford.edu/). Son las instancias que definen la infraestructura usada.
  
## Fecha de Recogida
- **Fecha:** 03/11/2024

## Formato de los Datos
- **Formato:** Formato 'turtle' (.ttl)
  
## Licencia de Uso
- **Licencia:** Sin licencia.

## Descripción del conjunto de datos
### Classes
- `ont:DataCatalog`: Representa un catálogo de datos, describiendo los metadatos relacionados con diversas entidades de datos. Puede incluir información sobre la organización y clasificación de los datos dentro de un repositorio o sistema, permitiendo a los usuarios comprender qué datos están disponibles y cómo acceder a ellos.

- `ont:DataSource`: Describe el origen de los datos, especificando la fuente desde la cual los datos son obtenidos. Esta entidad puede estar relacionada con bases de datos, archivos, APIs, o cualquier otra fuente de datos estructurados o no estructurados.

- `ont:Datawarehouse`: Hace referencia a un sistema de almacenamiento de datos estructurados, como un almacén de datos o Data Warehouse. En este contexto, describe los metadatos asociados con la infraestructura de almacenamiento, donde los datos consolidados de diversas fuentes se almacenan para análisis y generación de informes.

- `ont:Dataset`: Representa un conjunto de datos o dataset. Es una colección organizada de datos, que puede incluir múltiples tipos de información estructurada o no estructurada. Los datasets son la unidad básica para el análisis y procesamiento de datos.

- `ont:Database`: Representa una fuente de datos estructurados. Puede incluir tanto bases de datos relacionales como no relacionales, y sistemas de almacenamiento como ElasticSearch. Describe la infraestructura de almacenamiento que organiza y mantiene los datos de forma que sea accesible para su consulta y procesamiento.

- `ont:RelationalDB`: Describe una base de datos relacional. Esta entidad define los metadatos asociados a sistemas de bases de datos que siguen un modelo de tablas y relaciones entre ellas, como PostgreSQL, MySQL, o SQL Server. Las bases de datos relacionales permiten realizar consultas SQL para gestionar los datos.

- `ont:NoRelationalDB`: Describe una base de datos no relacional (NoSQL). Este tipo de base de datos se utiliza para almacenar datos que no siguen el modelo de tablas relacionales, como bases de datos de documentos (ej., MongoDB), clave-valor (ej., Redis), o de gráficos (ej., Neo4j). Se usa comúnmente en sistemas que requieren escalabilidad y flexibilidad en el manejo de datos no estructurados o semi-estructurados.

- `ont:File`: Representa un archivo que contiene datos. Esta entidad describe los metadatos de un archivo, que puede ser de diferentes tipos (como CSV, JSON, XML, etc.), y puede estar almacenado localmente o en un sistema de almacenamiento distribuido.

- `ont:Table`: Representa una tabla en una base de datos relacional. Cada tabla contiene datos organizados en filas y columnas, donde cada fila es un registro y cada columna representa un atributo de ese registro. Las tablas son los elementos fundamentales de las bases de datos relacionales y se utilizan para organizar la información de manera estructurada.

La imagen siguiente refleja las instancias de la ontología que describe el diseño de la infraestructura.

![image](https://github.com/user-attachments/assets/045e5a99-8bb5-4c95-b6cc-830920544b17)



