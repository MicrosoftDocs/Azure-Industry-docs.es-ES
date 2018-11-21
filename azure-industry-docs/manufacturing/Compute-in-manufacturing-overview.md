---
title: Proceso de alta potencia, a petición y escalable
author: ercenk
ms.author: ercenk
ms.date: 07/11/2018
ms.topic: article
ms.service: industry
description: Información general sobre el proceso de alta potencia necesario en la industria de fabricación.
ms.openlocfilehash: b34f2dd3930a2f663382a20f195e147aae66f90d
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654222"
---
# <a name="on-demand-scalable-high-power-compute"></a>Proceso de alta potencia, a petición y escalable

## <a name="introduction"></a>Introducción

Los clientes demandan productos que tengan estos atributos: ligero, fuerte, seguro, sostenible y personalizado. Como resultado, la fase de diseño se ha vuelto cada vez más compleja. En esa fase, se usan equipos para visualizar, analizar, simular y optimizar. Y esas tareas se volverán más sofisticadas y consumirán más recursos. A esto se agrega el hecho de que los productos están cada vez más conectados y generan enormes cantidades de datos que es necesario procesar y analizar.

Todo esto conduce a una única necesidad: grandes recursos de computación, a petición.

En este artículo, se examinarán algunas áreas conocidas de ingeniería y fabricación que necesitan gran potencia de computación, y se explora cómo puede ayudar la plataforma Microsoft Azure.

## <a name="cloud-design-workstations"></a>Estaciones de trabajo de diseño en la nube

Los diseñadores de productos usan diversas herramientas de software durante las fases de diseño y planeamiento del ciclo de vida de desarrollo de un producto. Las herramientas CAD requieren sólidas capacidades gráficas en la estación de trabajo del diseñador, y el costo de estas estaciones de trabajo es elevado. Estas estaciones de trabajo mejoradas se encuentran normalmente dentro de las oficinas de los diseñadores, lo que los vincula físicamente a una ubicación.

A medida que las soluciones en la nube comenzaron a ganar más popularidad y nuevas funcionalidades se volvieron disponibles, la idea de estaciones de trabajo en la nube comenzó a ser más viable. El uso de una estación de trabajo hospedada en la nube permite al diseñador acceder a ella desde cualquier parte. También, permite que la organización cambie el modelo de costo de gastos de capital a gastos operativos.

### <a name="remote-desktop-protocol"></a>Protocolo de escritorio remoto

El protocolo de escritorio remoto (RDP) de Microsoft ha admitido únicamente TCP durante mucho tiempo. TCP presenta más sobrecarga que UDP. A partir de RDP 8.0, UDP está disponible para los servidores que ejecutan los Servicios de Escritorio remoto de Microsoft. Para que pueda usarse, una máquina virtual (VM) debe tener suficientes recursos de hardware, a saber: CPU, memoria y, principalmente, unidad de procesamiento gráfico (GPU). (Podría decirse que la GPU es el componente más importante de una estación de trabajo en la nube de alto rendimiento). Windows Server 2016 ofrece varias opciones para acceder a las funcionalidades gráficas subyacente. La solución de GPU de RDS predeterminada, también conocida como Windows Advanced Rasterization Platform (WARP), es una solución adecuada para los escenarios de trabajador del conocimiento, pero proporciona recursos inadecuados para los escenarios de estación de trabajo en la nube. vGPU de RemoteFX es una característica de RemoteFX que se introdujo para las conexiones remotas, que se proporciona para escenarios con altas densidades de usuarios por servidor, lo que permite la utilización de GPU de ráfaga elevada. Sin embargo, cuando llega el momento de usar la potencia de la GPU, se necesita la asignación de dispositivos discretos (DDA) para utilizarla plenamente.

Las máquinas virtuales de la serie NV están disponibles con una o varias GPU NVDIA como parte de la oferta de la serie N de Azure. Estas máquinas virtuales están optimizadas para escenarios de visualización remota y VDI mediante plataformas como OpenGL y DirectX. Si se aumenta a 4 GPU, es posible aprovisionar estaciones de trabajo que aprovechan por completo las ventajas de la GPU mediante DDA en Azure.

Un punto muy importante que vale la pena mencionar es la capacidad de programación de la plataforma Azure. Así, ofrece varias opciones para una máquina virtual. Por ejemplo, puede aprovisionar una estación de trabajo a petición. También puede mantener el estado de la máquina remota en archivos locales mediante discos de Azure en Premium Storage o Azure Files. Estas opciones le otorgan la capacidad de controlar los costos. La colaboración de Microsoft con Citrix, con sus soluciones XenDesktop y XenApp, también proporciona otra alternativa para una solución de virtualización de escritorio.

## <a name="analysis-and-simulation"></a>Análisis y simulación

El análisis y simulación de los sistemas físicos en equipos se conoce desde hace tiempo. El análisis de elementos finitos (FEA) es un método numérico utilizado para resolver muchos problemas de análisis. FEA requiere mucha potencia de cálculo para realizar cálculos de matrices de gran tamaño. El número de matrices implicadas en la solución de un modelo FEA explota exponencialmente según avanzamos de 2D a 3D, y a medida que se agrega granularidad a la malla de FEA. Por tanto, se necesita potencia de computació a petición.
Es importante que el código de solución de problemas se pueda ejecutar en paralelo, a fin de aprovechar la escalabilidad de los recursos.

La solución de problemas de simulación requiere recursos de computación a gran escala. La informática de alto rendimiento (HPC) es una clase de computación a gran escala. HPC requiere baja latencia de red de back-end, con funcionalidades de acceso directo a memoria remota (RDMA) para realizar cálculos paralelos rápidos. La plataforma Azure ofrece máquinas virtuales creadas para informática de alto rendimiento. Cuentan con procesadores especializados emparejados con memoria DDR4, y permiten que las soluciones de proceso intensivo se ejecuten de forma eficaz, tanto en instalaciones Windows como Linux. Y están disponibles en varios tamaños. Consulte [Tamaños de máquina virtual de procesos de alto rendimiento](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk).
Para ver cómo Azure admite HPC de otras maneras, consulte [Big Compute: HPC y Batch](https://azure.microsoft.com/en-us/solutions/big-compute/?WT.mc_id=computeinmanufacturing-docs-ercenk).

La plataforma Azure permite escalar vertical y horizontalmente las soluciones. Uno de los paquetes de software más conocidos para la simulación es STAR-CCM +, de CD-adapco. [En un estudio publicado](https://azure.microsoft.com/en-us/blog/availability-of-star-ccm-on-microsoft-azure/?WT.mc_id=computeinmanufacturing-docs-ercenk) que demuestra STAR-CCM + mediante la ejecución del modelo de dinámica de fluidos computacional (CDF), "Le Mans 100 million cell", se proporciona una perspectiva de la escalabilidad de la plataforma. El siguiente gráfico muestra la escalabilidad observada cuando se agregan más núcleos al ejecutar la simulación:

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/34f1873b-4db5-4c62-b963-8bdf3966cf60.png](assets/bigcompute-assets/starccm.png)

Otro popular paquete de software conocido de análisis de ingeniería es ANSYS CFD. Este paquete permite a los ingenieros realizar análisis multifísicos, como fuerzas de fluidos, efectos térmicos, integridad estructural y radiación electromagnética. [El estudio publicado](https://azure.microsoft.com/en-us/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk) demuestra la escalabilidad de la solución en Azure, con resultados similares.

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/77129585-f25c-4c29-b22b-80c627d03daa.png](assets/bigcompute-assets/fluent.png)

En lugar de invertir en un clúster de proceso local, se puede implementar un paquete de software que requiere la ejecución en paralelo en máquinas virtuales de Azure, o [conjuntos de escalado de máquinas virtuales (VMSS)](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) mediante las familias de [máquinas virtuales HPC y GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk) para una solución íntegramente en la nube.

### <a name="burst-to-azure"></a>Ráfaga a Azure

Si se dispone de un clúster local, otra opción es ampliarlo a Azure y, por tanto, descargar las cargas de trabajo máximas (lo que también se conoce como creación de ráfagas a Azure). Para ello, es necesario usar uno de los administradores locales de cargas de trabajo que admiten Azure (p. ej. [Alces Flight Compute](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview?WT.mc_id=computeinmanufacturing-docs-ercenk), [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/?WT.mc_id=computeinmanufacturing-docs-ercenk), [Bright Cluster Manager](http://www.brightcomputing.com/technology-partners/microsoft), [IBM Spectrum Symphony y Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/?WT.mc_id=computeinmanufacturing-docs-ercenk), [PBS Pro](http://pbspro.org/), [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx?WT.mc_id=computeinmanufacturing-docs-ercenk)).

Otra opción es Azure Batch, que es un servicio para ejecutar trabajos por lotes HPC y en paralelo a gran escala de forma eficaz. Azure Batch permite trabajos que usan la API de interfaz de paso de mensajes (MPI). Batch admite familias de máquinas virtuales optimizadas para [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx?WT.mc_id=computeinmanufacturing-docs-ercenk) e Intel MPI con [HPC](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-hpc?WT.mc_id=computeinmanufacturing-docs-ercenk) y [GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk). Microsoft ha adquirido también [Cycle Computing](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk), que proporciona una solución que ofrece un mayor nivel de abstracción para la ejecución de clústeres en Azure. Otra opción consiste en ejecutar [superequipos de Cray](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) en Azure con acceso completo a servicios de Azure complementarios, como [Azure Storage](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/premium-storage?WT.mc_id=computeinmanufacturing-docs-ercenk) y [Azure Data Lake](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview?WT.mc_id=computeinmanufacturing-docs-ercenk).

## <a name="generative-design"></a>Diseño generativo

El proceso de diseño siempre es iterativo. Un diseñador comienza con un conjunto de restricciones y parámetros para un diseño de destino y recorre en iteración varias alternativas de diseño hasta finalmente fijar una que cumpla las restricciones.
Sin embargo, cuando la potencia de cálculo es prácticamente infinita, se podrían evaluar *miles* o incluso *millones* de alternativas de diseño en lugar de unas pocas. Este viaje comenzó con modelos paramétricos y su uso en herramientas CAD. Ahora, gracias a la gran cantidad de recursos de cálculo de las plataformas de nube, la industria avanza otro paso. El diseño generativo es el término que describe el proceso de diseño de proporcionar parámetros y restricciones a la herramienta de software. La herramienta genera entonces alternativas de diseño y se crean varias permutaciones de una solución.
Existen varios enfoques para el diseño generativo: optimización de la topología, optimización del entramado, optimización de la superficie y síntesis de la forma. Los detalles de estos enfoques escapan del ámbito de este artículo. Sin embargo, el modelo común entre estos enfoques es la necesidad de acceder a entornos de proceso intensivo.

El punto de partida del diseño generativo es definir los parámetros de diseño sobre los que se debe recorrer en iteración el algoritmo, junto con los incrementos y los intervalos de valores razonables. El algoritmo crea entonces una alternativa de diseño para cada combinación válida de estos parámetros. El resultado es un gran número de alternativas de diseño. La creación de estas alternativas requiere una gran cantidad de recursos de computación.
También debe ejecutar todas las simulaciones y tareas de análisis para cada alternativa de diseño. El resultado neto es que necesita entornos de proceso intensivo.

Las diversas opciones de Azure para escalar verticalmente a petición según las necesidades de proceso mediante [Azure Batch](https://docs.microsoft.com/en-us/azure/batch/batch-technical-overview?WT.mc_id=computeinmanufacturing-docs-ercenk) y [VMSS](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) son los destinos naturales para esas cargas de trabajo.

## <a name="machine-learning-ml"></a>Machine Learning (ML)

En un nivel muy simplista, podemos generalizar los sistemas de aprendizaje automático de este modo: dado un punto de datos o un conjunto de puntos de datos, el sistema devuelve un resultado correlacionado. De este modo, los sistemas de aprendizaje automático se usan para resolver preguntas, como por ejemplo:

- Dados los últimos precios de casas y propiedades de casas, ¿cuál es el precio previsto de una casa determinada en el mercado?

- Dadas las lecturas de diversos sensores y los casos pasados de averías de una máquina, ¿cuál es la posibilidad de que esa máquina se averíe en el período siguiente?

- Dado un conjunto de imágenes, ¿cuál es un gato doméstico?

- Dada una fuente de vídeo de un oleoducto, ¿hay una sección dañada de este con abolladuras importantes?

La incorporación de una funcionalidad analítica avanzada mediante inteligencia artificial (IA) y aprendizaje automático (ML) comienza con el desarrollo de un modelo, con un proceso similar al siguiente.

![](assets/bigcompute-assets/aipipeline.png)

La [selección del algoritmo](https://docs.microsoft.com/en-us/azure/machine-learning/studio/algorithm-choice?WT.mc_id=computeinmanufacturing-docs-ercenk) depende del tamaño, la calidad y la naturaleza de los datos, así como del tipo de la respuesta esperada. Según el tamaño de entrada y el algoritmo seleccionado, junto con el entorno de computación, este paso normalmente requiere recursos de proceso intensivo y puede tardar diferentes horas en completarse. El gráfico siguiente es de [un artículo técnico](https://blogs.technet.microsoft.com/machinelearning/2017/07/25/lessons-learned-benchmarking-fast-machine-learning-algorithms/?WT.mc_id=computeinmanufacturing-docs-ercenk) para comparar el entrenamiento de algoritmos de aprendizaje automático; muestra el tiempo en completarse el ciclo de entrenamiento dados diferentes algoritmos, tamaños de conjunto de datos y destinos de cálculo (GPU o CPU).

![](assets/bigcompute-assets/vmsizes.png)

El factor principal para la decisión es el problema empresarial. Si el problema requiere el procesamiento de un gran conjunto de datos con un algoritmo adecuado, el factor determinante son los recursos de proceso a escala de nube para entrenar el algoritmo.
[Azure Batch AI](https://azure.microsoft.com/en-us/services/batch-ai/?WT.mc_id=computeinmanufacturing-docs-ercenk) es un servicio que entrena modelos de inteligencia artificial en paralelo y a escala.

Con Azure Batch AI, un científico de datos puede desarrollar una solución en la estación de trabajo mediante [Azure Data Science Virtual Machine (DSVM)](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) o [Azure Deep Learning Virtual Machine (DLVM)](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/deep-learning-dsvm-overview?WT.mc_id=computeinmanufacturing-docs-ercenk) e insertar el entrenamiento en el clúster. DSVM y DLVM son imágenes de máquina virtual especialmente configuradas con un amplio conjunto de herramientas y ejemplos ya instalados.

![](assets/bigcompute-assets/azurebatchai.png)

## <a name="conclusion"></a>Conclusión

El sector de fabricación requiere una cantidad enorme de cálculos matemáticos, mediante el acceso a componentes de hardware de gama alta, incluidas las unidades de procesamiento gráfico (GPU). Es fundamental la escalabilidad y elasticidad de la plataforma que hospeda los recursos. Para controlar los costos, debe estar disponible a petición y proporcionar al mismo tiempo la velocidad óptima.

La plataforma Microsoft Azure proporciona una amplia gama de opciones para satisfacer estas necesidades. Puede empezar desde cero y controlar todos los recursos y aspectos de ella para crear su propia solución. O bien, puede encontrar un asociado de Microsoft que acelere la creación de la solución. Nuestros asociados saben cómo aprovechar la potencia de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Configure una estación de trabajo en la nube mediante la implementación de una [máquina virtual de la serie NV](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk).

- Revise las [opciones](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/high-performance-computing?WT.mc_id=computeinmanufacturing-docs-ercenk) para la implementación de una herramienta para su necesidades de diseño a fin de aprovechar las ventajas de las funcionalidades HPC de Azure

- Conozca las posibilidades de [Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/?WT.mc_id=computeinmanufacturing-docs-ercenk)
