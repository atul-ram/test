curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"



sudo apt-get update

apt-cache policy docker-ce
sudo apt-get install -y docker-ce

sudo docker --version


sudo curl -sSL https://get.docker.io/ | sh

sudo apt-get purge docker-ce



FROM tomcat:jre8-alpine

# For wget to work
RUN   apk update \                                                                                                                                                                                                                        
&&   apk add ca-certificates wget \                                                                                                                                                                                                      
&&   update-ca-certificates 

# Copy tomcat server.xml
WORKDIR /usr/local/tomcat

# Start tomcat
CMD ["catalina.sh", "run"]



docker build -t tomcatimage .

docker inspect tomcatimage

Login to docker hub using the below command

docker login --username <username>

key in the password once prompted.

Command to tag the image with the repository image name

docker tag tomcat01 <username>/tomcatimage

Now let us push the image to the hub

docker push <username>/tomcatimage


docker run --name tomcatRunner -p 8080:80 -d tomcatimage

