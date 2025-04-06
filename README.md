# Ejecicio: levantar un cluster de k3s 

El proyecto consiste en crear un cluster de kubernetes usando la herramienta de k3s, tener las siguientes caracteristicas

- Red privada entre nodos con la ip 192.168.56.X 
- Instalar Helm
- Instalar ArgoCD en su propio namespace
- Levantar Nginx en un Daemon Set en su propio namespace

## Caracteristicas de los nodos

- 2 nucles
- 4 Gb de RAM

## Herramientas a usar

- Vagrant - Levantar infraestructura local
- Libvirt - Herramienta de virtualizacion en ubuntu
- K3s     - Gestor de kubernetes
- Lens    - Herramienta visual de kubernetes

## Proceso

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

4. Instalar k3s en nodo maestro
curl -sfL https://get.k3s.io | sh -

# Obtener Token de k3s

/etc/rancher/k3s/k3s.yaml

# Agregar nodo a cluster 

curl -sfL https://get.k3s.io | K3S_URL=https://192.168.56.10:6443 K3S_TOKEN=<Token> sh -

# Crear kubeconfig
kubectl config view --minify --raw --> Ejecutar en nodo Master

# Referencias
- [Documentacion Vagrant](https://developer.hashicorp.com/vagrant/install)
- [Instalacion libvirt](https://phoenixnap.com/kb/ubuntu-install-kvm)
- [Instalacion Vagrant - Libvirt Plugin](https://gist.github.com/PaulNeumann/81f299a7980f0b74cec9c5cc0508172b)