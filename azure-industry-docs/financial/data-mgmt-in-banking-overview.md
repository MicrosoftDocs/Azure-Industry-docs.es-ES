---
title: Introducción a la administración de datos en la banca
author: dstarr
ms.author: dastarr
ms.date: 05/21/2018
ms.topic: article
ms.service: industry
description: Describe las técnicas de administración de datos en un entorno bancario regulado mediante Microsoft Azure.
ms.openlocfilehash: 69b0abfc4908431397e47752bcbe56a440703406
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654252"
---
# <a name="data-management-in-banking-overview"></a>Introducción a la administración de datos en la banca

## <a name="introduction"></a>Introducción

En la actualidad, los bancos tienen la responsabilidad de proteger y almacenar enormes cantidades de información valiosa dentro de sus firewalls, tanto sobre sus clientes como sobre el cambiante panorama financiero. En muchos casos, esa información no se utiliza porque no es fácil acceder a ella ni explorarla, incluso cuando el uso de los datos podría mejorar el proceso de toma de decisiones en varias actividades bancarias.

Con estos datos, los bancos podrían encontrar más rápido información sobre quién está en riesgo de incurrir en el no pago de un crédito o para decidir qué ajustes de valoración de cartera se necesitan. Los bancos también podrían tener una visión más clara de cómo se almacenan y administran sus datos para cumplir con los requisitos reglamentarios, para que esos datos se puedan aprovechar, conservar, archivar o eliminar a fin de cumplir las normativas.

Con miles de decisiones, tanto grandes como pequeñas, necesarias para cumplir con los requisitos de funciones bancarias cada día, los datos resultan cada vez más importantes. Y no solo eso: dados los requisitos reglamentarios estrictos y las obligaciones por concepto de delitos financieros, los bancos necesitan tener la capacidad de auditar los resultados de cualquier proceso de análisis de datos, hasta la llegada de información inicial a un repositorio de datos. La rastreabilidad requiere de transparencia desde la ingesta hasta la producción de datos útiles.

Con el fin de administrar todas las cuentas o negocios que los bancos atienden, deben comprender todos estos datos de manera rápida y rentable. A medida que los bancos alcanzan su maduración digital, la cantidad de datos y las oportunidades para aprovecharlos de maneras nuevas crece de manera exponencial, lo que permite que los bancos persigan nuevos modelos de negocio y áreas de oportunidades centradas en el cliente.

Tener implementada la estrategia de almacenamiento de datos correcta resulta fundamental para obtener eficacia operativa, un buen rendimiento de las aplicaciones y el cumplimiento reglamentario. La estrategia de almacenamiento de datos también es la pieza clave inicial para transformar los datos a un formato en el que se puedan usar para la inteligencia empresarial e información procesable.

El siguiente es un patrón común para la administración de datos:

![Flujo de administración de datos](./assets/data-mgmt-in-banking-overview/data-mgmt-00.png)

En este modelo, "Data Services" (Servicios de datos) describe cualquier transformación, combinación de datos o cualquier otra operación de datos distinta del archivado. Esta es la actividad clave que se necesita para aprovechar los datos a fin de tomar decisiones más fundamentadas.

Todos los bancos e instituciones financieras ingieren, mueven y almacenan datos. Este artículo se centra en llevar datos a Azure y alejarse del almacenamiento, procesamiento, archivado y eliminación de datos locales tradicionales. Al migrar los datos a Azure, tanto los bancos como las instituciones financieras pueden aprovechar las ventajas fundamentales, las que incluyen:

- Control de costo a través de una escala global efectivamente limitada, mediante el uso de recursos de proceso y la capacidad de datos solo en el momento y el lugar en que es necesario.

- Reducción de gastos de capital y costos de administración a través de la retirada de los servidores físicos locales.

- Copia de seguridad y recuperación ante desastres integrada, lo que disminuye el costo y la complejidad de la protección de datos.

- Archivado automatizado de datos inactivos en almacenamiento a bajo costo, a la vez que se garantiza que se satisfacen las necesidades de cumplimiento.

- Acceso a servicios de datos avanzados e integrados para procesar datos para aprendizaje, previsión, transformación u otras necesidades.

En este artículo se brindan técnicas recomendadas para garantizar la entrada eficaz de los datos en Azure y técnicas fundamentales de administración de datos para usarlas una vez en la nube.

## <a name="data-ingest"></a>Ingesta de datos

Las instituciones financieras tendrán datos ya recopilados y usados por las aplicaciones actuales. Hay varias opciones para mover estos datos a Azure. En muchos casos, las aplicaciones existentes pueden conectarse a datos en Azure del mismo modo que si estuvieran en el entorno local, con un mínimo de cambios en esas aplicaciones existentes. Esto es especialmente cierto cuando se usa Microsoft [Azure SQL Database](/azure/sql-database/?WT.mc_id=bankdm-docs-dastarr), pero en [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/databases?WT.mc_id=bankdm-docs-dastarr) se pueden encontrar soluciones para Oracle, TeraData MongoDB, etc.

Existen distintas estrategias de migración de datos para trasladar datos desde el entorno local a Azure con distintos grados de latencia. Todas las técnicas a las que se hace referencia a continuación brindan transparencia de los datos y seguridad confiable.

### <a name="virtual-network-vnet-service-endpoints"></a>Puntos de conexión de servicio de redes virtuales (VNET)

La seguridad es una preocupación principal cuando se trabaja con información financiera de los clientes.
La protección de los recursos (como una base de datos) dentro de Azure a menudo depende de configurar una infraestructura de red dentro de Azure mismo y, luego, acceder a esa red a través de un punto de conexión específico.

Antes de transferir datos a Azure, resulta útil considerar la topología de red que proteger tanto a los recursos de Azure como a la conexión con ellos desde el entorno local.
[Puntos de conexión del servicio de redes virtuales (VNet?WT.mc_id=bankdm-docs-dastarr)](/azure/virtual-network/virtual-network-service-endpoints-overview?WT.mc_id=bankdm-docs-dastarr) proporciona una conexión directa protegida a una red virtual definida por Azure.

Las redes virtuales se definen en Azure para contener recursos de Azure dentro de una red virtual enlazada. Un punto de conexión a esa red virtual protege entonces el acceso a los recursos críticos del servicio de Azure y solo a los que se encuentran en la red virtual definida.

## <a name="database-lift-and-shift"></a>Migración mediante lift-and-shift de bases de datos

Un modelo de migración de base de datos mediante lift-and-shift es uno de los escenarios más comunes para usar Azure SQL Database. La migración mediante lift-and-shift simplemente significa tomar las bases de datos locales existentes y migrarlas directamente a la nube. Entre las razones para hacerlo se incluyen las siguientes:

- Migrar desde un centro de datos actual donde los precios son más altos o alguna otra razón operativa
- El hardware de la base de datos SQL Server local actual está a punto de expirar o está cerca del final del ciclo de vida
- Para admitir una estrategia general de "migración a la nube" para la empresa
- Aprovechar las funcionalidades de disponibilidad y recuperación ante desastres de SQL Azure

En el caso de las bases de datos más pequeñas, el primer paso de la ingesta de datos suele ser la creación de los almacenes de datos y las estructuras (como tablas) que se necesitan mediante Azure Portal, la CLI de Azure o Azure SDK. Para estos almacenes de datos más pequeños, los próximos pasos los puede realizar una aplicación personalizada escrita para copiar los datos correctos en el almacenamiento de datos de Azure adecuado. En el caso de las migraciones de datos de mayor tamaño, la restauración de copias de seguridad en Azure suele ser la ruta más rápida.

Hay muchas maneras de transferir datos a Azure de manera segura y rápida. [Consulte este artículo](/azure/architecture/data-guide/scenarios/data-transfer?WT.mc_id=bankdm-docs-dastarr) para conocer algunas técnicas estándar y las ventajas y desventajas de cada una de ellas.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

Cuando se usa la migración mediante lift-and-shift de bases de datos de SQL Server, se puede usar [Microsoft Azure Database Migration Service](/azure/dms/dms-overview?WT.mc_id=bankdm-docs-dastarr) para mover las bases de datos a Azure. El servicio usa [Data Migration Assistant](https://docs.microsoft.com/en-us/sql/dma/dma-overview?WT.mc_id=bankdm-docs-dastarr) para asegurarse de que la base de datos local sea compatible con las características que se ofrecen en Azure SQL. Cualquier cambio necesario antes de migrar la base de datos depende del usuario. Además, el uso del servicio requiere una conexión a Internet de sitio a sitio entre la red local y Azure.

### <a name="bulk-copy-program-for-sql-server"></a>Programa de copia masiva para SQL Server

Si SQL Server está actualmente en el entorno local y el objetivo es migrar a SQL Azure, otra excelente técnica es usar SQL Server Management Studio [y la utilidad de BCP para migrar datos](https://azure.microsoft.com/en-us/blog/bcp-and-sql-azure/?WT.mc_id=bankdm-docs-dastarr) a SQL Azure. Después del scripting y la creación de bases de datos SQL de Azure a partir del servidor local original, se puede usar BCP para transferir rápidamente datos a SQL Azure.

### <a name="blob-and-file-storage"></a>Blob Storage y Files Storage

A menudo, las sucursales bancarias individuales tiene sus propios almacenes de archivos en servidores locales. Esto puede provocar problemas con el uso compartido de archivos entre las sucursales y hacer que no exista un origen confiable único para un archivo determinado. Y, lo que es peor, puede que la institución tenga un almacén de archivos "oficial" al que acceden las sucursales, pero con conectividad intermitente u otros problemas para acceder al recurso compartido de archivos.

Azure cuenta con servicios para ayudar a mitigar estos problemas. La migración de estos datos a Azure brinda un origen único para todos los datos y almacenamiento a disposición de todos los usuarios con permisos centralizados y controles de acceso.

Distintas soluciones de almacenamiento de datos podrían resultar más adecuadas para otros formatos de datos.
Por ejemplo, los datos almacenados en el entorno local en SQL Server probablemente sean más adecuados para Azure SQL. Los datos almacenados en archivos .csv o Excel probablemente sean más adecuados para el almacenamiento de [Azure Blob Storage](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr) o [Azure Files Storage](/azure/storage/files/storage-files-introduction?WT.mc_id=bankdm-docs-dastarr), que se implementa en Blob service.

Casi todos los datos que fluyen desde y hacia Azure pasan por Blob Storage como parte del movimiento de los datos. Blob Storage tiene los siguientes pilares.

- Durable y disponible
- Seguro y compatible
- Administrable y rentable
- Escalable y eficaz
- Abierto e interoperable

La conexión de todas las sucursales al mismo recurso compartido de archivo en Azure se realiza con frecuencia mediante el centro de datos existente del banco, tal como se muestra en la ilustración 1. El centro de datos corporativo se conecta a Files Storage a través de una conexión de SMB (Bloque de mensajes del servidor).
Lógicamente, y desde el punto de vista de la red del sitio, el recurso compartido de archivo puede estar en el centro de datos corporativo y se puede montar como cualquier otro recurso compartido de archivo en red.
Cuando se usa esta técnica, los datos se cifran en reposo y durante el transporte entre el centro de datos y Azure.

![Uso compartido de archivos lógicos](./assets/data-mgmt-in-banking-overview/data-mgmt-01.png)

En la Ilustración 1

Las empresas suelen usar Files Storage para consolidar y proteger grandes volúmenes de archivos. Esto permite retirar los servidores de archivos antiguos o reasignar el propósito del hardware.
Otra ventaja de migrar a Files Storage es centralizar la administración de los datos y los servicios de recuperación.

### <a name="azure-data-box"></a>Azure Data Box

A menudo, los bancos tendrán terabytes (si no petabytes) de información que traer a Azure. Por suerte, los almacenes de datos en Azure son muy elásticos y altamente escalables.

Un servicio centrado en migran grandes volúmenes de datos a Azure es [Azure Data Box](https://azure.microsoft.com/en-us/services/storage/databox/?WT.mc_id=bankdm-docs-dastarr). Este servicio está diseñado para migrar datos sin transferir los datos ni las copias de seguridad a través de una conexión de Azure. Adecuado para terabytes de datos, Azure Data Box es un dispositivo que se puede pedir en Azure Portal. Se envía a su ubicación, donde lo puede conectar en la red y cargar con datos a través de protocolos NAS estándar y protegido mediante cifrado AES de 256 bits estándar. Una vez que los datos están en el dispositivo, se envían de vuelta al centro de datos de Azure donde los datos se hidrata en Azure.
Luego, el dispositivo se borra de manera segura.

## <a name="azure-information-protection"></a>Azure Information Protection

Azure Information Protection (AIP) es una solución basada en la nube que ayuda a las organizaciones a clasificar, etiqueta y proteger sus documentos y correos electrónicos. Esto puede ser automático para los administradores que definen reglas y condiciones, manual para los usuarios o una combinación de ambas opciones cuando los usuarios reciben recomendaciones.

## <a name="data-services"></a>Servicios de datos

Los bancos tienen dificultades con la administración de datos maestros, los metadatos en conflicto debido a sistemas bancarios principales distintos y los datos que provienen de sistemas de origen, sistemas de incorporación, sistemas de administración de ofertas, sistemas CMR, etc. Azure cuenta con herramientas para ayudar a mitigar estos y otros problemas frecuentes con los datos.

Hay muchas operaciones que las organizaciones de servicios financieros tienen que realizar en sus datos. Cuando se escriben datos en los almacenes de datos de Azure, es posible que sea necesario transformar dichos datos o combinarlos con otros datos que aumenten lo que se está ingiriendo.

### <a name="azure-data-factory"></a>Azure Data Factory

[Microsoft Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=bankdm-docs-dastarr) es un servicio totalmente administrado que ayuda a ingresar, procesar y supervisar el movimiento de datos en una canalización de Data Factory. Las actividades de Data Factory conforman la estructura de la canalización de administración de datos.

Data Factory permite la transformación o el aumento de los datos a medida que fluyen a Azure y entre otros servicios de Azure. Data Factory es un servicio en la nube administrado creado para complejos proyectos híbridos de extracción, transformación y carga (ETL), extracción, carga y transformación (ELT) e integración de datos.

Por ejemplo, los datos se pueden ingresar en canalizaciones de análisis o herramientas que generan información procesable. Los datos pueden fluir a una solución de aprendizaje automático o transformarse a otro formato para su posterior procesamiento descendente. Un ejemplo es convertir archivos .csv en archivos Parquet, que son más adecuados para los sistemas de aprendizaje automático, y almacenar dichos archivos Parquet en Blob Storage.

Los datos también se pueden enviar a servicios de proceso descendentes como [Azure HDInsight](/azure/hdinsight/hadoop/apache-hadoop-introduction?WT.mc_id=bankdm-docs-dastarr), [Spark](/azure/hdinsight/spark/apache-spark-overview?WT.mc_id=bankdm-docs-dastarr), [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview?WT.mc_id=bankdm-docs-dastarr) y [Azure Machine Learning](/azure/machine-learning/?WT.mc_id=bankdm-docs-dastarr). Esto permite alimentar los sistemas directamente, lo que genera informes inteligentes y de análisis. En la ilustración 2 que aparece a continuación, se muestra un modelo común para la entrada de datos. Los datos se conservan en un [Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=bankdm-docs-dastarr) común para usarlos en los servicios de análisis descendentes.

![Un modelo de ingesta de Data Lake](./assets/data-mgmt-in-banking-overview/data-mgmt-02.png)

Ilustración 2

Las canalizaciones de Data Factory constan de actividades, que toman conjuntos de datos de entrada y de salida. Las actividades se pueden ensamblar en una canalización que define de dónde quiere obtener los datos, cómo quiere procesarlos y dónde quiere almacenar los resultados. La compilación de canalizaciones con actividades es el centro de Data Factory y componer un flujo de trabajo visual justo en Azure Portal facilita la creación de las canalizaciones. [Consulte aquí para ver una lista completa](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr) de las actividades.

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr) es una plataforma de análisis administrada basada en Apache Spark en Azure. Es altamente escalable y los trabajos de Spark se ejecutan en clústeres de máquina tan grandes como sea necesario. Databricks funciona desde un cuaderno que proporciona un lugar único de colaboración entre los científicos de datos, los ingenieros de datos y los analistas de negocio.

Databricks es una canalización de procesamiento lógico cuando se necesita analizar o transformar los datos. Se puede insertar directamente mediante Data Factory en escenarios de aprendizaje automático en los que el tiempo de acceso a los datos es fundamental o para transformaciones de archivos simples.

![Databricks](./assets/data-mgmt-in-banking-overview/data-mgmt-03.png)

## <a name="archiving-data"></a>Archivado de datos

Cuando ya no se necesitan datos en un almacén de datos activos, se pueden archivar por motivos de cumplimiento o de pista de auditoría, en conformidad con las reglamentaciones bancarias estatales y locales. Azure tiene opciones disponibles para el almacenamiento de datos a los que no se accede con frecuencia. A menudo, existen problemas de privacidad con los datos que requieren mantener en almacenamiento los datos durante años.

Los costos de almacenar datos pueden ser altos, especialmente cuando se almacenan en bases de datos locales. En algunas ocasiones, a estas bases se datos se accede con poca frecuencia y solo para escribir nuevos datos archivados o liberar la base de datos de datos que ya no se quieren en el archivo.
El acceso poco frecuente a las máquinas locales significa un mayor costo total de propiedad del hardware.

### <a name="azure-archive-storage"></a>Azure Archive Storage

En el caso de datos no estructurados como archivos o imágenes, Azure ofrece [varios niveles de almacenamiento](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=bankdm-docs-dastarr) para Blob Storage, incluidos el nivel de acceso frecuente, el nivel de acceso esporádico y el nivel de archivo. El nivel de acceso frecuente es para los datos activos y que se espera que sean los que tengan un mayor rendimiento y que se usen más en las aplicaciones. El nivel de acceso esporádico es para conjuntos de datos de copia de seguridad y recuperación ante desastres a corto plazo, así como para los datos disponibles para una aplicación, pero a los que se accede en raras ocasiones. El nivel de archivo tiene el costo más bajo y está pensado para los datos sin conexión.

Los datos en el nivel de archivo se pueden rehidratar en los niveles de acceso esporádico o de acceso frecuente, pero esta acción puede tardar varias horas en completarse. El almacenamiento de archivo puede resultar adecuado si no se va a acceder a los datos durante al menos 180 días. Cuando hay un blob en el almacenamiento de archivo, no se puede leer, pero sí se pueden realizar otras operaciones existentes, como enumerar, eliminar y recuperar metadatos. El nivel de datos de archivo es el nivel de datos menos costoso para Blob Storage.

### <a name="azure-sql-database-long-term-retention"></a>Retención a largo plazo de Azure SQL Database

Cuando se usa Azure SQL, hay un [servicio de retención de copia de seguridad a largo plazo](/azure/sql-database/sql-database-long-term-retention?WT.mc_id=bankdm-docs-dastarr) para almacenar las copias de seguridad hasta por diez años. Los usuarios pueden programar la retención de copias de seguridad para un almacenamiento a largo plazo, como que la copia de seguridad se retendrá por semanas, meses o incluso años.

Para restaurar una base de datos desde el almacenamiento a largo plazo, seleccione una copia de seguridad específica en función de su marca de tiempo. La base de datos se puede restaurar en cualquier servidor existente en la misma suscripción que la base de datos original.

## <a name="deleting-unwanted-data"></a>Eliminación de datos no deseados

Para mantener el cumplimiento con reglamentaciones bancarias o directivas relacionadas con la retención de datos, a menudo se debe eliminar los datos cuando ya no se necesitan. Antes de implementar una solución técnica para estos datos no deseas, es importante tener implementado un plan de purga para no infringir las directivas acordadas. Los datos se pueden eliminar del archivo o cualquier otro almacén de Azure en cualquier momento.

Una estrategia eficaz para eliminar los datos no deseados es hacerlo según un intervalo, siendo el más común todas las noches o todas las semanas. Se puede escribir una instancia de [Azure Function desencadenada por tiempo](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr) para realizar correctamente este trabajo. Si se elimina algún dato, Microsoft Azure eliminará los datos, incluidas las copias en memoria caché y las copias de seguridad.

## <a name="getting-started"></a>Introducción

Hay muchas maneras de empezar a trabajar según el uso actual y la madurez de los modelos de datos que se usan actualmente. En todos los casos, es el momento perfecto para revisar el almacenamiento de datos, el procesamiento y el modelo de retención que se necesita por almacén de datos. Esto resulta crítico para crear sistemas de administración de datos en escenarios de cumplimiento reglamentario.
Aquí, la nube ofrece nuevas oportunidades que no está disponibles actualmente en el entorno local. Esto puede significar actualizaciones de los modelos de datos existentes que pueda tener.

Una vez que se familiarice con el modelo de datos nuevo, determine una estrategia para la ingesta de datos. ¿Qué orígenes de datos existen? ¿Dónde residirán los datos en Azure? ¿Cómo y cuándo se migrarán a Azure? Aquí hay varios recursos disponibles para ayudar en la migración según el tipo de contenido, el tamaño, etc. Azure Data Migration Service es ejemplo de aquello.

Una vez que los datos estén hospedados en Azure, cree un plan de purga de datos para los datos que han sobrevivido a su utilidad o duración. Si bien el almacenamiento a largo plazo (en frío) siempre es una excelente opción para el archivado, limpiar los datos expirados disminuye la superficie y los costos de almacenamiento general. La copia de seguridad y archivado de las [arquitecturas de la solución de Azure](https://azure.microsoft.com/en-us/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr) es un recurso ideal para ayudarlo a planear su estrategia global.

## <a name="relevant-technologies"></a>Tecnologías relevantes

- [Azure Functions](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr) son scripts sin servidor y programas pequeños que se pueden ejecutar como respuesta a un evento de sistema o según un temporizador.

- [Herramientas de cliente de Azure Storage](/azure/storage/common/storage-explorers?WT.mc_id=bankdm-docs-dastarr) son herramientas para acceder a los almacenes de datos e incluyen mucho más que Azure Portal.

- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr) es adecuado para almacenar archivos como texto o imágenes y otros tipos de datos no estructurados.

- [Databricks](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr) es un servicio totalmente administrado que ofrece la implementación sencilla de un clúster de Spark.

- [Data Factory](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr) es un servicio de integración de datos en la nube que se usa para crear servicios de almacenamiento, traslado y procesamiento de datos en canalizaciones de datos automatizadas.

## <a name="conclusion"></a>Conclusión

Con el cambio rápido del panorama digital del sector bancario y financiero, los clientes buscan cada vez más soluciones y asociados que puedan usar de inmediato sin ningún tiempo de inicialización que los retrase. Dado que la ingesta de datos aumenta de manera exponencial, los bancos necesitan formas rápidas, innovadoras y seguras para almacenar, analizar y usar los datos importantes.

Azure puede ayudar con los requisitos de ingesta de datos, procesamiento, archivado y eliminación mediante el uso de varias tecnologías y estrategias. La ingesta de datos en Azure es simple y hay varios almacenes de datos disponibles para almacenar datos en función de su tipo, estructura, etc. Hay soluciones de datos disponibles más allá de SQL Server y SQL Azure para incluir bases de datos de terceros.

Operar y actuar en función de esos datos puede ser algo sencillo con servicios de Azure como Databricks y Data Factory. El almacenamiento de archivo está disponible para el almacenamiento a largo plazo de datos a los que rara vez se accede y se puede eliminar según un ciclo continuo cuando sea necesario.

Visite la biblioteca de soluciones de Azure para información sobre la [copia de seguridad y el almacenamiento de archivo](https://azure.microsoft.com/en-us/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr) con el fin de empezar a diseñar el plan de administración de datos.

**Artículo de** Howard Bush y David Starr
