# Tools for Coding

## MySQL SERIES

MySQL is a popular open-source relational database management system. Below is a video link for installing MySQL:


- **Installation Video**: [Installing MySQL](https://youtu.be/wgRwITQHszU)
- **Reset MySQL root password**: [Reset MYSQL password](https://youtu.be/wgRwITQHszU)

## Ubuntu

MySQL is a popular open-source relational database management system. Below is a video link for installing MySQL:


- **Create a background service**: [Understanding daemons in Unix systems](https://youtu.be/S5TfD50s4Lo)

## AWS 

Amazon Web Services or AWS, commonly known in the tech industry, is the world’s most reliable cloud computing platform. It provides hundreds of unique services which are all cloud-based, ranging from databases to virtual servers.

- **Identity Access And Management**: [Setting up IAM](https://youtu.be/bO25vbkoJlA)
- **PEM key unprotected error**: [SSH instance using .pem key](https://youtu.be/C36fKJsXV3U)



## AWS S3 storage

Object-based cloud storage service to store and retrieve data from anywhere on the web

- **Setup Video**: [Setting up S3](https://youtu.be/Ko52pn1KXS0)
- **Setup Video**: [Uploading images and static files in s3 bucket](https://youtu.be/JQVQcNN0cXE)
- **Setup Video**: [Setting up S3 and postgesql](https://youtu.be/LaoYcQsPyD8)


## Render Platform

Object-based cloud storage service to store and retrieve data from anywhere on the web

- **Setup Video**: [Setting up render for django](https://youtu.be/wczWm8j4v9w)

## Django

Python framework for server-side application

- **Django lifecycle**: [Gunicorn WSGI](https://youtu.be/Ko52pn1KXS0)
- **Django environment variables**: [python dotenv](https://youtu.be/7tRLkZO6D3Y)


## Setting Up and Using Python on Ubuntu

Most Ubuntu distributions come with Python 3 pre-installed. To check your Python version, open your terminal and run:

```bash
python3 --version
```

First, update your package list, then install Python 3 if Python is not installed:

```bash
sudo apt update
sudo apt install python3
```

Python on Windows comes with pip pre-installed, but on Ubuntu, you may need to install it separately

```bash
sudo apt install python3-pip
```

Virtual environments in Ubuntu may need manual setup. To install the Python venv package:

```bash
sudo apt install python3-venv
```

To create a new virtual environment named myenv:

```bash
python3 -m venv myenv
```

To activate the virtual environment on Ubuntu:

```bash
source myenv/bin/activate
```

To deactivate the virtual environment:

```bash
deactivate
```
Managing and installing  versions 

```bash
  sudo add-apt-repository ppa:deadsnakes/ppa
  133  sudo apt update
  135  sudo apt install python3.12 -y
  136  clear
  137  sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1
  138  sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 2
  139  sudo update-alternatives --config python3
```


# Creating systemd Socket and Service Files for Gunicorn

## Step 1: Create systemd Socket File

```bash
sudo nano /etc/systemd/system/shopy.socket
```
Paste the code below

``` bash
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/shopy.sock

[Install]
WantedBy=sockets.target
```
## Step 2: Create systemd Service File

```bash
sudo nano /etc/systemd/system/shopy.service
```
Paste this code below

```bash
[Unit]
Description=gunicorn daemon
Requires=shopy.socket
After=network.target
[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/Style_Hub
ExecStart=/home/ubuntu/Style_Hub/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/shopy.sock \
          style_hub.wsgi:application
[Install]
WantedBy=multi-user.target
```
## Step 3: Start and Enable Gunicorn Socket

``` bash
sudo systemctl start shopy.socket
sudo systemctl enable shopy.socket
sudo systemctl status shopy.socket
```
Check socket Status

```bash
sudo systemctl status shopy.socket
```
you should see this output

```bash
● shopy.socket - gunicorn socket
     Loaded: loaded (/etc/systemd/system/shopy.socket; enabled; vendor preset: enabled)
     Active: active (listening) since Sat 2024-05-04 18:30:12 UTC; 22min ago
   Triggers: ● shopy.service
     Listen: /run/shopy.sock (Stream)
     CGroup: /system.slice/shopy.socket

May 04 18:30:12 ip-172-31-1-254 systemd[1]: Listening on gunicorn socket
Apr 18 17:53:25 django systemd[1]: Listening on gunicorn socket.
```
Reload deamon services as shown below

```bash
sudo systemctl daemon-reload
sudo systemctl restart gunicorn
```
## Step 4: Configure Nginx to Proxy Pass to Gunicorn

```bash
sudo nano /etc/nginx/sites-available/shopy
```

```bash
server {
    listen *:80;
    server_name 54.206.41.225;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
       alias /home/ubuntu/Shopy/static/;
    }

    location /{
        include proxy_params;
        proxy_pass http://unix:/run/shopy.sock;

    }
}

```
## Step 5: Link the sites-available to sites-enabled

```bash
sudo ln -s /etc/nginx/sites-available/shopy /etc/nginx/sites-enabled
```


# Sockets as Communication Endpoints

Sockets are endpoints for sending and receiving data across a network. They can be used for inter-process communication (IPC) on the same host or across different hosts over a network.

## Types of Sockets

### Unix Sockets (also known as Unix Domain Sockets)
These are used for IPC on the same host. They use file paths in the file system as their address namespace.

### TCP Sockets
These are used for communication over a network using the TCP/IP protocol suite. They use IP addresses and port numbers as their address namespace.

## Communication Between Processes on the Same Host
When two processes on the same host need to communicate, they can use Unix sockets. This method is efficient because it avoids the overhead of network protocols.

## Socket File
A Unix socket is represented by a special file in the file system. This file is created when the socket is initialized and is used by processes to connect to the socket.

## Initialization and Boot-Up
A socket file can be created and initialized at boot-up by a service manager (like systemd on Linux). This socket file acts as a listening endpoint for connections.

## Service Bound to the Socket
A service can be bound to a socket, meaning it will start and listen for incoming connections on this socket. When another process wants to communicate, it connects to this socket file.

## Example Workflow

- **Process A**: Creates a Unix socket file (e.g., `/tmp/socket_file`) and starts listening for connections.
- **Process B**: Knows the path to the socket file and connects to it to communicate with Process A.
- **Service Execution**: When a connection is made to the socket file, the bound service (Process A) handles the communication, executing its logic based on the data received.

# Nginx and Gunicorn as Separate Processes

## Nginx
Acts as a reverse proxy server. It handles incoming HTTP requests from clients.

## Gunicorn
Is a WSGI server that runs your web application (e.g., a Django or Flask app) and handles the application logic.

## Communication Through a Unix Socket
Nginx and Gunicorn communicate via a Unix socket, which is a file-based communication endpoint.

## Unix Socket File
The Unix socket file (e.g., `/run/shopy.sock`) is created and listened to by Gunicorn. This file acts as the address for the socket.

Gunicorn is bound to this socket file, meaning it listens for connections on this socket.

## Nginx Configuration
In the Nginx configuration, the `proxy_pass http://unix:/run/shopy.sock;` directive tells Nginx to forward incoming HTTP requests to the Unix socket file where Gunicorn is listening.

## Startup and Initialization
Typically, the Unix socket file is created and Gunicorn starts listening on it when the system boots up or when the service is started.

## Request Handling Workflow

- **Client Request**: A client sends an HTTP request to Nginx.
- **Nginx Receives Request**: Nginx processes the request and, based on its configuration, forwards it to the Unix socket file specified in the `proxy_pass` directive.
- **Gunicorn Receives Request**: Gunicorn, which is bound to the Unix socket, receives the request, processes it using the web application, and sends back a response.
- **Nginx Forwards Response**: Nginx receives the response from Gunicorn through the Unix socket and forwards it back to the client.



