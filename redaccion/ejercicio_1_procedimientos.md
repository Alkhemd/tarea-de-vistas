Parte 5: Procedimientos Almacenados (Ejercicio 1)

En esta nueva sección, trabajé con la creación de un Procedimiento Almacenado (Stored Procedure) dentro de la base de datos Northwind. El objetivo de este ejercicio era crear un script temporal inteligente que me permitiera insertar una nueva categoría en la tabla Categories, pero validando primero que tanto el número de ID (Código) como el Nombre de la categoría no existieran previamente.

1. Creación del Procedimiento Almacenado
Para comenzar, abrí una hoja de consultas nueva y desarrollé el código del procedimiento. Utilicé la instrucción CREATE PROCEDURE y lo nombré utilizando mi número de carnet, quedando como sp_Insertar_Categorias19091113.

Dentro de la lógica, definí dos parámetros de entrada: el ID y el NombreCategoria. Luego, implementé una condición IF comprobando con un COUNT que ningún registro en la tabla coincidiera con el ID o el nombre proporcionados. Si la cuenta era cero (no existían), procedía con la inserción (INSERT INTO); de lo contrario, el sistema debía imprimir el mensaje: Error la categoría ya existe.

Al ejecutar el bloque de código completo (F5), el servidor me confirmó que la compilación fue exitosa con el mensaje "Comandos completados correctamente".
[INSERTAR CAPTURA 1: Pantalla con el código del CREATE PROCEDURE y el mensaje de comandos completados correctamente abajo aquí]

2. Ejecución y validación del Procedimiento
Una vez almacenado el procedimiento, pasé a verificar su funcionamiento realizando tres inserciones de prueba individuales (EXEC).

Primero, intenté insertar la categoría 'Alimentos' con el ID 1.
EXEC sp_Insertar_Categorias19091113 1, 'Alimentos'

Al ejecutar esta línea, la pestaña de Mensajes me devolvió exactamente la validación programada: Error la categoría ya existe, ya que el ID 1 ya está ocupado internamente por el sistema original.
[INSERTAR CAPTURA 2: Pantalla mostrando la ejecución del ID 1 y el mensaje de error en rojo aquí]

Segundo, cambié el ID a un número que yo sabía que estaba libre (9) manteniendo el mismo nombre.
EXEC sp_Insertar_Categorias19091113 9, 'Alimentos'

En esta ocasión, el sistema sí me permitió la inserción y me mostró el mensaje regular de que los comandos se habían completado correctamente.
[INSERTAR CAPTURA 3: Pantalla mostrando la ejecución exitosa usando el ID 9 aquí]

Tercero, intenté insertar nuevamente la categoría 'Alimentos', pero ahora asignándole el ID número 10.
EXEC sp_Insertar_Categorias19091113 10, 'Alimentos'

El resultado fue nuevamente: Error la categoría ya existe. Esto comprobó al cien por ciento la robustez de mi procedimiento, ya que, aunque el ID 10 estaba libre, el nombre 'Alimentos' ya había sido registrado en la base de datos durante el paso anterior.
[INSERTAR CAPTURA 4: Pantalla mostrando la ejecución del ID 10 y el mensaje de error por nombre duplicado aquí]

3. Verificación final en la tabla
Como comprobación final dictaminada por el documento, realicé una consulta general (SELECT) a la tabla Categories para ver físicamente que el registro número 9 fue el único que logró insertarse exitosamente. En la cuadrícula de resultados, pude observar todas las ocho categorías originales, y justo al final de la lista de la base de datos, apareció mi nuevo registro integrado correctamente.
[INSERTAR CAPTURA 5: Pantalla mostrando la tabla Categories con el registro número 9 de Alimentos al final aquí]
