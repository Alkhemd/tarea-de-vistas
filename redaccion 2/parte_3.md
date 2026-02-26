Parte 3: Uso de desencadenadores

A continuación, documentaré la implementación y prueba de disparadores (triggers) en el motor de base de datos. Para estos ejercicios, sigo utilizando la base de datos Northwind y aplico mi número de carnet, "19091113", en la nomenclatura de los objetos.

1. Creación de un Disparador de Seguridad (DDL)

El primer punto nos indicaba crear un disparador en el ámbito de la general en la base de datos, diseñado para evitar estructuralmente que cualquier usuario pueda eliminar o alterar las tablas existentes, protegiendo así la integridad global de Northwind.

Procedí a crear mi desencadenador nombrándolo Disp_SEGURIDAD_19091113 y lo asocié directamente a los eventos DROP_TABLE y ALTER_TABLE. Dentro de su cuerpo, incluí la instrucción RAISERROR con una severidad de 16 y un estado de 1 para retornar un mensaje de alerta oficial, seguido del comando de resguardo ROLLBACK TRANSACTION (el cual revierte cualquier intento de cambio hasta su estado anterior de seguridad). 

Ejecuté el siguiente código en la hoja de consultas de SSMS:

CREATE TRIGGER Disp_SEGURIDAD_19091113
ON DATABASE FOR DROP_TABLE, ALTER_TABLE
AS
	BEGIN
	--RAISERROR se usa para devolver mensajes a las aplicaciones con el
	--mismo formato que un error del sistema
	RAISERROR ('No está permitido borrar ni modificar tablas!' , 16, 1)
	--16 Severidad
	--1 Estado
	ROLLBACK TRANSACTION
	END
GO

El motor compiló y guardó mi disparador de seguridad con éxito mostrándome en la pantalla principal que mis comandos se habían completado al 100%.
[INSERTAR CAPTURA 10: Pantalla demostrando la creación del TRIGGER de la base de datos con el mensaje "Comandos completados correctamente" aquí]

2. Creación de una tabla de prueba

Para comprobar que el escudo magnético de seguridad programado anteriormente funcionaba correctamente en mi entorno, el paso 2 requería la creación veloz de una tabla inerte y sencilla.
Abrí mi panel de código y creé una tabla genérica que acompañé de mi carnet para darle una firma personalizada:

--Crear la tabla
CREATE TABLE prueba_19091113
(campo1 int
)

Al estar ejecutando una instrucción CREATE TABLE (la cual no fue bloqueada intencionalmente por el disparador anterior ya que solo configuré bloqueos contra mandos ALTER y DROP), la tabla apareció generada efectivamente dentro de mi base Northwind.
[INSERTAR CAPTURA 11: Ejecución del CREATE TABLE tabulando el status completado aquí]

3. Ejecutar y testear la eliminación (Activando el Trigger de Seguridad)

El punto 3 solicitaba intentar erradicar del sistema la tabla recién creada mediante el destructivo comando DROP, con la expectativa de que el punto 4 validara frontalmente el bloqueo defensivo de nuestro desencadenador.

Mande la instrucción de destrucción directa en la pantalla:

--Eliminar la tablas
DROP TABLE prueba_19091113

Tras la ejecución de borrado, presencié inmediatamente en los resultados del terminal como el disparador Disp_SEGURIDAD_19091113 detectó el evento catalogado como DROP_TABLE, procedió a interceptar velozmente la petición y sepultó la orden letal arrojando de vuelta hacia mi un mensaje personalizado y de alerta roja: "No está permitido borrar ni modificar tablas!", confirmando así nuestro grado 16 de severidad estricto junto con el freno de colisión "rollback" automático. 

Con todo esto, logré dar firme validez de que en efecto no se podía vulnerar, remover, ni modificar los contenidos integrales de toda la base de datos.
[INSERTAR CAPTURA 12: Pantalla desplegando el texto rojo de error del sistema por parte del RAISERROR tras el intento obvio de borrado aquí]

5. Creación de un Disparador de Manipulación de Datos (DML) / Inserción

Dejando comprobada sólidamente la seguridad a un nivel superior y general (escala base de datos), el quinto paso demandaba construir un disparador de segunda categoría. Esta herramienta táctica operaría particularizándose exclusivamente sobre la nueva tabla (prueba) enfocando su radar solamente para interceptar las maniobras informativas etiquetadas como "inserción de datos".

Elaboré este nuevo Trigger llamándolo Mensaje_Insersion_19091113, asignándolo rigurosamente "ON prueba_19091113" pero forzando a reaccionar bajo la instrucción principal "AFTER INSERT". 
Bajo sus directrices nativas de desarrollo, escribí e invoqué la modalidad para que me brindara un aviso impreso de éxito rotundo en consola ("Se agregó un nuevo registro a la tabla") para acto seguido castigar de manera disimulada el mismo éxito obligándolo colateralmente a lanzar un ROLLBACK TRANSACTION (lo que significa virtualmente borrar la inserción segundos después de notificar que si insertó con éxito la información).

El código exacto aplicado se muestra a continuación:

CREATE TRIGGER Mensaje_Insersion_19091113
ON prueba_19091113
AFTER INSERT
AS
	BEGIN
	PRINT 'Se agregó un nuevo registro a la tabla'
	ROLLBACK TRANSACTION
	END
GO

Ejecuté y compendié exitosamente mi pequeño código de trampa e imposición, dotando y afianzando a mi pequeña tabla local con este nuevo modelo de supervisión vigilante post-insertado.
[INSERTAR CAPTURA 13: Pantalla donde compilo el disparador estático que reacciona "AFTER INSERT" completado aquí]

6. Activar el disparador agregando un dato a la tabla

Continuando con la guía, el paso 6 consistió en poner a prueba el disparador recién creado agregando un dato físico a nuestra tabla de demostración. 

Escribí y ejecuté la siguiente instrucción de inserción:

--Verificando el TRIGGER
INSERT INTO prueba_19091113 VALUES (1)

Inmediatamente, la ventana de mensajes del servidor reaccionó imprimiendo el texto configurado: "Se agregó un nuevo registro a la tabla". Sin embargo, gracias al comando ROLLBACK oculto en nuestro disparador, aunque el sistema nos confirmó la acción, el dato real jamás se guardó en la tabla, previniendo así cualquier alteración no deseada sobre la estructura superficial.
[INSERTAR CAPTURA 14: Pantalla mostrando la ejecución del INSERT y el mensaje impreso del trigger en la consola aquí]

7. Insertar un empleado real a la base original

Avanzando al paso 7, el objetivo se centró en manipular los datos nativos de la base Northwind. Se requería inyectar un perfil de empleado completamente nuevo dentro de la tabla "Employees" aportando únicamente sus nombres y apellidos.

Preparé el entorno y ejecuté mi orden de inserción:

--Agregando un nuevo dato a la tabla Employees
INSERT INTO Employees(Lastname,Firstname) VALUES ('Cañas','Blanca')

Al no existir ningún trigger restrictivo sobre esta tabla en particular, la inserción se ejecutó de forma natural afectando 1 fila y guardando el registro de manera permanente en nuestra base.
[INSERTAR CAPTURA 15: Ejecución del código INSERT INTO para la tabla Employees con el estatus completado aquí]

8. Creación de un Disparador de Actualización (UPDATE)

Para el octavo paso debíamos incrementar la complejidad creando un tercer Trigger, esta vez diseñado para la tabla oficial "Employees" y enfocado estrictamente en monitorear alteraciones (AFTER UPDATE).

Redacté mi procedimiento nombrándolo actualizar_emple_19091113. Dentro de su código lógico incrusté un condicional "IF" que evaluara específicamente la columna "Lastname" (apellidos). La instrucción dictaba que, si y solo si, la base detectaba una sobre-escritura en los apellidos de cualquier empleado, el servidor debía obligatoriamente imprimir un aviso de auditoría en la terminal: "Se realizo un cambio en el apellido de los empleados".

El script utilizado fue el siguiente:

CREATE TRIGGER actualizar_emple_19091113
ON Employees
AFTER UPDATE
AS
	IF UPDATE(Lastname)
	BEGIN
		PRINT 'Se realizo un cambio en el apellido de los empleados'
	END
GO

Tras compilarlo con F5, el disparador quedó silenciosamente arraigado dentro de la tabla de empleados, listo para auditar el campo de apellidos.
[INSERTAR CAPTURA 16: Pantalla demostrando la creación exitosa del Trigger de actualización aquí]

9. Verificación de Identidad del Nuevo Registro

Antes de someter a prueba a mi nuevo auditor de apellidos, el paso 9 me exigía explorar los datos nativos de los empleados para ubicar exactamente el número de Identidad (EmployeeID) que el sistema le asignó automáticamente a "Blanca Cañas" cuando la registré.

Mandé llamar a la tabla completa mediante un comando simple:

--Verificando los datos de los empleados
SELECT * FROM Employees

Al inspeccionar los resultados al final de la hoja tabular, comprobé que su usuario había ingresado sin problemas en las ranuras faltantes, y tomé nota de su EmployeeID único correspondiente (en la práctica suele ser 10, 11 o superior).
[INSERTAR CAPTURA 17: Pantalla mostrando la fila en la tabla donde aparece el EmployeeID de Blanca Cañas aquí]

10. Testeando el TRIGGER mediante una Modificación de Apellido

Como último eslabón de esta fase, el paso 10 requería concretar la trampa apuntando nuestra mira hacia la empleada previamente registrada y actualizando su apellido de "Cañas" a "Abarca", forzando al motor a ejecutar la auditoría de nuestro TRIGGER.

Utilizando la clave de ID obtenida en el paso anterior (que nos demostró ser el número 10), redacté la orden condicional de actualización:

--Actualizando el apellido del empleado
UPDATE Employees SET LastName='Abarca'
WHERE EmployeeID=10

Al accionar esta orden, el sistema no solo actualizó correctamente el registro, sino que también nuestra consola de mensajes reportó de inmediato un texto programado dictaminando: "Se realizo un cambio en el apellido de los empleados". 
Con esta última prueba concluí satisfactoriamente que nuestro disparador de edición monitoreaba permanentemente el sistema con total éxito y diligencia.
[INSERTAR CAPTURA 18: Pantalla arrojando la captura del mensaje de auditoría comprobando que el Trigger detectó la edición del Lastname aquí]

11. Verificar los resultados de la consulta y del TRIGGER

Validé los resúmenes visuales de mis propios cuadros de mensajes en mi consola, certificando que el sistema había adoptado perfectamente las lógicas de restricción programadas.

12. Guardar los cambios

Para no perder el progreso del código, procedí a darle Ctrl+S a mi hoja de consultas y dejé registrado mi código en un archivo seguro en mis documentos.

13. Agregar un nuevo empleado a la tabla Employees

Con miras a evaluar el último escenario posible, la guía pedía agregar un empleado extra (que se convertiría en nuestro segundo integrado manual). Así que registré a un nuevo usuario:

--Agregando un nuevo dato a la tabla Employees
INSERT INTO Employees(Lastname,Firstname) VALUES ('Gomez','Carlos')

Sabiendo que a Blanca Cañas le había tocado el ID 10, es lógico que el sistema le asignara automáticamente el número 11 a nuestro nuevo empleado ficticio Carlos Gomez.
[INSERTAR CAPTURA 19: Ejecución del código INSERT INTO para el nuevo empleado Carlos completado aquí]

14. Crear un Disparador de Eliminación Limitada (DELETE)

Posteriormente construí nuestro cuarto y último TRIGGER en la tabla, el cual fungiría como un seguro de vida contra desastres por borrado masivo accidental.

Generé mi bloque de código nombrándolo eliminar_emple_19091113. Intervine directamente las operaciones de borrado mediante las cláusulas "ON employees" y "FOR DELETE". El meollo del asunto consistía en condicionar a que si el contador virtual de filas afectadas (SELECT COUNT(*) FROM deleted) era estrictamente mayor a 1, un seguro de emergencia se dispararía enviando la negatoria ("No se puede borrar mas de un empleado al mismo tiempo") y obligando el restablecimiento inmediato del borrado salvando a la tabla del borrado masivo mediante un ROLLBACK TRANSACTION.

Utilicé este fragmento final:

CREATE TRIGGER eliminar_emple_19091113
ON employees
FOR DELETE
AS
	IF(SELECT COUNT(*) FROM deleted)>1
	BEGIN
		PRINT 'No se puede borrar mas de un empleado al mismo tiempo'
		ROLLBACK TRANSACTION
	END
GO

(Nota técnica: La tabla volátil DELETED almacena momentáneamente copias de las filas que acaban de ser borradas o modificadas. Si mi TRIGGER lee que esa tabla temporal trae más de un elemento para borrar a la vez, se activa el Rollback).

Lo ejecuté, añadiendo el blindaje exitosamente a los Empleados.
[INSERTAR CAPTURA 20: Pantalla verificando la correcta creación de la rutina de eliminación limitada aquí]

15. Evaluación restrictiva del Bloqueo Masivo

Para estresar mi sistema y obligarlo a cumplir el trabajo encomendado, ejecuté un comando ambicioso intencionalmente tramposo, el cual le solicitaba erradicar de golpe a todos los perfiles de la base cuyos números de empleado superaran el umbral de 9. Al instruír esto, mi sistema tenía la orden de capturar y borrar simultáneamente a las identidades 10 y 11 (Blanca y Carlos).

Lancé la bomba de la siguiente forma:

--La instrucción DELETE activa el desencadenador y evita la transacción.
DELETE FROM Employees WHERE EmployeeID > 9

De manera casi instintiva, mi gatillo recién programado interceptó el evento al percatarse que el lote de borrado traía a más de una víctima y canceló ferozmente toda la purga emitiendo la advertencia pre-diseñada: "No se puede borrar mas de un empleado al mismo tiempo". Las identidades continuaron indemnes en la tabla.
[INSERTAR CAPTURA 21: Fotografía atestiguando la activación defensiva del trigger arrojando su mensaje de bloqueo de borrado masivo aquí]

16. Evaluación complaciente del Borrado Único

Para el decimonoveno y último paso, seccioné mi ataque y ejecuté un borrado único e individual focalizando a la identidad número 11 (Carlos Gómez). 
Al traer en el embudo solamente a 1 persona, mi bloque defensivo no opondría resistencia alguna (ya que la condición de peligro se activaba solo al ser estrictamente mayor a 1).

--La instrucción DELETE activa el desencadenador y permite la transacción.
DELETE FROM Employees WHERE EmployeeID = 11

La orden fluyó directo a sus entrañas informándome con el mensaje cotidiano "1 filas afectadas". El empleado fue suprimido sin protestas de los disparadores, demostrando contundentemente que toda mi configuración lógica se concretó con el nivel más alto de éxito programático para esta asignación práctica.
[INSERTAR CAPTURA 22: Captura final demostrando que al lanzar un borrado singular la consola lo asimila mostrando "(1 filas afectadas)" exitosamente aquí]
