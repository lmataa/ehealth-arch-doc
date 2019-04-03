---
title: "Documentación de una arquitectura"
titlepage: "True"
date: 02-04-2019
author: [Luis Mata Aguilar, María Gallego Martín, Daniel Rodriguez Manzanero, Yeray Granada Layos, Carlos Gómez Robles, Alejandro de la Fuente Perdiguero]
...

# Documentando la Arquitectura

## 1. Introducción

### 1.1 Descripción general del problema

En Madrid, desde el 1 de febrero de 2016, se han implantado protocolos de actuación debido a la contaminación atmosférica, situándose ésta como un [problema de salud pública](https://diario.madrid.es/blog/2016/02/03/la-contaminacion-atmosferica-un-grave-problema-de-salud-publica/) en el que se invierten una cantidad desmesurada de recursos.

Se propone la construcción de un sistema eHealth en el contexto de Madrid entendida como una ciudad inteligente, teniendo en cuenta el Plan de acción sobre la salud electrónica 2012-2020: antención sanitaria innovadora para el siglo XXI (Comisión europea).

Para conciliar las ideas propuestas en dicho plan, el sistema pretende facilitar el trato con pacientes permitiendo cosultas mediante vía telemática; diagnóstico remoto de enfermedades con perfil establecido; tratamiento de enfermedades terminales o crónicas así como monitoreo de las mismas mediante el uso de tecnologías IoT; avisos de rigesgos de salud pública con un sistema de alertas personalizables (alérgenos, nivel de contaminación); categorización y estudio de enfermedades a nivel de población, de forma que la investigación se vea explícitamente favorecida.

En cuanto a las comunicaciones telemáticas, el sistema contará con aplicaciones cliente a las que los ciudadanos podrán conectarse y ligar a su centro médico o seguro sanitario. En caso de tratarse de un paciente que requiera de un seguimiento personalizado, obtendrá una extensión del cliente, que le permitirá, junto con un pequeño equipo de sensores IoT, ser susceptible de monitoreo y seguimineto por los especialistas médicos que le estén tratando. Mejorando de esta forma la calidad de vida tanto del paciente terminal o crónico como la del doctor especialista, elevando la sostenibilidad de recursos públicos de Madrid en sanidad, así como de recursos humanos en centros médicos y hospitales.

En caso de haber parámetros en el seguimiento de estos pacientes fuera del rango esperado, se categorizará la urgencia de la anomalía y en caso de ser urgente, una ambulancia (ahora equipada con dispositivos IoT que las conecten al sistema) se dirigirá a la ubicación del sensor. Los datos, anomalías e incidencias serán comprobados a varios niveles de redundacia para resolver cualquier tipo de inconsistencia.



### 1.2 Business Goals

- Resolver las dudas sanitarias de cualquier ciudadano de Madrid vía telemática, lo cual incluye:
    - Respuestas automáticas de dudas puntuales con características específicas.
    - Consultas personales con un doctor por medios telemáticos en su jornada laboral y dentro de ciertos horarios.

- Monitoreo y seguimiento para pacientes que tengan establecida una extensión del sistema para el control de enfermedades crónicas o de tratamiento continuado mediante dispositivos IoT, incluye:
    - Para datos que salgan del rango permitido en el monitoreo:
        - Urgentes: Sistema sincronizado de ambulancias para atender la emergencia.
        - No urgentes: Derivan en alertas a doctor especialista y al usuario concertando cita (telemática o no).

- Como doctor se podrá acceder a los datos de seguimiento de los pacientes para observaciones periódicas sin necesidad de verles personalmente, siendo necesario sin embargo una visita personal cada cierto tiempo según convenga.

- Alertas informativas a todos los usuarios sobre el entorno saludable de Madrid, incluye:
    - Niveles de contaminación atmosférica por zonas.
    - Niveles de contaminación auditiva por zonas.
    - Niveles de alérgenos **personalizados** en la ciudad.

- Información y estadísticas derivadas del uso del servicio por todos los usuarios del sistema (respetando la LOPD, serán datos no personales). Se pretende ayudar a la investigación de enfermedades y tratamientos así como respuesta al entorno por parte de la ciudad para analizar situaciones de salud pública. De forma que los doctores especialistas así como instituciones pertinentes que dispongan de autorización, puedan ver estos resultados para realizar I+D+i sobre salud pública.

- Para probar y lanzar el sistema se considera una user base de 50.000 usuarios lo cual corresponde con aproximadamente 1/60 de la población de Madrid. A partir de la cual el sistema irá optimizando y escalando a su uso real.

### 1.3 Business Drivers

1. Para cualquier usuario autenticado como paciente, podrá realizar consultas sanitarias de dos tipos. Unas personales y otras no personales.
    - Las personales se procesarán en una cola con prioridades en función de la urgencia (derivada a partir de los síntomas expuestos por el paciente). Si la consulta revisada, es aceptada, se procede a enlazar una comunicación en tiempo real con un doctor especialista y dicho paciente. Una vez terminada, se archiva el historial de consulta así como otros parámetros afectados y terminan las comunicaciones.
    - Las no personales se activan mediante un formulario con parámetros a rellenar como la temperatura corporal, tensión arterial, ... Añadiendo un campo para que los pacientes puedan expresar libremente sus síntomas. Este formulario se procesa automáticamente para buscar valores fuera de rango y así establecer una prioridad. Más tarde, un equipo de doctores especializados en este tipo de consultas, la revisarán y decidirán su tratamiento así como sí es necesario, dependiendo de la prioridad y el tipo de síntomas, una atención médica presencial.

2. Para el monitoreo y seguimiento de pacientes crónicos o terminales, se les otorgará, además de los dispositivos IoT de seguimiento, un dispositivo para comunicaciones urgentes (como por ejemplo en el caso de ausencia de una sesión de monitoreo). Estos pacientes deben autenticarse en el sistema (el cual los tiene previamente a su uso clasificados) y además de las funcionalidades de los pacientes de Tipo_1, tienen un apartado de monitorización en el que figura su historial de seguimiento hecho con el dispositivo IoT y que se actualiza en tiempo real. Para la actualización de dicho historial se pueden dar estos casos:
    - Los datos recibidos del dispositivo IoT son normales (con lo cual el sistema no tiene que reaccionar de ninguna forma aparte de actualizar el historial).
    - Los datos recibidos del dispositivo IoT son anómalos. Esto generará una alerta en el sistema que hará que el CEP (Complex Event Processing) lo procese inmediatamente (ya que es prioritario, en tiempo real y dirigido por eventos).


## 2. Stakeholders

Gurpos de prioridad | Stakeholder | Descripción 
 --- | --- | --- 
 **Grupo de prioridad 1** | Paciente Tipo 1 (paciente con seguimiento de enfermedad crónica),especialistas sanitarios y emergencias (sistema automatizado de ambulancias) | Usuarios críticos y más importantes del sistema.
 **Grupo de prioridad 2** | Paciente Tipo 2 (pacientes sin seguimiento de enfermedad crónica) y médicos de consulta | Usuarios importantes pero sin necesidad de urgencia ni total disponibilidad.
 **Grupo de prioridad 3** | Instituciones sanitarias (públicos y/o privados) y Comunidad de Madrid | Aprueban el uso del sistema.
 **Grupo de prioridad 4** | Equipo desarrollo y mantenimiento Software | Encargados del mantenimiento y correcto funcionamiento del sistema.
 **Grupo de prioridad 5** | Empresa encargada de la recogida de información relativa a los índices de calidad de vida en la ciudad | Añaden información al sistema para mejorar la vida de los usuarios.
 **Grupo de prioridad 6** | Equipo de marketing y ventas | Distribuyen información para fomentar la implantación del sistema y su uso.


## 3. Atributos de calidad

Para el orden de prioridad se ha usado la técnica de **dote-voting**.

1. **Disponibilidad**: Nuestro sistema necesita estar operativo para ofrecer un buen servicio a nuestros usuarios, disponemos de 2 tipos de disponibilidad:
     - Disponibilidad siempre operativa: para pacientes con seguimiento de enfermedad crónica en tiempo real, necesitaremos que el sistema esté operativo 24/7/365
     - Disponibilidad limitada: para el resto de pacientes que son gestionados por el sistema, tendremos una franja horaria para el uso de la aplicación.
2. **Usabilidad**: Como nuestro sistema está diseñado para cualquier tipo de usuario (edad, discapacidad…) deberá ser lo más simple y fácil de usar.
3. **Seguridad**: Los datos de todos los usuarios de la aplicación deben estar cifrados y la información debe estar protegida cumpliendo la LOPD.
4. **Interoperabilidad**: Las distintas partes de la aplicación deberán estar integradas entre ellas correctamente y compartir datos de unas a otras.
5. **Rendimiento**: Nuestro sistema tiene que funcionar con tiempos de respuestas cortos consiguiendo una eficiencia excelente.
6. **Portabilidad**: La aplicación sea usable en cualquier dispositivo y sistema.
7. **Mantenibilidad**: El sistema debe mantenerse en el tiempo sin reducir su rendimiento y sus funciones.
9. **Modificabilidad**: El sistema debe soportar modificaciones manteniendo su integridad..
10. **Escalabilidad**: Se necesita que nuestro sistema soporte una gran carga de usuarios y datos.


### 3.1 Descripción de los atributos de calidad más importantes y su priorización justificada

### 3.2 Árbol de utilidad

 Atributo de calidad       | Atributo refinado          | ASR 
---| --- | ---
 **Disponibilidad** | Aplicación siempre operativa | Los pacientes tipo 1 necesitan que la aplicación esté siempre operativa para el seguimiento de la enfermedad crónica de forma que en todo momento se conozca el estado de los pacientes (H,H)
 \- | Aplicacion Limitada | Los pacientes tipo 2 tendrán limitados a un horario el uso de la aplicación para consultas con los médicos. (H,H)
 \- | Sistema crítico | Como de la aplicación dependen vidas humanas, debemos mantener un plan alternativo en caso de un fallo crítico del sistema.  (H,H)
 \- | Datos de los pacientes | Como la aplicación toma decisiones usando los parámetros de los pacientes, los dispositivos de los pacientes deben estar actualizando sus parámetros en la base datos y a su vez la base de datos debe estar disponible para que los especialistas sanitarios puedan leer esta información. (H,H)
**Usabilidad** | Facilidad de uso | Necesidad de que la aplicación sea fácilmente entendible e intuitiva para todo tipo de usuarios. (H,M)
**Seguridad** |   Integridad de los datos   |   Como cliente necesito que la seguridad de los datos (tanto de los pacientes como de los especialistas) sea íntegra para evitar problemas con los usuarios. (H,H)
 \- | Restricción al acceso de los datos | Como cliente necesito que exista una restricción al acceso de los datos para aumentar la seguridad de la aplicación y de los usuarios. (H,M)
 **Interoperabilidad** | comunicacion e integracion de las partes del sistema | las diferentes partes del sistema deben comunicarse entre sí para funcionar correctamente y compartir datos (H,M)
 **Portabilidad** | Disponibilidad en distintos sistemas operativos y dispositivos |  Como cliente necesito que la aplicación sea usable en cualquier dispositivo independientemente del sistema operativo utilizado.   (M,M)
 **Mantenibilidad** | Cambios en el sistema | Como cliente necesito que se puedan realizar cambios para poder mejorar la aplicación. (M,M)
 **Rendimiento** | Funcionalidad correcta en tiempos de respuesta y de ejecución cortos | La aplicación deberá funcionar con una respuesta rápida entre sus módulos. (H,M)
 **Modificabilidad** | Actualización de los parámetros recogidos de los pacientes | Como cliente necesito que el sistema pueda actualizar los datos de los pacientes. (M,M)
 \- | Actualización de los parámetros que establecen los especialistas sanitarios | Se actualizará la información relativa a tratamientos, medicamentos, fármacos…(M,M)
 \- | Actualización de los niveles de contaminación y alérgenos | El sistema tiene que ser capaz de actualizar la información referente a los niveles de contaminación y alérgenos de la ciudad. (M,M)
 **Escalabilidad** | Soporte de varios usuarios simultaneamente | El sistema deberá soportar que grandes cantidades de usuarios accedan simultáneamente sin problema y con alto rendimiento. (M,L)

## 4. Vistas arquitectónicas

### 4.1 Vista lógica

#### 4.1.1 Descripción

#### 4.1.2 Notación

#### 4.1.3 Vista

#### 4.1.4 Catálogo

#### 4.1.5 Rationale

### 4.2 Vista de procesos

#### 4.2.1 Descripción

#### 4.2.2 Notación

#### 4.2.3 Vista

#### 4.2.4 Catálogo

#### 4.2.5 Rationale

### 4.3 Vista de desarrollo 

#### 4.3.1 Descripción

#### 4.3.2 Notación

#### 4.3.3 Vista

#### 4.3.4 Catálogo

#### 4.3.5 Rationale

## 4.4 Vista de despliegue

#### 4.4.1 Descripción

#### 4.4.2 Notación

#### 4.4.3 Vista

#### 4.4.4 Catálogo

#### 4.4.5 Rationale

### 4.5 Escenarios

#### 4.5.1 Descripción

#### 4.5.2 Notación

#### 4.5.3 Vista

#### 4.5.4 Catálogo

#### 4.5.5 Rationale

## 5. Trazabilidad

### 5.1 Entrevistas

### 5.2 Entre Business Goals y vistas

### 5.3 Entre atributos de calidad y vistas

## 6. Conclusiones

### 6.1 Relativas a la arquitectura

### 6.2 Personales

