# 🧪 Test Your Understanding – Interactive Notebook App

This repository hosts an interactive quiz notebook built with `ipywidgets` and rendered using `Voilà`. It is containerized with Docker and deployable via Binder for seamless sharing and learning.

## 🚀 Quick Launch via Binder

[![Launch on Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/<your-username>/<your-repo>/main?urlpath=voila/render/voila/render/Test-Your_Understanding-1.ipynb)

> 🔗 Replace `<your-username>` and `<your-repo>` with your GitHub details if you fork.

## 📦 Project Structure

```
.
├── voila/
│   └── render/
│       └── Test-Your_Understanding-1.ipynb
├── Dockerfile
├── requirements.txt
└── README.md
```

## 🛠️ Local Development

### 1. Run with Docker
```bash
docker build -t quiz-app .
docker run -p 8888:8888 quiz-app
```
Visit: [http://localhost:8888](http://localhost:8888)

### 2. Run Locally (Without Docker)
```bash
pip install voila ipywidgets
voila voila/render/Test-Your_Understanding-1.ipynb
```

## 📚 Requirements

```
voila
ipywidgets
```

## 📖 About

This project serves as a minimal, reproducible example for deploying educational content using interactive widgets and sharing it via Binder or Docker.
