
# 🌐 Flask + Nginx Reverse Proxy using Docker (Deployed on EC2)

This project demonstrates a real-world Docker setup:
- A **Flask app** running in one container
- **Nginx** acting as a reverse proxy in another container
- Managed together using **Docker Compose**
- Deployed on an **AWS EC2 instance**

---

## 🧱 Folder Structure

```
flask-nginx-docker/
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── nginx/
│   └── default.conf
└── docker-compose.yml
```

---

## 🚀 Setup Instructions

### 🔹 Step 1: Create the Directory Structure

```bash
mkdir flask-nginx-docker
cd flask-nginx-docker

mkdir app nginx
touch app/app.py app/requirements.txt app/Dockerfile nginx/default.conf docker-compose.yml
```

---

### 🔹 Step 2: Flask App (`app/app.py`)

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "<h1>Hello from Flask behind Nginx Reverse Proxy!</h1>"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

### 🔹 Step 3: Requirements (`app/requirements.txt`)

```
Flask
```

---

### 🔹 Step 4: Dockerfile for Flask (`app/Dockerfile`)

```Dockerfile
FROM python:3.10

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

---

### 🔹 Step 5: Nginx Config (`nginx/default.conf`)

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://flask:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

### 🔹 Step 6: Docker Compose (`docker-compose.yml`)

```yaml
version: '3'

services:
  flask:
    build: ./app
    container_name: flask-app
    expose:
      - "5000"

  nginx:
    image: nginx:latest
    container_name: nginx-proxy
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - flask
```

---

## 🧪 Build and Run

```bash
docker compose build
docker compose up -d
```

---

## 🌐 Access the App

1. Make sure **port 80** is allowed in your EC2 security group.
2. Open your browser and visit:

```
http://<your-ec2-public-ip>
```

---

## 🛑 Useful Commands

- Stop containers:
  ```bash
  docker compose down
  ```

- View logs:
  ```bash
  docker compose logs -f
  ```

- Rebuild after changes:
  ```bash
  docker compose build --no-cache
  docker compose up -d
  ```

---

## 🧑‍💻 Author

**Aditya**  
Deployed with Flask + Nginx + Docker on AWS EC2 🚀
