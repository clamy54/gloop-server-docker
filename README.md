# Gloop Server - Docker Compose Deployment

This repository contains a `docker-compose.yml` file to deploy an instance of **Gloop Server**.

**Gloop** is a centralized software lifecycle management solution for Windows workstations. It leverages [Chocolatey](https://chocolatey.org/) to manage the installation, update, and removal of software packages across multiple endpoints.

## 1. Overview

This Docker Compose setup defines two services:

- `mariadb` - A MariaDB container for storing persistent application data.
- `gloop` - The main Gloop server container, which communicates with agents and serves the API.

Gloop Server listens on port `8000` and uses MariaDB as its backend database.

## 2. Quick Start

### 1. Create Persistent Folders (Optional)

To persist configuration and database data outside of Docker volumes, you can bind local paths manually. By default, Docker named volumes are used:

```
docker volume create mariadb_data
docker volume create gloop_config
```

### 2. Start the Stack

```
docker compose up -d
```

Gloop Server will be available at:

```
http://your.ip:8000
```

If `USE_TLS` is set to `"yes"`, Gloop will generate a self-signed certificate and serve over HTTPS.

## 3. Configuration Options

The `gloop` container supports the following environment variables:

| Variable         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| MYSQL_SERVER     | Hostname of the MariaDB server (default: `mariadb`)                        |
| MYSQL_PORT       | Port number of the MariaDB server (default: `3306`)                        |
| MYSQL_USER       | MariaDB username (must have access to the `gloop` database)                |
| MYSQL_PASSWORD   | Password for the MariaDB user                                               |
| MYSQL_DATABASE   | Name of the MariaDB database (default: `gloop`)                            |
| SITE_NAME        | Displayed name of your organization or Gloop site                          |
| CLIENT_TOKEN     | Token used by Gloop clients for authentication                             |
| ADMIN_TOKEN      | Token used to authenticate admin API requests                              |
| USE_TLS          | Set to `"yes"` to enable self-signed TLS on server startup                 |

## 4. Volumes

| Volume Name     | Mounted Path         | Description                         |
|-----------------|----------------------|-------------------------------------|
| mariadb_data    | /var/lib/mysql       | MariaDB data storage                |
| gloop_config    | /gloop/config        | Gloop configuration (e.g. INI file) |

## 5. Security Notes

For production environments:

- Replace default tokens with secure, random secrets

## 6. Troubleshooting

If Gloop fails to start due to MySQL connection errors, ensure:

- The `mariadb` container is running and accepting connections
- All credentials are consistent between Gloop and MariaDB
- Gloop waits for MySQL to be ready (a retry mechanism is included internally)

## 7. License

This project is provided under the MIT License. 

