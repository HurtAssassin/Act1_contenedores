FROM php:8.2-apache

# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Copiar Composer desde la imagen oficial
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copiar proyecto
COPY . /var/www/html

# Ir al directorio de trabajo
WORKDIR /var/www/html

# Instalar dependencias PHP
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Cambiar DocumentRoot a /var/www/html/public
RUN sed -i 's#/var/www/html#/var/www/html/public#' /etc/apache2/sites-available/000-default.conf

# >>> Definición de Server Name, solicitada en arranques posteriores <<<
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Configurar acceso al directorio correcto
RUN printf "<Directory /var/www/html/public/>\n\
    AllowOverride All\n\
    Require all granted\n\
</Directory>\n" > /etc/apache2/conf-available/slim.conf \
    && a2enconf slim

# Permisos
RUN chown -R www-data:www-data /var/www/html && chmod -R 755 /var/www/html

EXPOSE 80
