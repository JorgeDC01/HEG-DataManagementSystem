*En la primera release del repositorio, se recuperaron dos datasets de crímenes y arrestos en Los Angeles. Esta primera idea se ha sustituido temporalmente por la recolección de películas y reviews, facilitando la creación de una relación entre los datos estructurados (películas) y los no estructurados (reviews).*

# Datos estructurados - Películas
## Fuente de los Datos
- **Origen: Repositorio en Github** 
- **URLs de origen:**
  - [Movies](https://github.com/melodiromero/movies/blob/main/dataset/movies_limpio.csv)
  
## Fecha de Recogida
- **Fecha:** 27-10-2024

## Formato de los Datos
- **Formato:**
  - CSV
  
## Licencia de Uso
- **Licencia:** [Licencia de Datos](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)
- **Enlace a la licencia:** [URL de la licencia](https://help.imdb.com/article/imdb/general-information/content-licensing/GZGA5HDQ8NE97LVR?ref_=helpart_nav_45#)

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



