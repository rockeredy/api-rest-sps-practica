
# Proceso que seguí para resolver la práctica

1. **Comprensión del Problema**  
   Leí detalladamente el problema para entender la situación del desarrollo del API REST que el equipo frontend necesita consumir.

2. **Investigación y Documentación**  
   Investigé las herramientas disponibles, principalmente bajo el entorno de GitHub. Leí la documentación relevante para tener una visión informada y general de las decisiones e implementaciones necesarias.

3. **Creación del Repositorio Público**  
   Creé un repositorio público en GitHub con la estructura inicial solicitada y un archivo README.

4. **Subida del Código Inicial**  
   Hice un push con el código proporcionado para el API REST. Añadí títulos y descripciones claras en cada commit para documentar de manera precisa los cambios realizados.

5. **Configuración del Pipeline de CI/CD**  
   Creé y desarrollé un archivo YML en los workflows de GitHub bajo el nombre "ci-cd-pipeline.yml". Inicialmente, configuré el pipeline con los eventos de pull request y push, por lo que el primer workflow falló, pero es normal ya que faltaba añadir los jobs e instrucciones. Incluí una versión visible en el archivo y comentarios detallados en cada paso del archivo YML. Estos comentarios los incluyo para asegurarme de que cualquier miembro del equipo pueda entender y modificar el pipeline en el futuro.

6. **Añadí jobs al archivo YML**  
   Seguí la estructura de la documentación y ejemplo del PDF. Todo lo estoy ejecutando bajo "ubuntu-latest". Para ejecutar correctamente Spring utilizo la versión de Java 11 (JDK 11) mediante una action. Para compilar y generar el artefacto utilizo Maven.

7. **Me encontré con un error de "./mvnw: Permission denied"**  
   Revisé el proceso de build en GitHub Actions y me encontré con este error. Para solucionarlo, agregué un paso adicional para dar permisos a `mvnw` con el comando de Linux "chmod +x mvnw". Este parámetro `+x` permite otorgar el permiso para la ejecución. Actualicé mi archivo YML y la etiqueta de la versión de @1 a @2 para seguir una buena práctica de control de versiones.

8. **Me encontré con un error de "Error: Input required and not supplied: distribution"**  
   Revisé mi configuración y, al parecer, tenía que especificar la distribución de Java. Utilicé "adopt". Actualicé mi archivo YML y la etiqueta de la versión de @2 a @3 para seguir una buena práctica de control de versiones.

9. **Me encontré con un error relacionado a `mvnw.cmd`**  
   Al ver este error, me di cuenta de que estaba mal planteado el sistema operativo que elegí inicialmente, que era Ubuntu, por lo que decidí cambiarlo a Windows. Una vez cambiado, me salió otro error relacionado con que no podía ejecutar el archivo `mvnw.cmd`, por lo que opté por agregar más parámetros al run, tratando de especificar que se ejecutara de forma correcta. Sin embargo, también me encontré con pasos adicionales, como crear carpetas para almacenar las descargas de las dependencias. Actualicé mi archivo YML y la etiqueta de la versión de @3 a @4 para seguir una buena práctica de control de versiones.

10. **Validación de pruebas para construir y probar la aplicación**  
    Evalué los logs al correr el archivo `mvnw` en la tarea de "Build with Maven" y creé otra tarea "Check test results" para poder evaluar los resultados de la prueba, creando un archivo temporal `maven-output.log` en la tarea de "Build with Maven". Fue un reto, ya que la sintaxis de evaluación me marcaba error y lo tuve que traducir en términos de sintaxis de Windows, específicamente de PowerShell, para tener una mejor visualización de lo que estaba obteniendo, ya que tenía algunos errores de sintaxis y conceptualización. Con esto, lo solucioné.  
    Después, me dirigí a GitHub a modificar la configuración del repositorio en settings y agregué una regla "merge permission" para aprobar o rechazar según el resultado del job "build-and-test" con la opción de "Require status checks to pass" y la subopción de "Require branches to be up to date before merging". En esta última, agregué el nombre del job: "build-and-test". Por último, modifiqué mi YAML y agregué permisos al repositorio de escritura y lectura.  
    Creé una rama para probar los tests y realicé varios escenarios (hice varias solicitudes de pull request). Intencionalmente, modifiqué los tests del código de Java para que marcaran errores y, efectivamente, no pasó el pull request con el merge. Hice un revert y lo dejé como estaba originalmente, sin errores.

11. **Publicación de artefactos en GitHub Packages**  
    Modifiqué el archivo `pom.xml` debido a que me enfrenté a errores porque no estaba especificado "distributionManagement" para lograr publicar el artefacto en los packages de GitHub. Esto lo encontré en la documentación: [GitHub Actions](https://docs.github.com/es/actions/using-workflows/storing-workflow-data-as-artifacts).  
    Posteriormente, agregué un job "publish" que dependía de "build-and-test" para publicar el artefacto generado en packages.

12. **Implementación de caché**  
    Al momento del job de publicar (publish), agregué un paso para agregar caché.

# Preguntas

- **¿Consideras útil agregar la acción de caché al workflow?**  
   Sí, para mejorar el rendimiento y evitar la descarga repetida de dependencias de Maven y otros archivos necesarios en cada ejecución.

- **¿Es posible desplegar automáticamente el artefacto guardado en Packages con GitHub Actions?**  
   Considero que sí es posible. Agregué el mensaje solicitado de “Despliegue en curso" en mi job de "publish".

- **¿Qué modificaciones consideras que se tendrían que realizar a los workflows para trabajar con imágenes de contenedores, siguiendo las mejores prácticas que conozcas?**  
   Para adaptar nuestro pipeline de CI/CD y prepararnos para trabajar con imágenes de contenedores, consideraría las siguientes modificaciones clave:
   - **Estructura del Proyecto:**  
     Crearía un archivo Dockerfile en la raíz de nuestro proyecto para definir cómo podría construirse la imagen del contenedor.
   - **Modificaciones al Pipeline:**  
     - Añadiría pasos específicos para construir la imagen del contenedor utilizando Docker.
     - Integraría pruebas automáticas enfocadas en la validación de la imagen del contenedor.
     - Implementaría la publicación automática de la imagen en un registro de contenedores, como AWS ECR o Docker Hub.

- **¿Qué pasos y/o herramientas utilizarías para entender las plantillas de CloudFormation y evaluar qué ajustes se tendrían que realizar a la etapa de despliegue?**  
   1. Estudiaría la estructura y componentes de las plantillas.
   2. Validaría parámetros y variables.
   3. Probaría en un entorno local o de prueba.
   4. Revisaría las mejores prácticas y documentaría ajustes necesarios.
   5. Colaboraría con el equipo de Arquitectura y AWS para alinear requisitos y estándares. Creo que la comunicación es muy importante y, si tengo alguna duda, siempre la pregunto.
