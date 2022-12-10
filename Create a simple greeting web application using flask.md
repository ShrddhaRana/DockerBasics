# Creating a static web application in the Ubuntu VM

#### Check the python3 version
```
$ python3 --version
Python 3.8.10
```

#### Install pip
```
$ sudo apt install python3-pip -y
```

#### Creating with python virtual environment

1. Install the `virtualenv` module
   ```
   $ pip install virtualenv -U

    Collecting virtualenv
    Downloading virtualenv-20.16.7-py3-none-any.whl (8.8 MB)
        |████████████████████████████████| 8.8 MB 4.1 MB/s
    Collecting platformdirs<3,>=2.4
    Downloading platformdirs-2.5.4-py3-none-any.whl (14 kB)
    Collecting filelock<4,>=3.4.1
    Downloading filelock-3.8.0-py3-none-any.whl (10 kB)
    Collecting distlib<1,>=0.3.6
    Downloading distlib-0.3.6-py2.py3-none-any.whl (468 kB)
        |████████████████████████████████| 468 kB 5.4 MB/s
    Installing collected packages: platformdirs, filelock, distlib, virtualenv
    WARNING: The script virtualenv is installed in '/home/sgrandhi-local-docker/.local/bin' which is not on PATH.
    Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
    Successfully installed distlib-0.3.6 filelock-3.8.0 platformdirs-2.5.4 virtualenv-20.16.7
   ```

2. Create the virtual environment
    ```
    $ python3 -m virtualenv python_venv

    created virtual environment CPython3.8.10.final.0-64 in 203ms
    creator CPython3Posix(dest=/home/sgrandhi-local-docker/python_venv, clear=False, no_vcs_ignore=False, global=False)
    seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/sgrandhi-local-docker/.local/share/virtualenv)
        added seed packages: pip==22.3.1, setuptools==65.5.1, wheel==0.38.4
    activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator    
    ```

3. Activate the python virtual environment
   ```
   $ source python_venv/bin/activate
   ```

4. Verify python path
   ```
   $ which python

   /home/xyz-local-docker/python_venv/bin/python
   ```
   Python will be used from the virtual environment

5. Running 'Hello world' python example
   
   Create a file "helloworld.py" with following contents.
   ```
   print("Hello World")
   ```
    Run your python file
   ```
   $python helloworld.py
   Hello World
   ```

## Run the static web application using python virtual env

1. Create the static web application directory
   ```
   $ mkdir static_web_application
   ```
2. Create static_web_application.py file
   ```
   $cd static_web_application
   ```
   create the static_web_application.py file
    ```
    from flask import Flask
    name = "My Name"
    port = 8000

    app = Flask(__name__)

    @app.route('/')
    def index():
        return f"Hello {name}"

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=port)
    ```
3. pip `flask' python module.
    ```
    $ pip install flask

    Collecting flask
    Downloading Flask-2.2.2-py3-none-any.whl (101 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 101.5/101.5 kB 1.9 MB/s eta 0:00:00
    Collecting Jinja2>=3.0
    Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.1/133.1 kB 4.1 MB/s eta 0:00:00
    Collecting Werkzeug>=2.2.2
    Downloading Werkzeug-2.2.2-py3-none-any.whl (232 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 232.7/232.7 kB 4.3 MB/s eta 0:00:00
    Collecting importlib-metadata>=3.6.0
    Downloading importlib_metadata-5.0.0-py3-none-any.whl (21 kB)
    Collecting click>=8.0
    Downloading click-8.1.3-py3-none-any.whl (96 kB)
        ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 96.6/96.6 kB 5.1 MB/s eta 0:00:00
    Collecting itsdangerous>=2.0
    Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
    Collecting zipp>=0.5
    Downloading zipp-3.10.0-py3-none-any.whl (6.2 kB)
    Collecting MarkupSafe>=2.0
    Downloading MarkupSafe-2.1.1-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
    Installing collected packages: zipp, MarkupSafe, itsdangerous, click, Werkzeug, Jinja2, importlib-metadata, flask
    Successfully installed Jinja2-3.1.2 MarkupSafe-2.1.1 Werkzeug-2.2.2 click-8.1.3 flask-2.2.2 importlib-metadata-5.0.0 itsdangerous-2.1.2 zipp-3.10.0
    ```
4. Run the static web application
    ```
    $ python static_web_application.py
    * Serving Flask app 'static_web_application'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:8000
    * Running on http://172.21.152.9:8000
    Press CTRL+C to quit
    ```
5. Verify static_web_application
   - From the host machine(Windows)
     visit http:{ubuntu_vm_ip}:{port}
     e.g `http://172.21.152.9:8000/` will show below o/p
     ```
     My Name
     ```
   - From the Ubuntu machine
     * Connect ubuntu machine with another ssh session
     * ```
       $ sudo apt install curl
       $ curl http://127.0.0.1:8000
       My Name
       $ curl http://172.21.152.9:8000/
       My Name
       ```
