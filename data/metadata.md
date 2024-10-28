*En la primera release del repositorio, se recuperaron dos datasets de crímenes y arrestos en Los Angeles. Esta primera idea se ha sustituido temporalmente por la recolección de películas y reviews, facilitando la creación de una relación entre los datos estructurados (películas) y los no estructurados (reviews).*

# Datos estructurados - Películas
## Fuente de los Datos
- **Origen:** 
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
| budget                      | int64        | El presupuesto de la película, en dólares.                                                         |
| genres                      | object       | Un diccionario que indica todos los géneros asociados a la película.                               |
| id                          | int64        | ID de la película en la base de datos.                                                             |
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

# Datos no estructurados - Reviews
## Fuente de los Datos
- **Origen:** 
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

## Descripción de las variables del conjunto de datos de Reviews

| Nombre de la Variable       | Tipo de Dato | Descripción                                                                                         |
|-----------------------------|--------------|-----------------------------------------------------------------------------------------------------|
| name                        | object       | Título de la película.                                                                              |
| review                      | object       | Reseña acerca de la película.                                                                       |

## Proceso de recolección de las reviews
El dataset de Kaggle contiene 17 archivos con formato ".csv", cada uno de ellos contiene un conjunto de películas de un género en concreto. 
![image](https://github.com/user-attachments/assets/9c97fca9-e10c-4f9c-9dd9-8b12d6de2fa3)

Cada ".csv" almacena un conjunto de películas con diferentes atributos: name, year, movie_rating ... review_url. Este último atributo es un enlace a una página web en IMDB que contiene 25 reviews de la película correspondiente. La siguiente imagen corresponde a una página web de reviews:
![image](https://github.com/user-attachments/assets/a5631176-9997-4a74-b969-c7313b3ccea9)

Nuestro dataset no estructurado se obtiene a partir de la técnica de ***webscraping*** sobre estas páginas web.
