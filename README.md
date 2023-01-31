# UT3.PC01

***Segunda parte: Docker-Bench***

En la etapa de deploy nos interesa analizar el entorno. En el caso de Docker nos interesa ver cómo está todo montado, si está bien implementado y que cuenta con todos los requisitos necesarios. Para ello contamos con herramientas cómo Docker-bench, que se basa en una guía que es el CIS Docker Bench.

1. Utiliza docker bench y realiza un análisis previo de tu Docker

![Untitled](UT3%20PC01%20a093f1d7c326423d8d5e3a624f834bb7/Untitled.png)

1. Utiliza AuditD para que analice todas las pruebas de la Sección A, referente al host configuration.

**Editar el fichero /etc/audit/rules.d/audit.rules e incluir el siguiente contenido.**

-w /run/containerd -p wa

-w /var/lib/docker -p wa

-w /etc/docker -p wa

-w /etc/default/docker -p wa

-w /etc/docker/daemon.json -p wa

-w /etc/containerd/config.toml -p wa

-w /etc/sysconfig/docker -p wa

-w /usr/bin/containerd -p wa

-w /usr/bin/containerd-shim -p wa

-w /usr/bin/containerd-shim-runc-v1 -p wa

-w /usr/bin/containerd-shim-runc-v2 -p wa

-w /usr/bin/runc -p wa

![Untitled](UT3%20PC01%20a093f1d7c326423d8d5e3a624f834bb7/Untitled%201.png)

1. Comenta 2 warnings que creas convenientes, y explica qué posible solución tendría.

**[WARN] 2.2 - Ensure network traffic is restricted between containers on the default bridge (Scored)**

De forma predeterminada, se permite todo el tráfico de red entre contenedores en el mismo host en el puente de red predeterminado.

Edite el archivo de configuración del demonio de Docker para asegurarse de que icc esté deshabilitado. Debe incluir la siguiente configuración

'icc': false Como alternativa, ejecute el demonio docker directamente y pase --icc=false como argumento. Por ejemplo, dockerd --icc=false Como alternativa, puede seguir la documentación de Docker y crear una red personalizada y unir solo los contenedores que necesitan comunicarse con esa red personalizada. El parámetro --icc solo se aplica al puente docker predeterminado; si se usan redes personalizadas, se debe adoptar el enfoque de segmentación de redes.

**[WARN] 2.9 - Enable user namespace support (Scored)**

Debe habilitar la compatibilidad con el espacio de nombres de usuario en el demonio Docker para utilizar el usuario contenedor para alojar la reasignación de usuarios. Esta recomendación es beneficiosa cuando los contenedores que está utilizando no tienen un usuario de contenedor explícito definido en la imagen del contenedor.

Paso 1: Asegúrese de que existan los archivos /etc/subuid y /etc/subgid.

touch /etc/subuid /etc/subgid Paso 2: Inicie el demonio docker con --userns-remap flag dockerd --userns-remap=default Valor predeterminado: De manera predeterminada, el espacio de nombres de usuario no se reasigna. Se debe considerar la implementación de esto de acuerdo con los requisitos de las aplicaciones que se utilizan y la política de seguridad de la organización.

***Tercera parte: Análisis de archivos dockerfile***

Elegid cualquier archivo dockerfile que hayáis generado en la actividad *[UT3.AP00a - Afianzando el uso de Dockerfile.](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/assign/view.php?id=454669)* y realizar un testeo con Trivy.

Para comenzar instalamos trivy:

• `docker run aquasec/trivy`

Una vez instalado para realizar un test lo haremos con el siguiente comando:

$trivy filesystem Dockerfile

Se descargara la actualizacion de su base de datos y comenzara a testear:

![Untitled](UT3%20PC01%20a093f1d7c326423d8d5e3a624f834bb7/Untitled%202.png)

***Cuarta parte: Análisis de imágenes***

Debéis escanear la imagen creada de Wordpress que hayáis generado en la actividad *[UT3.AP03 - Docker-compose Wordpress](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/assign/view.php?id=454668)*  con Trivy, Snyk o Docker Desktop. A continuación, descargate la versión wordpress:4.6 y realiza el mismo proceso.

Haz una comparativa en cuanto estadísticas y puntos críticos de ambas.

Escaneo de mi wordpress:

![Untitled](UT3%20PC01%20a093f1d7c326423d8d5e3a624f834bb7/Untitled%203.png)

Escaneo de wordpress 4.6:

De tantas vulnerabilidades que tiene, no encuentro el resumen como en el anterior caso, se sale de la terminal.

Mi wordpress al ser una versión mas actualizada podemos apreciar a simple vista que contiene menos vulnerabilidades que la version 4.6.