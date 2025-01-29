# HotelBooking

This is a website for deploying a application by using Docker and Checks the code by using Sonarqube.


Step-1 :  Created an EC2-instance with allowing inbound ports 80,9000.

          Make your u created your instance in t2.medium or greater, Because sonarqube requires min 2GB RAM.

Step-2 : So Log in to that instance and installed GIT and Docker by running below commands
 
         sudo yum install git -y        #Installed Git in my instance.
         sudo yum install docker -y     #Installed Docker in my Instance
         sudo service docker start      #To start Docker Services.
         docker --version               #Checks the Docker Version.

Step-3 : Now Clone the Git Repo which u repo u want to deploy by using git clone command.

Step-4 : Once repo cloned we can create a Dockerfile to run this website on Nginx Server, You can find Dockerfile in this repo, you can check that.

Step-5 : Once image created create a container by running 
        
         docker run -d -p 80:80 <imagename>

Step-6 : once done verify the output by going to http://<publicip>:80

Step-7 : Now we can install SonarQube, We can do this by running docker command.

         docker run -d --name sonar -p 9000:9000 sonarqube:lts-community   

Step-8 : Once sonarqube installed, we have to install SonarScanner, This will analyse your code.

         wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.1.4610-linux-x64.zip  #A ZIP FILE WAS CREATED.
         unzip sonar-scanner-cli-6.2.1.4610-linux-x64.zip #UNZIPPING THE FOLDER.
         rm -rf sonar-scanner-cli-6.2.1.4610-linux-x64.zip #REMOVES THE ZIP FOLDER.
         sudo mv sonar-scanner-6.2.1.4610-linux-x64 /opt/sonar-scanner  #MOVING THE ZIP FILE TO /opt/sonar-scanner folder.
         
         echo 'export PATH=$PATH:/opt/sonar-scanner/bin' >> ~/.bashrc
         source ~/.bashrc

         sonar-scanner --version   ##To Check Version.


Step-9 : Now to go to your SonarQube UI and create a project and generate a token.
         Default Credentails are admin,admin.
         http://<publicip>:9000         
         You can change the credenattils and Create a Project and generate a token.
         Administartion > Security > Tokens (click this word).

Step-10 : Comeback to the server and create a file with name sonar-project.properties
            
          vim sonar-project.properties
          
            ######Change the sonarqube URL and give the token###
            sonar.projectKey=hotelbooking
            sonar.host.url=http://13.235.27.133:9000/
            sonar.login=squ_1bd0c6b7dc8778bd190f5b59ac73ab76bb696f27
            sonar.sources=.
            sonar.exclusions=**/node_modules/**, **/*.test.js
            sonar.language=js
            sonar.sourceEncoding=UTF-8
            sonar.javascript.lcov.reportPaths=coverage/lcov.info

          esc:wq #SAVE IT

Step-11: Now Verify the output in SonarQube Portal

         http://<publicip>:80












