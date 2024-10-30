# 04/10/2023

Comenzamos con una introducción con ***menti.com*** 71276000
Se trata de una especie de tormentas de ideas con dos preguntas
- Qué esperas del curso /  módulo
- Qué crees que es la BI


1. Vídeos de YouTube
   1. Qué es Bussines Intelligence
2. Pasamos a ver el el primer pdf de "***01 Introducción BI.pdf***"
3. ***02 Analisis de Datos.pdf***
   En las empresas se disponen diferentes perfiles, como se ve en la primera página. Una persona se puede especializar en algo concreto o en todo.
   En cuanto a los roles en Data Analytics, salen 5, pero ahora ya se han duplicado o mas. 
   `Kibana` la vamos a utilizar con Elastic Stack

El ejercicio de consultar una API, nos lleva a la URL: https://servicios.ine.es/wstempus/jsCache/ES/SERIES_TABLA/4067

# 11/10/2023

Hacer una máquina virtual con PostgreSQL

En ejemplo de diseño de un cubo; modelado de datos en base de datos multidimiensional. 
- [YouTube](https://www.youtube.com/watch?v=jJG0INtiOa8)


ETL y ELT, ELT es una alternativa que hace la carga antes de la transformación.


# 18/10/2023

Comenzamos la clase, dando tiempo para la practica de modelado

Ponemos en marcha Nifi y hacemos Actividad 1 

# 25/10/2023

Seguimos haciendo actividades de NiFi desde la 2 hasta la 6. La mayoría de la gente no llega al final.


# 08/11/2023

Comenzamos la clase instalando PostgreSQL para poder hacer las actividades 7 y 8


---
---
---
---
---


# Anotaciones

En casa: Web API genera nombres de forma automática
- https://randomuser.me/api/
- https://randomuser.me/api/0.8


# Resolución y Problemas con actividades: 

## 03.07 Conexión con PostgreSQL

En la práctica 7 de Nifi, donde insertamos en una tabla de PostgreSQL, tenemos que crear una tabla, por ejemplo como la siguiente: 

```sql
create table public.tbl_users(
	id serial PRIMARY KEY,
	title VARCHAR ( 20 )  NOT NULL,
	"first" VARCHAR ( 20 )  NOT NULL,
	"last" VARCHAR ( 20 )  ,
	email VARCHAR ( 255 ) NOT NULL,
	created_on TIMESTAMP NOT NULL);
```

También podemos probar que funciona la tabla mediante la sentencia:

```sql
insert into tbl_users (title, first, last, email, created_on)
values( 'Sergio', 'Rey', 'Martinez', 'a@a.a', NOW());
```

Por otra parte, los valores a configurar en el ***DBCPConnectionPool*** de la propiedad *JDBC Connection Pool* dentro del processor ***ConvertJSONToSQL*** son:

- Connection URL: *jdbc:postgresql://127.0.0.1:5432/pruebasnifi*
- Driver class name: *org.postgresql.Driver*
- Driver Location: */home/hadoop/Descargas/postgresql-42.6.0.jar*
- Database User: *xxxxxxxxxxx*
- Password: *xxxxxxxxx*

Aunque una vez definidas (ejercicio 8) las variables en el fichero de configuración ***conf/db.propierties***, estos valores pasarán a ser:

- Database Connection URL: *${postgres.url}*
- Database Driver Class Name: *${postgres.driverclassname}*
- Database Driver Location: *${postgres.driverpath}*
- Database User: *${postgres.username}*
- Database Password: *${postgres.password}*

##  03.08. Variables

> **Problema**: No consigo que acepte la variable de `${postgres.driverpath}`. Si no la pongo funciona, y si la pongo no va. He cambiado de nombre a la variable y tampoco.
- Nota: en los apuntes pone `${postgres.uri}` y no `${postgres.url}`


## P03.09. Consulta de una API con NiFi. Estadístiques INE

> **Problema**: No soy capaz de utilizar el Processor para obtener los datos del INE directamente

En teoría los datos se extraen de:

- https://servicios.ine.es/wstempus/jsCache/ES/SERIES_TABLA/4067?nult=1

La sql para crear la tabla de postgres

```sql
CREATE TABLE tbl_INE(
	ID_auto		SERIAL primary key
  ,Id               INTEGER  NOT NULL 
  ,COD              VARCHAR(9) 
  ,FK_Operacion     INTEGER 
  ,Nombre           VARCHAR(200) 
  ,Decimales        BIT 
  ,FK_Periodicidad  INTEGER 
  ,FK_Publicacion   INTEGER 
  ,FK_Clasificacion INTEGER 
  ,FK_Escala        INTEGER
  ,FK_Unidad        INTEGER 
);
```
La actividad la he tenido que repetir hasta que he conseguido meter los datos en la tabla, porque daba un error de que los campos no cuadraban, pero al final van: no es case-sensitive, en teoría, porque todo en postgres es en minúsculas y los datos en json vienen con mayúsculas.

> **Pregunta**: No comprendo porqué esta el paso de aplanar el json en un csv.

### Para hacer el json plano en csv

Aquí hay unos tutoriales, pero no han hecho falta, sin tocar nada ha ido.

Sigo el tutorial de [Hands on Apache NiFi: Converting JSON to CSV](https://rihab-feki.medium.com/converting-json-to-csv-with-apache-nifi-a9899ca3f24b). Dice que se necesita hacer el AvroSchema, y para eso utilizo la web [Convert JSON to Avro](https://konbert.com/convert/json/to/avro), pero esto ha sido un lio y no ha funcionado.

## P03.10. Consulta de una API con NiFi. Estadístiques Futbol

Aquí es donde se debe tener la BBDD MongoDB Cloud. 

> **Preguntar** a Jose cómo se accede, ahora mismo pone que no esta disponible

## P03.11. Cas d'us amb model analitic. Evolució temperatures Tarea

En esta actividad deberíamos actualizar el enunciado de 2022 a 2023

- `API KEY` de AEMET= "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJzLnJleW1hcnRpbmV6QGVkdS5ndmEuZXMiLCJqdGkiOiI4MmUwMmQ1ZS1mMzQ2LTQ1ZTUtYTNjMS1iZGE4YTgzODZkZWUiLCJpc3MiOiJBRU1FVCIsImlhdCI6MTY5ODI0NzgyNCwidXNlcklkIjoiODJlMDJkNWUtZjM0Ni00NWU1LWEzYzEtYmRhOGE4Mzg2ZGVlIiwicm9sZSI6IiJ9.Gz70HA68AIbaTeQZBAQpEkVgFaD77KLt_HaxXil1Rr0"

Para la petición de datos se ponen las fechas, no pueden superar los 30 días:
- 2021-01-01T00:00:00UTC   # Fecha Inicial
- 2021-02-01T00:00:00UTC   # Fecha Final
- 8293X                    # Indicativo

- Acceso mediante: https://opendata.aemet.es/dist/index.html?#/valores-climatologicos/Climatolog%C3%ADas_diarias

Con esto se obtienen enlaces a datos efímeros, que tiene fecha de caducidad 

> **Pregunta:** No tengo claro el objetivo de la práctica: 
> 1. Obtención de datos
> 2. Realización del modelado de datos.
> 3. Ingesta de datos desde NiFi según el modelado obtenido
> 4. ¿reporting?¿con qué herramienta?
> **Pregunta:** 
> 1. Los datos de AEMET ¿Tienen que pedirse mes a mes?¿Cómo se hace?¿Se descargan los ficheros?
> 2. Los datos de los municipios también se meten a mano?¿Sólo tienen sentido si se hace de todas las estaciones meteorológicas pero no si solo se hace de Xàtiva?
> 3. ¿Como se hace el *match* entre poblaciones y estaciones, por el nombre?

### Tablas de Base de Datos

```sql
-- Tabla equivalente a json datos aemet Xàtiva 
CREATE TABLE tbl_aemet(
   id          SERIAL primary KEY
  ,fecha       DATE  
  ,indicativo  VARCHAR(5) 
  ,nombre      VARCHAR(6) 
  ,provincia   VARCHAR(8) 
  ,altitud     INTEGER  
  ,tmed        VARCHAR(4) 
  ,prec        VARCHAR(4) 
  ,tmin        VARCHAR(4) 
  ,horatmin    VARCHAR(6) 
  ,tmax        VARCHAR(4) 
  ,horatmax    VARCHAR(5) 
  ,dir         INTEGER 
  ,velmedia    VARCHAR(3) 
  ,racha       VARCHAR(4) 
  ,horaracha   VARCHAR(5) 
  ,sol         VARCHAR(3)
  ,presMax     VARCHAR(6) 
  ,horaPresMax VARCHAR(6) 
  ,presMin     VARCHAR(6) 
  ,horaPresMin VARCHAR(6) 
);
```

# Enlaces con ejemplos:

Enlaces con ejemplos

- [Connect and Import data from any REST API in Apache Nifi](https://www.progress.com/tutorials/jdbc/connect-and-import-data-from-any-rest-api-in-apache-nifi)
- [Como transformar un CSV en JSON usando Apache Nifi en 6 pasos](https://www.linkedin.com/pulse/como-transformar-un-csv-en-json-usando-apache-nifi-6-pasos-)
- [Ingesta Datos - Apache NiFi - Twitter](https://www.youtube.com/watch?v=DGDckUZdjhg)
- [Generate Retail Data using Apache NiFi Data Pipeline(eCommerce Data Simulator) | Data Making | DM](https://www.youtube.com/watch?v=V2skgFgm32c)
  - Hace un `InvokeHTTP` de https://randomuser.me/api/0.8
  - Añade información al *json* mediante `JoltTransformJSON`
- [Data Flow With NiFi: (Writing Data to MySQL, MongoDB and Slack from Stream Data API)](https://medium.com/@bilative/data-flow-with-nifi-writing-data-to-mysql-mongodb-and-slack-from-stream-data-api-d8c983ed66a0)
  - Comienza con un `InvokeHttp`, continua con una transformacion `JoltTranfomationJSON`, MUY INTERESANTE

# Estudios de casos:

## Pasar de un JSON a un PostgreSQL -- Práctica 8

Si tenemos un JSON sencillito, lo podemos insertar en una tabla postgres con :

GenerateFlowFile --> ConvertJSONToSQL  -->  PutSQL

Se debe generar el servicio con los datos de PostgreSQL en los dos últimos procesadores.

{
"title": "mr",
"first": "John ${random():mod(10):plus(1)}",
"last": "Doe ${random():mod(10):plus(1)}",
"email": "johndoe${random():mod(10):plus(1)}nail.com",
"created_on": "${now():toNumber()}"
}

## Práctica 10. 

A partir de la web https://randomuser.me/api/1.0 realizamos varias acciones.

### Aplanar el Json

- **opción 1**: nos interesa todo el JSON

Usamos un `FlatenJson` y lo configuramos cambiando el *separator* 

- **opción 2**: Solo nos interesa una parte o clave del JSON:

En primer lugar hacemos un `SplitJson` indicando en la propiedad `JsonPath Expression` la ruta que nos lleva a la parte de la estructura a extraer, por ejemplo *$.results*. 
Una vez llegados este punto pasamos por el `FlatenJson` tal y como se indica en la anterior opción

Debemos tener en cuenta que `SplitJson` obtendra la division del FlowFile inicial en 0, 1 o más FlowFiles, según sea el Json original, por ejemplo, si ponemos *$[\*]*, entonces hará un FlowFile por cada clave del nivel superior del Json, si ponemos *$.results*, entonces obtendremos un FlowFile si esta clave se encuentra en el nivel superior o ninguno si no se encuentra. 


- **opción 3**: Es un subnivel interior

En este caso, se siguen las indicaciones de la opción anterior y después del `SplitJson` se debe añadir un `EvaluateJson` donde se especifica la parte que se desea extraer en la propiedad `JsonPaths` (que debe ser añadida al processor), y se indica el path hasta llegar al datos o datos que se quieren por ejemplo *$.name*.

Más ejemplos de cómo acceder a un valor o valores de una clave en [JsonPath](https://nifi.apache.org/docs/nifi-docs/html/expression-language-guide.html#jsonpath).

Si después de hacer el `FlattenJson` nos encontramos con campos con nombre compuesto, como por ejemplo *name_first* y *name_last" y solo nos queremos quedar con *first* y *last*, esto debemos tratarlo usando el Processor `ResplaceText` y especificando los literales que queremos cambiar o eliminar. Si tenemos más de un literal a eliminar, lo podemos hace con un mismo processor utilizando `!` como separador entre diferentes expresiones, por ejemplo "Hola|Adios" reemplazaría ambas palabras.

Posteriormente, para terminar el paso a PostgreSQL necesitamos `ConvertJSONtoSQL` y `PutSQL`, especificando los valores de siempre e ***ignorando los campos no encontrados***



### Jolt

Por si hacemos un poco de repaso de este processor, aquí algunos enlaces de interés:

- [Cloudera: Jolt quick reference for Nifi Jolt Processors](https://community.cloudera.com/t5/Community-Articles/Jolt-quick-reference-for-Nifi-Jolt-Processors/ta-p/244350)






24/10/2024

Probando con máquina Proxmox instaladon mongoDB


# MongoDB en Contenedores Proxmox

Puesta en marcha de los contenedores con MongoDB

1. Hacemos la isntalación tal y como pone la web
Pasos para conectar


# MongoDB Atlas

Dado de alta:
Usuario : s.reymartinz@edu.gva.es
Password: Simarret23

Usuario para conexión al *Cluster()*
Usuario administrativo
Usr: sreymartinez
Pwd: AW0yoVT2B829MJup


## Accesp a **Mongo Atlas**

Diferentes formas de acceder :

### Con Python

Instalación del driver
```bash
python -m pip install "pymongo[srv]"
```

Ejemplo de conexión a la base de datos:

```python

from pymongo.mongo_client import MongoClient
from pymongo.server_api import ServerApi

uri = "mongodb+srv://sreymartinez:AW0yoVT2B829MJup@cluster0.azyjt.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"

# Create a new client and connect to the server
client = MongoClient(uri, server_api=ServerApi('1'))

# Send a ping to confirm a successful connection
try:
    client.admin.command('ping')
    print("Pinged your deployment. You successfully connected to MongoDB!")
except Exception as e:
    print(e)
```

### Con MongoçDB Compass

La conexión es 

```
mongodb+srv://sreymartinez:AW0yoVT2B829MJup@cluster0.azyjt.mongodb.net/
```


### Utilizando *mongosh*

Comando siguiente:

```
mongosh "mongodb+srv://cluster0.azyjt.mongodb.net/" --apiVersion 1 --username sreymartinez --password AW0yoVT2B829MJup
```

### Utilizando **VS Code**

Connecting with MongoDB for VS Code
1. Install MongoDB for VS Code.
In VS Code, open "Extensions" in the left navigation and search for "MongoDB for VS Code." Select the extension and click install.
2. In VS Code, open the Command Palette.
Click on "View" and open "Command Palette."
Search "MongoDB: Connect" on the Command Palette and click on "Connect with Connection String."
3. Connect to your MongoDB deployment.
Paste your connection string into the Command Palette.
Show Password

```
mongodb+srv://sreymartinez:AW0yoVT2B829MJup@cluster0.azyjt.mongodb.net/
```

The password for sreymartinez is included in the connection string for your first time setup. This password will not be available again after exiting this connect flow.
4. Click “Create New Playground” in MongoDB for VS Code to get started.
Learn more about Playgrounds 



### Utilizando Atlas SQL Connect

En este caso tenemos dierentes opciones:

 - **Power Bi Connection** : mongodb://atlas-sql-671d161b7183374ebf59511c-azyjt.a.query.mongodb.net/sample_mflix?ssl=true&authSource=admin
 - **JDBC Driver** : jdbc:mongodb://atlas-sql-671d161b7183374ebf59511c-azyjt.a.query.mongodb.net/sample_mflix?ssl=true&authSource=admin  - Puerto: 27017
 - **ODBC Driver** : mongodb://atlas-sql-671d161b7183374ebf59511c-azyjt.a.query.mongodb.net/sample_mflix?ssl=true&authSource=admin


# Instalación NiFi en Ubuntu Docker

Generated Username [52fa93d8-14a8-4507-8c9c-5697aa1fbb51]
Generated Password [i4x6oUWeiXk6MidVqUGKJj4SXdInu0ek]

https://localhost:8443/nifi/login