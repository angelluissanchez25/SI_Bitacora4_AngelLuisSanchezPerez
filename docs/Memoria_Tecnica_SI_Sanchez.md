# 

# 

# **BITACORA 4**

**Ángel Luis Sánchez Pérez**

**Desarrollo de aplicaciones multiplataforma**

**15/05/26**

# **Índice**

[**1\. Análisis de Necesidades	3**](#1.-análisis-de-necesidades)

[1.1. Contexto y Problemática Actual	3](#1.1.-contexto-y-problemática-actual)

[1.2. Solución Propuesta: Infraestructura Híbrida Docker-Guacamole	3](#1.2.-solución-propuesta:-infraestructura-híbrida-docker-guacamole)

[1.3. Justificación Técnica y Beneficios (TCO)	4](#1.3.-justificación-técnica-y-beneficios-\(tco\))

# 1\. Análisis de Necesidades {#1.-análisis-de-necesidades}

## 1.1. Contexto y Problemática Actual {#1.1.-contexto-y-problemática-actual}

La infraestructura tecnológica de la organización presentaba una serie de vulnerabilidades críticas derivadas de una gestión descentralizada de los accesos remotos. Anteriormente, la administración de servidores de bases de datos y entornos de desarrollo dependía de la apertura individual de puertos en el firewall para protocolos RDP y SSH. Esta práctica no solo incrementaba exponencialmente la superficie de ataque ante posibles intrusiones externas, sino que también complicaba la auditoría de conexiones y la gestión de identidades.

Desde el punto de vista operativo, la dependencia de clientes de escritorio pesado limitaba la movilidad de los técnicos y generaba conflictos de dependencias en las estaciones de trabajo locales. La falta de una capa de abstracción entre el usuario y el recurso final derivaba en una infrautilización de los recursos de red y una gestión ineficiente de las actualizaciones de seguridad en los puntos finales.

## 1.2. Solución Propuesta: Infraestructura Híbrida Docker-Guacamole {#1.2.-solución-propuesta:-infraestructura-híbrida-docker-guacamole}

Para mitigar los riesgos identificados y optimizar la operatividad, se ha implementado una solución basada en Apache Guacamole desplegada mediante la tecnología de contenedores Docker Compose. Esta arquitectura permite centralizar todo el flujo de trabajo en un único punto de acceso vía navegador (puerto 8080/443), eliminando la necesidad de exponer servicios internos directamente a la red pública.

La elección de Docker como motor de despliegue responde a la necesidad de aislamiento y portabilidad. Cada componente de la pila (PostgreSQL para la persistencia, Guacd para la gestión de protocolos y el túnel de Guacamole) opera en un entorno estanco. Por consiguiente, se garantiza que las dependencias de un servicio no interfieran con otros, facilitando enormemente las tareas de mantenimiento y escalabilidad. Asimismo, esta estructura permite una recuperación ante desastres (DRP) casi instantánea, ya que la infraestructura puede ser recreada en cualquier host compatible en cuestión de segundos mediante la ejecución de los archivos de configuración YAML.

En conclusión, la solución no solo resuelve el problema de seguridad mediante la centralización y el cifrado de la sesión en el navegador, sino que profesionaliza la entrega de servicios TI, permitiendo un control granular de quién accede, cuándo y a qué recurso exacto dentro de la red corporativa.

## 

## 1.3. Justificación Técnica y Beneficios (TCO) {#1.3.-justificación-técnica-y-beneficios-(tco)}

La implementación de esta infraestructura bajo el paradigma de software libre permite una reducción significativa del Coste Total de Propiedad (TCO). Al utilizar licencias permisivas como la Apache License 2.0 y la PostgreSQL License, la organización queda exenta de costes de licenciamiento recurrentes y de la dependencia de proveedores específicos (vendor lock-in). Esto permite reinvertir el capital en la mejora de la seguridad física y lógica de los sistemas.  
Además, se observa una mejora en la eficiencia de los recursos de hardware, ya que la sobrecarga (overhead) de los contenedores Docker es mínima en comparación con la virtualización tradicional, permitiendo una mayor densidad de servicios por servidor físico.

# 2. Estimación de Costes de Infraestructura

<img src="Captura de pantalla 2026-05-22 090013.png" alt="captura">

# 3. Estrategia de Despliegue y Comunicación

## 3.1. Sistema de Transferencia de Ficheros Seguro

Para el despliegue de nuestra aplicación desde el entorno de desarrollo local hacia la infraestructura de producción, utilizaremos el protocolo SFTP (SSH File Transfer Protocol), integrado con flujos de despliegue continuo (CI/CD) mediante GitHub Actions.

Descartamos por completo el uso de FTP tradicional, ya que transmite las credenciales y los datos en texto plano, exponiendo el proyecto a ataques de interceptación. A diferencia de FTPS, SFTP opera de forma nativa sobre el protocolo SSH. Esto nos garantiza que todo el tráfico de datos, comandos y autenticaciones viaje cifrado mediante algoritmos de alta seguridad como AES-256.

La autenticación no se realizará mediante contraseñas vulnerables, sino utilizando un par de claves criptográficas SSH. La clave pública se alojará de forma segura en nuestro servidor VPS y la clave privada se gestionará de manera hermética. Así garantizamos una transferencia de ficheros automatizada, íntegra y completamente blindada.

## 3.2. Canales de Mensajería e Integración de Alertas

Para la gestión del proyecto y el soporte operativo, nuestro equipo utilizará Discord como centro de operaciones y mensajería electrónica. No se limitará a la comunicación presencial entre las parejas del proyecto.

En caso de que ocurra una incidencia técnica, herramientas de monitorización como Uptime Kuma o AWS CloudWatch enviarán un ping automático inmediato a Discord. Esto nos permitirá reaccionar en tiempo real, centralizar los logs del sistema y garantizar la alta disponibilidad de la aplicación sin depender de revisiones manuales.

