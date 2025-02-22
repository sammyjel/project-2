## INSTALLING THE NGINX WEB SERVER

## INSTALLING APACHE AND UPDATING THE FIREWALL.

I updated my server by using the command "sudo apt update," which produced the output shown in the image below..

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

once the installation was done. I entered the mysql console by entering the command "sudo mysql," and I produced the output displayed in the following image:

![sudo mysql](./images2/login%20mysql.png)

After entering mysql native password as the root password, I defined the user password and used the command "exit" to shut down mysql.

The command "sudo mysql secure installation" was used to launch an interactive script that created the result images below.

![passsword mysql vallidated](./images2/validation%20mysql.png)
![passwird mysql validation2](./images2/validation%20mysql2.png)

I run the command "sudo mysql -p" to see if MySQL security was functioning as intended, and the output seen in the image below was produced.

![sudo mysql -p](./images2/password%20confirmation.png)

 ## INSTALLING PHP

 creating bridge between php interpreter, web server and the database

The output of the command "sudo apt install php-fpm php-mysql" produced the image below, which serves as a bridge between the PHP interpreter and the web server.

![sudo apt install php](./images2/php%20-fpm%20php-mysql.png)

## CONFIGURING NGINX TO USE PHP PROCESSOR

The command "sudo chown -R $USER:$USER /var/www/lempstackproject" creates the owner for the directory.

Then I run the command "sudo nano /etc/nginx/sites-available/lempstackproject" after creating a configuration file in Nginx's sites-available directory using the command-line editor nano.

I then use the command listed below to create a brand-new, blank file:

#/etc/nginx/sites-available/samyLEMPproject

server {
    listen 80;
    server_name samyLEMPproject www.samyLEMPproject;
    root /var/www/samyLEMPproject;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

By running the command "sudo nginx -t," I then checked my settings for syntax errors.

The output I received after I tried my configuration is shown in the image below along with the commands I used.

![nginx sytax test output](./images2/php%20nginx%20test.png)

I used the commands "sudo unlink /etc/nginx/sites-available/default" to disable the default Nginx host and "sudo systemctl reload nginx" to reload Nginx.

Since the index.html page in the var/www/samyLEMPproject directory was initially empty, I built a new one to make sure my configuration was functioning as intended.

In order to do this, I added the command "sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) "with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/samyLEMPproject/index.html." abd the the image below was generated from the browser using the IP address.

![hello LEMP](./images2/hello%20LEMP.png)

## Testing PHP with Nginx

To validate if Nginx can correctly hand .php to the php processor that I configured. To do this, I opened a new file named info.php by running the command below:

sudo nano /var/www/lempstackproject/info.php and a blank space output was generated in which run command

< ?
phpinfo();

The output shown below, which was created, served as confirmation that my configuration was right.

![php with Nginx](./images2/info.php%20nginx.png)

## *Retrieving data from MySQL database with PHP*

To enable the Nginx website to query the database and show the data, a simple "To do list" was established in a test database and access to it was configured.

Using the root account, I run the command sudo mysql to log into the MySQL console.

I run the command CREATE DATABASE "sam_database" to create a new database, and the output shown below was produced.

![create dbase](./images2/create%20of%20sam%20dbase.png)

The default authentication method was mysql native password, and I established a new user named olaniyi user. I therefore defined ******* as the sam_user password.

After that, I execute the command GRANT ALL ON sam_database to grant the new user access to the database.

* TO 'sam_user'@'%'; the following output was produced as a result

![FRANT ALL ON](./images2/dbase%20pswd%20created.png)

I exit the MySQL console, log into the sam_user console, and run the command -u sam_user -p to see if the new user's access is functioning as intended. The output is shown in the image below.
![new user access](./images2/mysql%20-u%20test.png)

The command SHOW DATABASES was executed to verify that the new user has access to the database, and the results are displayed in the image below:

![show dbase command](./images2/show%20dbase.png)

I used the following command to build a test table called todo_list after evaluating sam_user's database access.

(item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item id)), CREATE TABLE sam_database.todo_list);

I then ran the command INSERT INTO sam_database.todo_list (content) VALUES ("My first important item") to add "My first important item" to the test table;

By executing the same command with different settings, I added other items.

I run the command SELECT * FROM sam_database.todo_list to see if the items I created had been inserted as planned, and the output shown below was produced.

![todo items ](./images2/dbase%20to%20list%20gen.png)

I close the MySQL console once this has been verified. As a result, I develop a PHP script that must connect to Mysql and perform a content query. To do this, I used the vi editor to run the command sudo nano /var/www/lempstackproject/todo list.php in my custom web root directory to create a new PHP file.

The script below was executed to link the query for the data in the todo_list table I generated to the MySQL database and display the results in a list.

< ?php
$user = "sam_user";
$password = "Samfem@1";
$database = "sam_database";
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

I then used my browser to access the page at http://my_IP/todo_list.php. and the browser produced the output below.

![todo web display](./images2/to%20do%20list%20web.png)

