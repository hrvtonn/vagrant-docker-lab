Vagrant.configure("2") do |config|
  # Etapa 1: Definição da Imagem Base
  config.vm.box = "ubuntu/jammy64"

  # Etapa 5: Provisionamento Automático
  $script = <<-SCRIPT
    export DEBIAN_FRONTEND=noninteractive

    # Atualizar lista de pacotes e instalar utilitários essenciais
    apt-get update
    apt-get install -y curl git unzip ca-certificates software-properties-common

    # Instalar Docker
    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    chmod a+r /etc/apt/keyrings/docker.asc

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null
    
    apt-get update
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    # Garantir permissões do Docker para o usuário padrão
    usermod -aG docker vagrant
  SCRIPT

  (1..3).each do |i|
    config.vm.define "vm#{i}" do |node|
      node.vm.hostname = "vm#{i}"
      
      # Etapa 3: Configurações de Rede
      # IP fixo único para evitar conflitos
      node.vm.network "private_network", ip: "192.168.56.#{10 + i}"
      
      # Redirecionamento de portas (mapeando com um offset para não haver conflito entre as 3 VMs)
      # Exemplo: a VM 1 vai expor a porta 3001, a VM 2 a 3002, etc.
      node.vm.network "forwarded_port", guest: 3000, host: 3000 + i, auto_correct: true # Frontend
      node.vm.network "forwarded_port", guest: 8080, host: 8080 + i, auto_correct: true # Backend
      node.vm.network "forwarded_port", guest: 5432, host: 5430 + i, auto_correct: true # Banco de Dados
      
      # Etapa 4: Sincronização de Pastas
      # Espelhamento do diretório local para um diretório raiz na VM (/vagrant)
      node.vm.synced_folder ".", "/vagrant"

      # Etapa 2: Dimensionamento de Hardware
      node.vm.provider "virtualbox" do |vb|
        vb.name = "vm#{i}-dev"
        vb.memory = "2048"
        vb.cpus = 2
      end
      
      node.vm.provision "shell", inline: $script
    end
  end
end
