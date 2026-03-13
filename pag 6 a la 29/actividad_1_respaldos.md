Parte 1: Iniciando sesion desde SQL Server Managment Studio

1. Se hace clic en el boton Inicio

2. Se hace clic en la opcion Todos los programas y se hace clic en Microsoft SQL Server 2022

Para conectarse con el servidor de base de datos se eligen los siguientes parametros de autenticacion:

Tipo de servidor: Database Engine
Nombre del servidor: Colocar el nombre del servidor local, por ejemplo PCNumMaquina-SALA
Nota: NumMaquina es el numero de la maquina local
Autenticacion: SQL Server Authentication
Login: sa
Password: 123456

3. Luego de clic en el boton conectar y asi ingresar a la ventana del SQL Server Managment Studio.

[ESPACIO PARA CAPTURA DEMOSTRANDO LOS PARAMETROS EN LA PANTALLA DE CONEXION ANTES DE CONECTAR]


Parte 2: Creando copias de seguridad

Ejercicio 1. Como realizar una copia de seguridad de una base de datos con SQL Server Management Studio

1. Se crea una carpeta en la Unidad C con el nombre Backups_SuCarnet, recordando cambiar la palabra SuCarnet por el numero de carnet correspondiente.

2. Se expande la carpeta Bases de datos (Databases) y se selecciona la base de datos NORTHWIND (o la base de datos a respaldar).

3. Se hace clic derecho en la base de datos, se selecciona la opcion Tareas (Tasks) y se hace clic en Copia de seguridad (Back Up...).

4. Aparece el cuadro de dialogo Copia de seguridad de base de datos (Back Up Database).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU TAREAS Y LA OPCION COPIA DE SEGURIDAD SELECCIONADA]

5. En el cuadro de lista Base de datos (Database), se verifica el nombre de la base de datos, en este caso tiene que estar la base de datos NORTHWIND. Tambien es posible seleccionar otra base de datos en la lista.

[ESPACIO PARA CAPTURA DEL CUADRO DE DIALOGO COPIA DE SEGURIDAD CON LOS PARAMETROS INICIALES]


6. Es posible realizar una copia de seguridad de la base de datos en cualquier Modelo de recuperacion (Recovery model) (FULL, BULK_LOGGED o SIMPLE).

7. En el cuadro de lista Tipo de copia de seguridad (Backup type), se selecciona la opcion Completa (Full). 
   Se tiene en cuenta que despues de crear una copia de seguridad de la base de datos completa, se puede crear una copia de seguridad diferencial.

8. En Componente de copia de seguridad (Backup component), se hace clic en Base de datos (Database).

9. Se acepta el nombre del conjunto de copia de seguridad predeterminado sugerido en el cuadro de texto Nombre (Name), o bien se especifica otro nombre. Para este ejercicio se coloca el nombre NORTHWIND-Backup.

10. Opcionalmente, en Descripcion (Description), se escribe una descripcion del conjunto de copia de seguridad, por ejemplo: Copia de seguridad de la base de datos NORTHWIND.

[ESPACIO PARA CAPTURA DEMOSTRANDO LOS CAMPOS DE NOMBRE Y DESCRIPCION LLENOS EN LA VENTANA GENERAL]


11. Se especifica cuando expirara el conjunto de copia de seguridad y se podra sobrescribir sin omitir explicitamente la comprobacion de los datos de expiracion:
    Para que el conjunto de copia de seguridad expire en una determinada fecha, se hace clic en El (On) y se escribe la fecha en la que expirara. 
    Se selecciona la cantidad de dias y la fecha que expirara la copia de seguridad, para el ejercicio se selecciona la fecha de mañana.

[ESPACIO PARA CAPTURA DEMOSTRANDO LA FECHA DE EXPIRACION SELECCIONADA]

12. Se elige el tipo de destino (Destination) de la copia de seguridad haciendo clic en Disco (Disk) o Cinta (Tape). Para seleccionar una ruta de disco o cinta que contenga un solo conjunto de medios, se hace clic en Agregar (Add). En el ejercicio se agrega la ruta de la carpeta que se creo en el paso 1.

Se hace clic en los puntos suspensivos (...)

En la opcion Nombre de archivo (File name) se digita el nombre: NORTHWINDBackup_SuCarnet, y se hace clic en el boton Aceptar (OK).

Se hace clic en el boton Aceptar (OK).

[ESPACIO PARA CAPTURA MOSTRANDO LAS 3 VENTANAS DE DESTINO DE LA COPIA DE SEGURIDAD]


13. Al final la configuracion queda mostrando las opciones elegidas en el cuadro de dialogo.

[ESPACIO PARA CAPTURA DE LA VENTANA GENERAL CON TODA LA CONFIGURACION COMPLETADA ANTES DE ACEPTAR]

14. Se hace clic en Aceptar (OK) y se desplegara una ventana indicando que se ha creado satisfactoriamente la copia de seguridad de la base de datos.

[ESPACIO PARA CAPTURA DEL MENSAJE DE EXITO DE LA COPIA DE SEGURIDAD]

15. Se comprueba en la carpeta creada en C: que el archivo .bak ha sido generado.

[ESPACIO PARA CAPTURA DEL EXPLORADOR DE ARCHIVOS MOSTRANDO EL ARCHIVO .BAK CREADO]


Ejercicio 2. Como restaurar una copia de seguridad de base de datos con SQL Server Management Studio

1. Se hace clic derecho en la base de datos NORTHWIND, se selecciona Tareas (Tasks) y, a continuacion, se hace clic en la opcion Restaurar (Restore) y luego se hace clic en Base de datos (Database...).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU TAREAS Y LA OPCION RESTAURAR BASE DE DATOS SELECCIONADA]

2. Con lo que se abrira el cuadro de dialogo Restaurar base de datos (Restore Database).

[ESPACIO PARA CAPTURA DEL CUADRO DE DIALOGO RESTAURAR BASE DE DATOS INICIAL]

3. En la pagina General, en la opcion Destination, el nombre de la base de datos en restauracion aparecera en el cuadro de lista Base de datos (Database). Para crear una nueva base de datos, se escribe el nombre en el cuadro de lista, en este ejemplo se colocara el nombre: CopiaNORTHWIND.

4. En el cuadro de texto Restaurar a (Restore to), se puede conservar el valor predeterminado: La ultima copia de seguridad tomada (The last backup taken) o seleccionar una fecha y hora determinada haciendo clic en el boton Timeline..., esto abrira el cuadro de dialogo Linea de tiempo de copia de seguridad (Backup timeline). Para el ejercicio se deja el valor predeterminado.

5. En la cuadricula Conjuntos de copia de seguridad que se van a restaurar (Backup sets to restore), se seleccionan las copias de seguridad que se desean restaurar; en el ejemplo se selecciona la que se creo en el ejercicio anterior.

[ESPACIO PARA CAPTURA DEMOSTRANDO LOS PARAMETROS DE RESTAURACION CON EL NOMBRE COPIANORTHWIND Y EL CONJUNTO DE COPIA DE SEGURIDAD SELECCIONADO]


6. Se hace clic en la opcion Archivos (Files) y se selecciona la carpeta que se creo en la unidad C para guardar los archivos .mdf y .ldf de la copia de la base de datos NORTHWIND.

[ESPACIO PARA CAPTURA DEMOSTRANDO LA OPCION REUBICAR TODOS LOS ARCHIVOS Y LA RUTA DE LA CARPETA SELECCIONADA]

7. Se hace clic en el boton Aceptar (OK).

8. Se desplegara un mensaje indicando que la restauracion se realizo satisfactoriamente.

[ESPACIO PARA CAPTURA DEL MENSAJE DE EXITO DE LA RESTAURACION]

9. Se actualiza la carpeta Bases de datos (Databases) y se observa que se creo la base de datos CopiaNORTHWIND con las mismas propiedades y objetos de la base de datos NORTHWIND.

[ESPACIO PARA CAPTURA REVELANDO LA NUEVA BASE DE DATOS COPIANORTHWIND EN EL EXPLORADOR DE OBJETOS]


EJERCICIO: Realice la restauracion de la base de datos colocandole el nombre Copia2NORTHWIND

[ESPACIO PARA CAPTURAS DEMOSTRANDO LA RESTAURACION COMO COPIA2NORTHWIND (OPCIONAL SEGUN INDICACIONES)]


Ejercicio 3. Definir un dispositivo logico de copia de seguridad en un archivo de disco

1. Se expande la carpeta Objetos de servidor (Server Objects) y, a continuacion, se hace clic derecho en Dispositivos de copia de seguridad (Backup Devices).

2. Se hace clic en Nuevo dispositivo de copia de seguridad (New Backup Device...).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU DE OBJETOS DE SERVIDOR Y NUEVO DISPOSITIVO SELECCIONADO]

3. Se abrira el cuadro de dialogo Dispositivo de copia de seguridad (Backup Device).

4. Se escribe un nombre para el dispositivo (Device Name), el cual sera: DispCopiaNORTHWIND.

5. Para indicar el destino, se hace clic en los puntos suspensivos (...) de la opcion Archivo (File) y se especifica la ruta de acceso completa del archivo; se abrira la ventana Locate Database File, se selecciona la carpeta que se creo en la unidad C y en la opcion File se escribe el mismo nombre del dispositivo que se coloco en el punto 4 de este ejercicio, y se hace clic en Aceptar (OK).

6. Se hace clic en el boton Aceptar (OK).

7. Se verifica que se creo el nuevo dispositivo en el panel de Objetos de servidor.

[ESPACIO PARA CAPTURA MOSTRANDO EL NUEVO DISPOSITIVO BAJO LA CARPETA DISPOSITIVOS DE COPIA DE SEGURIDAD]


Ejercicio 4. Creando una copia de seguridad diferencial y utilizando el dispositivo de copia con SQL Management Studio

La creacion de una copia de seguridad diferencial de base de datos requiere que haya una copia de seguridad de base de datos completa (Full) previa. Si nunca se ha hecho una copia de seguridad de la base de datos seleccionada, realizar una copia de seguridad de base de datos completa antes de crear la copia de seguridad diferencial.

1. Se hace clic derecho en la base de datos NORTHWIND, se selecciona la opcion Tareas (Tasks) y se hace clic en Copia de seguridad (Back Up...).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU TAREAS Y LA OPCION COPIA DE SEGURIDAD SELECCIONADA DE NUEVO]

Aparece el cuadro de dialogo Copia de seguridad de base de datos (Back Up Database).

2. En el cuadro de lista Base de datos (Database), se verifica el nombre de la base de datos, en este caso tiene que estar la base de datos NORTHWIND.

3. En el cuadro de lista Tipo de copia de seguridad (Backup type), se selecciona la opcion Diferencial (Differential).

4. En Componente de copia de seguridad (Backup component), se hace clic en Base de datos (Database).

5. Se puede aceptar el nombre del conjunto de copia de seguridad predeterminado sugerido en el cuadro de texto Nombre (Name) o especificar otro nombre, en el ejercicio se colocara el nombre NORTHWIND-Backup-Diferencial.

6. Opcionalmente, en Descripcion (Description), se escribe una descripcion del conjunto de copia de seguridad.

7. Se especifica cuando expirara el conjunto de copia de seguridad y se podra sobrescribir sin omitir explicitamente la comprobacion de los datos de expiracion:
   a. Para que el conjunto de copia de seguridad expire al cabo de un numero de dias especifico, se hace clic en Despues de (After) (opcion predeterminada) y se escribe el numero de dias tras la creacion del conjunto en que este expirara. Este valor puede estar entre 0 y 99999 dias; el valor 0 significa que el conjunto de copia de seguridad no expirara nunca.
   b. Para que el conjunto de copia de seguridad expire en una determinada fecha, se hace clic en El (On) y se escribe la fecha en la que expirara.
   En el ejercicio no se cambiara nada.

[ESPACIO PARA CAPTURA DE LA VENTANA REVELANDO EL TIPO DE COPIA DE SEGURIDAD DIFERENCIAL Y LOS CAMPOS NOMBRADOS SEGUN EL EJERCICIO]

8. Se elige el tipo de destino (Destination) de la copia de seguridad haciendo clic en Disco (Disk) o Cinta (Tape). Para seleccionar una ruta de disco o cinta que contenga un solo conjunto de medios, se hace clic en Agregar (Add) y se abrira la siguiente ventana.

9. Se elige la opcion Dispositivo de copia de seguridad (Backup device) y se asegura que aparece el dispositivo de copia de seguridad creado en el ejercicio anterior.

[ESPACIO PARA CAPTURA DEMOSTRANDO LA SELECCION DEL DISPOSITIVO DE COPIA DE SEGURIDAD RECIEN CREADO]

Se hace clic en el boton Aceptar (OK) y regresara a la ventana anterior.

10. Se selecciona la ruta de la copia de seguridad completa que habia previamente por defecto y se hace clic en la opcion Quitar (Remove), luego se verifican las opciones de los cambios realizados.

[ESPACIO PARA CAPTURA MOSTRANDO LA VENTANA PRINCIPAL CON EL DISPOSITIVO SELECCIONADO COMO DESTINO Y CON EL TIPO DIFERENCIAL]

11. Se hace clic en Aceptar (OK) y se desplegara una ventana indicando que la copia de seguridad se ha completado correctamente.

[ESPACIO PARA CAPTURA DEL MENSAJE DE EXITO DE LA COPIA DE SEGURIDAD DIFERENCIAL]

Indicando que se ha creado satisfactoriamente la copia de seguridad de la base de datos.


Ejercicio 5. Creacion y restauracion de copias de seguridad utilizando Transact SQL

1. En el ejemplo siguiente se realizara una copia de seguridad completa de la base de datos library, en la carpeta de la unidad C.
   Se utiliza el siguiente script en una nueva consulta:

   USE library
   GO
   BACKUP DATABASE library
   TO DISK = 'C:\Backups_SuCarnet\Library.bak'
   WITH FORMAT

2. Al ejecutar la consulta se observara que se ha creado el BACKUP de la base de datos en los mensajes de salida.

[ESPACIO PARA CAPTURA MOSTRANDO EL SCRIPT SQL Y EL MENSAJE DE EXITO AL FINAL DE LA PANTALLA]

Y se puede comprobar verificando la existencia del archivo fisico en el explorador de Windows.

[ESPACIO PARA CAPTURA DEL EXPLORADOR DE WINDOWS MOSTRANDO LOS ARCHIVOS DE BACKUP CREADOS]

3. Se procede a eliminar la base de datos library, para que despues se vuelva a crear por medio del backup creado anteriormente utilizando el siguiente comando:

   DROP DATABASE library

[ESPACIO PARA CAPTURA DEL SCRIPT DROP DATABASE Y SU MENSAJE DE CONFIRMACION]

Se verifica en el Explorador de objetos que ya no existe la base de datos.

4. En el siguiente ejemplo se restaura una base de datos desde una copia de seguridad completa agregada a un dispositivo de copia de seguridad:

   RESTORE DATABASE library
   FROM DISK = 'C:\Backups_SuCarnet\Library.bak'

[ESPACIO PARA CAPTURA MOSTRANDO EL COMANDO RESTORE Y LOS MENSAJES DE EXITO DE LAS PAGINAS PROCESADAS]

5. Por ultimo, se verifica en el Explorador de objetos que la base de datos se creo nuevamente (puede requerir actualizar la vista).

[ESPACIO PARA CAPTURA DEL EXPLORADOR DE OBJETOS MOSTRANDO LA BD LIBRARY RESTAURADA]


Parte 3: Uso del SQL Agent

Ejercicio 1. Creacion de operador y configuracion de alertas

Si el agente SQL Server esta detenido hay que levantar los servicios haciendo clic derecho sobre SQL Server Agent y seleccionar la opcion Start.

1. Se expande el Agente SQL Server (SQL Server Agent)

2. Se hace clic con el boton derecho en Operadores (Operators) y se selecciona Nuevo Operador (New Operator).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU OPERADORES Y LA OPCION NUEVO OPERADOR SELECCIONADA]

3. En el cuadro de texto Name (Nombre), se escribe el nombre AdministradorSuCarnet (sustituyendo SuCarnet por el numero de carnet).

4. Se escribe el nombre del equipo en el cuadro Net send address.

5. En la parte inferior de la pantalla, se pueden seleccionar los dias y las horas a las que esta disponible este operador. Si se activa un dia, el operador se notificara en ese dia entre las horas de Inicio del dia laborable y Fin del dia laborable; se selecciona el dia de ahora y la hora que empieza y termina la practica.

[ESPACIO PARA CAPTURA DE LA VENTANA NUEVO OPERADOR CON LOS DATOS LLENOS ANTES DE ACEPTAR]

6. Se hace clic en OK y se verifica que se creo el operador.

[ESPACIO PARA CAPTURA DEMOSTRANDO EL NUEVO OPERADOR EN EL EXPLORADOR DE OBJETOS]

7. Se hace clic con el boton derecho en el icono del Agente de SQL Server en el Explorador de objetos, y luego se selecciona la opcion Propiedades (Properties).

[ESPACIO PARA CAPTURA MOSTRANDO EL MENU CONTEXTUAL DEL AGENTE DE SQL Y LA OPCION PROPIEDADES]

8. Se abrira la ventana de Propiedades.

[ESPACIO PARA CAPTURA DE LA VENTANA DE PROPIEDADES GENERALES DEL AGENTE DE SQL]

9. En la pagina Sistema de alerta (Alert System), se seleccionan las siguientes casillas de verificacion:
   - Enable mail profile (si esta disponible configurar un perfil, de lo contrario obviar para el ejercicio)
   - Enable fail-safe operator (Habilitar operador de buscapersonas/a prueba de errores)

10. Se selecciona AdministradorSuCarnet en la lista desplegable del operador.

11. Se seleccionan las casillas de verificacion E-mail y Net send para recibir mensajes como operador a prueba de errores.

[ESPACIO PARA CAPTURA DE LA VENTANA SISTEMA DE ALERTA CON LAS OPCIONES CONFIGURADAS]

12. Se hace clic en Aceptar (OK) para aplicar los cambios.


Ejercicio 2. Creacion de trabajos de servidor local

Los trabajos locales son los trabajos estandar con una serie de pasos y programaciones. Estan diseñados para ejecutarse en el equipo en que se crean. Para explicar los trabajos locales, vamos a programar uno que crea una nueva base de datos y hace una copia de seguridad.

1. Ubicarse en la opcion del SQL Server Agent (Agente de SQL Server).

2. Hacer clic derecho en Trabajos (Jobs) y seleccionar Nuevo Trabajo (New Job...).

[ESPACIO PARA CAPTURA DEMOSTRANDO EL MENU TRABAJOS Y LA OPCION NUEVO TRABAJO SELECCIONADA]

3. En el cuadro Nombre (Name), escribir Create Prueba Database, deje el resto de los cuadros de esta pagina con los valores predeterminados, como se muestra en la siguiente figura:

[ESPACIO PARA CAPTURA DE LA VENTANA NUEVO TRABAJO CON EL NOMBRE DEFINIDO]

4. Hacer clic en la pagina Pasos (Steps) y hacer clic en el boton Nuevo (New...) para crear un nuevo paso.

5. Se abrira una nueva ventana llamada Nuevo paso del trabajo (New Job Step).

[ESPACIO PARA CAPTURA DE LA VENTANA NUEVO PASO DEL TRABAJO INICIAL]

6. En el cuadro Nombre del paso (Step name), escribir Create database.

7. En la opcion Tipo (Type) dejar el valor: Transact-SQL script (T-SQL) y escribir el siguiente codigo para crear una base de datos con el nombre PRUEBA en la unidad C

   CREATE DATABASE PRUEBA ON
   PRIMARY (NAME=prueba_dat,FILENAME='C:\Backups_SuCarnet\prueba.mdf',
   SIZE=10MB,
   MAXSIZE=15MB,
   FILEGROWTH=10%)

[ESPACIO PARA CAPTURA DEL CODIGO T-SQL ESCRITO EN LA PESTANA GENERAL DEL PASO]

8. Hacer clic en el boton Analizar (Parse) para verificar que el codigo esta escrito correctamente:

[ESPACIO PARA CAPTURA DEL MENSAJE DE CONFIRMACION DEL ANALIZADOR DE CODIGO]

9. Hacer en la pagina Avanzado (Advanced)

10. En la pagina Avanzado, verificar que la Accion en caso de exito (On success action) es Ir al siguiente paso (Go to the next step) y que la accion En caso de error (On failure action) es Salir del trabajo e informar del error (Quit the job reporting failure). Si es asi hacer clic en Aceptar (OK) y sino realizar los cambios.

[ESPACIO PARA CAPTURA DE LA PESTAÑA AVANZADO MOSTRANDO ESTAS ACCIONES DE EXITO/ERROR]

11. Siempre en la ventaba New Job, se debera crear el segundo paso del trabajo, hacer clic en el boton Nuevo (New).

12. En el cuadro de texto Nombre (Name), escribir Backup Test.

13. En la opcion Tipo (Type) dejar el valor: Transact-SQL script (T-SQL) y escribir el siguiente codigo el cual crea una copia de seguridad de la base de datos creada en el paso 1 del trabajo:

    EXEC sp_addumpdevice 'disk', 'PRUEBA_back', 'PRUEBA_backup.dat'
    GO
    BACKUP DATABASE PRUEBA TO PRUEBA_back

[ESPACIO PARA CAPTURA DEL SEGUNDO PASO CON EL CODIGO DE BACKUP]

14. Analizar que el codigo este bien escrito, haciendo clic en la opcion Analizar (Parse).

[ESPACIO PARA CAPTURA DEL MENSAJE DE EXITO DEL ANALIZADOR DEL SEGUNDO PASO]

15. Hacer clic en Aceptar (OK) para crear el paso (despues de revisar la pestaña avanzado si es necesario cerrar la ventana del paso 2).

16. Pasar a la pagina Programaciones (Schedules) en la ventana lateral izquierda (en la ventana principal New Job) y hacer clic en el boton Nuevo (New) para crear una programacion que indique a SQL Server cuando debe activar el trabajo.

17. En el cuadro Nombre (Name), escribir Create and Backup Database.

18. En Tipo de programacion (Schedule type), seleccionar: Una Vez (One time).

19. La casilla Enabled (Habilitado) debe estar habilitada.

20. En la opcion Tiempo que ocurre el trabajo (One-time occurrence) seleccionar en la opcion Fecha (Date) la fecha de la práctica y en la opcion Hora (Time) cambiarla a 5 minutos despues de la hora del sistema.

[ESPACIO PARA CAPTURA DE LA PANTALLA NEW JOB SCHEDULE CONFIGURADA]

21. Hacer clic en el boton Aceptar (OK) para crear la programacion y seleccionar la pagina Notificaciones (Notifications).

22. En la pagina Notificaciones, seleccionar la casilla de verificación a NET SEND especificando AdministradorSuCarnet como el operador al que hay que informar. Junto a estas opciones, seleccionar Si el trabajo falla (When the job fails) en el cuadro de lista desplegable (con lo que se notificará el resultado del trabajo).

[ESPACIO PARA CAPTURA DE LA PANTALLA DE NOTIFICACIONES CONFIGURADA PARA NET SEND]

23. Hacer clic en OK para crear el trabajo. Esperar hasta la hora indicada en el paso 20 para verificar que el trabajo ha terminado.

24. Actualizar la carpeta Databases y verificar que se creó la base de datos:

[ESPACIO PARA CAPTURA DEL EXPLORADOR DE OBJETOS MOSTRANDO LA NUEVA BASE DE DATOS PRUEBA Y DEL EXPLORADOR DE ARCHIVOS MOSTRANDO LOS ARCHIVOS GENERADOS]

25. También verificar que se creó el dispositivo de disco de copia de seguridad, actualizar la carpeta Backup Devices

[ESPACIO PARA CAPTURA DEL EXPLORADOR MOSTRANDO EL NUEVO DISPOSITIVO 'PRUEBA_back']

Por lo tanto el trabajo se ejecutó correctamente


---

V. Análisis de resultados

1.  **Investigar y crear un ejemplo de los siguientes tipos de backups: Differential Backup y Transaction Log Backup con Transact SQL**.

    **A. Ejemplo de Differential Backup (Copia de seguridad diferencial)**
    
    Una copia de seguridad diferencial contiene únicamente los datos que han cambiado o se han añadido a la base de datos desde la última copia de seguridad completa (FULL) exitosa. Es más rápida de realizar y ocupa menos espacio.
    
    *Ejemplo de T-SQL (Suponiendo que ya existe un backup FULL de la bd)*:
    ```sql
    -- Primero, nos aseguramos de usar master u otra BD para no bloquear la que respaldaremos
    USE master;
    GO
    -- Realizamos la copia de seguridad diferencial utilizando la cláusula WITH DIFFERENTIAL
    BACKUP DATABASE NORTHWIND
    TO DISK = 'C:\Backups_SuCarnet\NORTHWIND_Diff.bak'
    WITH DIFFERENTIAL,
         NAME = 'Respaldo Diferencial de NORTHWIND',
         STATS = 10;
    GO
    ```

    **B. Ejemplo de Transaction Log Backup (Copia de seguridad del registro de transacciones)**
    
    Este tipo de backup guarda el registro de todas las transacciones que han ocurrido en la base de datos desde el último backup del Log. Permite la restauración a un punto en el tiempo específico ("Point-in-time recovery"). *Nota: La base de datos debe estar en el modelo de recuperación FULL o BULK_LOGGED para que esto funcione.*
    
    *Ejemplo de T-SQL*:
    ```sql
    USE master;
    GO
    -- Realizamos la copia de seguridad del registro de transacciones
    BACKUP LOG NORTHWIND
    TO DISK = 'C:\Backups_SuCarnet\NORTHWIND_Log.trn'
    WITH NAME = 'Respaldo de Transacciones de NORTHWIND',
         STATS = 10;
    GO
    ```

2.  **Investigar como ejecutar procedimientos almacenados desde la programacion de un trabajo con transact SQL, colocar un ejemplo**.

    Para ejecutar un Procedimiento Almacenado (Stored Procedure) desde un Trabajo (Job) del Agente de SQL Server, se crea un paso de trabajo de tipo "Script Transact-SQL (T-SQL)". En el campo de comando, simplemente se llama al procedimiento utilizando el comando `EXEC` o `EXECUTE`.

    *Ejemplo de T-SQL (lo que se colocaría en el cuadro de "Comando" del paso del trabajo)*:
    
    Supongamos que tenemos un procedimiento almacenado llamado `sp_LimpiarTablasTemporales` en la base de datos `VentasDB`.
    
    ```sql
    -- Especificar la base de datos donde reside el procedimiento almacenado
    USE VentasDB;
    GO
    
    -- Ejecutar el procedimiento almacenado
    EXEC dbo.sp_LimpiarTablasTemporales;
    GO
    ```
    *En la configuración del Job en SSMS, se seleccionaría `VentasDB` en el menú desplegable "Base de datos" del paso, y el código T-SQL de arriba se pegaría en el cuadro de texto de comandos.*


VI. Referencia Bibliográfica

1. La Biblia de SQL Server 2005
   Madrid, España: Anaya, 2006
   Autor: Mike Gundelerloy y Joseph L. Jorden
   Biblioteca UDB – Clasificación: 005.361 G975 2006

2. Microsoft SQL Server 2005: Diseño de una estructura de servidor de base de datos. MCITP Examen 70-443
   Madrid, España: ANAYA, 2007
   Autor: J.C. Mackin y Mike Hotek
   Biblioteca UDB – Clasificación: 005.361 M158 2007

3. SQL Server 2008
   Madrid, España: ANAYA, 2009
   Autor: Francisco Charte Ojeda
   Biblioteca UDB – Clasificación: 005.361 Ch436 2009

4. https://technet.microsoft.com/es-es/library/ms187358(v=sql.110).aspx
