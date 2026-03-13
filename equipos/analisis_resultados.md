VI. Analisis de resultados

Crear en parejas el siguiente ejercicio:

Utilizando la siguiente base de datos realizar lo siguiente.

CREATE DATABASE Control_Pedidos
GO

USE Control_Pedidos
GO

CREATE TABLE producto
(idproducto CHAR(8) not null,
nombreproducto VARCHAR(25),
existencia INT not null,
precio DECIMAL(10,2) not null, --precio costo
preciov DECIMAL(10,2) not null, --precio venta
CONSTRAINT pk_idproducto PRIMARY KEY (idproducto)
)
GO

CREATE TABLE pedidos
(
idpedido INT IDENTITY,
idproducto CHAR(8) not null,
cantidad_pedido INT,
CONSTRAINT fk_idbod FOREIGN KEY (idproducto) REFERENCES producto(idproducto)
)
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION DE LA BASE DE DATOS Y TABLAS]


Agregar los siguientes registros:

INSERT INTO producto VALUES('prod001','Filtros pantalla',5,10,12.5)
INSERT INTO producto VALUES('prod002','parlantes',7,10,11.5)
INSERT INTO producto VALUES('prod003','mouse',8,4.5,6)
INSERT INTO producto VALUES('prod004','monitor',10,60.2,80.0)
INSERT INTO producto VALUES('prod005','lapiz',5,1.2,2.0)

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION DE LOS REGISTROS]


Ejercicios:

1. Crear un desencadenador que se active cada vez que se inserte un registro en la tabla pedidos y otro para la tabla producto.

--Disparador para tabla producto
CREATE TRIGGER trg_ins_producto
ON producto
FOR INSERT
AS
BEGIN
    PRINT 'Se ha insertado un nuevo registro en la tabla producto'
END
GO

--Disparador para tabla pedidos
CREATE TRIGGER trg_ins_pedido
ON pedidos
FOR INSERT
AS
BEGIN
    PRINT 'Se ha insertado un nuevo registro en la tabla pedidos'
END
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION Y PRUEBA DE LOS DISPARADORES SOLICITADOS EN EL PREGUNTA 1]


2. Crear un desencadenador para la tabla producto, que se active cada vez que se inserte un registro o se actualice la columna precio, la condicion para aceptar al insercion o la actualizacion es que el precio costo no debe ser mayor que el precio venta.

--Disparador de validacion de precios en producto
CREATE TRIGGER trg_valida_precios
ON producto
FOR INSERT, UPDATE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM inserted WHERE precio > preciov)
    BEGIN
        RAISERROR('Error: El precio de costo no puede ser mayor que el precio de venta.', 16, 1)
        ROLLBACK TRANSACTION
    END
END
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION Y PRUEBA DEL DISPARADOR DE LA PREGUNTA 2]


3. Crear un desencadenador para la tabla pedidos que cada vez que se realice un pedido descuente la existencia de la tabla productos, en caso que la cantidad del pedido supere a la existencia debe deshacer la transaccion y mostrar un mensaje de error.

--Disparador de actualizacion de stock en pedidos
CREATE TRIGGER trg_valida_stock
ON pedidos
FOR INSERT
AS
BEGIN
    DECLARE @idprod CHAR(8), @cantidad INT, @stock INT

    --Se obtienen los datos del registro recien insertado
    SELECT @idprod = idproducto, @cantidad = cantidad_pedido FROM inserted
    --Se obtiene el nivel de existencias actual en el inventario
    SELECT @stock = existencia FROM producto WHERE idproducto = @idprod

    --Si la cantidad solicitada supera el stock en la base de datos
    IF (@cantidad > @stock)
    BEGIN
        RAISERROR('Error: La cantidad solicitada supera las existencias disponibles.', 16, 1)
        ROLLBACK TRANSACTION
    END
    ELSE
    --Si aun hay existencias se procesa a descontarlas del catalogo original
    BEGIN
        UPDATE producto 
        SET existencia = existencia - @cantidad 
        WHERE idproducto = @idprod
    END
END
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION Y EL BLOQUEO/ERROR GENERADO POR EL DISPARADOR DE LA PREGUNTA 3]


VII. Referencia Bibliografica

Microsoft SQL Server 2008, Guia Practica, Francisco Charte Ojeda; Anaya Multimedia.
