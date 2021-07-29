ARG PHP_IMAGE_VERSION
FROM php:${PHP_IMAGE_VERSION}

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH ${PATH}:/composer/vendor/bin
ENV LANG C.UTF-8
ENV LANGUAGE en_US:

ARG XDEBUG_VERSION
ARG NODE_VERSION
ARG NPM_VERSION

# apt-getでインストールするのはOS用のパッケージ
# peclでインストールするのはPHP用のパッケージ

# インストール可能なパッケージの「一覧」を更新
RUN apt-get update \
  # node インストール
  && curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
  && apt-get install -y libzip-dev zlib1g-dev unzip vim zip libpq-dev libonig-dev \
  # Dockerコンテナ内でIlluminate\Http\UploadedFileクラスを利用するには PHP GD が必要
  libpng-dev libjpeg-dev libicu-dev \
  nodejs \
  && npm install -g npm@${NPM_VERSION} \
  && pecl install xdebug${XDEBUG_VERSION} apcu \
  && echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini \
  && docker-php-ext-enable xdebug \
  # Dockerコンテナ内でIlluminate\Http\UploadedFileクラスを利用するには PHP GD が必要
  && docker-php-ext-configure gd --with-jpeg \
  && docker-php-ext-install gd zip pdo_mysql bcmath intl mbstring opcache intl \
  # composer
  # https://getcomposer.org/download/
  # インストーラーを現在のディレクトリにダウンロードします
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  # インストーラーSHA-384を確認します
  && php -r "if (hash_file('sha384', 'composer-setup.php') === trim(file_get_contents('https://composer.github.io/installer.sig'))) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  # インストーラーを実行します
  && php composer-setup.php \
  # インストーラーを削除します
  && php -r "unlink('composer-setup.php');" \
  # グローバルに配置
  && mv composer.phar /usr/local/bin/composer \
  # Laravel のインストーラをダウンロード
  && composer global require laravel/installer \
  # Clear cache
  # apt-get clean では /var/cache/apt/archives にキャッシュされている全てのパッケージを削除
  # rm -rf /var/lib/apt/lists/* では /var/cache/apt/list にキャッシュされている全てのパッケージリストを削除
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# port
EXPOSE 22 80 443
