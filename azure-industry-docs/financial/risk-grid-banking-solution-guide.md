---
title: Guía de soluciones de la computación en malla de riesgos en la banca
author: dstarr
ms.author: dastarr
ms.date: 5/2/2018
ms.topic: article
ms.service: industry
description: Presenta los aspectos técnicos de la implementación de Azure Batch para la computación en malla de riesgos en la banca.
ms.openlocfilehash: d3470a2e546e73f4c0f1478413ca4b1af7433a66
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654302"
---
# <a name="risk-grid-computing-in-banking-solution-guide"></a>Guía de soluciones de la computación en malla de riesgos en la banca

En este artículo se proporciona una introducción técnica del uso de Microsoft Azure para respaldar y mejorar la computación en malla de riesgos en la banca, incluidos los sistemas recomendados y las arquitecturas de alto nivel.

Este documento está pensado para arquitectos de soluciones y, en algunos casos, para los responsables de las tomas de decisiones técnicas, que quieran profundizar en las soluciones propuestas para el cálculo de riesgos.

## <a name="introduction"></a>Introducción

Los modelos de análisis de riesgos financieros normalmente se procesan como trabajos por lotes, con cargas de proceso pesadas que generan una gran demanda de capacidad de cálculo, acceso a datos y análisis. La demanda de cálculos de computación en malla de riesgos a menudo crece con el tiempo y, por consiguiente, la necesidad de recursos de proceso aumenta.

La amplia gama de productos disponibles y los servicios de Azure significa que puede haber más de una solución para la mayoría de los problemas. En este artículo se proporciona información general de las tecnologías, los patrones y las prácticas que se consideran más eficaces para una solución de computación en malla de riesgos en la banca utilizando [Microsoft Azure Batch](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr).

Azure Batch es un servicio gratuito que proporciona soluciones rentables y seguras tanto para la infraestructura como para las distintas fases del procesamiento por lotes que se utiliza normalmente con modelos de computación en malla de riesgos. Azure Batch puede aumentar, ampliar o incluso reemplazar las inversiones de recursos de proceso locales actuales mediante redes híbridas o moviendo todo el proceso de Batch a Azure.  Los datos pueden desplazarse hacia arriba y hacia abajo desde la nube o permanecer en el entorno local, mientras que los nodos de proceso pueden procesar otros datos en un modelo de ampliación a la nube cuando los recursos locales se agotan.

## <a name="anatomy-of-a-batch-run"></a>Anatomía de una ejecución de Batch

Normalmente hay al menos dos aplicaciones implicadas en una ejecución de Batch. Una aplicación, que normalmente se ejecuta en un “nodo principal”, envía el trabajo al grupo y a veces organiza los nodos de proceso. La orquestación también se puede configurar a través de Azure Portal. Los nodos de proceso ejecutan la otra aplicación como una tarea (consulte la figura 1).

La aplicación de nodos de proceso realiza la tarea de procesamiento en paralelo de archivos de modelado de riesgos. Puede haber más de una aplicación instalada y ejecutada en los nodos de proceso.

Estas aplicaciones se pueden cargar a través de la [API de Batch](/azure/batch/batch-apis-tools?WT.mc_id=gridbanksg-docs-dastarr), directamente a través de Azure Portal o a través de los [comandos de la CLI de Azure para Batch](/azure/batch/cli-samples?WT.mc_id=gridbanksg-docs-dastarr).

![Computación en malla de Azure Batch](./assets/risk-grid-compute-assets/05-batch-grid-computing.png)

**Figura 1:** Computación en malla de Azure Batch

Una ejecución de Batch consta de varios elementos lógicos. En la figura 2 se muestra el modelo lógico de un trabajo por lotes. Un grupo es un contenedor de las máquinas virtuales implicadas en la ejecución de Batch y aprovisiona las máquinas virtuales de nodos de proceso. Un grupo también es el contenedor de las aplicaciones instaladas en los nodos de proceso. Los trabajos se crean y ejecutan dentro del grupo. Los trabajos ejecutan las tareas. Las tareas son una ejecución de la aplicación de trabajo y se invocan mediante una instrucción de línea de comandos.

La aplicación de trabajo se instala en el nodo de proceso cuando se crea.

![Grupo, trabajos y tareas](./assets/risk-grid-compute-assets/06-pool-job-logical-model.png)

**Figura 2:** Modelo de concepto de Batch lógico

Cuando se ejecuta el trabajo, el grupo aprovisiona todas las máquinas virtuales de trabajo necesarias e instala las aplicaciones de trabajo. El trabajo asigna tareas a los nodos de proceso que, a su vez, ejecutan una instrucción de línea de comandos que normalmente llama a las aplicaciones instaladas o scripts.
Con Batch normalmente sigue un patrón de prototipo, que se describe a continuación:

1. Crear un grupo de recursos que contiene recursos de Batch.
2. Dentro del grupo de recursos, crear una cuenta de Batch.
3. Crear una cuenta de almacenamiento vinculada.
4. Crear un grupo en el que se van a aprovisionar las máquinas virtuales de trabajo.
5. Cargar la aplicación de nodos de proceso o los scripts en el grupo.
6. Crear un trabajo para asignar tareas a las máquinas virtuales del grupo.
7. Agregar el trabajo al grupo.
8. Comenzar la ejecución de Batch.
9. Las tareas de las colas de trabajo se ejecutan en los nodos de proceso.
10. Los nodos de proceso ejecutan las tareas a medida que las máquinas virtuales están disponibles.

En la figura 3 se muestra una ilustración de este proceso.

![Proceso de ejecución de Batch](./assets/risk-grid-compute-assets/07-batch-run-process.png)

**Figura 3:** Modelo de concepto de Batch lógico

Una vez que finalizan las tareas, puede ser útil quitar los nodos de proceso para no incurrir en gastos mientras no se usan. Para eliminarlos, mediante código o el portal, se puede eliminar el grupo contenedor, lo que quitará las máquinas virtuales de trabajo.

Para tutoriales más detallados sobre cómo empezar con Batch, hay disponibles [Inicios rápidos en 5 minutos](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) que le guiarán a través del proceso en varios idiomas o a través de Azure Portal.

## <a name="batch-process-scheduling"></a>Programación de procesos de Batch

Azure Batch tiene un programador integrado, por lo que la programación de cada ejecución se puede definir en el portal o a través de API. El programador de trabajos de Batch puede definir diferentes programaciones para desencadenar varios trabajos. Cada trabajo tiene sus propias propiedades, como qué hacer cuando el trabajo se inicia y finaliza. Las programaciones de trabajos se pueden establecer en intervalos periódicos o para una ejecución de un solo uso.

Muchos sistemas de computación en malla de la banca ya tienen su propio servicio de programación. Puede que no haya una necesidad inmediata de mover mi programador a Azure. Esto puede funcionar perfectamente porque Azure Batch se pueden invocar de forma manual o mediante un SDK, lo que permite que la programación siga teniendo lugar en el entorno local y que las cargas de trabajo se procesen en Azure.

El procesamiento de Batch puede ocurrir en una programación predeterminada o bajo demanda, pero en cualquier caso no hay ninguna necesidad de mantener las máquinas virtuales de nodos de proceso activas cuando no se usan.  Al usar cientos, si no miles, de nodos de proceso de máquina virtual, se pueden lograr ahorros significativos en los costos al desaprovisionar los servidores cuando terminan de ejecutar sus tareas en cola.

## <a name="compute-node-applications"></a>Aplicaciones de nodos de proceso

Los nodos de proceso necesitan que una aplicación se ejecute cuando se invoca una tarea. Estas aplicaciones están escritas por la empresa para realizar los trabajos de procesamiento cuando se instalan en los trabajos. En la computación en malla de riesgos para escenarios de la banca, esta aplicación a menudo asume la tarea de transformar los datos en formatos especialmente adecuados para el análisis descendente u otro procesamiento.

Cuando se proporciona la aplicación al grupo para su distribución a los nodos de proceso, se carga en un paquete de aplicación. Un paquete de aplicación puede ser otra versión de un paquete de aplicación previamente cargado. Se puede instalar más de un paquete de aplicación en un nodo de proceso. El trabajo contiene los paquetes de aplicación para cargar en los equipos de trabajado.

La implementación del paquete de aplicación también puede administrarse por versión. Si varias versiones de un paquete de aplicación se han cargado en un grupo, se puede designar una versión específica para su uso en una ejecución de Batch como se muestra en la figura 4. Esto puede ser necesario en entornos de auditoría o cuando la empresa desea reproducir una ejecución anterior. También se puede usar para fines de reversión si se introduce un error en la aplicación de trabajo.

![Proceso de ejecución de Batch](./assets/risk-grid-compute-assets/08-versioning-worker-applications.png)

**Figura 4:** Control de versiones de las aplicaciones de tarea de nodos de proceso

Un paquete de aplicación se carga en el grupo como un archivo .zip que contiene los binarios de aplicación y los archivos auxiliares que se requieren para que las tareas ejecuten la aplicación. Hay 2 ámbitos para los paquetes de aplicación. Puede designar un paquete de aplicación en el ámbito del grupo o en el ámbito de las tareas.

## <a name="pool-application-packages"></a>Paquetes de aplicación de grupo

Estos paquetes se implementan en cada nodo de proceso en el grupo. Cuando una máquina virtual de nodos de proceso se aprovisiona o reinicia, o su imagen inicial se restablece, se instala una nueva copia de los paquetes de aplicación de grupo, en caso de que exista una aplicación actualizada. Uno o varios paquetes de aplicación pueden asignarse a un grupo, lo que significa que los nodos de proceso obtendrán todos los paquetes designados.

## <a name="task-application-packages"></a>Paquetes de aplicación de tarea

Los paquetes de aplicación que tienen como destino el nivel de tarea solo se instalan en nodos de proceso programados para ejecutar una tarea. Los paquetes de aplicaciones de tarea están diseñados para usarse cuando se ejecuta más de un trabajo en un grupo.

Las aplicaciones de tarea son útiles cuando se agregan datos generados por los trabajos de nivel de grupo, que pueden ser relevantes en escenarios de computación en malla de riesgos. Por ejemplo, una aplicación de tarea puede ejecutar un conjunto de cálculos de riesgos que generan datos que se usarán más adelante en el flujo de trabajo del cálculo de riesgos.

## <a name="scaling-batch-jobs"></a>Escalado de trabajos por lotes

Los bancos a menudo realizan ejecuciones por lotes de análisis de riesgos durante los fines de semana o por la noche cuando los recursos informáticos están infrautilizados. Si bien este modelo funciona para algunos, puede quedarse pequeño rápidamente, lo que requiere más capital para agregar más máquinas de trabajo a la red.

Si los trabajos de Batch tardan demasiado tiempo en ejecutarse, o si desea más capacidad de computación en las ejecuciones de Batch, Azure ofrece varias opciones.

1. Asignar más máquinas de nodo de proceso para escalar horizontalmente.
2. Asignar más máquinas de nodo de proceso versátiles para escalar verticalmente. Las máquinas de Azure se pueden aprovisionar para satisfacer las necesidades de alto rendimiento de los núcleos y la memoria, e incluso la capacidad de computación de GPU.

> Nota: El uso de Microsoft HPC Pack con Batch es un modelo más complejo y no se trata en este artículo.

En un clúster de procesamiento de Batch, puede haber tan solo dos máquinas virtuales de procesamiento, o miles de tareas simultáneas que se ejecutan en miles de nodos de proceso de máquinas virtuales con decenas de miles de núcleos. Cada máquina virtual es responsable de ejecutar una única tarea a la vez. El número de máquinas virtuales de un grupo se puede escalar manual o automáticamente según la configuración cuando la carga aumenta o disminuye.

### <a name="burst-to-cloud"></a>Ampliación a la nube

Cuando los recursos de proceso de una malla local se están agotando debido a la ejecución de un trabajo de análisis de gran tamaño, la “ampliación a la nube” ofrece una manera de aumentar esos recursos mediante la incorporación de más nodos de proceso en Azure. La ampliación a la nube es un modelo en el que la infraestructura o las nubes privadas distribuyen su carga de trabajo a los servidores en la nube cuando la demanda es alta para los recursos locales.

Estos nodos de proceso pueden preconfigurarse como máquinas virtuales Linux o Windows para aprovisionarse en la plataforma IaaS de Azure. Además, los servidores se pueden aprovisionar y configurar automáticamente para trabajar con las inversiones existentes, como Tibco Gridserver e IBM Symphony.

### <a name="automatic-scaling-formulas"></a>Fórmulas de escalado automático

Esta flexibilidad puede configurarse en Azure Portal o mediante el uso de [fórmulas de escalado automático](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr). Las fórmulas de escalado automático son scripts cargados en el programador de procesamiento de Batch para un mayor control del comportamiento de Batch. El escalado automático en un grupo de nodos de proceso se realiza mediante la asociación de los nodos con fórmulas de escalado automático.

A continuación se muestra un ejemplo de una fórmula de escalado automático que dirige dicho escalado para comenzar con una máquina virtual y escalar verticalmente hasta cincuenta máquinas virtuales según sea necesario. Cuando las tareas finalizan, las máquinas virtuales quedan libres una a una y la fórmula de escalado automático reduce el grupo.

```Formula
startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

### <a name="other-scaling-techniques"></a>Otras técnicas de escalado

El escalado automático también se puede habilitar mediante el cmdlet de PowerShell Enable-AzureBatchAutoScale. El cmdlet Enable-AzureBatchAutoScale permite el escalado automático del grupo especificado. A continuación, encontrará un ejemplo.

1. El primer comando define una fórmula y luego la guarda en la variable `$Formula`.
2. El segundo comando habilita el escalado automático en el grupo denominado `RiskGridPool` mediante la fórmula de `$Formula`.

```console
C:\> $Formula = ‘startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second?WT.mc_id=gridbanksg-docs-dastarr);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);’;

C:\> Enable-AzureBatchAutoScale -Id "RiskGridPool" -AutoScaleFormula $Formula -BatchContext $Context
```

El escalado también puede lograrse mediante la CLI de Azure con el comando `az batch pool resize` y Azure Portal.

## <a name="data-storage-and-retention"></a>Almacenamiento y retención de datos

Una vez que un nodo de proceso ingiere y procesa los datos, los datos de salida resultantes se pueden almacenar en una base de datos para su posterior procesamiento y análisis o transformarse en la ingesta antes del almacenamiento para garantizar los formatos adecuados para el procesamiento descendente. Microsoft Azure ofrece varias opciones de almacenamiento. La elección de [qué tecnología de almacenamiento de datos utilizar](/azure/architecture/data-guide/?WT.mc_id=gridbanksg-docs-dastarr) depende en gran medida del análisis y/o las necesidades de informes de los procesos descendentes.

Cuando se usa una red híbrida, el destino de almacenamiento de datos puede ser local. Cuando se usa Batch a través de una red híbrida, los nodos de proceso pueden volver a escribir datos en un almacén de datos local sin usar una ubicación de almacenamiento basada en Azure. Los trabajos también pueden escribir en el almacenamiento de archivos de Azure, que se puede montar como un disco en una máquina local para facilitar el acceso a cualquier proceso que funciona con los archivos locales.

## <a name="monitoring-and-logging"></a>Supervisión y registro

Para optimizar futuras ejecuciones del trabajo de Batch, se deben registrar datos para ayudar a identificar áreas de optimización. Por ejemplo, si los trabajos se ejecutan cerca de la capacidad de la CPU, la incorporación de núcleos a los nodos de proceso puede ayudar a evitar la vinculación a la CPU y el trabajo puede finalizar con mayor rapidez. Cada ejecución de aplicación en el trabajo de Batch tiene sus propias características y las optimizaciones realizadas en las máquinas virtuales de las ejecuciones de Batch pueden ser diferentes. Para tareas de uso intensivo de memoria, se puede asignar más memoria mediante la configuración de las máquinas de manera diferente en la próxima ejecución.

El registro se puede realizar mediante el nodo de proceso y las aplicaciones principales de malla o mediante un trabajo que use el [registro de diagnóstico de Batch](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr). La información de registro sobre el rendimiento de las ejecuciones de Batch se puede configurar para ayudar a identificar las áreas que son susceptibles de optimizar para mejorar el rendimiento y acelerar la finalización de las tareas.

### <a name="custom-batch-monitoring-and-logging"></a>Personalización de la supervisión y el registro de Batch

La aplicación de control y las aplicaciones de nodo de proceso pueden generar estos datos y almacenarlos para su posterior análisis. Los datos que se consideran útiles para optimizar los trabajos de Batch incluyen:

- Horas de inicio y finalización de cada tarea
- El tiempo que cada nodo de proceso está activo y ejecutando tareas
- El tiempo que cada nodo de proceso está activo y no está ejecutando tareas
- El tiempo de ejecución de trabajos por lotes general

### <a name="batch-diagnostic-logging"></a>Registro de diagnóstico de Batch

Hay una alternativa al uso del controlador y las aplicaciones de nodo de proceso para emitir los datos de instrumentación. El [registro de diagnóstico de Batch](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr) puede capturar una gran cantidad de datos de ejecución. El registro de diagnóstico de Batch no está habilitado de forma predeterminada y debe habilitarse para la cuenta de Batch.

El registro de diagnóstico de Batch proporciona una cantidad significativa de datos para ayudar a solucionar problemas y optimizar las ejecuciones de Batch. Horas de inicio y finalización del trabajo y las tareas, número de núcleos, número de nodos total y muchas otras métricas.

El registro de Batch requiere un destino de almacenamiento para los registros emitidos, almacenar los eventos que la ejecución de Batch genera, como la creación de grupos, la ejecución de trabajos, la ejecución de tareas, etc. Además de almacenar los eventos de registro de diagnóstico en una cuenta de Azure Storage, los eventos del registro del servicio Batch se pueden transmitir a un [centro de eventos de Azure](/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=gridbanksg-docs-dastarr) y enviarse a [Azure Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=gridbanksg-docs-dastarr).

Con estos datos, se pueden optimizar la computación principal y las aplicaciones de nodo principal. Esto puede reducir los costos debido a situaciones como el desaprovisionamiento más rápido de máquinas virtuales de trabajo cuando ya no se necesitan, en lugar de esperar a que la ejecución de Batch termine.

### <a name="batch-management-tools"></a>Herramientas de administración de Batch

Azure Portal proporciona un panel de supervisión de Batch que muestra información sobre Batch, como los trabajos que se están ejecutando e incluso el uso de cuota de cuenta. Es suficiente para muchas aplicaciones de trabajo de Batch.

Además de las herramientas de administración y visualización de Batch disponibles en Azure Portal, hay una herramienta gratuita de código abierto ([BatchLabs](https://github.com/Azure/BatchLabs)) para administrar Batch. Es una herramienta de cliente independiente, completa y gratuita que puede ayudarle a crear, depurar y supervisar aplicaciones de Azure Batch. Descargue un paquete de instalación para Mac, Linux o Windows.

## <a name="network-models"></a>Modelos de red

El análisis de riesgos a menudo requiere cientos, si no miles, de documentos que se ingieren en el proceso de computación en malla de riesgos. Estos archivos suelen ubicarse en local en un almacén de archivos, un recurso compartido de red o en otro repositorio. Al usar máquinas virtuales basadas en Azure para acceder a esos archivos y procesarlos, suele resultar útil que la red local se conecte sin problemas a la red de Azure, de forma que el acceso a los archivos sea sencillo y rápido. Este enfoque puede incluso significar que no se necesitan cambios de código para el código que realiza el procesamiento en los nodos de proceso.

Azure ofrece dos modelos para conectar de manera segura y confiable los sistemas actuales locales con Azure: [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr) y [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr). Ambos ofrecen conectividad confiable y segura, aunque existen diferencias en la implementación, el rendimiento y [otros atributos](/azure/networking/networking-overview?WT.mc_id=gridbanksg-docs-dastarr).

Como alternativa, el nodo principal de la computación en malla de riesgos puede encontrarse en el entorno local y ejecutar el trabajo de Batch a través de API de REST o SDK en .NET y otros lenguajes.

Hay otras técnicas para llenar el vacío entre Azure y los recursos locales sin una solución de red híbrida. A continuación se proporciona más información sobre esto.

### <a name="expressroute"></a>ExpressRoute

ExpressRoute une su red local o de centro de datos a Azure a través de una conexión privada facilitada por un asociado de conectividad, como su proveedor de servicios de Internet actual (ISP?WT.mc_id=gridbanksg-docs-dastarr). Esto permite que ambas redes se vean entre sí como la misma instancia de red, lo que proporciona acceso sin problemas entre las redes. La integración de las redes es fundamental cuando desea integrar sistemas locales existentes con una red de Azure y ExpressRoute ofrece las velocidades de conexión más rápidas posible.

[Aquí puede encontrar](https://azure.microsoft.com/en-us/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr) más información sobre precios de Azure ExpressRoute.

### <a name="vpn-gateway"></a>VPN Gateway

VPN Gateway es otra manera de conectar la red a Azure. La desventaja de este modelo es que el tráfico fluye a través de Internet. El resultado es que la conexión puede ser menos resistente y las velocidades de la red no pueden alcanzar las de ExpressRoute; sin embargo, esto puede no ser una barrera para un escenario de computación en malla de riesgos, ya que la lectura de los archivos de datos suele ser una operación rápida.

[Aquí puede encontrar](https://azure.microsoft.com/en-us/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr) más información sobre precios de VPN Gateway.

### <a name="choices-for-connectivity-details"></a>Detalles sobre las opciones de conectividad

Existen básicamente dos modelos para ampliar la red a Azure, como se muestra en la figura 5.

1. Puerta de enlace virtual: de sitio a sitio
2. ExpressRoute: proveedor de Exchange o ISP

![Sitio a sitio y ExpressRoute](./assets/risk-grid-compute-assets/10-s2s-expressroute.png)

**Figura 5:** Sitio a sitio y ExpressRoute

#### <a name="virtual-gateway-site-to-site-integration"></a>Integración de sitio a sitio con puerta de enlace virtual

[VPN Gateway de sitio a sitio](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal?WT.mc_id=gridbanksg-docs-dastarr) conecta la red local a una red virtual de Azure. Esto cierra la brecha entre las redes esencialmente haciendo que formen parte de la misma red, con acceso bidireccional a recursos, servidores y artefactos. Esto permite un acceso directo a archivos de datos desde las máquinas virtuales de trabajo de Azure que ejecutan el trabajo por lotes de la computación en malla de riesgos.

#### <a name="expressroute-integration"></a>Integración de ExpressRoute

Una conexión de ExpressRoute facilitada por un proveedor de red asociado de Azure obtiene los mismos beneficios que una conexión de sitio a sitio, pero con velocidades y confiabilidad más altas.

Obtenga más información sobre [modelos de conectividad de ExpressRoute] (/azure/expressroute/expressroute-connectivity-models?WT.mc_id=gridbanksg-docs-dastarr).

### <a name="batch-processing-without-an-azure-hybrid-network"></a>Procesamiento por lotes sin una red híbrida de Azure

Otro escenario de Batch es cargar todos los archivos de datos en el almacenamiento de Azure para que posteriormente los procesen las máquinas de proceso basadas en Azure. El almacenamiento de archivos y el almacenamiento de blobs son candidatos probables para almacenar datos de computación en malla de riesgos.

En este escenario, el controlador de trabajos y todos los nodos de proceso se encuentran en Azure como se muestra en la figura 6. El destino probable para los datos procesados es un almacén de datos de Azure, en preparación para el procesamiento posterior por parte de las soluciones de Azure Machine Learning u otros sistemas. Este procesamiento adicional queda fuera del ámbito de este artículo.

![Sitio a sitio y ExpressRoute](./assets/risk-grid-compute-assets/09-batch-upload-process.png)

**Figura 6:** Carga por lotes al ciclo de vida de ejecución

### <a name="hybrid-network-connectivity-resources"></a>Recursos de conectividad de red híbrida

Se pueden aplicar varias configuraciones en su situación. Para ayudar con las decisiones y la guía arquitectónica con respecto a la conexión de la red a Azure, consulte el artículo _[Conexión de una red local a Azure](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr)_ del grupo de asociados y prácticas.

- [Consulte este artículo](/azure/vpn-gateway/vpn-gateway-about-vpngateways?WT.mc_id=gridbanksg-docs-dastarr) para las alternativas de la configuración de VPN Gateway.
- Información acerca de los [Modelos de conectividad de ExpressRoute](/azure/expressroute/expressroute-connectivity-models?WT.mc_id=gridbanksg-docs-dastarr).
- Calcule los [precios de ExpressRoute](https://azure.microsoft.com/en-us/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr).
- Calcule los [precios de VPN Gateway](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr).

## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Se puede crear una [red virtual (VNet)](/azure/virtual-network/virtual-networks-overview?WT.mc_id=gridbanksg-docs-dastarr) de Azure así como los nodos de proceso del grupo dentro de ella. Esto proporciona un nivel adicional de aislamiento para las ejecuciones de Batch y permite la autenticación mediante [Azure Active Directory (AAD)](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr). Consulte [Configuración de la red del grupo](/azure/batch/batch-api-basics#pool-network-configuration?WT.mc_id=gridbanksg-docs-dastarr) para más información.

Hay dos maneras de autenticar una aplicación de Batch mediante AAD:

1. **Autenticación integrada**. Una aplicación de Batch que usa cuentas de AAD puede usar la cuenta para obtener recursos a almacenes de datos y otros recursos.

2. **Entidad de servicio**. Las entidades de servicio de AAD definen la directiva de acceso y los permisos de los usuarios y las aplicaciones. Una entidad de servicio proporciona autenticación para los usuarios mediante una clave secreta asociada a esa aplicación. Esto permite autenticar una aplicación desatendida con una clave secreta. Una entidad de servicio define la directiva y los permisos de una aplicación para representar dicha aplicación cuando se accede a los recursos en tiempo de ejecución. [Obtenga más información aquí](/azure/active-directory/develop/active-directory-application-objects?WT.mc_id=gridbanksg-docs-dastarr).

Para más información sobre la seguridad en el procesamiento por lotes con AAD, [consulte este artículo](/azure/batch/batch-aad-auth?WT.mc_id=gridbanksg-docs-dastarr).

El servicio Batch también se puede autenticar con una clave compartida. El servicio de autenticación requiere que se agreguen dos valores de encabezado a la solicitud HTTP, los datos y la autorización. [Consulte aquí para más información](/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service?WT.mc_id=gridbanksg-docs-dastarr) sobre la autenticación mediante clave compartida.

## <a name="cost-considerations"></a>Consideraciones sobre el costo

El uso de Azure Batch es gratuito. Solo deberá pagar por los recursos subyacentes que utilice, como el tiempo de actividad de las máquinas virtuales, el almacenamiento y las redes. Sin embargo, las máquinas virtuales de nodos de proceso todavía cuestan dinero cuando están inactivas, por lo que es una buena idea desaprovisionarlas cuando ya no se necesitan. A menudo, esto se hace mediante la eliminación del grupo que las contiene.
>
Al crear un grupo, puede especificar los tipos de nodos de proceso que le interesan y un número de destino para cada uno. Los dos tipos de nodos de proceso son los siguientes:
>
>**Nodos de proceso dedicados**, que están reservados a las cargas de trabajo que usted tenga. Son más caros que los nodos de prioridad baja, pero se garantiza que nunca se verán reemplazados.
>
>**Nodos de proceso de baja prioridad**, que aprovechan la capacidad sobrante en Azure para ejecutar las cargas de trabajo de Batch. Los nodos de prioridad baja son menos costosos por hora que los nodos dedicados y habilitan las cargas de trabajo que requieren una gran potencia de proceso. Para obtener más información, vea [Use low-priority VMs with Batch](/azure/batch/batch-low-pri-vms?WT.mc_id=gridbanksg-docs-dastarr) (Usar máquinas virtuales de prioridad baja con Batch).
>
>Los nodos dedicados y de baja prioridad pueden existir en el mismo grupo.
>
>Para obtener información sobre los precios de los nodos de proceso de prioridad baja y dedicados, vea [Precios de Batch](https://azure.microsoft.com/pricing/details/batch/?WT.mc_id=gridbanksg-docs-dastarr).

Cuando se usa el servicio de registro de diagnóstico de Batch, la emisión de datos al almacenamiento de Azure conlleva un costo. Estos son datos de almacenamiento como cualquier otro dato y el precio se ve afectado por la cantidad de datos de diagnóstico retenidos.

## <a name="getting-started"></a>Introducción

Si bien hay muchos lugares para comenzar con un dominio complejo como la computación de Batch para la computación en malla de riesgos, aquí hay algunos puntos de partida lógicos para entender mejor la tecnología de Batch.

[Se trata de un excelente lugar para comenzar](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) a trabajar con Batch usando ejemplos de Azure Portal y código. Las aplicaciones de ejemplo de Azure Batch también están disponibles gratuitamente [en GitHub](https://github.com/Azure/azure-batch-samples).

A continuación se muestran algunos tutoriales rápidos para ayudarle a crear una aplicación sencilla para crear y ejecutar trabajos de proceso por lotes. Estas son las opciones para crear la aplicación:

- [API de .NET de Batch](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [SDK de Batch para Python](/azure/batch/batch-python-tutorial?WT.mc_id=gridbanksg-docs-dastarr)
- [SDK de Batch para Node.js](/azure/batch/batch-nodejs-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Administración de Batch con PowerShell](/azure/batch/batch-powershell-cmdlets-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Administración de Batch con la CLI de Azure](/azure/batch/batch-cli-get-started?WT.mc_id=gridbanksg-docs-dastarr)

Considere la posibilidad de llevar a cabo una iniciativa de prueba de concepto. ¿Cuál será su enfoque para la ingesta de datos en Azure? ¿Usará una red híbrida o cargará datos a través de un SDK o una interfaz de REST? Si está pensando en una red híbrida, considere la posibilidad de iniciar un piloto para implementar todo esto.

Evalúe el tamaño de los trabajos de proceso de Batch y seleccione la solución de escalado correcta. Las [fórmulas de escalado automático](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr) habilitan escenarios de programación complejos mientras que los escenarios más sencillos se logran con Azure Portal.

## <a name="technologies-presented"></a>Tecnologías presentadas

[Azure Batch](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) proporciona funcionalidades para ejecutar trabajos de procesamiento en paralelo a gran escala en la nube.

[Azure Active Directory](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr) es un servicio multiinquilino de administración de identidades y de directorios basado en la nube que combina los servicios de directorio principales, la administración del acceso de las aplicaciones y la protección de identidades en una única solución.

Las [fórmulas de escalado automático](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr) son scripts cargados en el programador de procesamiento por lotes para un mayor control de los comportamientos de escalado de Batch.

El [registro de diagnóstico de Batch](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr) es una característica de Azure Batch que permite la creación de un registro detallado de las ejecuciones de Batch y los eventos generados. Los registros se almacenan en Azure Storage.

[BatchLabs](https://github.com/Azure/BatchLabs) es una aplicación independiente para la administración y supervisión de Batch disponible en Windows, MacOS y Linux.

[ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr) es una solución de red híbrida de alta velocidad y confiabilidad para unir redes locales y de Azure.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr) es una solución de red híbrida que usa Internet para unir redes locales y de Azure.

## <a name="conclusion"></a>Conclusión

En este documento se proporcionó información general sobre las soluciones técnicas y las consideraciones al usar Azure Batch para computación en malla de riesgos para la banca. En el artículo se ha tratado mucha información, desde la definición de Azure Batch hasta las [opciones de red](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr), e incluso se han planteado las consideraciones relacionadas con el costo.

Si piensa seguir avanzando en la evaluación de Azure Batch para la computación en malla de riesgos, [esta página](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr) es un buen recurso para comenzar. Proporciona tutoriales de ejemplo guiados para el procesamiento de archivos en paralelo, que es inherente en computación en malla de riesgos. Los tutoriales se proporcionan mediante Azure Portal, la CLI de Azure, .NET y Python.
