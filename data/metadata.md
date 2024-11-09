*En la primera release del repositorio, se recuperaron dos datasets de crímenes y arrestos en Los Angeles. Esta primera idea se ha sustituido temporalmente por la recolección de películas y reviews, facilitando la creación de una relación entre los datos estructurados (películas) y los no estructurados (reviews).*

# Datos estructurados - Películas

**Explicación**: Se escogen dos fuentes de datos estructurados de películas. La primera fuente de datos se obtiene de un repositorio de Github con formato ".csv". Este conjunto de películas se almacenará en una base de datos de PostgreSQL. La segunda fuente de películas se obtiene a partir del dataset de puntuaciones de películas en formato ".csv" de *rotten tomatoes*. Este nuevo conjunto de películas se mantendrá en el formato de almacenamiento ".csv".

La idea principal de utilizar ambos conjuntos de películas es realizar un JOIN entre el dataset de películas disponible en GitHub y el dataset de puntuaciones de películas de Rotten Tomatoes. Esto permitirá que cada película tenga asociada su puntuación en Rotten Tomatoes. Para realizar este JOIN, se emplea Apache Hive como intermediario entre el archivo .csv y la base de datos en PostgreSQL.

Apache Hive es una infraestructura de almacenamiento de datos distribuida, construida sobre Hadoop, que funciona como un **data warehouse** y permite gestionar y analizar grandes volúmenes de datos procedentes de diversas fuentes. Hive proporciona un lenguaje de consulta similar a SQL, lo cual facilita el acceso, la combinación y la transformación de distintos conjuntos de datos de forma eficiente. Esto posibilita una integración efectiva de los datos de ambos datasets, consolidando la información de películas y sus puntuaciones en una sola fuente.


## Fuente de los Datos
- **Origen: Repositorio en Github y Kaggle** 
- **URLs de origen:**
  - [Movies](https://github.com/melodiromero/movies/blob/main/dataset/movies_limpio.csv)
  - [Puntuaciones](https://www.kaggle.com/datasets/stefanoleone992/rotten-tomatoes-movies-and-critic-reviews-dataset)
  
## Fecha de Recogida
- **Fecha:** 27-10-2024 / 6-11-2024

## Formato de los Datos
- **Formatos:**
  - Películas: CSV. Almacenamiento en PostgreSQL
  - Puntuaciones: CSV. Almacenamiento en formato ".csv"
  
## Licencias de Uso
- **Películas**
  - **Licencia:** [Licencia de Datos](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)
  - **Enlace a la licencia:** [URL de la licencia](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)

- **Puntuaciones**
  - **Licencia:** CC0 1.0 Universal
  - **Enlace a la licencia:** [URL de la licencia](https://creativecommons.org/publicdomain/zero/1.0/)

## Descripción de las variables del conjunto de datos de Películas

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


## Descripción de las variables del conjunto de datos de Puntuaciones de Películas

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

## Data Warehouse. Apache Hive

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

Para implementar el **data warehouse**, se crean dos tablas en Hive mediante consultas SQL y estárán enlazadas con la tabla de películas de PostgreSQL y el archivo ".csv". Las consultas SQL para la creación de las tablas "movies_linked" y "scoring_movies" son las siguientes:

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

CREATE EXTERNAL TABLE scoring_movies (
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
LOCATION '/opt/hive/data/warehouse/scoring_movies';

LOAD DATA LOCAL INPATH '/opt/hive/data/warehouse/rotten_tomatoes_movies.csv' INTO TABLE scoring_movies;
```


---

# Datos no estructurados - Reviews
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


# Datos Enlazados - Metadatos
## Fuente de los Datos
- **Origen:** Estructurado por los miembros del grupo con la herramienta [WebProtégé](https://webprotege.stanford.edu/).
  
## Fecha de Recogida
- **Fecha:** 03/11/2024

## Formato de los Datos
- **Formato:** Formato 'turtle' (.ttl)
  
## Licencia de Uso
- **Licencia:** Sin licencia.

## Descripción del conjunto de datos
### Classes
- `void:Dataset`: Representa los conjuntos de datos 'datasets'.
- `ont:Database`: Representa la fuente de los datos. Puede incluir bases de datos relacionales (como PostgreSQL) o sistemas de búsqueda (como Elasticsearch).
- `ont:Field`: Representa columnas o campos específicos en cada tabla o índice.
- `ont:Index`: Clase índice, representa los índices que se crean en las bases de datos.
- `ont:Table`: Representa cada tabla en una base de datos relacional.
- `ont:Genre`: Representa el género de una película, como un string.
- `ont:Review`: Representa la entidad Review del dataset no estructurado.
- `ont:ProductionCompany`: Representa la productora de la película.
- `ont:Franchise`: Representa la franquicia a la que pertenece una película.
- `ont:Movie`: Representa la entidad Película del dataset estructurado.
- `dcat:DataCatalog`: Descripción de los metadatos según la entidad que describa.

### Data properties
- `ont:count`: Cuantifica el número de tablas, índices creados en un dataset o el número de registros.
- `ont:creationDate`: Fecha de creación de los metadatos.
- `ont:databaseEngine`: Describe el tipo de base de datos: relacional, noSQL (documental, clave-valor, etc).
- `ont:databaseSize`: Describe el tamaño en GB de la base de datos o de un dataset en concreto.
- `ont:datatype`: Describe el tipo de dato que almacena un campo de una tabla o índice.
- `ont:description`: Descripción del dataset, base de datos, campo, índice o tabla.
- `ont:endpoint_db`: Punto de acceso de la base de datos.
- `ont:isIndexed`: Describe si un campo está indexado. En tal caso, se pueden realizar consultas más rápidas.
- `ont:isRequired`: Describe si un campo puede tomar valores nulos.
- `ont:last_Update`: Última modificación del dataset, índice o tabla.
- `ont:name`: Nombre del dataset, base de datos, campo, índice, tabla o catálogo de datos.
- `ont:version_db`: Versión de la base de datos empleada.
- `ont:vote_count`: Número de votos recibidos de la película en TMDB.
- `ont:spoken_languages`: Idiomas hablados en la película.
- `ont:budget`: Presupuesto de la película.
- `ont:overview`: Resumen de la película.
- `ont:review_text`: Texto de la reseña.
- `ont:movieId`: ID de la película en la base de datos.
- `ont:runtime`: Duración de la película.
- `ont:release_date`: Fecha de estreno de la película.
- `ont:title`: Título de la película.
- `ont:vote_average`: Puntaje promedio de reseñas de la película.
- `ont:retorno`: Retorno financiero de la película.
- `ont:status`: Estado de la película.
- `ont:revenue`: Recaudación total de la película.
- `ont:popularity`: Puntaje de popularidad de la película.
- `ont:release_year`: Año de estreno de la película.
- `ont:original_language`: Idioma original de la película.

### Object Properties
- `ont:contains_Index`: Relaciona el índice que se ha creado en un dataset.
- `ont:contains_Table`: Relaciona una tabla que se ha creado en un dataset.
- `ont:has_Dataset`: Relaciona los metadatos de un catálogo de datos con el dataset que describe.
- `ont:has_field`: Relaciona una tabla o índice con sus campos `Field`.
- `ont:partOf_DataCatalog`: Relaciona los metadatos de un dataset o base de datos con un catálogo de datos.
- `ont:partOf_Database`: Relaciona un dataset con la base de datos en la que reside.
- `ont:partOf_Dataset`: Relaciona una tabla o índice con el dataset del que forma parte. Este es el inverso de `contains_Table`.
- `ont:stores`: Relaciona una base de datos con un dataset creado en ella.
- `ont:has_producer`: Relaciona una película con la productora que la realizó.
- `ont:has_genre`: Relaciona una película con su género.
- `ont:has_franchise`: Relaciona una película con la franquicia a la que pertenece.
- `ont:has_review`: Relaciona una película con las reseñas `Review` que ha recibido.
### Individuals
- `ont:MoviesData`: Instancia de `Dataset`. Representa un dataset con información sobre películas.
  - `ont:partOf_DataCatalog`: Pertenece al catálogo de datos `MoviesDataCatalog`.
  - `ont:partOf_Database`: Está almacenado en la base de datos `PostgreSQL`.
  - `ont:description`: "Dataset con información sobre películas".

- `ont:PostgreSQL`: Instancia de `Database`. Representa una base de datos de tipo relacional usando PostgreSQL.
  - `ont:partOf_DataCatalog`: Pertenece al catálogo de datos `MoviesDataCatalog`.
  - `ont:stores`: Almacena el dataset `MoviesData`.
  - `ont:creationDate`: "2024-10-20T00:00:00" (Fecha de creación de la base de datos).
  - `ont:databaseEngine`: "Relacional. PostgreSQL" (Tipo de motor de base de datos).
  - `ont:description`: "Base de datos de PostgreSQL".

- `ont:MoviesDataCatalog`: Instancia de `DataCatalog`. Es un catálogo de datos relacionado con el dataset de películas.
  - `ont:has_Dataset`: Incluye el dataset `MoviesData`.
  - `ont:description`: "Catálogo de datos relacionado con el dataset de películas".



