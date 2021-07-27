<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
-->

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/aldomatus/flask-sqlachemy_postgresql_rest-api">
    <img src="https://i.imgur.com/DoWW1wx.png" alt="Header" >
  </a>
   <div align="center">
   <a href="https://www.facebook.com/aldo.matusmartinez" ><img src="https://github.com/edent/SuperTinyIcons/blob/master/images/svg/facebook.svg" title="Facebook" width="60"  margin="30px"/></a><a href="https://github.com/aldomatus/" ><img src="https://github.com/edent/SuperTinyIcons/blob/master/images/svg/github.svg" title="Github" width="60"/></a><a href="https://www.instagram.com/aldomatus1/" ><img src="https://github.com/edent/SuperTinyIcons/blob/master/images/svg/instagram.svg" title="Instagram" width="60"  /></a><a href="https://www.linkedin.com/in/aldomatus/" ><img src="https://github.com/edent/SuperTinyIcons/blob/master/images/svg/linkedin.svg" title="Linkedin" width="60"  /></a>

  </div>

  <h4 align="center"></h4>

  <p align="center">
    A REST api made with Flask in which you can practice the methods: GET, POST, PUT and DELETE, of a tasks list application. This API will be connected to the postgres database.
  </p>
</p>



<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

This project is made with the intention of teaching how to use Docker with Flask in the project we are going to take into account the following points:

* Create the dockerfile that will have the necessary instructions to create a Python image that will later be converted into a single application.
* Docker Compose allows you through YAML files to instruct the Docker Engine to perform tasks, programmatically. Here we will install the mysql image, declare the environment variables for both mysql and Flask, and also declare the volumes.
* We will add the list of requirements in a requirements.txt file that will be executed automatically within the Dockerfile
* Create a REST API with the methods: GET, POST, PUT and DELETE

### Built With

This section should list any major frameworks that you built your project using. Leave any add-ons/plugins for the acknowledgements section. Here are a few examples.
* [Flask](https://flask.palletsprojects.com/en/2.0.x/)
* [Postgres](https://www.postgresql.org/)

### Libraries

#### SQLAlchemy (Offers an ORM along with a Core)
The Python SQL Toolkit and Object Relational Mapper
SQLAlchemy is the Python SQL toolkit and Object Relational Mapper that gives application developers the full power and flexibility of SQL.

#### Flask-Marshmallow (Serializer)
Flask-Marshmallow is a thin integration layer for Flask (a Python web framework) and marshmallow (an object serialization/deserialization library) that adds additional features to marshmallow, including URL and Hyperlinks fields for HATEOAS-ready APIs. It also (optionally) integrates with Flask-SQLAlchemy.


### To check your rest api
#### Insomnia

With their streamlined API client, you can quickly and easily send REST, SOAP, GraphQL, and GRPC requests directly within Insomnia.
Link to visit insomnia website: - [Link](https://insomnia.rest/download)
<div align="center">
 <img src=https://seeklogo.com/images/I/insomnia-logo-A35E09EB19-seeklogo.com.png width="150" alt="Header" >
  </div>


#### Postman
Postman is a collaboration platform for API development. Postman's features simplify each step of building an API and streamline collaboration so you can create better APIs—faster.
Link to visit postman website: - [Link](https://www.postman.com/downloads/)
<div align="center">
 <img src=https://seeklogo.com/images/P/postman-logo-F43375A2EB-seeklogo.com.png width="150" alt="Header" >
</div>


<!-- GETTING STARTED -->
## Getting Started



### Prerequisites
For this project you need to have the postgres database manager installed. If you have not installed it yet, you can install it by entering the following: link, you can work with its graphical interface or from the console, both ways will serve you.
Let's create a database from the terminal:

1. Once postgres is installed we can open a terminal and type the following code to access postgres
```
   sudo -u postgres psql
   
```

2. once in postgres we can create a database for our application. You can decide the name of the database, in this case I will call it flaskpostgres
```
   create database flaskpostgres;
   
```
3. You can check that your database is created with the following command, where all the databases that you have on your computer will appear, among them must be flaskpostgres
```
   \l
   
```
You will see something like this:
```
                                      List of databases
       Name        |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-------------------+----------+----------+-------------+-------------+-----------------------
 articulosclientes | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 flaskpostgres     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 flaskprueba1      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 online_store      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
```

<!-- EXPLAIN CODE -->
## Description of the REST API code
<details close="close">
    <summary>Click to see all the code</summary>
    
```python
# Flask
from flask import Flask, request, jsonify, render_template
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:postgres@localhost/flaskpostgres'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
ma = Marshmallow(app)


class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(70), unique=True)
    description = db.Column(db.String(100))

    def __init__(self, title, description):
        self.title = title
        self.description = description

db.create_all()

class TaskSchema(ma.Schema):
    class Meta:
        fields = ('id', 'title', 'description')


task_schema = TaskSchema()
tasks_schema = TaskSchema(many=True)

@app.route('/', methods=['GET'])
def home():
    return jsonify({'message': 'Welcome to my API'})

@app.route('/tasks', methods=['POST'])
def create_task():
    title = request.json['title']
    description = request.json['description']

    new_task= Task(title, description)

    db.session.add(new_task)
    db.session.commit()

    return task_schema.jsonify(new_task)

@app.route('/tasks', methods=['GET'])
def get_tasks():
    all_tasks = Task.query.all()
    result = tasks_schema.dump(all_tasks)
    return jsonify(result)

@app.route('/tasks/<id>', methods=['GET'])
def get_task(id):
    task = Task.query.get(id)
    return task_schema.jsonify(task)

@app.route('/tasks/<id>', methods=['PUT'])
def update_task(id):
    task = Task.query.get(id)
    title = request.json['title']
    description = request.json['description']

    task.title = title
    task.description = description

    db.session.commit()
    return task_schema.jsonify(task)

@app.route('/tasks/<id>', methods=['DELETE'])
def remove_task(id):
    task = Task.query.get(id)
    db.session.delete(task)
    db.session.commit()
    return task_schema.jsonify(task)
    

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```
  
</details>
  
### Describing the code 
  1. first import libraries, __name__ is just a convenient way to get the import name of the place the app is defined. Flask uses the import name to know where to look up resources, templates, static files, instance folder, etc. The application instance is an object of class Flask:
```python
  
# Flask
from flask import Flask, request, jsonify, render_template
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app = Flask(__name__)
```
 2. Configure Flask by providing the PostgreSQL URI so that the app is able to connect to the database, through : app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://DB_USER:PASSWORD@HOST/DATABASE' where you have to replace all the parameters in capital letters (after postgresq://). Find out more on URI definition for PostgreSQL [here](https://www.postgresql.org/docs/9.3/libpq-connect.html#AEN39449).
  
```python  
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:postgres@localhost/flaskpostgres'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
```  
 3. We make the instances of SQLALchemy (to work with the database) and Marshmallow (For schemas and work with the database) of our application app
```python 
db = SQLAlchemy(app)
ma = Marshmallow(app)
``` 
4. should include the definition of classes which define the models of your database tables. Such classes inherit from the class db.Model where db is your SQLAlchemy object.
  
```python 
class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(70), unique=True)
    description = db.Column(db.String(100))

    def __init__(self, title, description):
        self.title = title
        self.description = description
db.create_all()
```
5. Generate marshmallow Schemas from your models using SQLAlchemySchema or SQLAlchemyAutoSchema.
```python 
class TaskSchema(ma.Schema):
    class Meta:
        fields = ('id', 'title', 'description')


task_schema = TaskSchema()
tasks_schema = TaskSchema(many=True)
```
6. The client (such as a web browser) sends the request to the web server and the web server sends the request to the Flask application instance. The application instance needs to know what code to execute for each URL request, so the application instance keeps a function mapping relationship from URL to Python. The program that handles the relationship between URLs and functions is called routing.

Use what is provided by the application instance in Flask app.route The decorator registers the decorated function as a path:
> A decorator (@) is a design pattern in Python that allows a user to add new functionality to an existing object without modifying its structure. Decorators are usually called before the definition of a function you want to decorate.
  
```python
  
@app.route('/', methods=['GET'])
def home():
    return jsonify({'message': 'Welcome to my API'})
```
  
   3. The following decorated function uses the POST method. "Used to send HTML form data to the server. The data received by the POST method is not cached by the server."[1] The following decorated function uses the POST method. The most common method. A GET message is send, and the server returns data. In our example we receive the data sent wi

```python
  
@app.route('/tasks', methods=['POST'])
def create_task():
    title = request.json['title']
    description = request.json['description']

    new_task= Task(title, description)

    db.session.add(new_task)
    db.session.commit()

    return task_schema.jsonify(new_task)
```
  
  
 4. The following decorated function uses the GET method. The most common method. A GET message is send, and the server returns data. In our case, it will return the list of products to us.
```python
  
# get data routes
@app.route('/products', methods=['GET'])
def getProducts():
    return jsonify({'products': products})
```
  5. Like the previous function, the next one uses the GET method, but as we can see, within the path / products / <string: product_name> we obtain a variable product_name of type string, which our client will send us so that we can send the specific product information.
```python
  
@app.route('/products/<string:product_name>')
def getProduct(product_name):
    productsFound = [product for product in products if product['name'] == product_name]
    if len(productsFound)>0:
        return jsonify({'product': productsFound[0]})
    return jsonify({'message': 'Product not found'})
```
  
  
  
    6. GET method "replace all current representations of the target resource with uploaded content"[1], We obtain the name of the specific product with the product_name variable, with the help of the request we extract the information received from the client and then save it either in a database or as in this case that being only an example we save it in the productsFound variable that later will be lost.
```python
  
  # Update products
@app.route('/products/<string:product_name>', methods=['PUT'])
def editProduct(product_name):
    productsFound = [product for product in products if product['name'] == product_name]
    if len(productsFound) > 0:
        productsFound[0]['name'] = request.json['name']
        productsFound[0]['price'] = request.json['price']
        productsFound[0]['quantity'] = request.json['quantity']

        return jsonify({
            'message': 'Product updated',
            'product': productsFound[0]
        })
    return jsonify({'message': 'Product not found'})
```

  
      7. DELETE: "Deletes all current representations of the target resource given by the URL"[1], with the listcompehension we can find the product from the name we receive from the url, once the product is found we can use the remove function to remove it from the list and send the user the list with its removed product.
  
  ```python
  
# Delete products
@app.route('/products/<string:product_name>', methods=['DELETE'])
def deleteProduct(product_name):
    productFound = [product for product in products if product['name'] == product_name]
    if len(productFound) > 0:
        products.remove(productFound[0])
        return jsonify({
            'message': 'Product deleted',
            'products': products
        })
    return jsonify(
        {'message': 'Product not found'}
    )
```
      8. using the application instance run Method to start the embedded Flask web server:
  
```python
  
if __name__=='__main__':
    app.run(debug=True, port=5000)
```


### Installation

1. To obtain my repository you must create a folder in a desired directory and within this folder open a terminal or use cmd in the case of windows.
2. Clone the repo
   ```
   git remote add origin git@github.com:aldomatus/flask-docker_simple-rest-api.git
   
   ```
3. Make the pull request from a branch called main
   ```
   git pull origin main --allow-unrelated-histories
   
   ```
  > git branch -m main is the command to rename the branch
  
4. In the folder where docker-compose.yml is located, open a terminal (the same address where you ran the previous line) and write the following command to build the image.
   ```
   docker-compose build
   ```
5. Once the previous execution is finished, you must run the services made in the build.
   ```
   docker-compose up
   ```
6. If all goes well, our application should already be executing the app.py file with python using the mysql database, now we just have to check by entering the following link in our browser:

   ```
   http://localhost:5000/
   ```
7. You should have a response like this:
   ```
   {"message": "Welcome to my API"}
   ```


<!-- USAGE EXAMPLES -->
## Usage

With this base you can make any flask code, modify the API and adapt it to your projects. It is important that you study the docker code to understand what is behind each file in both the Docker and the docker-compose.yml.



<!-- ROADMAP -->
## Roadmap

See the [open issues](https://github.com/aldomatus/flask_rest_api/issues) for a list of proposed features (and known issues).



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.

## References 
  - [1 pythonbasics.org](https://pythonbasics.org/flask-http-methods/) 

<!-- CONTACT -->
## Contact

Aldo Matus - [Linkedin](https://www.linkedin.com/in/aldomatus/) [Facebook](https://www.facebook.com/aldo.matusmartinez/)

Project Link: [Repository](https://github.com/aldomatus/flask_rest_api/)






