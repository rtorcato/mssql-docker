# MSSQL-Docker

Use Docker to setup MS-SQL Server

Get Docker Desktop for your system: https://www.docker.com/products/docker-desktop if you don't have it running already.

This repo was setup with help from this link: https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-2017&pivots=cs1-bash


## Notes for Macintosh users

Volumes doesn't work on MacOS. # Ref: https://github.com/Microsoft/mssql-docker/issues/12


## To Tear Down
```
$ docker-compose down --volumes
$ docker stop myMSSQL
$ docker rm myMSSQL
```

## Connect to container

Use the docker exec -it command to start an interactive bash shell inside your running container. In the following example sql1 is name specified by the --name parameter when you created the container.

```
sudo docker exec -it sql1 "bash"
```

Once inside the container, connect locally with sqlcmd. Sqlcmd is not in the path by default, so you have to specify the full path.
```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourNewStrong!Passw0rd>'
```

If successful, you should get to a sqlcmd command prompt: 1>

With the sqlcmd prompt you can create and query data.

### Create a Database
```
CREATE DATABASE TestDB
SELECT Name from sys.Databases
GO
```
### Insert data

```
USE TestDB
CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
GO
```

### Select data

```
SELECT * FROM Inventory WHERE quantity > 152;
GO
```

To end your sqlcmd session, type QUIT

## Connect from outside the container

You can also connect to the SQL Server instance on your Docker machine from any external Linux, Windows, or macOS tool that supports SQL connections.
The following steps use sqlcmd outside of your container to connect to SQL Server running in the container. These steps assume that you already have the SQL Server command-line tools installed outside of your container. The same principles apply when using other tools, but the process of connecting is unique to each tool.
Find the IP address for the machine that hosts your container. On Linux, use ifconfig or ip addr. On Windows, use ipconfig.
Run sqlcmd specifying the IP address and the port mapped to port 1433 in your container. In this example, that is the same port, 1433, on the host machine. If you specified a different mapped port on the host machine, you would use it here.

```
sqlcmd -S 10.3.2.4,1433 -U SA -P '<YourNewStrong!Passw0rd>'
```