# Distributed Web Infrastructure

## 📊 Infrastructure Diagram

[Check  README.md](./README.md)

---

## 📝 Infrastructure Explanation

### 1. User Request Flow

When a user accesses ```www.foobar.com```:

1. Browser sends HTTP request to DNS resolver
2. DNS resolves to load balancer IP
3. Load balancer distributes request using Round Robin algorithm
4. Request reaches one of the web servers (Nginx)
5. Static files served directly, dynamic requests processed by PHP-FPM
6. PHP-FPM communicates with MySQL database if needed
7. Complete response sent back to user through load balancer

---

### 2. Components Breakdown

Load Balancer (HAProxy):

- Distributes traffic between servers
- Operates on Round Robin algorithm
- Single point of entry for all requests

Web Servers (Nginx):

- Two identical servers (8.8.8.1 and 8.8.8.2)
- Handle HTTP requests on port 80
- Serve static files and forward dynamic requests

Application Servers (PHP-FPM):

- Process PHP code on each server
- Generate dynamic content
- Communicate with respective databases

Databases (MySQL):

- Primary-Replica configuration
- Primary (8.8.8.1): Handles all write operations
- Replica (8.8.8.2): Read-only, receives asynchronous replication
- Both run on port 3306

File Storage:

- Each server maintains its own copy of application files
- Static files (HTML/CSS/JS) served directly by Nginx

---


## ⚠️ Infrastructure Issues

1. Single Points of Failure:
    - Load balancer: If fails, entire system becomes unavailable
    - Primary database: If fails, no write operations possible

2. No Automatic Failover:
    - Manual intervention required if primary DB fails
    - No health checks for automatic traffic redirection

3. Security Concerns:
    - No HTTPS encryption (traffic in clear text)
    - Database ports exposed without firewall
    - No authentication between servers

4. Scalability Limitations:
    - Adding more servers requires manual configuration
    - Database replication adds load to primary
    - No caching layer for frequent queries

5. Maintenance Challenges:
    - Updates require coordinated downtime
    - No blue-green deployment capability
    - Database schema changes need careful planning
