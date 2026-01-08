# Docker Test App - Learning Project

A full-stack Node.js application with MongoDB and Mongo Express, containerized using Docker. This project demonstrates Docker fundamentals including multi-container orchestration, volumes, networking, and Docker Compose.

## 🚀 Project Overview

This is a simple user management application that allows you to:
- Add users via POST request
- Retrieve all users via GET request
- View and manage MongoDB data through Mongo Express GUI
- Persist data using Docker volumes

## 🛠️ Technologies Used

- **Node.js** - Backend server
- **Express.js** - Web framework
- **MongoDB** - Database
- **Mongo Express** - Database GUI
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration

## 📚 What I Learned

### Docker Fundamentals
- Creating Dockerfiles and building custom images
- Understanding Docker layers and caching
- Pushing images to Docker Hub
- Managing containers (start, stop, remove)

### Docker Compose
- Writing docker-compose.yaml files
- Orchestrating multiple containers
- Setting up container networking
- Managing environment variables
- Using depends_on for container dependencies

### Docker Volumes
- Understanding volume persistence
- Creating named volumes vs bind mounts
- Data persistence across container lifecycles
- Volume inspection and management

### Docker Networking
- Container-to-container communication
- Using service names as hostnames
- Port mapping (host:container)

## 📋 Prerequisites

- Docker Desktop installed
- Docker Compose installed
- Git (for cloning)
- A Docker Hub account (optional, for pushing images)

## 🔧 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/Arbinnn/Docker-Learning.git
cd docker-testapp
```

### 2. Build the Application Image

```bash
docker build -t testapp:1.0 .
```

### 3. Start All Services

```bash
docker compose -f mongodb.yaml up -d
```

This will start:
- **MongoDB** on port `27017`
- **Mongo Express** on port `8081`
- **Test App** on port `5051`

### 4. Verify Running Containers

```bash
docker ps
```

## 🎯 Usage

### Access the Application

- **Application API**: http://localhost:5051
- **Mongo Express GUI**: http://localhost:8081

### API Endpoints

**Get All Users:**
```bash
curl http://localhost:5051/getUsers
```

**Add New User:**
```bash
curl -X POST http://localhost:5051/addUser \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "name=John&email=john@example.com"
```

### Using Mongo Express

1. Open http://localhost:8081 in your browser
2. Navigate to the `Arbin` database
3. View/edit the `users` collection
4. Add documents through the GUI

## 💾 Docker Volumes

This project uses Docker volumes to persist MongoDB data.

### Check Volume

```bash
# List all volumes
docker volume ls

# Inspect the MongoDB volume
docker volume inspect docker-testapp_mongodb_data
```

### Test Data Persistence

```bash
# 1. Add some data via Mongo Express or API

# 2. Stop all containers
docker compose -f mongodb.yaml down

# 3. Restart containers
docker compose -f mongodb.yaml up -d

# 4. Check data still exists
curl http://localhost:5051/getUsers
```

Your data persists because it's stored in the volume, not inside the container!

### Remove Volume (Deletes Data)

```bash
docker compose -f mongodb.yaml down
docker volume rm docker-testapp_mongodb_data
```

## 🐳 Useful Docker Commands

### Container Management

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop all services
docker compose -f mongodb.yaml down

# Start services
docker compose -f mongodb.yaml up -d

# View container logs
docker logs <container_name>
docker logs mongodb
docker logs testapp

# Follow logs in real-time
docker logs -f testapp

# Execute commands inside container
docker exec -it mongodb bash
```

### Image Management

```bash
# List images
docker images

# Remove an image
docker rmi testapp:1.0

# Remove unused images
docker image prune

# Tag image for Docker Hub
docker tag testapp:1.0 yourusername/testapp:1.0

# Push to Docker Hub
docker login
docker push yourusername/testapp:1.0

# Pull from Docker Hub
docker pull yourusername/testapp:1.0
```

### Volume Management

```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect docker-testapp_mongodb_data

# Remove specific volume
docker volume rm docker-testapp_mongodb_data

# Remove all unused volumes
docker volume prune
```

### Cleanup Commands

```bash
# Stop and remove all containers
docker compose -f mongodb.yaml down

# Remove containers, networks, and volumes
docker compose -f mongodb.yaml down -v

# Clean up everything (use with caution!)
docker system prune -a --volumes
```

## 📁 Project Structure

```
docker-testapp/
├── Dockerfile              # Instructions to build testapp image
├── mongodb.yaml            # Docker Compose configuration
├── package.json            # Node.js dependencies
├── server.js               # Express server code
├── public/
│   ├── index.html         # Frontend UI
│   └── style.css          # Styling
└── README.md              # This file
```

## 🔍 Docker Compose Configuration

The `mongodb.yaml` defines three services:

1. **mongodb**: Database service with volume for data persistence
2. **mongo-express**: Web-based MongoDB admin interface
3. **testapp**: Custom Node.js application

All services are connected via a Docker network and can communicate using service names.

## 🐛 Troubleshooting

### Port Already in Use

```bash
# Check what's using the port
netstat -ano | findstr :5051

# Kill the process or use a different port in mongodb.yaml
```

### Connection Refused

Make sure all containers are running:
```bash
docker ps
```

Check container logs:
```bash
docker logs testapp
docker logs mongodb
```

### Volume Data Not Persisting

Ensure volumes are properly defined in `mongodb.yaml`:
```yaml
volumes:
  mongodb_data:
```

### Image Not Found

Rebuild the image:
```bash
docker build -t testapp:1.0 .
```

## 📝 Notes

- MongoDB credentials: `admin` / `qwerty` (for learning purposes only)
- The application uses MongoDB's `Arbin` database and `users` collection
- Data persists in the `mongodb_data` Docker volume
- Containers communicate using Docker's internal network

## 🎓 Key Takeaways

1. **Dockerfile**: Defines how to build your application image
2. **Docker Compose**: Orchestrates multiple containers with one command
3. **Volumes**: Keep your data safe when containers are removed
4. **Networking**: Containers can talk to each other using service names
5. **Port Mapping**: Expose container services to your host machine

## 📄 License

This is a learning project - feel free to use and modify as needed.

## 👤 Author

**Arbin**
- GitHub: [@Arbinnn](https://github.com/Arbinnn)
- Docker Hub: [arbinnnn](https://hub.docker.com/u/arbinnnn)

---

Happy Dockering! 🐳
