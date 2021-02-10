# A Sample Tomcat Application

## About
Application deployment can vary on case to case basis. Outlined here are few of the ways to achieve this. User can deploy the application to local tomcat, a local docker container or minikube/EKS cluster. 

Note: This repository does not explain steps to configure setup required for deployment of the application.

## Running the application
Following steps provide an outline about how to deploy and run the application.

1. <b>Local Tomcat Server: </b>
    The app can be packaged as a war file and can also be downloaded from [here](https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war)  (Note: make  sure your browser doesn't change file extension or append a new one).

    The easiest way to run this application is simply to move the war file to your CATALINA_HOME/webapps directory. Tomcat will automatically expand and deploy the application for you. You can view it with the following URL (assuming that you're running tomcat on port 8080 as is the default):
    
    ``
    http://localhost:8080/sample
    ``

2. <b>Using Docker </b> - A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. 
Clone the repository and navigate to the path.

    ```sh
    $ cd SampleTomcatApp
    ```

    Run the following commands to build, tag and run the application.
    ```sh
    $ docker login #Skip this step if you do not wish to tag and push image to your repository
 
    $ docker build -t prg/app .

    $ docker tag prg/app:v.1 <username/repo>:app_v1

    $ docker image push <username/repo>:app_v1

    $ docker run --name sampleapp -p 8080:8080 -p 50000:50000 <username/repo>:app_v1
    ```

4. <b>Using Helm </b> -
    
   Prerequisite: 
   -    [Helm](https://helm.sh/docs/intro/install/) installation on local machine.
   -    Either minikube or any other cluster configured.

   Navigate to helm directory in you workspace.

   ```sh
    $ helm upgrade install app -n tomcatapps ./helm
   ```
   Refer [here](https://helm.sh/docs/intro/using_helm/) for Helm usage.

3. <b>Using Jenkins Pipeline </b> - This repository contains a Jenkinsfile. You can implement a pipeline as code using this JenkinsFile. 

## License

Apache 2.0 © [Parag Bawane]()