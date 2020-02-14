---
title: Mantenimiento predictivo con Azure Machine Learning e IoT en los procesos de fabricación
author: ercenk
ms.author: ercenk
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Descripción de la solución sobre cómo desarrollar el mantenimiento predictivo para los clientes de fabricación en Azure.
ms.openlocfilehash: c32893d534279cda35f7c6a142869d2983eaca67
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053851"
---
# <a name="predictive-maintenance-in-manufacturing-solution-guide"></a>Guía de soluciones sobre el mantenimiento predictivo en la fabricación


## <a name="introduction"></a>Introducción

El mantenimiento predictivo usa una combinación de sensores, inteligencia artificial y ciencia de datos para optimizar el mantenimiento de equipos. La previsión de las necesidades de mantenimiento de equipos para minimizar los costos de mantenimiento y maximizar el tiempo de actividad proporciona un valor significativo a los fabricantes.

Los datos constituyen el núcleo de la solución. Los datos deben tener los indicadores de error adecuados, así como otros aspectos que describen el contexto. Puede proceder de varios orígenes, tales como sensores, registros de máquina y registros de aplicación de fabricación.

En este artículo se presentan opciones para crear una solución de mantenimiento predictivo. Presenta diversas perspectivas y materiales existentes de referencia para que pueda comenzar. Los requisitos para una solución de mantenimiento predictivo varían por equipo, entorno, proceso y organización; procuramos proporcionar enfoques y tecnologías alternativos para guiarle en el proceso de conseguir una solución para sus necesidades.

Comencemos con los componentes de alto nivel de una solución de mantenimiento predictivo.


![Solución de alto nivel](./assets/pdm-assets/highlevelsolution.png)

En este desglose, se producen las siguientes actividades de alto nivel:

1. Recolectar datos de aprendizaje, incluidos los datos del error

2. Con estos datos de aprendizaje, entrene un modelo de Machine Learning (ML) para predecir errores del recurso futuros una vez especificado un conjunto de condiciones

3. Seguir recopilando datos de forma continua

4. Ingrese los datos recopilados en el modelo de ML, el cual predecirá el error, normalmente con cierto nivel de confianza (por ejemplo: “existe una probabilidad del 85% de que la máquina produzca un error en las próximas 24 horas”)

5. Hacer emerger los casos de error previstos

6. Planear y reaccionar a la información vista en los datos

## <a name="training-the-ml-model"></a>Entrenamiento del modelo de ML

La creación de un modelo de ML requiere datos suficientes, correctos y completos. Además, el mantenimiento predictivo supone desafíos únicos, siendo uno importante la disponibilidad de datos del error. Los errores suelen ser eventos relativamente raros, especialmente en el caso del equipo de capital elevado como, por ejemplo, máquinas CNC o componentes de refinerías de petróleo; así pues, si hemos recopilado datos del sensor durante un prolongado período de tiempo, es posible que no tengamos suficientes datos del error. Tenga en cuenta la definición de “error“; ¿qué es lo que constituye un error exactamente? ¿Cuando el dispositivo deja de funcionar de forma inesperada?
¿Cuando el dispositivo se degrada hasta alcanzar un nivel en el cual ya no rinde al nivel deseado? ¿Es el caso de error una máquina vanguardista que se destruye a causa de un error de componente provocado por fatiga de metal u otros indicadores que señalan un error antes de producirse una catástrofe?

## <a name="considering-the-data-needed-for-ml"></a>Consideración de los datos necesarios para ML

Tenga también en cuenta si capturamos datos suficientes para registrar estos errores correctamente. En muchos casos, es posible que solo los datos del sensor no sean suficientes para identificar un error. A veces puede que necesitemos datos externos para “etiquetar” el estado de una máquina como estado de error, o bien una fuente secundaria de información, como un operador que captura el caso de error a través de un sistema diferente. Estos datos pueden residir en sistemas externos como ERP, sistemas de ejecución de fabricación (MES), historiadores, etc., y cruzar la infame división entre TI y OT tan predominante en las empresas de fabricación, lo que hace que proteger los datos necesarios resulte aún más desafiante.

Por naturaleza, el mantenimiento predictivo es un problema dinámico y, como tal, los modelos de Machine Learning asociados deben actualizarse continuamente (o volverse a entrenar). Si se realiza correctamente, el mantenimiento predictivo debe reducir las instancias de errores (esto es bueno, pero da lugar a menos datos del error). Además, las características que afectan al error pueden cambiar la invalidación anterior a los modelos de Machine Learning. Recomendamos el entrenamiento periódico de los modelos con cualquier cambio en las condiciones de error.

“Actualizar” los datos también supone la llegada de nuevas condiciones en el modelo distintas de las usadas anteriormente para entrenar el modelo. Es decir, podemos modelar el error como la función de las variables _x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>n</sub>, f(x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>n</sub>)_ , pero finalmente podemos descubrir que las variables _x<sub>(n+1)</sub>,⋯,x<sub>(m+n)</sub>_ también influyen en el error, de modo que debemos modificar nuestro entrenamiento del modelo para _f(x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>(m+n)</sub>)_ . Es posible que el modelo no rinda bien a la hora de detectar los errores y que se cree un nuevo modelo incluyendo también puntos de datos de los registros MES de la máquina para la iteración siguiente.

Incluso sin un entorno de IoT moderno que transmita datos a la nube, es posible que los datos necesarios para entrenar los modelos de Machine Learning se encuentren ya en sus MES, historiadores u otros sistemas de producción. Es solo cuestión de preparar los datos de modo que se puedan usar para entrenar los modelos de Machine Learning.

En la siguiente ilustración se muestra el flujo de trabajo típico para entrenar un modelo de ML. La flecha etiquetada “repeat” sugiere que se trata de un proceso iterativo, uno en el cual volvemos a entrenar continuamente los modelos a medida que llegan datos nuevos y que las condiciones cambian. Cuándo debe repetirse este bucle y con qué frecuencia depende de las condiciones específicas de la implementación y requiere una supervisión cuidadosa del rendimiento de los modelos creados anteriormente prediciendo errores para detectar cuándo “envejecen” o “se degradan” los modelos.

Entrenar nuevos modelos continuamente e implementarlos da lugar al desafío de administrarlos. Microsoft ofrece el servicio Administración de modelos de Azure Machine Learning para la CI/CD (integración continua y entrega continua) de los modelos.

![Fases de creación de modelos de ML](assets/pdm-assets/mlmodelbuildingstages.png)

Microsoft ha publicado [una guía detallada](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk) sobre cómo preparar los datos y entrenar el modelo de Machine Learning. Existen tres preguntas de mantenimiento típicas y los algoritmos de Machine Learning relacionados:

- _Para el recurso, ¿cuál es la probabilidad de que ocurra un error en las próximas X horas?_ Respuesta: 0-100%
  - **Clasificación binaria:** la clasificación binaria es un método de Machine Learning que usa datos para determinar la categoría, el tipo o la clase de un elemento o fila de datos, como un miembro de una de las dos clases. Hay varios tipos de algoritmos de clasificación. Microsoft publicó un conjunto de algoritmos disponibles como [módulos de Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-classification?WT.mc_id=pdmsolution-docs-ercenk).
- _¿Cuál es la vida útil restante del recurso?_ Respuesta: X horas
  - **Regresión:** la regresión es una clase de algoritmos de Machine Learning que predice el valor de una variable una vez especificado un conjunto de otras variables. Machine Learning Studio incluye un conjunto de algoritmos de regresión como [módulos](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-regression?WT.mc_id=pdmsolution-docs-ercenk).
    - **Memoria a corto plazo (LSTM):** las redes [LSTMs](https://colah.github.io/posts/2015-08-Understanding-LSTMs/?WT.mc_id=pdmsolution-docs-ercenk) son un tipo de redes neuronal profundas (DNN). La inspiración de las DNN viene del modelado del comportamiento de las neuronas individuales en el cerebro. Microsoft ha publicado una [guía paso a paso](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk) para describir cómo se usa una LSTM para el mantenimiento predictivo
- _¿Qué recurso requiere un mantenimiento más urgente?_ Respuesta: Recurso X
  - **Clasificación multiclase:** la clasificación multiclase es un método de Machine Learning que usa datos para determinar la categoría, el tipo o la clase de un elemento o fila de datos, como un miembro de más de dos clases.

Una vez más, la incorporación de los datos puede conllevar el uso de varios canales y la inicialización de los mismos de forma masiva en primer lugar para, a continuación, seguir recibiendo datos de streaming para predecir errores, además de usarlos para posteriores compilaciones del modelo.

## <a name="bringing-data-to-azure"></a>Traer datos a Azure

Microsoft Azure ofrece numerosos servicios para ingerir y almacenar los datos. Recomendamos los métodos por lotes para obtener los datos transferidos a Azure si aún no están allí. Si puede exportar sus datos como archivos en formatos conocidos como csv, json, xml, etc., estas opciones son buenas. También puede elegir comprimirlos antes de cargarlos y procesarlos adicionalmente en el lado de la nube.

- Cargar mediante [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?WT.mc_id=pdmsolution-docs-ercenk) en Blob Storage (Windows y Linux)

- [Montaje de Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux?WT.mc_id=pdmsolution-docs-ercenk) como sistema de archivos en Linux

- Uso del [servicio Import/Export](https://docs.microsoft.com/azure/storage/common/storage-import-export-service?WT.mc_id=pdmsolution-docs-ercenk), si el tamaño de los datos es grande y tarda demasiado tiempo en cargarse

- [Montaje de un recurso compartido de archivos de](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows?WT.mc_id=pdmsolution-docs-ercenk) Azure en Windows, Linux y MacOS

Si los datos están en una base de datos de SQL Server, también puede usar [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?WT.mc_id=pdmsolution-docs-ercenk) para cargar los datos en una base de datos de Azure SQL.

Existe una variedad de herramientas y servicios en la plataforma Azure para las operaciones de extracción, transformación y carga de datos (ETL). El servicio más importante es [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/?WT.mc_id=pdmsolution-docs-ercenk), que proporciona un conjunto completo de características para manipular los datos. Otras opciones para manipular los datos están presentes en los muchos servicios de ML disponibles en Azure a través de las bibliotecas de código abierto.

En cuanto al entrenamiento del modo ML, Microsoft Azure proporciona muchas opciones, las cuales pueden usarse combinadas de distintas formas.

- [Servicios de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/?WT.mc_id=pdmsolution-docs-ercenk)

- [Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/?WT.mc_id=pdmsolution-docs-ercenk)

- [MLLib de Spark en HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-machine-learning-mllib-ipython?WT.mc_id=pdmsolution-docs-ercenk)

- [Servicio de entrenamiento de IA por lotes](https://docs.microsoft.com/azure/batch-ai/?WT.mc_id=pdmsolution-docs-ercenk)

Decidir qué herramienta usar depende de la complejidad de las operaciones, la experiencia del equipo y el tamaño de los datos.

La ecuación de costo en las soluciones en la nube contiene muchas variables, además de los costos del servicio en la nube, como la ingeniería, la administración, las transferencias de datos, etc. adicionales. Use estas variables al evaluar el costo y tome una decisión informada. Los servicios solo no constituyen la ecuación de costos totales.

Tanto el diseño del proceso para analizar los datos como la publicación del modelo son temas detallados y se diferencian entre sí por las tecnologías usadas. Esos temas se encuentran fuera del ámbito de este artículo. Una serie de artículos donde se explica el proceso y los servicios de Azure que pueden usarse en la generación del modelo se encuentran disponibles. Microsoft también proporciona un enfoque sistemático para crear soluciones que permite a los equipos de científicos de datos colaborar de forma eficaz durante el ciclo de vida de los datos.

La [documentación de Machine Learning](https://docs.microsoft.com/azure/machine-learning?WT.mc_id=pdmsolution-docs-ercenk) de Microsoft es un buen punto inicial para explorar las opciones para crear, implementar y administrar modelos de ML e IA en la nube.

La plataforma de Microsoft Azure ofrece numerosas opciones para procesar datos a escala y crear modelos de ML. La disponibilidad de un proceso escalable y prácticamente infinito, y una potencia de almacenamiento en las plataformas de nube permite la creación de modelos de ML e IA. Por tanto, usar los servicios de Azure para crear los modelos es la opción más lógica para implementar este flujo de datos.

## <a name="using-the-model"></a>Uso del modelo

Una vez que tenemos un modelo de ML, necesitamos un mecanismo para consumirlo (o “usarlo”) a fin de predecir la necesidad de mantenimiento del equipo. Tras recibirse los datos desde el equipo, se mueve a través de una capa de procesamiento para predecir casos de error futuros y presentar diversos medios para que los equipos de mantenimiento actúen.

![Uso del modelo](assets/pdm-assets/usingthemodel.png)

La ingesta de los datos puede llevarse a cabo en línea transmitiendo los datos del sensor en directo en la solución, o bien sin conexión importando los datos del sensor de forma periódica en la solución.

La plataforma de Microsoft Azure ofrece una variedad de servicios para ingerir, procesar y almacenar los datos, por ejemplo:

- [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub?WT.mc_id=pdmsolution-docs-ercenk)

- [Apache Kafka para HDInsight](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=pdmsolution-docs-ercenk)

A diferencia del proceso de creación del modelo de ML, su consumo no requiere muchos recursos computacionales. Dependiendo de sus necesidades, el modelo se puede implementar en un servicio en la nube, o bien de forma local en la fábrica.

Existen dos alternativas principales para el lugar donde se ejecuta el modelo de ML, de forma local o solo en la nube.

## <a name="local-execution"></a>Ejecución local

El modelo de ML se consume localmente, mientras los datos se envían a la nube para su ingesta, almacenamiento y procesamiento adicional. Esta opción se adapta bien a escenarios en los cuales la detección temprana es esencial.

![Ejecución local](assets/pdm-assets/localandcloud.png)

## <a name="cloud-execution"></a>Ejecución local

La ingesta, el proceso y almacenamiento, y la ejecución del modelo de ML pueden producirse en la nube de Azure. Esta opción puede ser más adecuada en los casos en los que se comparten los resultados de la ejecución del modelo de ML entre varios inquilinos o ubicaciones geográficas (y la latencia no es fundamental). Un componente opcional, conocido a menudo como “puerta de enlace perimetral”, se puede agregar localmente para realizar parte del trabajo (como la agregación y proyección de datos, Stream Analytics, etc., siguiendo un patrón conocido como [patrón “Ambassador”](https://docs.microsoft.com/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk)).

Hay varias formas de usar el modelo en Azure. El [servicio web Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio/consume-web-services?WT.mc_id=pdmsolution-docs-ercenk) es el más sencillo y usa [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=pdmsolution-docs-ercenk) como opción para crear el modelo. También podría elegirse [Administración de modelos de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview?WT.mc_id=pdmsolution-docs-ercenk), que proporciona un completo conjunto de servicios para administrar modelos, así como puntos de conexión de la API de REST, con características de autenticación, equilibrio de carga, escalabilidad horizontal automática y cifrado. El modelo se puede implementar en una sola máquina (p. ej., Data Science Virtual Machine, un dispositivo IoT o un equipo local) o [Azure Container Service](https://docs.microsoft.com/azure/aks/intro-kubernetes?WT.mc_id=pdmsolution-docs-ercenk). Una vez expuesto el modelo a través de una API de REST, las posibilidades de usarlo son infinitas, desde aplicaciones personalizadas a la integración de soluciones empresariales.

![Solo en la nube](assets/pdm-assets/cloudonly.png)

Una implementación solo en la nube no conlleva necesariamente que haya solo streaming de datos en directo. Una posible estrategia consiste en exportar datos de forma periódica desde un sistema local (por ejemplo, un historiador o MES), importarlos en el sistema de nube y presentar el resultado. Esta opción puede ser un enfoque factible si los dispositivos no pueden insertar datos directamente en el sistema, los sistemas existentes recopilan datos ya o ningún procesamiento de datos en tiempo casi real es necesario. En estos casos, no es necesario tener en cuenta una puerta de enlace perimetral.

## <a name="predictive-maintenance-in-the-iot-context"></a>Mantenimiento predictivo en el contexto de IoT

Muchas soluciones de IoT ingieren y almacenan datos como parte de su conjunto de características. Además, como las soluciones de mantenimiento predictivo suelen basarse en datos de IoT, pueden constituir una adición de características natural a las soluciones de IoT. Un punto esencial que se debe resaltar en este contexto es la importancia de tener errores registrados en los datos existentes para entrenar un modelo predictivo para las características.

Algunos casos de uso requieren un procesamiento de datos en tiempo casi real. En estos casos, necesitamos una solución de IoT escalable con funcionalidades de tasa de ingesta de datos elevada. La plataforma de Microsoft Azure ofrece muchos servicios que pueden habilitar soluciones para aquellas necesidades de IoT altamente escalables. La [arquitectura de solución IoT de Microsoft](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-is-azure-iot?WT.mc_id=pdmsolution-docs-ercenk) de la plataforma Azure cuenta con componentes lógicos a lo largo de tres fases:

- Conectividad de dispositivos

- Procesamiento de datos y análisis

- Presentación

![Arquitectura de solución IoT](assets/pdm-assets/iot.png)

Los detalles de la arquitectura de solución IoT de Azure están [disponibles en línea](https://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf?WT.mc_id=pdmsolution-docs-ercenk).
Sin embargo, hay desafíos únicos que pueden aparecer debido al número potencialmente sustancial de dispositivos conectados a los servicios de back-end.

## <a name="data-ingestion-and-stream-processing"></a>Ingesta de datos y procesamiento de flujos

La incorporación de datos de los dispositivos constituye un problema de comunicación entre dos servicios independientes; es decir, sistemas que generan datos (dispositivos) y sistemas que procesan estos datos (es decir, entrenan el modelo de ML, ejecutando los puntos de datos entrantes en el modelo entrenado para predecir la vida útil restante).

Los sistemas distribuidos, por definición, constan de diversos componentes con una necesidad inherente de comunicarse entre sí. Una opción para permitir la comunicación puede consistir en contar con componentes relacionados que se comunican directamente entre sí. De este modo se crea un sistema que resulta difícil de mantener y escalar. A medida que aumente el número de componentes, creará la complejidad _O(n<sup>2</sup>)_ de los vínculos de comunicación. Un enfoque mejor es aquel en que se publican y se leen los datos en y desde un centro de conectividad común.

![Comunicación de subcomponentes](./assets/pdm-assets/subcomponentcommunication.png)

Insertar un nuevo componente para la ingesta de datos hace que la comunicación resulte más escalable. Este componente debe ser escalable, seguro y muy probablemente accesible a nivel global, con la opción de crear particiones del proceso de ingesta de datos geográficamente. 

Considere que el mantenimiento predictivo es una característica de la solución de IoT. Igual que los flujos de datos a través de la puerta de enlace, debe enrutarse a servicios relacionados con la funcionalidad de mantenimiento predictivo.
Otro patrón que se debe tener en cuenta es [Enrutamiento de puerta de enlace](https://docs.microsoft.com/azure/architecture/patterns/gateway-routing?WT.mc_id=pdmsolution-docs-ercenk).

Ambos patrones pueden implementarse mediante el servicio de Azure, [IoT Hub](https://azure.microsoft.com/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk) y [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/?WT.mc_id=pdmsolution-docs-ercenk).

## <a name="edge-and-cloud-processing-cooperation"></a>Cooperación de procesamiento perimetral y en la nube

No todos los dispositivos ni todo el equipo pueden tener acceso a Internet de forma directa y coherente.
A veces, sus datos deben extraerse desde una puerta de enlace común. Por ejemplo, los agentes [MTConnect](https://www.mtconnect.org/) solo proporcionan una interfaz REST para extraer los datos.

Puede haber otras consideraciones como la latencia, la necesidad de limpiar los datos del dispositivo localmente antes de enviarlos a la nube (casos de multiinquilino) y la necesidad de realizar proyecciones o agregaciones en los datos del dispositivo. El [patrón Ambassador](https://docs.microsoft.com/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk) es un buen enfoque para abordar estas necesidades. [Microsoft Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works?WT.mc_id=pdmsolution-docs-ercenk) es una implementación que puede actuar como proxy para [Microsoft Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk), así como proporcionar funcionalidades de procesamiento local con la administración remota.

Una implementación común puede incluir alertas en tiempo casi real en la tienda, mientras se siguen limpiando y publicando datos en una solución multiinquilino en la nube para el archivado, el entrenamiento del modelo y la creación de informes sin dependencia del tiempo. Con las funcionalidades de Azure IoT Edge e IoT Hub, los clientes pueden controlar las opciones de filtrado de datos en el dispositivo perimetral, así como interactuar con otros sistemas de tienda para entregar alertas.

![Multiinquilino](assets/pdm-assets/multitenant.png)

## <a name="multitenant-perspective"></a>Perspectiva multiinquilino

Como se mencionó anteriormente, es posible que algunos fabricantes o terceros quieran ofrecer servicios de mantenimiento predictivo a sus clientes. Estos servicios se ofrecerán con toda probabilidad en implementaciones en la nube multiinquilino, que presentan su propio conjunto de desafíos:

### <a name="data-security-and-isolation"></a>Aislamiento y seguridad de los datos

La entidad que proporciona un servicio debe garantizar que la información confidencial de sus clientes se identifica y se protege o limpia adecuadamente. Microsoft Azure proporciona funciones para cifrar los datos en función del servicio de almacenamiento utilizado.

El modo en que los dispositivos generan y envían datos también debe protegerse, mediante métodos conocidos tales como certificados por dispositivo, habilitación/deshabilitación por dispositivo, seguridad TLS, compatibilidad con X.509, listas blanca y negra de direcciones IP y políticas de acceso compartido. La entidad que proporciona un servicio debe garantizar que la información confidencial de los clientes se identifica y se protege o limpia adecuadamente. [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Cosmos dB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest?WT.mc_id=pdmsolution-docs-ercenk)y [Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?WT.mc_id=pdmsolution-docs-ercenk) son ejemplos de servicios que se pueden usar para cifrar los datos en reposo. Los proveedores de soluciones también deben tener en cuenta cómo crear particiones de los datos en el mismo recurso (p. ej., base de datos) o en varios. 

### <a name="geographical-considerations"></a>Consideraciones geográficas

Lo más probable es que los dispositivos que generan datos se dispersen geográficamente. La solución debe proporcionar un punto de ingesta más cercano al origen de datos. También puede haber veces en que la conectividad continua constituya un problema, los datos tengan que ingerirse de forma masiva o un mecanismo de guardar y reenviar deba estar presente.

### <a name="scalability"></a>Escalabilidad

La creación de los modelos de ML requiere recursos de proceso que puedan escalarse de forma elástica.
El proveedor de soluciones debe diseñar procesos que usen eficazmente los recursos de proceso y los procesos para escalar la solución a petición.

### <a name="provisioning-tenants-and-secure-access"></a>Aprovisionamiento de los inquilinos y acceso seguro

El proveedor de servicios debe diseñar métodos para incorporar eficazmente nuevos inquilinos y proporcionarles los medios a fin de que administren sus cuentas por sí mismos. Este también es el momento en el que se toman las decisiones relativas a las implementaciones en recursos exclusivos o compartidos.


## <a name="pillars-of-software-quality-review"></a>Fundamentos de revisión de calidad del software 

Los sistemas complejos requieren atención adicional más allá del cumplimiento de los requisitos funcionales. Las soluciones correctas en la nube se centran en estos cinco fundamentos: la escalabilidad, la disponibilidad, la resistencia, la administración y la seguridad. Además de los cinco fundamentos, también deseamos destacar la eficacia de la solución en cuanto al costo.

Consulte el artículo [Fundamentos de calidad del software](https://docs.microsoft.com/azure/architecture/guide/pillars?WT.mc_id=pdmsolution-docs-ercenk) para ver los detalles.

| Fundamento                      |                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Escalabilidad                 | La mayoría de los servicios de Azure admiten las opciones de escalado vertical y horizontal. Aproveche la implementación a petición de recursos en la plataforma Azure y controle su escala (tamaño y número de instancias) a través de servicios automatizados.                                                                                                                                                                   |
| Disponibilidad y resistencia | Los recursos de proceso y almacenamiento se pueden aprovisionar según sea necesario haciendo uso, de forma elástica, de muchos servicios de Azure. Todos los servicios de Azure proporcionan diversos niveles de Acuerdos de Nivel de Servicio. Sin embargo, las soluciones deben tener en cuenta y usar los niveles de Acuerdos de Nivel de Servicio según corresponda utilizando los principios de diseño adecuados.                                                                                                                    |
| Administración                  | Los recursos de Azure deben implementarse y administrarse a través de distintos medios, tales como plantillas ARM, herramientas de línea de comandos y cmdlets de PowerShell, así como API de administración de Azure. Considere la posibilidad de crear soluciones automatizadas al administrar recursos de Azure, en lugar de usar herramientas con interfaces de usuario.                                                                                                                                |
| Seguridad                    | Azure IoT Hub admite claves simétricas y asimétricas (certificados X509 y TPM) a través de TLS. Los almacenes de datos están protegidos con la configuración de Administración de identidad y acceso (IAM) y también admiten el cifrado de datos en reposo. Como lista de comprobación de seguridad de alto nivel general, considere la posibilidad de revisar la autorización, autenticación, transporte y cifrado en reposo, además de los mecanismos de auditoría. |
| Rentable              | Considere la posibilidad de aprovisionar recursos cuando sea necesario y descartarlos cuando no se usen mediante la automatización.                                                                                                                                                                                                                                                                                                  |

## <a name="conclusion"></a>Conclusión

El mantenimiento predictivo ha sido un tema de debate durante mucho tiempo. Recientes avances en las plataformas de nube, como Microsoft Azure, permiten que los implementadores del mantenimiento predictivo superen muchos retos que en el pasado eran obstáculos a la hora de trabajar con los datos. Con la escala elástica de la capacidad de proceso y almacenamiento, las plataformas de nube presentan nuevas oportunidades para implementar el mantenimiento predictivo, junto con nuevas oportunidades de ingresos. La plataforma de Microsoft Azure proporciona muchos servicios de diferentes funcionalidades para lograr los objetivos empresariales de una solución de mantenimiento predictivo.

En este artículo se proporcionó una visión de cómo recopilar datos y entrenar modelos de datos, junto con la utilización del modelo entrenado para realizar acciones en relación a los resultados previstos en las secciones anteriores.

## <a name="further-reading"></a>Lecturas adicionales

1. [Centrado en el futuro: deje de pensar en el pasado y deje atrás los imprevistos con IoT](https://blogs.microsoft.com/iot/2017/02/28/future-focused-stop-thinking-in-the-past-and-get-ahead-of-the-unexpected-with-iot-2/?WT.mc_id=pdmsolution-docs-ercenk)

2. [Aumente la confiabilidad de los equipos con mantenimiento predictivo compatible con IoT](https://www.microsoft.com/internet-of-things/predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk)

3. [Capture el valor del Internet de las cosas: cómo enfocar un proyecto de mantenimiento predictivo](https://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF?WT.mc_id=pdmsolution-docs-ercenk)

4. [Perspectivas de los partners: mantenimiento predictivo en la primera línea](https://blogs.microsoft.com/iot/2017/03/21/partner-perspectives-predictive-maintenance-on-the-frontlines/?WT.mc_id=pdmsolution-docs-ercenk)

5. [Desde la mercantilización a la servitización: transformación de su empresa para competir en la nueva era del servicio de campo con IoT](https://blogs.microsoft.com/iot/2016/11/07/from-commodization-to-servitization-transforming-your-business-to-compete-in-the-new-age-of-field-service-with-iot/?WT.mc_id=pdmsolution-docs-ercenk)
