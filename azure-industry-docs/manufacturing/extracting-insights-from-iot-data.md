---
title: Arquitectura para extraer información de IoT para procesos de fabricación
description: Extraiga información de los datos de IoT con los servicios de Azure.
author: ercenk
ms.author: ercenk
manager: gmarchet
ms.service: industry
ms.topic: article
ms.date: 11/28/2019
ms.openlocfilehash: c08e6bbb1da47084122dae1ed6a9e1cea0b59473
ms.sourcegitcommit: db3bee67c1467884af223a48a895715afba8e08c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2019
ms.locfileid: "75005314"
---
# <a name="extracting-actionable-insights-from-iot-data"></a>Extracción de información útil de los datos de IoT

Si usted está a cargo de las máquinas de una fábrica, ya sabrá que la adopción de Internet de las cosas es el siguiente paso que debe dar para la mejora de sus procesos y resultados. Disponer de sensores en las máquinas, o en la planta de fabricación, constituye el primer paso. El siguiente paso es usar los datos, y ese es precisamente el objeto de este documento. Esta guía proporciona una introducción técnica de los componentes necesarios para extraer información útil del análisis de datos de IoT.

Las soluciones de análisis de IoT se ocupan de transformar los datos de IoT sin procesar que provienen de un conjunto de dispositivos en un formato más adecuado para el análisis. Una vez que los datos estén en un formato que permite el análisis, podremos llevar a cabo este proceso. Estos son algunos ejemplos de análisis:

- visualizaciones simples de los datos de telemetría; por ejemplo, un gráfico de barras que muestra las temperaturas en un período de tiempo
- cálculo de los KPI, por ejemplo, la eficacia global de los equipos
- análisis predictivo basado en modelos de aprendizaje automático

Estos análisis, a su vez, proporcionan información que fundamentan otras acciones. Estas acciones pueden ir desde el envío de un comando simple a una máquina hasta el ajuste de los parámetros operativos, la realización de una acción en otro sistema de software o la implantación de programas de mejora operativa en toda empresa.

La siguiente ilustración muestra una cadena de acciones que se producen entre la máquina de una fábrica y el resultado final, que es una representación en panel de la "utilización" con un gráfico y la etiqueta "87,5 %".

![De la fábrica al panel.](assets/extracting-insights-from-iot/factory-to-dashboard.png)

Usaremos como ejemplo el cálculo de un KPI sencillo: el uso de la máquina. El *uso de la máquina* es el porcentaje de tiempo que la máquina pasa realmente *produciendo* piezas. Por ejemplo, si un turno consta de 8 horas, y la máquina produce piezas durante 7 de esas horas, el uso de la máquina en ese turno es el 87,5 % (7/8 x 100).

## <a name="approach"></a>Enfoque

Las aplicaciones de IoT tienen tres componentes: **cosas** (o dispositivos), el envío de datos o eventos que se usan para generar **información**, que a su vez se usan para generar **acciones** destinadas a mejorar un negocio o proceso.

Los equipos de una planta de fabricación (cosas) envían varios tipos de datos mientras se encuentran en funcionamiento. Por ejemplo, una fresadora que envía datos de temperatura y velocidad de alimentación; esto se usa para evaluar si la máquina está funcionando o no —información—, lo cual sirve para optimizar la planta —acción—. Vamos a ver cuáles son los pasos necesarios para extraer los datos, visualizarlos en un panel, extraer la nueva información y adoptar medidas adicionales.
![Cosas a Información a Acciones.](assets/extracting-insights-from-iot/things-insights-actions.png)

Microsoft ha publicado una arquitectura de referencia detallada para aplicaciones de IoT que le guía a través de los distintos subsistemas y los enfoques recomendados para estos.
Una aplicación de IoT se compone de los siguientes subsistemas:

1. Dispositivos, o puertas de enlace perimetrales locales, que son un tipo específico de dispositivo que puede registrar con seguridad orígenes de mensajes (dispositivos) en la nube. La puerta de enlace perimetral también puede transformar mensajes de un protocolo nativo a otro formato (por ejemplo, JSON).
2. Un servicio de puerta de enlace en la nube, o un concentrador (como [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk) o [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk)), para ingerir datos de forma segura y ofrecer funcionalidades de administración de dispositivos. 
3. Procesadores de secuencias que consumen datos de streaming. Los procesadores también pueden integrarse con procesos empresariales y enviar los datos al almacenamiento.
4. Una interfaz de usuario, con forma de panel, para visualizar datos de IoT y facilitar la administración de dispositivos.

![Arquitectura de la solución.](assets/extracting-insights-from-iot/architecture.png)

En este artículo, nos centramos en el proceso de extracción de la información. Estos son los pasos principales:

1. Acceder a los datos y procesarlos en un flujo de datos.
2. Procesar y almacenar los datos.
3. Visualizar o presentar los datos. 

La ilustración siguiente es un diagrama que muestra el flujo de datos, desde el origen de datos hasta la conversión, ingesta, procesamiento y almacenamiento, presentación y, finalmente, acción.

![Flujo de datos: origen-ingesta-presentación-acción.](assets/extracting-insights-from-iot/data-flow.png)

## <a name="converting-the-data-to-a-stream"></a>Conversión de los datos a una secuencia

Los datos de IoT son datos de series temporales: valores de "cosas" que pueden ser más significativas durante un intervalo de tiempo especificado. Los equipos de una planta de fabricación funcionan durante un periodo de tiempo, y en ese tiempo se producen eventos. Si los datos de la planta no se envían a un servicio de ingesta de datos, como [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk), debemos sondear los datos periódicamente desde su almacén —por ejemplo, un sistema de ejecución de fabricación, o un punto de conexión HTTP— y enviar los datos a un servicio de ingesta.  
Para convertir los datos en una secuencia, normalmente hacemos lo siguiente:

1. Acceder al origen de los datos.
2. Transformar y enriquecer los datos.
3. Publicar los datos en un punto de conexión que pueda ingerir datos en streaming.

No trataremos aquí en detalle el acceso al origen de datos, ya que depende de dónde residan los datos, y las variaciones son demasiado numerosas.

## <a name="transforming-and-enriching-the-data"></a>Transformación y enriquecimiento de los datos

Los datos sin procesar normalmente deben someterse a operaciones de transformación para estandarizarlos y prepararlos para la ingesta.  Las transformaciones específicas variarán según el tipo de análisis utilizado.  Algunos ejemplos habituales de transformaciones de datos son los datos de series temporales en las que es posible que falten las mediciones y deban ingresarse, o donde las escalas de tiempo entre diferentes máquinas deban racionalizarse.  Nos gustaría disponer de registros de datos con marca de tiempo (si el origen lo contiene) y en pares de nombre-valor. Normalmente, los datos vienen en un formato jerárquico y deben transformarse a una estructura plana.

La siguiente ilustración muestra una estructura jerárquica de datos con un perfil escalonado convertida en un formato de columna y fila normalizado (un bloque).

![Forma de datos de jerárquica a plana.](assets/extracting-insights-from-iot/hierarchy-to-flat.png)

Normalmente, los datos no son accesibles desde Internet. Un patrón común es usar una puerta de enlace perimetral para insertar datos desde la planta de fabricación al punto de ingesta. [Azure IoT Edge](/azure/iot-edge?WT.mc_id=iotinsightssoln-docs-ercenk) es un servicio que se basa en IoT Hub. Un dispositivo IoT Edge puede actuar como puerta de enlace para abarcar tres [modelos](/azure/iot-edge/iot-edge-as-gateway?WT.mc_id=iotinsightssoln-docs-ercenk): puerta de enlace transparente, traducción del protocolo y traducción de la identidad.

Si los datos están disponibles externamente y son accesibles desde Internet, pueden utilizarse varios servicios de Azure para tener acceso a ellos, transformarlos y enriquecerlos. Entre estas opciones se encuentran las siguientes:

- Código personalizado implementado en varios servicios de proceso de Azure, como [App Service](/azure/app-service/?WT.mc_id=iotinsightssoln-docs-ercenk), [Azure Kubernetes Service](/azure/aks/?WT.mc_id=iotinsightssoln-docs-ercenk) (AKS), [Container Instances](/azure/container-instances/?WT.mc_id=iotinsightssoln-docs-ercenk) o [Service Fabric](/azure/service-fabric/service-fabric-overview?WT.mc_id=iotinsightssoln-docs-ercenk).
- [Azure Logic Apps](/azure/logic-apps/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Canalizaciones y actividades en Azure Data Factory](/azure/data-factory/copy-activity-overview/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [BizTalk Services](https://azure.microsoft.com/services/biztalk-services/)

Cada uno de los servicios anteriores tiene sus propias ventajas y costos, según el escenario. Por ejemplo, Logic Apps proporciona un medio para [transformar documentos XML](/azure/logic-apps/logic-apps-enterprise-integration-transform?WT.mc_id=iotinsightssoln-docs-ercenk). Sin embargo, los datos pueden ser un documento XML excesivamente complejo, por lo que puede no resultar práctico desarrollar un script XSLT de gran volumen para transformar los datos. En este caso, es posible desarrollar una solución híbrida con varios microservicios desde diferentes servicios de Azure. Por ejemplo, un microservicio, implementado en Azure Logic Apps, puede sondear un punto de conexión HTTP, almacenar temporalmente el resultado sin formato y hacérselo llegar a otro microservicio. El otro microservicio —que transforma el mensaje— puede ser código personalizado hospedado en un [host de Azure Functions](https://github.com/Azure/azure-functions-host).  

![Sondeo de datos en HTTPS y procesamiento por Functions.](assets/extracting-insights-from-iot/poll-logic-process.png)

También puede optar por un flujo de trabajo organizado mediante Azure Data Factory donde una secuencia de actividades realiza las transformaciones. Para obtener más detalles sobre el tipo de actividades disponibles, consulte [Canalizaciones y actividades en Azure Data Factory](/azure/data-factory/concepts-pipelines-activities).
Los mensajes pueden recibir la marca de tiempo en la recepción, o bien pueden contener una marca de tiempo para que podamos reconstruir la serie temporal de varios valores medidos. Por lo tanto, una latencia de ingestión insignificante y un alto rendimiento son fundamentales para garantizar la integridad de la información y la oportunidad de las posibles respuestas. Para minimizar la latencia, normalizamos las marcas de tiempo lo más cerca posible de la planta.

## <a name="ingesting-the-data-stream"></a>Ingesta de flujo de datos

Para analizar los datos como una secuencia, podemos realizar consultas sobre los datos basadas en ventanas temporales para identificar patrones y relaciones. Hay varios servicios en la plataforma Azure que pueden ingerir datos con un elevado rendimiento.
La elección entre los siguientes servicios depende de las necesidades del proyecto, como la administración de dispositivos, la compatibilidad con los protocolos, la escalabilidad, las preferencias del equipo del modelo de programación, etc. Por ejemplo, el equipo puede preferir el uso de Kafka debido a su experiencia, o a la necesidad de tener un agente de Kafka para la solución. O bien, en otro caso, el proyecto puede necesitar que el sistema de ingesta de datos aproveche la [atestación de clave de TPM de IoT Hub Device Provisioning](/azure/iot-dps/?WT.mc_id=iotinsightssoln-docs-ercenk) para asegurar el acceso de los dispositivos al punto de ingesta.

- [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk) es un centro de la comunicación bidireccional entre las aplicaciones de IoT y los dispositivos. Es un servicio escalable que proporciona completas soluciones de IoT por medio de comunicaciones seguras, enrutamiento de mensajes, integración con otros servicios de Azure y funciones de administración para controlar y configurar los dispositivos.

- [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk) es un servicio de solo ingesta a gran escala para recopilar datos de telemetría desde orígenes simultáneos con tasas de rendimiento extremadamente altas.
- [Apache Kafka en HDInsight](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk) es un servicio administrado que aloja [Apache Kafka](https://kafka.apache.org/). Apache Kafka es una plataforma de streaming distribuida de código abierto que proporciona también la funcionalidad de agente de mensajes. El servicio hospedado tiene un Acuerdo de Nivel de Servicio (SLA) del 99,9 % sobre el tiempo de actividad de Kafka.

## <a name="processing-and-storing-the-data"></a>Procesamiento y almacenamiento de datos

Las aplicaciones de IoT presentan algunos desafíos, ya que son sistemas impulsados por eventos que también se deben mantener y operan sobre datos históricos. Los datos entrantes son un tipo de datos adjuntos y potencialmente pueden crecer. Es necesario mantener los datos durante períodos más prolongados, principalmente con estos fines: archivo, análisis de lotes y modelos de aprendizaje automático. Por otro lado, la secuencia de eventos es fundamental para el análisis casi en tiempo real a fin de detectar anomalías, reconocer patrones durante ventanas de tiempo consecutivas o desencadenar alertas, si los valores suben o bajan de un umbral establecido.
La arquitectura de referencia de IoT de Azure de Microsoft presenta un flujo de datos recomendado para mensajes y eventos del dispositivo a la nube en una solución de IoT con una arquitectura Lambda. La arquitectura Lambda permite el análisis de datos tanto casi en tiempo real como en streaming, así como datos archivados, lo que la convierte en la mejor opción para procesar los datos entrantes.

## <a name="lambda-architecture"></a>Arquitectura lambda

La arquitectura lambda aborda este problema mediante la creación de dos rutas de acceso para el flujo de datos. Todos los datos que entran en el sistema atraviesan estas dos rutas de acceso:

- Una capa por lotes (ruta de acceso inactiva) almacena todos los datos de entrada sin formato y realiza el procesamiento por lotes de los datos. El resultado de este procesamiento se almacena en forma de vista por lotes. Es una canalización de procesamiento lento que ejecuta un análisis complejo, por ejemplo, combinar datos de varios orígenes y durante un período más largo (horas, días o más) y generar información nueva, como informes, modelos de aprendizaje automático, etc.
- Una capa de velocidad (ruta de acceso activa) analiza los datos en tiempo real. Este nivel está diseñado para que tenga una latencia baja, a costa de la precisión. Es una canalización de procesamiento más rápida que archiva y muestra los mensajes entrantes y analiza estos registros generando información crítica a corto plazo y acciones como las alarmas.
- La capa por lotes se distribuye en una "capa de servicio", que responde a las consultas. La capa por lotes indexa la vista por lotes para realizar consultas eficaces. La capa de velocidad actualiza el nivel de servicios con actualizaciones incrementales en función de los datos más recientes.

La siguiente imagen muestra cinco bloques que representan las fases de transformación. El primer bloque es el flujo de datos, que se distribuye por la capa de velocidad y la capa por lotes en paralelo. Ambas capas se distribuyen por la capa de servicio. La capa de velocidad y la capa de servicio se envían al cliente de análisis.
![Arquitectura lambda.](assets/extracting-insights-from-iot/lambda-schematic.png)

 La plataforma Azure proporciona diversos servicios que se pueden usar para implementar la arquitectura. El diagrama siguiente muestra cómo se pueden asignar esos servicios con tal fin. La ilustración muestra las cinco fases de transformación, donde cada fase contiene tecnologías pertinentes de Azure. Los cuadros de colores más oscuros representan la disponibilidad de varias opciones para realizar esas tareas.

![Arquitectura lambda.](assets/extracting-insights-from-iot/lambda-architecture-all.png)

Las opciones para el servicio de ingesta de datos en la capa de velocidad se tratan en la sección anterior, "Ingesta de flujo de datos".

[Apache Kafka en HDInsight](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk) puede ser una opción de servicio para implementar la secuencia de datos tanto para el servicio de ingesta de datos como para el procesamiento de secuencias.

Recurra a [Azure Stream Analytics](/azure/stream-analytics?WT.mc_id=iotinsightssoln-docs-ercenk) (ASA) si usa Event Hubs para el servicio de ingesta de datos. Azure Stream Analytics es un motor de procesamiento de eventos que permite examinar grandes volúmenes de streaming de datos procedentes de dispositivos. Los datos de entrada pueden proceder de dispositivos, sensores, sitios web, fuentes de redes sociales, aplicaciones, etc. También admite la extracción de información de flujos de datos y la identificación de patrones y relaciones.

Las consultas de Stream Analytics comienzan con un origen de datos en streaming que se ingieren en Azure Event Hub, Azure IoT Hub o desde un almacén de datos como Azure Blob Storage. Para examinar los flujos, cree un trabajo de Stream Analytics que especifica el origen de entrada que transmite los datos. El trabajo también especifica una consulta de transformación que define cómo buscar datos, patrones o relaciones. Dicha consulta aprovecha un lenguaje de consulta similar a SQL que se utiliza para filtrar, ordenar, agregar y unir los datos de transmisión a lo largo de un período de tiempo.

## <a name="warm-path"></a>Ruta de acceso activa

El escenario de ejemplo de este documento es un KPI de uso de la máquina (que se presentó al principio de la guía). Podríamos optar por una interpretación sencilla de los datos y suponer que, si la máquina envía datos, es que se está utilizando. Sin embargo, la máquina podría estar enviando datos sin producir nada realmente (por ejemplo, podría estar inactiva o con tareas de mantenimiento). Esto pone de manifiesto una dificultad con la que nos encontramos habitualmente al tratar de extraer información de los datos de IoT: los datos que se están buscando no están disponibles en los datos que se están obteniendo. Por lo tanto, en nuestro ejemplo, no estamos recibiendo datos que nos indiquen de forma clara e inequívoca si la máquina está produciendo.  Así pues, es necesario inferir el uso a partir de la combinación de los datos que obtenemos con otros orígenes de datos, y aplicar reglas para determinar si la máquina está produciendo. Además, estas reglas pueden cambiar entre las empresas porque es posible que cada una tenga interpretaciones diferentes de lo que significa "producir". La ruta de acceso activa consiste en analizar a medida que los datos fluyen a través del sistema. Esta secuencia se procesa casi en tiempo real, se guarda en el almacenamiento intermedio y se envía a los clientes encargados del análisis.

Azure Event Hubs es una plataforma de streaming de macrodatos y un servicio de ingesta de eventos, capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Event Hubs es la herramienta idónea para el primer paso en la ruta de acceso activa para el flujo de datos.

La siguiente ilustración muestra la fase de la capa de velocidad. Consta de un centro de eventos, una instancia de Stream Analytics y un almacén de datos para el almacenamiento intermedio.

![Arquitectura lambda: capa de velocidad resaltada.](assets/extracting-insights-from-iot/lambda-2.png)
  
La plataforma Azure proporciona muchas opciones para procesar los eventos en un centro de eventos, sin embargo, se recomienda Stream Analytics. Stream Analytics también puede insertar datos en el servicio Power BI para visualizar los datos transmitidos.

Stream Analytics puede ejecutar un análisis complejo a escala, por ejemplo, ventanas de saltos de tamaño constante, deslizantes o de salto; agregaciones de secuencias y combinaciones de orígenes de datos externos. Para un procesamiento aún más complejo, el rendimiento puede ampliarse aplicando cascadas a varias instancias de Event Hubs, trabajos de Stream Analytics y funciones de Azure, como se muestra en la ilustración siguiente.

![Event Hubs para análisis en Power BI.](assets/extracting-insights-from-iot/event-hubs-to-power-bi.png)
  
El almacenamiento intermedio puede implementarse con diversos servicios en la plataforma Azure, como Azure SQL Database. Se recomienda [Azure Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=iotinsightssoln-docs-ercenk). Se trata de la base de datos multimodelo de distribución global de Microsoft. Es mejor para conjuntos de datos que pueden beneficiarse de interfaces flexibles, sin esquemas, de indexación automática y de consulta enriquecida. Cosmos DB permite la ejecución en varias regiones y la lectura/escritura y admite la conmutación por error manual además de la conmutación automática por error. Además, Cosmos DB permite al usuario establecer un período de vida (TTL) en sus datos, lo que hace que los datos antiguos expiren de manera automática. Se recomienda usar la característica para controlar el tiempo que los registros permanecen en la base de datos, controlando así el tamaño de la base de datos.

Los precios para Cosmos DB se basan en el almacenamiento usado y las [unidades de solicitud](/azure/cosmos-db/request-units) aprovisionadas. Cosmos DB es mejor para escenarios que no requieren consultas que implican la agregación en grandes conjuntos de datos, ya que esas consultas exigen más unidades de solicitud que una consulta básica, como el último evento de un dispositivo.  

[Microsoft Power BI](/power-bi/power-bi-overview?WT.mc_id=iotinsightssoln-docs-ercenk) es una colección de servicios de software, aplicaciones y conectores que funcionan conjuntamente para convertir los orígenes de datos no relacionados en información coherente, interactiva y visualmente atractiva. Power BI le ayuda a mantenerse al día con la información que realmente le importa. Puede usar el [streaming en tiempo real en Power BI](/power-bi/service-real-time-streaming?WT.mc_id=iotinsightssoln-docs-ercenk) para insertar datos en el servicio. Este flujo en tiempo real puede actuar como un origen de datos de transmisión en tiempo real para varias imágenes en el panel de Power BI.

## <a name="cold-path"></a>Ruta de acceso inactiva

La ruta de acceso activa es donde se produce el procesamiento de la secuencia para descubrir patrones a lo largo del tiempo. Sin embargo, también nos gustaría calcular la utilización durante un período de tiempo en el pasado, con diferentes ejes y agregaciones, como máquina, línea, planta, pieza producida, etc. Queremos combinar dichos resultados con los resultados de la ruta de acceso activa para presentar una vista unificada al usuario. La ruta de acceso inactiva incluye la capa por lotes y las capas de servicio. La combinación proporciona una vista del sistema a largo plazo.

La ruta de acceso inactiva contiene el almacén de datos a largo plazo para la solución. También contiene la capa por lotes, que crea las vistas agregadas calculadas previamente para proporcionar respuestas rápidas a las consultas durante largos períodos de tiempo. Las opciones de tecnología disponibles para esta capa en la plataforma Azure son muy variadas.

![Arquitectura lambda: capa por lotes resaltada.](assets/extracting-insights-from-iot/lambda-3.png)
  
[Azure Time Series Insights](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk) (TSI) es un servicio de análisis, almacenamiento y visualización de datos de series temporales. Proporciona filtrado y agregación de tipo SQL, lo que reduce la necesidad de funciones definidas por el usuario. TSI puede recibir datos de Event Hubs, IoT Hub o Azure Blob Storage. Todos los datos en TSI se almacenan en memoria y en SSD, lo que garantiza que estén siempre listos para el análisis interactivo. Por ejemplo, una agregación típica en decenas de millones de eventos se devuelve en milisegundos. También proporciona visualizaciones como superposiciones de serie temporales diferentes, comparaciones de paneles, vistas tabulares accesibles y mapas térmicos. Entre las principales características de TSI cabe mencionar las siguientes:

- Servicios de visualización integrados para las soluciones que no requieren informar sobre los datos de inmediato. TSI tiene una latencia aproximada para consultar registros de datos de entre 30 y 60 segundos. 
- La capacidad de consultar grandes conjuntos de datos.
- Cualquier número de usuarios puede llevar a cabo un número ilimitado de consultas sin ningún costo adicional.

TSI tiene una retención máxima de 400 días y un límite de almacenamiento máximo de 3 TB. Si necesita un período de retención más largo o más capacidad, use una base de datos de almacenamiento en frío (llevando los datos a TSI para consultar según sea necesario).

Es seguro que el almacenamiento en frío de una aplicación crecerá con el tiempo. Aquí es donde los datos se almacenan a largo plazo y se agregan en las vistas de lotes para análisis. Los datos para modelos de aprendizaje automático también se almacenan aquí. Se recomienda [Azure Storage](/azure/storage/?WT.mc_id=iotinsightssoln-docs-ercenk) para el almacenamiento en frío. Se trata de un servicio administrado por Microsoft que proporciona almacenamiento en la nube altamente disponible, seguro, duradero, escalable y redundante. Azure Storage incluye Azure Blobs (objetos), Azure Data Lake Storage Gen2, Azure Files, Azure Queues y Azure Tables. El almacenamiento en frío puede producirse en Blobs, Data Lake Storage Gen2 o Azure Tables, o una combinación de ellos.

[Azure Table Storage](/azure/cosmos-db/table-storage-overview?WT.mc_id=iotinsightssoln-docs-ercenk) es un servicio que almacena datos NoSQL estructurados en la nube y proporciona un almacén de claves y atributos con un diseño sin esquema. Como Table Storage carece de esquema, es fácil adaptar los datos a medida que evolucionan las necesidades de la aplicación. El acceso a los datos de Table Storage es rápido y rentable para muchos tipos de aplicaciones y, por lo general, el costo es normalmente menor que con el SQL tradicional para volúmenes parecidos de datos. Se usa una tabla para los ejemplos y otra tabla para los eventos que se reciben de los flujos de datos. El diseño de la clave de partición es un concepto especialmente importante; ambas tablas usan la hora de la marca de tiempo en el ejemplo o el evento. Para obtener más información, consulte [Descripción del modelo de datos del servicio Tabla](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?WT.mc_id=iotinsightssoln-docs-ercenk).

Para almacenar grandes cantidades de datos no estructurados, como JSON o documentos XML que contienen los datos no procesados recibidos por la aplicación IoT, [Blob storage](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=iotinsightssoln-docs-ercenk), [Azure Files](/azure/storage/files/storage-files-introduction?WT.mc_id=iotinsightssoln-docs-ercenk) o [Azure Data Lake Storage Gen2](/azure/storage/data-lake-storage/introduction?WT.mc_id=iotinsightssoln-docs-ercenk) son las mejores opciones.

Se puede acceder a Azure Blob Storage desde cualquier lugar del mundo a través de HTTP o HTTPS de forma segura. El acceso al almacenamiento de blobs debe autorizarse mediante uno de los [mecanismos de autorización](/azure/storage/common/storage-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk) utilizados por el servicio. El servicio proporciona múltiples [opciones](/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk) de replicación: redundancia local, redundancia de zona, redundancia geográfica y redundancia geográfica con acceso de lectura. También hay tres [niveles de acceso](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=iotinsightssoln-docs-ercenk) que permite que las soluciones sean más rentables.

Una vez que los datos están almacenados en frío, se deben crear vistas por lotes en la capa de servicio de la arquitectura Lambda. [Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=iotinsightssoln-docs-ercenk) es una excelente solución para crear las vistas de lote en la capa de servicio. Se trata de un servicio de integración de datos administrados basado en la nube que le permite crear flujos de trabajo orientados a datos en la nube a fin de coordinar y automatizar el movimiento y la transformación de datos. Con Azure Data Factory, puede crear y programar [flujos de trabajo basados en datos](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=iotinsightssoln-docs-ercenk) (llamados canalizaciones) que pueden ingerir datos de distintos almacenes de datos. Los datos se pueden procesar y transformar mediante servicios como [Azure HDInsight Hadoop](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk), [Spark](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk) y [Azure Databricks](/azure/azure-databricks/?WT.mc_id=iotinsightssoln-docs-ercenk). Esto le permite crear modelos de aprendizaje automático y usarlos con los clientes de análisis.

Por ejemplo, como se muestra en la ilustración siguiente, las canalizaciones de Data Factory leen datos desde el almacén de datos maestros. Una canalización resume y agrega los datos para completar una instancia de Azure Data Warehouse. La canalización de Data Factory también contiene [actividades de bloc de notas de Azure Databricks](/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=iotinsightssoln-docs-ercenk) que se usan para compilar modelos de aprendizaje automático.

![Arquitectura lambda: capa por lotes resaltada.](assets/extracting-insights-from-iot/master-data-to-ml-analytics.png)
  
[Azure SQL Database](/azure/sql-database/?WT.mc_id=iotinsightssoln-docs-ercenk) o [Azure SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=iotinsightssoln-docs-ercenk) son las mejores opciones para hospedar las vistas de lote. Estos servicios pueden servir vistas precalculadas y agregadas en los datos maestros. 

Azure SQL Database (SQL DB) es una base de datos como servicio relacional basada en la última versión estable del Motor de base de datos de Microsoft SQL Server. SQL DB es una base de datos de alto rendimiento, confiable y segura que puede usar para crear sitios web y aplicaciones controladas por datos. En tanto que servicio de Azure, no es necesario ocuparse de la administración de su infraestructura. A medida que aumenta el volumen de datos, la solución puede comenzar a utilizar técnicas para agregar y almacenar datos para acelerar las consultas. Las agregaciones calculadas previamente son una técnica bien conocida, especialmente para datos de solo anexar. También es útil para administrar los costos.

Azure SQL Data Warehouse ofrece muchas características adicionales que pueden ser útiles en algunos escenarios. Se trata de un almacenamiento de datos empresarial en la nube que aprovecha el procesamiento paralelo masivo para ejecutar rápidamente consultas complejas en petabytes de datos. Si necesita mantener petabytes de datos y desea consultas que se ejecuten rápidamente, se recomienda SQL Data Warehouse.

## <a name="visualizing-the-data"></a>Visualización de los datos

En esta capa, queremos fusionar las dos canalizaciones de datos (rutas de acceso activa e inactiva) para presentar una vista coherente de los datos. En este ejemplo, se usan varias métricas para deducir el uso de la máquina en las rutas de acceso activa e inactiva. En la fase de análisis, se proporcionan visualizaciones que combinan los datos de esas rutas de acceso.

![Arquitectura lambda: capa de los clientes de análisis resaltada.](assets/extracting-insights-from-iot/lambda-4.png)

[Microsoft Power BI ](/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk) y [Azure Time Series Insights](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk) proporcionan visualizaciones de datos listas para usar. Power BI es una solución de análisis de negocios que le permite visualizar sus datos y compartir los resultados en su organización, o insertarlos en su aplicación o sitio web. [Power BI Desktop](https://powerbi.microsoft.com/desktop/?WT.mc_id=iotinsightssoln-docs-ercenk) es una herramienta eficaz y gratuita para el modelado de informes y sus orígenes de datos subyacentes.  Las aplicaciones que incorporan visualizaciones de Power BI utilizan los informes creados por la herramienta de escritorio y hospedados en el servicio Power BI.

Time Series Insights tiene un explorador de datos para visualizar y consultar los datos, así como API de consulta REST. Además, expone una biblioteca de controles de JavaScript que permite incorporar gráficos con tecnología de TSI en aplicaciones personalizadas. La siguiente es una vista de mapa térmico básica en el TSI para los datos entrantes que se aproxima a la utilización de las máquinas de la planta, examinando simplemente el número de muestras observado.

![Arquitectura lambda: capa por lotes resaltada.](assets/extracting-insights-from-iot/client-screen.png)

Si necesita una interfaz de usuario basada en explorador que agrega datos de varios orígenes, los servicios TSI y Power BI permiten insertar controles de visualización. Ambos ofrecen también diferentes API REST ([API REST de Power BI](/rest/api/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk), [API REST de TSI](/rest/api/time-series-insights/time-series-insights-reference-queryapi?WT.mc_id=iotinsightssoln-docs-ercenk)) y SDK de JavaScript ([SDK de JavaScript de Power BI](https://github.com/Microsoft/PowerBI-JavaScript?WT.mc_id=iotinsightssoln-docs-ercenk), [SDK DE JavaScript de TSI](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk)), lo cual permite muchas personalizaciones.

## <a name="next-steps"></a>Pasos siguientes

Hemos tratado muchos conceptos y nos gustaría brindar al lector un conjunto de puntos de partida para aprender más y aplicar las técnicas adaptadas a sus propias necesidades. Aquí ofrecemos algunos tutoriales que creemos que pueden ser útiles para este propósito.

- Conversión de los datos a una secuencia
  - [Creating a Logic App running on a schedule](/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow?WT.mc_id=iotinsightssoln-docs-ercenk) (Creación de una aplicación de Logic Apps que se ejecuta según una programación)
  - [Ejemplos de código de operaciones de datos para Azure Logic Apps](/azure/logic-apps/logic-apps-data-operations-code-samples?WT.mc_id=iotinsightssoln-docs-ercenk)
  - La [ejecución de Azure Functions en un contenedor](/azure/azure-functions/functions-create-function-linux-custom-image?WT.mc_id=iotinsightssoln-docs-ercenk) para hospedar la función de Azure se trata en varios lugares. Creación de una función en Linux con una imagen personalizada, ejecución de funciones en cualquier plataforma, imágenes de Docker para Azure Functions Runtime
  - [Uso de varios enlaces en Azure Functions](/azure/azure-functions/functions-triggers-bindings?WT.mc_id=iotinsightssoln-docs-ercenk)

- Ruta de acceso activa
  - Tutoriales exhaustivos que demuestran el uso de Event Hubs, Azure Stream Analytics y Power BI. Vea [Tutorial: Visualización de anomalías de datos](/azure/event-hubs/event-hubs-tutorial-visualize-anomalies?WT.mc_id=iotinsightssoln-docs-ercenk) de eventos en tiempo real enviados a Azure Event Hubs y [Creación de un trabajo de Stream Analytics para analizar los datos de llamadas de teléfono](/azure/stream-analytics/stream-analytics-manage-job?WT.mc_id=iotinsightssoln-docs-ercenk) y visualizar los resultados en un panel de Power BI.
  -[Uso de Azure Cosmos DB con .NET](/azure/cosmos-db/sql-api-get-started?WT.mc_id=iotinsightssoln-docs-ercenk)
- Ruta de acceso inactiva
  - [Transformación de datos en la nube mediante una actividad de Spark en Azure Data Factory](/azure/data-factory/tutorial-transform-data-spark-portal?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Tutorial: Creación de un entorno de Azure Time Series Insights](/azure/time-series-insights/tutorial-create-populate-tsi-environment?WT.mc_id=iotinsightssoln-docs-ercenk)
- Clientes de análisis
  - [Aprendizaje de Power BI](/power-bi/guided-learning/?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Creación de una aplicación de página única de Time Series Insights](/azure/time-series-insights/tutorial-create-tsi-sample-spa?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Exploración de la biblioteca de cliente JavaScript de Time Series Insights](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk)
  - Consulte la [demostración de TSI](https://insights.timeseries.azure.com/demo) y la [demostración de Power BI](https://microsoft.github.io/PowerBI-JavaScript/demo/v2-demo/index.html).

## <a name="appendix-pillars-of-software-quality-posq"></a>Apéndice: Fundamentos de calidad del software (PoSQ)

Una buena aplicación en la nube se crea sobre estos cinco [fundamentos de calidad del software](/azure/architecture/guide/pillars?WT.mc_id=iotinsightssoln-docs-ercenk): Escalabilidad, disponibilidad, resistencia, administración y seguridad. En esta sección, trataremos brevemente los fundamentos para cada componente según sea necesario. No trataremos la disponibilidad, la resistencia, la administración y DevOps, ya que se abordan principalmente en el nivel de implementación, y queremos mencionar que la plataforma Azure proporciona varios medios para lograrlos por medio de API, herramientas, diagnóstico y registro. Además de los fundamentos mencionados, también se mencionará la rentabilidad.

Vamos a revisarlos rápidamente:

- La **escalabilidad** es la capacidad de un sistema para controlar el aumento de la carga. Una aplicación tiene fundamentalmente dos maneras de escalar. El escalado vertical implica el aumento de la capacidad de un recurso, por ejemplo mediante el uso de una máquina virtual de mayor tamaño. El escalado horizontal consiste en agregar nuevas instancias de un recurso, como máquinas virtuales o réplicas de base de datos. El fundamento de la escalabilidad también incluye el rendimiento y la capacidad de controlar la carga.
- La **disponibilidad** es la proporción de tiempo que un sistema es funcional y está en funcionamiento. Normalmente se mide como porcentaje del tiempo de actividad. Los errores de aplicación, los problemas de infraestructura y la carga del sistema pueden reducir la disponibilidad. Los contratos de nivel de servicio para los servicios de Microsoft Azure están publicados y disponibles en [Contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/?WT.mc_id=iotinsightssoln-docs-ercenk). La disponibilidad es la única métrica significativa a nivel del sistema. Los componentes por separado contribuyen a la disponibilidad general del sistema.
- La **resistencia** es la capacidad de un sistema de recuperarse de los errores y seguir funcionando. El objetivo de la resistencia es devolver la aplicación a un estado plenamente operativo después de un error. La resistencia está estrechamente relacionada con la disponibilidad.
- **DevOps y administración**. Este fundamento abarca los procesos de las operaciones que mantienen a una aplicación ejecutándose en producción. Las implementaciones deben ser confiables y predecibles. Se deben automatizar para reducir la posibilidad de que ocurran errores humanos. Deben ser un proceso rápido y rutinario, de manera que no ralenticen la publicación de nuevas características o correcciones de errores. Igualmente importante, debe ser capaz de revertir o poner al día la aplicación rápidamente en caso de que tenga problemas.
- La **seguridad** debe ser un punto de atención prioritario durante todo el ciclo de vida de una solución, desde su diseño e implementación hasta la implementación y las operaciones. La administración de identidades, la protección de su infraestructura, la seguridad de la aplicación, la autorización, la soberanía y el cifrado de datos y la auditoría son áreas amplias que deben abordarse.

## <a name="posq-converting-the-data-to-a-stream"></a>Fundamentos de calidad del software: Conversión de los datos a una secuencia

**Escalabilidad**: la escalabilidad se puede enfocar desde dos perspectivas. En primer lugar, desde la perspectiva del componente; en segundo lugar, desde la perspectiva del sistema que proporciona los datos de origen.

Cada servicio de Azure proporciona opciones para el escalado vertical y horizontal. Se recomienda encarecidamente tener en cuenta los requisitos de escalabilidad al diseñar la solución.

En cuanto a los sistemas que proporcionan los datos de origen, debemos tener cuidado de no sobrecargar el sistema y, básicamente, provocar un ataque de denegación de servicio (DoS) en este al consultarlo con demasiada frecuencia. Si está sondeando el sistema, debe tener en cuenta que ajustar la frecuencia de sondeo tiene dos efectos: la granularidad de los datos (cuanto más a menudo consulte, más se acercará al tiempo real) y la carga creada en el sistema remoto.

**Seguridad**: si se accede al sistema remoto mediante claves simétricas o asimétricas, se recomienda que los secretos se conserven en [Azure Key Vault](/azure/key-vault/?WT.mc_id=iotinsightssoln-docs-ercenk).

## <a name="posq-warm-path"></a>Fundamentos de calidad del software: Ruta de acceso activa

**Escalabilidad**: si se utiliza Azure Event Hubs en el subsistema de ingesta, el mecanismo principal de escalabilidad lo constituyen las [unidades de procesamiento](/azure/event-hubs/event-hubs-features#throughput-units?WT.mc_id=iotinsightssoln-docs-ercenk). Event Hubs ofrece la capacidad de establecer las unidades de rendimiento estáticamente o a través de la [característica de inflado automático](/azure/event-hubs/event-hubs-auto-inflate?WT.mc_id=iotinsightssoln-docs-ercenk).

Las [unidades de streaming](/azure/stream-analytics/stream-analytics-streaming-unit-consumption?WT.mc_id=iotinsightssoln-docs-ercenk) en Stream Analytics representan los recursos informáticos que se asignan para ejecutar un trabajo. Cuanto mayor sea el número de unidades de streaming, más recursos de CPU y memoria se asignarán al trabajo. Esta función le permite centrarse en la lógica de consulta y abstrae la necesidad de administrar el hardware para ejecutar el trabajo de Stream Analytics de manera oportuna. Además de las unidades de streaming, es crucial hacer un uso eficiente de estas a través de [la paralelización adecuada de las consultas ](/azure/stream-analytics/stream-analytics-scale-jobs?WT.mc_id=iotinsightssoln-docs-ercenk).

Las implementaciones de Azure Cosmos DB se deben aprovisionar con los parámetros de rendimiento adecuados y el diseño de la partición pertinente. El aprovisionamiento del rendimiento está disponible en el contenedor o en el nivel de la base de datos, y se expresa en [unidades de solicitud ](/azure/cosmos-db/request-units?WT.mc_id=iotinsightssoln-docs-ercenk). Cosmos DB proporciona una herramienta para calcular las unidades de solicitud. Además de aprovisionar el rendimiento, es fundamental [crear particiones eficientes de la base de datos](/azure/cosmos-db/partition-data?WT.mc_id=iotinsightssoln-docs-ercenk).

**Seguridad**: el acceso a Azure Event Hubs mediante clientes se realiza a través de una combinación de tokens de firma de acceso compartido (SAS) y publicadores de eventos para la autenticación de clientes. La seguridad para aplicaciones de back-end sigue los mismos conceptos que los temas de Service Bus. Para obtener una descripción detallada del modelo de seguridad de Event Hubs, consulte [Introducción al modelo de autenticación y seguridad de Event Hubs](/azure/event-hubs/event-hubs-authentication-and-security-model-overview?WT.mc_id=iotinsightssoln-docs-ercenk).

La protección de las bases de datos de Cosmos DB proporciona acceso controlado a los datos y cifrado en reposo. Para más información, consulte [Seguridad de base de datos de Azure Cosmos DB](/azure/cosmos-db/database-security?WT.mc_id=iotinsightssoln-docs-ercenk).

**Rentabilidad**: el precio de Event Hubs depende de la SKU (estándar o premium) y los millones de eventos recibidos, más las unidades de procesamiento. La combinación óptima se puede lograr mediante la observación de la tasa de ingesta de datos que dictan los mensajes entrantes.

Al usar Cosmos DB, recomendamos observar el uso más óptimo del almacén a través de la utilización de unidades de solicitud. Cosmos DB también tiene una característica para controlar la retención de datos; como se mencionó anteriormente, recomendamos usarla para controlar el tiempo que permanecen los registros en la base de datos, controlando así su tamaño.

## <a name="posq-cold-path"></a>Fundamentos de calidad del software: Ruta de acceso inactiva

**Escalabilidad**: Azure Time Series Insights (TSI) se escala con una métrica denominada "capacidad", que es un multiplicador aplicado a la velocidad de entrada, la capacidad de almacenamiento y el costo asociado con la SKU. 

Azure Time Series Insights tiene varias SKU que también repercuten directamente en su escalado vertical. Consulte el documento [Planee el entorno de Azure Time Series Insights](/azure/time-series-insights/time-series-insights-environment-planning?WT.mc_id=iotinsightssoln-docs-ercenk) para obtener más información sobre el escalado. Al igual que muchos otros servicios de Azure, el TSI también está sujeto a limitación para evitar el problema del "vecino ruidoso". Un "vecino ruidoso es una aplicación de un entorno compartido /azure/sql-database/sql-database-service-tiers-vcore?WT.mc_id=iotinsightssoln-docs-ercenk que monopoliza los recursos y priva de estos a los demás usuarios. Consulte la [documentación de TSI](/azure/time-series-insights/time-series-insights-environment-mitigate-latency?WT.mc_id=iotinsightssoln-docs-ercenk) para administrar la limitación. 

Los objetivos de escalabilidad de las cuentas de almacenamiento se documentan en [Objetivos de escalabilidad y rendimiento de Azure Storage](/azure/storage/common/storage-scalability-targets?WT.mc_id=iotinsightssoln-docs-ercenk). Una técnica común para almacenar datos superando la capacidad de una sola cuenta de almacenamiento consiste en crear particiones entre varias cuentas de almacenamiento.

Azure SQL Database presenta muchas opciones para administrar la escalabilidad, tanto vertical como horizontalmente, según el modelo de compra ([basado en DTU](/azure/sql-database/sql-database-service-tiers-dtu?WT.mc_id=iotinsightssoln-docs-ercenk) y el núcleo virtual). Se recomienda emprender una mayor investigación para encontrar la mejor opción para una solución futura siguiendo la [documentación de SQL Database](/azure/sql-database/sql-database-scale-resources?WT.mc_id=iotinsightssoln-docs-ercenk) sobre el tema.

**Seguridad**: los entornos TSI proporcionan [directivas de acceso](/azure/time-series-insights/time-series-insights-data-access?WT.mc_id=iotinsightssoln-docs-ercenk) para el acceso de administración y el acceso a datos independientes entre sí. No hay una forma directa de agregar datos a un entorno TSI que no sean los orígenes de datos definidos. Las directivas de acceso de administración conceden permisos relacionados con la configuración del entorno. Las directivas de acceso a datos conceden permisos para emitir consultas de datos, manipular datos de referencia en el entorno y compartir consultas guardadas y perspectivas asociadas con el entorno.

El servicio Azure Data Factory proporciona varios métodos para proteger las credenciales del almacén de datos, ya sea en su almacén administrado o en Azure Key Vault. El cifrado de datos en tránsito depende de transporte del almacén de datos (por ejemplo, HTTPS o TLS). El cifrado de datos en reposo también depende de los almacenes de datos. Consulte [Consideraciones de seguridad para el movimiento de datos en Azure Data Factory](/azure/data-factory/data-movement-security-considerations?WT.mc_id=iotinsightssoln-docs-ercenk) para obtener más información.

SQL Database proporciona un amplio conjunto de características de seguridad para el acceso a datos, la supervisión y la auditoría, así como el cifrado de datos en reposo. Para obtener más información, consulte [Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database?WT.mc_id=iotinsightssoln-docs-ercenk).

**Rentabilidad**: en el centro de cualquier solución de análisis se encuentra el almacenamiento. Los motores de análisis necesitan velocidad, eficiencia, seguridad y rendimiento para procesar volúmenes de datos en tiempos razonables. El diseño de mecanismos para aprovechar al máximo la plataforma subyacente, mediante la agregación y el resumen de datos, y el uso eficiente de los almacenes políglotas son los medios para administrar los costos de manera eficiente. Como Azure es una plataforma en la nube, existen métodos para retirar, reponer y cambiar el tamaño de los recursos mediante programación. Por ejemplo, la [operación de creación o actualización](/rest/api/sql/databases/createorupdate?WT.mc_id=iotinsightssoln-docs-ercenk) proporciona una manera de cambiar el tamaño de la base de datos de Azure SQL Database.