Actividad Final

1. Ejercicio complementario

En un nuevo Script, se procede a crear el ejercicio complementario, colocando debidamente como nombre al archivo: Guia11_EjercicioComplementario_SuCarnet.sql

[ESPACIO PARA CAPTURA DEMOSTRANDO EL NOMBRE DEL ARCHIVO EN SSMS]


2. Crear la base de datos

A continuacion, se procede a crear y utilizar la base de datos copiando y ejecutando el siguiente codigo:

--Crear la base de datos
CREATE DATABASE Control_Examen_SuCarnet
GO

--Hacer uso de la base de datos
USE Control_Examen_SuCarnet
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO QUE LOS COMANDOS SE COMPLETARON CON EXITO]


3. Crear las tablas

Habiendo seleccionado la base de datos, se procede a crear las siguientes tablas copiando de igual forma el codigo otorgado:

CREATE TABLE Alumno
(
	Carnet VARCHAR(10) NOT NULL PRIMARY KEY,
	Nombre VARCHAR(30) NOT NULL,
	Apellido1 VARCHAR(30) NOT NULL,
	Apellido2 VARCHAR(30) NOT NULL,
	Sexo VARCHAR(2) NOT NULL
)
GO

CREATE TABLE Examenes
(
	cod_examen INT NOT NULL PRIMARY KEY,
	titulo VARCHAR(100) NOT NULL,
	n_preguntas INT NOT NULL
)
GO

CREATE TABLE Notas
(
	cod_examen INT NOT NULL FOREIGN KEY REFERENCES Examenes(cod_examen),
	Carnet VARCHAR(10) NOT NULL FOREIGN KEY REFERENCES Alumno(Carnet),
	nota_examen DECIMAL(4,2) NOT NULL,
	fecha SMALLDATETIME NOT NULL
)
GO

CREATE TABLE Respuestas
(
	cod_examen INT NOT NULL FOREIGN KEY REFERENCES Examenes(cod_examen),
	Carnet VARCHAR(10) NOT NULL FOREIGN KEY REFERENCES Alumno(Carnet),
	n_pregunta INT NOT NULL,
	opcion_respuesta INT NOT NULL
)
GO

CREATE TABLE Preguntas
(
	cod_examen INT NOT NULL FOREIGN KEY REFERENCES Examenes(cod_examen),
	n_pregunta INT NOT NULL,
	texto_pregunta VARCHAR(100) NOT NULL,
	n_opciones INT NOT NULL,
	opcion_correcta INT NOT NULL
)
GO

CREATE TABLE Opciones
(
	cod_examen INT NOT NULL FOREIGN KEY REFERENCES Examenes(cod_examen),
	n_pregunta INT NOT NULL,
	n_opcion INT NOT NULL,
	texto_opcion VARCHAR(50) NULL
)
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO QUE TODAS LAS TABLAS FUERON CREADAS SATISFACTORIAMENTE EN LA BD]


4. Crear el diagrama de la base de datos

Habiendo establecido la estructura completa de las tablas, se procede a generar el diagrama visual de la base de datos para confirmar que todas las relaciones y llaves foraneas se hayan aplicado correctamente entre las entidades Alumno, Examenes, Notas, Respuestas, Preguntas y Opciones.

[ESPACIO PARA CAPTURA DEMOSTRANDO EL DIAGRAMA RELACIONAL GRAFICO DE LA BASE DE DATOS]


5. Insertando Datos a partir de Procedimientos Almacenados

6. Para insertar datos en la tabla Alumno, se procede a crear un procedimiento llamado Insert_Alumno, tal como se muestra a continuacion:

CREATE PROCEDURE Insert_Alumno
@carnet CHAR(8), @nombre CHAR(15), @apell1 CHAR(15), @apell2 CHAR(15), @sexo CHAR(1)
AS
	INSERT INTO Alumno VALUES ( @carnet, @nombre, @apell1, @apell2, @sexo)
	return (0)
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION EXITOSA DEL PROCEDIMIENTO INSERT_ALUMNO]


7. Se procede a utilizar el procedimiento almacenado creado anteriormente para insertar los siguientes datos:

EXEC Insert_Alumno 'SB970326', 'Alicia', 'Salinas', 'Benitez', 'F'
EXEC Insert_Alumno 'SC970245', 'Pedro', 'Salazar', 'Calderon', 'M'
EXEC Insert_Alumno 'RC970201', 'Karla', 'Ramirez', 'Chicas', 'F'

[ESPACIO PARA CAPTURA DEMOSTRANDO QUE LOS TRES REGISTROS SE INSERTARON CORRECTAMENTE]


8. Se insertaran datos en las tablas Examenes, Preguntas y Opciones en base al siguiente formato de examen:

Codigo del Examen: 1
Titulo: Conceptos de Procedimientos Almacenados
No. de Preguntas: 2

1. Con que sentencia se crea un procedimiento almacenado:
   1. CREATE PROCEDURE
   2. CREATE PROC
   3. Ambas
   (Numero de opcion correcta: 3)

2. Con que comando se ejecuta un procedimiento almacenado:
   1. EXECUTE
   2. sp_executesql
   (Numero de opcion correcta: 1)


9. Crear el siguiente procedimiento almacenado para llenar los datos en la tabla Examenes:

CREATE PROC Insert_Exa
@cod INT,
@title VARCHAR(100),
@NPreg INT
AS
	INSERT Examenes VALUES(@cod, @title, @NPreg)
	return(0)
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION EXITOSA DEL PROCEDIMIENTO INSERT_EXA]


10. Agregar los siguientes registros a la tabla Examenes por medio del procedimiento almacenado

--Agregando Datos del Examen
EXEC Insert_Exa 1,'Conceptos de Procedimientos Almacenados',2
EXEC Insert_Exa 2,'Conceptos sobre Redes de Area Local',3

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION EXITOSA EN LA TABLA EXAMENES]


11. Por medio de instruccion INSERT agregar los siguientes registros a la tabla Preguntas

--Agregando datos a preguntas
INSERT INTO Preguntas VALUES (1,1,'Con que sentencia se crea un procedimiento almacenado:',3,3)
INSERT INTO Preguntas VALUES (1,2,'Con que comando se ejecuta un procedimiento almacenado:',2,1)

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION EXITOSA EN LA TABLA PREGUNTAS]


12. Agregando registros a la tabla Opciones por medio de la instruccion INSERT

--Agregando datos a opciones
INSERT INTO Opciones VALUES (1,1,1,'Create Procedure')
INSERT INTO Opciones VALUES (1,1,2,'Create Proc')
INSERT INTO Opciones VALUES (1,1,3,'Ambas opciones')
INSERT INTO Opciones VALUES (1,2,1,'EXECUTE')
INSERT INTO Opciones VALUES (1,2,2,'sp_executesql')

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION EXITOSA EN LA TABLA OPCIONES]


Mensajes de Error definidos por el usuario

Antes de insertar datos en la tabla de Respuestas, se creara un TRIGGER que despliegue un mensaje de advertencia cuando el alumno este contestando la ultima pregunta del examen (en este caso seria la pregunta 2).

Para ello crear primero un mensaje definido por el usuario, este mensaje es anadido a la tabla sysmessages (sysmessages se encuentra en la base de datos master) utilizando el procedimiento almacenado del sistema sp_addmessage.

Para crear el mensaje debe especificarse el numero del mensaje, el tipo o nivel de severidad y el texto del mensaje. (Los mensajes de error deben tener un numero arriba de 50000, pues los otros numeros son reservados por SQL Server).


13. Crear el siguiente mensaje:

--Creando los mensajes o verificar si ya existe el mensaje
USE master
GO
EXEC sp_addmessage 50001, 16, N'Usted se encuentra en la ultima pregunta del examen'

Si al ejecutar la consulta anterior le aparece el siguiente mensaje de error:

Msg 15043, Level 16, State 1, Procedure sp_addmessage, Line 133
You must specify 'REPLACE' to overwrite an existing message.

Significa que el numero del mensaje 50001 ya existe en la base de datos master, asi que cambie el numero por uno consecutivo (por ejemplo 50002)

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION DEL MENSAJE 50001 EN MASTER]


14. Hacer uso de nuevo de la base de datos Control_examen_SuCarnet y creamos un procedimiento almacenado el cual va a realizar la operacion de calcular la nota del alumno despues de haber contestado el examen

USE Control_Examen_SuCarnet
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO EL USO DE LA BASE DE DATOS]


15. Antes de crear el procedimiento almacenado, se verifica la existencia de este en la base de datos, si el procedimiento existe se elimina y sino no se realiza ninguna accion

--Verificar si existe el procedimiento y si este existe se elimina
IF EXISTS (SELECT name FROM sysobjects
	WHERE name = 'Calculo_de_Nota' AND type = 'P')
	DROP PROCEDURE Calculo_de_Nota
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA VERIFICACION Y ELIMINACION DEL PROCEDIMIENTO]


16. Crear el siguiente procedimiento almacenado

--Creacion del procedimiento
CREATE PROC Calculo_de_Nota
@carnetx VARCHAR(10), @codexamen INT
AS
BEGIN
DECLARE @totcorrecta INT, --contador para respuestas correctas
	@porcentaje FLOAT, --porcentaje de cada pregunta
	@npreguntas INT, --numero de preguntas del examen
	@correctaresp INT, --captura la opcion correcta del examen(opc_correcta)
	@respuesta INT, --captura la opcion que selecciono el alumno (opcion_respuesta)
	@perfect FLOAT, --constante = 10
	@NOTA FLOAT--nota del examen

--Verifica la cantidad de preguntas que tiene el examen
SELECT @npreguntas= CONVERT(FLOAT,COUNT(n_pregunta))FROM Preguntas
WHERE cod_examen=@codexamen
SELECT @perfect = 10.00 --Asignar un dato a una variable
SELECT @porcentaje= @perfect/@npreguntas --calculo de porcentaje para c/pregunta
SELECT @totcorrecta=0 --se inicializa un contador de respuestas correctas a 0

WHILE(@npreguntas > 0)
BEGIN
	--Asignar la opcion correcta del examen
	SELECT @correctaresp = opcion_correcta FROM Preguntas
	WHERE (cod_examen = @codexamen and n_pregunta = @npreguntas)
	--Asignar la respuesta del alumno del examen
	SELECT @respuesta = opcion_respuesta FROM Respuestas
	WHERE (cod_examen = @codexamen and Carnet = @carnetx and n_pregunta = @npreguntas)


	--Compara la opcion correcta del examen con la respuesta del alumno
	IF(@correctaresp = @respuesta)
	BEGIN
		--Si coinciden los datos se incrementa un contador, el cual controla el total de respuestas
		--correctas por parte del alumno
		SELECT @totcorrecta = @totcorrecta + 1
	END -- fin de IF
	SELECT @npreguntas=@npreguntas-1 --se decrementa el total de preguntas
END --fin de WHILE

--Calcula la nota del examen
SELECT @NOTA = @totcorrecta * @porcentaje
--Agrega la nota a la tabla Notas
INSERT INTO Notas VALUES (@codexamen,@carnetx,@NOTA,GETDATE())
END -- fin del cuerpo del procedimiento

--Para ver la nota en la ventana de resultados
SELECT "NOTA_EXAMEN"=CONVERT(FLOAT,@NOTA)
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION EXITOSA DEL PROCEDIMIENTO CALCULO_DE_NOTA]


17. Crear el siguiente TRIGGER el cual se activa despues de cada insercion que se realiza en la tabla respuesta verifica que pregunta a contestado el alumno de un examen especifico y se llama al procedimiento Calculo_de_Nota para calcular la nota del examen

18. Verificar la existencia del TRIGGER, ejecutar la siguiente consulta:

--Verificamos si existe el trigger y si este existe se elimina
IF EXISTS (SELECT name FROM sysobjects
	WHERE name = 'Warning' AND type = 'TR')
	DROP TRIGGER Warning
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA VERIFICACION Y/O ELIMINACION DEL TRIGGER]


19. Digitar el siguiente codigo:

--Creacion del trigger
CREATE TRIGGER Warning
ON Respuestas
FOR INSERT	--indica que se disparara al insertar datos en la tabla respuestas
AS
	DECLARE @tot_preguntas INT,
			@Npregunta INT,
			@codExa INT,
			@Nota FLOAT,
			@carnetx VARCHAR(10)

	SELECT @codExa = cod_examen FROM inserted --La tabla inserted almacena copias de las filas
	--afectadas durante las instrucciones INSERT y UPDATE
	SELECT @tot_preguntas = n_preguntas FROM Examenes
	WHERE cod_examen = @codExa
	--Asigna a la variable @Npregunta la pregunta del examen que se esta contestando
	SELECT @Npregunta = n_pregunta FROM inserted
	--Evalua el numero de la pregunta con el total que tiene el examen
	IF @Npregunta = @tot_preguntas
	BEGIN
		--Colocar el numero de mensaje de error que creo en el punto 13 del ejercicio
		RAISERROR (50001, 16, 1)
		SELECT @carnetx = carnet FROM inserted
		--Notar que un Trigger puede llamar a un Store Procedure
		EXEC Calculo_de_Nota @carnetx,@codExa
		--Almacenda en la variable @Nota la nota del alumno con respecto al examen que a contestado
		SELECT @Nota = nota_examen FROM Notas WHERE cod_examen=@codExa
		AND carnet=@carnetx
		PRINT 'Su nota fue: ' + convert(varchar(4),@Nota)
	END --fin de IF
GO

[ESPACIO PARA CAPTURA DEMOSTRANDO LA CREACION EXITOSA DEL TRIGGER WARNING]


20. Realice las siguientes pruebas, ejecutando la siguiente consulta:

--Realizando pruebas
--A la pregunta 1 contesto la opcion 3, es la correcta
INSERT INTO Respuestas VALUES (1,'RC970201',1,3)

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION DE LA PRIMERA RESPUESTA]


21. Ejecutar la siguiente consulta

--A la pregunta 2 contesto la opcion 2, constesto mal
INSERT INTO Respuestas VALUES (1,'RC970201',2,2)

Al contestar esta ultima pregunta se activa el TRIGGER y cuando el alumno contesta la ultima pregunta se ejecuta el procedimiento almacenado para calcular la nota del examen

[ESPACIO PARA CAPTURA DEMOSTRANDO LA INSERCION DE LA SEGUNDA RESPUESTA Y LOS MENSAJES DEL TRIGGER Y CALCULO]


22. Ejecutar las siguientes consultas

--Verificar los registros de la tabla Respuestas
SELECT * FROM Respuestas
--Verificar los registros de la tabla Notas
SELECT * FROM Notas

[ESPACIO PARA CAPTURA DEMOSTRANDO LA VERIFICACION DE LOS DATOS EN LAS TABLAS RESPUESTAS Y NOTAS]


23. Guardar los cambios en el archivo
