Parte 1: Iniciar Sesión desde SQL Server Management Studio

En esta primera fase, me dediqué a instalar y configurar las herramientas necesarias para conectarme a la base de datos local.

1. Descarga e Instalación del Motor de Base de Datos
Primero, me di cuenta de que la versión original que pedía el documento (SQL Server 2012) me generaba un error de compatibilidad de idioma en mi sistema. 
[INSERTAR CAPTURA 1: Pantalla del error de idioma de SQL Server 2012 aquí]

Decidí descargar e instalar una versión más moderna y compatible: SQL Server 2022 Express. Durante la instalación, seleccioné el tipo de instalación "Básica" para tener el motor funcionando con la configuración predeterminada de una manera mucho más rápida y limpia.
[INSERTAR CAPTURA 2: Pantalla eligiendo el tipo de instalación "Básica" de SQL Server 2022 aquí]

Al no ser compatible la instalación con Español de México, el sistema me arrojó un aviso preguntando si deseaba continuar la instalación en inglés. Acepté sin problemas, comprendiendo que el motor se ejecutaría en segundo plano.
[INSERTAR CAPTURA 3: Pantalla del aviso de idioma preguntando si instalar en inglés aquí]

La instalación se completó exitosamente y mi instancia local quedó nombrada como MSSQLSERVER, lista para ser administrada.
[INSERTAR CAPTURA 4: Pantalla de "La instalación se ha completado correctamente" aquí]

2. Descarga e Instalación de SQL Server Management Studio (SSMS)
Posteriormente, procedí a descargar el instalador de la interfaz gráfica requerida: SQL Server Management Studio 22.
[INSERTAR CAPTURA 5: Pantalla web oficial de descarga de SSMS aquí]

Durante su instalación, no necesité Componentes Adicionales. Mantuve la configuración por defecto y seleccioné "Instalar".
[INSERTAR CAPTURA 6: Pantalla del instalador de SSMS mostrando las cargas de trabajo aquí]

Al finalizar el proceso, el instalador me pidió reiniciar mi computadora para limpiar los archivos restantes y aplicar todos los cambios correctamente, lo cual acaté de inmediato.
[INSERTAR CAPTURA 7: Pantalla de la "Instalación finalizada" de SSMS pidiendo reinicio aquí]

3. Conexión y Configuración del Usuario 'sa' (System Administrator)
Una vez reiniciada la máquina, busqué y abrí el SQL Server Management Studio 22. Me recibió una pantalla ofreciéndome iniciar sesión con una cuenta de Microsoft o GitHub para sincronizar trabajo en la nube, pero decidí pulsar en "Omitir y agregar cuentas más tarde" para pasar directamente a las conexiones locales.
[INSERTAR CAPTURA 8: Pantalla inicial pidiendo iniciar sesión con Microsoft/GitHub aquí]

Al abrirse la ventana de conexión:
1. Utilicé en "Servidor" la palabra localhost o el nombre de mi máquina.
2. Usé la "Autenticación de Windows" por default para entrar por primera vez temporalmente, dado que mi usuario sa aún no estaba activo ni tenía contraseña asignada.

Me topé con un error de inicio de sesión debido al certificado SSL por defecto (situación normal en SSMS 20+ al intentar acceder a servidores locales).
[INSERTAR CAPTURA 9: Pantalla del error de certificado SSL "La cadena de certificación fue emitida por una entidad en la que no se confía" aquí]

Lo resolví marcando la casilla "Certificado de servidor de confianza" en las propiedades de mi conexión. Con este pequeño ajuste logré acceder a mi motor.

Una vez adentro:
- Fui a las propiedades de mi servidor e ingresé en la sección "Seguridad". 
- Cambié el modo al de "Autenticación de Windows y SQL Server".
- Fui a "Inicios de sesión", busqué al usuario sa y, luego de habilitarlo en su estado, procedí a borrar su antigua contraseña para establecer la que pedía el manual (123456).
- Reinicié en dos clics el servicio del servidor desde el Explorador de objetos para guardar los cambios.

Finalmente, modifiqué las propiedades de mi ventana de conexión inicial para lograr ingresar utilizando única y exclusivamente las credenciales pedidas en el documento (Autenticación de SQL Server, usuario: sa, contraseña: 123456).
[INSERTAR CAPTURA 10: Pantalla del cuadro de conexión local con el Explorer de objetos del lado izquierdo activado aquí]
