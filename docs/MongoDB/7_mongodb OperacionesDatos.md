---
title: 7. Consulta de datos
description: Descripción de las funciones y métodos que nos permiten realizar consultas sobre colecciones de datos
---


# 7. Operaciones con datos: Consultas y métodos

Ahora que ya tenemos más herramientas y hemos visto las operaciones básicas de *MongoDB* vamos a profundizar sobre las consultas de los datos, aunque ya las hemos visto brevemente con anterioridad.

Aprovechamos para introducir una base de datos con una colección con datos de prueba. En el siguiente [enlace](https://www.w3resource.com/mongodb-exercises/mongodb-sample-dataset/sample_mflix/movies.zip) tenemos una base de datos de películas que podemos introducir en *MongoDB* por ejemplo usando en *MongoDB* Compass. Para ello creamos una base de datos llamada **consultas** e importamos el fichero descargado.

El comando básico es `.find()`

```js
db.collection.find()            // devuelve todos los documentos
db.collection.find(<filter>)    // devuelve los documentos que cumplen el filtro
```

## Operadores *MongoDB*

Antes de continuar, en la siguiente tabla esta el listado de los principales operadores utilizados en consultas *MongoDB* para la construcción de los filtros:

| Operador    | Descripción                                  | Ejemplo                                   |
|-------------|----------------------------------------------|-------------------------------------------|
| `$eq`       | Igualdad                                     | `db.collection.find({campo: {$eq: valor}})` |
| `$ne`       | No igual                                     | `db.collection.find({campo: {$ne: valor}})` |
| `$gt`       | Mayor que                                    | `db.collection.find({campo: {$gt: valor}})` |
| `$gte`      | Mayor o igual que                            | `db.collection.find({campo: {$gte: valor}})` |
| `$lt`       | Menor que                                    | `db.collection.find({campo: {$lt: valor}})` |
| `$lte`      | Menor o igual que                            | `db.collection.find({campo: {$lte: valor}})` |
| `$in`       | Igual a cualquiera de los valores en un array | `db.collection.find({campo: {$in: [valor1, valor2]}})` |
| `$nin`      | No igual a ninguno de los valores en un array| `db.collection.find({campo: {$nin: [valor1, valor2]}})` |
| `$exists`   | Verifica si el campo existe                 | `db.collection.find({campo: {$exists: true/false}})` |
| `$type`     | Verifica el tipo de datos del campo         | `db.collection.find({campo: {$type: tipo}})` |
| `$regex`    | Realiza una búsqueda de expresión regular   | `db.collection.find({campo: {$regex: /patrón/}})` |
| `$or`       | Realiza una disyunción lógica               | `db.collection.find({$or: [{condición1}, {condición2}]})` |
| `$and`      | Realiza una conjunción lógica               | `db.collection.find({$and: [{condición1}, {condición2}]})` |
| `$not`      | Niega una expresión                          | `db.collection.find({campo: {$not: {condición}}})` |
| `$nor`      | Realiza una disyunción negada               | `db.collection.find({$nor: [{condición1}, {condición2}]})` |

Estos operadores son fundamentales para realizar consultas avanzadas en *MongoDB*, permitiendo filtrar y buscar documentos en función de diferentes criterios. Puedes combinar estos operadores para construir consultas complejas y poderosas que se adapten a tus necesidades específicas.


## Consultas con `.find`. Ejemplos prácticos.

Utilizando la tabla de *movies* vamos a realizar algunas consultas que servirán para ilustrar los aspectos más interesantes de las búsquedas en *MongoDB*. 

Veamos en primer lugar un registro tipo

```js
{
  "_id": {
    "$oid": "573a1390f29313caabcd4135"
  },
  "plot": "Three men hammer on an anvil and pass a bottle of beer around.",
  "genres": [
    "Short"
  ],
  "runtime": 1,
  "cast": [
    "Charles Kayser",
    "John Ott"
  ],
  "num_mflix_comments": 1,
  "title": "Blacksmith Scene",
  "fullplot": "A stationary camera looks at a large anvil with a blacksmith behind it and one on either side. The smith in the middle draws a heated metal rod from the fire, places it on the anvil, and all three begin a rhythmic hammering. After several blows, the metal goes back in the fire. One smith pulls out a bottle of beer, and they each take a swig. Then, out comes the glowing metal and the hammering resumes.",
  "countries": [
    "USA"
  ],
  "released": {
    "$date": {
      "$numberLong": "-2418768000000"
    }
  },
  "directors": [
    "William K.L. Dickson"
  ],
  "rated": "UNRATED",
  "awards": {
    "wins": 1,
    "nominations": 0,
    "text": "1 win."
  },
  "lastupdated": "2015-08-26 00:03:50.133000000",
  "year": 1893,
  "imdb": {
    "rating": 6.2,
    "votes": 1189,
    "id": 5
  },
  "type": "movie",
  "tomatoes": {
    "viewer": {
      "rating": 3,
      "numReviews": 184,
      "meter": 32
    },
    "lastUpdated": {
      "$date": "2015-06-28T18:34:09Z"
    }
  }
}
```

Comenzamos con consultas sencillas y vamos incrementando el nivel.

### Consultas básicas

- ^^Obtener películas lanzadas en el año 1983^^ :

```js
db.movies.find({ year: 1993 })
``` 

- ^^Buscar películas de genero "corto" (genres = short)^^

```js
db.movies.find({ "genres": "Short" })
```

### Consultas con operadores lógicos 

- ^^Buscar películas de duración (runtime) superior a 120 minutos^^

```js
db.movies.find({ "runtime": { $gt: 120 } })
```

- ^^Buscar películas cuya duración se encuentre entre 90 y 100 minutos ambos incluidos^^

```js
db.movies.find({ "runtime": { $gte: 90, $lte: 100} })
```

- ^^Buscar películas que no tengan puntuación (UNRATED) y que duren más de 100 minutos^^. 

En este caso, podemos utilizar `$and` de forma implícita añadiendo la condición con un array o explicita si poner nada más que los campos que deben cumplirse

```js
db.movies.find({ rated: "PASSED", runtime: { $gt: 100} })
```

Equivale exactamente a 

```js
db.Movies.find({ 
    $and: [ 
        { rated: "PASSED"}, 
        { runtime: { $gt: 100}} 
    ]
})
```

A partir de esta el `$or` es similar.

!!! nota

    Podemos utilizar `.count()` al final para hacer recuento y verificar que el resultado de las dos consultas anteriores son iguales.

- ^^Buscar las películas que han recibido algún premio^^: 

En este caso debe existir la propiedad *awards*

```js
db.movies.find({
  "awards": { $exists: true }
})
```

- ^^Buscar las películas donde ha intervenido el actor "Charles Kayser", pero mostrar solo titulo, fecha de lanzamiento, idioma, director y premios ganados^^
```js
db.movies.find({
    "cast": "Charles Kayser"
    }, {
    "title": 1,
    "released": 1,
    "languages": 1,
    "directors": 1,
    "awards.wins": 1,
})
```


- ^^Buscar películas lanzadas en una fecha determinada, y mostrando unos campos determinados.^^:

```js
db.movies.find({
    released: ISODate("1997-05-01T00:00:00.000Z")
    }, {
    title: 1,
    languages: 1,
    released: 1,
    directors: 1,
    writers: 1,
    countries: 1
  }
)
```

### Consultas sobre objetos anidados

- ^^Buscar películas que han recibido más de 1000 votos en IMDb^^

```js
db.movies.find({ "imdb.votes": { $gt: 1000 } })
```

- ^^Buscar películas que tienen una puntuación IMDb superior a 7^^

```js
db.movies.find({ "imdb.rating": { $gt: 7 } })
```

- ^^Buscar las películas que han sido nominadas y mostrar solo el titulo, directores y año^^. 

En este caso como novedad seleccionamos solo unos campos

```js
db.movies.find({
    "awards.nominations": { $gt: 0 }
  }, {
    "title": 1,
    "directors": 1,
    "year": 1
  })
```


- ^^Buscar películas con una puntuación entre 3 y 4 en el rating de viewer en tomatoes^^. Mostrar solo algunos campos por simplicidad

```js
db.movies.find(
  { 'tomatoes.viewer.rating': { $gte: 3, $lt: 4 } },
  { 
    title: 1, 
    languages: 1, 
    released: 1, 
    directors: 1, 
    'tomatoes.viewer': 1, 
    writers: 1, 
    countries: 1 
  }
)
```

### Consultas con expresiones regulares

Si queremos realizar consultas sobre partes de un campo de texto, hemos de emplear expresiones regulares. Para ello, tenemos el operador `$regexp` o, de manera más sencilla, indicando como valor la expresión regular a cumplir:

- ^^Buscar todas las películas (titulo, lenguaje, lanzamiento, directores y guionistas) que tenga el literal "scene" en el titulo^^

```js
db.movies.find(
    { title: { $regex: /scene/i } },
    { title: 1, 
      languages: 1, released: 1, directors: 1, writers: 1, countries: 1 }
)
```

equivale a 

```js
db.movies.find(
    { title: /scene/i },
    { title: 1, languages: 1, released: 1, directors: 1, writers: 1, countries: 1 }
)
```

- ^^Buscar películas que tengan en su sinopsis (fullplot) la palabra "fire", sean anteriores al 1980 y que sean del genero "Shorts"^^

```js
db.movies.find(
  {
    fullplot: { $regex: /fire/i },
    year: { $lt: 1980 },
    genres: "Short"
  }, 
  { 
    title: 1, 
    year: 1, 
    languages: 1, 
    fullplot: 1, 
    released: 1, 
    directors: 1, 
    writers: 1, 
    countries: 1 
  }
)
```

- ^^Buscar pelicuas que tengan cualquiera de estos actores: "Tom Cruise", "Tom Hanks" o un actor que contenga "Smith" en su nombre^^

```js 
db.movies.find({
    cast: /Tom Cruise|Tom Hanks|Smith/
  }, 
  { 
    _id: 0,
    title: 1, 
    cast: 1,
    year: 1
  }
)
```

!!! note "Consulta de elementos de un array"

    Mediante las expresiones regulares hemos visto cómo se consulta de forma sencialla elementos de un array, sin embargo tenemos un filtro especifico para estos casos `$elemMatch`  

    ```js
      db.movies.find({ cast: { $elemMatch: { $eq: "Leonardo DiCaprio" }}});
    ```
    Mientras que expresiones regulares encuentra elementos que coindicen parcial o totalmente, con este filtro buscamos elementos exactos.

## Otros métodos interesantes.

Existen gran variedad de métodos que nos permiten realizar todo tipo de consultas sobre una base de datos y más concretamente sobre una colección. 

Un listado detallado de todos estos métodos lo encontramos en la web oficial de MongoDB: [MongoDB Manual - Collection Methods](https://www.mongodb.com/docs/manual/reference/method/js-collection/)

Pueden resultar interesante las siguientes:

### Método `.distinct()`

Obtiene los valores diferentes de un campo de una colección: 

```js
db.movies.distinct( 'genres' );
```
```js
db.movies.distinct( 'type' );
```
Estos dos ejemplos obtienen los diferentes valores de los atributos indicados.


### Método `.count()`

Cuenta la cantidad de documentos que cumplen una condición.

```js
db.movies.find({
    year: { $lt: 2000 },
    genres: "Short"
}).count()
```

Obtenemos la cantidad de *cortos* de la colección, posterior al año 2000

- ^^Buscar la cantidad películas que tengan en su sinopsis (fullplot) la palabra "fire", sean anteriores al 1980 y que sean del genero "Shorts"^^

```js
db.movies.find({
    fullplot: { $regex: /fire/i },
    year: { $lt: 1900 },
    genres: "Short"
}).count()
```


### Método `.limit()`

Muestra una cantidad de documentos indicada que cumplan una condición.

```js
db.movies.find({
    year: { $lt: 2000 },
    genres: "Short"
}).limit(5)
```

Obtenemos 5 documentos de *cortos* de la colección, posterior al año 2000

### Método `.sort()`

Ordena los documentos devueltos por una consulta. 

La sintaxis básica es la siguiente:

```javascript
db.collection.find().sort({ campo: orden })
```

- **`campo`**: El nombre del campo por el cual deseas ordenar.  
- **`orden`**: Puede ser `1` para orden ascendente o `-1` para orden descendente.

Por ejemplo, para ordenar por año de lanzamiento de forma ascendente:

```javascript
db.movies.find().sort({ year: 1 })
```

Si deseas ordenarlas de forma descendente:

```javascript
db.movies.find().sort({ year: -1 })
```

También podemos ordenar por más de un campo. Por ejemplo, para ordenar primero por `genre` y luego por `rating`:

```javascript
db.movies.find().sort({ genre: 1, rating: -1 })
```


También podemos encadenar estas funciones: 

- ^^Listado de las 5 mejores películas según la puntuación "imdb"^^

```js
db.movies.find({}, {
    title: 1,
    imdb: 1
}).sort({ imdb: 1 }).limit(5)
```

Mejoramos el resultado anterior y le quitamos los valores vacios en la puntuación:

```js
db.movies.find({ "imdb.rating": {"$ne": ""}}, {
    title: 1,
    imdb: 1
}).sort({ imdb: -1 }).limit(5)
```


## Cursores

Al hacer una consulta en el shell se devuelve un cursor. Este cursor lo podemos guardar en un variable, y partir de ahí trabajar con él como haríamos mediante cualquier lenguaje de programación. Si `cur` es la variable que referencia al cursor, podremos utilizar los siguientes métodos:

| Método | Uso | Lugar de ejecución |
| --- | --- | --- |
| `cur.hasNext()` | true/false para saber si quedan elementos | Cliente |
| `cur.next()` | Pasa al siguiente documento | Cliente |
| `cur.limit(*cantidad*)` | Restringe el número de resultados a cantidad | Servidor |
| `cur.sort({*campo*:1})` | Ordena los datos por campo: 1 ascendente o -1 o descendente | Servidor |
| `cur.skip(*cantidad*)` | Permite saltar cantidad elementos con el cursor | Servidor |
| `cur.count()` | Obtiene la cantidad de documentos | Servidor |

Como tras realizar una consulta con `find` realmente se devuelve un cursor, un uso muy habitual es encadenar una operación de `find` con `sort` y/o `limit` y/o `count` para ordenar el resultado por uno o más campos y posteriormente limitar el número de documentos a devolver o simplemente contar.

Estos cursores se utilizan cuando accedemos a consulta desde script en *javascript* u otros lenguajes. 

Por ejemplo, a continuacion tenemos código en *javascript* que imprime por consola todos los documentos obtenidos tras una consulta

=== "javascript"

    ```javascript
    // necesario node.js
    // Conectar a la base de datos test
    const db = connect("mongodb://localhost:27017/test");

    // Realizar una consulta para encontrar todas (5) las películas
    var cursor = db.movies.find().limit( 5);

    // Iterar sobre los resultados usando un cursor
    while (cursor.hasNext()) {
        printjson(cursor.next());
    }
    ```

=== "python"

    ```py
    # necesaria librería pymongo: 
    from pymongo import MongoClient

    # Conectar a la base de datos
    client = MongoClient("mongodb://localhost:27017/")
    # Utilizamos la base de datos "text"
    db = client.test

    # Realizar una consulta para encontrar todas (5) las películas
    cursor = db.movies.find().limit(5)

    # Iterar sobre los resultados usando un cursor
    for document in cursor:
        print(document)
    ```

Se puede también usar métodos como `limit` y `skip` para controlar la cantidad de resultados:

- Ejemplo: ^^obtenemos 5 registros de una colecccón^^

=== "javascript" 

    ```javascript
    // Obtener las primeras 5 películas
    var limitedCursor = db.movies.find().limit(5);

    // Iterar sobre los resultados limitados
    while (limitedCursor.hasNext()) {
        printjson(limitedCursor.next());
    }

    // Saltar las primeras 2 películas y obtener las siguientes 3
    var paginatedCursor = db.movies.find().skip(2).limit(3);

    while (paginatedCursor.hasNext()) {
        printjson(paginatedCursor.next());
    }
    ```

=== "python"

    ```py
    from pymongo import MongoClient

    # Conectar a la base de datos
    client = MongoClient("mongodb://localhost:27017/")
    db = client.test

    # Obtener las primeras 5 películas
    limited_cursor = db.movies.find().limit(5)

    # Iterar sobre los resultados limitados
    for document in limited_cursor:
        print(document)

    # Saltar las primeras 2 películas y obtener las siguientes 3
    paginated_cursor = db.movies.find().skip(2).limit(3)

    # Iterar sobre los resultados paginados
    for document in paginated_cursor:
        print(document)
    ```

En todo caso, estos ejemplos quedan fuera del alcance de nuestro objetivo para este curso. Para más información [MongoDB Documentation → MongoDB Drivers → Node.js](https://www.mongodb.com/docs/drivers/node/v3.6/fundamentals/connection/connect/)

## Agregaciones

Las **agregaciones en MongoDB** son una poderosa herramienta que permite procesar y analizar grandes volúmenes de documentos en una colección. A través de un proceso conocido como **pipeline de agregación**, puedes realizar diversas operaciones en los datos, como filtrar, agrupar, ordenar y transformar documentos. Aquí te explico más sobre su funcionamiento y usos:

Las agregaciones permiten realizar cálculos y transformaciones sobre los datos. En lugar de simplemente recuperar documentos, puedes aplicar funciones que devuelven resultados calculados, como sumas, promedios, conteos, etc. Esto es similar a las consultas SQL que utilizan `GROUP BY`.

### Usos y ventajas de las agreraciones

1. **Agrupación de Datos**: Puedes agrupar documentos por un campo específico y realizar cálculos sobre esos grupos. Por ejemplo, sumar las ventas por cada vendedor.

2. **Filtrado de Datos**: Utilizando la etapa `$match`, puedes filtrar documentos antes de realizar otras operaciones, lo que optimiza el rendimiento.

3. **Transformación de Datos**: Puedes remodelar documentos usando la etapa `$project`, lo que te permite seleccionar y renombrar campos.

4. **Análisis de Tendencias**: Las agregaciones son útiles para analizar cambios en los datos a lo largo del tiempo, como el crecimiento de ventas mensuales.

5. **Operaciones Complejas**: Puedes realizar operaciones más complejas, como uniones entre colecciones, utilizando la etapa `$lookup`.

Las agregaciones son esenciales para obtener `insights` significativos de tus datos en *MongoDB*, acotando el rango de los datos sobre los que trabajamos y facilitando el análisis y la toma de decisiones. 

### Método `.aggregate()`

Para poder agrupar datos y realizar cálculos sobre éstos, *MongoDB* ofrece diferentes alternativas una de ellas es mediante el método `.aggretate`

#### Pipeline de agregación

Las agregaciones usan un pipeline, conocido como Aggregation Pipeline, de ahí que el método aggregate use un array con [ ] donde cada elemento es una fase del pipeline, de modo que la salida de una fase es la entrada de la siguiente:

```js 
db.coleccion.aggregate([op1, op2, ... opN])
```

En la siguiente imagen se resumen los pasos de una agrupación donde primero se eligen los elementos que vamos a agrupar mediante `$match`, el resultado saliente se agrupan con `$group`, y sobre los agrupado mediante `$sum` se calcula el total:

<figure markdown="span" align="center">
  ![Image title](../images/MongoDB/MongoDB31.png){ width="75%"  }
  <figcaption>Ejemplo de pipeline con $match y $group</figcaption>
</figure>



Al realizar un *pipeline* dividimos las consultas en fases, donde cada fase utiliza un operador para realizar una transformación. Aunque no hay límite en el número de fases en una consulta, es importante destacar que el orden importa, y que hay optimizaciones para ayudar a que el pipeline tenga un mejor rendimiento (por ejemplo, hacer un `$match` al principio para reducir la cantidad de datos)


#### Operadores del pipeline

Antes de nada cabe destacar que las fases se pueden repetir, por lo que una consulta puede repetir operadores para encadenar diferentes acciones.

A continuación vamos a estudiar todos estos operadores:

| Operador | Descripción | Cardinalidad |
| --- | --- | :---: |
| `$project` | Proyección de campos, es decir, propiedades en las que estamos interesados. También nos permite modificar un documento, o crear un subdocumento (reshape) | 1:1 |
| `$match` | Filtrado de campos, similar a where | N:1 |
| `$group` | Para agrupar los datos, similar a group by | N:1 |
| `$sort` | Ordenar | 1:1 |
| `$skip` | Saltar | N:1 |
| `$limit` | Limitar los resultados | N:1 |
| `$unwind` | Separa los datos que hay dentro de un array | 1:N |

### Ejemplos de uso

La fase **group** agrupa los documentos con el propósito de calcular valores agregados de una colección de documentos. Por ejemplo, podemos usar **$group** para calcular la cantidad de peliculas por tipo.


```js
db.movies.aggregate([
    { $group: {
        _id: "$type",
        total: { $sum:1}
      }
    }
])
```

!!! warning
    Cuidado: La salida de `$group` esta desordenada

La salida de `$group` depende de cómo se definan los grupos. Se empieza especificando un identificador (por ejemplo, un campo `_id`) para el grupo que creamos con el *pipeline*. Para este campo `_id`, podemos especificar varias expresiones, incluyendo un único campo proveniente de un documento del *pipeline*, un valor calculado de una fase anterior, un documento con muchos campos y otras expresiones válidas, tales como constantes o campos de subdocumentos. 

!!! note
    Cuando referenciemos al valor de un campo lo haremos poniendo entre comillas un `$` delante del nombre del campo. Así pues, para referenciar al fabricante de un producto lo haremos mediante `$fabricante`.


Si lo que queremos es que el valor del identificador contenga un objeto, lo podemos asociar como valor:

```js
db.movies.aggregate([
    { $group: {
        _id: { "Tipo" : "$type"},
        total: { $sum:1}
      }
    }
])
```

<div align="center">
    <img src="../../images/MongoDB/MongoDB32.png" alt="MongoDB Aggregate" width="90%" />
</div>


También podemos agrupar más de un atributo, de tal modo que tengamos un `_id` compuesto. Por ejemplo:

```js
db.movies.aggregate([
    { $group: {
        _id: { 
            "Tipo" : "$type",
            "Género": "$genres"},
        total: { $sum:1}
      }
    }
])
```


El operador `$sum` acumula los valores y devuelve la suma. En los ejemplos anteriores, sumaba 1 por cada elemento contrado, pero podría sumar cualquier valor numérico. 

Por ejemplo, calculamos las nominaciones y premios ganados de las peliculas según el tipo: 

```js
db.movies.aggregate([
    { $group: {
        _id: { "Tipo" : "$type"},
        Nominaciones: { $sum: "$awards.nominations"},
        Ganadas: { $sum: "$awards.wins"}
        
      }
    }
])
```

<div align="center">
    <img src="../../images/MongoDB/MongoDB33.png" alt="MongoDB Aggregate" width="90%" />
</div>


No solo tenemos el operados `$sume`; en la siguiente tabla se indican algunos de los operadores acumuladores más comunes que puedes utilizar en el método `.aggregate` de MongoDB:

| **Operador** | **Descripción**  | **Ejemplo**   |
| --- | --- |  --- |
| `$sum`       | Suma de valores.                                     | `{$group: { _id: null, total: { $sum: "$cantidad" } }}`                      |
| `$avg`       | Promedio de valores.                                 | `{$group: { _id: "$categoria", promedio: { $avg: "$precio" } }}`             |
| `$min`       | Valor mínimo.                                        | `{$group: { _id: "$categoria", minimo: { $min: "$precio" } }}`               |
| `$max`       | Valor máximo.                                        | `{$group: { _id: "$categoria", maximo: { $max: "$precio" } }}`               |
| `$push`      | Añade valores a un array.                            | `{$group: { _id: "$categoria", items: { $push: "$nombre" } }}`               |
| `$addToSet`  | Añade valores únicos a un array.                     | `{$group: { _id: "$categoria", itemsUnicos: { $addToSet: "$nombre" } }}`     |
| `$first`     | Primer valor en el grupo.                            | `{$group: { _id: "$categoria", primerValor: { $first: "$nombre" } }}`        |
| `$last`      | Último valor en el grupo.                            | `{$group: { _id: "$categoria", ultimoValor: { $last: "$nombre" } }}`         |
| `$stdDevPop` | Desviación estándar de la población de los valores.  | `{$group: { _id: "$categoria", desviacion: { $stdDevPop: "$precio" } }}`     |
| `$stdDevSamp`| Desviación estándar de la muestra de los valores.    | `{$group: { _id: "$categoria", desviacion: { $stdDevSamp: "$precio" } }}`    |

Estos operadores son muy útiles para realizar cálculos y transformaciones en los datos dentro de un pipeline de agregación en MongoDB [MongoDB Aggregation Operators](https://www.mongodb.com/docs/manual/reference/operator/aggregation/)


Por ejemplo, mediante `$avg` podemos obtener el promedio de los valores de un campo numérico.

- ^^Calculamos la media de la puntuación por país^^

```js
db.movies.aggregate([
    { $group: {
        _id: { "Pais" : "$countries"},
        Cantidad: { $sum: 1},
        Puntuación: { $avg: "$imdb.rating"}
      }
    }
])
```

El operador `$sort` ordena los documentos recibidos por el campo, y el orden indicado por la expresión indicada al pipeline.

Si queremos ordenar el listado anterior por la cantidad de películas descendentemente (-1):

```js
db.movies.aggregate([
    { $group: {
        _id: { "Pais" : "$countries"},
        Cantidad: { $sum: 1},
        Puntuación: { $avg: "$imdb.rating"}
      }
    },
    { $sort: {Cantidad: -1}}
])
```

El operador `$match` se utiliza principalmente para filtrar los documentos que pasarán a la siguiente etapa del pipeline o a la salida final.

Por ejemplo, filtramos las películas que lanzadas a partir de 2015:

```js
db.movies.aggregate([
    { $match: { 
        released: {$gt: new Date('2015-01-01')}
      }
    },
    { $group: {
        _id: { "Pais" : "$countries"},
        Cantidad: { $sum: 1},
        Puntuación: { $avg: "$imdb.rating"}
      }
    },
    { $sort: {Cantidad: -1}}
])
```

Observar el concepto de *pipeline* con el siguiente ejemplo. Vamos a contabilizar las películas lanzadas desde 2015 por paises, pero solo si hay al menos 30 peliculas de cada pais. Para esto, añadimos tras el cálculo de la `Cantidad` un nuevo filtro:

```js
db.movies.aggregate([
    { $match: { 
        released: {$gt: new Date('2015-01-01')}
      }
    },
    { $group: {
        _id: { "Pais" : "$countries"},
        Cantidad: { $sum: 1},
        Puntuación: { $avg: "$imdb.rating"}
      }
    },
    { $match: { 
        Cantidad: { $gt: 30 }
      }
    },
    { $sort: {Cantidad: -1}}
])
```

<div align="center">
    <img src="../../images/MongoDB/MongoDB34.png" alt="MongoDB Aggregate" width="90%" />
</div>

Si queremos realizar una proyección sobre el conjunto de resultados y quedarnos con un subconjunto de los campos usaremos el operador `$project`. Como resultado obtendremos el mismo número de documentos, y en el orden indicado en la proyección.

Veamos el resultado del siguiente ejemplo:

<div align="center">
    <img src="../../images/MongoDB/MongoDB35.png" alt="MongoDB Aggregate" width="90%" />
</div>

Donde:

1. Ocultamos el campo `_id`
2. Pasamos un campo a mayúsculas
3. Agrupamos varios campos
4. Renombramos un campo
5. Realizamos operaciones matemáticas
6. Obtenemos un número determinado de documentos, aunque el método `$limit` no es un modificador de `$project`

Así pues, como hemos visto el abanico de posiblidades es muy extenso, aunque en nuestro curso no tenemos tiempo suficiente para profundizar en todas las posibilidades, nos quedamos aqui. 

Para más información tienes el [Manual de MongoDB: Aggregation Operations](https://www.mongodb.com/docs/manual/aggregation/#aggregation-operations)


<hr>


## Base de datos de ejemplo 


1. Bases de datos que ofrece directamente MongoDB:
   https://github.com/neelabalan/mongodb-sample-dataset/tree/main/




2. Datos de AirBnB de Valencia: [Inside AirBnB](https://insideairbnb.com/get-the-data/)

Tabla con enlaces a datos a fecha de 22/06/2024

| Country/City | File Name | Description | 
| --- | --- | --- |
| Valencia | [listings.csv.gz](https://data.insideairbnb.com/spain/vc/valencia/2024-06-22/data/listings.csv.gz) | Detailed Listings data | 
| Valencia | [calendar.csv.gz](https://data.insideairbnb.com/spain/vc/valencia/2024-06-22/data/calendar.csv.gz) | Detailed Calendar Data | 
| Valencia | [reviews.csv.gz](https://data.insideairbnb.com/spain/vc/valencia/2024-06-22/data/reviews.csv.gz) | Detailed Review Data | 
