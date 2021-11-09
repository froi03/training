Step #1: Crete a network named 'mynet'
docker network create mynet 

Step #2: Crete a volume named 'mysql-vol'
docker volume create mysql-vol

Step #3: Run northwind-db container with name 'workshop-mysql' and attached it to mynet network and use mysql-vol volume storage
docker run --name workshop-mysql \
    --network mynet  \
    -e MYSQL_ROOT_PASSWORD=changeit \
    -v mysql-vol:/var/lib/mysql \
    -d stackupiss/northwind-db:v1

Step #4: Go inside the container
docker exec -it workshop-mysql bash

Step #5: Login to mysql and check the database
mysql -uroot -pchangeit

mysql> show databases;
mysql> use northwind;

Step #6: Launch the northwind app with the environment variables and attached it to mynet network using workshop-mysql db
docker run -d -p 8080:3000 \
    -e DB_HOST=workshop-mysql \
    -e DB_USER=root \
    -e DB_PASSWORD=changeit \
    --network mynet \
    --name northwind-app \
    stackupiss/northwind-app:v1