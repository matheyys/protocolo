# protocolo
#!/bin/bash

# Atualiza pacotes
sudo apt update && sudo apt upgrade -y

# Instala dependências básicas
sudo apt install -y gdebi-core libcurl4-openssl-dev libssl-dev libxml2-dev build-essential

# Adiciona o repositório do R
sudo apt install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common
sudo wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | gpg --dearmor > r.gpg
sudo install -o root -g root -m 644 r.gpg /etc/apt/trusted.gpg.d/
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

# Atualiza e instala o R
sudo apt update
sudo apt install -y r-base

# Verifica se o R foi instalado
R --version

# Baixa e instala o RStudio Server (última versão estável)
RSTUDIO_URL=$(wget -qO- https://posit.co/download/rstudio-server/ | grep -Eo 'https://.*rstudio-server.*amd64\.deb' | head -1)
wget "$RSTUDIO_URL" -O rstudio-server.deb
sudo gdebi -n rstudio-server.deb

# Inicia e habilita o serviço
sudo systemctl enable rstudio-server
sudo systemctl start rstudio-server

# Status final
sudo systemctl status rstudio-server
echo "R e RStudio Server instalados com sucesso!"
