---
title: Introducción a la computación en malla de riesgos en la banca
author: dstarr
ms.author: dastarr
ms.date: 04/12/2018
ms.topic: article
ms.service: industry
description: Presenta las consideraciones de negocio de la implementación de la computación en malla de riesgos de la banca en Azure.
ms.openlocfilehash: 49d3d5223bee85689043d84eb5236cca4f53fc03
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654182"
---
# <a name="risk-grid-computing-in-banking-overview"></a>Introducción a la computación en malla de riesgos en la banca

## <a name="introduction"></a>Introducción

En finanzas corporativas y banca de inversión, uno de los trabajos más importantes es el análisis del riesgo.

Para ofrecer una imagen completa del riesgo asociado a una cartera de inversiones, los analistas de riesgos financieros revisan los estudios, supervisan las condiciones económicas y sociales, se mantienen al tanto de las normativas y crean modelos informáticos del clima de inversión.

El análisis de riesgos mediante los numerosos vectores que afectan a una cartera es lo suficientemente complejo como para que se requiera un modelo informático. La mayoría de los analistas dedican bastante tiempo a trabajar con modelos informáticos para simular y predecir cómo cambiarán las condiciones financieras. Al evaluar los riesgos de inversión (riesgo de mercado, riesgo de crédito y riesgo operativo), la carga de cálculo del procesamiento de los modelos predictivos puede ser bastante grande debido al volumen y diversidad de los datos.

La informática en la nube ofrece beneficios significativos para la computación en malla de riesgos o el modelado de riesgos porque permite a los analistas acceder a recursos de proceso masivos a petición, sin incurrir en costos de capital ni administrar la infraestructura. En este artículo se examina el aprovechamiento de Microsoft Azure para aumentar los recursos de la computación en malla de riesgos actuales y optimizar el costo y la velocidad de las cargas de trabajo de la computación en malla de riesgos. Los temas tratados incluyen la conectividad segura y confiable, el procesamiento por lotes y el aumento de los recursos de proceso en función de la demanda cuando los servidores locales están a pleno rendimiento.

## <a name="grid-computing-services"></a>Servicios de computación en malla

Los analistas necesitan una forma sencilla y confiable de proporcionar sus modelos a una canalización de procesamiento por lotes, que comienza con la ingesta de datos y fluye a través del procesamiento de datos hasta el análisis, donde se puede obtener información a partir de los datos resultantes.

Los datos de entrada del modelo de riesgo se presentan en varias formas, y las más comunes son archivos Excel o archivos .csv. Estos archivos a menudo se reestructuran en formatos más adecuados para procesar el modelo de riesgo en etapas posteriores de la canalización del proceso de riesgos. Una técnica común para analizar y procesar estos archivos es el procesamiento por lotes con una cuadrícula de máquinas virtuales (VM?WT.mc_id=gridbank-docs-dastarr) que trabajan juntas para alcanzar un objetivo común.

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) es un servicio de Azure que permite que varias máquinas virtuales de trabajo se ejecuten en paralelo, como se muestra a continuación. El procesamiento de archivos de datos y el envío de los resultados a los sistemas de aprendizaje automático o a los almacenes de datos son tareas comunes para los nodos de trabajo. El cliente crea el código de aplicación que se ejecuta por los nodos de trabajo, por lo que casi cualquier acción se puede realizar en el trabajo por lotes.

![Lote local](./assets/risk-grid-compute-assets/01-on-prem.png)

Azure proporciona una solución elegante para la computación en malla de riesgos con Azure Batch. Los clientes pueden utilizar Azure Batch para ampliar la computación en malla de riesgos existente, o para reemplazar los recursos locales con una solución completamente basada en la nube.

Se admite totalmente la conexión directa y segura a la nube de Azure. Los nodos de trabajo de la computación en malla de riesgos del servicio Batch pueden acceder a los datos de modelado cuando se conectan a los datos almacenados localmente y cuando se conectan a Azure con una red híbrida. El cliente también puede cargar los datos en un almacenamiento apropiado dentro de Azure, lo que permite al servicio Batch tener acceso directo a los datos.

## <a name="secure-connectivity-to-azure"></a>Conectividad segura a Azure

Cuando se crea una solución de computación en malla de riesgos en Azure, el negocio a menudo continuará utilizando aplicaciones locales existentes como sistemas de comercio, administración de riesgos Middle Office, análisis de riesgos, etc. Azure se convierte en una extensión de las inversiones existentes.

Al conectarse a la nube, la seguridad es una consideración fundamental. Explicar el modelo de seguridad actual es el primer paso para conectarse directamente con Azure. Para los clientes que ya utilizan Active Directory (AD?WT.mc_id=gridbank-docs-dastarr) en su entorno local, la conexión a Azure puede aprovechar los recursos de identidad existentes. Las cuentas de servicio pueden residir en Active Directory local.

### <a name="hybrid-network-solution"></a>Solución de red híbrida

Una red híbrida une a Azure directamente a la red local del cliente. Azure ofrece dos modelos para conectar de manera segura y confiable los sistemas actuales locales con Azure, [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr) y [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr). Ambas son soluciones de conectividad de confianza, aunque existen diferencias en la implementación, el rendimiento, el costo y otros atributos.

![Conectividad de Azure](./assets/risk-grid-compute-assets/02-connectivity.png)

La opción de &quot;ráfagas a la nube&quot; descarga los trabajos de procesamiento a las máquinas basadas en la nube cuando los recursos existentes aumentan, lo que incrementa los recursos del centro de datos del cliente o de la nube privada. El uso del modelo de red híbrido permite escenarios de ráfagas a la nube, ya que la computación en malla de riesgos basada en la nube es una simple extensión de la red existente.

Hay varias configuraciones de conectividad de red más allá de las del modelo simple presentado en la arquitectura lógica anterior. Para ayudar con las decisiones y la guía arquitectónica con respecto a la conexión de la red a Azure, consulte el artículo [_Conexión de una red local a Azure_](/azure/architecture/reference-architectures/hybrid-networking/).

### <a name="rest-api-solution-over-internet"></a>Solución de la API REST a través de Internet

Una alternativa a la creación de una red híbrida es cargar datos en Azure Storage (siendo probable que el almacenamiento de archivos o blobs sea un candidato?WT.mc_id=gridbank-docs-dastarr) y hacer que el servicio Batch lea los archivos de datos desde el almacenamiento. Esto puede lograrse mediante una conexión segura (SSL?WT.mc.mc_id=gridbank-docs-dastarr) para conectarse a Azure, el almacenamiento de los documentos en Azure Storage y, después, la administración de los trabajos de computación en malla de riesgo mediante la [API REST del servicio Batch](/rest/api/batchservice/?WT.mc_id=gridbank-docs-dastarr) o el SDK con una aplicación adecuada, con la coordinación de la ejecución de Batch.

### <a name="azure-data-factory"></a>Azure Data Factory

Otra solución para el escenario puede ser el uso de [Azure Data Factory](/azure/data-factory/?WT.mc_id=gridbank-docs-dastarr), un servicio de integración de datos basado en la nube, para componer grandes canalizaciones de almacenamiento, movimiento y procesamiento. Los datos se pueden cargar a petición mediante una canalización de Data Factory. El servicio proporciona un diseñador visual en Azure Portal para crear soluciones de extracción, transformación y carga de datos (ETL?WT.mc_id=gridbank-docs-dastarr) en Azure. Data Factory puede ayudar a ingerir datos en Azure para su posterior procesamiento.

## <a name="matching-processing-needs-with-demand"></a>Adaptación de las necesidades de procesamiento a la demanda

Cuando se calcula el riesgo, ya sea diariamente o con las cargas más pesadas al final del mes, los cálculos consumen recursos informáticos significativos. Estos cálculos no se ejecutan ininterrumpidamente. Cuando los cálculos de riesgo no se ejecutan en la cuadrícula local, la organización deja que servidores valiosos y costosos funcionen sin carga de trabajo, pero con costos continuos de energía, refrigeración y espacio en el centro de datos, junto con otros costos fijos.

### <a name="augmenting-on-premises-grid-with-azure-batch"></a>Aumento de la cuadrícula en el entorno local con Azure Batch

Para minimizar los costos, una empresa puede optar por poseer y administrar solo los nodos de trabajo suficientes para satisfacer los requisitos cuando la demanda es baja. Los trabajos de computación en malla de riesgos y alta demanda se pueden trasladar a servidores de alto rendimiento en Azure, para poder escalar y reducir verticalmente de manera elástica con la demanda de carga de trabajo.

El modelo de procesamiento de Azure Batch posee varias ventajas para la computación en malla de riesgos:

- Incrementa las inversiones existentes en varios sistemas locales.
- Permite que la infraestructura existente sirva a las necesidades de análisis de riesgos cuando la demanda es baja, desasignando los nodos de trabajo basados en Azure.
- Proporciona capacidad adicional a la cuadrícula de cálculo de riesgos cuando la demanda es alta.
- Permite ajustar los perfiles de las máquinas a la potencia de procesamiento necesaria para la carga de trabajo de Batch, incluso cuando la carga requiere configuraciones de [informática de alto rendimiento (HPC)](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr).

Una solución común es agregar automáticamente nodos de trabajo en Azure cuando todos los trabajos locales están en uso. El nodo principal de la cuadrícula de riesgos simplemente solicita más trabajos. Esto permite escalar automáticamente el número de nodos de trabajo de la cuadrícula en Azure y permite una solución de demanda elástica.

![Nube híbrida](./assets/risk-grid-compute-assets/03-hybrid-cloud.png)

Junto con un uso eficaz de los recursos, esta organización proporciona otras ventajas. Para tareas independientes, la adición de más trabajos permite que la carga se escale linealmente. Azure también ofrece la flexibilidad para probar una instancia de máquina virtual muy grande o una máquina con varias tarjetas GPU. Esta flexibilidad permite experimentación e innovación.

Para los momentos en que se necesita más capacidad de proceso, como las valoraciones trimestrales, la capacidad adicional también puede provenir del escalado automático de Azure Batch. El escalado automático proporciona elasticidad a la solución de Batch. Al escalar los recursos para que se ajusten a la carga necesaria, Azure proporciona una capacidad significativamente mayor a un costo más bajo que el de poseer el hardware.

La mayoría de los productos comerciales de cuadrícula admiten alguna forma de ráfaga a la nube, lo que permite pruebas de concepto más fáciles para la carga de análisis de riesgos. Por ejemplo, [Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) se puede ejecutar en Azure, al igual que los productos de empresas como TIBCO, Univa y otros. Muchas de estas herramientas o sistemas de terceros están disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/?WT.mc_id=gridbank-docs-dastarr).

### <a name="migrating-additional-resources-to-the-cloud"></a>Migración de recursos adicionales a la nube

A medida que las cargas de trabajo crecen o la infraestructura del centro de datos local envejece, las organizaciones pueden trasladar a Azure todo el procesamiento de Batch para la computación en malla de riesgos.

### <a name="growing-into-azure"></a>Crecimiento en Azure

A medida que las máquinas locales llegan al final de su ciclo de vida, puede distribuir aún más los nodos de trabajo en la nube. Lo mismo se aplica al nodo principal de Batch. Esto invierte la relación entre la red local y Azure. Esto puede ser una oportunidad para disminuir los costos al desmantelar cualquier producto de red a red como Azure ExpressRoute, y cualquier nodo restante de trabajos en el entorno local.

Como parte de este cambio, los datos pueden estar disponibles en Azure mediante varias técnicas de entrada de archivos. Azure tiene muchas opciones de almacenamiento para elegir, incluyendo puntos de conexión de descanso para permitir la carga de datos directamente, en lugar de que los trabajos de proceso los recojan de la red local.

 ![Lote local](./assets/risk-grid-compute-assets/04-batch-process.png)

Bajo este modelo, todas las actividades de computación en malla de riesgos pueden tener lugar en la nube. Los archivos de datos procesados por los trabajos pueden almacenarse en el almacenamiento de Azure, los datos pueden insertarse directamente en Azure Data Lake y Azure HDInsight puede ocuparse de las necesidades de aprendizaje automático. Finalmente, Power BI y Azure Analytics son excelentes herramientas de análisis de datos y pueden funcionar con todos los datos almacenados en Azure.

### <a name="data-security-considerations-for-risk-grid-computing"></a>Consideraciones de seguridad de datos para la computación en malla de riesgos

Aunque los datos de cálculo a menudo no incluyen ninguna información de identificación personal (PII?WT.mc_id=gridbank-docs-dastarr), es probable que la mayoría de los bancos realicen una evaluación de riesgos de seguridad antes de colocar cualquier carga de trabajo en la nube. Esta evaluación puede requerir la aportación de Microsoft y puede dar lugar a recomendaciones de seguridad.

Una importante consideración para la computación en malla de riesgos es [ejecutar los procesos por lotes dentro de una red virtual de Azure](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr). Esto permite que los nodos de proceso de grupo se comuniquen de forma segura con otros nodos de proceso o con una red local. Los nodos de proceso por lotes deben crear y utilizar cuentas de servicio apropiadas y grupos de seguridad de red (NSG). [Azure también tiene soluciones](/azure/security/blueprints/financial-services-regulated-workloads?WT.mc_id=gridbank-docs-dastarr) para el cifrado de datos en tránsito y en reposo en el almacenamiento de Azure.

Algunas áreas a tener en cuenta pueden ser: Active Directory (AD) o nodos de proceso no unidos a AD (para nodos de Windows Server?WT.mc_id=gridbank-docs-dastarr), [Cifrado de discos de máquinas virtuales](/azure/security/azure-security-disk-encryption?WT.mc_id=gridbank-docs-dastarr), seguridad de la entrada y salida del cálculo de los datos en reposo y en tránsito, configuraciones de red de Azure, permisos y mucho más. La autenticación también se puede controlar en el nivel de la API REST mediante una clave secreta.

## <a name="getting-started"></a>Introducción

Muchos clientes tienen una computación en malla de riesgos interna ya existente. Si la empresa ha desarrollado la cuadrícula internamente, considere utilizar Azure Batch para ampliar la cuadrícula. Un buen lugar para comenzar con Azure Batch es la ampliación de cualquier solución local actual mediante la réplica de la lógica de la aplicación de procesamiento actual y su ejecución como un trabajo de Batch en Azure. Esto puede requerir una solución de redes para unir los nodos de proceso de Azure Batch a la red local, dependiendo de la funcionalidad de nuestra aplicación.

Para mitigar cualquier problema de seguridad, velocidad y confiabilidad de la conexión, considere conectar la red local a Azure mediante Azure ExpressRoute o VPN Gateway. A partir de ahí, es posible que el nodo principal local aprovisione un grupo de nodos de trabajo basados en Azure, acelerándolos o desacelerándolos según sea necesario.

Por último, es posible que esté preparado para una migración completa de la infraestructura de proceso de riesgos a Azure. Si es así, [este artículo](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) le ayudará a comenzar hoy mismo.

## <a name="technologies-presented"></a>Tecnologías presentadas

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr) permite aumentar los nodos de trabajo de computación de riesgos locales para proporcionar dinámicamente recursos de proceso basados en la demanda.

[Azure Data Lake](/azure/data-lake-store?WT.mc_id=gridbank-docs-dastarr) proporciona almacenamiento, procesamiento y análisis a través de los datos de análisis de riesgos.

[Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr) amplía la red local en Azure sobre una conexión privada que facilita un proveedor de conectividad.

[Azure HDInsight](/azure/hdinsight/?WT.mc_id=gridbank-docs-dastarr) es un servicio de análisis de código abierto totalmente administrado para procesar cantidades masivas de datos, como los datos proporcionados en las ejecuciones por lotes de fin de mes.

[Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) permite el aprovisionamiento de clústeres de informática de alto rendimiento para el procesamiento por lotes.

[Power BI](/power-bi/?WT.mc_id=gridbank-docs-dastarr) es un conjunto de herramientas de análisis de negocios que utilizan los analistas de riesgos para obtener y compartir conocimientos.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr) extiende su red local a la nube Azure en Internet.

## <a name="conclusion"></a>Conclusión

Las soluciones que se tratan en este artículo son los enfoques de la computación en malla de riesgos en la banca. Se pueden utilizar otras arquitecturas dadas las funcionalidades enriquecidas de los productos y servicios de Azure y las diversas arquitecturas de sistemas cliente existentes. Aún así, el servicio Batch ofrece un modelo razonable para la computación en malla de riesgos, dadas las ventajas que se exponen en este artículo.

[Ampliar la red local a Azure](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbank-docs-dastarr) permite a Azure un acceso fácil a los recursos de red y a otros sistemas de procesamiento ya presentes en la red local. Cuando las máquinas locales están llegando al final de su ciclo de vida, puede tener más sentido utilizar el proceso de Batch completamente en Azure en lugar de admitir un modelo híbrido.

Cargar archivos a Azure Storage antes de que comience el trabajo de Batch es otra forma de aprovechar el servicio Batch sin necesidad de una red híbrida. Esto se puede hacer de forma incremental o como un proceso de inicio de la ejecución del servicio Batch.

Después de seleccionar una estrategia de conectividad, un lugar lógico para comenzar con el proceso de riesgo es colocar los trabajos existentes en los nodos de trabajo de cálculo de Azure y ejecutarlos en un entorno de prueba para ver si es necesario cambiar algún código. [En este artículo se ofrece un punto de partida](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr) para comenzar con Azure Batch en el lenguaje o herramienta de su elección.

[Guía de soluciones de la computación en malla de riesgos en la banca](/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=banking-docs-dastarr)
