# MSSQL-Docker

Use Docker to setup MS-SQL Server

Get Docker Desktop for your system: https://www.docker.com/products/docker-desktop if you don't have it running already.


## Volumes doesn't work on MacOS.
    # Ref: https://github.com/Microsoft/mssql-docker/issues/12

## To Tear Down
```
$ docker-compose down --volumes
```