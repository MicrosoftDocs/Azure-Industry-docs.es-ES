---
title: Optimización del motor de recomendaciones
author: kbaroni
ms.author: kbaroni
ms.date: 07/18/2018
ms.topic: article
ms.service: industry
description: Reutilización y optimización de aplicaciones de recomendaciones escritas con el lenguaje R. Basa Machine Learning Server en una máquina virtual de Azure.
ms.openlocfilehash: bce6d7df548e7bd80d73495dc0bec642817d8641
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654262"
---
# <a name="optimize-and-reuse-an-existing-recommendation-system"></a>Optimización y reutilización de un sistema de recomendaciones existente  

En este documento se describe el proceso de reutilizar y mejorar correctamente un sistema de recomendaciones existente escrito en R. La clave de esto era el paralelismo de las bibliotecas MicrosoftML y RevoScaleR integradas en Microsoft Machine Learning Server.

## <a name="recommendation-systems-and-r"></a>Sistemas de recomendaciones y R

Para un comerciante minorista, entender las preferencias del consumidor y el historial de compras es una ventaja competitiva. Los minoristas han usado dichos datos durante años, en combinación con el aprendizaje automático, para identificar los productos pertinentes para el consumidor y brindar una experiencia de compra personalizada. El enfoque se denomina **recomendación de productos** y genera un flujo de ingresos importante para los minoristas. Los sistemas de recomendaciones ayudan a responder preguntas tales como: *¿Qué película será la próxima que vea esta persona? ¿En qué servicios adicionales es probable que esté interesado este cliente? ¿Adónde le gustaría ir a este cliente en vacaciones?*
Recientemente, un cliente quería saber lo siguiente: *¿Los consumidores (suscriptores) renovarán sus contratos?* El cliente tenía un modelo de recomendación existente que podría prever la probabilidad de que un suscriptor renovara un contrato. Una vez que se generaba la previsión, se aplicaba un procesamiento adicional para clasificar una respuesta en sí, no o quizás. Luego, la respuesta del modelo se integraba en un proceso de negocio de un centro de llamadas. Ese proceso habilitada un agente de servicio para entregar una recomendación personalizada al consumidor.  
Muchos de los primeros productos de análisis de este cliente se crearon en el [lenguaje de programación R](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server), incluido el modelo de Machine Learning en el centro del sistema de recomendaciones. A medida que ha crecido su base de suscriptores, también lo han hecho los requisitos de datos y proceso. Tanto es así que la carga de trabajo de recomendaciones ahora resulta tremendamente lenta y engorrosa de procesar. Ahora, Python forma cada vez más parte de su estrategia de productos de análisis. Pero, en el corto plazo, necesitan conservar su inversión en R y encontrar un proceso de desarrollo e implementación más eficaz. El desafío era optimizar el enfoque existente mediante las funcionalidades de Azure. Nos embarcamos entonces en una tarea para brindar y validar una pila de tecnología de prueba de concepto para la carga de trabajo de recomendaciones. Este documento resumen un enfoque general que se puede usar para proyectos similares.  

## <a name="design-goals"></a>Diseño de objetivos

Una prioridad clave era rediseñar y automatizar el flujo de trabajo del modelo para brindar varias ventajas:

- Ciclos de desarrollo del modelo más rápidos y un tiempo de innovación más rápido. Cuando los ciclos de preparación de datos y de desarrollo del modelo son eficaces, el volumen de experimentos puede aumentar.  
- Resultados del modelo más precisos y mejores recomendaciones a través de ciclos de reentrenamiento más frecuentes en los datos actualizados.
- La implementación de una prueba de concepto (POC) más escalable, que pudiera funcionar en un entorno de producción completo.
- Tiempos de producción más rápidos mediante la automatización de los procesos entre el desarrollo, las pruebas y la implementación. La automatización también disminuye los errores operativos y las demoras.  

## <a name="requirements"></a>Requisitos

El rediseño del flujo de trabajo tenía estos requisitos:

- Aprovechar los conocimientos y las aptitudes existentes sobre R del equipo.
- Reutilizar el código cuando tuviera sentido.
- Almacenar los datos de suscriptores en una base de datos para integrarla de manera sencilla y rápida en el proceso de entrenamiento y reentrenamiento del modelo.
- Invocar el reentrenamiento del modelo y la puntuación a través de una interfaz de aplicación web.
- Usar Azure Active Directory para la autenticación en el front-end web.

## <a name="technology-stack"></a>Pila de tecnología

La canalización de este proyecto requirió varias tecnologías y herramientas y se centró en cuatro áreas:

1. Persistencia y accesibilidad de los datos de suscriptores.
2. Desarrollo, entrenamiento y selección de los modelos.  
3. Implementación de modelo y operacionalización.
4. Uso de una aplicación web para invocar la puntuación y el reentrenamiento de modelo.

Los componentes de tecnología del diagrama de la canalización se analizan más detalladamente a continuación.

![Arquitectura de optimización](assets/recommendation-engine-optimization/recommendation-architecture.png)

### <a name="microsoft-machine-learning-server"></a>Servidor de Microsoft Machine Learning

El motivo principal para seleccionar cargas de trabajo de R: **[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)** y **Microsoft ML**. Las funciones incluidas con estos paquetes se usaban ampliamente en todo el código para importar los datos, crear los modelos de clasificación e implementarlos en producción.
MLS se implementó en dos máquinas virtuales Linux en Azure: una configurada para "desarrollo" y una para "operaciones". La máquina virtual de desarrollo se aprovisionaba con mucha más memoria y capacidad de procesamiento para facilitar el entrenamiento y la prueba de cientos de modelos. También hospedaba RStudio Server para brindar acceso sencillo a un IDE de RStudio para usuarios remotos. El servidor de operaciones estaba configurado en una máquina virtual más pequeña con las extensiones adicionales necesarias para hospedar modelos de R que se podían llamar desde una aplicación web a través de API REST.

### <a name="rstudio-server"></a>RStudio Server

**[RStudio Server](https://www.rstudio.com/products/rstudio/#Server)** es una aplicación Linux que brinda una interfaz basada en explorador para clientes remotos o portátiles. Se ejecuta en el puerto 8787 y está disponible para las conexiones remotas una vez que se crea una regla de seguridad de red en la máquina virtual de Azure. Para los analistas y los científicos de datos que prefieren el IDE de RStudio, puede ser una opción eficaz para brindar acceso a una máquina virtual con una gran capacidad de memoria y proceso. Está disponible para descarga tanto en su edición de origen como comercial.

### <a name="azure-sql-database"></a>Azure SQL Database

Originalmente, los datos de suscriptor se almacenaban en un archivo .csv muy grande con 6 millones de filas de información sobre compras y preferencias de 500 suscriptores únicos. Almacenar los datos en una base de datos significaba un acceso más rápido a los datos desde R y permitía filtrar las lecturas. Ya no sería necesario importar todo el conjunto de datos para entrenamiento o reentrenamiento: los datos se filtrarían por suscriptor en el origen de la base de datos, lo que reduciría considerablemente los recursos necesarios para importar y procesar los datos.  
Hay varias opciones de bases de datos en la nube administradas en Azure. Se seleccionó [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) debido a la familiaridad del cliente con SQL Server y, lo que es más importante, a planes a futuros para introducir SQL Server Machine Learning Services en una escala más amplia en Azure SQL Database. [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017) son las funcionalidades en la base de datos para ejecutar cargas de trabajo de R y Python a través de procedimientos almacenados.

### <a name="nodejs-and-reactjs"></a>Node.js y React.js

Se creó una interfaz web para invocar scripts de R y proteger el sitio web. Node.js se seleccionó como el marco de servidor web porque habilita un entorno en tiempo de ejecución ligero y eficaz para aplicaciones web dentro de un entorno distribuido. React se usa para crear interfaces de usuario, se ubica en el front-end y llamada a los servicios web hospedados en el servidor web. Se creía que la combinación de Node y React proporcionaría la ruta más rápida para crear prototipos de servicios web para la canalización del modelo.  

## <a name="infrastructure-implementation"></a>Implementación de la infraestructura

En las secciones siguientes se describe cómo se implementó la infraestructura de servidor para este proyecto. Obtener la infraestructura adecuada de desarrollo e implementación puede ser tan fundamental como determinar el enfoque de creación de modelos y las técnicas que se aplicarán.

### <a name="initial-database-load"></a>Carga de base de datos inicial

El primer paso fue importar los datos de suscriptores desde un archivo .csv de gran tamaño a Azure SQL Database. Existen varias opciones para importar datos a Azure SQL Database descritas en esta [referencia](https://blogs.msdn.microsoft.com/sqlcat/2010/07/30/loading-data-to-sql-azure-the-fast-way/). Le mostramos cómo lo hicimos:

1. Creamos la base de datos en Azure Portal con los pasos descritos [aquí](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
2. Descargamos y usamos [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) para conectar a la base de datos desde la máquina virtual.
3. Seleccionamos [Asistente para importación y exportación de SQL](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard?view=sql-server-2017) (si tiene poco tiempo, hay opciones de importación de datos con más funcionalidades). Tenga en cuenta que el Asistente para importación y exportación asigna tipos de datos desde el origen de datos al destino y, con nuestro escenario, todos los elementos de datos se asignaron a un tipo de datos varchar(max) que era aceptable. Si su escenario requiere asignaciones distintas, puede modificar los tipos de datos en el asistente ([referencia](https://docs.microsoft.com/sql/integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard?view=sql-server-2017)).  
4. Debido a que la mayoría de las consultas enviadas a la base de datos se filtrarían según el campo *subscriber_id*, se creó un índice en ese campo.

### <a name="web-application"></a>Aplicación web

La aplicación web es responsable de tres funciones:

- Autenticación: los usuarios web se autentican en *Azure Active Directory* a través del front-end de *React*.
- Puntuación de modelo: acepta datos de entrada desde el usuario para un suscriptor específico, envía los datos del suscriptor al servicio web mediante la API REST, lo que devuelve una respuesta de predicción.  
- Reentrenamiento de modelo: acepta un identificador de suscriptor como entrada e invoca un script de R en el servidor de desarrollo para volver a entrenar el modelo para ese suscriptor.

Implementar el inicio de sesión único (SSO) con *Azure Active Directory* resultó ser un desafío más grande de lo esperado. Esto se debió al marco de aplicación de página única (SPA). Una biblioteca de Azure Active Directory específica fue clave para el éxito: [react-adal](https://github.com/salvoravida/react-adal). Las referencias siguientes brindaron una guía útil para implementar la autenticación:

- [Escenarios de autenticación para Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)
- [Aplicación de página única](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios#single-page-application-spa)

### <a name="development-vm-mls-930"></a>Máquina virtual de desarrollo (MLS 9.3.0)

La máquina virtual de desarrollo hospeda el desarrollo, el entrenamiento y el reentrenamiento del modelo, además de la implementación del modelo de clasificación. Una máquina virtual de Azure (DS13 V2) se aprovisionó con Linux/Ubuntu 16.10 y se instaló lo siguiente en la máquina virtual base:

- Las instrucciones de uso de Machine Learning Server 9.3.0 están disponibles [aquí](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-install). Asegúrese de llevar a cabo los pasos de comprobación de la configuración para confirmar la instalación. Como se trataba de la máquina virtual de desarrollo, se pasó por alto la sección "*Enable web service deployment and remote connections*" (Habilitación de la implementación del servicio web y conexiones remotas).
- [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/) (versión de código abierto). Tenga cuidado de no volver a instalar R/r-base (se instaló anteriormente con MLS 9.3.0).  
- [Agregue un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal) a la máquina virtual para permitir conexiones entrantes a través del puerto 8787 para RStudio Server.  
- Controladores ODBC para controlar la comunicación entre la máquina virtual de desarrollo y Azure SQL Database. Los siguientes controladores ODBC estaban instalados en la máquina virtual:  
- El [controlador ODBC para SQL Server 17](https://www.microsoft.com/download/details.aspx?id=56567) compatible con Linux/Ubuntu 16.10  
- Un unixODBC de controlador ODBC de código abierto con las instrucciones de instalación que aparecen en el artículo sobre la [importación de datos relacionales mediante ODBC](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-odbc). Nota: En este artículo hay dos errores ortográficos en las instrucciones de Ubuntu.  
- Para comprobar si uniODBC está instalado:       ````Apt list –installed | grep unixODBC (should be unixodbc)````
      - Y para instalar el controlador: ````sudo apt-get install unixodbc-devel unixodbc-bin unixodbc (should be unixodbc-dev)````

### <a name="operations-vm-mls-930"></a>Máquina virtual de operaciones (MLS 9.3.0)

La máquina virtual de operaciones hospedaba los puntos de conexión y los servicios web del modelo, almacenaba los archivos de Swagger y las versiones serializadas de los modelos de clasificación. La configuración es muy similar al servidor de desarrollo de MLS. Sin embargo, se configura para la operacionalización, lo que significa que se instalan los servicios web necesarios para atender los puntos de conexión REST. Para implementar la máquina virtual de operaciones, hay plantillas de ARM que hacen que la implementación se realice de manera rápida. Consulte: [Configuring Microsoft Machine Learning Server 9.3 to Operationalize Analytics using ARM Templates](https://blogs.msdn.microsoft.com/mlserver/2018/02/27/configuring-microsoft-machine-learning-server-9-3-to-operationalize-analytics-using-arm-templates/) (Configuración de Microsoft Machine Learning Server 9.3 para operacionalizar el análisis mediante plantillas de ARM). Para nuestro proyecto, se implementó una configuración *One-Box* mediante esta [plantilla de ARM](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates/one-box-configuration/linux).  
Con esto, los componentes de servidor para admitir la canalización del modelo estaban listos para empezar.

## <a name="model-implementation"></a>Implementación del modelo

Una decisión clave que influyó en el diseño del modelo final para el proyecto en cuestión fue migrar a un diseño de "varios modelos" en lugar de un modelo monolítico único. La diferencia: cada suscriptor tiene su propio modelo de clasificación en lugar de un modelo de clasificación grande que atienda a todos los suscriptores. Para este cliente, se prefirió este enfoque porque un modelo más pequeño tenía una superficie de memoria menor y era más sencillo de escalar horizontalmente en el entorno de producción.

### <a name="data-import"></a>Importación de datos

Todos los datos necesarios para la implementación del modelo residían en una instancia de *Azure SQL Database*. Para el entrenamiento y el reentrenamiento del modelo, la importación de datos se realizó en dos etapas:

1. Se envió una consulta a la base de datos para recuperar datos para un *subscriber_id* específico y se devolvió un conjunto de resultados. Se tomaron en cuenta dos opciones para el acceso de la consulta a la base de datos:

- Una función de RevoScaleR denominada [RxSQLServerData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata)
- El paquete ODBC de R

Se decidió usar la biblioteca "odbc" de R, lo que permitió el filtrado de datos en el nivel de base de datos. Filtrar la tabla de base de datos solo para ver las filas necesarias para un modelo de suscriptor específico minimizó la cantidad de filas a leer en R y a procesar. Eso redujo la memoria, el proceso y el tiempo global necesario para entrenar o volver a entrenar cada modelo.  

1. El conjunto de resultados se convirtió en una trama de datos de R y algunos de los tipos de datos se convirtieron específicamente de varchars a valores enteros o numéricos, según lo requerido por los algoritmos de clasificación. Para esta funcionalidad, se usó la función [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) de RevoScaleR. La función *rxImport* se incluye con RevoScaleR y MicrosoftML y está diseñada para ser multiproceso. Este es un ejemplo de cómo la usamos:

````r

# Populate the data frame and modify column types as needed
input_data <- rxImport(sqlServerDataDS, colClasses = c(  
Approved="integer",
      OnTimeArrivalRate="numeric",
      Amount="numeric",
      IsInformed="numeric",
      <continue with list of columns> )
# View the characteristics of the variables in the data source
rxGetInfo (input_data, getVarInfo = TRUE)
````

## <a name="unbalanced-data"></a>Datos no equilibrados

Como el objetivo del modelo de recomendación era predecir la probabilidad de que un cliente renovara un contrato y clasificar la probabilidad según "sí", "no" o "quizás", se usó un algoritmo de clasificación. Un problema que puede afectar seriamente la precisión y el rendimiento de un algoritmo de clasificación es un conjunto de datos no equilibrado.  
Un conjunto de datos no es equilibrado si hay muchos más ejemplos de una "clase" que de otra "clase". En este caso, el número de filas disponibles de cada suscriptor no estaba equilibrado: en el extremo superior, un suscriptor tenía más de un millón de filas; en el límite inferior, 330 clientes tenían menos de 100 filas de datos. En el gráfico siguiente se muestra el desequilibrio entre el número de filas (ejemplos) por suscriptor: ![imbalance-chart](assets/recommendation-engine-optimization/recommendaton_engine_chart.png)

Una técnica para manejar un conjunto de datos no equilibrado es cambiar el conjunto de datos y agregar ejemplos a la clase subrepresentada o quitar ejemplos de la clase sobrerrepresentada. Otra técnicas consiste en generar de manera artificial datos adicionales mediante el uso del conocimiento exacto que el propietario de los datos tiene de los datos y sus atributos. El cliente estableció un umbral para el tamaño de ejemplo mínimo para un suscriptor. En el caso de los suscriptores que estaban por debajo de dicho umbral, era necesario manejar los datos. En este proyecto, se exploraron ambos enfoques.

## <a name="algorithm-selection"></a>Selección del algoritmo

Se evaluaron tres implementaciones de algoritmo de clasificación distintos: *rxDForest*, *rxFastTrees* y *rxFastForest*. Los tres algoritmos aprovechan los subprocesos múltiples y el paralelismo. Microsoft ML usará varias CPU o GPU, si hay disponibles. Los criterios para evaluar los modelos incluidos:

- ¿Los modelos nuevos eran más precisos que el modelo original?
- ¿La superficie de memoria de los modelos se ejecutó en el entorno de producción? ¿El entorno operativo podía admitir la ejecución simultánea de muchos modelos con una respuesta de predicción casi en tiempo real, sin poner en peligro la precisión?
- ¿Qué tan bien manejó el algoritmo el conjunto de datos no equilibrado? ¿Sería necesario manejar previamente el desequilibrio mediante la generación de datos artificiales?

En la tabla siguiente se resumen los resultados:

| Algoritmo | DESCRIPCIÓN | Resultados | Notas |
| :--------- | :------------ | :--------- | :--------------- |
| [rxFastTrees](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees) | Implementación en paralelo del árbol de decisión incrementado que implementa versiones multiproceso de FastRank. | Rendimiento preciso y más rápido. | Sin características especiales para los datos no equilibrados. Los datos tratados previamente se deben proporcionar como entrada. |
| [rxFastForest](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastforest) | Implementación en paralelo de bosque aleatorio y usa rxFastTrees para crear un sistema aprendiz de conjunto de árboles de decisión. | Mayor precisión que el original con datos tratados previamente. Menor uso intensivo de memoria, más rápido que rxDForest. |Sin características especiales para los datos no equilibrados. Datos tratados previamente proporcionados como entrada. |
| [rxDForest](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdforest) | Implementación en paralelo de bosque aleatorio. Se incluye en RevoScaleR. Puede tratar con datos no equilibrados (quita datos omitidos, condiciona los datos, controla la estratificación de los ejemplos), todo dentro de la llamada de función. | Más rápido que el modelo original. Mejor o igual precisión que el modelo original. Controla el conjunto de datos no equilibrado mediante una variedad de técnicas para volver a tomar ejemplos y sintetizar. La superficie de memoria más grande.  | La superficie de memoria más grande porque incluye los datos condicionados dentro de la función.   El tratamiento de los datos era bueno, pero no tanto como las transformaciones personalizadas proporcionadas por el propietario de los datos. |

Al final, el cliente seleccionó el algoritmo *rxFastForest* y decidió tratar los datos no equilibrados mediante el uso de la biblioteca [vtreat](https://cran.r-project.org/web/packages/vtreat/index.html) y la incorporación de un paso de procesamiento previo de los datos personalizados para generar de manera artificial datos para los suscriptores subrepresentados.

## <a name="model-deployment--web-services"></a>Implementación de modelo y servicios web

La publicación de un modelo para implementación en la máquina virtual de operaciones es un proceso sencillo y se encuentra en esta documentación de inicio rápido: [Deploy an R Model as a web service with mrsdeploy](https://docs.microsoft.com/machine-learning-server/operationalize/quickstart-publish-r-web-service) (Implementación de un modelo de R como servicio web con mrsdeploy).
En nuestro escenario, una vez que se crearon los modelos en la máquina virtual de desarrollo, se publicaron en la máquina virtual de operaciones con estos pasos:

1. Establezca un inicio de sesión remoto desde la máquina virtual de desarrollo a la máquina virtual de operaciones con una de las dos funciones disponibles en el paquete mrsdeploy para la autenticación: remoteLogin() con un nombre de administrador local y una contraseña, o bien remoteLoginAAD() con Azure Active Directory. Ambas opciones se describen en esta referencia, Log in to Machine Learning Server or R Server with mrsdeploy and open a remote session (Inicio de sesión en Machine Learning Server o R Server con mrsdeploy y apertura de una sesión remota).  
2. Una vez que se entrena un modelo, publíquelo en la máquina virtual de operaciones con las funciones publishService o updateService en el paquete mrsdeploy. Para este proyecto, usamos varios enfoques de implementación y, dependiendo del enfoque, se publicó un modelo nuevo o se actualizó un modelo existente. El código siguiente se implementó para controlar ambos casos:

````r
# If the service does not exist, publish it and if it does exist, update it.  
# No service by this name so publish one
 api <- publishService(serviceName, code =sc_predict, model = model,  
      inputs = list(prop_data="data.frame"),  
      outputs = list(answer = "numeric"), v = "v1.0.0" )
 print("=========== Model Created =============")
} else {
# A service by this name already exists, update it  
 api <- updateService(serviceName, model = model,
     inputs = list(prop_data="data.frame"), v = "v1.0.0" )
 print("=========== Model Updated =============")
}
 ````

Al implementarlos, los modelos se serializan y almacenan en el servidor de operaciones y se pueden consumir a través de servicios web en modo *estándar* o *en tiempo real*. Cada vez que se llama a un servicio web con el modo estándar, con cada llamada se carga y descarga R y cualquier biblioteca necesaria. Por el contrario, con el modo *en tiempo real*, R y las bibliotecas se cargan solo una vez y se vuelven a usar para llamadas subsiguientes de servicio web. Dado que la mayor parte de la sobrecarga con una llamada de servicio web es la carga de R y las bibliotecas, el modo en tiempo real ofrece una latencia mucho menor para la puntuación del modelo y los tiempos de respuesta pueden ser menores a los 10 ms. Consulte [aquí](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) ejemplos de referencia y documentación sobre las opciones estándar y en tiempo real. El modo en tiempo real se presta bien para predicciones únicas, pero también puede pasar una trama de datos de entrada para la puntuación. Eso se describe en esta referencia: [Asynchronous web service consumption via batch processing with mrsdeploy](https://docs.microsoft.com/machine-learning-server/operationalize/how-to-consume-web-service-asynchronously-batch) (Consumo de servicio web asincrónico a través del procesamiento por lotes con mrsdeploy).

## <a name="conclusion"></a>Conclusión

Aprovechar el paralelismo de las bibliotecas MicrosoftML y RevoScaleR integrado en Microsoft Machine Learning Server aceleró el rendimiento, la implementación y la puntuación de modelos de clasificación individuales para cientos de suscriptores. La precisión del modelo mejoró y los tiempos de entrenamiento y reentrenamiento se comprimieron, todo con un mínimo de cambio en la base de código R existente.
Implementar la infraestructura para admitir una canalización de modelo y configurar correctamente los componentes de tecnología de un extremo a otro puede ser complejo. Estas son algunas referencias para empezar con su propio enfoque:

- [Machine Learning Server Documentation](https://docs.microsoft.com/machine-learning-server/) (Documentación sobre Machine Learning Server)
- [R Tutorials for Machine Learning Server](https://docs.microsoft.com/advanced-analytics/tutorials/sql-server-r-tutorials?view=sql-server-2017) (Tutoriales de R para Machine Learning Server)
- [R Samples for Machine Learning Server](https://docs.microsoft.com/machine-learning-server/r/r-samples) (Ejemplos de R para Machine Learning Server)
- [R Function Library Reference](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) (Referencia de la biblioteca de funciones de R)

## <a name="references"></a>Referencias

Si está interesado en compilar otras soluciones predictivas para su negocio minorista, visite la [sección sobre comercio minorista](https://gallery.azure.ai/industries/retail) de Azure [AI Gallery](https://gallery.azure.ai/).  