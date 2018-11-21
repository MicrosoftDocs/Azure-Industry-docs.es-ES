# <a name="enabling-the-financial-services-risk-lifecycle-with-azure-and-r"></a><span data-ttu-id="542f5-101">Habilitación del ciclo de vida de riesgos de los servicios financieros con Azure y R</span><span class="sxs-lookup"><span data-stu-id="542f5-101">Enabling the financial services risk lifecycle with Azure and R</span></span>


<span data-ttu-id="542f5-102">Los cálculos de riesgos son fundamentales en varias etapas del ciclo de vida de operaciones de servicio financieros clave.</span><span class="sxs-lookup"><span data-stu-id="542f5-102">Risk calculations are pivotal at several stages in the lifecycle of key financial services operations.</span></span> <span data-ttu-id="542f5-103">Por ejemplo, una forma simplificada del ciclo de vida de la administración de productos de seguros puede tener un aspecto similar al diagrama siguiente.</span><span class="sxs-lookup"><span data-stu-id="542f5-103">For example, a simplified form of the insurance product management lifecycle might look something like the diagram below.</span></span> <span data-ttu-id="542f5-104">Los aspectos del cálculo de riesgos se muestran con el texto azul.</span><span class="sxs-lookup"><span data-stu-id="542f5-104">The risk calculation aspects are shown in blue text.</span></span>

![](./assets/fsi-risk-modeling/image1.png)

<span data-ttu-id="542f5-105">Un escenario en una empresa dedicada a los mercados de capitales puede tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="542f5-105">A scenario in a capital markets firm might look like this:</span></span>

![](./assets/fsi-risk-modeling/image2.png)

<span data-ttu-id="542f5-106">A través de estos procesos, hay necesidades comunes en torno al modelado de riesgo que incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="542f5-106">Through these processes there are common needs around risk modelling including:</span></span>

1.  <span data-ttu-id="542f5-107">La necesidad de una experimentación ad hoc relacionada con los riesgos por parte de los analistas de riesgos, los actuarios de una entidad aseguradora o los analistas cuantitativos de una empresa dedicada a los mercados de capitales.</span><span class="sxs-lookup"><span data-stu-id="542f5-107">The need for ad-hoc risk-related experimentation by risk analysts; actuaries in an insurance firm or quants in a capital markets firm.</span></span>
    <span data-ttu-id="542f5-108">Habitualmente, estos analistas trabajan con herramientas de modelado y código populares en sus dominios: R y Python.</span><span class="sxs-lookup"><span data-stu-id="542f5-108">These analysts typically work with code and modelling tools popular in their domain: R and Python.</span></span> <span data-ttu-id="542f5-109">Muchos programas de estudios universitarios incluyen cursos de R o Python relacionados con las finanzas matemáticas y en cursos de MBA.</span><span class="sxs-lookup"><span data-stu-id="542f5-109">Many university curriculums include training in R or Python in mathematical finance and in MBA courses.</span></span>
    <span data-ttu-id="542f5-110">Ambos lenguajes ofrecen una amplia variedad de bibliotecas de código abierto que admiten los cálculos de riesgos populares.</span><span class="sxs-lookup"><span data-stu-id="542f5-110">Both languages offer a wide range of open source libraries which support popular risk calculations.</span></span> <span data-ttu-id="542f5-111">Junto con las herramientas adecuadas, los analistas a menudo necesitan acceso a:</span><span class="sxs-lookup"><span data-stu-id="542f5-111">Along with appropriate tooling, analysts often require access to:</span></span>

    <span data-ttu-id="542f5-112"> a.</span><span class="sxs-lookup"><span data-stu-id="542f5-112">a.</span></span>  <span data-ttu-id="542f5-113">Datos precisos sobre los precios de mercado.</span><span class="sxs-lookup"><span data-stu-id="542f5-113">Accurate market pricing data.</span></span>

    <span data-ttu-id="542f5-114">b.</span><span class="sxs-lookup"><span data-stu-id="542f5-114">b.</span></span>  <span data-ttu-id="542f5-115">Datos de reclamaciones y directivas existentes.</span><span class="sxs-lookup"><span data-stu-id="542f5-115">Existing policy and claims data.</span></span>

    <span data-ttu-id="542f5-116">c.</span><span class="sxs-lookup"><span data-stu-id="542f5-116">c.</span></span>  <span data-ttu-id="542f5-117">Datos existentes de la posición de mercado.</span><span class="sxs-lookup"><span data-stu-id="542f5-117">Existing market position data.</span></span>

    <span data-ttu-id="542f5-118">d.</span><span class="sxs-lookup"><span data-stu-id="542f5-118">d.</span></span>  <span data-ttu-id="542f5-119">Otros datos externos.</span><span class="sxs-lookup"><span data-stu-id="542f5-119">Other external data.</span></span> <span data-ttu-id="542f5-120">Los orígenes incluyen datos estructurados, como tablas de mortalidad y datos de precios de la competencia.</span><span class="sxs-lookup"><span data-stu-id="542f5-120">Sources include structured data such as mortality tables and competitive pricing data.</span></span> <span data-ttu-id="542f5-121">También se pueden usar orígenes menos tradicionales, como el clima, las noticias, etc.</span><span class="sxs-lookup"><span data-stu-id="542f5-121">Less traditional sources such as weather, news and others may also be used.</span></span>

    <span data-ttu-id="542f5-122">e.</span><span class="sxs-lookup"><span data-stu-id="542f5-122">e.</span></span>  <span data-ttu-id="542f5-123">Capacidad informática para permitir investigaciones rápidas de datos interactivos.</span><span class="sxs-lookup"><span data-stu-id="542f5-123">Computational capacity to enable quick interactive data investigations.</span></span>

2.  <span data-ttu-id="542f5-124">También pueden usar algoritmos de aprendizaje automático ad hoc para fijar los precios o determinar la estrategia de mercado.</span><span class="sxs-lookup"><span data-stu-id="542f5-124">They may also make use of ad-hoc machine learning algorithms for pricing or determining market strategy.</span></span>

3.  <span data-ttu-id="542f5-125">La necesidad de visualizar y presentar datos para usarlos en el planeamiento de productos, la estrategia comercial y análisis similares.</span><span class="sxs-lookup"><span data-stu-id="542f5-125">The need to visualize and present data for use in product planning, trading strategy, and similar discussions.</span></span>

4.  <span data-ttu-id="542f5-126">La ejecución rápida de los modelos definidos, configurados por los analistas, para precios, valoraciones y riesgo de mercado.</span><span class="sxs-lookup"><span data-stu-id="542f5-126">The rapid execution of defined models, configured by the analysts, for pricing, valuations, and market risk.</span></span> <span data-ttu-id="542f5-127">Las valoraciones usan una combinación de modelado de riesgos dedicada, herramientas de riesgo de mercado y código personalizado.</span><span class="sxs-lookup"><span data-stu-id="542f5-127">The valuations use a combination of dedicated risk modelling, market risk tools, and custom code.</span></span> <span data-ttu-id="542f5-128">El análisis se ejecuta en un lote con diversos cálculos nocturnos, semanales, mensuales, trimestrales y anuales que generan picos en las cargas de trabajo.</span><span class="sxs-lookup"><span data-stu-id="542f5-128">The analysis is executed in a batch with varying nightly, weekly, monthly, quarterly, and annual calculations generating spikes in workloads.</span></span>

5.  <span data-ttu-id="542f5-129">La integración de los datos con otras medidas de riesgo empresarial para informes de riesgo consolidados.</span><span class="sxs-lookup"><span data-stu-id="542f5-129">The integration of data with other enterprise wide risk measures for consolidated risk reporting.</span></span> <span data-ttu-id="542f5-130">En las organizaciones más grandes, las estimaciones de riesgo de menor nivel se pueden transferir a una herramienta de informes y modelado de riesgos empresariales.</span><span class="sxs-lookup"><span data-stu-id="542f5-130">In larger organizations lower level risk estimates may be transferred to an enterprise risk modelling and reporting tool.</span></span>

6.  <span data-ttu-id="542f5-131">Los resultados se deben informar en un formato definido en el intervalo necesario para satisfacer los requisitos de inversores y los requisitos reglamentarios.</span><span class="sxs-lookup"><span data-stu-id="542f5-131">Results must be reported in a defined format at the required interval to meet investor and regulatory requirements.</span></span>

<span data-ttu-id="542f5-132">Microsoft respalda las preocupaciones mencionadas a través de una combinación de servicios de Azure y de ofertas de asociados en [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="542f5-132">Microsoft supports the above concerns through a combination of Azure services and partner offerings in the [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span> <span data-ttu-id="542f5-133">En este artículo, mostramos ejemplos prácticos de cómo realizar una experimentación ad hoc mediante R. Para empezar, explicaremos cómo ejecutar el experimento en una sola máquina y, luego, se mostrará cómo ejecutar el mismo experimento en [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely) y cerraremos mostrando cómo aprovechar los servicios externos en nuestro modelado.</span><span class="sxs-lookup"><span data-stu-id="542f5-133">In this article, we show practical examples of how to perform ad-hoc experimentation using R. We begin by explaining how to run the experiment on a single machine, then show how to run the same experiment on [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely), and close by showing how to take advantage of external services in our modelling.</span></span> <span data-ttu-id="542f5-134">Las opciones y consideraciones para la ejecución de modelos definidos en Azure se describen en estos artículos centrados en la [banca](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) y los [seguros](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="542f5-134">Options and considerations for the execution of defined models on Azure are described in these articles focused on [banking](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) and [insurance](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span>

## <a name="analyst-modelling-in-r"></a><span data-ttu-id="542f5-135">Modelado de analistas en R</span><span class="sxs-lookup"><span data-stu-id="542f5-135">Analyst Modelling in R</span></span> 

<span data-ttu-id="542f5-136">Para empezar, examinemos cómo un analista puede usar R en un escenario de mercados de capitales simplificado y representativo.</span><span class="sxs-lookup"><span data-stu-id="542f5-136">Let's start by looking at how R may be used by an analyst in a simplified, representative capital markets scenario.</span></span> <span data-ttu-id="542f5-137">Para crearlo, haga referencia a una biblioteca existente de R para el cálculo o escriba código desde cero.</span><span class="sxs-lookup"><span data-stu-id="542f5-137">You can build this either by referencing an existing R library for the calculation or by writing code from scratch.</span></span> <span data-ttu-id="542f5-138">En el ejemplo, también debemos capturar datos externos relacionados con los precios.</span><span class="sxs-lookup"><span data-stu-id="542f5-138">In our example, we also must fetch external pricing data.</span></span> <span data-ttu-id="542f5-139">Para que este ejemplo sea simple, pero ilustrativo, calculamos la posible exposición futura (PFE) de un contrato a plazo de índices bursátiles.</span><span class="sxs-lookup"><span data-stu-id="542f5-139">To keep the example simple but illustrative, we calculate the potential future exposure (PFE) of an equity stock forward contract.</span></span>
<span data-ttu-id="542f5-140">En este ejemplo se evitan las técnicas de modelado cuantitativo complejas para instrumentos como derivados complejos y se centra en un factor de riesgo único para concentrarse en el ciclo de vida del riesgo.</span><span class="sxs-lookup"><span data-stu-id="542f5-140">This example avoids complex quantitative modelling techniques for instruments like complex derivatives and focuses on a single risk factor to concentrate on the risk life cycle.</span></span> <span data-ttu-id="542f5-141">En el ejemplo se hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="542f5-141">Our example does the following:</span></span>

1.  <span data-ttu-id="542f5-142">Seleccionar un instrumento de interés.</span><span class="sxs-lookup"><span data-stu-id="542f5-142">Select an instrument of interest.</span></span>

2.  <span data-ttu-id="542f5-143">Obtener los precios históricos para el instrumento.</span><span class="sxs-lookup"><span data-stu-id="542f5-143">Source historic prices for the instrument.</span></span>

3.  <span data-ttu-id="542f5-144">Modelar el precio de las acciones mediante un cálculo de Montecarlo (MC) sencillo, que usa el movimiento browniano geométrico (GBM):</span><span class="sxs-lookup"><span data-stu-id="542f5-144">Model equity price by simple Monte Carlo (MC) calculation, which uses Geometric Brownian Motion (GBM):</span></span>

    <span data-ttu-id="542f5-145"> a.</span><span class="sxs-lookup"><span data-stu-id="542f5-145">a.</span></span>  <span data-ttu-id="542f5-146">Calcular la devolución μ (mu) y la volatilidad σ (theta) esperadas.</span><span class="sxs-lookup"><span data-stu-id="542f5-146">Estimate expected return μ (mu) and volatility σ (theta).</span></span>

    <span data-ttu-id="542f5-147">b.</span><span class="sxs-lookup"><span data-stu-id="542f5-147">b.</span></span>  <span data-ttu-id="542f5-148">Calibrar el modelo según los datos históricos.</span><span class="sxs-lookup"><span data-stu-id="542f5-148">Calibrate the model to historic data.</span></span>

4.  <span data-ttu-id="542f5-149">Visualizar las diversas rutas para comunicar los resultados.</span><span class="sxs-lookup"><span data-stu-id="542f5-149">Visualize the various paths to communicate the results.</span></span>

5.  <span data-ttu-id="542f5-150">Trazar max(0,Stock Value) para demostrar el significado de PFE, la diferencia respecto del valor en riesgo (VaR).</span><span class="sxs-lookup"><span data-stu-id="542f5-150">Plot max(0,Stock Value) to demonstrate the meaning of PFE, the difference to Value at Risk (VaR)</span></span>

    <span data-ttu-id="542f5-151"> a.</span><span class="sxs-lookup"><span data-stu-id="542f5-151">a.</span></span>  <span data-ttu-id="542f5-152">Para aclarar: PFE = precio por acción (T) -- precio K del contrato a plazo</span><span class="sxs-lookup"><span data-stu-id="542f5-152">To clarify: PFE = Share Price (T) -- Forward Contract Price K</span></span>

6.  <span data-ttu-id="542f5-153">Tomar el centil 0,95 para obtener el valor de PFE en cada período o al final del período de simulación</span><span class="sxs-lookup"><span data-stu-id="542f5-153">Take the 0.95 Quantile to get the PFE value at each time step / end of simulation period</span></span>

<span data-ttu-id="542f5-154">Se calculará la posible exposición futura de un contrato de participación a futuro en función de las acciones de MSFT.</span><span class="sxs-lookup"><span data-stu-id="542f5-154">We will calculate the potential future exposure for an equity forward based on MSFT stock.</span></span> <span data-ttu-id="542f5-155">Como ya se mencionó, para modelar los precios de las acciones, se requieren los precios históricos de las acciones de MSFT para poder calibrar el modelo según los datos históricos.</span><span class="sxs-lookup"><span data-stu-id="542f5-155">As mentioned above, to model the stock prices, historic prices for the MSFT stock are required so we can calibrate the model to historical data.</span></span> <span data-ttu-id="542f5-156">Hay muchas maneras de adquirir los precios históricos de las acciones.</span><span class="sxs-lookup"><span data-stu-id="542f5-156">There are many ways to acquire historical stock prices.</span></span> <span data-ttu-id="542f5-157">En el ejemplo, usamos una versión gratuita de un servicio de cotización bursátil de un proveedor de servicios externo, [Quandl](https://www.quandl.com/).</span><span class="sxs-lookup"><span data-stu-id="542f5-157">In our example, we use a free version of a stock price service from an external service provider, [Quandl](https://www.quandl.com/).</span></span>


> <span data-ttu-id="542f5-158">Nota: En el ejemplo se usa el [conjunto de datos WIKI Prices](https://www.quandl.com/databases/WIKIP), que se puede usar para aprender los conceptos.</span><span class="sxs-lookup"><span data-stu-id="542f5-158">Note: The example uses the [WIKI Prices dataset](https://www.quandl.com/databases/WIKIP) which can be used for learning concepts.</span></span> <span data-ttu-id="542f5-159">Para el uso en producción de capitales de los EE. UU., Quandl recomienda usar el [conjunto de datos End of Day US Stock Prices](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).</span><span class="sxs-lookup"><span data-stu-id="542f5-159">For production usage of US based equities, Quandl recommends using the [End of Day US Stock Prices dataset](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices).</span></span>

<span data-ttu-id="542f5-160">Para procesar los datos y definir el riesgo asociado con el capital, es necesario hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="542f5-160">To process the data and define the risk associated with the equity, we need to do the following things:</span></span>

1.  <span data-ttu-id="542f5-161">Recuperar datos históricos relacionados con el capital.</span><span class="sxs-lookup"><span data-stu-id="542f5-161">Retrieve history data from the equity.</span></span>

2.  <span data-ttu-id="542f5-162">Determinar la devolución μ y la volatilidad σ esperadas a partir de los datos históricos.</span><span class="sxs-lookup"><span data-stu-id="542f5-162">Determine the expected return μ and volatility σ from the historic data.</span></span>

3.  <span data-ttu-id="542f5-163">Modelar los precios de acciones subyacentes mediante el uso de alguna simulación.</span><span class="sxs-lookup"><span data-stu-id="542f5-163">Model the underlying stock prices using some simulation.</span></span>

4.  <span data-ttu-id="542f5-164">Ejecutar el modelo.</span><span class="sxs-lookup"><span data-stu-id="542f5-164">Run the model</span></span>

5.  <span data-ttu-id="542f5-165">Determinar la exposición del capital en el futuro.</span><span class="sxs-lookup"><span data-stu-id="542f5-165">Determine the exposure of the equity in the future.</span></span>

<span data-ttu-id="542f5-166">Para empezar, recuperamos las acciones a partir del servicio de Quandl y trazamos el historial de cotizaciones de cierre de los últimos 180 días.</span><span class="sxs-lookup"><span data-stu-id="542f5-166">We start by retrieving the stock from the Quandl service and plotting the closing price history over the last 180 days.</span></span>

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

<span data-ttu-id="542f5-167">Con datos en mano, calibramos el modelo de GBM.</span><span class="sxs-lookup"><span data-stu-id="542f5-167">With the data in hand, we calibrate the GBM model.</span></span>

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

<span data-ttu-id="542f5-168">A continuación, modelamos los precios de acciones subyacentes.</span><span class="sxs-lookup"><span data-stu-id="542f5-168">Next, we model the underlying stock prices.</span></span> <span data-ttu-id="542f5-169">Podemos implementar el proceso discreto de GBM desde cero o utilizar uno de los muchos paquetes de R que brindan esta funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="542f5-169">We can either implement the discrete GBM process from scratch or utilize one of many R packages which provide this functionality.</span></span> <span data-ttu-id="542f5-170">Usamos el paquete [*sde* (simulación e inferencia de ecuaciones diferenciales estocásticas)](https://cran.r-project.org/web/packages/sde/index.html) de R, que proporciona un método para resolver este problema.</span><span class="sxs-lookup"><span data-stu-id="542f5-170">We use the R package [*sde* (Simulation and Inference for Stochastic Differential Equations)](https://cran.r-project.org/web/packages/sde/index.html) which provides a method of solving this problem.</span></span> <span data-ttu-id="542f5-171">El método de GBM requiere un conjunto de parámetros que se calibran según los datos históricos o que se dan como parámetros de simulación.</span><span class="sxs-lookup"><span data-stu-id="542f5-171">The GBM method requires a set of parameters which are either calibrated to historic data or given as simulation parameters.</span></span> <span data-ttu-id="542f5-172">Usamos los datos históricos, brindando los valores de μ, σ y los precios de las acciones al comienzo de la simulación (P0).</span><span class="sxs-lookup"><span data-stu-id="542f5-172">We use the historic data, providing μ, σ and the stock prices at the beginning of the simulation (P0).</span></span>

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

<span data-ttu-id="542f5-173">Ahora estamos preparados para iniciar una simulación de Montecarlo para modelar la posible exposición futura de algún número de rutas de simulación.</span><span class="sxs-lookup"><span data-stu-id="542f5-173">We are now ready to start a Monte Carlo simulation to model the potential exposure for some number of simulation paths.</span></span> <span data-ttu-id="542f5-174">Limitaremos la simulación a 50 rutas de Montecarlo y 256 períodos.</span><span class="sxs-lookup"><span data-stu-id="542f5-174">We will limit the simulation to 50 Monte Carlo paths and 256 time steps.</span></span> <span data-ttu-id="542f5-175">Para preparar el escalado horizontal de la simulación y aprovechar la paralelización en R, el bucle de simulación de Montecarlo usa una instrucción foreach.</span><span class="sxs-lookup"><span data-stu-id="542f5-175">In preparation for scaling out the simulation and taking advantage of parallelization in R, the Monte Carlo simulation loop uses a foreach statement.</span></span>

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

<span data-ttu-id="542f5-176">Ahora hemos simulado el precio de la acción de MSFT subyacente.</span><span class="sxs-lookup"><span data-stu-id="542f5-176">We have now simulated the price of the underlying MSFT stock.</span></span> <span data-ttu-id="542f5-177">Para calcular la exposición del contrato de participación a futuro, restamos la prima y limitamos la exposición solo a los valores positivos.</span><span class="sxs-lookup"><span data-stu-id="542f5-177">To calculate the exposure of the equity forward, we subtract the premium and limit the exposure to only positive values.</span></span>

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

<span data-ttu-id="542f5-178">Las dos imágenes siguientes muestran el resultado de la simulación.</span><span class="sxs-lookup"><span data-stu-id="542f5-178">The next two pictures show the result of the simulation.</span></span> <span data-ttu-id="542f5-179">La primera imagen muestra la simulación de Montecarlo del precio de acción subyacente para 50 rutas.</span><span class="sxs-lookup"><span data-stu-id="542f5-179">The first picture shows the Monte Carlo simulation of the underlying stock price for 50 paths.</span></span> <span data-ttu-id="542f5-180">En la segunda imagen, se muestra el riesgo crediticio subyacente del contrato de participación a futuro después de restar la prima del contrato en cuestión y limitar la exposición a los valores positivos.</span><span class="sxs-lookup"><span data-stu-id="542f5-180">The second picture illustrates the underlying Credit Exposure for the equity forward after subtracting the premium of the equity forward and limiting the exposure to positive values.</span></span>


|       |    |
|---    |--- |
| <img src="./assets/fsi-risk-modeling/image3.png" width="400px" alt="Figure 1 - 50 Monte Carlo Paths"/> | <img src="./assets/fsi-risk-modeling/image4.png" width="400px" alt="Figure 2 - Credit Exposure for Equity Forward"/> |
| <span data-ttu-id="542f5-181">Ilustración 1: 50 rutas de Montecarlo</span><span class="sxs-lookup"><span data-stu-id="542f5-181">Figure 1 - 50 Monte Carlo Paths</span></span> | <span data-ttu-id="542f5-182">Ilustración 2: Riesgo crediticio del contrato de participación a futuro</span><span class="sxs-lookup"><span data-stu-id="542f5-182">Figure 2 - Credit Exposure for Equity Forward</span></span> |


<span data-ttu-id="542f5-183">En el último paso, el PFE del centil 0,95 de un mes se calcula mediante el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="542f5-183">In the last step, the 1-Month 0.95 Quantile PFE is calculated by the following code.</span></span>

````R
# Calculate the PFE at each time step
df_pfe <- cbind(t,apply(pfe_mc,2,quantile,probs = instrument.pfeQuantile ))

resulting in the final PFE plot
plot(df_pfe, t = 'l', ylab = "Potential Future Exposure in USD", xlab = "time t in Years")
````

<img src="./assets/fsi-risk-modeling/image5.png" width="500px" alt="Potential Future Exposure for MSFT Equity Forward" /> 

<span data-ttu-id="542f5-184">Ilustración 3: Posible exposición futura del contrato de participación a futuro de MSFT</span><span class="sxs-lookup"><span data-stu-id="542f5-184">Figure 3 Potential Future Exposure for MSFT Equity Forward</span></span>

## <a name="using-azure-batch-with-r"></a><span data-ttu-id="542f5-185">Uso de Azure Batch con R</span><span class="sxs-lookup"><span data-stu-id="542f5-185">Using Azure Batch with R</span></span> 

<span data-ttu-id="542f5-186">La solución de R descrita anteriormente se puede conectar a Azure Batch y puede aprovechar la nube para realizar los cálculos de riesgos.</span><span class="sxs-lookup"><span data-stu-id="542f5-186">The R solution described above can be connected to Azure Batch and leverage the cloud for risk calculations.</span></span> <span data-ttu-id="542f5-187">Esto conlleva un pequeño esfuerzo adicional para realizar un cálculo en paralelo como el nuestro.</span><span class="sxs-lookup"><span data-stu-id="542f5-187">This takes little extra effort for a parallel calculation such as ours.</span></span> <span data-ttu-id="542f5-188">El tutorial, [Ejecución de una simulación de R en paralelo con Azure Batch](https://docs.microsoft.com/en-us/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely), brinda información detallada sobre cómo conectar R a Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="542f5-188">The tutorial, [Run a parallel R simulation with Azure Batch](https://docs.microsoft.com/en-us/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely), provides detailed information on connecting R to Azure Batch.</span></span> <span data-ttu-id="542f5-189">A continuación, se muestra el código y un resumen del proceso para conectar a Azure Batch y cómo aprovechar la extensión a la nube en un cálculo de PFE simplificado.</span><span class="sxs-lookup"><span data-stu-id="542f5-189">Below we show the code and summary of the process to connect to Azure Batch and how to take advantage of the extension to the cloud in a simplified PFE calculation.</span></span>

<span data-ttu-id="542f5-190">En este ejemplo se aborda el mismo modelo ya descrito.</span><span class="sxs-lookup"><span data-stu-id="542f5-190">This example tackles the same model described earlier.</span></span> <span data-ttu-id="542f5-191">Tal como vimos anteriormente, este cálculo se puede ejecutar en un equipo.</span><span class="sxs-lookup"><span data-stu-id="542f5-191">As we have seen before, this calculation can run on our personal computer.</span></span> <span data-ttu-id="542f5-192">Los aumentos en el número de rutas de Montecarlo o el uso de períodos más pequeños generará tiempos de ejecución mucho más largos.</span><span class="sxs-lookup"><span data-stu-id="542f5-192">Increases to the number of Monte Carlo paths or use of smaller time steps will result in much longer execution times.</span></span> <span data-ttu-id="542f5-193">Casi todo el código de R seguirá sin cambios.</span><span class="sxs-lookup"><span data-stu-id="542f5-193">Almost all of the R code will remain unchanged.</span></span> <span data-ttu-id="542f5-194">En esta sección se resaltarán las diferencias.</span><span class="sxs-lookup"><span data-stu-id="542f5-194">We will highlight the differences in this section.</span></span>

<span data-ttu-id="542f5-195">Cada ruta de la simulación de Montecarlo se ejecuta en Azure.</span><span class="sxs-lookup"><span data-stu-id="542f5-195">Each path of the Monte Carlo simulation runs in Azure.</span></span> <span data-ttu-id="542f5-196">Esto se puede hacer porque cada ruta es independiente de las demás, lo que brinda un cálculo "vergonzosamente en paralelo".</span><span class="sxs-lookup"><span data-stu-id="542f5-196">We can do this because each path is independent of the others, giving us an "embarrassingly parallel" calculation.</span></span>

<span data-ttu-id="542f5-197">Para usar Azure Batch, se define el clúster subyacente y se hace referencia a él en el código antes de poder usar el clúster en los cálculos.</span><span class="sxs-lookup"><span data-stu-id="542f5-197">To use Azure Batch, we define the underlying cluster and reference it in the code before the cluster can be used in the calculations.</span></span> <span data-ttu-id="542f5-198">Para ejecutar los cálculos, se usa la siguiente definición de cluster.json:</span><span class="sxs-lookup"><span data-stu-id="542f5-198">To run the calculations, we use the following cluster.json definition:</span></span>

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

<span data-ttu-id="542f5-199">Con esta definición de clúster, el siguiente código de R usa el clúster:</span><span class="sxs-lookup"><span data-stu-id="542f5-199">With this cluster definition, the following R code makes use of the cluster:</span></span>
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

<span data-ttu-id="542f5-200">Por último, se actualiza la instrucción foreach anterior para que use el paquete doAzureParallel.</span><span class="sxs-lookup"><span data-stu-id="542f5-200">Finally, we update the foreach statement from earlier to use the doAzureParallel package.</span></span> <span data-ttu-id="542f5-201">Es un cambio menor en el que se agrega una referencia al paquete sde y se cambia %do% a %dopar%:</span><span class="sxs-lookup"><span data-stu-id="542f5-201">It's a minor change, adding a reference to the sde package and changing the %do% to %dopar%:</span></span>

````R
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package and extend the computation to the cloud
exposure_mc <- foreach(i = 1:nt, .combine = rbind, .packages = 'sde') %dopar% GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()
````

<span data-ttu-id="542f5-202">Cada simulación de Montecarlo se envía como tarea a Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="542f5-202">Each Monte Carlo simulation is submitted as a task to Azure Batch.</span></span> <span data-ttu-id="542f5-203">La tarea se ejecuta en la nube.</span><span class="sxs-lookup"><span data-stu-id="542f5-203">The task executes in the cloud.</span></span> <span data-ttu-id="542f5-204">Los resultados se combinan antes de enviarlos de vuelta al área de trabajo del analista.</span><span class="sxs-lookup"><span data-stu-id="542f5-204">Results are merged before being sent back to the analyst workbench.</span></span> <span data-ttu-id="542f5-205">Los cálculos y el trabajo pesado se ejecutan en la nube para aprovechar completamente el escalado y la infraestructura subyacente que se requieren para los cálculos solicitados.</span><span class="sxs-lookup"><span data-stu-id="542f5-205">The heavy lifting and computations execute in the cloud to take full advantage of scaling and the underlying infrastructure required by the requested calculations.</span></span>

<span data-ttu-id="542f5-206">Una vez que se terminan los cálculos, los recursos adicionales se pueden apagar fácilmente si se invoca la siguiente instrucción única:</span><span class="sxs-lookup"><span data-stu-id="542f5-206">After the calculations have finished, the additional resources can easily be shut-down by invoking the following a single instruction:</span></span>

````R
# Stop the Cloud cluster
stopCluster(cluster)
````


## <a name="use-a-saas-offering"></a><span data-ttu-id="542f5-207">Uso de una oferta de SaaS</span><span class="sxs-lookup"><span data-stu-id="542f5-207">Use a SaaS offering</span></span>

<span data-ttu-id="542f5-208">En los dos primeros ejemplos se muestra cómo usar la infraestructura local y en la nube para desarrollar un modelo de valoración apropiado.</span><span class="sxs-lookup"><span data-stu-id="542f5-208">The first two examples show how to utilize local and cloud infrastructure for developing an adequate valuation model.</span></span> <span data-ttu-id="542f5-209">Este paradigma ha empezado a cambiar.</span><span class="sxs-lookup"><span data-stu-id="542f5-209">This paradigm has begun to shift.</span></span> <span data-ttu-id="542f5-210">Del mismo modo en que la infraestructura local se ha transformado en servicios de PaaS e IaaS basados en la nube, el modelado de las cifras de riesgos importantes se transforma en un proceso orientado al servicio.</span><span class="sxs-lookup"><span data-stu-id="542f5-210">In the same way that on-premises infrastructure has transformed into cloud-based IaaS and PaaS services, the modelling of relevant risk figures is transforming into a service-oriented process.</span></span>
<span data-ttu-id="542f5-211">Actualmente, los analistas enfrentan dos desafíos importantes:</span><span class="sxs-lookup"><span data-stu-id="542f5-211">Today's analysts face two major challenges:</span></span>

1.  <span data-ttu-id="542f5-212">Los requisitos reglamentarios usan una mayor capacidad de proceso para agregar a los requisitos de modelado.</span><span class="sxs-lookup"><span data-stu-id="542f5-212">The regulatory requirements use increasing compute capacity to add to modeling requirements.</span></span> <span data-ttu-id="542f5-213">Los reguladores piden cifras de riesgos más frecuentes y actualizadas.</span><span class="sxs-lookup"><span data-stu-id="542f5-213">The regulators are asking for more frequent and up-to date risk figures.</span></span>

2.  <span data-ttu-id="542f5-214">La infraestructura de riesgo existente ha crecido de manera orgánica con el tiempo y crea desafíos al implementar requisitos nuevos y modelado de riesgos más avanzado de manera ágil.</span><span class="sxs-lookup"><span data-stu-id="542f5-214">The existing risk infrastructure has grown organically with time and creates challenges when implementing new requirements and more advanced risk modeling in an agile manner.</span></span>

<span data-ttu-id="542f5-215">Los servicios basados en la nube pueden brindar la funcionalidad necesaria y admiten el análisis de riesgos.</span><span class="sxs-lookup"><span data-stu-id="542f5-215">Cloud-based services can deliver the required functionality and support risk analysis.</span></span> <span data-ttu-id="542f5-216">Este enfoque presenta algunas ventajas:</span><span class="sxs-lookup"><span data-stu-id="542f5-216">This approach has some advantages:</span></span>

-   <span data-ttu-id="542f5-217">Los cálculos de riesgos más comunes que el regulador requiere los debe implementar cualquier usuario sujeto a la reglamentación.</span><span class="sxs-lookup"><span data-stu-id="542f5-217">The most common risk calculations required by the regulator must be implemented by everyone under the regulation.</span></span> <span data-ttu-id="542f5-218">Al usar servicios de un proveedor de servicios especializado, el analista se beneficia de los cálculos de riesgos listos para usar y que cumplen con las reglamentaciones.</span><span class="sxs-lookup"><span data-stu-id="542f5-218">By utilizing services from a specialized service provider, the analyst benefits from ready to use, regulator-compliant risk calculations.</span></span> <span data-ttu-id="542f5-219">Dichos servicios pueden incluir los cálculos de riesgo de mercado, los cálculos de riesgo de contraparte, el ajuste del valor X (XVA) e, incluso, los cálculos de la Revisión fundamental de la carrera de negociación (FRTB).</span><span class="sxs-lookup"><span data-stu-id="542f5-219">Such services may include market risk calculations, counterparty risk calculations, X-Value Adjustment (XVA), and even Fundamental Review of Trading Book (FRTB) calculations.</span></span>

-   <span data-ttu-id="542f5-220">Estos servicios exponen sus interfaces a través de los servicios web.</span><span class="sxs-lookup"><span data-stu-id="542f5-220">These services expose their interfaces through web services.</span></span> <span data-ttu-id="542f5-221">Estos otros servicios pueden ampliar la infraestructura de riesgo existente.</span><span class="sxs-lookup"><span data-stu-id="542f5-221">The existing risk infrastructure can be enhanced by these other services.</span></span>

<span data-ttu-id="542f5-222">En el ejemplo, queremos invocar un servicio basado en la nube para realizar los cálculos de FRTB.</span><span class="sxs-lookup"><span data-stu-id="542f5-222">In our example, we want to invoke a cloud-based service for FRTB calculations.</span></span> <span data-ttu-id="542f5-223">Varios de estos se pueden encontrar en [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span><span class="sxs-lookup"><span data-stu-id="542f5-223">Several of these can be found on [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely).</span></span> <span data-ttu-id="542f5-224">Para este artículo, elegimos una opción de evaluación de [Vector Risk](http://www.vectorrisk.com/).</span><span class="sxs-lookup"><span data-stu-id="542f5-224">For this article we chose a trial option from [Vector Risk](http://www.vectorrisk.com/).</span></span> <span data-ttu-id="542f5-225">Seguiremos modificando el sistema.</span><span class="sxs-lookup"><span data-stu-id="542f5-225">We will continue to modify our system.</span></span> <span data-ttu-id="542f5-226">Esta vez, usaremos un servicio para calcular la cifra de riesgo de interés.</span><span class="sxs-lookup"><span data-stu-id="542f5-226">This time, we use a service to calculate the risk figure of interest.</span></span> <span data-ttu-id="542f5-227">Este proceso consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="542f5-227">This process consists of the following steps:</span></span>

1.  <span data-ttu-id="542f5-228">Llamar al servicio de riesgo pertinente con los parámetros adecuados.</span><span class="sxs-lookup"><span data-stu-id="542f5-228">Call the relevant risk service and with the right parameters.</span></span>

2.  <span data-ttu-id="542f5-229">Esperar hasta que el servicio termine el cálculo.</span><span class="sxs-lookup"><span data-stu-id="542f5-229">Wait until the service finishes the calculation.</span></span>

3.  <span data-ttu-id="542f5-230">Recuperar e incorporar los resultados en el análisis de riesgos.</span><span class="sxs-lookup"><span data-stu-id="542f5-230">Retrieve and incorporate the results into the risk analysis.</span></span>

<span data-ttu-id="542f5-231">Traducido al código de R, el código de R se puede mejorar con la definición de los valores de entrada necesarios a partir de una plantilla de entrada preparada.</span><span class="sxs-lookup"><span data-stu-id="542f5-231">Translated into R code, our R code can be enhanced by the definition of the required input values from a prepared input template.</span></span>

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


<span data-ttu-id="542f5-232">A continuación, necesitamos llamar al servicio web.</span><span class="sxs-lookup"><span data-stu-id="542f5-232">Next, we need to call the web service.</span></span> <span data-ttu-id="542f5-233">En este caso, llamamos al método StartCreditExposure para desencadenar el cálculo.</span><span class="sxs-lookup"><span data-stu-id="542f5-233">In this case, we call the StartCreditExposure method to trigger the calculation.</span></span> <span data-ttu-id="542f5-234">El punto de conexión de la API se almacena en una variable denominada *endpoint*.</span><span class="sxs-lookup"><span data-stu-id="542f5-234">We store the endpoint for the API in a variable named *endpoint*.</span></span>

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

<span data-ttu-id="542f5-235">Una vez que finalizan los cálculos, se recuperan los resultados.</span><span class="sxs-lookup"><span data-stu-id="542f5-235">Once the calculations have finished, we retrieve the results.</span></span>

````R
# get back high level results
result <- POST( paste(endpoint, "GetCreditExposureResults", sep = ""), 
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
               body = sprintf('{"getCreditExposureResults": {"token":"DataSource=Production;Organisation=Microsoft", "ticket": "%s"}}', ticket), encode = "raw")

result <- content(result)
````

<span data-ttu-id="542f5-236">Esto permite que el analista continúa con los resultados que recibió.</span><span class="sxs-lookup"><span data-stu-id="542f5-236">This leaves the analyst to continue with the results received.</span></span> <span data-ttu-id="542f5-237">Las cifras de riesgo pertinentes de interés se extraen de los resultados y se trazan.</span><span class="sxs-lookup"><span data-stu-id="542f5-237">The relevant risk figures of interest are extracted from the results and plotted.</span></span>

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


<span data-ttu-id="542f5-238">Los trazados resultantes tienen este aspecto:</span><span class="sxs-lookup"><span data-stu-id="542f5-238">The resulting plots look like this:</span></span>

|       |     |
|----    |---- |
| <img src="./assets/fsi-risk-modeling/image6.png" width="400px" alt="Figure 4 - Credit Exposure for MSFT Equity Forward - Calculated with a Cloud Based Risk Engine"/> | <img src="./assets/fsi-risk-modeling/image7.png" width="400px" alt="Figure 5 - Potential Future Exposure for MSFT Equity Forward - Calculated with a Cloud  Based Risk Engine" /> |
| <span data-ttu-id="542f5-239">Ilustración 4: Riesgo crediticio del contrato de participación a futuro de MSFT,</span><span class="sxs-lookup"><span data-stu-id="542f5-239">Figure 4 - Credit Exposure for MSFT Equity Forward -</span></span> <br/><span data-ttu-id="542f5-240">que se calcula con un motor de riesgos basado en la nube</span><span class="sxs-lookup"><span data-stu-id="542f5-240">Calculated with a Cloud Based Risk Engine</span></span> | <span data-ttu-id="542f5-241">Ilustración 5: Posible exposición futura del contrato de participación a futuro de MSFT,</span><span class="sxs-lookup"><span data-stu-id="542f5-241">Figure 5 - Potential Future Exposure for MSFT Equity Forward -</span></span> <br/> <span data-ttu-id="542f5-242">que se calcula con un motor de riesgos basado en la nube</span><span class="sxs-lookup"><span data-stu-id="542f5-242">Calculated with a Cloud  Based Risk Engine</span></span> |



## <a name="next-steps"></a><span data-ttu-id="542f5-243">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="542f5-243">Next Steps</span></span>

<span data-ttu-id="542f5-244">El acceso flexible a la nube a través de una infraestructura de proceso y los servicios de análisis de riesgos basados en SaaS pueden ofrecer mejoras en términos de velocidad y agilidad para los analistas de riesgos que trabajan en seguros y mercados de capitales.</span><span class="sxs-lookup"><span data-stu-id="542f5-244">Flexible access to the cloud through compute infrastructure and SaaS-based risk analysis services can deliver improvements in speed and agility for risk analysts working in capital markets and insurance.</span></span> <span data-ttu-id="542f5-245">En este artículo trabajamos con un ejemplo en el que se ilustra cómo usar Azure y otros servicios con las herramientas que los analistas de riesgos conocen.</span><span class="sxs-lookup"><span data-stu-id="542f5-245">In this article we worked through an example which illustrates how to use Azure and other services using tools risk analysts know.</span></span> <span data-ttu-id="542f5-246">Pruebe aprovechar las funcionalidades de Azure cuando crea y mejora los modelos de riesgo.</span><span class="sxs-lookup"><span data-stu-id="542f5-246">Try taking advantage of Azure's capabilities as you create and enhance your risk models.</span></span>

### <a name="tutorials"></a><span data-ttu-id="542f5-247">Tutoriales</span><span class="sxs-lookup"><span data-stu-id="542f5-247">Tutorials</span></span>


-   <span data-ttu-id="542f5-248">Desarrolladores de R: [Ejecución de una simulación de R en paralelo con Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)</span><span class="sxs-lookup"><span data-stu-id="542f5-248">R Developers: [Run a parallel R simulation with Azure Batch](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)</span></span>

-   <span data-ttu-id="542f5-249">[Basic R commands and RevoScaleR functions: 25 common examples](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely) (Comandos básicos de R y funciones de RevoScaleR: 25 ejemplos comunes)</span><span class="sxs-lookup"><span data-stu-id="542f5-249">[Basic R commands and RevoScaleR functions: 25 common examples](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely)</span></span>

-   <span data-ttu-id="542f5-250">[Visualize and analyze data using RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely) (Visualización y análisis de datos mediante RevoScaleR)</span><span class="sxs-lookup"><span data-stu-id="542f5-250">[Visualize and analyze data using RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely)</span></span>

-   [<span data-ttu-id="542f5-251">Introducción a las funcionalidades de ML Services y R de código abierto en HDInsight</span><span class="sxs-lookup"><span data-stu-id="542f5-251">Introduction to ML Services and open-source R capabilities on HDInsight</span></span>](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview?WT.mc_id=fsiriskmodelr-docs-scseely)

<span data-ttu-id="542f5-252">_Artículo escrito por el Dr. Darko Mocelj y Rupert Nicolay._</span><span class="sxs-lookup"><span data-stu-id="542f5-252">_This article was authored by Dr. Darko Mocelj and Rupert Nicolay._</span></span>