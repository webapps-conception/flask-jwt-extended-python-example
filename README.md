# flask-jwt-extended-python-example
[Flask-JWT-Extended’s Documentation](https://flask-jwt-extended.readthedocs.io/en/stable/)

## Installation
``` bash
pip install flask-restful
```

## Example usage
``` bash
$ python api.py
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
```

## Protected road access test
``` bash
$ curl http://localhost:5000/protected -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /protected HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> 
< HTTP/1.1 401 UNAUTHORIZED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:39:15 GMT
< Content-Type: application/json
< Content-Length: 39
< Connection: close
< 
{"msg":"Missing Authorization Header"}
* Closing connection 0
```

To access a jwt_required protected view you need to send in the JWT with each request.

``` bash
$ curl http://localhost:5000/login -d '{ "username":"test", "password":"test" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /login HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 40
> 
* upload completely sent off: 40 out of 40 bytes
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:40:18 GMT
< Content-Type: application/json
< Content-Length: 349
< Connection: close
< 
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0"}
* Closing connection 0
```

By default, this is done with an authorization header that looks like: `Authorization: Bearer <access_token>`

``` bash
$ export JWT="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0"

$ curl http://localhost:5000/protected -H "Authorization: Bearer $JWT" -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /protected HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0
> Content-Type: application/json
> 
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:47:25 GMT
< Content-Type: application/json
< Content-Length: 24
< Connection: close
< 
{"logged_in_as":"test"}
* Closing connection 0
```

