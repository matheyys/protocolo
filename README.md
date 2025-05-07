#protocolo
#!/bin/bash

# Script para instalar R e RStudio Server no Ubuntu
# Funciona no Ubuntu 20.04, 22.04 e 24.04

echo "Atualizando o sistema..."
sudo apt update && sudo apt upgrade -y

echo "Instalando dependências..."
sudo apt install -y gdebi-core dirmngr gnupg apt-transport-https ca-certificates software-properties-common \
    libcurl4-openssl-dev libssl-dev libxml2-dev build-essential wget

echo "Adicionando chave e repositório do R (CRAN)..."
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/cran.gpg > /dev/null
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

echo "Instalando R..."
sudo apt update
sudo apt install -y r-base

echo "Verificando instalação do R..."
R --version

echo "Baixando o RStudio Server (última versão estável)...",
RSTUDIO_URL=$(wget -qO- https://posit.co/download/rstudio-server/ | grep -Eo 'https://download\.posit\.co/.*rstudio-server.*amd64\.deb' | head -1)
wget "$RSTUDIO_URL" -O rstudio-server.deb

echo "Instalando o RStudio Server..."
sudo gdebi -n rstudio-server.deb

echo "Habilitando e iniciando o RStudio Server..."
sudo systemctl enable rstudio-server
sudo systemctl start rstudio-server

echo "Status do RStudio Server:"
sudo systemctl status rstudio-server

echo "Instalação finalizada! Acesse o RStudio Server via navegador: http://<SEU_IP>:8787"
