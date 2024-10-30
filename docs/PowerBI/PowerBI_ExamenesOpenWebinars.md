# Exámenes de OpenWebinars:

## 02. Curso de Power BI: Preparación de los datos


**¿Cuál de las siguientes afirmaciones es cierta?**
- Importando un Dataset como Datasource, te proporciona los mismos reports que otros existentes.
- Los datasets, son un conjunto de datos generados únicamente por PowerBI cuando se aplican optimizaciones de carga.
- El Dataset de nuestro PowerBI nunca podrá ser reutilizado para la creación de nuevos reports..
- Importando un Dataset obtienes los cálculos en DAX del dataset original. `<----`


**Power Query Editor nos permite...**
- Navegar, definir y realizar operaciones de transformación de datos en un origen de datos. `<----`
- Conectarnos a múltiples datasource como fuente de origen.
- Todas las respuestas son correctas.
- Generar Datasets en PBI Service.


**A la hora de importar datos con Excel, ¿es posible crear relaciones entre tablas de manera automática?**
- Sí, PowerBi, te genera por defecto las relaciones que encuentra en función del nombre de las columnas.  `<----`
- Sí, pero solamente crea relaciones tipo estrella.
- No, ya que hay que definir previamente las primary keys de las tablas.
- No, cuando utilizamos un datasource de Excel, esta función no está habilitada.


**¿Qué es Direct Query?**
- Es una técnica de Modelado de Datos, más eficiente que Import Query.
- Método de conectividad de datos, que mantiene los datos en la base de datos origen, y lanza queries para obtener los datos necesarios. `<----`
- Es un tipo de DataSource de los que dispone PowerBI.
- Acción disponible desde PowerBI Service, para conectarse a fuentes de datos de tipo bases de datos.

--- 

**Tratamos de cargar datos a través de una página web, pero el servicio web está caído, ¿cómo podemos añadir eficiencia en los tiempos de consulta?**
- Ninguna de las otras respuestas.
- Creando parámetros "What-If" en nuestra URL.
- Creando schedule refresh desde el Power Service.
- Añadiendo un "Comand Time out" en minutos, para establecer un tiempo de consulta máximo.  `<----`

--- 


**¿Puedo añadir una columna condicional dentro de PowerQuery Editor?**
- Sí, aunque solamente es posible si estamos importando un fichero Excel.
- Sí, aunque solamente es posible si estamos utilizando el método de Direct Query.
- No.
- Sí, dentro la pestaña "Add Column", encontramos la opción añadir columna condicional. `<----`

--- 


**¿Cuándo nos aparece el editor de consultas en PowerBI?**
- Cuando queremos editar consultas.
- Cuando cargamos nuevos datos.
- Todas las respuestas son correctas. `<----`
- Cuando queremos crear nuevas consultas.

---


¿Utilizando Direct Query necesito activar el schedule refresh desde PowerBI Service para actualizar los datos?
- Sí, pero es necesario hacerlo desde el Desktop.
- Sí, se podrá actualizar hasta 8 veces al día.
- No. `<----`
- Sí, solamente será necesario una vez al día.

---


¿Qué es el Dataset de nuestro PowerBI?
- El Dataset es un visual en PowerBI, que te permite visualizar datos en cualquier informe.
- El Dataset es un método de conectividad de datos más eficiente que el Import Query.
- Todas las respuestas son ciertas.
- Dataset es un conjunto de datos que se utilizan para crear visualizaciones e informes en PowerBI. `<----`

---


¿Cuál de las siguientes acciones no es posible realizar en PowerQuery Editor?
- Crear una nueva tabla e introducir manualmente los datos que desees.
- Ejecutar un script en Python. `<---- **Incorrecta**`
- Modificar el tipo de datos de cualquier columna.
- Modificar el método de conectividad de Import Query a Direct Query.

---


Si queremos conectarnos a una base de datos SQL, ¿cómo podemos hacerlo?:
- Mediante los modos de conectividad Direct Query e Import Query.
- Mediante modo de conectividad de Direct Query.
- Podemos hacer un mix mediante Direct Query e Import Query para optimizar recursos. `<---- **Incorrecta**`
- Mediante modo de conectividad de Import Query.

---


Cuando cargamos una tabla (mediante Import Query), ¿tenemos la opción de transformar los datos antes de cargarlos?
- Sí, aunque esta opción solo está disponible para las licencias de pago.
- No, primera hay que cargar la tabla en memoria, y luego podremos transformar los datos que queramos. 
- Todas las respuestas son incorrectas.
- No, es mejor modificar/transformar los datos previamente en origen, y luego importarlos directamente a PowerBI. `<----**Incorrecta**`

---


¿Qué es PowerBi DataFlow?
- Es un conjunto de entities que se crean y administran dentro de nuestros Workspace en PowerBI Service.
- Todas las respuestas anteriores son ciertas. `<----**Incorrecta**`
- PowerBI Dataflow nos permite reutilizar grandes flujos de datos en formato CDM.
- PowerBi DataFlow es un datasource disponible en PowerBI Desktop.


---


¿Cuál/es de las siguientes acciones está disponible en PowerQuery Editor?
- Podemos escoger las columnas que nos interesan mantener.
- Utilizar la primera fila del fichero/tabla como cabecera de nuestra tabla.
- Todas las respuestas son ciertas. `<----`
- Podemos eliminar las filas de la tabla que no nos interesen.

---


Para detectar el datatype de las columnas, de un fichero TXT, disponemos de las opciones...
- Podemos elegir no detectar el tipo de datos de las columnas.
- Utilizando todas las filas del fichero del documento. 
- Todas las respuestas son correctas. `<----`
- Basándose en las primeras 200 filas del documento.

---


¿Cómo funciona la extracción de datos mediante Direct Query?
- Deja los datos en la base de datos de origen, y ejecuta querys para cargar la información necesaria. `<----`
- Genera cubos virtuales (MOLAP), que son almacenados en Power BI Azure.
- PowerBI genera ficheros .JSON, donde los datos son codificados por temas de seguridad.
- Extrae la información de tu database y la introduce dentro del Power BI Service.

---


¿Para qué sirve Applied Steps dentro de PowerQuery Editor?
- Todas las otras respuestas son ciertas. `<----`
- Nos permite modificar el Origen (Source) de nuestra tabla.
- Nos permite visualizar en que order se han aplicado transformaciones en nuestra tabla.
- Nos permite eliminar uno o varios pasos aplicados en nuestra tabla.

---


Cuando nos conectamos a una base de datos SQL, las "Advanced Options" nos permiten...
- Utilizando el modo de conectividad Import query, además nos permite incluir relaciones entre columnas en nuestro relationship model.
- Crear una quick measure que se adapte a nuestra base de datos.
- Añadir un timeout, para establecer un límite de tiempo en respuesta para nuestras consultas. `<----`
- Los dataSource de bases de datos de SQL, no permiten utilizar las advanced options.


---


Cuando estamos importando datos en PowerBI, y los queremos transformar, ¿qué ventana emergente nos aparece?
- Power Query Editor. `<----`
- Transform Editor Query.
- Import Query Editor
- Direct Query Editor.

--- 


A la hora de importar datos con un TXT, ¿qué parámetros son necesarios para detectar las columnas del archivo?:
- Relationships entre columnas.
- Timeout de consulta.
- Ninguna de las otras respuestas.
- Delimitador del archivo. `<----`
