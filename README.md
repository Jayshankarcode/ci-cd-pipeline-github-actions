# CI/CD Pipeline for Java Application (GitHub Actions + Docker)

This project demonstrates a complete CI/CD pipeline built using **GitHub Actions** for a Java application.  
The pipeline automates the following stages:

- Code checkout  
- Build using Gradle  
- Run unit tests  
- Build Docker image  
- Push image to Docker Hub  
- Deploy to staging environment (optional)

---

## 🚀 Features
- Automated CI pipeline (build + test)
- Docker image build and push using GitHub Actions
- Branch-based workflows (dev / main)
- Reusable GitHub Actions workflow file
- Supports Java + Gradle based applications

---

## 🧰 Tech Stack
- GitHub Actions  
- Docker  
- Java (OpenJDK)  
- Gradle  
- Linux / Bash  
- Git  

---

📁 Repository Structure
.
├── .github/workflows/
│ └── ci-cd.yml
├── src/
│ └── main/java/...
├── build.gradle
├── Dockerfile
└── README.md


---  
yaml: 

## ⚙️ Run Locally

### 1️⃣ Clone the repository  
```bash
git clone https://github.com/Jayshankarcode/ci-cd-pipeline-github-actions.git
cd ci-cd-pipeline-github-actions

Build the project
./gradlew build

Build Docker image
docker build -t your-dockerhub-username/app-name .
Run the Docker container
docker run -p 8080:8080 your-dockerhub-username/app-name

CI/CD Workflow File (Example)
Create file:
.github/workflows/ci-cd.yml

name: CI/CD Pipeline

on:
  push:
    branches: [ "main", "dev" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Setup JDK
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'

    - name: Build with Gradle
      run: ./gradlew build --no-daemon

    - name: Run tests
      run: ./gradlew test

  docker:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        push: true
        tags: ${{ secrets.DOCKERHUB_USERNAME }}/devops-demo-app:latest

