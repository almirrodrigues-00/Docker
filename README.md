# Docker


Como instalar o docker e docker-compose no ubuntu 20.04 LTS
Vamos iniciar realizando a configuração para podermos instalar o Docker Engine e o Docker Compose através dos repositórios Docker e Docker Compose.

Exclua todas as versões anteriores se você as tiver instalado.
sudo apt-get remove docker docker-engine docker.io containerd runc

Atualize seu sistema e instale as dependências necessárias.
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

Para fins de segurança, adicione a chave GPG oficial do Docker. Leia mais sobre isso aqui.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Configure um repositório estável.

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
se até aqui deu tudo certo, você está pronto para instalar o Docker!

Instalando o Docker
Instale a versão mais recente do Docker, com todas as suas dependências.
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
Se você estiver executando uma versão do Ubuntu mais recente do que a versão oficial do Docker, você poderá receber o seguinte erro durante esta etapa.

Reading package lists... Done
Building dependency tree
Reading state information... Done
Package docker-ce is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'docker-ce' has no installation candidate
E: Unable to locate package docker-ce-cli
E: Unable to locate package containerd.io
E: Couldn't find any package by glob 'containerd.io'
E: Couldn't find any package by regex 'containerd.io'
Mas não se preocupe! Só precisamos instalar a versão mais recente disponível. Isso pode ser feito com o seguinte comando abaixo.
sudo apt-get install -y docker.io

Verifique se ocorreu tudo bem na sua instalação executando sua primeira imagem.
sudo docker run hello-world

Adicione seu usuário como administrador ao grupo de usuários do Docker, para não utilizar "sudo" ao executar comandos do Docker!
sudo usermod -aG docker $USER

Desinstalar
Não vai mais utilizar o Docker e quer removelo? Desinstale e remova os arquivos de configuração com os comandos a seguir.
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
Instalar o Docker-composer
Para instalar o docker-composer, você deverá ter concluído a instalação do Docker Engine nas etapas acima. Supondo que você tenha feito isso sem erros, vamos continuar!

Primeiro crie um diretorio e baixe a versão estável do Docker compose.
 mkdir -p ~/.docker/cli-plugins/
 curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
Ao contrário da instalação do Docker Engine, onde ele instala automaticamente a versão mais recente, o Docker Compose precisa ter a versão especificada manualmente. No momento da escrita, a versão mais recente é 2.2.3. A versão mais recente pode ser visualizada na página de lançamento do repositório do Compose no GitHub. Sinta-se à vontade para substituir a versão no comando acima conforme necessário.

Mude as permições do binário baixado para que ele se torne executavel.
chmod +x ~/.docker/cli-plugins/docker-compose
Pronto, nosso docker compose está instalado, para verificar o mesmo, basta executar o seguinte comando para visualizar a sua versão
docker compose version

Desinstalar o Docker-compose
Para remover o Docker Compose, utilize o seguinte comando (supondo que você instalou "curl" como fizemos acima).
sudo rm /usr/local/bin/docker-compose
Script de instalação automatizada para Ubuntu.
Chegou até aqui e achou tudo isso muito complicado ? então preparei esse script de instalação automatizada para simplificar todo o processo.

#!/bin/sh

echo "Iniciando a instalação do docker community edition..."
echo "Removendo todas as instâncias antigas do docker e instalando dependências"
apt remove -y docker docker-engine docker.io containerd runc
apt update
apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

echo "Baixando o docker mais recente e adicionando a chave GPG oficial"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

echo "Puxando o repositório mais recente"
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

apt update

echo "Instalando a edição da comunidade docker"
apt install -y docker-ce docker-ce-cli containerd.io

echo "Instalação do Docker concluída, instalando o docker-compose..."

echo "Baixando o docker-compose 2.2.3 - certifique-se de atualizar para o mais recente estável"
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

echo "Definindo permissões binárias"
chmod +x ~/.docker/cli-plugins/docker-compose

echo “Instalação do Docker e do docker-compose concluída”

# Execute o docker como usuário não root no Ubuntu
sudo usermod -aG docker $USER
