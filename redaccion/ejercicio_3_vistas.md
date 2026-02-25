Parte 4: Ejemplos de Vistas (Ejercicio 3)

Para este tercer ejercicio, el objetivo fue crear una nueva vista en la base de datos Northwind que mostrara la sumatoria de las ventas agrupadas por producto, aplicando funciones de agregación y cálculos de descuentos.

1. Análisis de los requerimientos y cálculos solicitados
Al revisar las instrucciones, identifiqué que la vista requería extraer el nombre del producto y calcular dos sumatorias distintas:
- El subtotal, obtenido multiplicando el precio unitario por la cantidad.
- El total real, al cual se le debía restar el descuento aplicado al subtotal original.

Para lograr esto, supe que debía relacionar la tabla Products con la tabla Order Details mediante un INNER JOIN, y utilizar la función matemática SUM para agrupar los totales por cada producto.

2. Creación de la vista
Teniendo clara la lógica y las operaciones matemáticas, abrí mi hoja de consultas en SSMS y redacté el script utilizando de nuevo mi matrícula (19091113) para cumplir con el requisito de nombrar la vista como TotalProductos_19091113.

El código diseñado fue el siguiente:


CREATE VIEW TotalProductos_19091113
AS
    SELECT 
        p.ProductName,
        SUM(od.UnitPrice * od.Quantity) AS Subtotal,
        SUM((od.UnitPrice * od.Quantity) - ((od.UnitPrice * od.Quantity) * od.Discount)) AS Total
    FROM Products p
    INNER JOIN [Order Details] od ON p.ProductID = od.ProductID
    GROUP BY p.ProductName
GO


Al ejecutar este código presionando F5, el analizador de SQL agrupó correctamente los productos gracias a la cláusula GROUP BY y generó las funciones de suma para construir la vista. Recibí el mensaje de "Comandos completados correctamente" en la parte inferior, validando mi estructura.
[INSERTAR CAPTURA 1: Pantalla con el código de creación de la vista TotalProductos_19091113 y el mensaje de éxito aquí]

3. Ejecución y ordenamiento de los resultados
Para finalizar el ejercicio, las instrucciones establecían que los resultados debían mostrarse ordenados por el campo ProductName. Como el ordenamiento definitivo se recomienda aplicar al momento de consultar la vista, preparé mi consulta final añadiendo la cláusula ORDER BY:


SELECT * FROM TotalProductos_19091113
ORDER BY ProductName


Tras ejecutar la consulta, obtuve una tabla consolidada mostrando cada uno de los productos de la empresa, acompañados de su subtotal histórico en ventas y el total ya con el descuento aplicado, todo perfectamente enlistado en orden alfabético. Este resultado evidenció el correcto funcionamiento de las funciones de agrupación (SUM) exigidas en la guía.
[INSERTAR CAPTURA 2: Pantalla mostrando los resultados de las sumatorias ordenados alfabéticamente aquí]
