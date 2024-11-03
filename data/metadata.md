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

Nuestro dataset no estructurado se obtiene a partir de la técnica de ***webscraping*** sobre estas páginas web. Se extraen 42407 reseñas de películas en total. Estas reseñas se insertan en ElasticSearch. 

ElasticSearch es un motor de búsqueda y análisis que permite almacenar, buscar y analizar grandes volúmenes de datos. Además, puede utilizarse como una base de datos NoSQL orientada a documentos, aunque no es un SGBD completo como lo pueden ser PostgreSQL o MySQL porque no ofrece el mismo nivel de consistencia de los datos.


Un ejemplo de documento extraido del dataset de reviews es el siguiente:

```
{'_index': 'reviews',
 '_id': 1,
 '_source': {'Unnamed: 0': 1,
  'review_text': 'this movie is a work of art the finest sequel ever made i dont think we will see another movie like
  this for a long time heath ledgers joker is the best movie charachter i have ever seen by far avengers endgame is
  great but the dark knight is much better the best batman ever the best joker ever the best dc movie ever the best
  superhero movie ever ando for me the best movie ever'}}
```

El propósito de almacenar las reseñas de películas junto con el nombre de la película es relacionar el conjunto de datos estructurado en PostgreSQL con el conjunto no estructurado de reseñas, de modo que ambas bases de datos se ejecuten en dos contenedores diferentes dentro de una misma red en Docker. De esta forma, se pueden realizar consultas de texto sobre un conjunto de películas o sobre una en específico.
