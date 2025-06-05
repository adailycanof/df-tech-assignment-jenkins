## Building Jenkins on Docker
Building jenkins on docker to test the jenkins pipeline - out of scope but with no access to any jenkins this was needed. 

## Build the image (make sure you're in the directory with the Dockerfile)

1. Build the docker image with the below command:

```
docker build -t my-jenkins .
```

2. Run Jenkins in deatched mode, exporting port 8080 (and agent connections omn 50000) and creates a named volume to persist Jenkins data (jobs, configurations, plugins, etc.) 
```
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock my-jenkins
```

Open your browser and go to
```
http://localhost:8080
```

3. Follow the setup to create a pipeline to hook into scm.