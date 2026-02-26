Parte 1: Preparación del Entorno (Base de Datos Northwind)

Para iniciar con esta nueva actividad enfocada en los Disparadores (Triggers), el primer paso de la guía nos pedía utilizar la base de datos Northwind. 

Dado que en las prácticas y ejercicios anteriores ya habíamos realizado la instalación, configuración y restauración completa de esta base de datos en nuestro servidor local de SQL Server, no fue necesario repetir el proceso de creación desde cero ni volver a correr los scripts de instalación. 

Simplemente abrí SQL Server Management Studio (SSMS), me conecté a mi instancia, y seleccioné la base de datos Northwind para dejarla activa en mi hoja de consultas y estar completamente lista para comenzar a programar los procedimientos y disparadores requeridos en los siguientes pasos.

Parte 2: Implementación de estructuras repetitivas en un procedimiento almacenado

Para esta segunda fase, el objetivo era implementar la estructura de control repetitiva WHILE dentro de un procedimiento almacenado. El manual me pedía utilizar mi propio número de carnet en cada objeto o script que creara, por lo cual utilicé el número "19091113".

1. Crear un procedimiento para mostrar 10 pedidos consecutivos
Comencé abriendo una nueva ventana de consultas y redactando la instrucción lógica del objeto CREATE PROCEDURE nombrándolo Mostrar_10_pedidos_19091113. 

La lógica de la repetición empleaba la declaración de dos variables numéricas enteras: @contador y @num. A la primera variable (@contador) le asigne el valor inicial de cero, mientras que a la segunda (@num) le inyecté de forma dinámica el primer valor existente rastreable en el campo OrderID de la tabla Orders, recurriendo a una subconsulta TOP 1.
Posteriormente, desarrollamos el núcleo: el bucle WHILE condicionado a ejecutarse estrictamente cuando el @contador resultara menor a 10. Dentro del ciclo, utilicé un SELECT que exhibiera en consola los datos "OrderID" y "OrderDate", apoyado matemáticamente de una variante WHERE en el que la búsqueda de la orden base (@num) fuera sumada progresivamente al valor actual de nuestro contador (@num+@contador). Finalmente, el mismo bucle debía escalar el valor actual del contador gradualmente para no generar un salto infinito (SET @contador=@contador+1).

El código exacto aplicado se muestra a continuación:

CREATE PROCEDURE Mostrar_10_pedidos_19091113
AS
DECLARE @contador int
DECLARE @num int

SET @contador=0
--Obteniendo el primer valor del campo OrderID de la tabla Orders
SET @num=(SELECT TOP 1 OrderID FROM Orders ORDER BY OrderID)

--Evalua si el contador es menor que 10, si la condicion se cumple
--realiza la instruccion SELECT
WHILE @contador<10
BEGIN
	SELECT OrderID, OrderDate FROM Orders WHERE OrderID=@num+@contador
	--Se incrementa el contador
	SET @contador=@contador+1
END
GO

Inmediatamente, lo ejecuté garantizando compilar todo el bloque con el mensaje respectivo "Comandos completados correctamente".
[INSERTAR CAPTURA 1: Pantalla con el bloque CREATE PROCEDURE del ciclo WHILE de pedidos y el mensaje de satisfacción aquí]

2. Ejecutar y testear el ciclo automático
Llegó el turno de poner en evaluación práctica mi nuevo script. Procedí utilizando el comando regular:
EXEC Mostrar_10_pedidos_19091113

El entorno reaccionó imprimiendo 10 iteraciones corridas en ventanas tabulares una tras la otra (result sets), comprobando cabalmente la funcionalidad progresiva de mi variable incremental donde cada llamado del ciclo localizó consecutivamente un nuevo y diferente pedido en Northwind sin mi intervención física.
[INSERTAR CAPTURA 2: Pantalla mostrando la cláusula de invocación EXEC y diferentes recuadros tabulares con resultados en secuencia aquí]

3. Actualización Condicional Aritmética Dinámica
El quinto paso oficial involucraba dar un nivel mayor de madurez programática aplicando una reescritura arriesgada y un procedimiento lógico "BREAK/CONTINUE" a la base de datos controlada por la estructura WHILE. 

Confeccioné localmente un script llamándolo Actualizar_precio_19091113. Su bucle iterativo quedó vinculado mediante funciones de agrupación matemática exigiendo continuar su trabajo a perpetuidad mientras que el AVG (Precio Promedio de venta) de todos y cada uno de los productos de la tabla Products resultara ser menor a un tope natural de $300 absolutos.

Dentro del vientre del ciclo, la instrucción primaria radicó en inyectar un UPDATE a gran escala alterando dramáticamente UnitPrice y re-asignándole artificialmente un doble exacto a su valor actual (UnitPrice = UnitPrice * 2), informando del progreso de esto vía un SELECT de supervisión (Precio Máximo de la tabla temporal). 
Junto a eso, se aplicaba una condición IF rigurosa evaluando ese SELECT de supervisión: si tras duplicar los precios se detectaba un máximo local por debajo de $500 continuaríamos con normalidad omitiendo restricciones (CONTINUE). Si esta agresiva inflación topaba por encima o rebasaba la métrica superior a los 500, un freno de emergencia detendría y extinguiría todo este bloque en acción a través del comando lógico BREAK dando por finalizado el objetivo del bucle.

CREATE PROCEDURE Actualizar_precio_19091113
AS
	WHILE (SELECT AVG(UnitPrice) FROM Products) < 300
	BEGIN
		UPDATE Products
		SET UnitPrice = UnitPrice * 2
		SELECT MAX(UnitPrice) AS [Precio Máximo] FROM Products
		IF (SELECT MAX(UnitPrice) FROM Products) < 500
			BREAK
		ELSE
			CONTINUE
	END
GO

Creé el procedimiento exitosamente, obteniendo validación del sistema de que quedó guardado.
[INSERTAR CAPTURA 3: Pantalla con la estructura WHILE interior (Update/Break) y generación del Action aquí]

4. Evaluar la tabla Products antes y después de la inflación
Reconociendo que la operación anterior tenía la capacidad de sobreescribir permanentemente datos económicos de nuestra tabla central, procedí con cautela a realizar el paso 6 y 7 de nuestra guía verificando los precios máximos actuales de nuestros productos antes de detonar el procedimiento.

Utilicé una consulta sencilla para generar el reporte de precios actuales y asegurarme de que mi base registrara UnitPrice ordenados desde el más caro hacia abajo:

SELECT UnitPrice FROM Products
ORDER BY UnitPrice DESC

Obtuve como resultado en pantalla una columna tabular que iniciaba con un artículo en $263.50 descendiendo paulatinamente, certificando el estado normal de los precios antes de alterarlos.
[INSERTAR CAPTURA 4: Pantalla mostrando la cuadrícula de respuesta al SELECT ordenada de manera descendente aquí]

5. Ejecutar la Rutina de Incremento de Precios (Bucle)
El paso 8 indicaba activar el gatillo (ejecución) de nuestro procedimiento recientemente programado.
Al ejecutar la instrucción "EXECUTE Actualizar_precio_19091113", el SQL Server Management Studio procesó silenciosamente el bucle WHILE iterando sobre cada evaluación.

Al forzar un barrido revisor repitiendo la instrucción de verificación de pasos previos (SELECT UnitPrice FROM Products ORDER BY UnitPrice DESC) para cumplir con el paso 9, noté en mi terminal un cambio rotundo. El primer registro encarecido escaló matemáticamente a la suma de $527.00, con los subsecuentes en la lista ajustados. Quedó documentado que la instrucción WHILE fue funcional a nivel estructura.
[INSERTAR CAPTURA 5: Pantalla mostrando la diferencia tarifaria con la cuadrícula de respuesta reflejando los nuevos super precios escalados aquí]

6. Re-configurando el Umbral del Bucle
Dado a que en el ejercicio logramos inflar los precios al doble de una sola pasada y el bucle interno descubrió que la tarifa máxima ya rebasaba los 500 topando los más de $527, las instrucciones posteriores se anulaban instantáneamente (BREAK).
Para continuar iterando los precios en múltiplos mayores, el inciso 9 sugería reajustar los límites de seguridad programados dentro del código principal (Procedimiento Almacenado de Actualizar Precios) vía la edición del Script "ALTER PROCEDURE".

Reemplacé de mi hoja todo el código y ajusté deliberadamente:
- El umbral primario (WHILE) determinando que correría si el Promedio General de todo (AVG) era menor de 500 (antes 300).
- La condición de apagado secundaria (IF) ampliándola a que el bucle rompería la orden sólo si detectaba que el Precio Máximo sobrepasaba la dramática cifra de los 3000 dólares (antes 500).

El script resultante bajo la modificación fue:

ALTER PROCEDURE Actualizar_precio_19091113
AS
	WHILE (SELECT AVG(UnitPrice) FROM Products) < 500
	BEGIN
		UPDATE Products
		SET UnitPrice = UnitPrice * 2
		SELECT MAX(UnitPrice) AS [Precio Máximo] FROM Products
		IF (SELECT MAX(UnitPrice) FROM Products) < 3000
			BREAK
		ELSE
			CONTINUE
	END
GO

Bajo este nuevo horizonte operativo, la rutina quedó actualizada permanentemente.
[INSERTAR CAPTURA 6: Pantalla mostrando la redacción de la versión ALTER incrementada a 500 y 3000 con el mensaje validado aquí]

7. Segunda y última iteración de Encarecimiento
Para culminar esta etapa como lo requerían los estatutos del paso 10, ejecuté por segunda ocasión la rutina global para evaluar su desempeño en entornos saturados de alta inflación simulada.

EXECUTE Actualizar_precio_19091113

Generé el reporte SELECT final y divisé que la aplicación forzó un siguiente pase total, posicionando la tarifa pico muy por encima de los límites anteriores, de manera documentada sin causar fallos de sistema.
[INSERTAR CAPTURA 7: Pantalla enseñando el resultado tras el segundo pase ejecutado exitosamente aquí]

Parte 3: Implementación de cursores en procedimientos almacenados

Concluida la etapa iterativa escalar, el apartado 11 de la guía me instruyó a realizar un ejercicio especial para aprender a manejar "Cursores". A diferencia de los comandos SELECT ordinarios que muestran todos los datos de golpe en una cuadrícula general, los cursores permiten leer y manipular una tabla registro por registro (fila por fila), de manera similar a como lo haría un lenguaje de programación de muy bajo nivel.

1. Crear el procedimiento integrando un Cursor
Mi objetivo general fue crear un listado impreso vía consola donde apareciera texto predeterminado concatenado con el nombre de cada uno de los integrantes de la tabla "Customers" de manera individualizada.

Creé un nuevo procedimiento con mi identificador: CREATE PROC Mostrar_Clientes_19091113. 

Su núcleo operativo requirió de la configuración estricta de variables e instrucciones paso a paso:
- Primero, declaré la variable estándar "@Nombre" como alojamiento de texto puro (NVARCHAR) para recibir los datos comerciales de la columna objetivo.
- Segundo, declaré obligatoriamente la variable abstracta "@cursor" y la castifiqué estrictamente como tipo "CURSOR". Esta variable funcionará como la aguja lectora del disco.
- Tercero, le inyecté a dicha aguja lectora el encargo de toda la tabla mediante el comando: SET @cursor = CURSOR FOR SELECT CompanyName FROM Customers. 
- Cuarto, abrí y activé la lectura con la orden "OPEN @cursor", e inmediatamente forcé a la misma a atrapar el primer nombre de la lista e insertarlo en mi variable de texto local a través del comando "FETCH NEXT FROM @cursor INTO @Nombre".
- Quinto, inicié un bucle condicional usando la variable global "@@FETCH_STATUS = 0" (cuyo significado es que mientras el cursor detecte una línea válida, el bucle repite su lectura). Dentro de sus márgenes "BEGIN y END", ordené la impresión individualizada del registro guardado ("PRINT 'El nombre del cliente es: ' + @Nombre") e instuí dar un salto inmediato para atrapar el nombre de la línea inferior ("FETCH NEXT FROM...").
- Como sexto y último paso crucial, por temas de no causar fallas fatales de consumo de memoria al servidor, decreté oficialmente el apagado ("CLOSE @cursor") y la exterminación absoluta del objeto lector ("DEALLOCATE @cursor").

El código quedó conformado de la siguiente manera:

CREATE PROC Mostrar_Clientes_19091113
AS
	DECLARE @Nombre NVARCHAR(40)
	--Se declara el cursor @cursor, el cual se utilizara para recorrer
	--cada resultado de la consulta SELECT
	DECLARE @cursor CURSOR

	--Se asigna el primer dato al cursor
	SET @cursor = CURSOR FOR 
	SELECT CompanyName FROM Customers
	--Abrir el cursor
	OPEN @cursor
	--Recupera las filas del cursor
	FETCH NEXT
	FROM @cursor INTO @Nombre
	WHILE @@FETCH_STATUS = 0
	BEGIN
		PRINT 'El nombre del cliente es: ' + @Nombre
		--Se mueve al siguiente registro
		FETCH NEXT FROM @cursor INTO @Nombre
	END
	--Este comando hace desaparecer el puntero sobre el registro actual
	CLOSE @cursor
	DEALLOCATE @cursor
GO

La compilación al darle compilar al servidor reportó éxito completo en la sintaxis.
[INSERTAR CAPTURA 8: Pantalla completa desde la creación del cursor hasta su deallocate y el mensaje de sistema de los comandos completados aquí]

2. Ejecutar y testear el comportamiento del Cursor
De cara a realizar la verificación y siguiendo lo descrito en el punto 12 de mi guía de proyecto, eliminé las líneas de programación en mi terminal e inserté mi llamado de ejecución formal:

--Ejecutamos el procedimiento almacenado
EXEC Mostrar_Clientes_19091113

A escasos milisegundos de correr dicha expresión, el motor interno del cursor iteró silenciosamente logrando desplegar exitosamente una inmensa cascada vertical de texto, la cual desglosaba de manera prístina una cadena imprimible por cada usuario extraído, ratificando un éxito absoluto en la prueba del manejo de lecturas unitarias a través de objetos cursores.
[INSERTAR CAPTURA 9: Pantalla registrando en la pestaña mensajes cómo se despliega la cascada de impresión con los nombres de todos y cada uno de los clientes en orden vertical aquí]
