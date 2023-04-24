## MongoDB cluster


# 1. Create a Docker network.

docker network create mongoCluster

# 2. Start three instances of MongoDB.

docker run -d --rm -p 27017:27017 --name mongo1 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo1
docker run -d --rm -p 27018:27017 --name mongo2 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo2
docker run -d --rm -p 27019:27017 --name mongo3 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip localhost,mongo3


# 3. Initiate the Replica Set.

docker exec -it mongo1 mongosh --eval "rs.initiate({
 _id: \"myReplicaSet\",
 members: [
   {_id: 0, host: \"mongo1\"},
   {_id: 1, host: \"mongo2\"},
   {_id: 2, host: \"mongo3\"}
 ]
})"

# Test and Verify the Replica Set

docker exec -it mongo1 mongosh --eval "rs.status()"

# Stop mongodb container

docker stop mongo1
docker stop mongo2
docker stop mongo3
