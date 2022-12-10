# Create dynamic web application which connects with redis container

### Create redis container (outside virtual environment)

``` $ docker run -p 9379:6379 --name redis-database -d redis```
    
    96437cb96f7692f590ded3f53c7d161ceaad8b18c7d7bb2d1701c5d2a14c06a8

### Find the ip address of the container

```$ docker inspect redis-database```

    ****
    "IPAddress": "172.17.0.3"
    ****

### Complete the step 1 to step 4 from this document: [here](https://github.com/ShrddhaRana/DockerBasics/blob/main/Create%20a%20simple%20greeting%20web%20application%20using%20flask.md))

### Install redis python module
    (python_env)$pip install redis

    Collecting redis
    Downloading redis-4.3.5-py3-none-any.whl (248 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 248.7/248.7 kB 2.5 MB/s eta 0:00:00
    Collecting async-timeout>=4.0.2
    Downloading async_timeout-4.0.2-py3-none-any.whl (5.8 kB)
    Collecting packaging>=20.4
    Downloading packaging-21.3-py3-none-any.whl (40 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 40.8/40.8 kB 2.3 MB/s eta 0:00:00
    Collecting pyparsing!=3.0.5,>=2.0.2
    Downloading pyparsing-3.0.9-py3-none-any.whl (98 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 98.3/98.3 kB 3.1 MB/s eta 0:00:00
    Installing collected packages: pyparsing, async-timeout, packaging, redis
    Successfully installed async-timeout-4.0.2 packaging-21.3 pyparsing-3.0.9 redis-4.3.5


### Connect the web application to the container using ip address of the container.

1. Create or modify the file dynamic_web_application.py
Note: We have updated the IP address to that of container
    
    from flask import Flask
    import redis

    redis_name = '172.17.0.3' # Ip address of the container
    port = 6379 # redis container port
    r = redis.Redis(host=redis_name, port=port)
    app = Flask(__name__)

    @app.route('/')
    def index():
        r.incr("Count")
        value = r.get("Count")
        return "Total number of visitors for this page: " + str(value.decode()) + "\n"

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=9000)

2. Execute the dynamic_web_application.py

    (python_env)$python dynamic_web_application.py

    * Serving Flask app 'dynamic_web_application'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:9000
    * Running on http://172.22.15.200:9000
    Press CTRL+C to quit


3. Testing: Open another terminal in ubuntu

    ubuntu$ curl localhost:9000

    Total number of visitors for this page: 1

    ubuntu$ curl localhost:9000

    Total number of visitors for this page: 2


### Connect the web application to the container using localhost of the machine.

1. Create or modify the file dynamic_web_application.py
Note: We have updated the port

    from flask import Flask
    import redis

    redis_name = 'localhost' # Ip address of the container
    port = 9379 # redis container port mapped to localhost
    r = redis.Redis(host=redis_name, port=port)
    app = Flask(__name__)

    @app.route('/')
    def index():
        r.incr("Count")
        value = r.get("Count")
        return "Total number of visitors for this page: " + str(value.decode()) + "\n"

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=9000)

2. Execute the dynamic_web_application.py
 
    (python_env)$python dyanmic_web_application.py

    * Serving Flask app 'dyanmic_web_application'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:9000
    * Running on http://172.22.15.200:9000
    Press CTRL+C to quit

    
3. Testing: Open another terminal in ubuntu

    ubuntu$ curl localhost:9000

    Total number of visitors for this page: 3

Since same redis databse is used, count will start from 3.

    ubuntu$ curl localhost:9000

    Total number of visitors for this page: 4
