FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV COMPOSER_ALLOW_SUPERUSER=1

# Instala herramientas base y dependencias
RUN apt-get update && apt-get install -y \
    curl \
    git \
    sudo \
    vim \
    wget \
    unzip \
    ca-certificates \
    build-essential \
    libnss3-tools \
    jq \
    xsel \
    openssl \
    bash \
    software-properties-common \
    gnupg2 \
    lsb-release \
    nginx \
    dnsmasq \
    network-manager \
 && add-apt-repository ppa:ondrej/php -y \
 && apt-get update

# Instalar PHP 8.3 y extensiones
RUN apt-get install -y \
    php8.3 \
    php8.3-cli \
    php8.3-fpm \
    php8.3-curl \
    php8.3-mbstring \
    php8.3-xml \
    php8.3-zip \
    php8.3-mysql \
    php8.3-readline \
    php8.3-sqlite3 \
    php8.3-gd \
    php8.3-bcmath \
    php8.3-intl \
    php8.3-soap \
    php8.3-pgsql \
    php8.3-xdebug \
    php8.3-opcache

# Instala Composer global
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Instala Bun
RUN curl -fsSL https://bun.sh/install | bash && \
    cp /root/.bun/bin/bun /usr/local/bin/

ENV NVM_DIR=/usr/local/nvm

# 3. Instalar NVM y Node
RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

# 4. Instalar Node.js 20 usando NVM sin necesidad de `source`
RUN bash -i -c "export NVM_DIR=$NVM_DIR && . $NVM_DIR/nvm.sh && nvm install 20 && nvm alias default 20"

# 5. Agregar Node al PATH (versión específica que acabamos de instalar)
ENV PATH=$NVM_DIR/versions/node/v20.0.0/bin:$PATH

RUN curl -o- https://raw.githubusercontent.com/ArdaGnsrn/pvm-ubuntu/master/install.sh | sudo bash

# Crea usuario "coder" con sudo sin contraseña
ARG USER=coder
RUN useradd --create-home --shell /bin/bash --groups sudo $USER && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/$USER && \
    chmod 0440 /etc/sudoers.d/$USER && \
    mkdir -p /home/$USER/.config/composer && \
    chown -R $USER:$USER /home/$USER

USER $USER
WORKDIR /home/$USER
