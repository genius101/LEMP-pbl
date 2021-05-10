In this project we will implement a similar stack, but with an alternative Web Server - NGINX, which is also very popular and widely used by many websites in the Internet.

In LAMP-pbl we implemented a similar stack but with Apache Server instead of NGINX Web Server. In the following steps I would show you steps alongside screenshots of implementation with NGINX Web Server


Step 1 – Installing the Nginx Web Server


a) Install Nginx Web Server and check for status

- [ ]	sudo apt update
- [ ]	sudo apt install nginx
- [ ]	sudo systemctl status nginx

![1 a](https://user-images.githubusercontent.com/10243139/117574572-a48a9400-b0d5-11eb-8c23-a50fee24bef3.jpg)


b) To verify that Nginx Web Server is running, use following command:

- [x]	From the Browser http://<Public-IP-Address>:80

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
- [ ]	sudo nano /etc/nginx/sites-available/projectLEMP

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

- [x]	You can test your configuration for syntax errors by typing:

- [ ]	sudo nginx -t

![4 b iv](https://user-images.githubusercontent.com/10243139/117576081-5a58e100-b0dc-11eb-93c4-4df9e84a1d9b.jpg)

- [x]	We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
		
- [ ]	sudo unlink /etc/nginx/sites-enabled/default

- [x]	When you are ready, reload Nginx to apply the changes:

- [ ]	sudo systemctl reload nginx

- [x]	Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:

- [ ]	sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

- [x]	Now go to your browser and try to open your website URL using IP address:
		
- [ ]	http://<Public-IP-Address>:80

![4 b viii](https://user-images.githubusercontent.com/10243139/117576255-2336ff80-b0dd-11eb-8967-e844a08114dc.jpg)



Step 5 – Testing PHP with Nginx


a) We would test to Validate that Nginx can correctly hand .php files off to our PHP processor.

- [x]	You can do this by creating a test PHP file in your document root:

- [ ]	nano /var/www/projectLEMP/info.php

- [x]	Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

		<?php
		phpinfo();
		
![5 a ii](https://user-images.githubusercontent.com/10243139/117576355-9cceed80-b0dd-11eb-9dc0-9368ae198dd8.jpg)

- [x]	You can now access this page in your web browser by visiting the public IP address you’ve set up in your Nginx configuration file, followed by /info.php

![5 a iii](https://user-images.githubusercontent.com/10243139/117576420-d9024e00-b0dd-11eb-86c2-b65421582bdf.jpg)

- [x]	it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:
		
- [ ]	sudo rm /var/www/projectLEMP/info.php


Step 6 — Retrieving data from MySQL database with PHP

a) In this step we will create a test database (DB) with simple “To do list” and configure access to it, so the Nginx website would be able to query data from the DB and display it. We will create a database named example_database and a user named example_user:

- [x]	Connect to the MySQL console using the root account:	
		
- [ ]	sudo mysql

- [x]	To create a new database, run the following command from your MySQL console:

- [ ]	CREATE DATABASE `example_database`;

	(Dont forget the Paréntesis as in the image below)

- [x]	Now we can create a new user and grant him full privileges on the database you have just created. Username as example_user and password as password:

- [ ]	CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

- [x]	Now we need to give this user permission over the example_database database:

- [ ]	GRANT ALL ON example_database.* TO 'example_user'@'%';
	(This is not in the image below, please dont forget to run this command before you exit)

- [x]	Now exit using the command exit

![6 a](https://user-images.githubusercontent.com/10243139/117576924-875ac300-b0df-11eb-9bf2-5f0cbee56b4f.jpg)


b) Test the new credential created

- [x]	Log on with the new user created, and input the password when prompted:
	
- [ ]	mysql -u example_user -p

- [x]	Confirm that you have access to the example_database database:
		
- [ ]	show DATABASES;

![6 b](https://user-images.githubusercontent.com/10243139/117577243-ecfb7f00-b0e0-11eb-8669-443068c45da7.jpg)

c) Next, we’ll create a test table named todo_list

- [x]	 From the MySQL console, run the following statement:

		CREATE TABLE example_database.todo_list (
		mysql>     item_id INT AUTO_INCREMENT,
		mysql>     content VARCHAR(255),
		mysql>     PRIMARY KEY(item_id)
		mysql> );
		
- [x]	Insert a few rows:

		INSERT INTO example_database.todo_list (content) VALUES ("My First Important Item")
		INSERT INTO example_database.todo_list (content) VALUES ("My Second Important Item")
		INSERT INTO example_database.todo_list (content) VALUES ("My Third Important Item")
		
![6 c ii](https://user-images.githubusercontent.com/10243139/117577365-61ceb900-b0e1-11eb-9485-b407a0dfada9.jpg)

- [x]	To confirm that the data was successfully saved to your table, run:

- [ ]	SELECT * FROM example_database.todo_list;

- [x]	You can exit the MySQL by typing exit

![6 c iv](https://user-images.githubusercontent.com/10243139/117577421-9e9ab000-b0e1-11eb-9954-4b9e0a2ace23.jpg)

d) Now you can create a PHP script that will connect to MySQL and query for your content.

- [x]	 Create a new PHP file in your custom web root directory using your preferred editor: 
	
- [ ]	nano /var/www/projectLEMP/todo_list.php

- [x]	Copy this content into your todo_list.php script:

		<?php
		$user = "example_user";
		$password = "password";
		$database = "example_database";
		$table = "todo_list";

		try {
  		   $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  		   echo "<h2>TODO</h2><ol>";
  		   foreach($db->query("SELECT content FROM $table") as $row) {
    		      echo "<li>" . $row['content'] . "</li>";
  		   }
  		   echo "</ol>";
		} catch (PDOException $e) {
    		      print "Error!: " . $e->getMessage() . "<br/>";
    		      die();
		}

![6 d ii](https://user-images.githubusercontent.com/10243139/117577576-3dbfa780-b0e2-11eb-9c6d-9122037b6374.jpg)

- [x]	Save and close the file when you are done editing.
	
- [x]	You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
		
- [ ]	http://<Public_domain_or_IP>/todo_list.php

![6 d iv](https://user-images.githubusercontent.com/10243139/117577616-6cd61900-b0e2-11eb-81de-f98eec62552f.jpg)

In this project, we have built a flexible foundation for serving PHP websites and applications to visitors, using Nginx as web server and MySQL as database management system.
