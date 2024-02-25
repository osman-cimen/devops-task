FROM php:8.3-apache

# Set environment variables.
ENV ACCEPT_EULA=Y

# Install system dependencies.
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

 # Enable Apache modules required for system.
RUN a2enmod rewrite   

# Set the Apache document root
ENV APACHE_DOCUMENT_ROOT /var/www/html/

# Update the default Apache site configuration
COPY apache-config.conf /etc/apache2/sites-available/000-default.conf

# Install PHP extensions.
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo

# Microsoft ODBC18
RUN curl https://packages.microsoft.com/keys/microsoft.asc | tee /etc/apt/trusted.gpg.d/microsoft.asc
RUN curl https://packages.microsoft.com/config/debian/11/prod.list | tee /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y msodbcsql18
# optional: for bcp and sqlcmd
RUN ACCEPT_EULA=Y apt-get install -y mssql-tools18
RUN echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc
RUN . ~/.bashrc
# optional: for unixODBC development headers
RUN apt-get install -y unixodbc-dev
# optional: kerberos library for debian-slim distributions
RUN apt-get install -y libgssapi-krb5-2

RUN pecl install pdo_sqlsrv sqlsrv
RUN printf "; priority=20\nextension=sqlsrv.so\n" > /usr/local/etc/php/conf.d/sqlsrv.ini
RUN printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /usr/local/etc/php/conf.d/pdo_sqlsrv.ini
RUN docker-php-ext-enable sqlsrv pdo_sqlsrv

# Create a directory for your app application.
WORKDIR /var/www/html

# Copy the app application files into the container.
COPY QuickDbTest.php /var/www/html/index.php

# Install app dependencies using Composer.
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y bash

# Set permissions for app.
RUN chown -R www-data:www-data /var/www/html
# Expose port 80 for Apache.
EXPOSE 80

CMD ["sh", "-c", "apache2-foreground && php index.php"]
