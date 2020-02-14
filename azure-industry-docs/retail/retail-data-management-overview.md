---
title: Información básica para la administración de macrodatos en el sector minorista
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Los minoristas tienen grandes almacenes de datos con datos sin utilizar de los que pueden obtener información valiosa. En este artículo se describe cómo Microsoft Azure puede ayudar a usar esos datos de forma eficaz.
ms.openlocfilehash: 198e0f609889eee86e005c5ee56090006ae2a413
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054123"
---
# <a name="data-management-in-retail-overview"></a>Introducción a la administración de datos en el comercio minorista

## <a name="introduction"></a>Introducción

Los datos son la base para el desarrollo y la entrega de mejores experiencias de venta. Se pueden encontrar datos en cada una de las facetas de una organización de comercio minorista y se pueden usar para extraer información a lo largo de la cadena de valor y transformarla en rendimiento operativo y comportamiento del cliente a fin de impulsar experiencias del servicio mejoradas. Desde la exploración en línea, pasando por la interacción social y hasta las compras dentro de la tienda, los datos abundan. Sin embargo, la captura de datos es solo una parte de la administración de datos. La unión de datos dispares para su análisis requiere un tratamiento adecuado de los datos en una organización, de forma que se mejora la capacidad de un minorista para tomar decisiones impactantes sobre el ejercicio de su actividad comercial.

Por ejemplo, con el crecimiento de las compras mediante dispositivos móviles, los clientes han llegado a esperar que los minoristas tengan una cantidad de datos razonable sobre sus hábitos de compra para usarlos en la mejora de la experiencia. Un ejemplo de caso de uso es una oferta personalizada de producto y promoción enviada directamente al dispositivo móvil de un cliente al comprar en un lugar específico dentro de una tienda física. Al aprovechar los datos sobre qué, dónde, cómo, cuánto y con cuánta frecuencia, además de entradas adicionales como la disponibilidad del producto en la tienda, se crean oportunidades para enviar mensajes promocionales en tiempo real al dispositivo de un cliente cuando este compra cerca de un producto de destino. 

El uso eficaz de los datos puede hacer que el cliente compre, ya que ayuda al minorista a proporcionar una experiencia más significativa; por ejemplo, los minoristas podrían enviar al cliente una notificación con un código de descuento para su sitio web de comercio electrónico.  Además, estos datos conducirán a información procesable con la que los líderes corporativos pueden guiar sus acciones con decisiones respaldadas por los datos.

La acción de ofrecer una promoción está fundamentada en una combinación de puntos de datos y se desencadena cuando el cliente entra en la tienda. La capacidad para realizar estas conexiones y las acciones resultantes se basan en el modelo de administración de datos que se muestra a continuación.

![Flujo del proceso](./assets/retail-data-management-assets/process-flow.png)

En la Ilustración 1

Al llevar los datos a Azure, tenga en cuenta las tres P de orígenes de datos y su aplicabilidad a los escenarios que quiere permitir el minorista. Las tres P de orígenes de datos son Purchased, Public y Proprietary (comprados, públicos y propietarios).

> Los datos **comprados** normalmente amplían y mejoran los datos existentes de la organización la mayoría de las veces con datos del mercado y demográficos que complementan el alcance de la captura de datos de dicha organización. Por ejemplo, un minorista puede comprar datos demográficos adicionales para aumentar un registro de clientes maestro y garantizar que el registro es preciso y está completo. 
>
> Los datos **públicos** están disponibles gratuitamente y se pueden obtener de medios sociales, recursos gubernamentales (por ejemplo, geográficos) y otros orígenes en línea. Estos datos pueden deducir información como patrones climáticos que se correlacionan con patrones de compra o interacción social que señalan la popularidad de un producto en una región geográfica específica. Los datos públicos suele estar disponibles mediante API.
>
> Los datos **propietarios** residen en la organización. Pueden ser sistemas locales de un minorista, aplicaciones SaaS o proveedores de nube. Para acceder a los datos de un proveedor de aplicaciones SaaS y a los datos de otros proveedores, las API se usan normalmente para comunicar el sistema del proveedor. Aquí se incluyen datos como registros de sitios de comercio electrónico, datos de ventas de PDV y sistemas de administración de inventario.

Estos distintos tipos de datos se usan con diversa información procedente de la canalización de administración de datos.

## <a name="ingest"></a>Ingesta

Inicialmente, los datos se cargan en Azure en su formato nativo y se almacenan en consecuencia. Recibir y administrar orígenes de datos dispares puede ser complicado, pero Microsoft Azure ofrece servicios para cargar datos en la nube de forma rápida y sencilla, de forma que están disponibles para su procesamiento en la canalización de administración de datos. 

Azure cuenta con varios servicios útiles para la migración de datos. La elección depende del tipo de datos que se migre. [Azure Data Migration Service](/azure/dms/dms-overview?WT.mc_id=retaildm-docs-dastarr) para SQL Server y el servicio [Azure Import/Export](/azure/storage/common/storage-import-export-service?WT.mc_id=retaildm-docs-dastarr) son servicios que ayudan a introducir datos en Azure. Otros servicios de entrada de datos que se pueden considerar son los conectores [Azure Data Factory](/azure/data-factory?WT.mc_id=retaildm-docs-dastarr) y [Azure Logic Apps](/azure/logic-apps/?WT.mc_id=retaildm-docs-dastarr). Cada uno tiene sus propias características y debe investigarse para ver qué tecnología funciona mejor en una situación determinada.

La ingesta de datos no se limita a las tecnologías de Microsoft. Mediante [Azure Marketplace](https://azuremarketplace.microsoft.com?WT.mc_id=retaildm-docs-dastarr), los minoristas pueden configurar muchas bases de datos de proveedores diferentes en Azure para trabajar con los sistemas locales existentes. 

No todos los datos deben mantenerse en Azure. Por ejemplo, los datos de punto de venta (PDV) se pueden mantener en el entorno local, así las interrupciones de Internet no afectan a las transacciones de ventas. Estos datos se pueden poner en cola y cargar en Azure según una programación (quizás todas las noches o semanalmente) para analizarlos, pero siempre se deben tratar los datos locales como los verdaderamente fiables.

## <a name="prepare"></a>Preparación

Antes de comenzar el análisis, se deben preparar los datos. Este modelado de los datos es importante para garantizar la calidad de los modelos predictivos, los KPI de informes y la relevancia de los datos.

Hay dos tipos de datos que hay que abordar al preparar los datos para el análisis: estructurados y no estructurados. Los datos estructurados son más fáciles de abordar dado que ya están formados y tienen un formato. Puede que solo sea necesaria una sencilla transformación para pasar de datos estructurados en el formato de origen a datos estructurados ya listos para los trabajos de análisis. Los datos no estructurados suelen presentar más dificultades. Los datos no estructurados no se almacenan en un formato con una longitud de registro flexible. Por ejemplo, documentos, fuentes de medios sociales e imágenes y vídeos digitales. Estos datos deben administrarse de manera diferente a los datos estructurados y, con frecuencia, requieren un proceso dedicado para garantizar que estos datos acaben en el almacén de datos adecuado y de una manera que resulten de utilidad.

El modelado de datos se produce durante el proceso de extracción, transformación y carga (ETL), en la fase de preparación. Los datos se extraen de los orígenes de datos sin modificar importados en Azure, se limpian y se les vuelve a aplicar formato si es necesario, y se almacenan en un nuevo formato más estructurado. Una operación habitual de la preparación de datos de ETL es transformar los archivos .csv o Excel en archivos de paquetes, que son más fáciles de leer y procesar rápidamente para los sistemas de aprendizaje automático, como Apache Spark. Otro escenario común es crear archivos XML o JSON a partir de archivos .csv u otros formatos. El formato resultante es más fácil de usar con otros motores de análisis.

En Azure, hay varias tecnologías de transformación disponibles como servicios ETL para volver a dar forma a los datos. Las opciones incluyen [Azure Databricks](/azure/azure-databricks?WT.mc_id=retaildm-docs-dastarr), [Azure Functions](/azure/azure-functions/?WT.mc_id=retaildm-docs-dastarr) o Logic Apps. Databricks es una instancia totalmente administrada de Apache Spark y se usa para transformar los datos de una forma a otra. Azure Functions se caracteriza por funciones sin estado (o "sin servidor") con desencadenadores que las activan y ejecutan código. Logic Apps integra servicios.

## <a name="store"></a>Almacenamiento

Es necesario reflexionar sobre el almacenamiento de los datos antes de procesarlos. Los datos pueden llegar en formatos estructurados o no estructurados y la forma de los datos a menudo determina su destino de almacenamiento. Por ejemplo, los datos muy estructurados pueden ser adecuados para SQL de Azure. Los datos menos estructurados se pueden mantener en almacenamiento de blobs, almacenamiento de archivos o almacenamiento de tablas.

Los datos almacenados en Azure tienen un rendimiento excelente respaldado por un Acuerdo de Nivel de Servicio (SLA) sólido. Los servicios de datos proporcionan soluciones más fáciles de administrar, alta disponibilidad, replicación entre varias ubicaciones geográficas y, por encima de todo, Azure ofrece los almacenes de datos y servicios necesarios para impulsar el aprendizaje automático.

Tanto los datos estructurados como no estructurados se pueden almacenar en [Azure Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=retaildm-docs-dastarr) y se pueden consultar mediante [U-SQL](/azure/data-lake-analytics/data-lake-analytics-u-sql-get-started?WT.mc_id=retaildm-docs-dastarr), un lenguaje de consulta específico de Azure Data Lake. Ejemplos de datos que pueden incluirse en una instancia de Data Lake son los siguientes, que se dividen en orígenes de datos normalmente estructurados y no estructurados.

### <a name="structured-data"></a>Datos estructurados

- Datos de CRM y otras aplicaciones de línea de negocio
- Datos de transacción de PDV
- datos del sensor
- Datos relacionales
- Datos de transacciones de comercio electrónico

### <a name="unstructured-data"></a>Datos no estructurados

- Fuentes sociales
- Vídeo
- Imágenes digitales
- Análisis clickstream del sitio web

Hay un número creciente de casos de uso que admiten datos no estructurados para generar valor. Este movimiento está impulsado por el deseo de decisiones basadas en los datos y el avance en tecnología, como la inteligencia artificial, para permitir la captura y el procesamiento de datos a escala. Por ejemplo, los datos pueden incluir fotografías o streaming de vídeo. Por ejemplo, el streaming de vídeo se puede aprovechar para detectar las selecciones de compra para una finalización de compra perfecta; o, los datos del catálogo de productos se pueden combinar íntegramente con una foto del cliente de su ropa favorita para proporcionar una vista de artículos parecidos o recomendados. 

Algunos ejemplos de datos estructurados son fuentes de datos de bases de datos relacionales, datos de sensor, archivos de Apache Parquet y datos de comercio electrónico. La estructura inherente de estos datos los hace adecuados para una canalización de aprendizaje automático.

El servicio Azure Data Lake también permite consultas por lotes e interactivas, junto con análisis en tiempo real mediante [Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview?WT.mc_id=retaildm-docs-dastarr). Además, Data Lake es especialmente adecuado para cargas de trabajo de análisis de datos muy grandes. Por último, los datos en Data Lake son persistentes y no tienen límite de tiempo.

Otros almacenes de datos, como las bases de datos relacionales, el almacenamiento de blobs, almacenamiento de Azure Files y el almacenamiento de documentos de Cosmos DB también pueden contener datos limpios preparados para el análisis de bajada en la canalización de administración de datos. No hay ningún requisito excepto el uso de una instancia de Data Lake.

## <a name="analyze"></a>Analizar

En caso de problemas, como la reducción del costo del inventario, los minoristas pueden usar el análisis realizado por un proceso de Machine Learning.

El análisis de datos prepara los datos para el procesamiento mediante un motor de Machine Learning para obtener información detallada sobre la experiencia del cliente. Este proceso genera un modelo que "aprende" y que se puede aplicar a los datos futuros para predecir resultados. Los modelos definen los datos que se examinarán y cómo se analizarán los datos mediante diversos algoritmos. El uso de los datos de salida del análisis con la visualización de datos es lo que podría desencadenar una conclusión; por ejemplo, ofrecer un cupón en la tienda para un artículo de la lista de deseos del cliente en la plataforma de comercio electrónico de los minoristas.

Para analizar los datos, los ecosistemas de aprendizaje se alimentan con los datos almacenados para el procesamiento. Normalmente, esto es aprendizaje automático que se realiza mediante Hadoop, Databricks o una instancia de Spark autoadministrada que se ejecuta en una máquina virtual. Este análisis se puede realizar también con una simple consulta de los datos. La información sobre los KPI se puede encontrar con frecuencia en los datos limpios sin pasar por la canalización de aprendizaje automático.

[Hadoop](/azure/hdinsight/hdinsight-hadoop-architecture?WT.mc_id=retaildm-docs-dastarr) forma parte del servicio de Azure totalmente administrado, [HDInsight](/azure/hdinsight/?WT.mc_id=retaildm-docs-dastarr). HDInsight es una colección de herramientas de aprendizaje de datos que se usan para entrenar modelos de datos, transferir los datos a un almacenamiento de datos y realizar consultas en Hadoop mediante el lenguaje de consulta de Hive. HDInsight puede analizar datos de streaming o históricos.

Se pueden aplicar diversos algoritmos de aprendizaje a los datos como parte del entrenamiento y para mantener los modelos de datos. Un modelo de datos determina explícitamente la estructura de datos generada para los analistas.

En primer lugar, los datos se limpian y se forman correctamente. Luego, se procesan mediante un sistema de aprendizaje automático, como HDInsight o Apache Spark. Para ello, se usan los datos existentes para entrenar un modelo que, a su vez, se usa en el análisis de datos. El modelo entrenado se actualiza periódicamente con nuevos datos conocidos para aumentar su precisión durante el análisis. Los servicios de aprendizaje automático emplean el modelo para realizar un análisis de los datos que se van a procesar.

Después de entrenar el modelo y ejecutar un proceso de análisis de datos, los datos derivados del análisis de aprendizaje automático se pueden almacenar en un almacenamiento de datos o en bases de datos de almacenamiento normalizadas para datos analíticos. Microsoft proporciona [Power BI](/power-bi/?WT.mc_id=retaildm-docs-dastarr), una completa herramienta de análisis de datos, para un análisis profundo de los datos en el almacenamiento de datos.

## <a name="action"></a>Acción

Los datos del comercio minorista se mueven constantemente, y los sistemas que los gestionan deben hacerlo también de forma oportuna. Por ejemplo, los datos de compradores de comercio electrónico deben procesarse rápidamente. Así, los artículos del carro de la compra de un comprador se pueden usar para ofrecer servicios adicionales o artículos complementarios durante el proceso de finalización de la compra. Esta forma de tratamiento y análisis de los datos se debe producir casi inmediatamente y normalmente la llevan a cabo sistemas que ejecutan transacciones en "microlotes". Es decir, los datos se analizan en un sistema que tiene acceso a datos ya procesados y se ejecutan mediante un modelo.

Podrían producirse otras operaciones "en lote" a intervalos regulares, pero no tienen que tener lugar casi en tiempo real. Cuando el análisis por lotes se produce en el entorno local, estos trabajos se ejecutan con frecuencia durante la noche, los fines de semana o cuando los recursos no están en uso. Con Azure, el escalado de grandes trabajos por lotes y las máquinas virtuales necesarias para respaldarlos pueden producirse en cualquier momento.

Para comenzar, realice los pasos siguientes:

1. Cree un plan de ingesta de datos para almacenes de datos que proporcione valor al análisis que se va a realizar. Con un plan detallado de migración y sincronización de datos implantado, introduzca los datos en Azure en su formato original. 

2. Determine la información procesable necesaria y elija una canalización de procesamiento de datos para dar cabida a las actividades de procesamiento de datos. 

3. Teniendo en cuenta estas características de datos, cree una canalización de procesamiento de datos mediante los algoritmos adecuados para obtener la información que se busca.

4. Si es posible, use un modelo de datos común para transferir la salida a un almacenamiento de datos; aquí se pueden exponer las características de datos más interesantes. Normalmente, esto significa leer los datos en los sistemas de almacenamiento de Azure originales y escribir la versión limpia en otro almacén de datos.

5. Procese los datos mediante las canalizaciones de aprendizaje automático que proporcionan Spark o Hadoop. A continuación, suministre la salida a un almacenamiento de datos. Existen muchos algoritmos predeterminados para procesar los datos, aunque los minoristas pueden implementar los suyos propios. Además de los escenarios de aprendizaje automático, cargue los datos en almacenamiento de datos estándar y aplique un modelo de datos comunes; luego, consulte los datos de KPI. Por ejemplo, los datos pueden estar almacenados en un esquema de estrella u otro almacén de datos.

Con los datos ahora listos para que los usen los analistas de datos, se puede detectar información procesable y realizar acciones para explotar este nuevo conocimiento. Por ejemplo, las preferencias de compra de un cliente se pueden volver a cargar en los sistemas del minorista y usarse para mejorar varios puntos de contacto del cliente, como los siguientes:

- Aumentar el promedio de transacciones de comercio electrónico o PDV mediante la agrupación de productos
- Historial de compras en CRM para dar soporte a todas las llamadas de los clientes al centro de llamadas
- Sugerencias de productos a medida mediante un motor de recomendaciones de comercio electrónico
- Anuncios dirigidos y pertinentes basados en los datos del cliente
- Disponibilidad actualizada del inventario basada en el movimiento de productos dentro de la cadena de suministro

Otro tipo de información que puede surgir son patrones no cuestionados anteriormente. Por ejemplo, se puede descubrir que se produce una mayor pérdida de inventario entre las 3 y las 7 de la tarde. Esto podría implicar la necesidad de datos adicionales para determinar la causa y un curso de acción, como mejorar la seguridad o los procedimientos operativos estándar.

## <a name="conclusion"></a>Conclusión

La administración de datos en el comercio minorista es compleja. Sin embargo, ofrece la valiosa capacidad de aportar relevancia y una mejor experiencia del cliente. Mediante las técnicas de este artículo, se puede obtener información para mejorar la experiencia del cliente, impulsar resultados empresariales rentables y descubrir tendencias que pueden conducir a mejoras operacionales.

Para seguir comprendiendo mejor las funcionalidades de Azure relacionadas con la implementación de una canalización de administración de datos, lea lo siguiente:

- Vea cómo [Azure Data Factory](/azure/data-factory/?WT.mc_id=retaildm-docs-dastarr) puede ayudar a la ingesta de datos desde los almacenes de datos locales hasta Azure.
- Más información sobre cómo [Azure Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=retaildm-docs-dastarr) puede servir para almacenar todos los datos, tanto estructurados como no estructurados.
- Vea los informes comerciales reales que ilustran cómo [Power BI](https://powerbi.microsoft.com/en-us/industries/retail/?WT.mc_id=retaildm-docs-dastarr) puede proporcionar respuestas detalladas a preguntas conocidas, pero permiten el análisis de tendencias.
- Visite [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=retaildm-docs-dastarr) para encontrar soluciones compatibles con las del entorno local.

_Este artículo ha sido creado por David Starr y Mariya Zorotovich._
