# [Taipeitrip](https://taipeitrip.meetgather.site/)
A platform you’re able to search tourist spots in Taipei and book schedules to let tour guide guide the attractions you like.
## Motivation
I'd like to display my full-stack skills which I learned from a bootcamp of soft engineer. 
## Technologies 
### Front ends
* ###### HTML, CSS, JavaScript  
I developed the web without using any templates on the internet. I did it from the scratch on my own.
* ###### Ajax  
Use async、await to fetch api and get data from server
* ###### Jinja  
Simplify HTML templates with Jinja
* ###### RWD  
Adapt web for any size of screen
### Back ends
* ###### Python Flask
Establish server with Flask
* ###### RESTful API
Make use of RESTful API to let front-ends read, create, patch or delete data. All API URLs are displayed on second.py. 
* ###### MVC
Separate different functions in each part. Models are wraped in models.db.py, views are in static and templates folders, controllers are in app.py
* ###### JWT
Produce a limited token when an user login, which is a credential for server to identify the user 
* ###### Nginx
Utilize a reverse proxy to conceal IP address of the web with a domain name
* ###### Domain Name
Create a domain name, meetgather.site, on GoDaddy and connecting it with IP address of Meetgather
* ###### SSL certificate
Secure URL with https by Certbot
* ###### Third-party payment
Make use of testing version of TapPay, a third-party payment processor in Taiwan, to complete booking order.
* ###### GitHub
Pull the files of project from GitHub to run the application.
### AWS 
* ###### EC2
Develop with a computer on EC2, using the Ubuntu 22.04.4 LTS system.
* ###### RDS
Deploy MySQL to store and preserve data.
## How to run the project?
### EC2
* Create AWS account and launch EC2 instance of Ubuntu 22.04.4
* Allocate and associate elastic IP address
* Go to Security Groups in EC2 and set up inbound rules, which is opening the port 3000, 80, 443, 3306
* Login EC2 and operate with Linux command line.
* The project needs to be run with .env file, so operator should ask authour for .env and put .env in a folder
### GitHub
* Install git on EC2
```bash
sudo apt-get install git
```
* See the version of git
```bash
git --version
```
* Initialize empty Git repository in the folder containing .env
```bash
git init
```
* Pull the files from the remote branch 
```bash
git pull https://github.com/alemapnil/taipei-day-trip-website.git
```
* Run the application in the background with nohup
```bash
nohup python3 app.py &
```
### Domain name registrar
* Set up domain name and subdomain with elastic IP in A record
### Nginx
* Install nginx on EC2 and run
```bash
sudo apt update 
```
```bash
sudo apt install nginx
```
```bash
sudo systemctl start nginx.service 
```
* Check nginx status of operation
```bash
sudo systemctl status nginx
```
* Go to _nginx.conf_
```bash
sudo vim /etc/nginx/nginx.conf 
```
* Amend _nginx.conf_ with commenting out two lines in _http{}_ and increasing _server{}_ in it.
```bash
http{
# include /etc/nginx/conf.d/*.conf;
# include /etc/nginx/sites-enabled/*; 
server{}
}
```
* Set up elastic IP and port in _server{}_ well so that users could visit the web with domain name
```bash
server {
        listen 80;
        server_name taipeitrip.meetgather.site;
        location / {
        proxy_pass      http://elastic IP:3000/;
        proxy_redirect  off;

        proxy_set_header   Host                 $host;
        proxy_set_header   X-Real-IP            $remote_addr;
        proxy_set_header   X-Forwarded-For      $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto    $scheme;
        add_header Content-Security-Policy upgrade-insecure-requests;
    }
}
```
* Confirm whether configuration of nginx is correct
```bash
sudo nginx -t
```
* Start nginx again and you can visit web through domain name
```bash
sudo systemctl restart nginx
```
### SSL
* Add SSL on domain name through Certbot
```bash
sudo snap install --classic certbot
```
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
```bash
sudo apt-get install python3-certbot-nginx
```
```bash
sudo certbot --nginx
```
## How to Use the Web?
