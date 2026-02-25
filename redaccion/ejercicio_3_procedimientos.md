Parte 7: Procedimientos Almacenados (Ejercicio 3)

Como último apartado, este ejercicio requería crear un procedimiento almacenado diseñado para el sistema de ventas de Northwind, el cual evolucionó a lo largo de varios pasos para calcular matemáticamente el total facturado a partir de un número de orden.

1. Creación Básica del Procedimiento
Para iniciar, modelé en mi hoja principal de SQL Server Management Studio una versión sencilla del mandato mediante la expresión CREATE PROCEDURE, nombrándolo PROCE_Total19091113 para usar mi matrícula.

Bajo la cláusula AS, ordené un SELECT OrderID agregando la función de suma: SUM. En el interior, introduje el núcleo matemático que multiplicaba UnitPrice por Quantity y extraía el Discount (Descuento final), concluyendo todo con su obligada conversión a tipo de dato Money. El código no pedía parámetros de entrada todavía.

CREATE PROCEDURE PROCE_Total19091113
AS
    SELECT OrderID,
    SUM(CONVERT(money,(UnitPrice*Quantity*(1-Discount)/100))*100) AS Total
    FROM [Order Details]
    GROUP BY OrderID
GO

Ejecuté el script y obtuve el mensaje de comandos completados correctamente.
[INSERTAR CAPTURA 1: Pantalla con el código CREATE PROCEDURE básico y el mensaje de éxito aquí]

2. Ejecución Básica
Para probar esta primera versión, ejecuté el procedimiento almacenado directamente sin enviarle ningún valor adicional:

EXEC PROCE_Total19091113

El resultado fue una lista general mostrando la sumatoria total de absolutamente todas las órdenes que existían en la tabla de Order Details, comprobando que la operación aritmética base funcionaba bien.
[INSERTAR CAPTURA 2: Pantalla con la gran tabla de resultados de la primera ejecución básica aquí]

3. Alteración para Filtrar por Orden Específica
A continuación, el manual solicitaba modificar el código para que el procedimiento pudiera recibir una orden específica que el usuario quisiera buscar. Borré mi hoja de consultas y reemplacé el código usando ALTER PROCEDURE para añadir la variable de entrada de texto @Cod_orden. 

Añadí la cláusula WHERE OrderID=@Cod_orden para asegurar que la suma se calculase únicamente para la orden solicitada.

ALTER PROCEDURE PROCE_Total19091113
@Cod_orden int
AS
    SELECT OrderID,
    SUM(CONVERT(money,(UnitPrice*Quantity*(1-Discount)/100))*100) AS Total
    FROM [Order Details]
    WHERE OrderID=@Cod_orden
    GROUP BY OrderID
GO

Ejecuté este código de alteración e inmediatamente recibí la confirmación de compilación exitosa.
[INSERTAR CAPTURA 3: Pantalla ejecutando el ALTER PROCEDURE básico aquí]

4. Ejecución de Prueba con Orden Específica
Llegó el momento de probar la nueva modificación utilizando el código de una orden existente, la orden número 10248.

EXEC PROCE_Total19091113 10248

En la pestaña de resultados, el sistema me arrojó satisfactoriamente una tabla pequeña mostrando de manera exclusiva la suma total perteneciente únicamente a la orden 10248.
[INSERTAR CAPTURA 4: Pantalla con el pequeño resultado de la orden 10248 aquí]

5. Ejecución con Orden Inexistente (Comprobación de Falla)
Siguiendo los pasos del manual, procedí a evaluar el comportamiento del programa al buscar una orden que carecía de registros en la tabla. Ejecuté la búsqueda para la orden número 10242.

EXEC PROCE_Total19091113 10242

El análisis de esta prueba comprobó cómo un comportamiento indeseable afecta el despliegue. El sistema buscó sin éxito el registro 10242 y debido a su programación rudimentaria inicial, procedió a devolver una tabla con las columnas organizadas, pero enteramente vacía de contenido. Esto evidenció la necesidad de programar una validación más robusta que entregara información útil al usuario en caso de consultar números inexistentes.
[INSERTAR CAPTURA 5: Pantalla con la tabla vacía devolviendo los datos sin registros para la orden 10242 aquí]

6. Alteración Final del Código a Formato Validado
Dado el problema que producía buscar órdenes inexistentes, apliqué la alteración final al código, dotándolo de inteligencia condicional (las famosas declaraciones IF / ELSE).

Para esta última mejora, declaré una variable interna llamada @total, a la cual le asigné el resultado de usar un comando lógico COUNT. Ese conteo revisaba la tabla en búsqueda del número de orden que se solicitaba y así podíamos usar el condicional IF: si el conteo superaba o igualaba a 1, significaba que la orden sí existía, activándose inmediatamente las matemáticas vistas previamente. Sin embargo (cláusula ELSE), si dicho conteo arrojaba cero, entonces el procedimiento evadiría imprimir una tabla blanca y en su lugar generaría directamente un mensaje de error detallando que la orden no existía.

El código exhaustivo resultó de esta forma:

ALTER PROCEDURE PROCE_Total19091113
@Cod_orden int
AS
DECLARE @total INT
SELECT @total=count(orderid) FROM [Order Details] WHERE OrderID=@Cod_orden
IF (@total>=1) 
    BEGIN
        SELECT OD.OrderID,
        SUM(CONVERT(money,(OD.UnitPrice*Quantity*(1-Discount)/100))*100) as Total
        FROM [Order Details] OD
        WHERE OrderID=@Cod_orden
        GROUP BY OD.OrderID
    END
ELSE
    BEGIN
        PRINT 'La orden no existe y el codigo es: ' + convert(varchar(10),@Cod_orden)
    END
GO

Corrí íntegramente este código final presionando F5 recibiendo la validación pertinente de los comandos. 
[INSERTAR CAPTURA 6: Pantalla con el código ALTER PROCEDURE completo versión IF/ELSE aquí]

7. Demostración y Ejemplos Finales
Para terminar con éxito absoluto esta validación, ejecuté las dos pruebas directas de nuevo.
Primero, ejecuté la orden real validando la protección de nuestro IF condicional:

EXEC PROCE_Total19091113 10248

El sistema generó y arrojó la tabla con su exacto conteo contable de valores.
[INSERTAR CAPTURA 7: Pantalla con resultado positivo del código 10248 aquí]

A continuación, forcé el error con el registro inventado para poner a prueba nuestro ELSE:

EXEC PROCE_Total19091113 10242

En concordancia con nuestra perfecta programación lógica, el sistema me avisó inmediatamente que el registro no era localizable e imprimió textualmente sin generar fallas de compilación ni arrojar tablas con contenido estéril: La orden no existe y el codigo es: 10242.
[INSERTAR CAPTURA 8: Pantalla mostrando el mensaje de error de la orden 10242 demostrando el ELSE aquí]

De esta forma logré cumplir todos los requerimientos y demostraciones pedidas durante estos ejercicios, concluyendo oficialmente la configuración de red y base de datos, la creación de vistas y el manejo seguro y condicionado de los procedimientos almacenados en SQL Server Management Studio. Todo el script de sentencias fue debidamente guardado como Guia10_BasedeDatos19091113.sql.
