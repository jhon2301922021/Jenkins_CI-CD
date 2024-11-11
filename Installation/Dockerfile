# Utilizar la versión de Soporte a Largo Plazo (LTS) de Jenkins como la imagen base
FROM jenkins/jenkins:lts

# Cambiar al usuario root para realizar operaciones a nivel de sistema
USER root

################# Preparar un entorno de Jenkins que pueda trabajar eficientemente con Docker #################
# Instalar el complemento docker-workflow para habilitar a Jenkins a trabajar con Docker
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

# Actualizar la lista de paquetes sin imprimir la lista de paquetes (modo silencioso) e instalar paquetes esenciales para la instalación
RUN apt-get update -qq \
  && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common

# Descargar la clave GPG del repositorio oficial de Docker y añadirla al sistema para verificar la autenticidad de los paquetes Docker
RUN mkdir -p /etc/apt/keyrings \
  && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Añadir el repositorio de Docker a la lista de repositorios de software del sistema
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
  | tee /etc/apt/sources.list.d/docker.list > /dev/null

# Actualizar la lista de paquetes en modo silencioso e instalar Docker
RUN apt-get update -qq \
  && apt-get install -y docker-ce docker-ce-cli containerd.io

# Instalar AWS CLI para interactuar con los Servicios Web de Amazon
RUN apt-get install -y awscli

# Instalar Python3 y pip
RUN apt-get install -y python3 python3-pip

# Instalar GnuPG para la gestión de claves y operaciones de seguridad
RUN apt-get install -y gnupg

# Añadir el usuario de jenkins al grupo docker para permitir que Jenkins ejecute comandos de Docker
RUN usermod -aG docker jenkins

# Cambiar de nuevo al usuario Jenkins
USER jenkins
