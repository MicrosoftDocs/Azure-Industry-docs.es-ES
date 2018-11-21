# <a name="enabling-the-financial-services-risk-lifecycle-with-azure-and-r"></a>Habilitación del ciclo de vida de riesgos de los servicios financieros con Azure y R


Los cálculos de riesgos son fundamentales en varias etapas del ciclo de vida de operaciones de servicio financieros clave. Por ejemplo, una forma simplificada del ciclo de vida de la administración de productos de seguros puede tener un aspecto similar al diagrama siguiente. Los aspectos del cálculo de riesgos se muestran con el texto azul.

![](./assets/fsi-risk-modeling/image1.png)

Un escenario en una empresa dedicada a los mercados de capitales puede tener el siguiente aspecto:

![](./assets/fsi-risk-modeling/image2.png)

A través de estos procesos, hay necesidades comunes en torno al modelado de riesgo que incluyen las siguientes:

1.  La necesidad de una experimentación ad hoc relacionada con los riesgos por parte de los analistas de riesgos, los actuarios de una entidad aseguradora o los analistas cuantitativos de una empresa dedicada a los mercados de capitales.
    Habitualmente, estos analistas trabajan con herramientas de modelado y código populares en sus dominios: R y Python. Muchos programas de estudios universitarios incluyen cursos de R o Python relacionados con las finanzas matemáticas y en cursos de MBA.
    Ambos lenguajes ofrecen una amplia variedad de bibliotecas de código abierto que admiten los cálculos de riesgos populares. Junto con las herramientas adecuadas, los analistas a menudo necesitan acceso a:

     a.  Datos precisos sobre los precios de mercado.

    b.  Datos de reclamaciones y directivas existentes.

    c.  Datos existentes de la posición de mercado.

    d.  Otros datos externos. Los orígenes incluyen datos estructurados, como tablas de mortalidad y datos de precios de la competencia. También se pueden usar orígenes menos tradicionales, como el clima, las noticias, etc.

    e.  Capacidad informática para permitir investigaciones rápidas de datos interactivos.

2.  También pueden usar algoritmos de aprendizaje automático ad hoc para fijar los precios o determinar la estrategia de mercado.

3.  La necesidad de visualizar y presentar datos para usarlos en el planeamiento de productos, la estrategia comercial y análisis similares.

4.  La ejecución rápida de los modelos definidos, configurados por los analistas, para precios, valoraciones y riesgo de mercado. Las valoraciones usan una combinación de modelado de riesgos dedicada, herramientas de riesgo de mercado y código personalizado. El análisis se ejecuta en un lote con diversos cálculos nocturnos, semanales, mensuales, trimestrales y anuales que generan picos en las cargas de trabajo.

5.  La integración de los datos con otras medidas de riesgo empresarial para informes de riesgo consolidados. En las organizaciones más grandes, las estimaciones de riesgo de menor nivel se pueden transferir a una herramienta de informes y modelado de riesgos empresariales.

6.  Los resultados se deben informar en un formato definido en el intervalo necesario para satisfacer los requisitos de inversores y los requisitos reglamentarios.

Microsoft respalda las preocupaciones mencionadas a través de una combinación de servicios de Azure y de ofertas de asociados en [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely). En este artículo, mostramos ejemplos prácticos de cómo realizar una experimentación ad hoc mediante R. Para empezar, explicaremos cómo ejecutar el experimento en una sola máquina y, luego, se mostrará cómo ejecutar el mismo experimento en [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely) y cerraremos mostrando cómo aprovechar los servicios externos en nuestro modelado. Las opciones y consideraciones para la ejecución de modelos definidos en Azure se describen en estos artículos centrados en la [banca](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) y los [seguros](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).

## <a name="analyst-modelling-in-r"></a>Modelado de analistas en R 

Para empezar, examinemos cómo un analista puede usar R en un escenario de mercados de capitales simplificado y representativo. Para crearlo, haga referencia a una biblioteca existente de R para el cálculo o escriba código desde cero. En el ejemplo, también debemos capturar datos externos relacionados con los precios. Para que este ejemplo sea simple, pero ilustrativo, calculamos la posible exposición futura (PFE) de un contrato a plazo de índices bursátiles.
En este ejemplo se evitan las técnicas de modelado cuantitativo complejas para instrumentos como derivados complejos y se centra en un factor de riesgo único para concentrarse en el ciclo de vida del riesgo. En el ejemplo se hace lo siguiente:

1.  Seleccionar un instrumento de interés.

2.  Obtener los precios históricos para el instrumento.

3.  Modelar el precio de las acciones mediante un cálculo de Montecarlo (MC) sencillo, que usa el movimiento browniano geométrico (GBM):

     a.  Calcular la devolución μ (mu) y la volatilidad σ (theta) esperadas.

    b.  Calibrar el modelo según los datos históricos.

4.  Visualizar las diversas rutas para comunicar los resultados.

5.  Trazar max(0,Stock Value) para demostrar el significado de PFE, la diferencia respecto del valor en riesgo (VaR).

     a.  Para aclarar: PFE = precio por acción (T) -- precio K del contrato a plazo

6.  Tomar el centil 0,95 para obtener el valor de PFE en cada período o al final del período de simulación

Se calculará la posible exposición futura de un contrato de participación a futuro en función de las acciones de MSFT. Como ya se mencionó, para modelar los precios de las acciones, se requieren los precios históricos de las acciones de MSFT para poder calibrar el modelo según los datos históricos. Hay muchas maneras de adquirir los precios históricos de las acciones. En el ejemplo, usamos una versión gratuita de un servicio de cotización bursátil de un proveedor de servicios externo, [Quandl](https://www.quandl.com/).


> Nota: En el ejemplo se usa el [conjunto de datos WIKI Prices](https://www.quandl.com/databases/WIKIP), que se puede usar para aprender los conceptos. Para el uso en producción de capitales de los EE. UU., Quandl recomienda usar el [conjunto de datos End of Day US Stock Prices](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).

Para procesar los datos y definir el riesgo asociado con el capital, es necesario hacer lo siguiente:

1.  Recuperar datos históricos relacionados con el capital.

2.  Determinar la devolución μ y la volatilidad σ esperadas a partir de los datos históricos.

3.  Modelar los precios de acciones subyacentes mediante el uso de alguna simulación.

4.  Ejecutar el modelo.

5.  Determinar la exposición del capital en el futuro.

Para empezar, recuperamos las acciones a partir del servicio de Quandl y trazamos el historial de cotizaciones de cierre de los últimos 180 días.

````R
# Lubridate package must be installed
if (!require(lubridate)) install.packages('lubridate')
library(lubridate)

# Quandl package must be installed
if (!require(Quandl)) install.packages('Quandl')
library(Quandl)

# Get your API key from quandl.com
quandl_api = "enter your key here"

# Add the key to the Quandl keychain
Quandl.api_key(quandl_api)

quandl_get <-
    function(sym, start_date = "2018-01-01") {
        require(devtools)
        require(Quandl)
        # Retrieve the Open, High, Low, Close and Volume Column for a given Symbol
        # Column Indices can be deduced from this sample call
        # data <- Quandl(c("WIKI/MSFT"), rows = 1)

        tryCatch(Quandl(c(
        paste0("WIKI/", sym, ".8"),    # Column 8 : Open
        paste0("WIKI/", sym, ".9"),    # Column 9 : High
        paste0("WIKI/", sym, ".10"),   # Column 10: Low
        paste0("WIKI/", sym, ".11"),   # Column 11: Close
        paste0("WIKI/", sym, ".12")),  # Column 12: Volume
        start_date = start_date,
        type = "raw"
        ))
    }

# Define the Equity Forward
instrument.name <- "MSFT"
instrument.premium <- 100.1
instrument.pfeQuantile <- 0.95

# Get the stock price for the last 180 days
instrument.startDate <- today() - days(180)

# Get the quotes for an equity and transform them to a data frame
df_instrument.timeSeries <- quandl_get(instrument.name,start_date = instrument.startDate)

#Rename the columns
colnames(df_instrument.timeSeries) <- c()
colnames(df_instrument.timeSeries) <- c("Date","Open","High","Low","Close","Volume")

# Plot the closing price history to get a better feeling for the data
plot(df_instrument.timeSeries$Date, df_instrument.timeSeries$Close)
````

Con datos en mano, calibramos el modelo de GBM.

````R
# Code inspired by the book Computational Finance, An Introductory Course with R by 
#    A. Arratia.

# Calculate the daily return in order to estimate sigma and mu in the Wiener Process
df_instrument.dailyReturns <- c(diff(log(df_instrument.timeSeries$Close)), NA)

# Estimate the mean of std deviation of the log returns to estimate the parameters of the Wiener Process

estimateGBM_Parameters <- function(logReturns,dt = 1/252) {

    # Volatility
    sigma_hat = sqrt(var(logReturns)) / sqrt(dt)

    # Drift
    mu_hat = mean(logReturns) / dt + sigma_hat**2 / 2.0

    # Return the parameters
    parameter.list <- list("mu" = mu_hat,"sigma" = sigma_hat)

    return(parameter.list)
}

# Calibrate the model to historic data
GBM_Parameters <- estimateGBM_Parameters(df_instrument.dailyReturns[1:length(df_instrument.dailyReturns) - 1])
````

A continuación, modelamos los precios de acciones subyacentes. Podemos implementar el proceso discreto de GBM desde cero o utilizar uno de los muchos paquetes de R que brindan esta funcionalidad. Usamos el paquete [*sde* (simulación e inferencia de ecuaciones diferenciales estocásticas)](https://cran.r-project.org/web/packages/sde/index.html) de R, que proporciona un método para resolver este problema. El método de GBM requiere un conjunto de parámetros que se calibran según los datos históricos o que se dan como parámetros de simulación. Usamos los datos históricos, brindando los valores de μ, σ y los precios de las acciones al comienzo de la simulación (P0).

````R
if (!require(sde)) install.packages('sde')
library(sde)

sigma <-  GBM_Parameters$sigma
mu <- GBM_Parameters$mu
P0 <- tail(df_instrument.timeSeries$Close, 1)

# Calculate the PFE looking one month into the future
T <- 1 / 12

# Consider nt MC paths
nt=50

# Divide the time interval T into n discrete time steps
n = 2 ^ 8 

dt <- T / n
t <- seq(0,T,by=dt)
````

Ahora estamos preparados para iniciar una simulación de Montecarlo para modelar la posible exposición futura de algún número de rutas de simulación. Limitaremos la simulación a 50 rutas de Montecarlo y 256 períodos. Para preparar el escalado horizontal de la simulación y aprovechar la paralelización en R, el bucle de simulación de Montecarlo usa una instrucción foreach.

````R
# Track the start time of the simulation
start_s <- Sys.time()

# Instead of a simple for loop to execute a simulation per MC path, call the
# simulation with the foreach package
# in order to demonstrate the similarity to the AzureBatch way to call the method.

library(foreach)
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package
exposure_mc <- foreach (i=1:nt, .combine = rbind ) %do%  GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()

# Track the end time of the simulation
end_s <- Sys.time()

# Duration of the simulation

difftime(end_s, start_s) 
````

Ahora hemos simulado el precio de la acción de MSFT subyacente. Para calcular la exposición del contrato de participación a futuro, restamos la prima y limitamos la exposición solo a los valores positivos.

````R
# Calculate the total Exposure as V_i(t) - K, put it to zero for negative exposures
pfe_mc <- pmax(exposure_mc - instrument.premium ,0)

ymax <- max(pfe_mc)
ymin <- min(pfe_mc)
plot(t, pfe_mc[1,], t = 'l', ylim = c(ymin, ymax), col = 1, ylab="Credit Exposure in USD", xlab="time t in Years")
for (i in 2:nt) {
    lines(t, pfe_mc[i,], t = 'l', ylim = c(ymin, ymax), col = i)
}
````

Las dos imágenes siguientes muestran el resultado de la simulación. La primera imagen muestra la simulación de Montecarlo del precio de acción subyacente para 50 rutas. En la segunda imagen, se muestra el riesgo crediticio subyacente del contrato de participación a futuro después de restar la prima del contrato en cuestión y limitar la exposición a los valores positivos.


|       |    |
|---    |--- |
| <img src="./assets/fsi-risk-modeling/image3.png" width="400px" alt="Figure 1 - 50 Monte Carlo Paths"/> | <img src="./assets/fsi-risk-modeling/image4.png" width="400px" alt="Figure 2 - Credit Exposure for Equity Forward"/> |
| Ilustración 1: 50 rutas de Montecarlo | Ilustración 2: Riesgo crediticio del contrato de participación a futuro |


En el último paso, el PFE del centil 0,95 de un mes se calcula mediante el código siguiente.

````R
# Calculate the PFE at each time step
df_pfe <- cbind(t,apply(pfe_mc,2,quantile,probs = instrument.pfeQuantile ))

resulting in the final PFE plot
plot(df_pfe, t = 'l', ylab = "Potential Future Exposure in USD", xlab = "time t in Years")
````

<img src="./assets/fsi-risk-modeling/image5.png" width="500px" alt="Potential Future Exposure for MSFT Equity Forward" /> 

Ilustración 3: Posible exposición futura del contrato de participación a futuro de MSFT

## <a name="using-azure-batch-with-r"></a>Uso de Azure Batch con R 

La solución de R descrita anteriormente se puede conectar a Azure Batch y puede aprovechar la nube para realizar los cálculos de riesgos. Esto conlleva un pequeño esfuerzo adicional para realizar un cálculo en paralelo como el nuestro. El tutorial, [Ejecución de una simulación de R en paralelo con Azure Batch](https://docs.microsoft.com/en-us/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely), brinda información detallada sobre cómo conectar R a Azure Batch. A continuación, se muestra el código y un resumen del proceso para conectar a Azure Batch y cómo aprovechar la extensión a la nube en un cálculo de PFE simplificado.

En este ejemplo se aborda el mismo modelo ya descrito. Tal como vimos anteriormente, este cálculo se puede ejecutar en un equipo. Los aumentos en el número de rutas de Montecarlo o el uso de períodos más pequeños generará tiempos de ejecución mucho más largos. Casi todo el código de R seguirá sin cambios. En esta sección se resaltarán las diferencias.

Cada ruta de la simulación de Montecarlo se ejecuta en Azure. Esto se puede hacer porque cada ruta es independiente de las demás, lo que brinda un cálculo "vergonzosamente en paralelo".

Para usar Azure Batch, se define el clúster subyacente y se hace referencia a él en el código antes de poder usar el clúster en los cálculos. Para ejecutar los cálculos, se usa la siguiente definición de cluster.json:

````JavaScript
{
  "name": "myMCPool",
  "vmSize": "Standard_D2_v2",
  "maxTasksPerNode": 4,
  "poolSize": {
    "dedicatedNodes": {
      "min": 1,
      "max": 1
    },
    "lowPriorityNodes": {
      "min": 3,
      "max": 3
    },
    "autoscaleFormula": "QUEUE"
  },
  "containerImage": "rocker/tidyverse:latest",
  "rPackages": {
    "cran": [],
    "github": [],
    "bioconductor": []
  },
  "commandLine": [],
  "subnetId": ""
}
````

Con esta definición de clúster, el siguiente código de R usa el clúster:
````R
# Define the cloud burst environment
library(doAzureParallel)

# set your credentials
setCredentials("credentials.json")

# Create your cluster if not exist
cluster <- makeCluster("cluster.json")

# register your parallel backend
registerDoAzureParallel(cluster)

# check that your workers are up
getDoParWorkers()
````

Por último, se actualiza la instrucción foreach anterior para que use el paquete doAzureParallel. Es un cambio menor en el que se agrega una referencia al paquete sde y se cambia %do% a %dopar%:

````R
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package and extend the computation to the cloud
exposure_mc <- foreach(i = 1:nt, .combine = rbind, .packages = 'sde') %dopar% GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()
````

Cada simulación de Montecarlo se envía como tarea a Azure Batch. La tarea se ejecuta en la nube. Los resultados se combinan antes de enviarlos de vuelta al área de trabajo del analista. Los cálculos y el trabajo pesado se ejecutan en la nube para aprovechar completamente el escalado y la infraestructura subyacente que se requieren para los cálculos solicitados.

Una vez que se terminan los cálculos, los recursos adicionales se pueden apagar fácilmente si se invoca la siguiente instrucción única:

````R
# Stop the Cloud cluster
stopCluster(cluster)
````


## <a name="use-a-saas-offering"></a>Uso de una oferta de SaaS

En los dos primeros ejemplos se muestra cómo usar la infraestructura local y en la nube para desarrollar un modelo de valoración apropiado. Este paradigma ha empezado a cambiar. Del mismo modo en que la infraestructura local se ha transformado en servicios de PaaS e IaaS basados en la nube, el modelado de las cifras de riesgos importantes se transforma en un proceso orientado al servicio.
Actualmente, los analistas enfrentan dos desafíos importantes:

1.  Los requisitos reglamentarios usan una mayor capacidad de proceso para agregar a los requisitos de modelado. Los reguladores piden cifras de riesgos más frecuentes y actualizadas.

2.  La infraestructura de riesgo existente ha crecido de manera orgánica con el tiempo y crea desafíos al implementar requisitos nuevos y modelado de riesgos más avanzado de manera ágil.

Los servicios basados en la nube pueden brindar la funcionalidad necesaria y admiten el análisis de riesgos. Este enfoque presenta algunas ventajas:

-   Los cálculos de riesgos más comunes que el regulador requiere los debe implementar cualquier usuario sujeto a la reglamentación. Al usar servicios de un proveedor de servicios especializado, el analista se beneficia de los cálculos de riesgos listos para usar y que cumplen con las reglamentaciones. Dichos servicios pueden incluir los cálculos de riesgo de mercado, los cálculos de riesgo de contraparte, el ajuste del valor X (XVA) e, incluso, los cálculos de la Revisión fundamental de la carrera de negociación (FRTB).

-   Estos servicios exponen sus interfaces a través de los servicios web. Estos otros servicios pueden ampliar la infraestructura de riesgo existente.

En el ejemplo, queremos invocar un servicio basado en la nube para realizar los cálculos de FRTB. Varios de estos se pueden encontrar en [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely). Para este artículo, elegimos una opción de evaluación de [Vector Risk](http://www.vectorrisk.com/). Seguiremos modificando el sistema. Esta vez, usaremos un servicio para calcular la cifra de riesgo de interés. Este proceso consta de los pasos siguientes:

1.  Llamar al servicio de riesgo pertinente con los parámetros adecuados.

2.  Esperar hasta que el servicio termine el cálculo.

3.  Recuperar e incorporar los resultados en el análisis de riesgos.

Traducido al código de R, el código de R se puede mejorar con la definición de los valores de entrada necesarios a partir de una plantilla de entrada preparada.

````R
Template <- readLines('RequiredInputData.json')
data <- list(
# drilldown setup
  timeSteps = seq(0, n, by = 1),
  paths = as.integer(seq(0, nt, length.out = min(nt, 100))),
# calc setup
  calcDate = instrument.startDate,
  npaths = nt,
  price = P0,
  vol = sigma,
  drift = mu,
  premium = instrument.premium,
  maturityDate = today()
  )
body <- whisker.render(template, data)
````


A continuación, necesitamos llamar al servicio web. En este caso, llamamos al método StartCreditExposure para desencadenar el cálculo. El punto de conexión de la API se almacena en una variable denominada *endpoint*.

````R
# make the call
result <- POST( paste(endpoint, "StartCreditExposure", sep = ""),  
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
                body = body, encode = "raw"
               )

result <- content(result)
````

Una vez que finalizan los cálculos, se recuperan los resultados.

````R
# get back high level results
result <- POST( paste(endpoint, "GetCreditExposureResults", sep = ""), 
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
               body = sprintf('{"getCreditExposureResults": {"token":"DataSource=Production;Organisation=Microsoft", "ticket": "%s"}}', ticket), encode = "raw")

result <- content(result)
````

Esto permite que el analista continúa con los resultados que recibió. Las cifras de riesgo pertinentes de interés se extraen de los resultados y se trazan.

````R
if (!is.null(result$error)) {
    cat(result$error$message)
} else {
    # plot PFE
    result <- result$getCreditExposureResultsResponse$getCreditExposureResultsResult
    df <- do.call(rbind, result$exposures)
    df <- as.data.frame(df)
    df <- subset(df, term <= n)   
}

plot(as.numeric(df$term[df$statistic == 'PFE']) / 365, df$result[df$statistic == 'PFE'], type = "p", xlab = ("time t in Years"), ylab = ("Potential Future Exposure in USD"), ylim = range(c(df$result[df$statistic == 'PFE'], df$result[df$statistic == 'PFE'])), col = "red")
````


Los trazados resultantes tienen este aspecto:

|       |     |
|----    |---- |
| <img src="./assets/fsi-risk-modeling/image6.png" width="400px" alt="Figure 4 - Credit Exposure for MSFT Equity Forward - Calculated with a Cloud Based Risk Engine"/> | <img src="./assets/fsi-risk-modeling/image7.png" width="400px" alt="Figure 5 - Potential Future Exposure for MSFT Equity Forward - Calculated with a Cloud  Based Risk Engine" /> |
| Ilustración 4: Riesgo crediticio del contrato de participación a futuro de MSFT, <br/>que se calcula con un motor de riesgos basado en la nube | Ilustración 5: Posible exposición futura del contrato de participación a futuro de MSFT, <br/> que se calcula con un motor de riesgos basado en la nube |



## <a name="next-steps"></a>Pasos siguientes

El acceso flexible a la nube a través de una infraestructura de proceso y los servicios de análisis de riesgos basados en SaaS pueden ofrecer mejoras en términos de velocidad y agilidad para los analistas de riesgos que trabajan en seguros y mercados de capitales. En este artículo trabajamos con un ejemplo en el que se ilustra cómo usar Azure y otros servicios con las herramientas que los analistas de riesgos conocen. Pruebe aprovechar las funcionalidades de Azure cuando crea y mejora los modelos de riesgo.

### <a name="tutorials"></a>Tutoriales


-   Desarrolladores de R: [Ejecución de una simulación de R en paralelo con Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)

-   [Basic R commands and RevoScaleR functions: 25 common examples](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely) (Comandos básicos de R y funciones de RevoScaleR: 25 ejemplos comunes)

-   [Visualize and analyze data using RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely) (Visualización y análisis de datos mediante RevoScaleR)

-   [Introducción a las funcionalidades de ML Services y R de código abierto en HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview?WT.mc_id=fsiriskmodelr-docs-scseely)

_Artículo escrito por el Dr. Darko Mocelj y Rupert Nicolay._