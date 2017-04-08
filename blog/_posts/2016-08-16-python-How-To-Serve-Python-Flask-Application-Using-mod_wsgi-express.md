---
layout: post
title: Serve Python Flask application using mod_wsgi
categories:
- blog
tags:
- Python
- web application
- Flask
---

# How to serve Python Flask Application using mod_wsgi-express and Apache in Mac OS X El Capitan

If you have a python web application, you will need to deploy it to a physical server after the development is done. Python WSGI (web service gateway interface) is a protocal where separates the web framework (for example, Flask) with the web server (for example, Apache). The WSGI module provides an interconnect of sorts between Apache and your Python processes.

It is suggested to deploy Flask application by using Apache and mod_wsgi, on Flask [website](http://flask.pocoo.org/docs/0.11/deploying/mod_wsgi/ "Flask"). To acheieve this we will install the python modules and setup the environment.

### Prepare the environment 

- install virtualenv

  ```
  $ pip install virtualenv
  ```
  
  once installed, we should also configure the virtual environment
  
  ```
  $ mkdir MyFlaskProject
  $ cd MyFlaskProject
  $ virtualenv venv
  $ source ./bin/venv
  ```
  
  It is highly suggested to use virtualenv as this will isolate the workspace of your python runtime environment and create a separate code dependency of your python process. You have the option to deactivate this after using this environment. 
   
- install mod_wsgi

  ```
  pip install mod_wsgi
  ```  
  
- install Flask

  ```
  pip install Flask

All the modules are installed with its latest version. They will be installed to the virtual environment's lib folder if this hasn't been installed before.  

### Create a Flask app

We will demonstrate a simple "Hello Flask" example in Flask app. 

hello.py

```
import sys, os
sys.path.append(os.path.dirname(__file__))

from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, I love Flask!"
if __name__ == "__main__":
    app.run(host= '0.0.0.0')
```

### Create a .wsgi file 

```
import sys, os
sys.path.insert(0, "<root directory of your flask app>") # e.g. /var/www/FlaskApp/

from hello import app as application
```

### Run the service

If you run this command,

```
$ mod_wsgi-express start-server hello.wsgi
```

You should be able to see below message from your terminal:

```
Server URL         : http://localhost:8000/
Server Root        : /tmp/mod_wsgi-localhost:8000:501
Server Conf        : /tmp/mod_wsgi-localhost:8000:501/httpd.conf
Error Log File     : /tmp/mod_wsgi-localhost:8000:501/error_log (warn)
Request Capacity   : 5 (1 process * 5 threads)
Request Timeout    : 60 (seconds)
Queue Backlog      : 100 (connections)
Queue Timeout      : 45 (seconds)
Server Capacity    : 20 (event/worker), 20 (prefork)
Server Backlog     : 500 (connections)
Locale Setting     : en_CA.UTF-8
```

If no error log is showing, this means that the Flask service is running and you should be able to open localhost URL [localhost](http://localhost:8000/ "localhost") to see it display "Hello Flask" message.

The apache httpd.conf file is located at /etc/apache2/httpd.conf . It is suggested on the Flask website to modify httpd.conf file to configure Apache ([here](http://flask.pocoo.org/docs/0.11/deploying/mod_wsgi/)). However, if you check the httpd.conf file in /tmp folder, you will find mod_wsgi is smart enough to pick up the Flask application by checking the path in .wsgi file, as below:

```
...
...
WSGIDaemonProcess localhost:8000 \
...
   home='/var/www/FlaskApp/' \
...
...
```

so you don't have to modify the httpd.conf file.

In OS X EI Capitan, it is possible if you see an error message as 

install: /usr/libexec/apache2/mod_wsgi.so: Operation not permitted

when invoking mod_wsgi to start the server. This is due to the System Integrity Protection by Mac EI Capitan and you may follow this [site](https://github.com/GrahamDumpleton/mod_wsgi/issues/98) thread to disable it. 



