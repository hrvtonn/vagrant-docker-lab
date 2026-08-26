# 🚀 Ambiente de Desenvolvimento Automatizado com Vagrant e Docker

Este projeto demonstra a criação e orquestração de uma infraestrutura local utilizando o **Vagrant** e o **VirtualBox**. Ele automatiza a criação de 3 máquinas virtuais Linux (Ubuntu) prontas para desenvolvimento, com ferramentas essenciais e Docker já instalados.

## 🛠️ Tecnologias Utilizadas

- [Vagrant](https://www.vagrantup.com/) (Orquestração de Máquinas Virtuais)
- [VirtualBox](https://www.virtualbox.org/) (Provedor de Virtualização)
- Ubuntu 22.04 LTS (Jammy Jellyfish)
- Shell Script (Provisionamento)
- Docker & Docker Compose

## ⚙️ Arquitetura do Ambiente

O `Vagrantfile` provisiona simultaneamente **3 máquinas virtuais** com as seguintes especificações:
- **SO:** Ubuntu 22.04
- **Recursos:** 2 GB de RAM e 2 vCPUs por máquina.
- **Rede:** IPs estáticos configurados e portas redirecionadas para acesso ao Host.
  - `vm1`: IP `192.168.56.11` (Portas: 3001, 8081, 5431)
  - `vm2`: IP `192.168.56.12` (Portas: 3002, 8082, 5432)
  - `vm3`: IP `192.168.56.13` (Portas: 3003, 8083, 5433)
- **Volumes:** O diretório local é sincronizado automaticamente para a pasta `/vagrant` dentro das VMs.

### Provisionamento Automático
Durante a primeira inicialização (`vagrant up`), um script shell é executado automaticamente em todas as máquinas para:
1. Atualizar os pacotes do sistema (`apt-get update`).
2. Instalar ferramentas básicas: `curl`, `git`, `unzip`.
3. Instalar o **Docker** e o **Docker Compose**.
4. Conceder permissões para o usuário padrão rodar contêineres sem `sudo`.

## 🚀 Como Executar

### Pré-requisitos
Certifique-se de ter o [VirtualBox](https://www.virtualbox.org/wiki/Downloads) e o [Vagrant](https://developer.hashicorp.com/vagrant/downloads) instalados em sua máquina.

### Iniciando a Infraestrutura

1. Clone este repositório e acesse a pasta do projeto:
   ```bash
   git clone https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git
   cd SEU_REPOSITORIO
   ```

2. Inicialize as máquinas virtuais:
   ```bash
   vagrant up
   ```
   *(Este processo pode demorar alguns minutos dependendo da sua conexão, pois baixará a imagem do Ubuntu e instalará o Docker).*

3. Para acessar via SSH qualquer uma das máquinas (ex: vm1):
   ```bash
   vagrant ssh vm1
   ```

4. Para desligar as máquinas:
   ```bash
   vagrant halt
   ```

5. Para destruir o ambiente e liberar espaço em disco:
   ```bash
   vagrant destroy -f
   ```

## 📚 Objetivos de Aprendizado
Este projeto foi desenvolvido para consolidar conhecimentos em **Infraestrutura como Código (IaC)**, redes de máquinas virtuais, provisionamento via scripts e preparação de ambientes de desenvolvimento padronizados.
