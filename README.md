> This is my first dive into MongoDB with Docker

## Docker Compose

`docker-compose.yaml` is a configuration file used with *Docker Compose*; used to define and manage multi-container Docker applications.

`docker-compose.yaml` defines a set of services, networks, volumes, and other configurations required for the application. 

1. **Services:** Each service corresponds to a container that is part of your application. It defines the Docker image to use, environment variables, ports to expose, volumes to mount, and more. Services can interact with each other through networking, which Docker Compose manages.

2. **Networks:** Networks define how the containers within the application can communicate with each other. Docker Compose automatically creates a network for your application if you don't explicitly define one. This allows containers to communicate using service names as hostnames.

3. **Volumes:** Volumes define where and how data should be stored and shared between containers. Volumes can be used to persist data even when containers are stopped or removed.

4. **Other Configurations:** The docker-compose.yaml file can also include other configurations like environment variables, restart policies, build instructions (for building custom images), and more.

# Docker setup
Following are the list of commands to create docker containers. See [`docker-compose.yaml`](https://github.com/bithapa/docker-mongodb/blob/main/docker-compose.yaml) file for mongodb docker configuration.

### `docker-compose -f docker-compose.yaml up`

used to start the services defined in `docker-compose.yaml` configuration file. It will create and start the containers specified in the services section of the `.yml` file. If the containers do not exist, they will be built according to the images specified and then started. This command also handles linking containers, networking, and volume mounting based on the configuration you've provided. Also 

- `docker compose -f docker-compose.yaml up`

Don't need to specify default file name

- `docker cpmpose -f up`

[-f flag is used to specify the path to a Docker Compose configuration file]

### `docker compose down`

used to stop and remove the containers; It will stop the containers that were started with docker-compose up and remove their associated resources, including volumes, networks, and named containers. This effectively shuts down the services that were launched with docker-compose up.

### `docker compose up -d`

starts the container in detached mode (means the containers will run in the background); creates and starts the containers according to the services defined in your docker-compose.yml file.

### `docker compose stop`

stops the containers that are currently running; gracefully stops the containers, allowing them to clean up resources before shutting down

### `docker compose start`

starts the containers that were previously stopped using the docker compose stop command; resumes the containers from the state they were in when they were stopped.

### `docker ps`

lists and displays information about the currently running Docker containers on your system.

---

# Connecting to MongoDB
Using Bash shell (Mongo Shell) inside the docker container to interact with database.

### `docker exec -it <coontainer-id> bash`

to run an interactive bash shell inside the docker container; will open up a bash shell (TTY) inside the provided container.

### `mongosh mongodb://localhost:<container-port> -u <db-username> -p <db-password>`

connects to a MongoDB server inside the MongoDB Shell using the username and password specified. 

Now you can interact with the database.

# MongoDB: Database Interaction

Within the MongoShell, you can use following commands:

### database
- **`use <database>`**: Creates/Switches to the specified database.
- **`show dbs`**: Lists all available databases.
- **`db.stats()`**: Provides statistics about the current database.
- **`db.dropDatabase()`**: Deletes the current database.
- 
### collection
- **`db.createCollection()`**: Creates a collection in the current database.
- **`show collections`**: Lists all collections in the current database.
- **`db.collection.stats()`**: Provides statistics about the current collection.
- **`db.collection.drop()`**: Deletes the specified collection.
- **`db.collection.count()`**: counts the documents in collection
- **`db.collection.find()`**: shows all the documents

### insert
- **`db.collection.insert()`**: inserts the document (json object passed as parameter) into specified `collection`
- **`db.collection.insrtMany()`**: inserts multiple documents

### find

- **`db.collection.find({<field value(s)>})`**: finds the document that matches the provided field values
```
btDB> db.student.find({firstName:"Sophia"},{firstName: 1, favoriteSubjects: 1})
[
  {
    _id: ObjectId("64e3b2ad38d56c69f4f69607"),
    firstName: 'Sophia',
    favoriteSubjects: [ 'history', 'anthropology', 'archaeology' ]
  }
]
```
- **`db.collection.find({field value(s)},{projection})`**: finds the document that matches the criteria, and displays in specified projection
*To exclude any field, use -1.*

### update
Updates are done by providing the unique keywords for type of updates, along with ObjectId, and update field/value pair.
- **`db.collection.update({_id: ObjectId("<id>")}, {$<keyword>: {<update parameters & values>}})`**

Following are the list of keywords and their corresponding update operations:
- **`$set`** finds the match and updates 
- **`$unset`** removes/deletes the specified field
- **`$inc: {<parameter>: N}`** increments the parameter (type **double**) by `N`

To **update the elements of an array field use `push` & `pull`:**
- **`$push: {arrFieldName: fieldVal}`** inserts new value in the `arrFieldName`
- **`$pull: {arrFieldName: fieldVal}`** removes `fieldVal` from the `arrfieldName` 

### delete Document

- **`db.collection.deleteOne(_id: ObjectId("<id>"))`** deletes the object of given `_id`
- **`db.collection.deleteOne({})`** only deletes one doc object
- **`db.collection.deleteMany({field: value})`** deletes all the object matching given field/value pair

---

# Mongo Document Data types
MongoDB Collection documents uses BSON data types. BSON (Binary JSON) is a binary-encoded serialization format used to store and exchange data in a lightweight and efficient manner.

1. **Double**: Represents a double-precision floating-point number.

2. **String**: Represents a sequence of UTF-8 encoded characters.

3. **Object**: Represents an embedded document within a BSON document. This is similar to a JSON object.

4. **Array**: Represents an ordered list of values, similar to a JSON array.

5. **Binary Data**: Represents binary data, such as images or files.

6. **Boolean**: Represents a boolean value (true or false).

7. **DateTime**: Represents a 64-bit integer, typically representing the number of milliseconds since the Unix epoch.

8. **Null**: Represents a null value.

9. **Regular Expression**: Represents a regular expression pattern and its options.

10. **JavaScript Code**: Represents JavaScript code or a script.

11. **Symbol**: Represents a symbol, which is a unique identifier.

12. **JavaScript Code with Scope**: Represents JavaScript code with an associated scope.

13. **32-bit Integer**: Represents a 32-bit integer.

14. **Timestamp**: Represents a special BSON data type used for timestamps.

15. **64-bit Integer**: Represents a 64-bit integer.

16. **Decimal128**: Represents a 128-bit decimal floating-point value.

17. **Min Key**: Represents the smallest possible key value.

18. **Max Key**: Represents the largest possible key value.


### Creating document

First define document structure.

