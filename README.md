# Node JS

This is simple Node JS application that demonstrate creating a simple REST API using sails js
and connecting with MS SQL.

## Node + Sails

To start, you’ll need node.js in your machine. Download and install node from http://nodejs.org/ if haven’t already. Then to start using sails you need to grab the package. To do this, go to your node js console and run

```javascript
> npm install –g sails
```

The –g argument installs the package globally. Now to create a new project, run the command:

```javascript
> sails new MySailsProject
```

This command will create the project folder together with the files. Go to the directory:

```javascript
> cd MySailsProject
```

Then type the command:
```javascript
	> sails lift
```	
Easy as that, you now have a sails.js project running! By default, the project will run on port 1337. You can change the port in *config\local.js*, add the snippet below under module.export:

```javascript
port: <port number>
```

## Controller
Sails comes with generator for creating controller and model. To create a controller, type the following in the console:

```javascript
	> sails generate controller User
```

This will create a *UserController.js* under *api/controllers*. So, to add an action, simply add a function, e.g.:

```javascript
module.exports = {
	List: function (req, res){
		return res.json({Hello: 'Hello World!'});
	}
}
```

The code will be accessible through the url http://localhost:1337/User/List.

##MSSQL Adapter

Sails comes with Waterline ORM. To interact with the database, we’ll need an adapter, for this instance we will add a mssql adpater.

```javascript
> npm install sails-sqlserver-adapter --save
```

Configure the database connection in *config/connections.js*
```javascript
  sqlserver: {
    adapter: 'sails-sqlserver-adapter',
    user: 'robot',
    password: 'Passw0rd123',
    host: 'localhost',
    database: 'prototype'
  },
```

## Model

Creating the model is easy, simply use the generator:

```javascript
	> sails generate model User 
```

This will create a file *User.js* under *api/Models*. Add the fields under the attribute object:

```javascript
module.exports = {

  connection: 'sqlserver',

  attributes: {
  	Name: 'string',
  	Username: 'string',
  	Password: 'string'	
  }
};
```
You can checkout the list of data type on their documentation http://sailsjs.org/#/documentation/concepts/ORM/Attributes.html.


Run the Sails JS by going to the console and typing the command:

```javascript
	> sails lift
```

The system will let you choose which database migration option. Choose 3, this will drop/wipe the data table
and create create it base on the models you created.

Now check the MS SQL Database. The user table should be created.

Now we’ll add a user controller. Stop the sails js by going to the console where the sails is running and press *Crt+C*.
Now on the console type the command below:

```javascript
> sails generate controller User
```

Sails JS will insert the file UserController.js under *api/controllers* with that code append the code below:

```javascript

		add: function(req, res){
		var driver = {Name: 'Juan'};
		List: function (req, res){
		var name = req.param("name");
		User.find({}).exec(function(err, result){
			return res.json(result);	
		});
	},

	Add: function(req, res){
		var name = req.param("name");
		var username = req.param("username");
		var password = req.param("password");
		if (name == undefined || username == undefined || password == undefined){
			return res.serverError('invalid parameters.')
		}
		else{
			var user = {Name: name,
				Username: username,
				Password: password
			};

			User.create(user).exec(function(err, result){
				return res.json(result);
			});
		}
	}
};
	

Start the Sails JS application. The following url will now be available:

1. localhost:1337/user/list 
2. localhost:1337/user/add?name=<name>&username=<usenname>&password=<password>


