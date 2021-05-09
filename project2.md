In this project we will implement a similar stack, but with an alternative Web Server - NGINX, which is also very popular and widely used by many websites in the Internet.

In LAMP-pbl we implemented a similar stack but with Apache Server instead of NGINX Web Server. In the following steps I would show you steps alongside screenshots of implementation with NGINX Web Server


Step 1 – Installing the Nginx Web Server


a) Install Nginx Web Server and check for status

- [ ]	sudo apt update
- [ ]	sudo apt install nginx
- [ ]	sudo systemctl status nginx

![1 a](https://user-images.githubusercontent.com/10243139/117574572-a48a9400-b0d5-11eb-8c23-a50fee24bef3.jpg)


b) To verify that Nginx Web Server is running, use following command:

- [ ] http://<Public-IP-Address>:80

![1 b](https://user-images.githubusercontent.com/10243139/117574662-1ebb1880-b0d6-11eb-81df-af9185c7e28e.jpg)


Step 2 — Installing MySQL


