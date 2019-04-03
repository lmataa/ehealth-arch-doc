---
title: "Documentación de una arquitectura"
titlepage: "True"
date: 02-04-2019
author: [Luis Mata Aguilar, ]
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
    - Consultas personales con un doctor por medios telemáticos.

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

### 1.3 Business Drivers




## 2. Stakeholders


## 3. Atributos de calidad

Atributo de calidad | Prioridad | Justificación
---| --- | ---


### 3.1 Descripción de los atributos de calidad más importantes y su priorización justificada

### 3.2 Árbol de utilidad

Atributo de calidad       | Atributo refinado          | ASR
---| --- | ---
 Disponibilidad | Aplicación siempre operativa | Como cliente necesito que la aplicación        esté siempre operativa para poder resolver dudas puntuales y consultas personales vía telemática.
 - | Sistema crítico | Como de la aplicación depende vidas humanas, debemos mantener un plan alternativo en caso de un fallo crítico del sistema.
 - | Datos de los pacientes | Como la aplicación toma decisiones usando los parámetros de los pacientes, los dispositivos de los pacientes deben estar actualizando sus parámetros en la base datos y a su vez la base de datos debe estar disponible para que los especialistas sanitarios puedan leer esta información.
 Usabilidad    |   duda(meter funciones que hace la aplicación)  |
 Seguridad |   Integridad de los datos   |   Como cliente necesito que la seguridad de los datos (tanto de los pacientes como de los especialistas) sea íntegra para evitar problemas con los usuarios.
 - | Restricción al acceso de los datos | Como cliente necesito que exista una restricción al acceso de los datos para aumentar la seguridad de la aplicación y de los usuarios.
 Interoperabilidad |  |
 Portabilidad | Disponibilidad en distintos sistemas operativos y dispositivos |  Como cliente necesito que la aplicación sea usable desde diferentes dispositivos independientemente del sistema operativo utilizado.
 Testabilidad | Capacidad del sistema para ser probado | Como cliente necesito que el sistema se pueda probar de forma sencilla para evitar y/o solucionar posibles errores.
 Mantenibilidad | Cambios en el sistema | Como cliente necesito que se puedan realizar cambios para poder mejorar la aplicación.
 Rendimiento | Funcionalidad correcta en tiempos de respuesta cortos | La aplicación deberá funcionar con una respuesta rápida entre sus módulos.
 Modificabilidad | Actualización de los parámetros recogidos de los pacientes | Como cliente necesito que el sistema pueda actualizar los datos de los pacientes.
 - | Actualización de los parámetros que establecen los especialistas sanitarios | Se actualizará la información relativa a tratamientos, medicamentos, fármacos…
 - | Actualización de los niveles de contaminación y alérgenos | El sistema tiene que ser capaz de actualizar la información referente a los niveles de contaminación y alérgenos de la ciudad.
 Escalabilidad | Soporte de varios usuarios simultaneamente | El sistema deberá soportar que grandes cantidades de usuarios accedan simultaneamente sin problema y con alto rendimiento.
 Forma de venta(marketing) | Alta distribución de información acerca del sistema | Como cliente quiero que el sistema sea usado por el mayor número de usuarios ya que mejora considerablemente su nivel de vida y por ello se necesita que el sistema y sus beneficios sea conocido por el mayor número de personas.

