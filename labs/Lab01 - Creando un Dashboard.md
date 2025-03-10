<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png">

# Laboratorio Hands-On 01 - Creando un Dashboard

Entrenamiento práctico en la plataforma Databricks con enfoque en las funcionalidades de Análisis Exploratorio y Paneles.
</br></br>

## Objetivos del Ejercicio

El objetivo de este laboratorio es montar un Panel, utilizando los datos de acciones de las Big Tech de la NASDAQ.
</br></br></br>


## Ejercicio 02.01 - Paso a Paso en la Creación del Panel

Entre en la opción "**Catalog**" en el menú lateral de Databricks, </br>
y filtre el catálogo "**academy**" y esquema "**genie_aibi**" </br>
y seleccione la tabla "**stock_bigtech**". </br>
Haga clic en el botón "**Open in a dashboard**".
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_01.png" width="800px">
</br></br></br>

Databricks generará una primera versión del panel</br>
y abrirá un cuadro para la creación del elemento gráfico del panel.</br>
La primera línea editable es un **Asistente GenAI**, </br>
donde podemos describir, en lenguaje natural, lo que queremos crear.</br>
Observe que debajo de la línea, ya existen algunas sugerencias dadas por el propio asistente.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_02.png" width="800px">
</br></br></br>

En la línea editable, a medida que vamos escribiendo, van apareciendo opciones basadas en el contexto de la tabla.</br>
Escriba la frase a continuación en la línea del asistente:
</br></br>
``` sql
 volumen de acciones por día y por empresa 
``` 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_03.png" width="600px">
</br></br></br>

Vamos a mejorar el diseño del panel y la disposición de los objetos.</br>
Arrastre el gráfico de "Volumen de Acciones" al centro del Panel.</br>
Deje que el ancho del gráfico ocupe la pantalla de lado a lado.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_04.png" width="800px">
</br>
</br>
</br>

## Ejercicio 02.02 - Segundo Elemento Gráfico

Vamos a crear un nuevo elemento gráfico.</br>
En el menú azul suspendido, que se encuentra en la parte inferior del panel, </br>
haga clic en el segundo botón (con el ícono de un gráfico).</br>
</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_botao.png" width="200px">

</br></br></br>


En la línea editable del asistente, escriba:</br>
</br>
``` sql
gráfico de líneas del valor de cierre por día y por empresa

``` 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_05.png" width="800px">
</br></br></br>

## Ejercicio 02.03 - Agregando un Filtro

Organice nuevamente el diseño del Panel, </br>
colocando el nuevo gráfico alineado con el gráfico de "Volúmenes".</br>
Luego haga clic en el menú azul suspendido en el ícono de FILTRO.</br>
Escoja el atributo (Field):  "**company**"
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_06.png" width="850px">
</br></br></br>


## Ejercicio 02.04 - Agregando una Imagen al Panel

En el cuadro de texto del título del panel, elimine el texto anterior, </br>
y sustitúyalo por el código (markdown) a continuación: </br>
</br>

``` sql
![image](https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_painel.png)
```

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_07.png" width="700px">
</br></br></br>

El Panel tendrá el siguiente aspecto.</br>
Haga el debido alineamiento del gráfico en el diseño.</br>
Cambie el nombre del Panel en la barra superior.</br>
Haga clic en el botón "**Publish**" para publicar el Panel.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_08.png" width="700px">
</br></br></br>


## (Opcional) Ejercicio 02.05 - Creando un Nuevo Contexto de Datos

Vamos a crear ahora un nuevo contexto de datos.</br>
Para ello, entre en la opción "**SQL Editor**" en el menú lateral de Databricks, </br>
seleccione el Catálogo y el Esquema en la barra superior del Editor de Consultas,</br>
y escriba el texto a continuación:</br>

``` 

Seleccione el nombre de la empresa, stock,
mínimo valor de cierre, máximo valor de cierre
y porcentaje de variación entre el mínimo y el máximo valor de cierre
de la tabla stock_bigtech
agrupando por empresa y stock

```
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_09.png" width="700px">
</br></br></br>

Agregue al resultado la línea de concatenación con el nombre de la acción (STOCK), </br>
con el ENLACE (URL) de una imagen. </br>

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM luis_assuncao.genie_aibi.stock_bigtech
GROUP BY company, stock;

```
</br>
Al ejecutar la consulta (botón RUN),</br>
el resultado esperado es el mostrado a continuación:</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10.png" width="700px">
</br></br></br>

## (Opcional) Ejercicio 02.06 - Agregando el Nuevo Contexto de Datos al Panel

Ahora vamos a agregar el nuevo contexto de datos (calculado en la Consulta);</br>
como otra fuente de datos en el Panel.</br>
Para ello, vuelva a la opción "**Dashboards**" en el menú lateral de Databricks,</br>
Escoja el nombre del Panel que se creó,</br>
y haga clic en el botón (combo box) de "**Published**" para "**Draft**".
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10b.png" width="700px">
</br></br></br>

Haga clic en la opción "**Data**".</br>
En el item "Crear otro Conjunto de Datos"</br>
haga clic en el botón "**+Create from SQL**".</br>

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_11.png" width="700px">
</br></br></br>

Haga el copia y pega del código SQL generado en el ejercicio anterior:

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM luis_assuncao.genie_aibi.stock_bigtech
GROUP BY company, stock;

```
Y luego haga clic en el botón "**RUN**"
</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_12.png" width="700px">
</br></br></br>

## (Opcional) Ejercicio 02.07 - Agregando un Nuevo Gráfico con el Nuevo Contexto de Datos

Haga clic en el menú azul suspendido en la posición inferior del panel, </br>
en el botón con el ícono de gráfico, </br>
y en la barra de configuración (lateral derecha del panel),</br>
escoja el nombre del conjunto de datos (que vino de la Consulta SQL). 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_13.png" width="700px">
</br></br></br>

Configure el tipo de Visualización para "Tabla"(Table).</br>
Marque la opción para incluir TODOS los campos en la tabla.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_14.png" width="700px">
</br></br></br>

En la configuración, haga clic en el campo (columna) con el nombre de "**image**".</br>
En la opción de "Display", configure como "image". </br>

</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_15.png" width="700px">
</br></br></br>

En la opción de "SIZE", coloque el valor de "25" en el campo "width".</br>
En la opción de "Default column width", coloque el valor de "150".
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_16.png" width="700px">
</br></br></br>

Como resultado esperado, tendremos la figura a continuación.</br>
Guarde (Publique) nuevamente el Panel.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_17.png" width="700px">
</br></br></br>

Espero que esto te sea útil. ¡Si necesitas algo más, no dudes en preguntar!