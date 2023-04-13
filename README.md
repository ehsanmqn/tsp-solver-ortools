# TSP Solver & Web API
This project contains two different software projects as submodules: tsp-solver and tsp-api. The tsp-solver is a basic TSP/VRP solver designed according to the task specification. The tsp-api is a Django project that provides APIs to the user to make a RESTful web service with a few endpoints, allowing the customer to input problem instances and obtain results from the underlying pub/sub queues.

## Prerequisites
* Docker and docker-compose should be installed on the host machine.

## How to run
1. Clone the repository and then run the following command to initialize the submodules:
```bash
git submodule update --init --recursive
```

2. In order to run the whole application, you can use the following command:
```bash
docker-compose up
```

This command will start both the tsp-solver and tsp-api services along with a RabbitMQ message broker service. All the necessary dependencies will be downloaded and installed automatically.

3. Once the containers are up and running, you can access the tsp-api on your browser at http://localhost:8000/ or using any REST client tool like Postman.

## How to use
The tsp-api project provides the following endpoints:
* `/vrp-submit/`: This endpoint allows the customer to upload problem instance data in the form of a JSON string.
* `/vrp-status/`: This endpoint allows the customer to get result of an uploaded problem instance by sending the corresponding id field as a parameter.

More information regarding how to use these APIs provided in the corresponding project README.md file located in the [project repository](https://github.com/ehsanmqn/tsp-api).

## Architecture
The tsp-solver and tsp-api services are designed to communicate with each other using a message broker. In this case, we are using RabbitMQ as our message broker.

Both services are running within Docker containers and connected via a shared network. The tsp-solver service listens to incoming messages on the RabbitMQ queue and processes them based on the specified algorithm. Once the solution is obtained, it publishes the result on another queue. The tsp-api service reads to the result queue and sends the result back to the client upon getting the result.

## License
This project is licensed under the MIT License - see the LICENSE file for details.