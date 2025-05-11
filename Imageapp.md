# Image Uploader App with Flask + Docker (on EC2)

# 🖼️ Flask Image Uploader (Dockerized)

A simple image upload and preview web app built using **Flask** and deployed inside a **Docker container** on an **AWS EC2 instance**. Users can upload images which are saved and displayed using HTML templates.

---

## 🚀 Features

- Upload images via a browser form
- Securely save uploaded files
- View uploaded image instantly
- Dockerized for consistent deployment
- Persistent image storage using EC2 volume

---

## 🧱 Project Structure

```

image-uploader-app/
├── app.py
├── requirements.txt
├── Dockerfile
├── uploads/              # (Used in Docker container)
└── templates/
├── index.html
└── result.html

````

---

## ⚙️ Setup Instructions (EC2 + Docker)

### 1️⃣ Create Project Folder

```bash
mkdir image-uploader-app
cd image-uploader-app
mkdir templates uploads
````

### 2️⃣ Add Project Files

* `app.py` — \[Flask backend logic]
* `templates/index.html` — \[Upload form]
* `templates/result.html` — \[Image display]
* `requirements.txt` — Add:

  ```
  Flask
  ```
* `Dockerfile` — Add:

  ```Dockerfile
  FROM python:3.10

  WORKDIR /app

  COPY . .

  RUN pip install -r requirements.txt

  RUN mkdir -p /app/uploads

  CMD ["python", "app.py"]
  ```

---

## 🐳 Docker Build & Run (With Persistent Volume)

### 3️⃣ Build the Docker Image

```bash
docker build -t image-uploader-app .
```

### 4️⃣ Create Persistent Uploads Folder on EC2

```bash
mkdir ~/image-uploads
```

### 5️⃣ Run the Container

```bash
docker run -d \
  -p 5000:5000 \
  --name uploader-cont \
  -v ~/image-uploads:/app/uploads \
  image-uploader-app
```

---

## 🌐 Access the App

1. Make sure **port 5000** is open in your EC2 Security Group
2. Visit in your browser:

   ```
   http://<your-ec2-public-ip>:5000
   ```

---

## 📁 Uploaded Image Storage

* All uploaded images are saved to:

  ```
  ~/image-uploads
  ```
* These persist even if the container is stopped or rebuilt

---

## 📌 Notes

* To stop container:

  ```bash
  docker stop uploader-cont
  ```

* To delete container:

  ```bash
  docker rm uploader-cont
  ```

* To rebuild after changes:

  ```bash
  docker build -t image-uploader-app .
  ```

---

## 🧑‍💻 Author

**Aditya**
Built and deployed using Flask + Docker on AWS EC2 🚀

```


