## INSTALLING THE NGINX WEB SERVER

## INSTALLING APACHE AND UPDATING THE FIREWALL.

I updated my server by using the command "sudo apt update," which produced the output shown in the image below.

![sudo apt update](./images2/apt%20update.png)

installing and running apache.

After updating the package, I run command "sudo apt install apache2" and the output in the image below was obtained

![sudo apt nginx](./images2/installing%20nginx.png)

I run "sudo systemctl status nginx" to ensure that nginx was successfully installed.

![sudo status nginx](./images2/nginx%20status.png)

## establishing TCP port 80

I opened TCP port 80 on my Ubuntu server so that the web server could receive traffic by running the command "curl http://localhost:80," which produced the output in the screenshot below:

![curl localhost](./images2/curl%20localhost.png)

By navigating to http://(my ip address):80 in my browser to see if my installation was functioning properly, I get the image below.

![ nginx welcome page](./images2/welcome%20nginx.png)

Mysql was installed using the command "sudo apt install mysql-server," and the result is shown in the image below.

![mysql installation](./images2/sudo%20mysql%20install.png)

After entering mysql native password as the root password, I defined the user password and used the command "exit" to shut down mysql.

![passsword mysql](./images2/mysql%20pswd.png)

