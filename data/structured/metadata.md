## Fuente de los Datos
- **Origen:** Ciudad de Los Ángeles, Datos Públicos
- **URLs de origen:**
  - [Crime Data from 2020 to Present](https://data.lacity.org/Public-Safety/Crime-Data-from-2020-to-Present/2nrs-mtv8/about_data)
  - [Arrest Data from 2020 to Present](https://data.lacity.org/Public-Safety/Arrest-Data-from-2020-to-Present/amvf-fr72/about_data)
  
## Fecha de Recogida
- **Fecha:** 21-10-2024

## Formato de los Datos
- **Formato:**
  - CSV
  - TSV
  
## Licencia de Uso
- **Licencia:** [Licencia de Datos Abiertos de la Ciudad de Los Ángeles](https://data.lacity.org/terms)
- **Enlace a la licencia:** [URL de la licencia](https://data.lacity.org/terms)

## Descripción de las variables del conjunto de datos de crímenes

| Nombre de la Variable       | Tipo de Dato | Descripción                                     |
|-----------------------------|--------------|-------------------------------------------------|
| DR_NO                       | int64        | Número de reporte del incidente.                |
| Date Rptd                   | object       | Fecha en que se reportó el incidente.           |
| DATE OCC                    | object       | Fecha en que ocurrió el incidente.              |
| TIME OCC                    | int64        | Hora en que ocurrió el incidente (formato 24h).|
| AREA                        | int64        | Código del área donde ocurrió el incidente.     |
| Crm Cd                      | int64        | Código del crimen específico.                    |
| Crm Cd Desc                 | object       | Descripción del crimen.                          |
| Vict Age                    | int64        | Edad de la víctima.                             |
| Vict Sex                    | object       | Sexo de la víctima (masculino, femenino, etc.).|
| LOCATION                    | object       | Descripción de la ubicación del incidente.      |
| LAT                         | float64      | Latitud de la ubicación del incidente.          |
| LON                         | float64      | Longitud de la ubicación del incidente.         |

## Descripción de las variables del conjunto de datos de arrestos

| Nombre de la Variable                    | Tipo de Dato | Descripción                                     |
|------------------------------------------|--------------|-------------------------------------------------|
| Report ID                               | int64        | Identificador único del reporte de arresto.     |
| Arrest Date                             | object       | Fecha en que se realizó el arresto.            |
| Area ID                                 | int64        | Código del área donde ocurrió el arresto.       |
| Area Name                               | object       | Nombre del área donde ocurrió el arresto.       |
| Age                                     | int64        | Edad del arrestado.                             |
| Sex Code                                | object       | Código de sexo del arrestado (M/F/otro).       |
| Charge                                  | object       | Cargo específico por el que se realizó el arresto. |
| Disposition Description                  | object       | Descripción del resultado o disposición del caso. |
| Address                                 | object       | Dirección donde ocurrió el arresto.            |
| LAT                                     | float64      | Latitud de la ubicación del arresto.           |
| LON                                     | float64      | Longitud de la ubicación del arresto.          |
