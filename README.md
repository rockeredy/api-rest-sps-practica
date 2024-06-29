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
    Decidí continuar con el paso de agregar el "Comando mvnw verify" para valdiar si se podia hacer el merge, actualice mi archivo ci-cd-pipeline.yml
    Después me dirigi a github a modigicar la configuración del repositorio en settings y agregue una regla "merge permission" para aprobar o rechazar según el resultado del test.
    En github agregue Require status checks to pass y asigne un job build-and-test. Por ultimo volvia modificar mi Yaml para agregar esto ultimo y utilicé el GitHub Actions de "peter-evans/merge-pull-request" para lograr esto actualice los mpermisos del Workflow del repositorio a escritura y lectura
