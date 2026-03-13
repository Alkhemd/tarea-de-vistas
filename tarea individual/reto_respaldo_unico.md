Act 3 - RETO: Respaldo Nombre Unico (INDIVIDUAL)

Nombre del Alumno: [Escribir tu nombre aqui]
Carnet: [Escribir tu carnet aqui]
Fecha: 12 de Marzo de 2026

---

Desarrollo de la Actividad

1. Inicio de Sesion en SQL Server
Se inicia abriendo Microsoft SQL Server Management Studio (SSMS) y conectandose al motor de base de datos local (localhost) utilizando las credenciales correspondientes.

[ESPACIO PARA CAPTURA: PANTALLA DE INICIO DE SESION DE SSMS (OPCIONAL SI YA ESTABAS ADENTRO) O EXPLORADOR DE OBJETOS CONECTADO]


2. Preparacion de la Base de Datos (Opcional pero recomendado)
Antes de realizar el respaldo, se verifica o se prepara la base de datos PRUEBA insertando algunas tablas (Clientes, Ventas) y registros de prueba mediante un script T-SQL, para asegurar que el archivo de respaldo contenga informacion real.

[ESPACIO PARA CAPTURA: EL SCRIPT DE CREACION DE TABLAS Y LOS MENSAJES DE EXITO MOSTRANDO LOS DATOS INSERTADOS (LA CAPTURA QUE TIENES AHORITA MISMO)]


3. Programacion del Trabajo en el Agente de SQL
Se procede a utilizar el Agente de SQL Server para automatizar la tarea. Se crea un Nuevo Trabajo (New Job).

[ESPACIO PARA CAPTURA: VENTANA GENERAL DEL NUEVO TRABAJO COLOCANDOLE UN NOMBRE COMO "Respaldo Nombre Unico"]

Luego, en la seccion de Pasos (Steps), se crea un nuevo paso de tipo Transact-SQL script (T-SQL) donde se agregara la logica para nombrar el archivo dinamicamente.

[ESPACIO PARA CAPTURA: VENTANA DEL NUEVO PASO DONDE ESTAS PEGANDO EL CODIGO (ASEGURATE DE QUE SE VEA EL CODIGO PEGADO AHI)]


Descripcion del Script T-SQL Utilizado
El codigo implementado en el paso del trabajo es el siguiente:

DECLARE @FechaHora VARCHAR(14);
DECLARE @NombreArchivo VARCHAR(MAX);
DECLARE @RutaCarpeta VARCHAR(MAX);

SET @RutaCarpeta = 'C:\Actividad 3\';

SET @FechaHora = 
    REPLACE(CONVERT(VARCHAR(10), GETDATE(), 112), '-', '') + 
    REPLACE(CONVERT(VARCHAR(8), GETDATE(), 108), ':', '');   

SET @NombreArchivo = @RutaCarpeta + 'PRUEBA_' + @FechaHora + '.bak';

BACKUP DATABASE PRUEBA 
TO DISK = @NombreArchivo 
WITH FORMAT, 
     NAME = 'Respaldo Dinamico de PRUEBA';


Parametros y funciones utilizadas:
- DECLARE: Se usa para crear tres variables. @FechaHora almacenara la cadena de texto de la fecha, @NombreArchivo almacenara la ruta completa final y @RutaCarpeta guarda la ubicacion fisica en el disco duro.
- GETDATE(): Es la funcion principal que obtiene la fecha y hora actuales del sistema en tiempo real.
- CONVERT: Convierte el valor de GETDATE() a texto (VARCHAR). Se usan los estilos 112 (para obtener formato YYYYMMDD) y 108 (para obtener formato de hora HH:MM:SS).
- REPLACE: Se utiliza para limpiar las cadenas convertidas. Elimina los guiones (-) de la fecha y los dos puntos (:) de la hora, dejandonos unicamente los numeros limpios para la nomenclatura requerida (YYYYMMDDHHMMSS).
- Concatenacion (+): Se unen todas las partes: la ruta de la carpeta, el prefijo 'PRUEBA_', la fecha/hora calculada, y la extension '.bak'.
- BACKUP DATABASE: El comando final que ejecuta el respaldo hacia la ruta (TO DISK) que acabamos de armar dinamicamente en la variable @NombreArchivo.

Finalmente, se configura la Programacion (Schedule) para definir en que momento exacto se ejecutara este trabajo automaticamente.

[ESPACIO PARA CAPTURA: LA VENTANA DE PROGRAMACION DEL TRABAJO CONFECCIONANDO LA HORA DE EJECUCION]


4. Resultado Final: Archivo Generado
Una vez llegada la hora programada, el Agente de SQL ejecuta el script. Al revisar la ruta especificada (C:\Actividad 3\), se comprueba la creacion exitosa del archivo de respaldo respetando la nomenclatura unica solicitada.

[ESPACIO PARA CAPTURA: EL EXPLORADOR DE ARCHIVOS DE WINDOWS MOSTRANDO LA CARPETA C:\Actividad 3\ Y EL ARCHIVO .BAK GENERADO CON LOS NUMEROS DE LA FECHA Y HORA]
