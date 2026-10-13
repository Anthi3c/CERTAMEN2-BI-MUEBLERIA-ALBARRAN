#CERTAMEN2-BI-MUEBLERIA-ALBARRAN

##KPI:
**Producto popular, con mayor número de ventas**: Mide la cantidad de productos vendidos, tomando en cuentas parámetros como la cantidad e ingreso total monetario generado por cada producto de la mueblería, ayudando a determinar cuales son los muebles más populares y con mayor “éxito comercial”. Siendo esto muy útil para tomar decisiones sobre la reposición de inventario y compras a otros proveedores. 

**Participación de ventas por sucursal**: Evalúa en que ubicación hay mayor concentración de ingresos y registra la cantidad de transacciones de cada sucursal de la empresa. Esto permite conocer que lugares tienen mayor rendimiento comercial y ayuda a tomar decisiones estratégicas en base a la posición geográfica. 

**Impacto y dependencia de los descuentos en las ventas**: Ayuda a detectar si un producto depende o no de las rebajas/descuentos. Permite evaluar si la estrategia de bajar los precios ayuda a “deshacerse” de productos que están en el inventario o si simplemente genera una gran brecha de las ganancias originales. 



##Pasos del Kimball:
**Proceso de Negocio**: Corresponde al proceso de venta de muebles. 
**Granularidad**: Cada fila de la tabla de hechos representará un producto específico vendido dentro de un documento de venta, permitiendo conservar el detalle exacto de las cantidades, precios unitarios y descuentos aplicados por ítem.
**Dimensiones**: Las dimensiones son, Tiempo, Cliente, Producto, Vendedor, Sucursal. 
**Medidas**: Las medidas son: Cantidad, Precio Unitario, Descuento, Monto Inicial (Cantidad * Precio Unitario), Monto Total (Precio Unitario * Cantidad - Descuento). 
**Tabla de hechos**: Se denominará “Hecho_Ventas” y almacenará la información cuantitativa (que sacamos de medidas) generada en cada transacción comercial de la mueblería.
