
Vagrant.configure("2") do |config|

  # Imagen base obtenida de Vagrant Cloud
  # https://app.vagrantup.com/generic/boxes/ubuntu2004
  config.vm.box = "generic/ubuntu2004"


  # No se simplifica el levantamiento haciendo uso de ciclos o relacionados ya que se requiere la facil comprencion del Ejercicio
  # Caracteristicas del Nodo maestro
  config.vm.define "k3s-master" do |master|
    master.vm.hostname = "k3s-master" # Nombre del nodo maestro
    master.vm.network "private_network", ip: "192.168.56.10" # IP del nodo maestro
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "2048" # Memoria RAM del nodo maestro
      vb.cpus = 2 # Numero de CPUs del nodo maestro
    end
    master.vm.provision "shell", inline: <<-SHELL
      apt update
    SHELL
  end
# Caracteristicas del Nodo trabajador 1
  config.vm.define "k3s-worker" do |worker|
    worker.vm.hostname = "k3s-worker" # Nombre del nodo trabajador 1
    worker.vm.network "private_network", ip: "192.168.56.11" # IP del nodo trabajador 1
    worker.vm.provider "virtualbox" do |vb|
      vb.memory = "2048" # Memoria RAM del nodo trabajador 1
      vb.cpus = 2 # Numero de CPUs del nodo trabajador 1
    end
    worker.vm.provision "shell", inline: <<-SHELL
      apt update
    SHELL
  end
# Caracteristicas del Nodo trabajador 2
config.vm.define "k3s-worker2" do |worker_2|
  worker_2.vm.hostname = "k3s-worker2" # Nombre del nodo trabajador 2
  worker_2.vm.network "private_network", ip: "192.168.56.12" # IP del nodo trabajador 2
  worker_2.vm.provider "virtualbox" do |vb|
    vb.memory = "2048" # Memoria RAM del nodo trabajador 2
    vb.cpus = 2 # Numero de CPUs del nodo trabajador 2
  end
  worker_2.vm.provision "shell", inline: <<-SHELL
    apt update
  SHELL
end
# Caracteristicas del Nodo trabajador 3
config.vm.define "k3s-worker3" do |worker_3|
  worker_3.vm.hostname = "k3s-worker3" # Nombre del nodo trabajador 3
  worker_3.vm.network "private_network", ip: "192.168.56.13" # IP del nodo trabajador 3
  worker_3.vm.provider "virtualbox" do |vb|
    vb.memory = "2048" # Memoria RAM del nodo trabajador 3
    vb.cpus = 2 # Numero de CPUs del nodo trabajador 3
  end
  worker_3.vm.provision "shell", inline: <<-SHELL
    apt update
  SHELL
end
end