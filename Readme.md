# GK862 GKUE
**Christof Zlabinger**

## MySQL in docker

````bash
docker run --name dzs-mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql
docker exec -it bash
mysql -u root -p root
````
```sql
CREATE DATABASE users;
```

## Create Project
```bash
mvn archetype:generate -DgroupId=at.czlabinger -DartifactId=WAREHOUSE_ORM -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

### Configure spring boot
```
# application.properties

# Name of Project
spring.application.name=WAREHOUSE_ORM 

# SQL dialect and host/database
spring.datasource.url=jdbc:mysql://localhost:3306/warehouse

#Account username and password to use
spring.datasource.username=root
spring.datasource.password=root

# Automatically create tables if not existing
spring.jpa.hibernate.ddl-auto=update
spring.jpa.generate-ddl=true

# Driver to connect to DB
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# MySQL dialect version to use
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```


## Code

### User class

This class represents one entry in the user table.

```java
@Entity // This class represents an entry in the user table
public class User {
    @Id // This field should be used as Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY) // The value should be created automatically and be unique
    int id;
    
    /... More fields
}
```

### UserRepository interface

This interface is used to acess the User table in the DB. The Crud Repository provides some opperations to Create, Read, Update and Delete data in the table.

```java
// User is the Object type that should be returned and Integer is the type of the primary key.
public interface UserRepository extends CrudRepository<User, Integer> {}
```

### MainController class

This class handles the request mappings.


This method handles the /add post request.
It creates a new User with the given arguments and saves it in the table.
```java
@PostMapping(path="/add")
    public @ResponseBody String addNewUser (@RequestParam String email, @RequestParam String password) {
        User n = new User();
        n.setEmail(email);
        n.setPassword(password);
        userRepository.save(n);
        return "Saved";
    }
```

This method mandles the /all get request.
It returns all Users stored in the table.

```java
    @GetMapping(path="/all")
    public @ResponseBody Iterable<User> getAllUsers() {
        return userRepository.findAll();
    }
```
