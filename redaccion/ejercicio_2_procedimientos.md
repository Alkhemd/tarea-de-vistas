Parte 6: Procedimientos Almacenados (Ejercicio 2)

En este segundo ejercicio de procedimientos almacenados, el objetivo fue crear un script que evaluara una condición específica utilizando la sentencia CASE. El requerimiento consistía en verificar si existían clientes registrados en una ciudad determinada, recibiendo el nombre de la ciudad como parámetro de entrada.

1. Creación del Procedimiento con CASE
Abrí una nueva pestaña de consultas en SSMS y redacté la estructura del procedimiento, nombrándolo sp_Hay_Clientes19091113 para incluir mi número de carnet.

Definí un parámetro de texto @ciudad para recibir el dato que quería evaluar. En el cuerpo del procedimiento, utilicé la sentencia CASE anidada con una consulta SELECT COUNT que contaba cuántos registros en la tabla Customers coincidían con la ciudad recibida:
- Cuando el resultado del conteo era cero (WHEN 0), el procedimiento devolvía el texto 'No hay clientes en la ciudad de ' concatenado con el nombre de la ciudad.
- En cualquier otro caso (ELSE), es decir, cuando el conteo era mayor a cero, devolvía 'Hay clientes en la ciudad de ' junto con el nombre de la ciudad.

El código utilizado fue el siguiente:

CREATE PROCEDURE sp_Hay_Clientes19091113
@ciudad varchar(15)
AS
SELECT
CASE (SELECT COUNT(*) FROM Customers WHERE City=@ciudad)
WHEN 0 THEN 'No hay clientes en la ciudad de ' + @ciudad
ELSE 'Hay clientes en la ciudad de ' + @ciudad
END
GO

Al ejecutar este bloque de código, el servidor me confirmó la creación exitosa del procedimiento almacenado con el mensaje "Comandos completados correctamente".
[INSERTAR CAPTURA 1: Pantalla con el código del procedimiento CASE y el mensaje de comandos completados correctamente abajo aquí]

2. Ejecución y validación del Procedimiento
Para comprobar que la lógica del CASE funcionaba, realicé dos pruebas de ejecución suministrando nombres de ciudades reales al procedimiento.

Primero, ejecuté la prueba consultando por la ciudad de 'Barcelona':
EXEC sp_Hay_Clientes19091113 'Barcelona'

Al correr esta línea, el sistema evaluó la ciudad en la tabla Customers y me devolvió la cadena de texto en la cuadrícula de resultados: 'Hay clientes en la ciudad de Barcelona'.
[INSERTAR CAPTURA 2: Pantalla mostrando la ejecución para Barcelona y el resultado devuelto aquí]

En segundo lugar, probé con la ciudad de 'New York':
EXEC sp_Hay_Clientes19091113 'New York'

Esta vez, el sistema me devolvió la cadena opuesta: 'No hay clientes en la ciudad de New York', demostrando que ingresó en la primera condición del CASE al haber contado exactamente cero registros para esa ciudad en la base de datos.
[INSERTAR CAPTURA 3: Pantalla mostrando la ejecución para New York y el mensaje respectivo aquí]

Estos resultados confirmaron que el uso de la sentencia CASE dentro del procedimiento almacenado me permitía crear validaciones lógicas dinámicas y efectivas dependiendo de los parámetros ingresados.
