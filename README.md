# Dockerised_CondaApp

## Main commands 

### Building Docker Image

   ```

   docker build -t <IMG_NAME> .   
   <!--If the directory of the dockerfile and the present directory is same-->
   docker build -f <PATH_TO_DOCKERFILE> -t <IMG_NAME> <DOCKERFILE DIRECTORY> 
   <!--If the directory of the dockerfile and the present directory is not same-->

   ```

### Running The Docker Image

```

docker run --name <CONTAINER_NAME> -d -p 5000:<LOCAL_HOST_PORT> <IMAGE_NAME>

```

### Visiting API

+ To check if the container is running run the following command :-

```

docker ps

```

+ Now access the API by opening the following LINK: https://localhost:<HOST_PORT>/
  Use xdg-open https://localhost:<HOST_PORT>/ to open the link through the terminal and to view the interface .

## How to see available ports in the local host machine?

+  Open a new terminal on your Ubuntu system and copy the code given below:
``` 

  # Specify the range of ports you want to check (e.g., 8000 to 9000)
  start_port=8000
  end_port=9000

  # Use a loop to check each port in the specified range
  for port in $(seq "$start_port" "$end_port"); do
    # Use netstat or ss to check if the port is in use
    if ! ss -tuln | grep -q ":$port\b"; then
      echo "Port $port is available"
    fi
  done

  ```

  ## Basic file structure for the project

  + ├── conda-flask-api
  + ├── start.ps1      		<- Windows powershell to build and run our docker
  + ├── Dockerfile     		<- Instructions to build our container
  + ├── environment.yml		<- Conda environment.yml
  + ├── serve.sh			<- bash script to run an application server
  + └── raw            		
  + └── flask-api.py	<- the flask app

 ## Purpose Of the project

 + You’ve conducted your research, analysed the data, built models, and packaged everything up in a user friendly 
   application that is ready to be shared and published within your research community and beyond.

 + There’s just one problem. Whilst your models and application run fine on your own development machine, perhaps 
   this machine is running on an older University Windows Server, or you’ve collected and installed various packages and 
   code libraries over time, set environment variables, configurations, etc etc. How can someone re-create your environment 
   to run and use all of your hard work?

 + Containers solve this problem by packaging up all of the software, settings and code used to execute and  
   application within a single and sharable Docker Container. In addition, containerization enables a wide variety of 
   different operating systems and environments to be used from a single underlying host’s operating system and 
   infrastructure.

   ## Code explanation for Dockerfile

   + This Dockerfile optimizes the creation of a Docker container for hosting a straightforward Python Flask API.  
     It commences by selecting the "continuumio/miniconda3" image from Docker Hub as the base. Subsequently, it appends 
     informative metadata labels, including the maintainer's identity, GitHub repository URL, version number, and a concise 
     image description.

   + The working directory is configured as "/home/flask-api," and it copies essential files: "environment.yml" 
     and the "app" directory. The "serve.sh" script is granted execution permissions using the "chmod" command. The next 
     step focuses on enhancing the Conda environment by updating it with specifications from the "environment.yml" file. 
     Furthermore, environment variables for Conda are established, ensuring a smooth operational environment.

   + Port 5000 is exposed, notifying Docker of the port to be used during runtime. Lastly, the Dockerfile designates the         "serve.sh" script as the entry point, dictating its execution when the container initiates. This refined Dockerfile 
     streamlines the process of building a Docker image tailored for deploying a Python Flask API within a container.

   ## Code explanation for Flask file

   + This script initializes a Flask application, defines routes for the API description and a function to square 
     a given integer, and then starts the app for local debugging or production use. The API description is in HTML format, 
     and the script handles requests and parameter validation, returning responses accordingly.

   ## Expected Output

    + At the root URL, the API provides information about its functionality, including details about its endpoints. 
      Additionally, it demonstrates a valid sample request to return the square of the "value" parameter supplied by the 
      user, offering a clear illustration of  how to interact with the API.

   ## Updates 

   + The multi-stage Docker build was used for the Dockerfile to optimize the final Docker image by minimizing its size and 
     improving security. This approach involves using multiple intermediate images to build and compile the application in 
     one stage and then copying only the necessary files and artifacts to a smaller, more efficient base image in the 
     second stage.

   + The multi-stage build helps reduce the final image size because the build environment (Stage 1) contains a 
     full set of tools and libraries needed for compiling and building the application, while the final image (Stage 2) is 
     much smaller, containing only the runtime dependencies. This not only makes the image more lightweight but also 
     improves security by reducing the attack surface of the container.  

   + The use of distroless image is used as a base image because this Dockerfile uses the "gcr.io/distroless/ python3" base 
     image, which is exceptionally lightweight and contains only essential components for running a Python application. 
     This minimizes the final image size  and reduces the attack surface, enhancing security and resource efficiency.

   + Instead of installing system-wide dependencies, it creates a Conda virtual environment based on the "environment.yml" 
     file, isolating the Flask API's dependencies. This approach simplifies management and maintenance while ensuring a 
     clean and efficient container setup.
