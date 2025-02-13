🚗📊 Dashboard Interactivo de Ventas - Concesionaria de Vehículos
=============

### 📌 Descripción del Proyecto
               
----
Este proyecto tiene como objetivo desarrollar un dashboard interactivo en Power BI para analizar el rendimiento de ventas de una concesionaria de vehículos. A través de la integración y análisis de datos, se busca proporcionar una herramienta visual intuitiva para la toma de decisiones estratégicas.

![](/images/01.jpg)

<img src="/images/01.jpg" alt="Descripción de la imagen" width="500" height="300">

### 🔥 Características Clave


#### 📊 **Cálculo de métricas clave: **

Determinación de indicadores de rendimiento, segmentados por año: 

📌**TOTAL DE VENTAS TOTALES:** 
Calculamos el *Total de Ventas* para conocer el rendimiento comercial sin considerar impuestos.

```DAX
Total de Ventas = SUM(Fact_Ventas[Precio Venta sin IGV])  
```

![](/images/total_ventas.jpg)

🧑‍**CANTIDAD DE CLIENTES:** Esta métrica calcula el número total de clientes únicos que han realizado compras. Es fundamental para analizar el alcance del negocio y la fidelización de clientes.

```DAX
Cantidad de Clientes_FACT_VENTAS = DISTINCTCOUNT(Fact_Ventas[Cliente])
```
![](/images/cant_clientes.jpg)

📊**Crecimiento Año contra Año (YoY Growth):** 
Para analizar el desempeño de las ventas a lo largo del tiempo, calculamos el crecimiento interanual **(YoY - Year over Year)** en porcentaje.

- **Valor positivo (+X%)** → Aumento en las ventas respecto al año anterior.
- **Valor negativo (-X%)** → Disminución de ventas comparado con el año anterior.
- **Valor cercano a 0%** → Ventas estables sin cambios significativos.

```DAX
YoYear = VAR VentasLY = CALCULATE([Total de Ventas], DATEADD(Dim_Fechas[Date],-1,YEAR)) RETURN DIVIDE([Total de Ventas] - VentasLY, VentasLY,0)
```
![](/images/YoY.jpg)

Calculamos una nueva medida. 
📊**Cumplimiento: ** 
Indica qué porcentaje del objetivo de ventas se ha alcanzado en comparación con el presupuesto establecido. Es una métrica clave para evaluar el desempeño comercial. 

-	**Si el resultado es 100%,** significa que las ventas alcanzaron exactamente el presupuesto.
-	**Si el resultado es menor a 100%,** las ventas están por debajo del objetivo.
-	**Si el resultado es mayor a 100%,** se superó el objetivo de ventas.

```DAX
Cumplimiento = DIVIDE([Total de Ventas], [Total PPTO])
```
![](/images/cump.jpg)

📊 **Crecimiento Trimestral:** 
Para analizar el desempeño de las ventas en periodos más cortos, realizamos una comparación de cada trimestre con el trimestre anterior del año pasado. 

- **Valor positivo (+X%)** → Indica un aumento en las ventas respecto al mismo trimestre del año anterior.
- **Valor negativo (-X%)** → Indica una disminución en las ventas en comparación con el trimestre pasado.
- **Valor cercano a 0%** → Sugiere que las ventas se han mantenido estables.

```DAX
Venta del Trimestre del Año Pasado = CALCULATE([Total de Ventas],PARALLELPERIOD(Dim_Fechas[Date],-4, QUARTER))

Crecimiento Trimestral = [Total de Ventas] - [Venta del Trimestre del Año Pasado]

```
![](/images/02.jpg)


📊 **Segmentación de Clientes:** 
Una visualización clave para el análisis de la ruta de mercado es la segmentación de clientes, ya que permite identificar patrones de compra.

![](/images/03.jpg)

📊 **Comparación de Ventas por Sede:** Para analizar el rendimiento de cada ubicación, realizamos un gráfico de barras agrupadas que compara las ventas por sede. Esta visualización permite identificar qué sucursales tienen un mejor desempeño y detectar oportunidades de mejora. 

![](/images/04.jpg)

📊 **Comparación del Porcentaje de Ventas por Marca en Cada Trimestre:** 
Para analizar el desempeño de las distintas marcas de vehículos, realizamos un gráfico de comparación del porcentaje de ventas por marca en cada trimestre. Esta visualización nos permite identificar tendencias y evaluar la participación de mercado de cada fabricante a lo largo del tiempo.

![](/images/05.jpg)

Ahora crearemos un gráfico de Pareto, pero antes, explicaré la teoría fundamental para entender su significado y aplicación.

#### **¿Qué es la regla de Pareto? **

La regla de Pareto (también conocida como la ley 80/20) establece que, en muchos casos, aproximadamente el 80% de los efectos provienen del 20% de las causas.

##### **Porcentaje de Pareto:**

El porcentaje de Pareto calcula la contribución relativa de cada elemento en un conjunto de datos respecto al total, aplicando el **Principio de Pareto** (también llamado la "regla 80/20").

##### **Fórmula General**

El porcentaje de Pareto se calcula con:
   
$$
\%Pareto = \frac{\text{Valor de cada elemento}}{\text{Total de todos los elementos}} \times 100
$$
                

##### **¿Por qué es útil?**
En este contexto, nos ayuda a identificar cuáles modelos son los más rentables en relación con las ventas totales. Si un modelo genera un porcentaje alto de las ventas, es posible enfocar más esfuerzos en promover ese modelo o en mantener su disponibilidad en inventario. 

Calculamos el porcentaje de Pareto para conocer el porcentaje de contribución de un modelo específico con respecto al total de ventas de todos los modelos.

```DAX
%Pareto = DIVIDE([ValorModelo], [Total de Venta Modelo])
```

Aplicando la regla de Pareto, observamos que el **20.42%** de los modelos representan la mayor parte de la ganancia generada en nuestro modelo de negocio.

![](/images/06.jpg)

