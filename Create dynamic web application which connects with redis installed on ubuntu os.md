# Create a dynamic web application which connects with Redis installed on ubuntu os

### Change the redis configuration to allow redis for another machine.

1. Modify "/etc/redis/redis.conf" as following

    Before                       After

    bind 127.0.0.1               bind 0.0.0.0
    protected-mode yes           protected-mode no


Why bind 0.0.0.0?

    # Protected mode is a layer of security protection, in order to avoid that
    # Redis instances left open on the internet are accessed and exploited.
    #
    # When protected mode is on and if:
    #
    # 1) The server is not binding explicitly to a set of addresses using the
    #    "bind" directive.
    # 2) No password is configured.
    #
    # The server only accepts connections from clients connecting from the
    # IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
    # sockets.
    #
    # By default protected mode is enabled. You should disable it only if
    # you are sure you want clients from other hosts to connect to Redis
    # even if no authentication is configured, nor a specific set of interfaces
    # are explicitly listed using the "bind" directive.
    protected-mode no

2. Restart the redis-server to apply the above configuration

    $ systemctl restart redis-server

3. Complete the step 1 to step 4 from this document: [static_greeting_application](/docs/Day1/static_greeting_application.md)

4. Install redis python module

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

5. Create the file dynamic_web_application.py

    from flask import Flask
    import redis

    redis_name = '<VM IP>'
    port = 6379
    r = redis.Redis(host=redis_name, port=port)
    app = Flask(__name__)

    @app.route('/')
    def index():
        r.incr("Count")
        value = r.get("Count")
        return "Total number of visitors for this page: " + str(value.decode()) + "\n"

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=9000)

6. Execute the dynamic_web_application.py

    (python_env)$python dynamic_web_application.py

    * Serving Flask app 'dynamic_web_application'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:9000
    * Running on http://172.22.15.200:9000
    Press CTRL+C to quit


7. Testing: Open another terminal in ubuntu
 
    $ curl localhost:9000

    Total number of visitors for this page: 1

    $ curl localhost:9000

    Total number of visitors for this page: 2
