# Proceso que seguí para resolver la práctica

1. Comprensión del Problema

   Leí detalladamente el problema para entender la situación del desarrollo del API REST que el equipo frontend necesita consumir.
    
2. Investigación y Documentación

   Investigé las herramientas disponibles, principalmente bajo el entorno de GitHub. Leí la documentación relevante para tener una visión informada y general de las decisiones e implementaciones necesarias.
    
3. Creación del Repositorio Público

   Creé un repositorio público en GitHub con la estructura inicial solicitada y un archivo README.
    
4. Subida del Código Inicial

   Hice un push con el código proporcionado para el API REST. Añadí títulos y descripciones claras en cada commit para documentar de manera precisa los cambios realizados.
    
5. Configuración del Pipeline de CI/CD

   Cree y desarrollé un archivo YML en los workflows de GitHub bajo el nombre "ci-cd-pipeline.yml". Inicialmente, configuré el pipeline con los eventos de pull request y push por lo que el primer workflow run fallo, pero es normal ya que me falta añadir los jobs e instrucciones.
    
   Incluí una versión visible en el archivo y comentarios detallados en cada paso del archivo YML. Estos comentarios los incluyo para asegurarme que cualquier miembro del equipo pueda entender y modificar el pipeline en el futuro.
    
6. Añadí al archivo YML jobs

   Seguí la estructura de la documentación y ejemplo del pdf, todo lo estoy ejecutando bajo de "ubuntu-latest"
   Para ejecutar correctamente spring utilizo la versión de java 11 (JDK 11) meidnate un action
   Para compilar y generar el artefacto utilizo Maven
    
7. Me encontre con un error de "./mvnw: Permission denied"

   Revise el proceso de build en Github actions y me encontre con este error, para soliconarlo agregué un paso adiconal add permisions to mvnw con el comando de linux "chmod +x mvnw" este parametro de la x me permite otorgarle el permiso para la ejecución
   Actualice mi archivo YML y actualicé la etiqueta de la versión @1 a @2 para seguir una buena practica de control de versión

8. Me encontre con un error de "Error: Input required and not supplied: distribution"

   Revise mi configuración y al parecer tenia que espefcificar la distribución de java , utilice "adopt"
   Actualice mi archivo YML y actualicé la etiqueta de la versión @2 a @3 para seguir una buena practica de control de versión

9. Me encontre con un error relacionado a mvnw.cmd

   Al ver este error me di cuenta de que estaba mal planteado el sistema operativo que elgí inicialmente de Ubuntu por lo que decidí cambiarlo a Windows.
   Una vez cambiado me salio otro error relacionado a que no podia ejecutar el archivo mvnw.cmd, por que opte agregarle mas parametros al run, tratando de especificar que se ejecutara de forma correcta, sin embargo tambien me enocntre con pasos adicionales como crear carpetas para almacenar las descargas de las dependencias.
   Actualice mi archivo YML y actualicé la etiqueta de la versión @3 a @4 para seguir una buena practica de control de versión

10. Validacion de pruebas para construir y probar la aplicación

    Evalue los logs al correr el archivo mvnw en  la tarea de "Build with Maven" y cree otra tarea "Check test results" para poder evaluar los resultados de la prueba creando un archivo temporal maven-output.log en la tarea de "Build with Maven", fue un reto ya que la sintaxis de evaluación me marcaba error y lo tuve que traducir en terminos de sintaxis de windows, especificamente de powershell para tener una mejor visualización de lo que estaba obteneindo por que tenia algunos errores de sintaxis y conecptualziacion, con esto lo solucione

    Después me dirigi a github a modificar la configuración del repositorio en settings y agregue una regla "merge permission" para aprobar o rechazar según el resultado del job "build-and-test" con la opcion de "Require status checks to pass" y subopción de "Require branches to be up to date before merging" en esta ultima agregue el nombre del job:"build-and-test".
    Por ultimo modifique mi Yaml y agregue permisos al repositorio de escritura y lectura.

    Cree una rama para probar los test y realice varios escenarios(hice varias solicitudes de pull request), intencionalmente modifique los test del codigo de java para que marcara errores y efectivamente no paso el pull requeste con el merge, hice un revert y lo deje como estaba originalmente sin errores

11. Publicación de artefactos en github packages

    Modifique el archivo pom.xml devido a que me enfrente a erroes por que no estava especificado "distributionManagement" para lograr publicar el artefacto en los packages de github, esto lo encontre en la documentacion https://docs.github.com/es/actions/using-workflows/storing-workflow-data-as-artifacts
    
    posteriormente agregue un job "publish" que dependia de build-and-test para publicar el artefacto generado en packages.

12. Implementación de cache

    Al momento del job de publicar(publish) agregue un paso para agregar cache.

# Preguntas

- ¿Consideras útil agrega la acción de caché al workflow? Si consideras que sí, agrégala.
   Si, para mejorar el rendimiento y evitar la descarga repetida de dependencias de Maven y otros archivos necesarios en cada ejecución.

- ¿Es posible desplegar automáticamente el artefacto guardado en Packages con Github Actions? 
   Considero que si es posible, agregué el mensaje solicitado de “Despliegue en curso" en mi job de "publish"