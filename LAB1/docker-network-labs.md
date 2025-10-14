
# Docker Network Labs

## 🧩 Task 1 — HR App Network Setup

### Objective
Create a **dedicated isolated bridge network** for the HR application stack and verify internal connectivity between containers.

### Commands
```bash
docker network create --driver bridge --subnet 192.168.20.0/24 --gateway 192.168.20.1 hr-app-net
docker inspect hr-app-net

docker run -d --name nginx-server --network hr-app-net nginx:latest
docker run -d --name alpine-tester --network hr-app-net alpine:latest sleep 3600

docker ps
docker inspect nginx-server
docker inspect alpine-tester

docker exec -it alpine-tester sh
ping nginx-server

    ____________________________-

# 🧩 Task 2 — Multi-Homed Container Architecture (NGINX Load Balancer)

## 🎯 Objective
Demonstrate a **multi-homed NGINX container** connected to two **isolated bridge networks**.  
This setup separates frontend client traffic from backend service communication, ensuring secure and controlled routing through the Load Balancer.

---

## ⚙️ Network Setup

### 1. Create the Frontend Network
```bash
docker network create --driver bridge --subnet 10.1.1.0/24 --gateway 10.1.1.1 frontend-net
docker network create --driver bridge --subnet 10.1.2.0/24 --gateway 10.1.2.1 backend-net
docker run -d --name client-tester --network frontend-net alpine:latest sleep 3600
docker run -d --name backend-db --network backend-net alpine:latest sleep 3600
docker ps
docker run -d --name nginx-lb --network frontend-net --network backend-net nginx:latest
docker exec -it client-tester sh
ping nginx-lb 
ping backend-db # ping: bad address 'backend-db' # Reason:
# client-tester is on frontend-net, while backend-db is on backend-net.
# No direct route exists between them → confirms network isolation.


ping nginx-lb # Both containers (client-tester and nginx-lb) share the same subnet (frontend-net).

# Docker’s internal DNS resolved nginx-lb to its IP 10.1.1.3.

# ICMP packets reached successfully → 0% packet loss.











