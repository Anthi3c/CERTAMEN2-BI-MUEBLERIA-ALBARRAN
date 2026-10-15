# CERTAMEN2-BI-MUEBLERIA-ALBARRAN
*Caso Seleccionado*: Mueblería Albarrán
<br>
*Paralelo*: 301 
<br>
*Integrantes*: Isidora Guerra, Agustin Aravena, Celestina Piniao
<br>

**Descripción del caso**: La mueblería Albarrán ocupa un sistema transaccional (OLTP) que registra sus ventas diarias, pero ahora necesita un nuevo sistema que le permita analizar la información de forma más eficiente debido a la expansión de su marca. Actualmente, resulta complejo identificar patrones de consumo, medir el rendimiento comercial por sucursal o evaluar la efectividad de los descuentos aplicados a los productos, por ende, nosotros buscamos facilitarle ese trabajo mediante .
<br>
<br>

**Objetivo del proyecto**: Se va a diseñar e implementar una solución de Inteligencia de Negocios (BI) basada en la metodología de Kimball, construyendo un Data Warehouse (OLAP) e implementando procesos ETL que permitan optimizar la toma de decisiones estratégicas comerciales basandose en la cantidad, inventario, lugar/sucursal, entre otros elementos.
<br>
<br>


## KPI:
**Producto popular, con mayor número de ventas**: Mide la cantidad de productos vendidos, tomando en cuentas parámetros como la cantidad e ingreso total monetario generado por cada producto de la mueblería, ayudando a determinar cuales son los muebles más populares y con mayor “éxito comercial”. Siendo esto muy útil para tomar decisiones sobre la reposición de inventario y compras a otros proveedores.

**Participación de ventas por sucursal**: Evalúa en que ubicación hay mayor concentración de ingresos y registra la cantidad de transacciones de cada sucursal de la empresa. Esto permite conocer que lugares tienen mayor rendimiento comercial y ayuda a tomar decisiones estratégicas en base a la posición geográfica. 

**Mes con mayor número de ventas**: Este indicador permite identificar patrones de estacionalidad, períodos con alta demanda y meses con menor desempeño comercial en la mueblería. Es fundamental para la planificación financiera, la programación de campañas de marketing de temporada y la gestión eficiente de inventarios antes de los mayores periodos de venta.
<br>
<br>
<br>

## Pasos del Kimball:

**Proceso de Negocio**: Corresponde al proceso de venta de muebles. 

**Granularidad**: Cada fila de la tabla de hechos representará un producto específico vendido dentro de un documento de venta, permitiendo conservar el detalle exacto de las cantidades, precios unitarios y descuentos aplicados por ítem.

**Dimensiones**: Las dimensiones son, Tiempo (Dim_Tiempo), Cliente (Dim_Cliente), Producto (Dim_Producto), Vendedor (Dim_Vendedor), Sucursal (Dim_Sucursal). 

**Medidas**: Las medidas son: Cantidad, Precio Unitario, Descuento, Monto Inicial (Cantidad * Precio Unitario), Monto Total (Precio Unitario * Cantidad - Descuento).

**Tabla de hechos**: Se denominará “Hecho_Ventas” y almacenará la información cuantitativa (que sacamos de medidas) generada en cada transacción comercial de la mueblería.
<br>
<br>
<br>

## Modelo Estrella
Para la construcción del Data Warehouse (DW_Albarran), se migró la estructura transaccional de origen (Albarran) hacia un Modelo en Estrella, basándonos en la metodología de Kimball. Esta decisión se justifica por: 
- **Rendimiento superior en consultas (OLAP)**: Al desnormalizar entidades como cargos y comunas dentro de las dimensiones que se han establecido, se reduce la cantidad de cruces (JOINs) requeridos, acelerando la velocidad de respuesta en la tabla Hecho_Ventas.   
- **Compatibilidad y simplicidad en BI**: Ofrece una estructura limpia e intuitiva para herramientas de reportabilidad (como Power BI), facilitando la creación de gráficos, filtros y KPIs.
- **Claridad del negocio**: Separa claramente las métricas cuantitativas (hechos) de los atributos descriptivos (dimensiones), haciendo que la información sea fácil de analizar y mantener. 


## PROCESO ETL - fase 1
1. Descripción General Del Proceso ETL:
Este proceso consiste en la extración, transformación y carga de los datos, lo desarrollaremos usando Visual Studio 2019 con el componente de SSIS (SQL Server Integration Services). Su objetivo principal es extraer los datos desde la base de datos transaccional (OLTP) original de la mueblería, limpiarlos y cargarlos de forma estructurada en las tablas dimensionales del nuevo Data Warehouse (DW_Muebleria).
<br>
2. A partir del respaldo de origen, creamos una nueva Base de Datos (BD) llamada DW_Albarran (nuestro Data Warehouse). En ella definimos las tablas basadas en las 5 dimensiones que ya establecimos anteriormente (Dim_Cliente, Dim_Producto, Dim_Sucursal, Dim_Vendedor, Dim_Tiempo) y la tabla central de hechos,  "Hecho_Ventas". Todo esto estructurado en un Modelo Estrella (OLAP) que nos permitira hacer consultas de manera fácil y rápidas.
<br>
3. En Visual Studio 2019 creamos un proyecto de Integration Services y configuramos dos administradores de conexiones OLE DB: el Origen (conectado a la BD transaccional Albarran) y el Destino (conectado a DW_Albarran). A continuación, estructuramos el flujo de control (Control Flow) utilizando componentes de Data Flow Task para automatizar la extracción de datos de cada entidad, aplicando transformaciones esenciales como el mapeo de columnas, la homogenización de tipos de datos y la desnormalización de atributos secundarias (por ejemplo, unificando cargos en vendedores y comunas en sucursales).
<br>
4. Establecimos una secuencia de ejecución obligatoria donde se cargan primero las 5 dimensiones independientes (Dim_Cliente, Dim_Producto, Dim_Sucursal, Dim_Vendedor, Dim_Tiempo). Una vez pobladas las dimensiones, ejecutamos la carga de la tabla de hechos (Hecho_Ventas) utilizando transformaciones de búsqueda (Lookup) en SSIS para relacionar las claves de negocio del origen con las claves subrogadas de nuestras dimensiones, garantizando la integridad referencial y dejando el Data Warehouse 100% operativo para su análisis.



## Tecnologías Usadas
- Visual Studio 2019
- SQL Server Management Studio (SSMS)



## Instrucciones de ejecución/revisión
Estos son los pasos que se realizarán para desarrollar la fase 1: 

1. **Restauración de la Base de Datos Origen:**
   - Restaurar el archivo de respaldo de la base de datos transaccional (archivo `.bak` original proporcionado) en SQL Server Management Studio (SSMS).

2. **Creación del Data Warehouse:**
   - Abrir y ejecutar el script `database/scripts/script_creacion_dw.sql` en SSMS para estructurar la base de datos de destino `DW_Albarran` con sus tablas dimensionales y de hechos en modelo estrella.

3. **Ejecución del Proceso ETL en SSIS (Visual Studio 2019):**
Abrir el proyecto de Integration Services en Visual Studio 2019. Configurar los administradores de conexión OLE DB tanto para la base de datos de origen (Albarran) como para la de destino (DW_Albarran). Ejecutar el paquete SSIS para poblar primero las tablas dimensionales (Dim_Cliente, Dim_Producto, Dim_Sucursal, Dim_Vendedor, Dim_Tiempo) y posteriormente la tabla central de hechos (Hecho_Ventas), asegurando la correcta integración y mapeo de las claves subrogadas.



## Estructura Del Proyecto (Por ahora)
- **CERTAMEN2-BI-MUEBLERIA-ALBARRAN/**
  - `README.md`
  - **docs/**
    - `modelo-dimensional/`
    - `presentacion/`
  - **database/**
    - `scripts/`
    - `backup/`
  - **etl/**
    - `proyecto-ssis/`
<br>
<br>

## Distribución de responsabilidades y aportes
- **Celestina Piniao — Análisis y Diseño Dimensional**: Definición de KPIs, redactar el diseño del modelo en estrella (Kimball) y redacción de Readme.   
- **Isidora Guerra — Base de Datos y Tabla de Hechos**: Crear DataWarehouse, incluyendo todas las dimensiones y tabla de Hechos. Crear modelo estrella de forma gráfica.
- **Agustin Aravena — Calidad, Validación y Repositorio**: Cargar en SSIS las dimensiones, validación de datos (origen vs destino).

Todos aportan con el ppt y organización del Github (Archivos). 



 
