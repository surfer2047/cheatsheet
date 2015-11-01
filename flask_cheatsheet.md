#Working with Flask

```python
virtualenv flasky
$ source bin activate
(flasky)$ pip install flask
```

##Basic Flask application Structure

All flask application needs to create a *application instance*,
The web server pass all the request it get pass to this application object using a protocol called WSGI
The application instance is an object of class **Flask** , the application object is created using the following code

```python
from flask import Flask
app = Flask(__name__)
```

The only required parameter to the Flask class is the name of the module or file which is given by the magic variable  name 
called **__name__** in python.


##Routes and View Functions

The association between the url and the functions in flask that handle the request is called the route, 
Route mapping is done through the decoraters in python app.route

```python
@app.route('/')
def index():
    return '<h1> Hello world </h1>'
```


> **note:** Decorators are the standard features of the python programming language, they can modify the behaviour of the functions. Here the function like index() are called the **View** functions.


we can also create the dynamic url mapping, for instance

```python
@app.route('/username/<name>')
def username(name):
    return "<h1> welcome to your Dashboard %s </h1>" % name
```

The portion enclosed in the angle bracket is the dynamic part.
The dynamic components are string by defaults in url, but we can also specify the type in the url like the **Integer** type, `/user/<int:id>`, this url
will be match if the id equals to the integer value, the flask url supports **int float path**


##Starting up a Server
```python
if __name__ == '__main__':
    app.run(debug=True)
```

Once the server startup it goes into the loop and only stop after hitting the control + C keystroke.

Now we can run our app by invoking our python interpreter as,
`python hello.py`

##Application and request Context

Flask use the **Contexts** to make certain objects globally accessibles,
Context enables flask to make access to the certain threads globally without intefering the other threads

Multithreaded web server starts with a **pool** of thread and select the thread from the pool to serve the incomming thread.

##Context in Flask
There are two context in flask

- **Application Context**
- **Request Context**

##Application Contexts

variable_name |  Description
------------- |-------------
current_app   | The application instance for the running application
get         | An object that can be used by the application for the temporary storage


##Request Context

variable_name | Description
------------  | ------------
request       | The request object which encapsulate the content carried by the http request
session       |  It is the dictionary that is used to remember the users session

Application context are only available when the flask application is running(pushes), at the time
of flask application running, the variables **current_app** and **g** application context are available

To demonstrate that the application context is only available in the app running, we can run the following commands

```python
(flasky)$ python
>>> from hello import app 
>>> from flask import current_app
>>> current_app.name 
RuntimeError: working outside of application context
```
To fix this error we need to run our application, so that we can have access to our application context, 
we can push (activate) our app from command line as well.

```python
>>>from hello import app
>>>from flask import current_app
>>> app_context = app.app_context()
>>>app_context.push()
>>>current_app.name
'hello'
>>>app_context.pop()
```
The important things to remember is, we can get the our application context by running the app.app_context() method on the 
application instance


##Request Dispatching
In the previous example we create our urls and its mapping to the methods using some decoraters and python functions.
To see the maps we can use the url_map method on app

```python
>>> from hello import app 
>>>app.url_map 
```
On running this above command, Flask display the following output in our console
```python
>>> app.url_map
Map([<Rule '/' (HEAD, OPTIONS, GET) -> index>,
 <Rule '/static/<filename>' (HEAD, OPTIONS, GET) -> static>,
 <Rule '/user/<name>' (HEAD, OPTIONS, GET) -> name>])
```
The **/** and **/user/<name>** are defined in our hello.py file, but **/static/<filename>** is the 
special route added by the flask to give access to the static route of our application.

The **HEAD OPTIONS** and **GET** are the request method that are handled by the route, while HEAD and OPTIONS methods are
automatically managed by the route.


##Request Hooks
Sometimes it is useful to execute the code before or after the each request is processed, and these code are implemented
as a function and these functions are called hooks.

- **before_first_request:**  Register a function to run before the first request get handled
- **before_request:** Register a function to run before each request
- **after_request** Register a function to run after each request if no unhandled exceptions occours
- **teardown_request** Register a function to run after a request even if unhandled exceptions occours

##Response
When flask invokes a view function through the request object, it expects a response object. when flask response,
by default it return the http response object wit status code, and the default status code will be the 200
if the request get replied successfully as **200** numeric code.

we can also override the behaviour by appending the numeric code in the return statement as follows in view methods.

```python
def index():
    return "<h2>Bad request</h2>", 400
```
we can also append the dictionary on the response as a third argument.
Instead of passing each value as a tuple in the return statement, It is benificial to use the response object provided
by the flask calld `make_response`, 

```python
from flask import make_response

@app.route('/')
def index():
    response = make_response("<h1>This document carries a cookie")
    response.set_cookie('answer', 42)
    return response
```

we have also another special type of response called redirect 
```python
@app.route('/')
def index():
    return redirect('http://google.com')
```
> Redirect wil be identified by the status code of 302 in http protocol.

In flask for error handling there a another special type of response called **abort**
```python
from flask import abort
@app.route(/user/<id>)
def get_user(id):
    user = load_user(id)
    if not user:
        abort(40r)
    return "<h1>%s is not a valid user</h1>" %user.name
```

##Flask Extensions
We can manage our flash application from the command line as well, to set up the configuration options to the 
web server we need to pass the options to app.run() instance, as flask is extendable, so, we will **flask-script**
to mange and set the configuration options to our aplication.

To install flask-script run the following command
```python
$pip install flask-script
```

To use the installed script we need to insert the following line of code in our **hello.py** application

```python
from flask.ext.script import Manager
manager = Manager(app)
```
so the final code of **hello.py** looks like this after the extension of flask-script extension

```python
from flask import Flask
from flask import request
from flask import redirect
from flask.ext.script import Manager

app = Flask(__name__)
manager = Manager(app)

@app.route('/')
def index():
    user_agent = request.headers.get('User-Agent')
    return "You are using %s browser" % user_agent

@app.route('/user/<name>')
def name(name):
    return "You said your name is %s" % name

@app.route('/google')
def google():   
    return redirect("http://google.com.np")

if __name__ == '__main__':
    manager.run()
```

so, with the flask-script extension, our **python hello.py** can provides the command line options
```python
$ python hello.py runserver
$ python hello.py shell
$ python hello.py --help
$ python hello.py runserver --helpm 
```

##App methods and attributes
from flask import Flask
from flask import make_response
from flask import redirect

response = make_response('<h1> Hello world </h1>')
response.creae_cookie('answer', 42)

from hello import app 
app.url_map 
context = app.app_context()
app.add_url_rule() # Non decoraters version for url mapping in flask

