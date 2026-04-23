# 🚀 Azurite Local Setup Guide (Docker + ZIP Package)

This guide will walk you through setting up **Azurite (Azure Storage Emulator)** locally using **Docker** and the provided ZIP package.

---

## 📦 Prerequisites

Before starting, make sure you have the following installed:

### 1. Install Docker Desktop

Download and install Docker:

👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

**Steps:**

1. Download Docker Desktop for Windows
2. Run the installer
3. Enable:

   * ✅ WSL 2 (recommended)
4. Restart your machine if required
5. Verify installation:

```bash
docker --version
docker compose version
```

---

## 📁 Project Setup

### 2. Extract the ZIP File

Extract the provided ZIP file to your preferred directory:

```bash
E:\AzureBlob
```

After extraction, your folder should look like:

```
AzureBlob/
│
├── docker-compose.yml
├── run.bat
└── (other files if included)
```

---

## ⚙️ Configuration (Optional)

Make sure your `docker-compose.yml` is configured properly.

### Sample `docker-compose.yml`

```yaml
version: '3.8'

services:
  azurite:
    image: mcr.microsoft.com/azure-storage/azurite
    container_name: azurite-local
    restart: always
    ports:
      - "10000:10000" # Blob
      - "10001:10001" # Queue
      - "10002:10002" # Table
    volumes:
      - ./data:/data
    command: >
      azurite
      --blobHost 0.0.0.0
      --queueHost 0.0.0.0
      --tableHost 0.0.0.0
      --location /data
      --debug /data/debug.log
```

---

## ▶️ Running Azurite

### Option 1: Using `run.bat`

Simply double-click:

```
run.bat
```

OR run via terminal:

```bash
run.bat
```

---

### Option 2: Manual Docker Command

If you prefer command line:

```bash
docker compose up -d
```

To stop:

```bash
docker compose down
```

---

## 🌐 Azurite Endpoints

Once running, Azurite will be available at:

| Service       | URL                                              |
| ------------- | ------------------------------------------------ |
| Blob Storage  | [http://localhost:10000](http://localhost:10000) |
| Queue Storage | [http://localhost:10001](http://localhost:10001) |
| Table Storage | [http://localhost:10002](http://localhost:10002) |

---

## 🔑 Default Connection String

Use this in your `.NET` app (`local.settings.json` or `appsettings.json`):

```json
"AzureWebJobsStorage": "UseDevelopmentStorage=true"
```

Or full connection string:

```text
DefaultEndpointsProtocol=http;
AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNo... (default key);
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
```

---

## 📂 Data Persistence

All Azurite data is stored locally in:

```
./data
```

This ensures:

* Data is preserved even if container restarts
* Easy debugging via logs (`debug.log`)

---

## 🧪 Verify Setup

Run:

```bash
docker ps
```

You should see:

```
azurite-local
```

---

## 🛠️ Useful Commands

### Restart Azurite

```bash
docker compose restart
```

### View Logs

```bash
docker logs azurite-local
```

---

## 🧩 Optional Tools

### Azure Storage Explorer

Download:

👉 [https://azure.microsoft.com/en-us/products/storage/storage-explorer/](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)

Connect using:

* **Local & Attached → Storage Accounts → Emulator**

---

## ❗ Troubleshooting

### Port Already in Use

Change ports in `docker-compose.yml`:

```yaml
- "11000:10000"
```

---

### Docker Not Running

Make sure Docker Desktop is running before executing commands.

---

### Permission Issues

Run terminal as **Administrator**

---

## ✅ Summary

✔ Install Docker
✔ Extract ZIP
✔ Run `run.bat` or `docker compose up -d`
✔ Use `UseDevelopmentStorage=true` in your app
✔ Access Azurite locally


