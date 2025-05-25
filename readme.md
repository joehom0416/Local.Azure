# Azure Local Development Environment

A Docker Compose setup providing local Azure service emulators for development and testing purposes.

## 🚀 Services Included

- **[Azure Service Bus Emulator](https://github.com/Azure/azure-service-bus-emulator-installer)** - Local Service Bus with queues, topics, and subscriptions
- **[Azurite](https://github.com/Azure/Azurite)** - Azure Storage emulator (Blob, Queue, Table services)
- **[SQL Server](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&tabs=cli&pivots=cs1-bash)** - Microsoft SQL Server 2022

## 📋 Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Docker context set to Linux containers
- At least 4GB of available RAM
- Ports 1433, 5672, 10000-10002 available on your machine

## 🛠️ Quick Start

### 1. Clone and Navigate
```bash
git clone [<repository-url>](https://github.com/joehom0416/Local.Azure.git)
cd Local.Azure
```

### 2. Environment Configuration
The [`.env`](.env) file contains default configuration. Review and modify if needed:
- `MSSQL_SA_PASSWORD`: SQL Server SA password (default: `Azure@123`)
- `CONFIG_PATH`: Path to Service Bus configuration file
- `ACCEPT_EULA`: Accept license terms (set to `Y`)

### 3. Start Services
```bash
docker compose up -d
```

### 4. Verify Services
Check that all containers are running:
```bash
docker compose ps
```

## 🔗 Service Endpoints

| Service | Endpoint | Purpose |
|---------|----------|---------|
| **Azurite Blob** | `http://localhost:10000` | Blob storage emulation |
| **Azurite Queue** | `http://localhost:10001` | Queue storage emulation |
| **Azurite Table** | `http://localhost:10002` | Table storage emulation |
| **SQL Server** | `localhost:1433` | Database services |
| **Service Bus** | `localhost:5672` | Message queuing services |

## ⚙️ Configuration

### Service Bus Configuration
The [`config.json`](config.json) file defines:
- **Namespace**: `sbemulatorns`
- **Queues**: `queue-01`, `queue-02`
- **Topics**: `topic-01` with subscriptions and correlation filters

### Connection Strings
Use these connection strings in your applications:

**Azurite (Development Storage)**:
```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
```

**SQL Server**:
```
Server=localhost,1433;Database=YourDatabase;User Id=sa;Password=Azure@123;TrustServerCertificate=true;
```

**Service Bus**:
```
Endpoint=sb://localhost:5672;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=DUMMY_KEY_FOR_EMULATOR
```

## 🛑 Managing Services

### Stop Services
```bash
docker compose down
```


### Pull Latest Images
To ensure you're using the latest versions of all services:
```bash
# Pull latest images for all services
docker compose pull

# Pull latest image for a specific service
docker compose pull azurite
docker compose pull mssql
docker compose pull emulator
```

### Update and Restart with Latest Images
```bash
# Stop services, pull latest images, and restart
docker compose down
docker compose pull
docker compose up -d
```

### View Logs
```bash
# All services
docker compose logs

# Specific service
docker compose logs azurite
docker compose logs mssql
docker compose logs emulator
```

### Restart Services
```bash
docker compose restart
```

## 🔧 Troubleshooting

### Common Issues

**Port Conflicts**
- Ensure ports 1433, 5672, 10000-10002 are not in use
- Check with: `netstat -an | findstr "1433\|5672\|10000\|10001\|10002"`

**SQL Server Connection Issues**
- Verify the SA password meets complexity requirements
- Wait 30-60 seconds for SQL Server to fully initialize

**Service Bus Emulator Issues**
- Check that the config.json file is properly mounted
- Ensure SQL Server is running before Service Bus emulator starts

### Reset Environment
```bash
docker compose down -v
docker compose up -d
```

## 📝 Development Notes

- The Service Bus emulator requires SQL Server as a dependency
- All services use a custom Docker network (`azure-local-networks`)
- Configuration files are mounted as volumes for easy modification
- Services are aliased within the Docker network for inter-container communication

## 📚 Additional Resources

- [Azure Service Bus Emulator Documentation](https://github.com/Azure/azure-service-bus-emulator-installer)
- [Azurite Documentation](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azurite)
- [SQL Server on Docker](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
