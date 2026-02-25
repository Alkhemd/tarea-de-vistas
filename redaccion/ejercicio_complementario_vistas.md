Parte 8: Ejercicio Complementario (Creación de Vistas)

En esta última sección, llevé a cabo una serie de cinco ejercicios complementarios enfocados en la base de datos Northwind. Mi objetivo fue aplicar los conocimientos adquiridos para diseñar e implementar vistas que resolvieran requerimientos de información específicos.

1. Vista A: Productos de la empresa
El primer requerimiento solicitaba mostrar el código, el nombre y el precio unitario de todos los productos.
Para ello, redacté la consulta seleccionando directamente los campos ProductID, ProductName y UnitPrice de la tabla Products. Nombré la vista incluyendo mi matrícula (19091113).

CREATE VIEW Vista_Productos_19091113 AS
SELECT ProductID, ProductName, UnitPrice 
FROM Products
GO

Ejecuté la instrucción y obtuve el mensaje de comandos completados correctamente. Posteriormente, consulté la vista (SELECT * FROM Vista_Productos_19091113) para confirmar la extracción correcta de la lista.
[INSERTAR CAPTURA 1: Pantalla con el código de la Vista A y su ejecución/resultado aquí]

2. Vista B: Productos de la categoría Beverages
El segundo punto requería visualizar únicamente aquellos productos pertenecientes a la categoría "Beverages" (Bebidas).
Para esto, utilicé un INNER JOIN para conectar la tabla Products con la tabla Categories, permitiéndome filtrar mediante una cláusula WHERE condicional donde el CategoryName fuera exactamente igual a 'Beverages'.

CREATE VIEW Vista_Bebidas_19091113 AS 
SELECT p.ProductID, p.ProductName, p.UnitPrice, c.CategoryName 
FROM Products p 
INNER JOIN Categories c ON p.CategoryID = c.CategoryID 
WHERE c.CategoryName = 'Beverages'
GO

Tras ejecutar y crear la vista de manera exitosa, realicé el SELECT correspondiente probando el despliegue del catálogo exclusivo de bebidas.
[INSERTAR CAPTURA 2: Pantalla con el código de la Vista B y su ejecución/resultado aquí]

3. Vista C: Datos del cliente y fechas de órdenes
El inciso C me encomendó unir la información de los clientes con el registro de las fechas de sus respectivos pedidos.
Logré este objetivo relacionando matemáticamente la tabla Customers con la tabla Orders a través de su campo coincidente (CustomerID) mediante otro INNER JOIN.

CREATE VIEW Vista_Clientes_Ordenes_19091113 AS 
SELECT c.CustomerID, c.CompanyName, c.ContactName, o.OrderID, o.OrderDate 
FROM Customers c 
INNER JOIN Orders o ON c.CustomerID = o.CustomerID
GO

Al generar y consultar la vista, se desplegó correctamente el emparejamiento entre cada cliente y la fecha en que realizó su historial de órdenes emitidas.
[INSERTAR CAPTURA 3: Pantalla con el código de la Vista C y su ejecución/resultado aquí]

4. Vista D: Conteo de productos por categoría
El cuarto requerimiento pidió calcular la cantidad total de productos existentes agrupados dentro de cada categoría.
Para solucionar esto, empleé la función de agregación COUNT() evaluando los códigos de producto, y añadí la directiva GROUP BY CategoryName para que el sistema agrupara el recuento total separado para cada una de las familias.

CREATE VIEW Vista_Cant_Prod_Categoria_19091113 AS 
SELECT c.CategoryName, COUNT(p.ProductID) AS CantidadProductos 
FROM Products p 
INNER JOIN Categories c ON p.CategoryID = c.CategoryID 
GROUP BY c.CategoryName
GO

Recibí una confirmación positiva y comprobé, con un SELECT, una tabla final que enlistaba las categorías acompañadas por su suma de artículos registrados.
[INSERTAR CAPTURA 4: Pantalla con el código de la Vista D y su ejecución/resultado aquí]

5. Vista E: Promedios de las categorías Produce y Confections
Finalmente, el inciso exigía mostrar exclusivamente el promedio matemático de los precios unitarios correspondientes solo a las categorías "Produce" y "Confections".

Utilicé un filtrado IN ('Produce', 'Confections') en la instrucción WHERE para discriminar al resto de las categorías y empleé la potente función de cálculo de promedio AVG() aplicándola sobre la columna UnitPrice. Todo esto también organizado bajo su correspondiente GROUP BY.

CREATE VIEW Vista_Promedio_Precios_19091113 AS 
SELECT c.CategoryName, AVG(p.UnitPrice) AS PromedioPrecio 
FROM Products p 
INNER JOIN Categories c ON p.CategoryID = c.CategoryID 
WHERE c.CategoryName IN ('Produce', 'Confections') 
GROUP BY c.CategoryName
GO

La ejecución concluyó exitosamente, y mi verificación con la consulta de tabla desplegó exactamente las filas conteniendo el cálculo tarifario medio solicitado, concluyendo en su totalidad mis ejemplos de reportes condicionados y cálculo modular.
[INSERTAR CAPTURA 5: Pantalla con el código de la Vista E y su ejecución/resultado aquí]
