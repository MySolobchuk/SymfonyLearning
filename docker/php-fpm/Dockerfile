FROM php:8.1-fpm

RUN apt-get update && apt-get install -y \
    libpq-dev \
    wget \
    zlib1g-dev \
    libmcrypt-dev \
    libzip-dev \
    libicu-dev \
    libxslt-dev \
    libpng-dev \
    libxml2 \
    build-essential \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libonig-dev \
    locales \
    zip \
    unzip \
    jpegoptim optipng pngquant gifsicle \
    git \
    vim \
    curl \
    wget \
    zsh \
    openssl\
    libsodium-dev

RUN apt-get install -y libpq-dev librabbitmq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql intl xsl sodium zip \
     && pecl install amqp redis \
    && docker-php-ext-enable amqp redis

RUN docker-php-ext-install gd


# Install oh-my-zsh
RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true

# Copy powerlevel10k zsh theme
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# Change the default theme, so to differentiate in case you use the default theme on your host
RUN sed -i 's:robbyrussell:powerlevel10k/powerlevel10k:g' ~/.zshrc

# Use minimalist-ish powerlevel10k configuration, run "p10k configure" to go through wizard
COPY ./.p10k.zsh /root/

# When running zsh, activate the p10k
RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> ~/.zshrc


WORKDIR /symfony