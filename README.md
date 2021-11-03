**Docker image**\
(https://hub.docker.com/r/ibmcom/ace-server/)

**Pull docker image**\
*docker pull ibmcom/ace-server*

Create the initial-config directory\
(in this example, mine is at **/Users/gavinlee@uk.ibm.com/AndreiDemo/initial-config**)

**Create the following folders in this directory:**\
keystore\
serverconf\
setdbparms\
truststore


**Add the files in the folders included into the matching folder:**\
keystore:\
myssl.crt = certificate\
myssl.key = private key

serverconf:\
overrides for keystore and truststore

setdbparms:\
mqsisetdbparms to create JVM alias for keystore and truststore passwords

truststore:\
ibmapiconnect.crt = signer certificate for the api

**Ensure that you have no other aceserver container images running**\
*docker ps*

**Run the image with the additional parameters required**\
*docker run --name aceserver -p 7600:7600 -p 7800:7800 -p 7843:7843 --env LICENSE=accept --env ACE_SERVER_NAME=ACESERVER --mount type=bind,src=**/Users/gavinlee@uk.ibm.com/AndreiDemo/initial-config**,dst=/home/aceuser/initial-config --env ACE_TRUSTSTORE_PASSWORD=password --env ACE_KEYSTORE_PASSWORD=password ibmcom/ace-server:latest*

Make sure you are pointing at your local directory for the mount for the initial-config

**Once the container is running, check that the initial config overrides are correct**\
*docker exec -it aceserver /bin/bash*\
Look for the following:\
server.conf.yaml in the overrides directory (this has the keystore and truststore locations)\
keystore.jks and truststore.jks in the ace-server directory (these are the newly created keystores and truststores from the initial-config\


Connect the toolkit to the server using 127.0.0.1:7600
Run the test for the flow - which should work :-)

Have fun!!!


