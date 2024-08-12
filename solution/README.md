CSVServer Assignment

This project demonstrates the deployment of a CSV server application using Docker and Docker Compose, with added metrics monitoring via Prometheus.
Project Structure

The project is divided into three parts:
- **Part-1**: Running the CSV Server Container
 - image: infracloudio/csvserver:latest



       docker run -d --name csvserver infracloudio/csvserver:latest

steate of The container exited with an error (Exited (1)) due to a missing file.

Error Troubleshooting:

  docker logs csvserver

    2024/08/10 17:44:59 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory

Generating the Required File:

    A script named **gencsv.sh** was used to generate the necessary input file (inputFile), which was then mounted into the container.


Running the Container with the Input File:

    docker run -d --name csvserver -p 9393:9300 -v "$(pwd)/inputFile:/csvserver/inputdata" infracloudio/csvserver:latest

The container is running successfully. 
kill the container need to be add env variable
Setting Environment Variables:

        docker run -d --name csvserver -p 9393:9300 -v "$(pwd)/inputFile:/csvserver/inputdata" -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest

**Part 2**: Docker Compose Setup



In this part, the Docker commands from Part 1 were converted into a docker-compose.yml file for easier management.

Environment Variables:
An environment file (csvserver.env) was created to store variables required by the Docker Compose file.

Docker Compose Configuration:
The docker-compose.yml file was created to automate the setup of the CSV server with the necessary configurations:


    docker-compose up -d

To stop the services, use:

   

        docker-compose down


**Part 3**: Adding Prometheus for Metrics Collection

  
Prometheus was integrated to collect metrics from the CSV server.

Prometheus Configuration:
A prometheus.yml file was created to specify the target application (csvserver:9300) for metrics collection.

Docker Compose Setup:
The docker-compose.yml file was updated to include the Prometheus container (prom/prometheus:v2.45.2), with the prometheus.yml file mounted as a volume.

Usage

To start the entire setup, run:


    docker-compose up -d

To stop the services, run:


    docker-compose down
