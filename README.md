# Explaining_php_services_for_docker-compose.yml    

![Untitled-3](https://user-images.githubusercontent.com/47202519/57298449-dedc8780-70ef-11e9-8d8b-2b59647baec7.gif)

### Dockerfile
<ol>
Dockerfile uses to set the environment inside the individual container this file helps you to create an image and also install the software required by your application.</br>
Dockerfile will set the base image and specify the necessary commands and instructions to build the Laravel application image.</br></br>

<li>first we create a base image of php7.2-fpm. This file also install prerequisite packages for Laravel: mcrypt, pdo_mysql, mbstring, and imagick with composer.</li>

<strong>FROM php:7.2-fpm</strong></br></br>
<li>The WORKDIR instruction specifies the /var/www directory as the working directory for the application.</li></br>

<li>RUN cummand specify to update, install and configure settings inside the container</li> 
<ol>
<li>Use it to find and install new packages, upgrade packages</li>
<strong>RUN apt-get update && apt-get install -y \</strong></br>

<li>The build-essentials package is a reference for all the packages needed to compile a Debian package.</li>
    <strong>build-essential \</strong></br>

<li>The mysql client to send commands to any mysql server   </li>
    <strong>mysql-client \</strong></br>

<li>It is a platform-independent library that contains C functions for handling PNG images. It supports almost all of PNG's features.</li>
    <strong>libpng-dev \</strong></br>

<li>The libjpeg-turbo JPEG library is a library for handling JPEG files.  </li>  
    <strong>libjpeg62-turbo-dev \</strong></br>

<li>FreeType 2 font engine, development files. The FreeType project is a team of volunteers who develop free, portable and high-quality software solutions for digital typography. They specifically target embedded systems and focus on providing small, efficient and ubiquitous products.    <li>
    <strong>libfreetype6-dev \</strong></br>

<li>Locales affect user interface language, case mapping, collation (sorting), date and time formats, and number and currency formats.   </li> 
    <strong>locales \</strong></br>

<li>Zip files make it easy to keep related files together and make transporting, e-mailing, downloading and storing data and software faster and more efficient.   </li> 
    <strong>zip \</strong></br>

<li>Image optimization / compression library. This library is able to optimize png, jpg and gif files in very easy and handy way. It uses optipng, pngquant, pngcrush, pngout, gifsicle, jpegoptim and jpegtran tools. </li>   
    <strong>jpegoptim optipng pngquant gifsicle \</strong></br>

<li>Vim is a highly configurable text editor built to enable efficient text editing. It is an improved version of the vi editor distributed with most UNIX systems.  </li>  
    <strong>vim \</strong></br>

<li>Unzipping is the act of extracting the files from a zipped single file or similar file archive. If the files in the package were also compressed (as they usually are), unzipping also uncompresses them. </li>      
    <strong>unzip \</strong></br>

<li> Git (/ɡɪt/) is a distributed version-control system for tracking changes in source code during software development. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files.  </li>      
    <strong>git \</strong></br>

<li>curl is a tool to transfer data from or to a server, using one of the supported protocols (HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, DICT, TELNET, LDAP or FILE). The command is designed to work without user interaction.  </li>      
    <strong>curl</strong></br></br>
</ol>

<li>apt-get clean` and remove /var/cache/apt/lists </li>
<strong>RUN apt-get clean && rm -rf /var/lib/apt/lists/*</strong></br></br>
</strong>
<li>Install extensions</li>
<ol>
<li>Docker-php-ext-install used to build extensions from code (and enable after), mostly used to install core extensions.</li>
<strong>RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl</strong></br>
<li>Install GD into PHP </li>
<strong>RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/</strong></br>
<li>Install PHP extensions with docker-php-ext-install</li>
<strong>RUN docker-php-ext-install gd</strong>
</ol>
</br>
<li>Creating a dedicated user and group with restricted permissions mitigates the inherent vulnerability when running Docker containers, which run by default as root. Instead of running this container as root we created the www user, who has read/write access to the /var/www folder.</li>

<strong>RUN groupadd -g 1000 www</br>
RUN useradd -u 1000 -ms /bin/bash -g www www</strong></br>


<li>COPY instruction that we are using with the --chown flag to copy the application folder's permissions.</li>

<strong>COPY --chown=www:www . /var/www</strong></br></br>

<li>EXPOSE command exposes a port in the container, 9000, for the php-fpm server.</li>

<strong>EXPOSE 9000</strong></br></br>

<li>CMD specifies the command that should run once the container is created. </br>
Here, CMD specifies "php-fpm", which will start the server.</li>

<strong>CMD ["php-fpm"]</strong>
</ol>

###  PHP(volume)
<ol>
<li>Create local.ini file inside the php folder. this is the file that you bind-mounted to /usr/local/etc/php/conf.d/local.ini inside the container.</li></br>

<strong>
upload_max_filesize=40M</br>
post_max_size=40M</strong></br></br>


<li>Upload_max_filesize and post_max_size directives set the maximum allowed size for uploaded files, and demonstrate(show round) how you can set php.ini configurations from your local.ini file. You can put any PHP-specific configuration that you want to override in the local.ini file.</li>
</ol>
