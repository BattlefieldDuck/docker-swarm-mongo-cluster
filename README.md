# Docker Swarm MongoDB Cluster

This repository contains a demo setup for deploying a MongoDB cluster on a Docker Swarm. The setup includes Docker Compose files and scripts to orchestrate the deployment of a highly available MongoDB cluster.

## Features

- **Docker Swarm Integration**: Leverage Docker Swarm for container orchestration and management.
- **MongoDB Replica Set**: Deploy a MongoDB replica set to ensure high availability and data redundancy.
- **Scalability**: Easily scale your MongoDB cluster by adding more nodes.
- **Health Checks**: Integrated health checks to monitor the status of MongoDB instances.
- **Persistent Storage**: Use Docker volumes to persist MongoDB data across container restarts.

## Prerequisites

- Docker (version 20.10 or higher)
- Docker Compose (version 3.7 or higher)
- Docker Swarm initialized on your environment, at least 1 manager, 2 workers

## Getting Started

1. **Clone the repository**:
    ```sh
    git clone https://github.com/yourusername/docker-swarm-mongo-cluster.git
    cd docker-swarm-mongo-cluster
    ```

2. **Deploy the MongoDB cluster**:
    ```sh
    docker stack deploy -c docker-compose.yml mongo-cluster
    ```

3. **Check the status of the services**:
    ```sh
    docker stack services mongo-cluster
    ```

4. **Find the MongoDB container ID**:
    ```sh
    docker container ls
    ```

5. **Initialize the MongoDB replica set**:
    ```sh
    docker exec -it <mongo_container_id> mongosh --host mongo1:27017 --eval 'rs.initiate({
         _id: "rs0",
         members: [
           { _id: 0, host: "mongo1:27017" },
           { _id: 1, host: "mongo2:27017" },
           { _id: 2, host: "mongo3:27017" }
        ]
       })'
    ```

6. **Check the replica set status**:
    ```sh
    docker exec -it <mongo_container_id> mongosh --host mongo1:27017 --eval 'rs.status()'
    ```

7. **Exit the MongoDB shell**:
    ```sh
    exit
    ```

## Scaling the MongoDB Cluster

To scale your MongoDB cluster by adding 2 more nodes (for a total of 5 nodes), follow these steps:

1. **Add Swarm Worker Nodes**: Ensure you have added the necessary Swarm worker nodes to your Docker Swarm cluster. You can add a worker node using the following command on the new node:
    ```sh
    docker swarm join --token <worker_token> <manager_ip>:2377
    ```

2. **Edit the `docker-compose.yml` file**:
    ```yaml
    services:
      mongo:
        deploy:
          replicas: 5
    ```

3. **Deploy the updated stack**:
    ```sh
    docker stack deploy -c docker-compose.yml mongo-cluster
    ```

4. **Find the MongoDB container ID**:
    ```sh
    docker container ls
    ```

5. **Add the new nodes to the replica set**:
    ```sh
    docker exec -it <mongo_container_id> mongosh --host mongo1:27017 --eval 'rs.add("mongo4:27017")'
    docker exec -it <mongo_container_id> mongosh --host mongo1:27017 --eval 'rs.add("mongo5:27017")'
    ```

6. **Check the replica set status**:
    ```sh
    docker exec -it <mongo_container_id> mongosh --host mongo1:27017 --eval 'rs.status()'
    ```

7. **Exit the MongoDB shell**:
    ```sh
    exit
    ```

By following these steps, you can scale your MongoDB cluster to include additional nodes, ensuring greater availability and redundancy.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.
