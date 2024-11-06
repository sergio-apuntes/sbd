---
title: 3. Diseño de modelado de bases de datos. schemaless
description: Debemos comprender el concepto de schemaless a la hora de trabajar con bases de datos noSQL documentales como mongoDB
---

## Concepto de ***schemaless***

El término ***schemaless*** (sin esquema) se refiere a la capacidad de una base de datos para manejar datos sin una estructura fija predefinida. En el contexto de *MongoDB*, esto significa que no estás obligado a definir un esquema estricto para tus datos antes de comenzar a almacenarlos. En lugar de eso, los datos se almacenan en documentos *BSON* (Binary *JSON*), y estos documentos pueden tener una estructura flexible y pueden variar de un documento a otro dentro de la misma colección.

A continuación, profundicemos en algunos aspectos clave del concepto de *schemaless* en *MongoDB*:

- **Flexibilidad de estructura**: Los documentos en *MongoDB* pueden contener diferentes campos y tipos de datos. No es necesario que todos los documentos en una colección tengan la misma estructura. Esto permite adaptarse fácilmente a cambios en los requisitos de la aplicación sin tener que modificar un esquema centralizado.

-  **Adición dinámica de campos**: En *MongoDB*, puedes agregar campos a un documento en cualquier momento sin afectar a otros documentos en la misma colección. Esto significa que puedes manejar datos evolutivos donde la estructura de los documentos puede cambiar con el tiempo.

-  **Consulta sin restricciones**: Dado que no hay un esquema fijo que imponga restricciones sobre la estructura de los datos, las consultas en *MongoDB* pueden ser más flexibles. Puedes realizar consultas sobre cualquier campo en cualquier documento, incluso si esos campos no están presentes en todos los documentos de la colección.

-  **Evita la migración de esquemas**: En las bases de datos tradicionales con esquemas fijos, los cambios en el esquema requieren migraciones de datos costosas. Con *MongoDB*, puedes evitar este problema ya que no hay un esquema centralizado que necesite ser modificado.

-  **Agilidad en el desarrollo**: La falta de un esquema fijo permite una mayor agilidad en el desarrollo de aplicaciones, ya que puedes iterar rápidamente y ajustar el modelo de datos según sea necesario sin tener que preocuparte por actualizar un esquema centralizado.

-  **Rendimiento**: La flexibilidad del modelo de datos *schemaless* puede traducirse en un mejor rendimiento en ciertos casos, ya que elimina la necesidad de realizar un join de datos dispersos en múltiples tablas, como suele ocurrir en bases de datos relacionales.

A pesar de las ventajas de un modelo de datos *schemaless*, es importante tener en cuenta que esto también puede presentar **desafíos**, especialmente en términos de **mantener la coherencia y la integridad de los datos**. Por lo tanto, es crucial diseñar cuidadosamente la base de datos y utilizar prácticas como la validación de datos y la indexación adecuada para garantizar un rendimiento óptimo y la integridad de los datos en aplicaciones *MongoDB*.

Observar la diferencia que tenemos en el diseño de la bases de datos relacionales con las NoSQL como lo vimos anteriormente

## Documentos embebidos

El concepto de embebido hace referencia a guardar una *‘cosa’* dentro de otra *‘cosa’*. En este caso, guardar un documento *JSON* dentro de
otro como valor de una de sus propiedades.

Por ejemplo, supongamos que tenemos una colección que guarda datos sobre cursos que se están impartiendo, en una base de datos relacional serían dos tablas, pero en *MongoDB* lo hacemos en una única colección:

```js
{
    id: 1,
    referencia: 'C0001',
    nombre: 'Curso de especialización de Inteligencia Artificial y Big Data',
    fechaInicio: new Date("2024-10-01"),
    activo: true,
    asignaturas: [
        {
            codAsig: 101,
            nombre: 'Sistemas de Big Data',
            horasSemana: 3,
            profesores: [
                {
                    codProf: 302,
                    nombre: 'Sergio Rey',
                    rol: 'Profesor'
                },
                {
                    codProf: 901,
                    nombre: 'Jorge Soro',
                    rol: 'Especialista'
                }
            ]
        },
        {
            codAsig: 102,
            nombre: 'Big Data Aplicado',
            horasSemana: 3,
            profesores: [
                {
                    codProf: 301,
                    nombre: 'Nacho Pachés',
                    rol: 'Profesor'
                },
                {
                    codProf: 901,
                    nombre: 'Jorge Soro',
                    rol: 'Especialista'
                }
            ]
        }
    ],
    alumnos: []
}
```


## Documentos referenciados

El concepto ***referenciado***  a diferencia de los documentos embebidos, en un documento *JSON* se guarda solo el valor de una o varias propiedades, en lugar del documento completo.

Normalmente, se guarda el valor de una propiedad que identifica unívocamente, al documento referenciado: por ejemplo en el caso anterior podríamos tener: 

```js
{
    id: 1,
    referencia: 'C0001',
    nombre: 'Curso de especialización de Inteligencia Artificial y Big Data',
    fechaInicio: new Date("2023-10-01"),
    activo: true,
    asignaturas: [ 101, 102],
    alumnos: []
}
```

En este caso, las asignaturas se referéncian mediante el código de identificación, en lugar de guardar en el *JSON* todos los datos. De esta forma es similar al adoptado por las bases de datos relacionales.


Esto plantea varias ventajas y desventajas:

Así pues en los **documentos embebidos** tenemos:
- Ventajas
  - Al recuperar un curso, podemos traernos toda la información relacionada en otras colecciones (p.e. asignaturas)
- Desventajas:
  - Al hacer consultas sobre la colección externa (cursos), si uno de los criterios afecta a la información de los documentos embebidos, el tiempo para realizar dicha consulta se verá incrementando.
  - Las actualizaciones serán más costosas si alguna de las propiedades a actualizar pertenece al documento embebido.

Mientras que en los **documentos referenciados**
- Ventajas
  - Las consultas sobre la colección principal y las relacionadas se ejecutará más rápido.
  - Al actualizar información relacionada, se hace directamente en sus documentos sin tener que revisar la colección externa.
- Desventajas:
  - Al recuperar un curso, habrá que consultar los identificadores que relacionan a los documentos en otras colecciones y hacer consultas adicionales para obtener la información relacionada.


Entonces ***¿Qué diseño elegir?***

Dependerá de:
- Cómo se quiere almacenar la información.
- la naturaleza y el contexto de las aplicaciones que vayan a consumir la información.
- Las preferencias de roles como arquitectos de software y de bases de datos teniendo en cuenta factores futuros como la Escalabilidad en cuanto a volumen de datos, usuarios / aplicaciones y sus formas de acceder a la información, etc.


