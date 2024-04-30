# Tools for Coding

## MySQL

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




