# Sistema de Procesamiento de Calificaciones con Spring Batch.
Este proyecto consiste en un sistema de arquitectura robusta y automatizada diseñado para el procesamiento por lotes (*Batch Processing*) de información academica. 
La aplicación lee datos de estudaintes , calcula promedios de manera efciente en una base de datos relacional y genera reportes consolidados en tiempo real
guardándolos en un sistema NoSQL. 

## Tecnologías Utilizadas: 
* Java 17* (LTS)
* Spring Boot y Spring Batch (Gestión y orquestación del procesamiento por lotes).
* MySQL (Base de Datos NoSQL).
* MongoDB. 
* Docker.
* Junit 5 y Mockito
* Apache Maven.

1. Arquitectura del proceso Batch: 
El Job principal ("procesarCalificacionesJob") se divide en una arquitectura segmentada en dos pasos (steps): 
**STEP 1: (MySQL); en este paso se hace la recuperación de los registros individuales de estudiantes. 
>>Processor: Realiza la lógica matemática para realizar el promedio por cada alumno.
>>Writer: Guarda en la base de datos relacional MySQL, el estado de cada alumno (APROBADO / REPROBADO).
**STEP 2: (MongoDB);
>>Processor: Evalúa las notas finales comparando con las métricuas requeridas.
>>Writer: Genera documentos JSON con sus estados antes guardados y lo sinserta en colecciones de MongoDB.

2. Infraestructura en Docker:
En clases anteriores ya se había hecho el levnatamiento de la base de datos tanto en MySQL y MongoDB, se configuro para MySQL el puerto 3306
y MongoDB con el puerto 27018.

3. Configuración del entorno (application.properties):
Este archivo de propiedades se ecuentra optimizado de manera local en la ruta: src/main/resources/aplication.properties
Ejecutándose bajo el puerto web 8081.

 4. Ejecución del Proyecto:
Para compilar, limpiar dependencias y levantar el servicio web de la aplicación junto al procesamiento del Job Batch: mvn clean spring-boot:run.

5. Pruebas disponibles:
>> Pruebas REST (Endpoints de Consulta), una vez que el Job finaliza con estado [COMPLETED], la API expone los siguientes controladores
HTTP en el puerto 8081:
- Lista de Estudiantes(MySQL): GET http://localhost:8081/api/estudiantes
- Total de aprobados (Servicio lógico): GET http://localhost:8081/api/estudiantes/aprobados/total
- Lista de Reportes por Estado (NoSQL): GET http://localhost:8081/api/reportes/estado/APROBADO

6. Pruebas Unitarias (JUnit 5 + Mockito)
Para validar el procesamiento lógico asilado y las simulaciones de repositorios (Mock) sin necesidad de dependencias de bases de datos
externas, ejecuta: mvn test

