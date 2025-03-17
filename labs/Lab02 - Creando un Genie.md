<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png">

# Laboratorio Hands-On 02 - Creando un Genie

Entrenamiento práctico en la plataforma Databricks con enfoque en las funcionalidades de preguntas y respuestas utilizando lenguaje natural.

</br></br>

## Objetivos del Ejercicio

El objetivo de este laboratorio es utilizar el AI/BI Genie para permitir el análisis de datos de ventas utilizando solo **Español**.

</br></br>

## Ejercicio 03.00 - Preparación

1. En algunos momentos, utilizaremos el SQL Editor. Déjelo preparado en una otra ventana y seleccione su base de datos.

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_04.png">

</br></br>

## Ejercicio 03.01 - Crear AI/BI Genie

Vamos a comenzar creando una Genie para hacer nuestras preguntas. Para ello, vamos a seguir los pasos a continuación:

1. En el menú principal (a la izquierda), haga clic en `New` > `Genie space`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_01.png"><br><br>

2. Configure su Genie
    - Cree un nombre para su Genie, por ejemplo `<sus iniciales> Genie de Ventas`
    - Seleccione su SQL Warehouse
    - Seleccione las siguientes tablas:
        - ventas
        - inventario
        - dim_medicamento
        - dim_loja
    - Haga clic en `Save`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_02.png" width=800>

</br></br>

## Ejercicio 03.02 - Hacer preguntas al AI/BI Genie

Con nuestra Genie preparada, podemos comenzar a hacer nuestros análisis!

Basta utilizar el chat para hacer las preguntas a continuación:

- ¿Cuál es la facturación en oct/22?
- Ahora, desglose por producto
- Mantenga solo los 10 productos con mayor facturación
- Monte un gráfico de barras
- ¿Cuál es el total de productos vendidos en genéricos?
- ¿Cuál es el valor total vendido de ansiolíticos?
- ¿Qué productos tuvieron una proporción de ventas por inventario mayor que 0.8 en octubre de 2022?

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_05.png"><br><br>

Noten que, incluso con muy poco contexto, la Genie ya pudo:
- Inferir qué tablas y columnas son relevantes para responder nuestras preguntas
- Aplicar filtros y agregaciones
- Responder preguntas adicionales sobre una respuesta anterior
- Entender cómo utilizar jergas
- Combinar diferentes tablas
- Calcular métricas derivadas

Aprovechen para explorar y hacer preguntas adicionales!

</br></br>

## Ejercicio 03.03 - Utilizando comentarios

Aun así, pueden ocurrir escenarios en los que debamos proporcionar algún contexto adicional a la Genie para obtener respuestas más precisas.

Ahora, vamos a explorar algunas formas de auxiliar a la Genie cuando identifiquemos alguna necesidad.

La primera de ellas es documentar nuestras tablas. Todos los comentarios que agreguemos a las tablas son utilizados por la Genie para entender mejor qué es ese dato.

Vamos a ver cómo funciona:

1. Haga la siguiente pregunta:
    - ¿Cuál es el valor total de venta por loja? Muestre el nombre de la loja

2. Ops, parece que la Genie no pudo descubrir qué columna contiene el nombre de la loja. Utilice el Editor de SQL para agregar un comentario en la columna **nlj** de la tabla **dim_loja** y explique que ella contiene esa información
    - `ALTER TABLE dim_loja ALTER COLUMN nlj COMMENT 'Nombre de la loja'`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_06.png"><br><br>

3. Haga nuevamente la pregunta anterior<br><br>

Noten que esta vez la Genie utilizó la columna correcta que contiene el nombre de la loja.

Documentar nuestras tablas con comentarios es siempre una buena práctica! Esto ayuda a la comprensión, el descubrimiento y el reaprovechamiento de esos datos por otras personas. Además, termina de ayudar a mejorar las respuestas de la Genie.

Sin embargo, nuestra consulta aún no retornó ningún resultado. Vamos a buscar una solución!

</br></br>

## Ejercicio 03.04 - Utilizando claves primarias

Aparentemente, la columna **id_loja** de la tabla **dim_loja** no es el mejor campo para hacer los cruces con la tabla de ventas. En realidad, la columna correcta es la **cod**!

Vamos a agregar claves primarias y extranjeras en esas tablas para que la Genie no precise inferir cómo hacer ese cruce!

1. Utilice el Editor de SQL para agregar las claves primarias y extranjeras en las tablas `dim_loja` y `vendas`

``` sql
ALTER TABLE dim_loja ALTER COLUMN cod SET NOT NULL;
ALTER TABLE dim_loja ADD CONSTRAINT pk_dim_loja PRIMARY KEY (cod);
ALTER TABLE vendas ADD CONSTRAINT fk_venda_dim_loja FOREIGN KEY (id_loja) REFERENCES dim_loja(cod);
```
2. Haga nuevamente la pregunta anterior en la Genie

¡Listo! Con esa información, la Genie ya puede responder nuestra pregunta correctamente!

</br></br>

## Ejercicio 03.05 - Utilizando instrucciones

Como vimos, la Genie utiliza toda la documentación de nuestras tablas para poder responder nuestras preguntas. Sin embargo, por motivos de seguridad, ella no tiene acceso a los datos en sí!

Por eso, para complementar el conocimiento que la Genie ya posee sobre nuestras bases de datos, podemos también crear instrucciones!

Las **Instrucciones** permiten guardarnos y parametrizar lógicas complejas dentro de nuestro catálogo para ser reutilizadas por otras personas y/o otras consultas de forma simple – inclusive fuera de la Genie. 

En nuestro contexto, las instrucciones también van a funcionar como herramientas validadas y certificadas por los equipos responsables que la Genie puede decidir utilizar en sus respuestas.

Vamos a ver en la práctica:

1. Haga la pregunta:
    - Calcule la cantidad de artículos vendidos para prescripción

2. Realmente, no tenemos información suficiente en nuestra base para responder a esa pregunta! Para eso, agregue la siguiente instrucción:
    - `* para calcular indicadores sobre prescripción use categoria_regulatoria <> 'GENÉRICO'`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_07.png">

3. Haga nuevamente la pregunta anterior

¡Listo! Ahora la Genie ya puede responder preguntas sobre prescripciones también!

</br></br>

## (Opcional) Ejercicio 03.06 - Utilizando ejemplos de consultas

En algunos casos, debemos hacer cruces y cálculos bastante complejos para poder responder a nuestras preguntas y la Genie puede no entender cómo montar todo el razonamiento necesario.

En esos casos, podemos proporcionar ejemplos de consultas validadas y certificadas por los equipos responsables. Este también es un mecanismo interesante para garantizar la precisión de las respuestas.

Vamos a ver cómo funciona:

1. Haga la pregunta:
    - Calcule la cantidad de artículos vendidos por ventana móvil de 3 meses 

2. Aquí la Genie ya hizo una suma en ventana móvil, pero no quedó exactamente como queríamos. Entonces, agregue un ejemplo de consulta siguiendo los pasos a continuación:
    - Haga clic en `Instructions`, en el menú a la izquierda
    - Haga clic en `Add Example Query`
    - En el campo superior, ingresa la pregunta anterior
    - En el campo inferior, ingresa la consulta SQL
        - `SELECT window.end AS dt_venda, SUM(vl_venda) FROM vendas GROUP BY WINDOW(dt_venda, '90 days', '1 day')`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_08.png">

3. Haga nuevamente la pregunta anterior

Noten que ahora la Genie pudo responder correctamente a nuestra pregunta!

</br></br>

## (Opcional) Ejercicio 03.07 - Utilizando funciones

Otro recurso que podemos utilizar para ayudar a la Genie con cálculos complejos son las funciones!

Las **Funciones** permiten guardarnos y parametrizar lógicas complejas dentro de nuestro catálogo para ser reutilizadas por otras personas y/o otras consultas de forma simple – inclusive fuera de la Genie. 

En nuestro contexto, las funciones también van a funcionar como herramientas validadas y certificadas por los equipos responsables que la Genie puede decidir utilizar en sus respuestas.

Vamos a ver en la práctica:

1. Haga la pregunta:
    - ¿Cuál es el lucro proyectado del AAS?

2. Realmente, no tenemos información suficiente en nuestra base para responder a esa pregunta! Para eso, cree la función a continuación con la lógica del cálculo del lucro medio proyectado de un producto:

``` sql
CREATE OR REPLACE FUNCTION calc_lucro(medicamento STRING)
  RETURNS TABLE(nombre_medicamento STRING, lucro_proyectado DOUBLE)
  COMMENT 'Use esta función para calcular el lucro proyectado de un medicamento'
  RETURN 
    SELECT
      m.nombre_medicamento,
      sum(case when m.categoria_regulatoria == 'GENÉRICO' then 1 else 0.5 end * v.vl_venda) / sum(v.qt_venda) as lucro_proyectado
    FROM vendas v
    LEFT JOIN dim_medicamento m
    ON v.id_produto = m.id_produto
    WHERE m.nombre_medicamento = calc_lucro.medicamento
    GROUP BY ALL
```

3. Agregue esta función a su Genie

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_09.png">

4. Haga nuevamente la pregunta anterior

¡Listo! Con eso, conseguimos calcular el lucro medio de nuestro producto!

<br><br>

# ¡Felicitaciones!

Ha completado el laboratorio del **AI/BI Genie**!

Ahora, ya sabe cómo utilizar la Genie para analizar sus datos utilizando solo lenguaje natural – además de los principales recursos para refinar su precisión y poder responder a las preguntas más complejas!