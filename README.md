# 🚀 Deploying a Streamlit App on AWS EC2 with Docker

This guide walks you through setting up an AWS EC2 instance, transferring files, installing Docker, and running a Streamlit app inside a Docker container.

---
## 🌍 1. Setting Up the EC2 Instance

1. **Launch an EC2 Instance:**
   - Select **Amazon Linux 2** as the AMI.
   - Choose an appropriate instance type (e.g., `t2.micro` for free-tier users).
   - Configure security group:
     - **Allow SSH** (Port 22) for remote access.
     - **Allow Custom TCP Rule** (Port 8501) for Streamlit.
   
2. **Create and Download the Key Pair:**
   - When launching the instance, create a new key pair (`.pem` file) and download it.
   - **Store it safely**—you’ll need it to connect!

---
## 🔄 2. Connecting to EC2 and Setting Up Docker

### 💻 Connect to the EC2 Instance
Run the following in **VS Code Git Bash** or **Terminal**:
```sh
chmod 600 vs-kp-1.pem  # Set correct permissions for security
ssh -i vs-kp-1.pem ec2-user@<YOUR_EC2_PUBLIC_IP>
```

### 📦 Install and Start Docker
Once inside the EC2 instance, update packages and install Docker:
```sh
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
```

---
## 📂 3. Transferring Files to EC2
Move your project files (Dockerfile, app.py, requirements.txt, mushrooms.csv) to the EC2 instance:
```sh
scp -i vs-kp-1.pem Dockerfile app.py requirements.txt mushrooms.csv ec2-user@<YOUR_EC2_PUBLIC_IP>:/home/ec2-user/downloads
```

---
## 🛠️ 4. Building and Running the Docker Container
Once the files are on EC2, navigate to the folder and build your Docker image:
```sh
cd /home/ec2-user/downloads
sudo docker build -t my_app:v1.0 -f Dockerfile .
```
Run the container and expose it on port 8501:
```sh
sudo docker run -d -p 8501:8501 my_app:v1.0
```

---
## 🌐 5. Accessing Your Streamlit App
Once the container is running, open your browser and visit:
```
http://<YOUR_EC2_PUBLIC_IP>:8501
```
You should now see your Streamlit app live! 🎉

---
## ⚠️ 6. Important: Deleting the EC2 Instance (To Avoid Costs!)

After you're done, **terminate the EC2 instance** to prevent unwanted charges:
1. Go to the AWS **EC2 Dashboard**.
2. Find your instance, select it, and click **Terminate**.
3. Confirm the termination.

---
## 🎯 Summary of Commands
| Task                        | Command |
|-----------------------------|---------|
| Connect to EC2              | `ssh -i vs-kp-1.pem ec2-user@<YOUR_EC2_PUBLIC_IP>` |
| Transfer files to EC2       | `scp -i vs-kp-1.pem Dockerfile app.py requirements.txt mushrooms.csv ec2-user@<YOUR_EC2_PUBLIC_IP>:/home/ec2-user/downloads` |
| Install Docker on EC2       | `sudo yum update -y` <br> `sudo amazon-linux-extras install docker` <br> `sudo service docker start` |
| Build Docker Image          | `sudo docker build -t my_app:v1.0 -f Dockerfile .` |
| Run the Docker Container    | `sudo docker run -d -p 8501:8501 my_app:v1.0` |
| Access the App              | `http://<YOUR_EC2_PUBLIC_IP>:8501` |
| Terminate EC2 Instance      | **AWS Console → Select Instance → Terminate** |

---
### Screenshots 
![image](https://github.com/user-attachments/assets/d93472b9-1b0b-4ff7-984b-a93c285536e7)
![image](https://github.com/user-attachments/assets/fbbd3853-21e2-4c4a-9a53-76b3c0c0b39b)


