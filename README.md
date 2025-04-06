# Ejecicio: levantar un cluster de k3s 

El proyecto consiste en crear un cluster de kubernetes usando la herramienta de k3s, tener las siguientes caracteristicas

- Red privada entre nodos con la ip 192.168.56.X 
- Instalar Helm
- Instalar ArgoCD en su propio namespace
- Levantar Nginx en un Daemon Set en su propio namespace

        !Este ejecicio es para pruebas locales y no para produccion
    
    

## Caracteristicas de los nodos

- 2 nucles
- 4 Gb de RAM

## Herramientas a usar

- [Vagrant](https://www.vagrantup.com/) - Levantar infraestructura local
- [Libvirt](https://libvirt.org/) - Herramienta de virtualizacion en ubuntu
- [K3s](https://k3s.io/)     - Gestor de kubernetes
- [Lens](https://k8slens.dev/)    - Herramienta visual de kubernetes
- [helm](https://helm.sh/)    - Gestor de paquetes para kubernetes

## Creacion del cluster

1. Instalacion de vagrant local

    Para la instalacion de dicha herramienta se debe de ir al sitio oficial de [hashicorp/vagrant](https://developer.hashicorp.com/vagrant/install) y posteriormente ir a la seccion correspondiente en este caso ubuntu
    ```

    wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

    sudo apt update && sudo apt install vagrant

    ```

2. Instalacion Libvirt / plugin Vagrant
    ```
    # Instalacion de libvirt
    sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils -y

    # Instalacion de librerias adiciones para el plugin de vagrant
    sudo apt-get -y install libxslt-dev libxml2-dev libvirt-dev zlib1g-dev ruby-dev

    # Instalacion Plugin libvirt - Vagrant 
    vagrant plugin install vagrant-libvirt

    # Instalacion del gestor VM
    sudo apt install virt-manager -y
    
    # Agregar usuario al grupo de libvirt y kvm
    sudo adduser [username] libvirt
    sudo adduser [username] kvm
    ```

3. Levantar instancias de ubuntu 20.04
   
   En base a los requerimientos solicitados por la practica se hace uso del archivo **Vagrantfile** en esta documentacion.

   ```
   # En la ruta donde este el archivo **Vagrantfile**
   - vagrant up
   ```

4. Instalar helm en nodo maestro
   
   Siguiendo las instrucciones de la pagina oficial de [helm](https://helm.sh/) se instala de la siguiente manera:

   ```
   # Descargar el script de instalacion

   curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
   
   # Brindar permisos de lectura, escritura y ejecucion al owner
   chmod 700 get_helm.sh
   
   # Ejecutar script
   ./get_helm.sh

   ```


5. Instalar k3s en nodo maestro
    
   `Nota: Sabemos que se puede automatizar haciendo uso de vagrant directamente con los providers o haciendo uso de ansible, sin embargo para este ejemplo se realizara manual.`
   
   Ejecutar el siguiente comando en nodo maestro
   ```
   curl -sfL https://get.k3s.io | sh -
   ```

6. Firewall

    Para este Ejercicio ya sea desactivar el firewall o permitir el trafico

    - Desactivar
    
            ufw disable

    - Permitir el trafico
            
            ufw allow 6443/tcp #Api server
            ufw allow from 10.42.0.0/16 to any #Pods
            ufw allow from 10.43.0.0/16 to any #Services

7. Obtener Token de k3s

    Una vez instado todo lo anterior y que se haya terminado todo sin errores tendremos que ir al siguiente paso y es anexar los nodos a nuestro cluster para ello requerimos obtener el token de nuestro nodo con el siguiente comando

   ```
   cat /var/lib/rancher/k3s/server/node-token
   ```


7. Agregar nodo a cluster k3s
   ```
    curl -sfL https://get.k3s.io | K3S_URL=https://192.168.56.10:6443 K3S_TOKEN=<Token> sh -
   ```

# Agregar cluster a Lens
   Agregar contexto de nuestro cluster a Lens **kubeconfig**
   
   Para poder montar el cluster en aplicativo en lens deberemos cargar el contexto que es el archivo de configuracion de kubernetes por lo cual requerimos ejecutar el siguiente comando
   ```
    cat /etc/rancher/k3s/k3s.yaml --> 
   ```
   Copiamos y creamos un archivo en nuestro equipo y posterior a ello daremos en agregar dentro del aplicativo de lens ya seleccionaremos nuestro archivo.
    
# Desplegar ArgoCD



# Referencias
- [Documentacion Vagrant](https://developer.hashicorp.com/vagrant/install)
- [Instalacion libvirt](https://phoenixnap.com/kb/ubuntu-install-kvm)
- [Instalacion Vagrant - Libvirt Plugin](https://gist.github.com/PaulNeumann/81f299a7980f0b74cec9c5cc0508172b)