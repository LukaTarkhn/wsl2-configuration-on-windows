# WSL2configuring on windows 11
1. Install **Ubuntu** from **Microsoft store** and configure

    after installation we should search windows features **Turn Windows features on or off** and enable **Virtual Machine Platform** and **Windows Subsystem for Linux**. then we can run Ubuntu in terminal.
    
2. Install **Node.js** and **npm**

    Install NodeJS using [Node Version Manager](https://github.com/nvm-sh/nvm) 

        sudo apt-get install curl
        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

    To download, compile, and install the latest release of node, do this:

        nvm install node # "node" is an alias for the latest version
        
3. Install **PHP** and **add-ons**

   It's important to install **php8.1-dev** if you want to compile any add-ons later.

        sudo add-apt-repository -y ppa:ondrej/php
        sudo apt update && sudo apt install -y php8.1-bz2 php8.1-cgi php8.1-cli php8.1-common php8.1-curl php8.1-dev php8.1-enchant php8.1-fpm php8.1-gd php8.1-gmp php8.1-imap php8.1-intl php8.1-ldap php8.1-mysql php8.1-odbc php8.1-opcache php8.1-pgsql php8.1-phpdbg php8.1-pspell php8.1-readline php8.1-sybase php8.1-tidy php8.1-xmlrpc php8.1-xsl php8.1-sqlite3 php8.1-mbstring php8.1-bcmath php8.1-soap php8.1-zip php8.1-xdebug php8.1-redis php8.1-igbinary php8.1-imagick

   If you are looking for more PHP modules try:

        sudo apt-cache search php8.1-
        
4. Install **Composer**

        curl -sS https://getcomposer.org/installer | php && sudo mv composer.phar /usr/local/bin/composer

5. Install **Git**

        sudo apt update
        sudo apt install git
        git --version
        
6. Install **mysql**

    - Open your WSL terminal (ie. Ubuntu).
    - Update your Ubuntu packages: 
    
          sudo apt update 
          
    - Once the packages have updated, install MySQL with: 
    
          sudo apt install mysql-server
    
    - Confirm installation and get the version number: 
    
          mysql --version
          
    - Start a MySQL server: 
    
          sudo /etc/init.d/mysql start
          
    - Start the security script prompts: 
    
          sudo mysql_secure_installation
    
      - To open the MySQL prompt, enter: **sudo mysql**
      
      - To see what databases you have available, in the MySQL prompt, enter: **SHOW DATABASES;**
      
      - To create a new database, enter: **CREATE DATABASE database_name;**
      
      - To delete a database, enter: **DROP DATABASE database_name;**
    
7. [install **Apache2** and **Setting Up Virtual Hosts**](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04)
    - Install Apache2:
    
          sudo apt update
          sudo apt install apache2
          sudo ufw app list
          sudo ufw allow 'Apache'
          sudo service apache2 status
          
    - Setting up Virtual Hosts:
    
          sudo mkdir /var/www/your_domain
          sudo chown -R $USER:$USER /var/www/your_domain
          sudo chmod -R 755 /var/www/your_domain
          sudo nano /var/www/your_domain/index.html
          
      Insert example HTML code:
      
          <html>
              <head>
                  <title>Welcome to Your_domain!</title>
              </head>
              <body>
                  <h1>Success!  The your_domain virtual host is working!</h1>
              </body>
          </html>
          
      Create config file:
          
          sudo nano /etc/apache2/sites-available/your_domain.conf
          
      Add in the following configuration block
      
          <VirtualHost *:80>
              ServerAdmin webmaster@localhost
              ServerName your_domain
              ServerAlias www.your_domain
              DocumentRoot /var/www/your_domain
              ErrorLog ${APACHE_LOG_DIR}/error.log
              CustomLog ${APACHE_LOG_DIR}/access.log combined
          </VirtualHost>
      
      Next steps:
      
          sudo a2ensite your_domain.conf
          sudo a2dissite 000-default.conf
          sudo apache2ctl configtest
          sudo service apache2 reload
          
       Now we should add our domain in WSL hosts file:
       
            sudo nano /etc/hosts
       
       and add this line:
       
          127.0.0.1       your_domain

       We can test if server can see our domain:
       
          curl http://your_domain
          
      Now we Should add our domain in Windows hosts file and configure **.wslconfig**
      
         - For fast changes in hosts file we can download [Hosts File Editor](https://hostsfileeditor.com/), and add 2 lines for our domain:
            
                127.0.0.1       your_domain
                ::1             your_domain
            
         - Now create **.wslconfig** file in **C:\Users\your_user_name**, and add in the following configuration block:

                [wsl2]

                memory = 4GB   # Limits VM memory in WSL 2 up to 4GB
                processors = 4 # Makes the WSL 2 VM use two virtual procesors
                #localhostForwarding=true


       
