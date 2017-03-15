# case_study


I have created a Docker Swarm setup using CloudFormation template with 1 Manger and 2 Worker nodes Architecture. The CLoudFormation template is uploaded in Git server named "swarm.tmpl"

I have created a jenkins Master/Slaves(2 Workers) as a Docker Container. Url for Jenkins is mentioned in the "Infrastructure_Details.doc"

Created ngnix server in a container and exposed the files pushed in Git Server through ngnix server. Url for ngnix server is mentioned in the "Infrastructure_Details.doc"
Note: Infrastructure for ngnix server is done in such a way that, if one of your Engineers push the code to Git server,you can view the changes reflected at the ngnix Url.


To login to any of the Swarm Cluster, use the "docker.pem" key(Uploded to case_study repository) and username as "docker".


Command to create ngnix server running on port 8000 in Swarm mode:

 docker service create --name my_web \
                        --replicas 3 \
                        --publish 8000:80 \
                        nginx
                        
Commands to create Jenkins Master/Slave in Swarm containers(Dockerized Jenkins Swarm-Slave):

First start a master!

$ docker run -d -p 8090:8080 --name jenkins blacklabelops/jenkins
This will pull the my jenkins container ready with swarm plugin and ready-to-use!

Now swarm the place!

$ docker run -d --link jenkins blacklabelops/jenkins-swarm
$ docker run -d --link jenkins blacklabelops/jenkins-swarm


Setting Jenkins Master URL :

 docker run -d \
  -e "SWARM_MASTER_URL=http://52.90.160.161:8090/" \
  blacklabelops/jenkins-swarm