# docker-project
I have dockerize joomla application with mysql database using docker-compose.
Docker-Compose
Project on docker compose. Integrating MySql with Joomla while both running on separate docker O.S. My first project ever.

"Beginner to Expert Level" was the title of the video I started watching on Docker. I was a beginner , just started docker with that only but now I can say that now I not only know Docker but actually pretty confident in it as well. It was an instructor-led live session by Mr Vimal Daga who helped me reach this stage under his IIEC-1.0 campaign. Here I share you my work and project on Docker:

Description
This is a project which checks the practical knowledge of Docker and Docker-Compose. We started with mysql and joomla. Setting up both and then integrating with each other. This system of using multiple O.S. or independent systems tied together is termed as Multi-tier architecture. Hence here we achieve multi-tier architecture. Using the concepts of PAT , persistent storage as well.

MULTI-TIER ARCHITECTURE ON DOCKER
Using docker-compose to code the whole architecture. MySql and Joomla were linked together.

TOOLS / PRODUCTS USED:
VMware or VirtualBox. Linux O.S. (used REDHAT here). MySql and Joomla container image. Clear Concepts of Networking (iptables, port forwarding). Clear Concepts of Volumes and storage. Client Web-browser. Docker-Compose.

About MySQL
MySQL is the world's most popular open source database. With its proven performance, reliability and ease-of-use, MySQL has become the leading database choice for web-based applications, used by high profile web properties including Facebook, Twitter, YouTube, Yahoo! and many more.

About Joomla
Joomla is an open source platform on which Web sites and applications can be created. It is a content management system (CMS) which connects your site to a MySQLi, MySQL, or PostgreSQL database in order to make content management and delivery easier on both the site manager and visitor. Joomla's primary focus has been on usability and extensibility since its initial release in 2005. It is because of this that the project has been the recipient of numerous awards, including being a three-time recipient of the PACKT Open Source Content Management System Award. Joomla is a completely free open source solution available to anyone and everyone with a desire to build dynamic and robust sites for a variety of purposes. Joomla has been utilized by some of the Web's most recognizable brands including Harvard, iHop, and MTV. It is capable of carrying out tasks ranging from corporate websites and blogs to social networks and e-commerce.

Prior-Settings:
After setting up the Linux O.S. and an active internet connection is an absolute prerequisite. After we basics setup, now begin installing. Download docker-ce and install on the base O.S. Then download the container images of MySQL and Joomla.

Some tweaks:
Some changes are required in the system to let the infrastructure work smoothly. First we disable firewall and allow all packets in iptables. Now I know it is not safe to do both of these steps but as a beginner my focus was mainly on setting up the docker infrastructure and didn't give much to security. For now, we can go along with this and when we finish this up we can do some tweaks in this security domain as well.

For firewall:
systemctl stop firewalld
systemctl disable firewalld
First command to just disable firewall but through 2nd command we permanently turn it off so that after O.S. resarts, it doesn't get active again.

For iptables:
iptables -F` 

iptables -nvL

iptables -P FORWARD ACCEPT

iptables -nvL
First command to flush all rules which were already set-up. 2nd to check the results for the 1st command. 3rd command to make changes in iptable and allow all traffic. Again 4th command to check the results.

Begin Core:
Now that the setting up is done, lets begin with the core of the project.

To download the docker-ce:

yum install docker-ce
If some minor hiccups then:

yum install docker-ce --nobest
Then let's start these services:

systemctl start docker
systemctl enable docker
This concludes the installing and running the docker engine. If any problems persist in installation then some changes might be required in repo files, you can contact me if the problem is still there.

Now we pull container images of MYSQL and Joomla:
First make your account on https://hub.docker.com/ so that you can pull and push images with ease. Mainly an account is required if you want to push.

docker pull mysql:5.7
Form the website: https://hub.docker.com/_/mysql

Then...

docker pull joomla
From the website : https://hub.docker.com/_/joomla

Yes, I have downloaded version 5.7 of MySQL for just some compatibility reasons.

Setup MySQL at both ends
At both ends here means at both client and server side. MySQL working at the server side needs the client side so that we can connect to it as well.

At Server Side:
‘docker run -it -e MYSQL_ROOT_PASSWORD= your_password -e MYSQL_USER=your_user name -e MYSQL_PASSWORD=user_password -e MYSQL_DATABASE=database name --name joomladb mysql:5.7`

This code will use mysql image to launch mysql server which we will connect to, for that we need its IPAddress , for this we do:

docker inspect container_id | grep IP
Suppose IP is 172.17.0.2

This will bring the ip address , note it down.

At client side (Base O.S.)
yum install mysql
After installation we need to connect to it , for this:

mysql -h 172.17.0.2 -u username -ppassword
Remember : NOT TO GIVE SPACE BETWEEN -p AND password.

Through this we now know how to set up mysql at both server and client side.

P.S. - Working with joomla is easy so we will directly configure it in docker-compose file.
Result Result

DOCKER COMPOSE
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment. Run docker-compose up and Compose starts and runs your entire app.

Docker compose can be downloaded and installed here: https://docs.docker.com/compose/install/ Once you install docker compose , go inside the specified directory. This is a standard directory so that you don't have to specify file path while launching docker compose.

cd /mycompose
vim docker-compose.yml
And also it has to be in yml format as this is the format that docker compose take as a file for instructions. Result Result

EXPLANATION:
Starting with the top version is of the connecting image which is here Joomla. Since its a Service we club the whole block into services.

joomladbos is the name of the container os you want to give after it is launched and it is Also beneficial as we have a name to connect it with giving the name of the container instead of ever changing IPs.

In some next lines we specify the image we will use to launch it from and volume which will be provided to give this container persistent storage. Then it turns on for restart , it is instructed to keep it on at all times.

Then we set-up some environment variables like giving password for root account and also forming a new account without root privileges since we do not give root access to Joomla service. Then at last the name of the database the data related to the application will be stored here.

Then let's move to JOOMOS. Then is also the name of the container os which will be launched which is actual software through which clients connect and interact.

Again here we specify the image , volume , and restart option. But here we have something new and that is depends_on , well specify the link between front-end applications (JOOMLA here) with the backend sites (MySQL here). With this we achieve multi-tier architecture. Then we come at ports , this is we are doing port forwarding which specifies that whenever someone comes at this port (8080) to base os , send it to this device at the port 80 , through which we provide web-service. Then again we meet these environment variables to be specified. At last we give volumes. Well this doesn't come under services as we are telling docker-compose to make available these volumes which are to be used by the above provided container O.S.

Hope it cleared you about the code above.

FIRING IT UP!!
Now since all have been set it time to start our docker-compose and set up the desired infrastructure with this single piece of file of code. It is started with:

docker-compose up
Result Result Result

RESULTS:
To check its actual results we now test it working. For this, we go to client browser , type the IP of Joomla container which you can find by doing : docker inspect docker_id | grep IP . enter this in search bar of browser and if you see the home page of Joomla like the picture below: Result Result Result

In the last image you can see both of containers running.
DOWN!!
Now if we want to terminate the whole infrastructure we can use

docker-compose down
Turnoff all

This will bring down the whole setup and if you want it to rev it up again, you can use docker-compose up again and it’s running again.

Future Works
Well my future work on this topic will be to make it better and make it more accessible. That is my plan is to host it on a cloud platform preferably AWS because of familiarity of that platform. Assign it a static IP address and launch on AWS with some basic services at start will be my first checkpoint.

References:
This is playlist by Mr. Vimal Daga Sir is best among all!!!
https://www.youtube.com/playlist?list=PLAi9X1uG6jZ30QGz7FZ55A27jPeY8EwkE

https://www.mysql.com/about/

https://hub.docker.com/_/mysql

https://hub.docker.com/_/joomla

https://rockettheme.com/docs/joomla/platform
