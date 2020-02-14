---
title: Introducción a la migración de una solución de comercio electrónico a Azure
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: En este artículo se explican las fases de migración de la infraestructura de comercio electrónico del entorno local a Azure.
ms.openlocfilehash: e918f1157dc2bc42a6c4d0decfef95a8daa7ccf0
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054191"
---
# <a name="migrating-your-e-commerce-solution-to-azure-overview"></a>Introducción a la migración de una solución de comercio electrónico a Azure

## <a name="introduction"></a>Introducción

Migrar una solución de comercio electrónico existente a la nube supone muchas ventajas para una empresa: permite a escalabilidad, ofrece a los clientes accesibilidad todo el día, todos los días, y facilita la integración de los servicios en la nube. Pero, primero que todo, migrar una solución de comercio electrónico a la nube es una tarea importante, que costos que el responsable de la toma de decisiones debe tener bien claros. En este documento se explica el ámbito de una migración de Azure con el objetivo de informarle las opciones. La primera fase empieza con los profesionales de TI que migran los componentes a la nube. Una vez en Azure, se describen los pasos que el equipo de comercio electrónico puede seguir para aumentar la rentabilidad de la inversión y aprovechar la nube.

**En la encrucijada**

Si bien las transacciones de comercio electrónico global solo representan una fracción de las ventas minoristas, el canal sigue viendo un crecimiento constante año a año. En 2017, el comercio electrónico representó el 10,2 % del total de las ventas minoristas, un alza desde el 8,6 % en 2016 ([fuente](https://www.emarketer.com/Report/Worldwide-Retail-Ecommerce-Sales-eMarketers-Updated-Forecast-New-Mcommerce-Estimates-20162021/2002182)). A medida que el comercio electrónico ha madurado, junto con la llegada de la informática en la nube, los comerciantes minoristas se encuentran en una encrucijada.  Es momento de que tomen decisiones. Pueden imaginarse su modelo de negocio con nuevas funcionalidades posibles gracias a la tecnología en constante evolución y también pueden planear su modernización dada su actual superficie de funcionalidades.

**Mejora del recorrido del cliente**

El comercio electrónico, que se centra principalmente en el recorrido del cliente, tiene muchos atributos distintos. Estos atributos se pueden agrupar en cuatro áreas principales: detección, evaluación, compra y después de la compra.

El comportamiento del cliente se captura como datos. Finalmente, el embudo de la compra es una colección de puntos de conexión a las aplicaciones que se usan para ver los datos de los productos, las transacciones, el inventario, el envío, el cumplimiento de pedidos, el perfil del usuario, el carro de la compra y recomendaciones de productos, por mencionar algunos aspectos.

Un negocio minorista típico se basa en una gran colección de soluciones de software que van desde aplicaciones orientadas al cliente hasta la pila y aplicaciones fundamentales.  En la ilustración siguiente se observa una vista de la funcionalidad presente en un negocio minorista típico.

 ![](./assets/migrating-ecommerce-solution-to-azure/ecommerce-system-sketch.png)

La nube presenta una oportunidad para cambiar la forma en que una organización obtiene, usa y administra la tecnología.  Entre las ventajas se encuentran las siguientes: menores costos de mantenimiento de los centros de datos, mejor confiabilidad y rendimiento y la flexibilidad para agregar otros servicios. En este caso de uso, nos centramos en una de las rutas que un negocio minorista puede tomar para migrar su infraestructura existente a Azure. También aprovechamos el nuevo entorno mediante el uso de un enfoque por fases de rehospedaje, refactorización y recompilación. Si bien es posible que muchas organizaciones sigan esta ruta en serie hacia la modernización, en la mayoría de los casos, las organizaciones pueden usar cualquier fase como su punto de partida.  Las organizaciones pueden elegir abstenerse de rehospedar su aplicación actual en Azure y pasar directamente a la refactorización o, incluso, a la recompilación.  Esta decisión será única para la aplicación y la organización para cumplir mejor con sus necesidades de modernización.

## <a name="rehost"></a>Rehospedaje

Esta etapa, también denominada como &quot;migración mediante lift-and-shit&quot;, implica la migración de servidores físicos y máquinas virtuales tal cual a la nube. Mediante un simple cambio del entorno de servidor actual directamente a IaaS, se aprovechan los ahorros de costos, la seguridad y la mayor confiabilidad. Los ahorros provienen de técnicas como la ejecución de cargas de trabajo en máquinas virtuales del tamaño adecuado. En la actualidad, las funcionalidades de las máquinas físicas y las máquinas virtuales locales suelen exceder las necesidades diarias de los minoristas. Las máquinas virtuales deben ser capaces de atender los picos estacionales de negocio que solo se producen algunas veces al año. Por tanto, paga por funcionalidades que no usa durante la temporada de menor demanda. Con Azure, puede elegir la máquina virtual de tamaño adecuado según las exigencias del ciclo comercial actual.

Hay tres fases para el rehospedaje en Azure:

- **Análisis**: identifique y haga un inventario de los recursos locales como aplicaciones, cargas de trabajo, redes y seguridad. Al finalizar esta fase, contará con la documentación completa del sistema existente.
- **Migración**: migre cada subsistema desde el entorno local a Azure. En esta etapa, usará Azure como una extensión del centro de datos con aplicaciones que siguen comunicándose.
- **Optimización**: a medida que los sistemas se migran a Azure, asegúrese de que las cosas tienen el tamaño adecuado. Si el entorno muestra que hay asignados demasiados recursos a algunas máquinas virtuales, cambie el tipo de máquina virtual a uno que tenga una combinación más adecuada de CPU, memoria y almacenamiento local.

### <a name="analyze"></a>Analizar

Haga lo siguiente:

1. Enumere los servidores y las aplicaciones locales. Esto se basa en un agente o una herramienta de administración para recopilar metadatos sobre los servidores, las aplicaciones que se ejecutan en lo servidores, el uso del servidor actual y cómo están configurados los servidores y sus aplicaciones. Este resultado es un informe de todos los servidores y las aplicaciones del entorno.
1. Identifique las dependencias. Puede usar herramientas para identificar qué servidores se comunican entre sí y qué aplicaciones hacen lo mismo. El resultado es un mapa (o mapas) de todas las aplicaciones y cargas de trabajo. Estos mapas contribuyen a planear la migración.
1. Analice las configuraciones. El objetivo es saber qué tipos de máquina virtual necesita una vez en Azure. El resultado es un informe sobre todas las aplicaciones que se pueden migrar a Azure. Además, se pueden clasificar de esta manera:
      1. Sin modificaciones
      1. Modificaciones básicas, como cambios de nombres
      1. Modificaciones menores, como leves cambios de código
      1. Cargas de trabajo incompatibles que requieren esfuerzo adicional para migrarlas
1. Cree su presupuesto. Ahora tiene una lista que enumera cada CPU (memoria, etc.) y los requisitos de cada aplicación. Coloque esas cargas de trabajo en máquinas virtuales de tamaño adecuado. Las plataformas en la nube facturan costos en función del uso. Existen herramientas para asignar sus necesidades a las máquinas virtuales de Azure del tamaño adecuado. Si migra máquinas virtuales Windows o SQL Server, también debe consultar la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/?WT.mc_id=retailecomm-docs-scseely), que disminuye sus gastos en Azure.

Microsoft brinda varias herramientas para analizar y catalogar los sistemas. Si ejecuta VMware, puede usar [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely) para ayudar en la detección y la evaluación. La herramienta identifica las máquinas que se pueden migrar a Azure, recomienda el tipo de máquina virtual a ejecutar y calcula el costo de la carga de trabajo. En el caso de los entornos de Hyper-V, use [Azure Site Recovery Deployment Planner](/azure/site-recovery/hyper-v-deployment-planner-overview?WT.mc_id=retailecomm-docs-scseely). En el caso de migraciones de gran tamaño donde deba migrar cientos de máquinas virtuales o más, es posible que quiera trabajar con un [asociado para la migración de Azure](https://azure.microsoft.com/migration/partners/?WT.mc_id=retailecomm-docs-scseely). Estos asociados tienen los conocimientos y la experiencia que se necesitan para migrar las cargas de trabajo.

### <a name="migrate"></a>Migrar

Comience a planear los servicios que se van a migrar a la nube y en qué orden se hará. Dado que esta etapa implica la migración de cargas de trabajo, siga este orden:

1. Cree la red.
2. Incorpore un sistema de identidad (Azure Active Directory).
3. Aprovisione los elementos de almacenamiento en Azure.

Durante la migración, el entorno de Azure es una extensión de la red local. Puede conectar las redes lógicas con [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely). Puede elegir usar [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely) para mantener las comunicaciones entre la red y Azure en una conexión privada que nunca afecta a Internet. También puede usar una VPN de sitio a sitio donde una instancia de Azure VPN Gateway se comunica con el dispositivo VPN local con todo el tráfico enviado de manera segura mediante el uso de comunicación cifrada entre Azure y la red. Publicamos [aquí](/azure/architecture/reference-architectures/hybrid-networking/vpn?WT.mc_id=retailecomm-docs-scseely) una arquitectura de referencia que detalla cómo configurar una red híbrida.

Una vez configurada la red, planee la continuidad empresarial. Una recomendación es usar la replicación en tiempo real para mover los datos de forma local en la nube y para asegurarse de que la nube y los datos existentes son los mismos. Las tiendas de comercio electrónico no cierran nunca, la duplicación brinda la capacidad de cambiar del entorno local a Azure sin apenas afectar a los clientes.

Empiece a migrar los datos, las aplicaciones y los servidores relacionados a Azure. Muchas empresas usan el servicio [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely) para hacer la migración a Azure. El servicio tiene como destino la continuidad empresarial y recuperación ante desastres (BCDR). Esto resulta ideal para una migración desde el entorno local a Azure. El equipo de implementación puede leer [aquí](/azure/site-recovery/migrate-tutorial-on-premises-azure?WT.mc_id=retailecomm-docs-scseely) los detalles sobre cómo migrar servidores físicos y máquinas virtuales locales a Azure.

Una vez que se migra un subsistema a Azure, haga una prueba para asegurarse de que todo funciona según lo previsto. Una vez solucionados todos los problemas, migre las cargas de trabajo a Azure.

### <a name="optimize"></a>Optimización

En este momento, el usuario seguirá supervisando el entorno y cambiará las opciones de proceso subyacentes para ajustarse a las cargas de trabajo a medida que cambia el entorno. Quienquiera que supervise el estado del entorno debe vigilar cuánto se usa cada recurso. El objetivo debe ser tener una utilización de entre el 75 % y el 90 % en la mayoría de las máquinas virtuales. En el caso de las máquinas virtuales con una utilización excepcionalmente baja, considere la posibilidad de empaquetarlas con más aplicaciones o migrarlas a las máquinas virtuales de menor costo en Azure que conservan el nivel adecuado de rendimiento.

Azure también brinda herramientas para optimizar el entorno. [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely) supervisa los componentes del entorno y brinda recomendaciones personalizadas en función de los procedimientos recomendados. Las recomendaciones ayudan a mejorar el rendimiento, la seguridad y la disponibilidad de los recursos que se usan en las aplicaciones. Azure Portal también expone información sobre el estado de las aplicaciones. Las máquinas virtuales debe aprovechar las [extensiones de máquina virtual de Azure para Linux y Windows](/azure/virtual-machines/extensions/overview?WT.mc_id=retailecomm-docs-scseely). Dichas extensiones proporcionan configuración posterior a la implementación, antivirus, supervisión de aplicaciones, etc. También puede aprovechar muchos otros servicios de Azure para el diagnóstico de red, el uso del servicio y las alertas a través de servicios como [Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview?WT.mc_id=retailecomm-docs-scseely), [Service Map](/azure/monitoring/monitoring-walkthrough-servicemap?WT.mc_id=retailecomm-docs-scseely), [Application Insights](/azure/application-insights/app-insights-overview?WT.mc_id=retailecomm-docs-scseely) y [Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=retailecomm-docs-scseely).

Si bien hay partes de la organización que están optimizando ahora el sistema en Azure, los equipos de desarrollo pueden empezar a avanzar a la fase posterior a la migración: la refactorización.

## <a name="refactor"></a>Refactorización

Una vez que se completa la migración, la aplicación de comercio electrónico puede empezar a aprovechar su nuevo hogar en Azure. No es necesario que la fase de refactorización espere hasta que se haya migrado todo el entorno. Si migró el equipo de CMS, pero el equipo de ERP no, no hay ningún problema. El equipo de CMS puede empezar de todos modos sus trabajos de refactorización. Esta fase implica el uso de servicios adicionales de Azure para optimizar el costo, la confiabilidad y el rendimiento mediante la refactorización de las aplicaciones. Mientras en la migración mediante lift-and-shift solo se aprovechaba el sistema operativo y hardware administrado por el proveedor, en este modelo también se aprovechan los servicios en la nube para disminuir el costo. Seguirá usando la aplicación actual tal cual, con pequeños cambios de configuración o en el código de las aplicaciones, y conectará la aplicación a nuevos servicios de infraestructura, como contenedores, bases de datos y sistemas de administración de identidades.

El trabajo de refactorización cambia muy poco el código y la configuración. El usuario se centrará más tiempo en la automatización, principalmente porque las tecnologías que se adoptan en esta fase se basan en scripts para crear e implementar los recursos; las instrucciones de implementación son un script.

Si bien muchos se pueden usar muchos de los servicios de Azure, nos centraremos en los servicios más comunes que se usan en la fase de refactorización: contenedores, servicios de aplicación y servicios de base de datos. ¿Por qué nos interesa la refactorización? La refactorización proporciona una base de código sólida que disminuye los costos a largo plazo al mantener la deuda de código dentro de lo razonable.

Los contenedores ofrecen una forma de empaquetar las aplicaciones. Debido al modo en que un contenedor virtualiza el sistema operativo, puede empaquetar varios contenedores en una sola máquina virtual. Puede mover una aplicación a un contenedor con pocos cambios o ningún cambio. Es posible que necesite cambios de configuración. Este trabajo también lleva a escribir scripts que empaquetan aplicaciones en un contenedor. Los equipos de desarrollo dedicarán su tiempo de refactorización a escribir y probar estos scripts. Azure admite la inclusión en contenedores mediante los servicios [Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely) (AKS) y [Azure Container Registry](https://azure.microsoft.com/services/container-registry/?WT.mc_id=retailecomm-docs-scseely) que puede usar para administrar las imágenes de contenedor.

En el caso de los servicios de aplicación, puede aprovechar varios servicios de Azure. Por ejemplo, la infraestructura existente puede controlar un pedido de cliente mediante la colocación de mensajes en una cola como [RabbitMQ](https://www.rabbitmq.com/). (Por ejemplo, un mensaje es cargar el cliente, un segundo es enviar el pedido). Al volver a hospedar, se coloca RabbitMQ en una máquina virtual independiente. Durante la refactorización, se agrega una cola o tema de [Service Bus](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions?WT.mc_id=retailecomm-docs-scseely) a la solución, se reescribe el código de RabbitMQ y se dejan de usar las máquinas virtuales que atendieron la funcionalidad de puesta en cola. Con este cambio se reemplaza un conjunto de máquinas virtuales por un servicio de cola de mensajes siempre activo a un costo inferior. Puede encontrar otros servicios de aplicación en Azure Portal.

En el caso de las bases de datos, puede migrar una base de datos desde una máquina virtual a un servicio. Azure admite cargas de trabajo de SQL Server con [Azure SQL Database](/azure/sql-database/sql-database-cloud-migrate?WT.mc_id=retailecomm-docs-scseely) e [Instancia administrada de Azure SQL Database](/azure/sql-database/sql-database-managed-instance?WT.mc_id=retailecomm-docs-scseely). [Data Migration Service](https://azure.microsoft.com/services/database-migration/?WT.mc_id=retailecomm-docs-scseely) evalúa la base de datos, informa sobre el trabajo que se debe realizar antes de la migración y, luego, migra la base de datos desde la máquina virtual al servicio. Azure admite también [MySQL](https://azure.microsoft.com/services/mysql/?WT.mc_id=retailecomm-docs-scseely), [PostgreSQL](https://azure.microsoft.com/services/postgresql/?WT.mc_id=retailecomm-docs-scseely) y otros servicios de motor de [base de datos](https://azure.microsoft.com/services/#databases?WT.mc_id=retailecomm-docs-scseely).

## <a name="rebuild"></a>Volver a generar

Hasta este momento, hemos intentado minimizar los cambios en los sistemas de comercio electrónico y no hemos hablado de los sistemas funcionales. Ahora veamos cómo aprovechar realmente la nube. Esta etapa significa revisar la aplicación existente mediante la adopción de manera agresiva de la arquitectura y los servicios de PaaS o, incluso, SaaS. El proceso abarca revisiones sustanciales para agregar funcionalidad nueva o rediseñar la aplicación para la nube.  Las _API administradas_ son un nuevo concepto que aprovecha los sistemas de nube. Mediante la creación de API para la comunicación entre servicios, podemos facilitar la actualización del sistema.  Una segunda ventaja es la capacidad de obtener información sobre los datos existentes. Para ello, migramos a una arquitectura de _microservicio más API_ y usamos el aprendizaje automático y otras herramientas para analizar los datos.

### <a name="microservices--apis"></a>Microservicios más API

Los microservicios se comunican a través de API orientadas a usuarios externos. Cada servicio es independiente y debe implementar una sola funcionalidad empresarial, por ejemplo, recomendar artículos a los clientes, mantener los carros de la compra, etc. Descomponer una aplicación en microservicios requiere tiempo y planeamiento. Si bien no existe ninguna regla fundamental que defina un microservicio, la idea general implica reducir la unidad implementable a un conjunto de componentes que casi siempre cambian en conjunto. Los microservicios permiten implementar cambios con la frecuencia que sea necesario mientras se disminuye la carga de la prueba de la aplicación en general. Es posible que algunos servicios sean muy pequeños. Para ellos, el modo sin servidor con [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely) funciona bien para escalar horizontalmente a los autores de llamada que sean necesarios a la vez que no se consume ningún recurso cuando no se está en uso. Otros servicios se dividirán según las funcionalidades empresariales: administrar el producto, capturar los pedidos de cliente, etc.

Los mecanismos sin servidor sí presentan inconvenientes: cuando se está bajo una carga ligera, pueden ser lentos en responder porque algún servidor en la nube tarda unos segundos en configurar y ejecutar el código. En el caso de los elementos del entorno que los clientes usan con mucha frecuencia, es posible que quiera asegurarse de que pueden encontrar productos, hacer pedidos, solicitar devoluciones, etc. de manera rápida y sencilla. Cada vez que el rendimiento se ralentiza, se corre el riesgo de perder clientes en el embudo de la compra. Si tiene una funcionalidad que debe responder rápidamente, recompile esa funcionalidad como unidades individualmente implementables en [Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely).  En otros casos, como los servicios que requieren cierta combinación de grandes cantidades de memoria, varias CPU y gran cantidad de almacenamiento local, puede que tenga sentido hospedar el microservicio en su propia máquina virtual.

Cada servicio usa una API para la interacción. El acceso a la API puede ser directo al microservicio, pero para esto se requiere que cualquier usuario que se comunique con el servicio conozca la topología de la aplicación. Un servicio como [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely) ofrece un modo centralizado para publicar las API. Todas las aplicaciones simplemente se conectan al servicio API Management. Los desarrolladores pueden detectar cuáles son las API que se encuentran disponibles. El servicio API Management también proporciona funcionalidades que permitirán que el entorno minorista funcione correctamente. El servicio puede limitar el acceso a la API por distintas partes de la aplicación (para evitar cuellos de botella), almacenar en caché las respuestas para disminuir los valores de cambios, convertir de JSON a XML, etc. [Aquí](/azure/api-management/api-management-policies?WT.mc_id=retailecomm-docs-scseely) puede encontrar una lista completa de las directivas.

### <a name="make-use-of-your-data-and-the-azure-marketplace"></a>Uso de datos y Azure Marketplace

Dado que tiene todos los datos y sistemas en Azure, puede incorporar de manera sencilla otras soluciones de SaaS al negocio. Puede hacer algunas acciones de inmediato. Por ejemplo, use [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely) para unir varios orígenes de datos con el fin de crear visualizaciones e informes y obtener información detallada.

Luego, eche un vistazo a las ofertas que existen en [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?WT.mc_id=retailecomm-docs-scseely) que pueden ayudarlo a llevar a cabo acciones como optimizar el inventario, administrar campañas en función de los atributos del cliente y presentar los artículos adecuados a cada cliente según sus preferencias e historial. Tendrá que dedicar algo de tiempo a configurar los datos para que funcionen en las ofertas de Marketplace.

## <a name="next-steps"></a>Pasos siguientes

Muchos equipos de desarrollo se ven tentados a realizar al mismo tiempo las etapas de rehospedaje y refactorización para abordar la deuda técnica y aprovechar mejor la capacidad. Hay ventajas si se realiza el rehospedaje antes de pasar a los pasos siguientes.  Cualquier problema en la implementación en el entorno nuevo será más fácil de diagnosticar y corregir. A su vez, esto brinda a los equipos de desarrollo y soporte técnico el tiempo necesario para aprender a usar Azure como el entorno nuevo. Cuando empieza a refactorizar y recompilar el sistema, se basa en una aplicación funcional y estable. Esto permite realizar cambios más pequeños y dirigidos y actualizaciones más frecuentes.

Hemos publicado un documento más general sobre la migración a la nube: [Cloud Migration Essentials](https://azure.microsoft.com/resources/cloud-migration-essentials-e-book/?_lrsc=9618a836-9f81-4087-901f-51058783c3a8&WT.mc_id=retailecomm-docs-scseely) (Aspectos fundamentales sobre la migración a la nube). Es un artículo excelente para leer mientras planea la migración.

## <a name="technologies-presented"></a>Tecnologías presentadas

Usadas durante la etapa de rehospedaje:

- [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely) es un consultor en la nube personalizado que le ayudará a seguir los procedimientos recomendados para optimizar las implementaciones de Azure.
- El servicio [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely) evalúa las cargas de trabajo locales para su migración a Azure.
- [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely) orquesta y administra la recuperación ante desastres de las máquinas virtuales de Azure y de las máquinas virtuales y servidores físicos locales.
- [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely) permite muchos tipos de recursos de Azure, como máquinas virtuales (VM) de Azure, para comunicarse de forma segura entre ellos, con Internet y con las redes locales.
- [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely) permite ampliar las redes locales en la nube de Microsoft a través de una conexión privada que facilita un proveedor de conectividad.

Usadas durante la etapa de refactorización:

- [Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely) permite administrar el entorno hospedado de Kubernetes, lo que hace que sea fácil y rápido implementar y administrar aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores.
- [Azure SQL Database](/azure/sql-database/sql-database-technical-overview?WT.mc_id=retailecomm-docs-scseely) es un servicio administrado de base de datos relacional de uso general de Microsoft, que permite estructuras como datos relacionales, JSON, espacial y XML. SQL Database ofrece bases de datos SQL individuales administradas, bases de datos SQL administradas en un grupo elástico e Instancias administradas de SQL.

Usadas durante la etapa de recompilación:

- Azure [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely) ayuda a las organizaciones a publicar API para desarrolladores externos, asociados e internos para liberar el potencial de sus datos y servicios.
- [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely) es una solución para ejecutar fácilmente pequeños fragmentos de código, o &quot;funciones&quot;, en la nube.
- [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely) es un conjunto de herramientas de análisis empresarial que proporciona información detallada acerca de toda la organización.

## <a name="conclusion"></a>Conclusión

Migrar un sistema de comercio electrónico a Azure requiere análisis, planeamiento y un enfoque definido. Analizamos un enfoque de tres fases: rehospedaje, refactorización y recompilación. Este permite que una organización pase de un estado funcional a otro, mientras se minimiza la cantidad de cambios en cada paso. Los comerciantes minoristas también pueden elegir refactorizar o incluso recompilar componentes, omitiendo por completo la fase de rehospedaje. En muchas ocasiones, tendrá una ruta clara hacia la modernización: aprovéchela cuando pueda. A medida que gana experiencia en la ejecución de Azure, verá más oportunidades de agregar funcionalidades nuevas, disminuir costos y mejorar el sistema general.


_Artículo escrito por Scott Seely y Mariya Zorotovich._