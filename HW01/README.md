# de-zoomcamp-work

# Question 1: Understanding Docker images

## Answer 1:
```docker run --it --entrypoint bash python3.13```
```pip -V```

 `pip 25.3 from /usr/local/lib/python3.13/site-packages/pip (python 3.13)`

 # Question 2. Understanding Docker networking and docker-compose

 ## Answer 2:
 Copied over the [docker-compose.yaml](docker-compose.yaml) file to test and confirm.

 Both `postgres:5432` and `db:5432` are viable options. Firstly, the postgres container is mapping to host machine port `5433`, while the pgadmin container is mapping to the host machine port `8080`. But these containers need to communicate to one another. So we have to tell pgadmin to map to the postgres container port of `5432`. Since these containers are in the same network (which was automatically created), they can see eachothers ports.

 Secondly, `postgres` or db as the `db` are both valid as the container name is `postgres` and the service name is `db`. Docker can register both, though it sounds like the better practice is to use the service name.