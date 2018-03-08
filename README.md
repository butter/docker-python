# Supported tags and respective `Dockerfile` links

-	[`2.7.14`, `2.7`, `2` (*2.7/Dockerfile*)](https://github.com/butter/docker-python/blob/71de0eb8dbc21b1ba4e1001b7a86d6121e20dd05/2.7/Dockerfile)
-	[`2.7.13` (*2.7/Dockerfile*)](https://github.com/butter/docker-python/blob/6f9909b0c74c852c456c586c120c3b4bd7ede772/2.7/Dockerfile)
-	[`3.6.4`, `3.6`, `3`, `latest` (*3.6/Dockerfile*)](https://github.com/butter/docker-python/blob/71de0eb8dbc21b1ba4e1001b7a86d6121e20dd05/3.6/Dockerfile)
-	[`3.6.2` (*3.6/Dockerfile*)](https://github.com/butter/docker-python/blob/6f9909b0c74c852c456c586c120c3b4bd7ede772/3.6/Dockerfile)
-	[`3.6.1` (*3.6/Dockerfile*)](https://github.com/butter/docker-python/blob/55a8c8a121f8fa8814ab42586b320758f2193941/3.6/Dockerfile)
-	[`3.6.0` (*3.6/Dockerfile*)](https://github.com/butter/docker-python/blob/ef2f8127358369ffc06c62a851d776188d084b78/3.6/Dockerfile)

# What is Python?

Python is an interpreted, interactive, object-oriented, open-source programming language. It incorporates modules, exceptions, dynamic typing, very high level dynamic data types, and classes. Python combines remarkable power with very clear syntax. It has interfaces to many system calls and libraries, as well as to various window systems, and is extensible in C or C++. It is also usable as an extension language for applications that need a programmable interface. Finally, Python is portable: it runs on many Unix variants, on the Mac, and on Windows 2000 and later.

> [wikipedia.org/wiki/Python_(programming_language)](https://en.wikipedia.org/wiki/Python_%28programming_language%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/01c12653951b2fe592c1f93a13b4e289ada0e3a1/python/logo.png)

# How to use this image

## Create a `Dockerfile` in your Python app project

```dockerfile
FROM butter/python:3.6.2
CMD [ "python", "./your-daemon-or-script.py" ]
```

or (if you need to use Python 2):

```dockerfile
FROM butter/python:2.7.13
CMD [ "python", "./your-daemon-or-script.py" ]
```

## Run a single Python script

For many simple, single file projects, you may find it inconvenient to write a complete `Dockerfile`. In such cases, you can run a Python script by using the Python Docker image directly:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp butter/python:3 python your-daemon-or-script.py
```

or (again, if you need to use Python 2):

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp butter/python:2 python your-daemon-or-script.py
```

# License

View license information for [Python 2](https://docs.python.org/2/license.html) and [Python 3](https://docs.python.org/3/license.html).
