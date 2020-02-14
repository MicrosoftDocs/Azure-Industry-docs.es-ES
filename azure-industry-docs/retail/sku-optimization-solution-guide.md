---
title: Optimización de SKU para marcas de consumidor con Azure Machine Learning y análisis
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Optimización del surtido del sector minorista. Optimización de SKU a través de la información de IA y ML.
ms.openlocfilehash: 22411776e830bb3c71f8c1277b30ec4331a3ef17
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054378"
---
# <a name="sku-optimization-for-consumer-brands-solution-guide"></a>Guía de soluciones de la optimización de SKU para las marcas de consumidor

## <a name="introduction"></a>Introducción

Los minoristas y las marcas de consumidor se centran en garantizar que tienen los productos y servicios adecuados que los consumidores buscan para comprar en marketplace. También es justo decir que, cuando se trata de maximizar las ventas, los productos (o combinaciones de productos) son la parte principal de la experiencia de compra. La disponibilidad de ofertas (inventario) constituye un problema constante para las marcas de consumidor.

El inventario de productos, también conocido como surtido de SKU, es un tema complejo que abarca la cadena de valor de suministro y logística. En este documento, nos centraremos específicamente en el problema de optimizar el surtido de SKU para maximizar los ingresos desde un punto de vista de los bienes de consumo. Puede resolver el rompecabezas de la optimización del surtido de SKU desarrollando algoritmos para responder a las preguntas siguientes:

- ¿Qué SKU rinden mejor en una tienda o mercado determinado? 
- ¿Qué SKU se deben asignar a un determinado mercado o tienda en función de su rendimiento?
- ¿Qué SKU tienen un rendimiento bajo y deben reemplazarse por SKU de mayor rendimiento?
- ¿Qué otra información podemos derivar sobre nuestros segmentos de mercado y consumidores?

## <a name="automate-decision-making"></a>Automatización de la toma de decisiones

Tradicionalmente, las marcas de consumidor abordaban la demanda de los consumidores aumentando el número de SKU en la cartera de SKU. Dado que el número de SKU ha crecido y la competencia ha aumentado, se estima que el 90 por ciento de los ingresos se atribuye a solo el 10 por ciento de SKU del producto dentro de la cartera. Normalmente, el 80 por ciento de los ingresos se acumula del 20 por ciento de SKU. Y esta relación es un candidato para mejorar la rentabilidad. 

Los métodos tradicionales de informes estáticos aprovechan los datos históricos, lo cual limita la información. En el mejor de los casos, las decisiones aún se toman e implementan manualmente. Esto se traduce en intervención humana y tiempo de procesamiento. Con los avances de la inteligencia artificial (IA) y la informática en la nube, es posible utilizar análisis avanzado para proporcionar diversas opciones y predicciones. Este tipo de automatización mejora los resultados y la comercialización.

## <a name="sku-assortment-optimization"></a>Optimización del surtido de SKU

Una solución de surtido de SKU debe controlar millones de SKU mediante la segmentación de datos de ventas en comparaciones significativas y detalladas. El objetivo de la solución consiste en maximizar las ventas en cada punto de venta o tienda ajustando el surtido de productos mediante análisis avanzados. Un segundo objetivo es eliminar el desabastecimiento y mejorar los surtidos. El objetivo fiscal es un aumento del 5 al 10 por ciento de las ventas. Con tal objetivo, las conclusiones permiten:

- Comprender el rendimiento de la cartera de SKU y administrar las de bajo rendimiento.
- Optimizar la distribución de SKU para reducir el desabastecimiento.
- Comprender cómo las nuevas SKU admiten las estrategias a corto y largo plazo.
- Crear conclusiones repetibles, escalables y procesables a partir de los datos existentes.

## <a name="descriptive-analytics"></a>Análisis descriptivo

Los modelos descriptivos agregan puntos de datos y exploran las relaciones entre los factores que pueden influir en las ventas de productos. La información se puede aumentar con algunos puntos de datos externos, como la ubicación, el tiempo, los datos del censo, etc. Las visualizaciones ayudan a una persona a obtener información mediante la interpretación de los datos. Sin embargo, al hacerlo, la persona se limita a comprender qué ha ocurrido durante el ciclo de ventas anterior o, posiblemente, lo que sucede en el período actual (según la frecuencia con la que se actualicen los datos).

En este caso, un enfoque de informes y almacenamiento de datos tradicional es suficiente para comprender, por ejemplo, qué SKU se han comportado mejor y peor durante un período de tiempo.

En la siguiente ilustración se muestra un informe típico de datos históricos (datos de ventas). Ofrece varios bloques con casillas para seleccionar los criterios para filtrar los resultados. En el centro aparecen dos gráficos de barras que muestran las ventas a lo largo del tiempo. El primer gráfico muestra el promedio de ventas por semana y el segundo las cantidades por semana.

 ![Ejemplo de panel que muestra datos históricos de ventas.](assets/sku-optimization-solution-guide/sku-max-model.png)
  
## <a name="predictive-analytics"></a>Análisis predictivo

Los informes históricos son útiles para comprender lo que ha sucedido. Por último, queremos una previsión de lo que es probable que ocurra. La información pasada puede ser útil para ese propósito. Por ejemplo, podemos identificar tendencias de temporada. Pero no puede abarcar escenarios “hipotéticos”, por ejemplo, para modelar la introducción de un producto nuevo. Para hacer eso, debemos cambiar nuestro enfoque hacia el modelado del comportamiento del cliente, ya que ese es el factor final que determina las ventas.

### <a name="an-in-depth-look-at-the-problem-choice-models"></a>Una mirada en profundidad al problema: modelos de elección

Vamos a empezar definiendo lo que estamos buscando y los datos que tenemos:

La optimización del surtido significa encontrar un subconjunto de los productos que se van a poner a la venta que maximice los ingresos esperados. Esto es lo que estamos buscando.

Los **datos de transacción** se recopilan de forma rutinaria con fines financieros. 

Los **datos de surtido** incluyen potencialmente todo lo relacionado con las SKU. Este es un ejemplo de lo que queremos: 

- El número de SKU
- Descripciones de las SKU
- Cantidades asignadas
- SKU y cantidad comprada
- Marcas de tiempo de los eventos (por ejemplo, la compra)
- Precio de la SKU
- Precio de la SKU en los puntos de venta
- Nivel de existencias de cada SKU en cualquier momento dado

Lamentablemente, tales datos no se recopilan tan confiablemente como los datos de transacción. 

En este documento, por motivos de simplicidad, solo consideraremos datos de transacciones y datos de SKU, no factores externos.

Aun así, tenga en cuenta que, dado un conjunto de n productos, hay 2n posibles surtidos. Esto hace que el problema de optimización sea un proceso muy intenso desde el punto de vista computacional. Evaluar todas las combinaciones posibles no es práctico con una gran cantidad de productos. Por lo general, los surtidos se segmentan por categoría (por ejemplo, cereales), ubicación y otros criterios para reducir el número de variables. Los modelos de optimización intentan “recortar” el número de permutaciones a un subconjunto manejable.

El quid del problema se basa en **modelar el comportamiento de los consumidores** eficazmente. En un mundo perfecto, los productos que se les presentan coincidirán con los que desean comprar.

A lo largo de décadas se han desarrollado modelos matemáticos para predecir la elección de los consumidores. La elección del modelo determinará en última instancia la tecnología de implementación más adecuada; por lo tanto, los resumiremos y ofreceremos algunas consideraciones.

## <a name="parametric-models"></a>Modelos paramétricos

Los modelos paramétricos aproximación el comportamiento de los clientes mediante el uso de una función con un conjunto finito de parámetros. Estimamos el conjunto de parámetros para ajustar mejor los datos a nuestra disposición. Uno de los más antiguos y conocidos es la [regresión logística multinomial](https://en.wikipedia.org/wiki/Multinomial_logistic_regression) (también conocida como MNL, función logit de varias clases o regresión softmax). Se utiliza para calcular las probabilidades de varios resultados posibles en los problemas de clasificación. En este caso, puede usar MNL para calcular:

- La probabilidad de que un consumidor (c) elija un elemento (i) en un momento determinado (t), dado un conjunto de elementos de esa categoría en un surtido (a) con una utilidad conocida para el cliente (v). 

También se supone que la utilidad de un elemento puede ser una función de sus características. También se puede incluir información externa en la medida de la utilidad (por ejemplo, un paraguas es más útil cuando llueve).

A menudo utilizamos MNL como punto de referencia para otros modelos debido a su manejabilidad en la estimación de parámetros y la evaluación de resultados. En otras palabras, si lo hace peor que MNL, su algoritmo no sirve de nada.
Se han derivado varios modelos de MNL, pero queda fuera del ámbito de este documento hablar sobre ellos. 

Existen bibliotecas para los lenguajes de programación R y Python. Para R, puede usar glm (y derivados).  Para Python, existen [scikit-aprender](http://scikit-learn.org/stable/), [biogeme](http://biogeme.epfl.ch/) y [larch](https://pypi.org/project/larch/). Estas bibliotecas ofrecen herramientas para especificar problemas de MNL y solucionadores paralelos para encontrar soluciones en diversas plataformas.

Más recientemente, se ha propuesto la implementación de modelos de MNL en GPU para calcular modelos complejos con una serie de parámetros que, de lo contrario, los harían intratables.

Se han utilizado de forma eficaz redes neuronales con una capa de salida softmax en grandes problemas de varias clases. Estas redes generan un vector de salidas que representan una distribución de probabilidad a través de una serie de resultados diferentes. Resultan lentos de entrenar en comparación con otras implementaciones, pero pueden controlar un gran número de clases y parámetros. 

## <a name="non-parametric-models"></a>Modelos no paramétricos

A pesar de su popularidad, MNL coloca algunas suposiciones importantes en el comportamiento humano que pueden limitar su utilidad. En concreto, se supone que la probabilidad relativa de que alguien elija entre dos opciones es independiente de alternativas adicionales introducidas en el conjunto más adelante. Eso es poco práctico en la mayoría de los casos.

Por ejemplo, si le gustan los productos “A” y “B” de igual manera, se elegirá uno sobre el otro el 50 % del tiempo. Vamos a introducir el producto “C” a la combinación. Aún puede elegir “A” el 50 % del tiempo, pero ahora divide su preferencia en un 25 % para “B” y otro 25 % para “C”. La probabilidad relativa ha cambiado.

Además, MNL y los derivados no tienen una forma sencilla de explicar la sustitución debido a una variedad de surtidos o desabastecimiento (es decir, cuando no tiene una idea clara y elige un artículo al azar entre los que están en el estante).
Los modelos no paramétricos están diseñados para explicar la sustitución e imponer menos restricciones en el comportamiento de los clientes. 

Introducen el concepto de “clasificación”, donde los consumidores expresan una preferencia estricta por los productos de un surtido. Por lo tanto, su comportamiento de compra se puede modelar clasificando los productos en orden descendente de preferencia. 

El problema de optimización del surtido se puede expresar como maximización de ingresos:

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/assortment-optimization-problem.png" width="150"/></center>
 
- $r_i$ denota los ingresos del producto i 
- $y_i^k$ es 1 si se elige el producto i en la clasificación k; en caso contrario, es 0  
- $\lambda_k$ es la probabilidad de que el cliente realice una elección según la clasificación k
- $x_i$ es 1 si el producto está incluido en el surtido; en caso contrario, es 0
- K es el número de clasificaciones 
- n es el número de productos

sujeto a restricciones:

- puede haber exactamente una elección para cada clasificación
- en una clasificación k, solo se puede elegir un producto i si forma parte del surtido
- si un producto i se incluye en el surtido, no se puede elegir ninguna de las opciones menos preferibles en la clasificación k 
- no comprar es una opción y, como tal, se puede no elegir ninguna de las opciones menos preferibles en una clasificación

En una formulación de este tipo, el problema puede considerarse como una [optimización de enteros mixtos](https://en.wikipedia.org/wiki/Integer_programming).

Consideremos que, si hay n productos, el número máximo de clasificaciones posibles, incluida la opción de no elección, es factorial: (n + 1)!.

Aunque las restricciones en la formulación permiten una eliminación de las posibles opciones relativamente eficaz (por ejemplo, solo la opción preferida se elige y se establece en 1, el resto se establecen en 0), puede imaginar que la escalabilidad de la implementación será importante, dado el número de las alternativas posibles.

### <a name="the-importance-of-data"></a>La importancia de los datos

Hemos mencionado antes que los datos de ventas están fácilmente disponibles. Queremos que se usen para informar a nuestro modelo de optimización del surtido. En particular, queremos encontrar nuestra distribución de probabilidad λ.

Los datos de ventas obtenidos del sistema de puntos de ventas se componen de transacciones con marcas de tiempo y de un conjunto de productos que se muestran a los clientes en ese momento y ubicación. A partir de ellos, podemos crear un vector de ventas reales, cuyos elementos vi,m representan la probabilidad de vender el artículo i a un cliente dado un surtido $S_m$.

También podemos construir una matriz:

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/matrix-construction.png" width="300"/></center>

La búsqueda de nuestra distribución de probabilidad λ dados nuestros datos de ventas se convierte en otro problema de optimización. Queremos encontrar un vector λ para minimizar nuestros errores de estimación de ventas:

$$min_\lambda|\Lambda\lambda - v|$$

Tenga en cuenta que el cálculo también se puede expresar como una regresión y, por lo tanto, se pueden usar modelos como árboles de decisión multivariantes. 

## <a name="implementation-details"></a>Detalles de la implementación

Como podemos deducir de la formulación anterior, los modelos de optimización se basan en datos y hacen un uso intensivo de cálculo.

Asociados de Microsoft, como Neal Analytics, han desarrollado arquitecturas sólidas para satisfacer esas condiciones. Consulte [SKU Max](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview?WT.mc_id=invopt-article-gmarchet). Utilizaremos esas arquitecturas como ejemplo y ofreceremos algunas consideraciones.

- En primer lugar, se basan en (1) una canalización de datos sólida y escalable para alimentar los modelos, etc. y (2) una infraestructura de ejecución sólida y escalable para ejecutarlos.
- En segundo lugar, los planificadores pueden consumir fácilmente los resultados a través de un panel.

En la figura 2 se muestra una arquitectura de ejemplo. Incluye cuatro bloques principales: Capturar, Procesar, Modelar y Poner en marcha. Cada bloque contiene procesos importantes. Capturar incluye “preprocesamiento de datos”; Procesar incluye la función “datos de la tienda”; Modelar incluye la función “entrenar modelo de Machine Learning”; y Poner en marcha incluye “datos de la tienda” y opciones de informe (por ejemplo, paneles).

![Arquitectura de cuatro partes: capturar, procesar, modelar y poner en marca.](assets/sku-optimization-solution-guide/architecture-sku-optimization.png)<center><font size="1">_Figura 2: Arquitectura para la optimización de SKU; por cortesía de Neal Analytics_</font></center>

## <a name="the-data-pipeline"></a>La canalización de datos

La arquitectura destaca la importancia de establecer una canalización de datos para el entrenamiento y las operaciones del modelo. Organizamos las actividades en la canalización utilizando [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet), un servicio administrado de extracción, transformación y carga (ETL) de datos que le permite diseñar y ejecutar los flujos de trabajo de integración.

Azure Data Factory es un servicio administrado con componentes denominados “actividades” que consumen y/o generan conjuntos de datos.

Las actividades se pueden dividir en:

- Movimiento de datos (por ejemplo, copiar desde el origen al destino)
- Transformación de datos (por ejemplo, agregación con una consulta SQL o ejecución de un procedimiento almacenado)

El servicio de factoría de datos puede programar, supervisar y administrar los flujos de trabajo que vinculan conjuntos de actividades. El flujo de trabajo completo se conoce como una “canalización”.

En la fase de captura, podemos aprovechar la actividad de copia (integrada en Data Factory) para transferir datos desde numerosos orígenes (tanto locales como en la nube) a Azure SQL Data Warehouse. En la documentación se proporcionan ejemplos de cómo hacerlo:

- [Copia de datos con Azure SQL Data Warehouse como origen o destino mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)
- [Carga de datos en Azure SQL Data Warehouse mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)

La siguiente figura muestra la definición de una canalización. Consta de tres bloques de igual tamaño seguidos. Los dos primeros son un conjunto de datos y una actividad conectados mediante flechas para indicar los flujos de datos. El tercer bloque tiene la etiqueta “canalización” y simplemente apunta a los dos primeros para indicar la encapsulación. 

 ![Conceptos de Azure Data Factory: conjuntos de datos consumidos por la canalización de actividades.](assets/sku-optimization-solution-guide/azure-data-factory.png)<center><font size="1">_Figura 3: Conceptos básicos de Azure Data Factory_</font></center>

Un ejemplo del formato de datos que usa la solución de Neal Analytics puede encontrarse en la página de Appsource de Microsoft. La solución incluye los conjuntos de datos siguientes:

- Datos históricos de ventas para cada combinación de tienda y SKU
- Registros de tienda y consumidor
- Códigos y descripción de SKU
- Atributos de SKU que capturan las características de los productos (por ejemplo, el tamaño y el material). Se suelen utilizar en modelos paramétricos para diferenciar entre las variantes de producto.

Si los orígenes de datos no se expresan en el formato determinado, Data Factory ofrece una serie de actividades de transformación. 

En la fase de proceso, SQL Data Warehouse es el motor de almacenamiento principal. Por lo tanto, puede que le interese expresar una actividad de transformación como un procedimiento almacenado de SQL, que se puede invocar automáticamente como parte de la canalización. La documentación proporciona instrucciones detalladas:

- [Transformación de datos mediante la actividad de procedimiento almacenado de SQL Server en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-stored-procedure?WT.mc_id=invopt-article-gmarchet)

Tenga en cuenta que Data Factory no le limita a SQL Data Warehouse y procedimientos almacenados de SQL. De hecho, se integra con diversas plataformas. Por ejemplo, puede optar por usar Databricks y ejecutar un script de Python para la transformación. Esto es una ventaja, ya que puede usar una plataforma para almacenamiento, transformación y entrenamiento de algoritmos de aprendizaje automático en la siguiente fase del “modelo”.

## <a name="training-the-ml-algorithm"></a>Entrenamiento del algoritmo de ML

Hay varias herramientas que le ayudarán a implementar los modelos no paramétricos y paramétricos. La elección depende de los requisitos de escalabilidad y rendimiento.

[Azure ML Studio](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet) es una excelente herramienta para la creación de prototipos. Proporciona una manera fácil de crear y ejecutar un flujo de trabajo de entrenamiento con los módulos de código (en R o Python), o con componentes de ML predefinidos (por ejemplo, clasificadores multiclase y regresión de árbol de decisión incrementado) en un entorno gráfico. También permite publicar un modelo entrenado como un servicio web para el consumo adicional con unos pocos clics, generando una interfaz REST automáticamente. 

Sin embargo, el tamaño de los datos que puede controlar se limita a 10 GB (por ahora) y el número de núcleos disponibles para cada componente también se limita a 2 (por ahora).

La siguiente figura muestra un ejemplo del estudio de ML en uso. Es una representación gráfica de un experimento de ML. La figura muestra varios grupos de bloques. Cada conjunto de bloques representa alguna fase del experimento y cada bloque está conectado a uno o más bloques para indicar la entrada y salida de datos.

![Ejemplo del estudio de aprendizaje automático en uso.](assets/sku-optimization-solution-guide/ml-training-pipeline-example.png)<center><font size="1">_Figura 4: Ejemplo de la canalización de entrenamiento de ML con R y componentes creados previamente_</font></center>

Si necesita escalar más lejos pero todavía desea usar alguna implementación rápida y paralela de Microsoft del algoritmo de aprendizaje automático común (por ejemplo, la regresión logística multinomial), es posible que desee examinar Microsoft ML Server que se ejecuta en Azure Data Science Virtual Machine.

Para tamaños de datos muy grandes (TB), tiene sentido elegir una plataforma donde el almacenamiento y el elemento de cálculo pueden:

- Escalar por separado, para limitar los costos cuando no se entrenan los modelos.
- Distribuir el cálculo entre varios núcleos.
- Ejecutar el cálculo “cerca” del almacenamiento para limitar el movimiento de datos.

Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet) y [Databricks](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet) cumplen esos requisitos. Además, ambas plataformas de ejecución son compatibles con el editor de Azure Data Factory. Es relativamente sencillo integrar cualquiera de ellas en un flujo de trabajo.

ML Server y sus bibliotecas se pueden implementar sobre HDInsight, pero para aprovechar al máximo las funcionalidades de la plataforma, es interesante implementar el algoritmo de ML que elija mediante SparkML, las bibliotecas de Microsoft ML Spark en Python u otro solucionador de programación lineal especialista, como TFoCS, Spark-LP o SolveDF. 

Iniciar el proceso de entrenamiento se convierte en una cuestión de invocar el cuaderno o el script de pySpark adecuado desde un flujo de trabajo de Data Factory. Esto se admite completamente en el editor gráfico. Para más detalles, consulte [Ejecución de un cuaderno de Databricks con la actividad Notebook de Databricks en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=invopt-article-gmarchet).

En la siguiente figura se muestra la interfaz de usuario de Data Factory, a la que se accede a través de Azure Portal. Incluye bloques para los distintos procesos del flujo de trabajo. 

![Interfaz de Data Factory que muestra actividad de cuaderno de Databricks.](assets/sku-optimization-solution-guide/data-factory-pipeline-databricks.png)<center><font size="1">_Figura 5: Ejemplo de canalización de Data Factory con actividad de cuaderno de Databricks_</font></center>.

Asimismo, tenga en cuenta que en nuestra [solución de optimización de inventario](https://gallery.azure.ai/Solution/Inventory-Optimization-3?WT.mc_id=invopt-article-gmarchet) proponemos una implementación basada en contenedor de los solucionadores que se escala a través de [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet). Las bibliotecas de optimización especialistas, como [pyomo](http://www.pyomo.org/about/), permiten expresar un problema de optimización en el lenguaje de programación Python y, después, invocar solucionadores independientes, como [bonmin](https://projects.coin-or.org/Bonmin) (código abierto) o [gurobi](http://www.gurobi.com/) (comercial) para encontrar una solución.

La documentación de optimización de inventario se ocupa de un problema diferente (cantidades de pedidos) de la optimización del surtido, aunque la implementación de solucionadores en Azure es aplicable de forma similar.

Aunque es más compleja que las sugeridas hasta ahora, esta técnica permite una escalabilidad máxima, limitada principalmente por la cantidad de núcleos que puede permitirse.

## <a name="running-the-model-operationalize"></a>Ejecución del modelo (puesta en marcha)

Una vez que se ha entrenado el modelo, su ejecución normalmente requiere una infraestructura diferente de la que se utilizó para la implementación. Para que sea fácil de consumir, puede optar por implementarlo como un servicio web con una interfaz de REST. Tanto Azure ML Studio como ML Server automatizan el proceso de creación de tales servicios. En el caso de ML Server, se proporcionan plantillas para la implementación de una infraestructura de soporte. Consulte la [documentación](https://docs.microsoft.com/machine-learning-server/what-is-operationalization?WT.mc_id=invopt-article-gmarchet) correspondiente.

La siguiente figura muestra la arquitectura de la implementación. Incluye las representaciones de los servidores que ejecutan el lenguaje R y Python. Ambos servidores se comunican con una subsección de nodos web que realizan el cálculo. Un almacén de datos de gran tamaño se conecta al bloque de cálculo.

![Diagrama de implementación de MS Server. Equilibrador de carga antes de varios nodos para su ejecución.](assets/sku-optimization-solution-guide/ml-server-deployment-example.png)<center><font size="1">_Figura 6: Ejemplo de implementación de ML Server_</font></center>


Para los modelos creados en HDInsight o Databricks y, por tanto, dependientes del entorno de Spark (bibliotecas, funcionalidades paralelas, etc.), es aconsejable plantearse ejecutarlos en un clúster. Puede ver una guía [aquí](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/spark-model-consumption?WT.mc_id=invopt-article-gmarchet).

Esto tiene la ventaja de que el modelo operacional se puede invocar a sí mismo a través de una actividad de canalización de Data Factory para la puntuación.

Para utilizar contenedores, puede empaquetar los modelos e implementarlos en Azure Kubernetes Service. El prototipo requerirá el uso de [Azure Data Science VM](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet); también debe instalar las herramientas de la [línea de comandos](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-service-deploy?WT.mc_id=invopt-article-gmarchet) de Azure ML en la máquina virtual.

## <a name="data-output-and-reporting"></a>Salida de datos e informes

Una vez implementado, el modelo será capaz de procesar los flujos de trabajo de transacciones financieras y las lecturas de existencias para generar predicciones de surtido óptimas. Por tanto, los datos generados se pueden almacenar en Azure SQL Data Warehouse para su posterior análisis. En concreto, es posible estudiar el rendimiento histórico de los diferentes SKU, identificando los mejores generadores de ingresos y los creadores de pérdidas. A continuación, podrá comparar estos con los surtidos sugeridos por los modelos y evaluar el rendimiento y la necesidad de volver a entrenar.

[Power BI](https://powerbi.microsoft.com/get-started/?&OCID=AID719832_SEM_uhlWLg3x&lnkd=Google_PowerBI_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvkyOLMJCrhqH8iac84aLX7EcUQIirSSqUCostzGi8y_XntJTCD73ZixoCQ4sQAvD_BwE?WT.mc_id=invopt-article-gmarchet) proporciona una manera de analizar y mostrar los datos generados en el proceso. 

La siguiente figura muestra un panel de Power BI típico. Incluye dos gráficos que muestran información de existencias de SKU. 

![Ejemplo de un panel que muestra los resultados de los últimos 12 meses.](assets/sku-optimization-solution-guide/sku-max-model.png)<center><font size="1">_Figura 7: Ejemplo de informe de resultados del modelo; por cortesía de Neal Analytics_</font></center>

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Una solución que se ocupa de la información confidencial contiene registros financieros, niveles de existencias e información de precios. Esta información confidencial se debe proteger. Para despejar preocupaciones sobre la seguridad y privacidad de los datos, tenga en cuenta que:

- Puede ejecutar algunas de las canalizaciones de Azure Data Factory localmente con Azure Integration Runtime. El entorno de ejecución ejecuta actividades de movimiento de datos hacia orígenes locales y desde estos. También enviará actividades para su ejecución en el entorno local.
  - En concreto, puede que le interese desarrollar una actividad personalizada para anonimizar los datos que se van a transferir a Azure (y ejecutarlos en el entorno local).
- Todos los servicios mencionados admiten el cifrado en tránsito y en reposo. Si decide almacenar los datos en una instancia de Azure Data Lake, el cifrado se habilita de forma predeterminada. Si usa Azure SQL Data Warehouse, puede habilitar el cifrado de datos transparente (TDE).
- Todos los servicios mencionados, con la excepción del estudio de ML, admiten la integración con Azure Active Directory para la autenticación y autorización. Si escribe su propio código, debe crear esa integración en la aplicación.

Para más información sobre RGPD, consulte nuestra página de [cumplimiento](https://www.microsoft.com/trustcenter?WT.mc_id=invopt-article-gmarchet).

## <a name="technologies-mentioned"></a>Tecnologías mencionadas

- [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet)
- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/?&OCID=AID719825_SEM_w1MNAVjn&lnkd=Google_Azure_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvk4bGtyQo11KBY-u2skor1SydsSl1vrYUmhyGhhwyJhDlAYpnMmIcRRoCTfsQAvD_BwE&dclid=CMn6lvfRkd0CFRwBrQYdtIoJOA?WT.mc_id=invopt-article-gmarchet)
- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet)
- [Azure Integration Runtime](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime?WT.mc_id=invopt-article-gmarchet)
- [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet)
- [Databricks](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet)
- [Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=invopt-article-gmarchet)
- [Azure ML Studio](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet)
- [Microsoft ML Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server?WT.mc_id=invopt-article-gmarchet)
- [Azure Data Science VM](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet)
- [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=invopt-article-gmarchet)
- [Microsoft PowerBI](https://powerbi.microsoft.com/?WT.mc_id=invopt-article-gmarchet)
- [Lenguaje de modelado de optimización de Pyomo](http://www.pyomo.org/)
- [Bonmin Solver](https://projects.coin-or.org/Bonmin)
- [Solucionador de TFoCS para Spark](https://github.com/databricks/spark-tfocs)
