+++
ShowToc = true
date = 2022-07-21T18:30:00Z
description = "Spinning up a MySQL Database with Docker, this is how I personally go about it and some other useful refrences."
tags = ["getting_started", "docker", "mysql"]
title = "Spinning up MySQL Database with Docker"
[cover]
alt = "Spinning up MySQL with Docker"
image = "/uploads/mysql_docker_blog_cover.png"

+++
This is the best way to start since you don't have to install MySQL locally and configure it and run into like thousands of issues and in the end not being able to make it, docker makes it much easier for me or probably for you also to spin up a MySQL database in minutes.

**To get started follow these steps:**

* Pull the latest image, 

      docker pull MySQL:latest
  * You don't have to do it manually it will download automatically if not installed when we spin up the container.
* Start the container using the following command

    docker run --name <container_name> -p 3306:3306 -v mysql_volume:/var/lib/mysql/ -d -e "MYSQL_ROOT_PASSWORD=<root_password>" mysql

This will create a new container with the name *<container_name>_, with the_ **mysql:latest** image and publish the ports **3306** to the local environment to use the instance outside of the container.

This also sets up the volume for the container to persist the database's data, so that it doesn't reset on restarts of the host system or the container.

We also pass in the environment variable _MYSQL_ROOT_PASSWORD_ to set a root password for the database.

If not provided, it will generate a random strong password, we can see it in the container logs using this command:

    docker container logs <container_name>

You can pass in the following _Environment Variables_ when starting the container:

* **MYSQL_ROOT_PASSWORD** Setup your password using this environment variable.
* **MYSQL_ALLOW_EMPTY_PASSWORD** Blank or Null password will be set. You have to set **MYSQL_ALLOW_EMPTY_PASSWORD=1**.
* **MYSQL_RANDOM_ROOT_PASSWORD**  random password will be generated when the container is started. You have to set **MYSQL_RANDOM_ROOT_PASSWORD=1** to generate the random password.

> **Don't pass in the permanent password in production**, as the password will be visible in the shell history, which we obviously don't want. 
>
> A possible solution is that we pass in a temporary password like _temp123_ and then change it afterwards.

**We can change the root password as follows:** 

* Get into the container using \`docker exec -it <container_name> bash\`.
* Login into the MySQL shell using 

      mysql -u root -p
* `this will prompt to `enter the passw`ord which we set`up bef`ore, in our case` it was _temp123_.
* After logging into the shell run the following SQL query to change the password

    ALTER USER 'root'@'localhost' IDENTIFIED BY '<new_password>';

* References:
  * View [this](https://www.instapaper.com/read/1523582174/20140126 "Instapaper Read") article on Instapaper with notes.
  * View the original one [here](https://ostechnix.com/setup-mysql-with-docker-in-linux "Original Article").