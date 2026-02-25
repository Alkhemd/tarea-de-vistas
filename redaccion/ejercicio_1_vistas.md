Parte 2: Ejemplos de Vistas (Ejercicio 1)

En este ejercicio se me solicitó crear una vista a partir de los datos almacenados en una base de datos de ejemplo llamada Northwind. 

1. Descarga y Restauración de la Base de Datos Northwind
Me percaté de que esta base de datos no viene preinstalada por defecto en SQL Server 2022. Por ende, el primer paso que realicé fue obtener el script original (instnwnd.sql) directamente desde el repositorio oficial de Microsoft SQL Server Samples en GitHub.

Dentro de mi entorno (SSMS), abrí una nueva hoja de consultas (Ctrl+N) y preparé el terreno ejecutando instrucciones manuales para crear una base de datos vacía y seleccionarla:

CREATE DATABASE Northwind;
GO
USE Northwind;

Posteriormente, dentro de la base de datos Northwind seleccionada, pegué la totalidad del script descargado y lo ejecuté exitosamente. Para confirmar, refresqué mi vista del "Explorador de objetos" y corroboré la aparición de las carpetas de Northwind.
[INSERTAR CAPTURA 1: Pantalla de la base de datos Northwind creada, en donde pegué código en 'master' y vimos que se ejecutó correctamente aquí]

2. Creación de la Vista (Pasos 1 al 4)
Una vez que el entorno estuvo preparado, procedí a resolver las instrucciones de la guía, las cuales pedían mostrar los productos del proveedor específico SupplierID = 14.

Para ello, escribí la siguiente instrucción CREATE VIEW. Prestando especial cuidado a la indicación resaltada en rojo del manual, sustituí el texto genérico para nombrar la vista con mi número de carnet/matrícula estudiantil real (19091113):


CREATE VIEW productos_proveedor_19091113
AS
    SELECT ProductID, ProductName, SupplierID, CategoryID, QuantityPerUnit, UnitPrice, Discontinued
    FROM Products
    WHERE SupplierID = 14
GO

Durante este proceso, el motor de inteligencia de código de SSMS (IntelliSense) pintó algunas alertas visuales en rojo (subrayados de error) bajo nombres de columnas o de tablas. Sin embargo, reconocí que esto se debía únicamente a que el programa acababa de cargar la base de datos y su sistema interno de auto-completado aún no la había indexado.
[INSERTAR CAPTURA 2: Pantalla de las alertas rojas de IntelliSense al crear la vista, antes de ejecutar aquí]

Desestimé esas falsas alertas y continué con la ejecución pulsando F5. Verifiqué que SSMS confirmaba la creación mostrando el mensaje: "Comandos completados correctamente".
[INSERTAR CAPTURA 3: Pantalla tras ejecutar el CREATE VIEW con éxito (sin errores reales) y el mensaje de éxito final aquí]

3. Consulta de la Vista (Paso 5)
Para asegurarme de que los datos obtenidos eran los correctos, ejecuté una petición regular que llamara a la estructura recién creada:

SELECT * FROM productos_proveedor_19091113

Como resultado, el sistema me arrojó bajo la consulta una tabla conteniendo los tres productos correspondientes ("Gorgonzola Telino", "Mascarpone Fabioli" y "Mozzarella di Giovanni") del respectivo proveedor.
[INSERTAR CAPTURA 4: Pantalla con el resultado de la consulta SELECT de la pequeña tabla de 3 filas aquí]

4. Modificación de la Vista (Paso 6)
Al verificar el éxito, el manual exigió que se mostrara adicionalmente el nombre de la compañía proveedora (CompanyName), dato que únicamente existe en la tabla Suppliers. 

A través del comando ALTER VIEW e introduciendo una relación estructurada con INNER JOIN, procedí a unir temporalmente ambas tablas por la columna en común, adaptando el script del primer apartado a lo siguiente:


ALTER VIEW productos_proveedor_19091113
AS
    SELECT 
        p.ProductID, 
        p.ProductName, 
        p.SupplierID, 
        s.CompanyName, 
        p.CategoryID, 
        p.QuantityPerUnit, 
        p.UnitPrice, 
        p.Discontinued
    FROM Products p
    INNER JOIN Suppliers s ON p.SupplierID = s.SupplierID
    WHERE p.SupplierID = 14
GO


5. Eliminación de la Vista (Paso 7)
Como demostración final requerida en este segmento, fue necesario saber borrar de manera controlada esta estructura o consulta temporal para limpiar la base de datos.
Apliqué el comando universal de borrado para vistas mediante:

DROP VIEW productos_proveedor_19091113

Al correrlo y obtener el exitoso mensaje de finalización, pasé a corroborarlo. Recargué y navegué en las subcarpetas del Explorador (Northwind > Vistas) cerciorándome de que ya no se encontraba por ningún lado; igualmente lo probé ejecutando de nuevo la consulta original, la cual me generó un error con letras rojas argumentando "El nombre de objeto 'productos_proveedor_19091113' no es válido", probando contundentemente la eliminación efectiva.
[INSERTAR CAPTURA 5: Pantalla con la consulta DROP VIEW demostrando que los comandos procedieron bien aquí]
