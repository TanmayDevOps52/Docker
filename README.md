# Docker  
Commands Use to Deploy SpringBoot CRUD app to Docker.

Pull MySQL image from docker
->docker pull mysql

Create a network to communicate SpringBoot App to MySQL Database
C:\Users\Tanmay\Desktop\ProductManager>->docker network create spring-boot-mysql-net

docker network ls 
list the networks

C:\Users\Tanmay\Desktop\ProductManager>docker run --name mysqldb --network springboot-mysql-net -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=sales -d mysql


Create a JAR File in the project using the Command 
mvn clean package

After JAR FIle gets created Create a dockerfile and Write the following commands

FROM eclipse-temurin:17



WORKDIR /app

COPY target/ProductManager-0.0.1-SNAPSHOT.jar /app/ProductManager-0.0.1-SNAPSHOT.jar

ENTRYPOINT ["java", "-jar", "ProductManager-0.0.1-SNAPSHOT.jar"]


Let's create an application-docker.properties file under the resources folder and the below property to active docker profile in the application.properties file:
spring.profiles.active=docker
Next, let's change the application-docker.properties file as per the docker environment:
spring.datasource.url=jdbc:mysql://mysqldb:3306/sales
spring.datasource.username=root
spring.datasource.password=root

#spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.hibernate.ddl-auto=update
The localhost won't work in the docker network so let's change it to the container name that is mysqldb.
spring.datasource.url=jdbc:mysql://mysqldb:3306/sales

Now build the file using the Command
C:\Users\Tanmay\Desktop\ProductManager>docker build -t productmanager . 

Tag the image with your username
C:\Users\Tanmay\Desktop\ProductManager>docker tag productmanager boogyman521/productmanager

Now push the image
C:\Users\Tanmay\Desktop\ProductManager>docker push boogyman521/productmanager

Run the Application
C:\Users\Tanmay\Desktop\ProductManager>docker run --network springboot-mysql-net --name springboot-mysql-container -p 9191:9191 boogyman521/productmanager


![image](https://user-images.githubusercontent.com/113125685/234085145-a8063cef-2b45-41ca-9b93-775816822739.png)
![image](https://user-images.githubusercontent.com/113125685/234085417-1e0b8bdf-8db6-49b8-9ee9-4eb8fc359271.png)
![image](https://user-images.githubusercontent.com/113125685/234085604-dfd9899b-3d24-4f56-a987-1ccbcb434ccc.png)

