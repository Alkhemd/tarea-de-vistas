Parte 3: Ejemplos de Vistas (Ejercicio 2)

Para este segundo ejercicio se me solicitó continuar trabajando con la base de datos Northwind. El objetivo principal fue crear una nueva vista que mostrara información detallada sobre las ventas de productos realizadas específicamente durante el año 1997.

1. Análisis de los requerimientos y tablas necesarias
Al leer las instrucciones, noté que necesitaba extraer cuatro campos específicos: el nombre del producto, la fecha de pedido, la fecha de envío y un cálculo matemático para el subtotal (multiplicando el precio unitario por la cantidad).

Dado que esta información no se encuentra en una sola tabla, me di cuenta de que necesitaría unir tres tablas diferentes usando la cláusula INNER JOIN:
- La tabla Products para obtener el nombre del producto (ProductName).
- La tabla Orders para obtener las fechas de pedido y envío (OrderDate y ShippedDate), además de servir para filtrar por el año 1997.
- La tabla Order Details (Detalles de la orden) para extraer el precio unitario y la cantidad, y así poder calcular el subtotal.

2. Creación de la vista
Procedí a abrir una hoja de consultas limpia en mi SQL Server Management Studio. Teniendo claro qué tablas unir, redacté el código utilizando mi número de carnet asignado (19091113) para nombrar la estructura como "PedidosProductos1997_19091113", tal como se pedía en las instrucciones.

El código que utilicé fue el siguiente:

CREATE VIEW PedidosProductos1997_19091113
AS
    SELECT 
        p.ProductName, 
        o.OrderDate, 
        o.ShippedDate, 
        (od.UnitPrice * od.Quantity) AS Subtotal
    FROM Products p
    INNER JOIN [Order Details] od ON p.ProductID = od.ProductID
    INNER JOIN Orders o ON od.OrderID = o.OrderID
    WHERE YEAR(o.OrderDate) = 1997
GO

Ejecuté el script presionando F5. A pesar de que el sistema interno de SSMS podía mostrar algún subrayado rojo de advertencia visual, sabía que mi código era estructuralmente correcto. Inmediatamente después, el programa me arrojó el mensaje de éxito indicando que los comandos se habían completado correctamente, lo que confirmó que la vista se había guardado en el servidor.
[INSERTAR CAPTURA 1: Pantalla con el código de la creación de la vista y el mensaje de éxito en la parte de abajo aquí]

3. Ejecución y ordenamiento de los resultados
El punto número dos del ejercicio pedía explícitamente que los resultados se ordenaran por el campo ProductName. En SQL Server, las buenas prácticas indican que el ordenamiento se debe realizar al momento de consultar la vista y no al crearla.

Por lo tanto, borré el código de creación de mi hoja de consultas y redacté una instrucción de consulta (SELECT) combinada con la cláusula ORDER BY para llamar a mi vista recién creada y acomodar los datos alfabéticamente por el nombre del producto.

El código de consulta fue este:

SELECT * FROM PedidosProductos1997_19091113
ORDER BY ProductName

Al ejecutar esta instrucción, obtuve exitosamente una tabla detallada con miles de registros de ventas, filtrados exclusivamente para el año 1997, mostrando las fechas precisas, el cálculo exacto del subtotal por línea, y todo perfectamente ordenado de la A a la Z por la primera columna (el nombre del producto), cumpliendo así con todos los incisos del ejercicio.
[INSERTAR CAPTURA 2: Pantalla mostrando la tabla de resultados de la consulta SELECT con el ORDER BY ejecutada aquí]
