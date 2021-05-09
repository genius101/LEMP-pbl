In this project we will implement a similar stack, but with an alternative Web Server - NGINX, which is also very popular and widely used by many websites in the Internet.

In LAMP-pbl we implemented a similar stack but with Apache Server instead of NGINX Web Server. In the following steps I would show you steps alongside screenshots of implementation with NGINX Web Server


Step 1 – Installing the Nginx Web Server


a) Install Nginx Web Server and check for status

- [ ]	sudo apt update
- [ ]	sudo apt install nginx
- [ ]	sudo systemctl status nginx

![1 a](https://user-images.githubusercontent.com/10243139/117574572-a48a9400-b0d5-11eb-8c23-a50fee24bef3.jpg)


b) To verify that Nginx Web Server is running, use following command:

- [x]	http://<Public-IP-Address>:80

![1 b](https://user-images.githubusercontent.com/10243139/117574662-1ebb1880-b0d6-11eb-81df-af9185c7e28e.jpg)


Step 2 — Installing MySQL

a) Use ‘apt’ to acquire and install mysql-server
	
- [ ]	sudo apt install mysql-server

- [x]	Run security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system:

- [ ]	sudo mysql_secure_installation

- [x]	VALIDATE PASSWORD COMPONENT:  Y

- [x] 	Level of Password: (I choose 0 for LOW)
	
- [x] 	Enter a secure password and confirm password

- [x]	Every other step: Y or Yes
	
- [x] 	Reload priviledge tables: Y
	
	Success. All done!
    
b) Log in to the MySQL console and Exit back to Ubuntu Console

- [ ]	sudo mysql	

- [ ] 	exit

![2](https://user-images.githubusercontent.com/10243139/117574774-9be68d80-b0d6-11eb-9c8c-967bc20427a3.jpg)


Step 3 — Installing PHP

a) Install php-mysql together with php-fpm, which stands for “PHP fastCGI process manager”:

- [ ]	sudo apt install php-fpm php-mysql 

![3](https://user-images.githubusercontent.com/10243139/117574905-5bd3da80-b0d7-11eb-89ba-ebb5e7c8c67c.jpg)


Step 4 — Configuring Nginx to Use PHP Processor

a) Create the root web directory for your_domain as follows:

- [ ]	sudo mkdir /var/www/projectLEMP

- [ ]	sudo chown -R $USER:$USER /var/www/projectLEMP 

![4 a](https://user-images.githubusercontent.com/10243139/117575113-57f48800-b0d8-11eb-8bc5-94689640f9a4.jpg)

b) Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
- [ ]	sudo nano /etc/apache2/sites-available/projectesla.conf

- [x]	Insert the following configuration:
		
		#/etc/nginx/sites-available/projectLEMP

		server {
    			listen 80;
    			server_name projectLEMP www.projectLEMP;
    			root /var/www/projectLEMP;

    			index index.html index.htm index.php;

    			location / {
        		try_files $uri $uri/ =404;
    			}

    			location ~ \.php$ {
        		include snippets/fastcgi-php.conf;
        		fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     			}

   			location ~ /\.ht {
        		deny all;
    			}

		}	

- [x]	Hit ctrl x to exit
- [x]	Type y to save
- [x]	Hit enter to save and exit back to the Ubuntu console

![4 b ii](https://user-images.githubusercontent.com/10243139/117575413-8c1c7880-b0d9-11eb-97a9-5647bb7bbe66.jpg)

- [x]	Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

- [ ]	sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
