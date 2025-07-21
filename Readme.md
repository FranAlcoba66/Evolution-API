# 🚀 Evolution API - Quick Deployment

This repository provides a straightforward way to deploy the **Evolution API** using Docker Compose, complete with **PostgreSQL** for data persistence and **Redis** for caching.

---

## ⚙️ Configuration

1.  **Create an `.env` file**: In the root of this project, create a file named `.env` and paste the following content:

    ```
    # Evolution API Configuration
    AUTHENTICATION_API_KEY=your_secure_api_key_here

    # PostgreSQL
    DATABASE_ENABLED=true
    DATABASE_PROVIDER=postgresql
    DATABASE_CONNECTION_URI=postgresql://evolution_user:evolution_password@postgres:5432/evolution
    DATABASE_CONNECTION_CLIENT_NAME=evolution_api
    DATABASE_SAVE_DATA_INSTANCE=true
    DATABASE_SAVE_DATA_NEW_MESSAGE=true
    DATABASE_SAVE_MESSAGE_UPDATE=true
    DATABASE_SAVE_DATA_CONTACTS=false
    DATABASE_SAVE_DATA_CHATS=true
    DATABASE_SAVE_DATA_LABELS=true
    DATABASE_SAVE_DATA_HISTORIC=false

    # Redis
    CACHE_REDIS_ENABLED=true
    CACHE_REDIS_URI=redis://redis:6379
    CACHE_REDIS_PREFIX_KEY=evolution
    CACHE_REDIS_SAVE_INSTANCES=false
    CACHE_LOCAL_ENABLED=false

    # QR Code Compatibility
    CONFIG_SESSION_PHONE_VERSION=2.3000.1023204200
    ```

    **Remember to replace `your_secure_api_key_here`** with your own strong API key.

2.  **`docker-compose.yml`**: This file is already configured for deployment:

    ```yaml
    version: "3.9"
    services:
      evolution-api:
        container_name: evolution_api
        image: atendai/evolution-api:v2.1.1
        restart: always
        ports:
          - "8080:8080"
        env_file:
          - .env
        volumes:
          - evolution_instances:/evolution/instances
        depends_on:
          - postgres
          - redis

      postgres:
        image: postgres:13
        container_name: evolution_postgres
        restart: always
        environment:
          POSTGRES_DB: evolution
          POSTGRES_USER: evolution_user
          POSTGRES_PASSWORD: evolution_password
        volumes:
          - postgres_data:/var/lib/postgresql/data

      redis:
        image: redis:latest
        container_name: evolution_redis
        restart: always
        volumes:
          - redis_data:/data

    volumes:
      evolution_instances:
      postgres_data:
      redis_data:
    ```

---

## 🚀 Deployment

Make sure you have **Docker** and **Docker Compose** installed.

1.  **Place the files**: Put the `.env` file (which you created in the "Configuration" step) and this `docker-compose.yml` file in the same directory.
2.  **Run Docker Compose**: Open your terminal in that directory and execute:

    ```bash
    docker-compose up -d
    ```

    This command will download the necessary images and start all services in the background.

3.  **Access the API**: The Evolution API will be available at `http://localhost:8080`.

---

## 🛑 Stopping Services

To stop and remove all containers, networks, and volumes (this will delete your data):

```bash
docker-compose down -v



```
## 🛑 To stop containers without removing data:
```
docker-compose stop

```
