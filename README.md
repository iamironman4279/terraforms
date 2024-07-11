# BankApp

This project is a banking application that utilizes Docker for containerization, Tomcat for serving the application, and MySQL as the database.

## Prerequisites

- Docker
- Docker Compose

## Setup

### Dockerfile

Create a Dockerfile in the root directory of your project with the following content:

Dockerfile
FROM tomcat:9.0
COPY bankapp.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]


### Docker Build

Build the Docker image using the following command:

sh
docker build -t bankapp-tomcat .


### Docker Network

Create a Docker network for the application:

sh
docker network create bankapp-network


### Docker Compose

Create a docker-compose.yml file in the root directory with the following content:

yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: bankapp-mysql
    environment:
      MYSQL_ROOT_PASSWORD: 4279
      MYSQL_DATABASE: bankapp
    networks:
      - bankapp-network
    ports:
      - "3306:3306"

  tomcat:
    image: bankapp-tomcat
    container_name: bankapp-tomcat-container
    build:
      context: .
      dockerfile: Dockerfile
    networks:
      - bankapp-network
    ports:
      - "8080:8080"

networks:
  bankapp-network:
    driver: bridge


### Start the Application

Use the following command to start the application:

sh
docker-compose up -d --build


## Java Database Connection

The Java class Database is responsible for managing the connection to the MySQL database. Below is the code for the Database class:

java
package com.bank;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Database {
    private static final String URL = "jdbc:mysql://bankapp-mysql:3306/bankapp";
    private static final String USER = "root";
    private static final String PASSWORD = "4279";

    // Static block to load the MySQL JDBC driver
    static {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            throw new RuntimeException("Error: MySQL JDBC Driver not found");
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}


## Repository Structure


.
├── Dockerfile
├── README.md
├── bankapp.war
├── docker-compose.yml
└── src
    └── main
        └── java
            └── com
                └── bank
                    └── Database.java


## Additional Information

- *Tomcat Version*: 9.0
- *MySQL Version*: 8.0
- *Port Mappings*:
  - Tomcat: 8080:8080
  - MySQL: 3306:3306

## Troubleshooting

If you encounter any issues, please check the Docker container logs using:

sh
docker logs <container_name>


Replace <container_name> with either bankapp-tomcat-container or bankapp-mysql.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to reach out if you have any questions or need further assistance.
