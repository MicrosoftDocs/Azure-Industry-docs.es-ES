---
title: Introducción al mantenimiento predictivo en la fabricación
author: scseely
ms.author: scseely
ms.date: 05/02/2018
ms.topic: article
ms.service: industry
description: Información general sobre cómo desarrollar el mantenimiento predictivo para los clientes de fabricación en Azure.
ms.openlocfilehash: c5f3737c29c1d16f5d343a1fd56ed924684a3a1d
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654162"
---
# <a name="predictive-maintenance-in-manufacturing-overview"></a>Introducción al mantenimiento predictivo en la fabricación

## <a name="introduction"></a>Introducción

El mantenimiento predictivo anticipa las necesidades de mantenimiento para evitar los costos asociados con el tiempo de inactividad no programado. Al conectarse a los dispositivos y supervisar los datos que estos producen, se pueden identificar patrones que conducen a posibles problemas o errores. A continuación, puede utilizar esta información para solucionar los problemas antes de que ocurran. Esta capacidad de predecir cuándo necesitan mantenimiento los equipos o recursos le permite optimizar la duración de los equipos y minimizar el tiempo de inactividad.

El mantenimiento predictivo extrae información de los datos producidos por el equipo en la zona de producción y actúa en función de esa información. La idea del mantenimiento predictivo se remonta a principios de la década de 1990 y aumenta el mantenimiento predictivo programado regularmente. Desde el principio, la falta de disponibilidad de sensores que generaran datos, así como la falta de recursos de proceso para recopilar y analizar los datos, dificultaban la implementación del mantenimiento predictivo. Hoy en día, los avances en Internet de las cosas (IoT), la informática en la nube, el análisis de datos y el aprendizaje automático están permitiendo que el mantenimiento predictivo se generalice.

El mantenimiento predictivo requiere que el equipo proporcione datos de los sensores que supervisan el equipo, así como otros datos operativos. El sistema de mantenimiento predictivo analiza los datos y almacena los resultados. Los humanos actúan según el análisis.

Después de presentar algunos antecedentes en este artículo, se describe cómo implementar las distintas partes de una solución de mantenimiento predictivo mediante una combinación de datos locales, el aprendizaje automático de Azure y el uso de los modelos de aprendizaje automático. El mantenimiento predictivo depende en gran medida de los datos para tomar decisiones, por lo que empezamos por considerar la recopilación de datos. Los datos se deben recopilar y luego utilizarse para evaluar lo que está sucediendo ahora, así como para crear mejores modelos predictivos en el futuro. Finalmente, explicamos cómo es una solución de análisis, incluida la visualización de los resultados del análisis en una herramienta de informes como [Power BI](https://docs.microsoft.com/en-us/power-bi/).

## <a name="maintenance-strategies"></a>Estrategias de mantenimiento

A lo largo de la historia de la industria de fabricación, surgieron varias estrategias de mantenimiento. El mantenimiento reactivo corrige problemas después de que sucedan. El mantenimiento preventivo soluciona los problemas antes de que ocurran siguiendo un programa de mantenimiento basado en la experiencia de error anterior. El mantenimiento predictivo también corrige los problemas antes de que ocurran, pero tiene en cuenta la utilización real del equipo en lugar de trabajar a partir de una programación fija. De los tres, el mantenimiento predictivo fue el más difícil de lograr debido a las limitaciones históricas en la recolección, procesamiento y visualización de datos. Veamos cada opción con un poco más detalle.

El mantenimiento reactivo encarna la mentalidad de &quot;si algo no está roto, no hay que arreglarlo&quot;. Dé servicio al recurso solo cuando se produzca un error. Por ejemplo, el motor del centro de mecanizado CNC de 5 ejes solo recibe mantenimiento cuando deja de funcionar. El mantenimiento reactivo maximiza la duración del componente que finalmente produce un error. El mantenimiento reactivo también introduce cantidades desconocidas de tiempo de inactividad, daños colaterales inesperados a los componentes dañados por el componente defectuoso y otros problemas.

El mantenimiento preventivo requiere que el usuario realice el mantenimiento del recurso a intervalos predeterminados. El intervalo se basa normalmente en el historial de frecuencia de errores experimentados para el recurso. Estos intervalos se basan en el rendimiento histórico, las simulaciones o el modelado estadístico, entre otros. La ventaja de esta estrategia es que aumenta el tiempo de actividad, da lugar a menos errores y permite planificar el mantenimiento. La desventaja en muchos casos es que es posible que el componente reemplazado en el recurso pueda haber durado más. Esto resulta en un exceso de mantenimiento y desperdicio. Por otro lado, algunas piezas pueden seguir dando un error antes del mantenimiento programado. Probablemente conoce bien el mantenimiento preventivo: después de una serie de horas establecidas de funcionamiento (o de cualquier otra métrica), deje de usar la máquina e inspecciónela. Reemplace todas las piezas que deban ser reemplazadas.

El mantenimiento predictivo supervisa el uso de los recursos mediante el uso de modelos para predecir cuándo es probable que un recurso experimente un error en un componente. Ese componente tiene entonces su mantenimiento programado para &quot;el mantenimiento Just-In-Time&quot;. El mantenimiento predictivo mejora las estrategias anteriores al maximizar tanto el tiempo de actividad como la duración de los recursos. Dado que el mantenimiento de los equipos se realiza cerca de la duración máxima de los componentes, se gasta menos dinero en la sustitución de las piezas en funcionamiento. La desventaja es que la naturaleza just-in-time del mantenimiento predictivo es más difícil de ejecutar ya que requiere una organización de servicios más receptiva y flexible. Volviendo al motor de mecanizado CNC de 5 ejes, se podría programar el mantenimiento &quot;fácilmente&quot;. (es decir, de forma planeada, sin interrumpir la producción) si un modelo predictivo predice que el motor tiene, por ejemplo, un 75 % de probabilidad de error en las próximas 24 horas (basándose en la información procedente de los sensores de la máquina).

 ![](./assets/pdm-assets/maintenancestrategies.png)


## <a name="different-ways-pdm-can-be-offered"></a>Se puede ofrecer el mantenimiento predictivo de diferentes formas

Un fabricante puede utilizar directamente la solución de mantenimiento predictivo, al supervisar los datos procedentes de sus propias operaciones de fabricación. Existen otras formas que significan nuevas oportunidades de negocio y fuentes de ingresos para otras organizaciones. Por ejemplo: 

- Un fabricante agrega valor para sus clientes al ofrecer servicios de mantenimiento predictivo para sus productos.
- Un fabricante ofrece sus productos bajo un modelo de producto como un servicio, donde los clientes &quot;se suscriben&quot; al producto en lugar de comprarlo directamente. Bajo este modelo, el fabricante quiere maximizar el tiempo de actividad del producto; el producto no generará ingresos cuando no esté funcionando.
- Una empresa proporciona productos y servicios de mantenimiento predictivo para productos fabricados por uno o más fabricantes.

## <a name="building-a-predictive-maintenance-solution"></a>Creación de una solución de mantenimiento predictivo

Para crear una solución de mantenimiento predictivo, partimos de datos; idealmente datos que muestren el funcionamiento normal, así como datos que muestren cómo era el equipo antes, durante y después de un error. Los datos proceden de sensores, notas mantenidas por los operadores de los equipos, información de funcionamiento, datos ambientales, especificaciones de la máquina, etc. Los sistemas de registro pueden incluir historiadores, sistemas de ejecución de fabricación o ERP, entre otros. Los datos está disponibles para el análisis en una variedad de formas. [El proceso de ciencia de datos del equipo](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/), que se ilustra a continuación y que está personalizado para la fabricación, hace un excelente trabajo al explicar las diversas preocupaciones que se tienen cuando se crean y ejecutan modelos de aprendizaje automático.

 ![](./assets/pdm-assets/DataScienceDiagram.png)


La primera tarea será identificar los tipos de errores que desea predecir. Con esto en mente, identificará entonces los orígenes de datos que tienen datos interesantes sobre ese tipo de error. La canalización lleva los datos al sistema desde el entorno. Los científicos de datos utilizarán sus herramientas favoritas de aprendizaje automático para preparar los datos. En este punto, están listos para crear y entrenar modelos que puedan identificar diversos tipos de problemas. Los modelos responden a preguntas como:

- _Para el recurso, ¿cuál es la probabilidad de que ocurra un error en las próximas X horas?_ Respuesta: 0-100 %
- _¿Cuál es la vida útil restante del recurso?_ Respuesta: X horas
- _¿Se comporta este recurso de manera inusual?_ Respuesta: Sí o No
- _¿Qué recurso requiere un mantenimiento más urgente?_ Respuesta: Recurso X

Una vez desarrollados, los modelos pueden colocarse en el propio equipo para autodiagnóstico, en un dispositivo de borde en algún lugar del entorno de fabricación o en Azure. También continuará enviando los datos de los orígenes principales a un almacén central para que pueda seguir con la creación y mantenimiento de la solución de mantenimiento predictivo.

La potencia de Azure le permite entrenar y probar los modelos en la tecnología de su elección. Puede hacer uso de GPU, FPGA, CPU, grandes máquinas de memoria, entre otros. Una de las grandes cosas de Azure es que la plataforma abarca completamente las herramientas de código abierto que utilizan los científicos de datos (como R y Python). A medida que se completa el análisis, los resultados se pueden mostrar en otras facetas del panel o en otros informes. Estos informes pueden aparecer en herramientas personalizadas o puede aprovechar las herramientas de generación de informes como [Power BI](https://docs.microsoft.com/en-us/power-bi/) o [Time Series Insights](https://docs.microsoft.com/en-us/azure/time-series-insights/).

Cualquiera que sea su necesidad de mantenimiento predictivo, Azure tiene las herramientas, la escala y las funcionalidades que necesita para crear una solución sólida.

## <a name="getting-started"></a>Introducción

Muchos de los equipos que se encuentran en la planta de producción recopilan datos y proporcionan mecanismos para recopilar datos de los dispositivos. Comience a recopilar esos datos lo antes posible. A medida que se producen los errores, pida a los científicos que analicen los datos para crear modelos que puedan detectar futuros errores. A medida que el conocimiento se acumula sobre la detección de errores, pasará a un modo predictivo en el que se corregirán los componentes durante el tiempo de inactividad planeado. La [Predictive Maintenance Modeling Guide](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) (Guía de modelado de mantenimiento predictivo) proporciona un sólido tutorial sobre cómo crear las piezas de aprendizaje automático de la solución.

Para ver una solución de ejemplo, revise la solución, guía y cuaderno de estrategias del [mantenimiento predictivo en la industria aeroespacial](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). Si necesita aumentar el número de modelos de creación, le recomendamos que visite [AI School](https://aischool.microsoft.com/). El curso [Introduction to Machine Learning with Azure ML](https://aischool.microsoft.com/learning-paths/4ZYo4wHJVCsUSAKa2EoAk8) (Introducción al aprendizaje automático con Azure ML) le ayudará a familiarizarse con nuestras herramientas.

## <a name="technologies-presented"></a>Tecnologías presentadas

[Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) almacena de forma sencilla y rentable desde cientos hasta miles de millones de objetos, en niveles de acceso frecuente, acceso esporádico o archivo, según las necesidades.

[Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/) es una base de datos para aplicaciones de latencia extremadamente baja y que se pueden escalar de forma masiva en cualquier parte del mundo, con compatibilidad nativa con NoSQL.

[Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/) incluye todas las funcionalidades necesarias para facilitar a los desarrolladores, científicos de datos y analistas el almacenamiento de datos de cualquier tamaño, forma y velocidad, y realizar todos los tipos de procesamiento y análisis entre plataformas y lenguajes.

[Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/) es un servicio de ingesta de datos de telemetría a hiperescala que recopila, transforma y almacena millones de eventos. Como plataforma de streaming distribuida, ofrece retención de tiempo configurable y baja latencia, lo que permite ingresar grandes cantidades de datos de telemetría en la nube y leer los datos en varias aplicaciones mediante semántica de publicación o suscripción.

[Azure IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/) es un servicio de Internet de las cosas (IoT) que se basa en IoT Hub. Este servicio está pensado para los clientes que desean analizar datos en dispositivos, situación también conocida como &quot;en el perímetro&quot;, en lugar de en la nube. Al mover elementos de la carga de trabajo al perímetro, los dispositivos pueden dedicar menos tiempo a enviar mensajes a la nube y reaccionan más rápidamente a los cambios de estado.

[Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/) es un servicio totalmente administrado que permite establecer una comunicación bidireccional fiable y segura entre millones de dispositivos IoT y un back-end de soluciones.

[Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/) permite que los equipos aprendan a partir de los datos y las experiencias y actúen sin haber sido programados de forma específica. Los clientes pueden crear aplicaciones de inteligencia artificial (IA) que detecten, procesen y actúen de forma inteligente según la información, lo que aumenta la capacidad humana, la velocidad y la eficacia, y ayuda a las organizaciones a llegar más lejos.

[Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus/) es un mecanismo de comunicación asincrónica. Los componentes principales de la infraestructura de mensajería de Service Bus son las colas, los temas y las suscripciones.

[Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/) es el servicio de base de datos relacional inteligente en la nube totalmente administrado que ofrece la mayor compatibilidad con el motor de SQL Server, de modo que puede migrar sus bases de datos de SQL Server sin necesidad de cambiar las aplicaciones.

[Power BI](https://docs.microsoft.com/en-us/power-bi/) es un conjunto de herramientas de análisis empresarial que proporciona información detallada acerca de toda la organización. Conéctese a cientos de orígenes de datos, simplifique la preparación de los datos y realice análisis ad hoc.

[Time Series Insights](https://docs.microsoft.com/en-us/azure/time-series-insights/) es un servicio de análisis, almacenamiento y visualización totalmente administrado que permite administrar los datos de serie temporal a escala de IoT en la nube.

## <a name="conclusion"></a>Conclusión

El mantenimiento predictivo requiere que las máquinas tengan algún nivel de instrumentación y conectividad que permita crear sistemas que puedan predecir los problemas y permitan actuar antes de que ocurra un error. El mantenimiento predictivo aumenta las programaciones de mantenimiento preventivo mediante la identificación de componentes específicos para inspeccionar y reparar o reemplazar. Existen muchos recursos para ayudarle a comenzar. La infraestructura de Microsoft puede ayudarle a crear soluciones que se ejecutan en el dispositivo, en el perímetro y en la nube. 

Para empezar, seleccione de uno a tres errores que le gustaría evitar y comience el proceso de detección con esos elementos. Después, identifique dónde residen los datos que ayudan a identificar los errores. Combine esos datos con las aptitudes que aprenda en el curso [Introduction to Machine Learning with Azure ML](https://aischool.microsoft.com/learning-paths/4ZYo4wHJVCsUSAKa2EoAk8) (Introducción al aprendizaje automático con Azure ML) para crear los modelos de mantenimiento predictivo.