# Eureka Server and Microservices Registration

Microservices project using **Netflix Eureka** for service discovery. A Spring Boot Eureka Server registers four microservices written in different technologies.

## 1. Architecture

| Service      | Technology  | Port(s)      | Eureka name  |
|--------------|-------------|--------------|--------------|
| Eureka Server| Spring Boot | 8761         | -            |
| CANDIDAT     | Spring Boot | 8085         | CANDIDAT     |
| JOB          | Spring Boot | <port>       | JOB          |
| MEETING      | Node.js     | 8083, 8086   | MEETING      |
| NOTIFICATION | Python      | 8084, 8087   | NOTIFICATION |

## 2. Project Structure

```
backend/
├── README.md
└── microservices/
    ├── eureka-server/
    ├── candidat/
    ├── job/
    ├── meeting/
    ├── notification/
    └── screenshots/
        └── eureka-dashboard.png
```

## 3. Eureka Server

The server is a Spring Boot application annotated with `@EnableEurekaServer`.

```properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

Start it first, then open the dashboard: http://localhost:8761

## 4. Eureka Clients

### CANDIDAT and JOB (Spring Boot)

```properties
spring.application.name=candidat
server.port=8085
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

### MEETING (Node.js)

Uses the `eureka-js-client` library.

```bash
cd microservices/meeting
npm install
node app.js
```

### NOTIFICATION (Python)

Uses the `py_eureka_client` library.

```bash
cd microservices/notification
pip install -r requirements.txt
python app.py
```

## 5. Multiple Instances

To demonstrate scalability and availability, multiple instances are configured:

- MEETING: ports 8083 and 8086
- NOTIFICATION: ports 8084 and 8087

Each instance registers independently with the Eureka Server.

## 6. Dashboard Screenshot

All four services (CANDIDAT, JOB, MEETING and NOTIFICATION) are registered and visible simultaneously, with multiple instances for MEETING and NOTIFICATION.

![Eureka Dashboard](microservices/screenshots/eureka-dashboard.png)

## 7. How to Run

1. Start the Eureka Server (port 8761).
2. Start CANDIDAT and JOB.
3. Start MEETING on ports 8083 and 8086.
4. Start NOTIFICATION on ports 8084 and 8087.
5. Open http://localhost:8761 and check that all services are **UP**.
