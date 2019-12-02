---
title: Implementación del plano técnico de inteligencia artificial para el sector sanitario
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: En este artículo se ofrece orientación para el plano técnico de Microsoft Azure para inteligencia artificial.
ms.openlocfilehash: 40919ffde2c2cac11339b40348cba7a5e0e0e16d
ms.sourcegitcommit: 2714a77488c413f01beb169a18acab45663bcfd7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308507"
---
# <a name="implementing-the-azure-blueprint-for-ai"></a>Implementación del plano técnico de Azure para inteligencia artificial

## <a name="introduction"></a>Introducción

Las organizaciones sanitarias se están dando cuenta de que la inteligencia artificial y el aprendizaje automático pueden ser herramientas valiosas para muchos aspectos de su sector, desde la mejora de los resultados de los pacientes hasta la simplificación de sus operaciones cotidianas. A menudo, las organizaciones sanitarias carecen de personal tecnológico para implementar sistemas de inteligencia artificial o aprendizaje automático. Para mejorar esta situación y hacer que las soluciones de inteligencia artificial o aprendizaje automático se ejecuten rápidamente en Azure, Microsoft ha creado el [plano técnico de inteligencia artificial para atención sanitaria de Azure](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr). Usando el plan, le mostramos cómo comenzar a usar inteligencia artificial o aprendizaje automático rápidamente de una manera segura, conforme y confiable.

El plano técnico sanitario para inteligencia artificial pone en marcha la inteligencia artificial o el aprendizaje automático en su organización utilizando Azure. En este artículo se describe cómo instalar el plano técnico y sus componentes y cómo usarlo para ejecutar un experimento de inteligencia artificial o aprendizaje automático que predice la duración de la estancia de un paciente.

### <a name="benefits"></a>Ventajas

El plano técnico se creó para brindar orientación a las organizaciones sanitarias y un tutorial rápido sobre las arquitecturas PaaS (plataforma como servicio) adecuadas para admitir inteligencia artificial o aprendizaje automático en entornos sanitarios altamente regulados, lo que incluye garantizar que el sistema cumple con los requisitos de HIPAA y HITRUST.

El departamento de tecnología de las organizaciones sanitarias a menudo tiene poco tiempo para nuevos proyectos, especialmente aquellos en los que deben aprender una tecnología nueva y compleja. El plano técnico puede ayudar a este personal a familiarizarse con Azure y algunos de sus servicios con rapidez, ahorrándose el costo de una curva de aprendizaje. Los encargados de la técnica pueden aprender del plano como implementación de referencia una vez instalado y usar ese conocimiento para ampliar sus capacidades, o bien crear una nueva solución de inteligencia artificial o aprendizaje automático basada en el plano.

Con este plano técnico, su organización estará rápidamente lista para utilizar las nuevas funciones de inteligencia artificial o aprendizaje automático. Una vez implementadas las soluciones de inteligencia artificial y aprendizaje automático, el departamento técnico podrá empezar a ejecutar experimentos con ellas empleando los datos reunidos de diferentes orígenes. Por ejemplo, es posible que ya existan datos sobre casos previos de sepsis y muchas de las variables adjuntas que se rastrearon para pacientes individuales con la condición. Al usar estos datos en un formato anónimo, el personal técnico puede buscar indicadores de sepsis potencial en los pacientes y ayudar a cambiar los procedimientos operativos para evitar la enfermedad con mayor eficacia.

El plano técnico proporciona los datos y el código de ejemplo para aprender a predecir la duración de la estancia del paciente. Se trata de un caso de uso de ejemplo que puede utilizarse para obtener más información sobre los componentes de la solución de inteligencia artificial o aprendizaje automático.

### <a name="platform-or-infrastructure-as-a-service"></a>Plataforma o infraestructura como servicio

Microsoft Azure dispone de ofertas tanto de PaaS como de SaaS, y elegir la que mejor se adapte a sus necesidades varía según el caso de uso. El plano técnico está diseñado para usar los servicios de PaaS que predigan la duración de la estancia de un paciente en el hospital. El plano técnico de inteligencia artificial para atención sanitaria de Azure proporciona todo lo necesario para crear una instancia de una solución de inteligencia artificial o aprendizaje automático segura y conforme preconfigurada para organizaciones de este tipo. El modelo de PaaS utilizado por este plano técnico lo instala y configura como una solución completa.

### <a name="paas-option"></a>Opción de PaaS

El uso de un modelo de servicios de PaaS da como resultado un menor costo total de la propiedad porque no hay hardware que administrar. La organización no necesita adquirir y mantener el hardware ni máquinas virtuales. El plano técnico usa exclusivamente los servicios de PaaS.

De este modo se reduce el costo de mantener una solución local y se libera al personal técnico para que se centre en iniciativas estratégicas en lugar de infraestructura. También puede pasar el pago de servicios de computación y almacenamiento de los presupuestos de inversiones de capital a los presupuestos de inversiones operativas. Los costos de la ejecución de este escenario de plano técnico se derivan del uso de los servicios más los costos de almacenamiento de datos.

### <a name="iaas-option"></a>Opción de IaaS

Si bien el plano técnico y este artículo se centran en la implementación de PaaS, existe una [extensión de código abierto](https://github.com/Azure/Azure-Health-Extension) para el plano técnico que permite usarlo en entornos de infraestructura como servicio (IaaS).

En un modelo de hospedaje de IaaS, los clientes pagan por el tiempo de actividad de las máquinas virtuales hospedadas en Azure y su capacidad de procesamiento. IaaS ofrece un mayor nivel de control, ya que el cliente está administrando sus propias máquinas virtuales, pero generalmente a un mayor costo, ya que las máquinas virtuales pagan por tiempo de actividad y no por uso. Además, el cliente es responsable de mantener las máquinas virtuales aplicando revisiones, protegiendo contra malware, etc.

El modelo de IaaS está fuera del ámbito de este artículo, que se centra en una implementación de PaaS del plano técnico.

## <a name="the-healthcare-aiml-blueprint"></a>Plano técnico de inteligencia artificial o aprendizaje automático para la atención sanitaria

El plano técnico crea un punto de partida para el uso de esta tecnología en un contexto de atención sanitaria. Cuando se instala el plano técnico en Azure, se crean todos los recursos y servicios y varias cuentas de usuario para admitir el escenario de inteligencia artificial o aprendizaje automático con los actores, permisos y servicios adecuados.

El plano técnico incluye un experimento de inteligencia artificial o aprendizaje automático, que puede ayudar a pronosticar el personal, la cantidad de camas y otros aspectos logísticos. El paquete incluye scripts de instalación, código de ejemplo, datos de prueba y soporte de seguridad y privacidad, entre otras cosas.

## <a name="blueprint-technical-resources"></a>Recursos técnicos para el plano técnico

Los siguientes recursos se encuentran en este repositorio de GitHub.

Los recursos principales son:

1. Scripts de [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?WT.mc_id=ms-docs-dastarr) para la implementación, la configuración y otras tareas.
2. [Instrucciones detalladas para la instalación](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md) que incluyen cómo usar el script de instalación.
3. [Una completa sección de preguntas frecuentes](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md).

Las preocupaciones transversales para este modelo incluyen la identidad y la seguridad, que son especialmente importantes cuando se trata de datos de pacientes. En este gráfico se muestran los componentes de la canalización de aprendizaje automático.

![Canalización de aprendizaje automático](assets/sg-healthcare-ai-blueprint-assets/ml-pipeline.png)

El siguiente gráfico muestra los productos de Azure que están instalados. Cada recurso o servicio proporciona un componente de la solución de procesamiento de inteligencia artificial o aprendizaje automático, incluidas las preocupaciones transversales de identidad y seguridad.

![Zonas de componentes](assets/sg-healthcare-ai-blueprint-assets/component-zones.png)

La implementación de un nuevo sistema en un entorno sanitario regulado es complejo. Por ejemplo, asegurarse de que todos los aspectos del sistema cumplan con HIPAA y la certificación HITRUST va más allá del desarrollo de una solución ligera. El plano técnico instala permisos de identificación y recursos para ayudar con estos asuntos complejos.

El plano técnico también proporciona scripts y datos adicionales que se utilizan para simular y estudiar los resultados de la admisión o el alta de pacientes. Estos scripts permiten al personal empezar a aprender de inmediato cómo implementar inteligencia artificial y aprendizaje automático utilizando la solución en un escenario seguro y aislado.

### <a name="additional-blueprint-resources"></a>Recursos adicionales del plano técnico

El plano técnico proporciona una guía e instrucciones excepcionales para el personal técnico, y también incluye artefactos para ayudar a crear una instalación completamente funcional. Entre estos otros artefactos están los siguientes:

1. Un [modelo de amenazas](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) para su uso con la herramienta [Microsoft Threat Modeling Tool](https://www.microsoft.com/download/details.aspx?id=49168&WT.mc_id=ms-docs-dastarr). Este modelo de amenazas muestra los componentes de la solución, los flujos de datos entre ellos y los límites de confianza. La herramienta se puede usar para modelar amenazas por aquellos usuarios que buscan ampliar el plano técnico básico o para aprender sobre la arquitectura del sistema desde una perspectiva de seguridad.

2. La [matriz de responsabilidades del cliente HITRUST](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr), en un libro de Excel.  Esto muestra lo que usted (el cliente) debe proporcionar en comparación con lo que ofrece Microsoft para cada requisito de la matriz. En este artículo se incluye más información sobre esta matriz de responsabilidades, en la sección Seguridad y cumplimiento > Matriz de responsabilidades del plano técnico.

3. El documento [HITRUST health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) (Revisión de inteligencia artificial y datos sanitarios de HITRUST) examina el proyecto desde el prisma de los requisitos que deben cumplirse para la certificación HITRUST.

4. El documento [HIPAA health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) (Revisión de inteligencia artificial y datos sanitarios de HIPAA) revisa la arquitectura con las normas de la HIPAA en mente.

Estos recursos están [aquí en GitHub](https://github.com/Azure/Health-Data-and-AI-Blueprint).

## <a name="installing-the-blueprint"></a>Instalación del plano técnico

Se necesita invertir poco tiempo para empezar a trabajar con esta solución de plano técnico. Se recomienda un poco de conocimiento sobre los scripts de PowerShell, pero hay instrucciones paso a paso disponibles para ayudar en la instalación de modo que los encargados de la tecnología puedan implementar con éxito este plano técnico con independencia de sus habilidades de scripting.

Se prevé que el personal técnico con poca experiencia en el uso de Azure pueda tardar entre 30 y 60 minutos en instalar el plano técnico.

### <a name="the-installation-script"></a>Script de instalación

El plano técnico proporciona unas excepcionales orientaciones e instrucciones de instalación. También ofrece scripts para la instalación y desinstalación de los servicios y recursos del plano técnico. La llamada al script de implementación de PowerShell es sencilla. Antes de instalar el plano técnico, deben recopilarse y utilizarse ciertos datos como argumentos para el script deploy.ps1 como se muestra a continuación.

```powershell
.\deploy.ps1 -deploymentPrefix <prefix> `
            -tenantId <tenant id> ` # also known as the AAD directory
            -tenantDomain <tenant domain> `
            -subscriptionId <subscription id> `
            -globalAdminUsername <user id> ` # ID from your AAD account
            -deploymentPassword <universal password> ` # applied to all new users and service accounts
            -appInsightsPlan 1 # we want app insights set up
```

### <a name="the-installation-environment"></a>Entorno de instalación

**¡Importante!** No instale el plano técnico desde una máquina fuera de Azure. Es mucho más probable que la instalación se complete correctamente si crea una instancia limpia de Windows 10 (u otra máquina virtual de Windows) en Azure y ejecuta los scripts de instalación desde ahí. Esta técnica usa una máquina virtual basada en la nube para reducir la latencia y ayudar a que la instalación finalice de manera fluida.

Durante la instalación, el script llama a otros paquetes para su carga y uso. Cuando se instala desde una máquina virtual en Azure, el retraso entre el equipo de instalación y los recursos de destino será mucho menor. Sin embargo, algunos de los paquetes de scripting descargados aún son vulnerables a la latencia, ya que los paquetes de scripts residen fuera del entorno de Azure, lo que puede provocar errores de tiempo de espera.

### <a name="install-failure-dont-panic"></a>¡Error de instalación! (No se asuste)

El instalador descarga algunos paquetes externos durante la instalación. A veces, una solicitud de recurso de script agotará el tiempo de espera debido al retraso entre la máquina de instalación y el paquete. Cuando esto sucede, tiene dos opciones:

1. Ejecutar el script de instalación de nuevo sin realizar ningún cambio. El instalador comprueba si hay recursos ya asignados e instala solo aquellos necesarios. Aunque esta técnica puede funcionar, existe el riesgo de que el script de instalación intente asignar los recursos ya vigentes. Esto puede producir un error y la instalación no se completará.

2. Ejecute de todos modos el script deploy.ps1, pero pase argumentos distintos para desinstalar los servicios del plano técnico.

```powershell
.\deploy.ps1 -clearDeploymentPrefix <prefix> `
             -tenantId <value> `
             -subscriptionId <value> `
             -tenantDomain <value> `
             -globalAdminUsername <value> `
             -clearDeployment
```

Una vez finalizada la desinstalación, cambie el prefijo del script de instalación e intente instalar de nuevo. El problema de latencia no debería producirse de nuevo. Si se produce un error en la instalación al descargar los paquetes de scripts, ejecute el script de desinstalación y, a continuación, el programa de instalación de nuevo.

Después de ejecutar el script de desinstalación, lo siguiente desaparecerá.

- Usuarios instalados por el script del instalador
- Los grupos de recursos y sus respectivos servicios, incluido el almacenamiento de datos
- La aplicación registrada en AAD (Azure Active Directory)

Tenga en cuenta que Key Vault se mantiene como una "eliminación temporal" y, si bien no se ve en el portal, no se desasigna hasta que transcurran 30 días. Esto permite reconstituir la instancia de Key Vault si fuera necesario. Para obtener más información acerca de las implicaciones derivadas de esto y cómo administrarlas, consulte la sección Problemas técnicos > Key Vault de este artículo.

### <a name="reinstall-after-an-uninstall"></a>Reinstalación después de una desinstalación

Si es necesario volver a instalar el plano técnico después de una desinstalación, debe cambiar el prefijo en la siguiente implementación porque, de no hacerlo, la instancia de Key Vault desinstalada provocará un error. Esta información se detalla en la sección _Problemas técnicos > Key Vault_ de este artículo.

### <a name="required-administrator-roles"></a>Roles de administrador necesarios

La persona que instala el plano técnico debe ostentar el rol de Administrador global en AAD. La cuenta de instalación también debe ser de un administrador de suscripciones de Azure para la suscripción que se está usando. Si la persona que realiza la instalación no ostenta estos dos roles, se producirá un error en la instalación.

![Instalador del plano técnico](assets/sg-healthcare-ai-blueprint-assets/blueprint-installer.png)

Además, la instalación no está diseñada para funcionar con las suscripciones a MSDN debido a la estrecha integración con AAD. Debe usarse una cuenta de Azure estándar. Si es necesario, [obtenga una evaluación gratuita](https://azure.microsoft.com/free/?WT.mc_id=ms-docs-dastarr) con crédito para utilizarlo en la instalación de la solución de plano técnico y la ejecución de sus demostraciones.

## <a name="adding-other-resources"></a>Adición de otros recursos

La instalación del plano técnico de Azure no incluye más servicios de los necesarios para implementar el caso de uso de inteligencia artificial o aprendizaje automático. Sin embargo, pueden agregarse más recursos o servicios al entorno de Azure, convirtiéndolo así en un buen banco de pruebas para otras iniciativas o un punto de partida para un sistema de producción. Por ejemplo, se pueden agregar otros servicios de PaaS o recursos de IaaS en la misma suscripción y AAD.

Tiene la posibilidad de incorporar nuevos recursos a la solución, como [Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=ms-docs-dastarr) o una nueva instancia de [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=ms-docs-dastarr), a medida que se necesiten nuevas funciones de Azure. Al agregar nuevos recursos o servicios, asegúrese de que están configurados para cumplir las directivas de seguridad y privacidad para mantener la conformidad con las normativas y la directiva.

Los nuevos recursos y servicios pueden crearse con las [API REST de Azure](https://docs.microsoft.com/rest/api/?view=Azure&WT.mc_id=ms-docs-dastarr), el [scripting de Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.6.0&WT.mc_id=ms-docs-dastarr) o mediante [Azure Portal](https://portal.azure.com/?WT.mc_id=ms-docs-dastarr).

## <a name="using-machine-learning-with-the-blueprint"></a>Uso del aprendizaje automático con el plano técnico

El plano técnico se ha diseñado para demostrar un escenario de aprendizaje automático con un algoritmo de regresión que se usa en un modelo para predecir la [duración de la estancia de un paciente](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#example-use-case). Esta es una predicción común para los proveedores de atención sanitaria, ya que ayuda a programar el personal y otras decisiones operativas. Además, se pueden detectar anomalías a lo largo del tiempo cuando aumenta o disminuye la duración media de la estancia para una condición determinada.

### <a name="ingesting-training-data"></a>Ingesta de datos de entrenamiento

Una vez que esté instalado el plano técnico y todos los servicios funcionen adecuadamente, se pueden ingerir los datos que se analizarán. Hay 100 000 [registros de pacientes disponibles para la ingesta](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#ingest) y el trabajo con el modelo. La ingesta de los registros de pacientes es el primer paso en el uso de [Azure Machine Learning Studio](/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=ms-docs-dastarr) para ejecutar el experimento de duración de la estancia del paciente como se muestra a continuación.

![Ingesta](assets/sg-healthcare-ai-blueprint-assets/ingest.png)

El plano técnico incluye un experimento y los datos necesarios para ejecutar un trabajo de aprendizaje automático en Machine Learning Studio (MLS). En el ejemplo se utiliza un modelo entrenado en un experimento para predecir la duración de la estancia del paciente en función de muchas variables.

En este entorno de demostración, los datos ingeridos en la base de datos de Azure SQL no contienen defectos ni carecen de elementos de datos. Estos datos están limpios. A menudo se ingieren datos sucios, que deben "limpiarse" para utilizarlos a fin de alimentar un algoritmo de entrenamiento de aprendizaje automático o antes de usar los datos en un trabajo de aprendizaje automático. Si faltan datos o hay valores incorrectos, los resultados del análisis de aprendizaje automático resultarán afectados negativamente.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

Muchas organizaciones de atención sanitaria no tienen personal técnico para centrarse en proyectos de aprendizaje automático. Esto a menudo implica que se dejan de usar datos valiosos o se contratan caros consultores para crear soluciones de aprendizaje automático.

Expertos en inteligencia artificial o aprendizaje automático, así como aquellos que están aprendiendo sobre inteligencia artificial o aprendizaje automático, pueden usar Azure Machine Learning Studio para diseñar los experimentos. Machine Learning Studio es un entorno de diseño basado en web que se usa para crear experimentos de aprendizaje automático. Con él puede crear, entrenar, evaluar y puntuar modelos, lo que ahorra un tiempo valioso cuando se usan diferentes herramientas para desarrollar modelos.

Machine Learning Studio ofrece un completo conjunto de herramientas para cargas de trabajo de aprendizaje automático. Esto significa que los recién llegados al aprendizaje automático pueden ponerse en marcha rápidamente utilizando la herramienta y producir resultados más rápido que con otras herramientas de aprendizaje automático. Gracias a ello, su personal informático puede centrarse en aportar valor en otros ámbitos y sin recurrir a un especialista en aprendizaje automático. Esta funcionalidad en su propia organización de atención sanitaria implica que se pueden probar varias hipótesis y los datos resultantes se pueden analizar para obtener información práctica; por ejemplo, [módulos escritos previamente](/azure/machine-learning/studio-module-reference/index?WT.mc_id=ms-docs-dastarr#help-for-machine-learning-modules) de ofertas sobre el intervencionismo del paciente para usarlos en un lienzo de arrastrar y soltar, componiendo visualmente flujos de trabajo de ciencia de datos de un extremo a otro como experimentos.

Existen módulos escritos previamente que encapsulan algoritmos específicos, como árboles de decisión, bosques de decisión, agrupación en clústeres, series temporales, detección de anomalías y otros.

Los módulos personalizados pueden agregarse a cualquier experimento. Están escritos en [lenguaje R](/azure/machine-learning/studio/extend-your-experiment-with-r?WT.mc_id=ms-docs-dastarr) o en [Python](/azure/machine-learning/studio/execute-python-scripts?WT.mc_id=ms-docs-dastarr). Esto permite usar los módulos precompilados, así como la lógica personalizada para crear un experimento más sofisticado.

Machine Learning Studio permite [crear y usar](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model?WT.mc_id=ms-docs-dastarr) modelos de aprendizaje, así como proporcionar un conjunto de experimentos prediseñados para usar en aplicaciones comunes. Además, pueden agregarse nuevos experimentos a Machine Learning Studio sin cambiar ninguno de los recursos del plano técnico.

Para ahorrar tiempo, visite [Azure AI Gallery](https://gallery.azure.ai/industries/healthcare?WT.mc_id=ms-docs-dastarr) para encontrar soluciones de aprendizaje automático listas para usar para sectores específicos, incluida la atención sanitaria. Por ejemplo, la galería incluye soluciones y los experimentos para la predicción de una enfermedad cardíaca y de detección de cáncer de mama.

## <a name="security-and-compliance"></a>Seguridad y cumplimiento normativo

La seguridad y el cumplimiento son dos de las cosas más importantes que se deben tener en cuenta al crear, instalar o administrar sistemas de software en un entorno de atención sanitaria. La inversión realizada en la adopción de un sistema de software puede verse menoscabada por no cumplir con las directivas de seguridad y las certificaciones requeridas.

Aunque este artículo y el plano técnico de atención sanitaria se centran en la seguridad técnica, otros tipos de seguridad también son importantes, como la seguridad física y la seguridad administrativa. Estos temas de seguridad están fuera del ámbito de este artículo, que se centra en la seguridad técnica del plano técnico.

### <a name="principle-of-least-privilege"></a>Principio de privilegio mínimo

El proyecto instala usuarios designados con roles para admitir y limitar sus necesidades de acceso a los recursos de la solución. Este modelo se conoce como el "principio de privilegio mínimo", un enfoque para el acceso a los recursos en el diseño del sistema. El principio afirma que las cuentas de servicio y de usuario deben tener acceso únicamente a los sistemas y los servicios necesarios para un propósito legítimo.

Este modelo de seguridad garantiza el cumplimiento del sistema de los requisitos de HIPAA y HITRUST, eliminando el riesgo para la organización.

### <a name="defense-in-depth"></a>Defensa en profundidad

Los diseños de sistemas que utilizan múltiples capas de abstracción de controles de seguridad están usando la defensa en profundidad. La defensa en profundidad proporciona redundancia de seguridad en varios niveles. Esto significa que no depende de una sola capa de defensa. Se garantiza que las cuentas de usuario y servicio tengan acceso adecuado a los recursos, los servicios y los datos. Azure proporciona recursos de seguridad y supervisión a todos los niveles de la arquitectura del sistema para proporcionar defensa en profundidad para todo el entorno de tecnologías.

En un sistema de software, como el instalado por el plano técnico, un usuario puede iniciar sesión pero no tener permiso para un recurso específico. En este ejemplo, la defensa en profundidad se proporciona mediante el control de acceso basado en rol y AAD, admitiendo el principio de privilegio mínimo.

La autenticación de dos factores también es una forma de defensa técnica en profundidad y puede incluirse opcionalmente cuando se instale el plano técnico.

### <a name="azure-key-vault"></a>Azure Key Vault

El servicio Azure Key Vault es un contenedor (almacén) utilizado para almacenar secretos, certificados y otros datos utilizados por las aplicaciones. Entre ellos hay cadenas de base de datos, direcciones URL de puntos de conexión en reposo, claves de API y otros elementos que los desarrolladores no desean codificar de forma rígida en una aplicación ni distribuir en un archivo .config.

Además, los almacenes son accesibles por las identidades de servicio de la aplicación u otras cuentas con permisos de AAD. De este modo, es posible tener acceso a los secretos en el entorno de ejecución por parte las aplicaciones que necesiten el contenido de un almacén.

Las claves almacenadas en un almacén pueden estar cifradas o firmadas, y el uso de las claves puede supervisarse para comprobar si existen cuestiones que puedan afectar a la seguridad.

Si se elimina una instancia de Key Vault, esta no se purga de inmediato en Azure. Las consecuencias de esto se detallan en la sección _Problemas técnicos > Key Vault_ de este artículo.

### <a name="application-insights"></a>Application Insights

Las organizaciones sanitarias a menudo disponen de sistemas de importancia crítica que deben ser confiables y resistentes. Las anomalías o interrupciones del servicio se deben detectar y corregir tan pronto como sea posible. [Application Insights](/azure/application-insights/app-insights-proactive-application-security-detection-pack?WT.mc_id=ms-docs-dastarr) es una tecnología de administración del rendimiento de las aplicaciones que supervisa las aplicaciones y envía alertas cuando algo va mal. Supervisa las aplicaciones en el entorno de ejecución para detectar errores o anomalías. Está diseñada para funcionar con múltiples lenguajes de programación y proporciona un amplio conjunto de capacidades para ayudar a garantizar que las aplicaciones estén funcionando correctamente.

Por ejemplo, es posible que una aplicación tenga una pérdida de memoria. Application Insights puede ayudarle a buscar y diagnosticar problemas similares a través de la funcionalidad de informes enriquecidos y los KPI que supervisa. Application Insights es un servicio de administración del rendimiento de las aplicaciones muy eficaz para los desarrolladores de aplicaciones.

Esta [demostración interactiva](https://analytics.applicationinsights.io/demo#/discover/home?WT.mc_id=ms-docs-dastarr) muestra las características y capacidades clave de Application Insights, incluido un completo panel de supervisión que los encargados de la tecnología de la organización sanitaria pueden usar para supervisar el estado de la aplicación.

#### <a name="azure-security-center"></a>Azure Security Center

La seguridad en tiempo real y la supervisión de los KPI es una necesidad en aplicaciones de crucial importancia. Azure Security Center (ASC) ayuda a garantizar que los recursos de Azure están seguros y protegidos. ASC es un servicio de administración de seguridad y protección avanzada contra amenazas. Se puede utilizar para aplicar directivas de seguridad en las cargas de trabajo, limitar la exposición a amenazas y detectar y responder a los ataques.

El nivel estándar de Security Center proporciona los siguientes servicios.

- **Seguridad híbrida**: obtenga una vista unificada de la seguridad de todas sus cargas de trabajo locales y en la nube. Esto resulta especialmente útil en redes de nube híbridas que utilizan las organizaciones sanitarias con Azure.
- **Detección de amenazas avanzada**: ASC utiliza análisis avanzado y [Microsoft Intelligent Security Graph](http://cloud-platform-assets.azurewebsites.net/intelligent-security-graph/?WT.mc_id=ms-docs-dastarr) para adelantarse a los ciberataques en constante cambio y mitigar su acción.
- **Controles de acceso y de aplicación**: bloquee el malware y otras aplicaciones no deseadas aplicando recomendaciones de inclusión en lista blanca para sus cargas de trabajo específicas y basadas en el aprendizaje automático.

En el contexto del plano técnico de inteligencia artificial para la atención sanitaria, ASC analiza los componentes del sistema y proporciona un panel que muestra las vulnerabilidades en los servicios y recursos de la suscripción. Los diferentes elementos del panel proporcionan visibilidad sobre los problemas de una solución del modo siguiente.

- Directiva y cumplimiento
- Protección de seguridad de recursos
- Protección contra amenazas

El siguiente es un ejemplo de panel en el que se identifican 13 sugerencias para mejorar las vulnerabilidades de amenazas contra el sistema. También muestra un escaso cumplimiento del 46 % con HIPAA y la directiva.

![Protección contra amenazas](assets/sg-healthcare-ai-blueprint-assets/threat_protection.png)

Al profundizar en los problemas de seguridad de alta gravedad se revela qué recursos resultan afectados y la solución necesaria para cada uno de ellos, como se muestra a continuación.

El personal informático puede pasar muchas horas intentando proteger manualmente todos los recursos y redes. Con el uso de ASC para identificar las vulnerabilidades en un sistema determinado, se puede invertir el tiempo en otros objetivos estratégicos. Para muchas de las vulnerabilidades identificadas, ASC puede aplicar automáticamente la acción correctiva y proteger el recurso sin que un administrador tenga que investigar el problema en profundidad.

![Riesgos altos](assets/sg-healthcare-ai-blueprint-assets/high_risks.png)

ACS hace aún más a través de sus funciones de alerta y detección de amenazas. Use ASC para supervisar las redes, las máquinas y los servicios en la nube para detectar ataques entrantes y la actividad posterior a una infracción de seguridad y mantener así su entorno seguro.
ASC recopila, analiza e integra información de seguridad y registros de diferentes recursos de Azure automáticamente. 

Las funciones de aprendizaje automático en ASC le permiten detectar amenazas que los enfoques manuales no revelarían. En ASC se muestra una lista de alertas de seguridad clasificadas según la prioridad, junto con la información que necesita para investigar rápidamente el problema y recomendaciones para solucionar un ataque.

### <a name="rbac-security"></a>Seguridad de RBAC

El [control de acceso basado en rol](/azure/role-based-access-control/role-assignments-portal?WT.mc_id=ms-docs-dastarr) (RBAC) proporciona o deniega el acceso a recursos protegidos, a veces con derechos específicos por recurso. Esto garantiza que solo los usuarios adecuados puedan tener acceso a sus componentes del sistema designado. Por ejemplo, un administrador de base de datos puede tener acceso a una base de datos que contiene datos cifrados de los pacientes, mientras que un proveedor de atención sanitaria solo puede tener acceso a los registros del paciente pertinentes a través de la aplicación que los muestra. Esto suele ser un sistema de historial médico electrónico o de historial sanitario electrónico. La enfermera no necesita acceder a las bases de datos y el administrador de la base de datos no necesita ver los datos del historial médico de un paciente.

Para hacer esto posible, RBAC forma parte de la seguridad de Azure y permite una administración de acceso centrada con precisión para los recursos de Azure. La configuración específica para cada usuario permite a los administradores de seguridad y sistemas ser muy precisos en los derechos que se conceden a cada usuario.

### <a name="blueprint-responsibility-matrix"></a>Matriz de responsabilidades del plano técnico

La matriz de responsabilidades del cliente HITRUST es un documento de Excel que admite la implementación y documentación por los clientes de controles de seguridad para los sistemas compilados en Azure. El libro enumera los requisitos de HITRUST pertinentes y explica cómo Microsoft y el cliente son responsables del cumplimiento de cada uno de ellos.

Comprender la responsabilidad compartida de implementar controles de seguridad en un entorno en la nube es esencial para los clientes que desarrollan sistemas en Azure. La implementación de un control de seguridad específico puede ser responsabilidad de Microsoft, responsabilidad de los clientes o una responsabilidad compartida entre Microsoft y los clientes. Las diferentes implementaciones en la nube afectan al modo en que las responsabilidades se comparten entre Microsoft y nuestros clientes.

Consulte la tabla de responsabilidades a continuación para obtener ejemplos.

|Responsabilidades de Azure  |Responsabilidades del cliente  |
|---------|---------|
|Azure es responsable de la implementación, administración y supervisión de los métodos y mecanismos del programa de protección de información en relación con su entorno de aprovisionamiento de servicios. | El cliente es responsable de la implementación, configuración, administración y supervisión de los métodos y mecanismos del programa de protección de información para activos controlados por el cliente que se usan para acceder a servicios de Azure y utilizarlos. |
|Azure es responsable de la implementación, configuración, administración y supervisión de los métodos y mecanismos de administración de cuentas en relación con su entorno de aprovisionamiento de servicios. |El cliente también es responsable de la administración de cuentas de las instancias de las máquinas virtuales de Azure implementadas y los componentes de aplicación residentes.|

Estos son solo dos ejemplos de las muchas responsabilidades que deben tenerse en cuenta al implementar sistemas en la nube. La matriz de responsabilidades del cliente HITRUST está diseñada para respaldar el cumplimiento de la organización con HITRUST en la implementación de un sistema Azure.

## <a name="customization"></a>Personalización

Es habitual personalizar el plano técnico después de instalarlo. Los motivos y las técnicas para ello varían según el caso.

El plano técnico se puede personalizar antes de la instalación mediante la modificación de los scripts de instalación. Si bien esto es posible, es aconsejable crear scripts de PowerShell independientes para ejecutar una vez completada la instalación inicial. También se pueden agregar nuevos servicios al sistema a través del portal una vez completada la instalación inicial.

Las personalizaciones pueden incluir uno o varios de los siguientes aspectos.

- Adición de nuevos experimentos a Machine Learning Studio
- Adición de servicios no relacionados adicionales al entorno
- Modificación de la ingesta de datos y el resultado del experimento de aprendizaje automático para usar un origen de datos diferente de la base de datos patientdb SQL de Azure
- Provisión de datos de producción al experimento de aprendizaje automático
- Limpieza de los datos de su propiedad que se ingieren para que coincidan con los que necesita el experimento

La personalización de la instalación es similar al trabajo que se desarrolla con cualquier solución de Azure. Pueden agregarse o quitarse servicios o recursos, proporcionando nuevas capacidades. Cuando personalice el plano técnico, tenga cuidado de no alterar la canalización de aprendizaje automático global para asegurar que la implementación continúe funcionando.

## <a name="technical-issues"></a>Problemas técnicos

Los siguientes problemas pueden provocar que se produzca un error en la instalación del plano técnico o que se instale con una configuración no deseada.

### <a name="key-vault"></a>Key Vault

Los almacenes de claves son únicos cuando se elimina un recurso de Azure. Azure conserva estos almacenes por si fuera necesario recuperarlos. En consecuencia, se debe pasar un prefijo diferente en el script de instalación cada vez que se ejecute dicho script o la instalación generará un error debido a una colisión con el nombre antiguo del almacén. Los almacenes de claves, y todos los demás recursos, reciben un nombre basado en el prefijo que proporcione al script de instalación.

Una instancia de Key Vault creada por el script de instalación se mantiene como una "eliminación temporal" durante 30 días. Aunque no están accesible a través del portal, [las instancias de Key Vault eliminadas temporalmente son fáciles de administrar desde PowerShell](/azure/key-vault/key-vault-soft-delete-powershell?WT.mc_id=ms-docs-dastarr) y pueden incluso eliminarse manualmente.

### <a name="azure-active-directory"></a>Azure Active Directory

Se recomienda encarecidamente que instale el plano técnico en una instancia de AAD vacía en lugar de en un sistema de producción. Cree una nueva instancia de AAD y use su identificador de inquilino durante la instalación para evitar la adición de cuentas de plano técnico a la instancia de AAD activa.

## <a name="technologies-presented"></a>Tecnologías presentadas

- Obtenga más información sobre el [plano técnico de inteligencia artificial y datos médicos de Azure](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr).
- Descargue, clone o bifurque [aquí el repositorio de GitHub](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md).
- [Machine Learning Studio](/azure/machine-learning/?WT.mc_id=ms-docs-dastarr) es el área de trabajo y la herramienta que los científicos de datos usan para crear los experimentos de Machine Learning. Permite usar algoritmos integrados, widgets de propósito especial y scripts de Python y R.
- Los secretos, certificados y otros datos privados se mantienen en [Azure Key Vault](/azure/key-vault/key-vault-whatis?WT.mc_id=ms-docs-dastarr).
- El lenguaje de scripting PowerShell es fundamental para configurar el proyecto, aunque los comandos necesarios se presentan en las instrucciones de instalación.
- [Azure AI Gallery](https://gallery.azure.ai/) ofrece un recetario de soluciones de inteligencia artificial o aprendizaje automático útiles para los clientes organizadas por sector. Existen varias soluciones publicadas por científicos de datos, junto con otros expertos para el sector sanitario.
- [Azure Security Center](/azure/security-center/?WT.mc_id=ms-docs-dastarr) proporciona información sobre el comportamiento, las vulnerabilidades y las técnicas de mitigación de su aplicación.

## <a name="wrapping-up"></a>Resumen

El [plano técnico de inteligencia artificial sobre datos médicos de Azure](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr) es una completa solución de aprendizaje automático y se puede usar como una herramienta de aprendizaje para que los encargados de la tecnología entiendan mejor cómo funciona Azure y cómo garantizar que los sistemas se ajusten a las normativas sanitarias. También puede usarse como punto de partida para un sistema de producción que use Azure Machine Learning Studio como punto focal.

[Descargue el plano técnico](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md) para empezar a trabajar con su implementación en horas, no días o semanas. Si tiene problemas con la instalación o le surgen dudas sobre el plano técnico, visite [la página de preguntas frecuentes](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md).

Descargue el material de apoyo para obtener una mayor información sobre la implementación del plano técnico más allá de la instalación y el experimento de aprendizaje automático. Este material incluye lo siguiente.

1. [Matriz de responsabilidades del cliente de HITRUST](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
2. [El modelo de amenazas completo](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
3. [Documento de revisión de inteligencia artificial y datos médicos de HITRUST](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
4. [Revisión de inteligencia artificial y datos médicos de HIPAA](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics?WT.mc_id=ms-docs-dastarr)

Con independencia de que está usando el plano técnico con fines de aprendizaje o como inicio de una solución de inteligencia artificial o aprendizaje automático para su organización, sirve como punto de partida para trabajar con inteligencia artificial o aprendizaje automático de Azure con un enfoque en la atención sanitaria.