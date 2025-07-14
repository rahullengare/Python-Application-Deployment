# Python-Application-Deployment-
## **Python Introduction:**

Python is a free and open-source, high-level programming language known for its simplicity and readability. It’s widely used for web development, automation, data analysis, machine learning, and backend services.

Python is popular because it has a clean syntax, a huge standard library, and a large ecosystem of third-party packages. It runs on all major platforms and is ideal for both beginners and experienced developers.

## About Project:

This project demonstrates how to deploy a basic **Python application** on an **AWS EC2 instance**. The main goal is to take a simple Python web app and host it on a cloud server so it can be accessed through a public IP.

The process includes setting up an EC2 instance, installing Python and required packages, running the app, managing it in the background using **Gunicorn**, and using **Nginx** as a reverse proxy to serve the application over the standard HTTP port (80). This provides a practical introduction to deploying backend applications in the cloud.

## Technologies Used:

- **Python** – Programming language used to build the application
- **pip** – Python package manager to install libraries and dependencies
- **Gunicorn** – To keep the app running in the background
- **Nginx** – Web server used to forward HTTP requests to the Python app
- **AWS EC2 (Amazon Elastic Compute Cloud)** – Virtual server where the app is hosted
- **Amazon Linux** – The operating system used on the EC2 instance

## Step 1: Launch an EC2 Instance

1. Go to AWS Console → EC2 
2. Launch Instance.
    - Name → python
    - Choose AMI → Amazon Linux.
    - Instance type → t2.micro
    - Key pair → pem_server_key
    - security group → launch-wizard-1 (port:5000)
  
![Project Screenshot](/images/instance.jpg)

## Step 2: SSH to connect the EC2 Instance

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-18-206-171-240.compute-1.amazonaws.com
```
![Project Screenshot](/images/connect-instance.jpg)

## Step 3: Installing node app

1. update your system

```bash
sudo yum update
```

2. install python in server 

```bash
sudo yum install python3 -y
```

3. install package manager

```bash
sudo yum install python3-pip -y
```

4. checking python app & pip is installed version

```bash
python3 --version
pip --version
```

5. make a **directory** python and go inside

```bash
mkdir pythonapp
cd pythonapp/
```

6. create Virtual **Environment** for python

```bash
python3 -m venv myenv
```

7. Activate **Environment** 

```bash
source myenv/bin/activate
```
![Project Screenshot](/images/myenv.jpg)

8. create requirements.txt with code 

```bash
sudo vim requirements.txt
```

```bash
click==8.0.3
colorama==0.4.4
Flask==2.0.2
itsdangerous==2.0.1
Jinja2==3.0.3
MarkupSafe==2.0.1
Werkzeug==2.0.2
gunicorn==20.1.0
```

9. create app.py with code 

```bash
sudo vim app.py
```

```bash
from flask import Flask
import os
app = Flask(__name__)

@app.route('/')
def hello_geek():
    return 'Now python app is hosting'
@app.route('/hi')
def hell():
    return '<h1>Hiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii from Flask & Docker</h1>'

if __name__ == "__main__":
    port = int(os.environ.get('PORT', 5000))
    app.run(debug=True, host='0.0.0.0', port=port)
```

10. install packages in pythonapp **directory**

```bash
sudo pip install -r requirements.txt
```

11. Run Python3 app

```bash
sudo python3 app.py
```

12. Now python app is Running in Port 5000

```bash
<enter-your-public-ip>:5000
```
![Project Screenshot](/images/port-5000-run.jpg)

13. Terminal not present that why not any changes is possible → background pythonapp run

```bash
sudo gunicorn --bind 0.0.0.0:5000 app:app --daemon
```

14. Now port 5000 is not secure to always run using 5000 any time now create proxy_pass that to install nginx 

```bash
sudo yum install nginx
```

15. start the nginx & enable 

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

16. now edit configuration file that to go 

```bash
cd /etc/nginx/
sudo chmod +x nginx.conf
sudo vim nginx.conf
```

17. add location block nginx.conf

```bash
location /{
proxy_pass http://localhost:5000;
}
```

18. restart nginx server 

```bash
sudo systemctl restart nginx
```

19. now checking your node app 

```bash
<enter-your-public-ip-only-not-port>
```
![Project Screenshot](/images/without-port-run.jpg)

20. Now Your **Python app** has successfully deployed on your EC2 instance
    
## Step 4: Terminating Your instance

1. Your use are done then got to AWS console 
2. Click on EC2 → instance 
3. Select instance You want to terminated
4. Click on Instance state 
5. Choose **Terminate (delete) instance**
6. Now click delete

![Project Screenshot](/images/delete-instance.jpg)
