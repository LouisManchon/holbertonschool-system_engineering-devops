# Web Infrastructure Design

## 📊 Diagramme d'Infrastructure


[Check  task0_diagram.md](./task0_diagram.md)

---

## 📝 Infrastructure Explanation

### 1. User Request Flow

When a user wants to access ````www.foobar.com````:

1. Their browser sends an HTTP request to the DNS resolver
2. DNS resolves www.foobar.com to IP address 8.8.8.8
3. The request reaches the web server (Nginx)
4. Static files are served directly, dynamic requests go to PHP-FPM
5. PHP-FPM communicates with MySQL database if needed
6. The complete response is sent back to the user

### 2. Components Breakdown

Server:

- A physical or virtual machine running Ubuntu 22.04 LTS
- Hosts all infrastructure components in this simple setup

Domain Name (www.foobar.com):

- Human-readable address that maps to IP 8.8.8.8
- Uses a CNAME record for the "www" subdomain pointing to foobar.com

DNS Record:

- ww is a CNAME record pointing to foobar.com
- foobar.com has an A record pointing to 8.8.8.8

Web Server (Nginx):

- Handles HTTP requests on port 80
- Serves static files (HTML, CSS, JS)
- Forwards dynamic requests to application server

Application Server (PHP-FPM):

- Processes PHP code
- Generates dynamic content
- Communicates with database when needed

Database (MySQL):

- Stores all application data
- Runs on port 3306
- Responds to SQL queries from PHP-FPM

Communication Protocol:

- Uses HTTP/HTTPS protocols
- TCP/IP for network communication

---

## ⚠️ Infrastructure Issues

1. Single Point of Failure (SPOF):
    - Entire infrastructure relies on one server
    - If server fails, website becomes completely inaccessible

2. Downtime During Maintenance:
    - Any update requiring server restart causes downtime
    - No redundancy means no failover during maintenance

3. Scalability Limitations:
    - Single server cannot handle traffic spikes
    - No load balancing mechanism
    - Database and application share same resources

4. Security Concerns:
    - No dedicated firewall
    - Database exposed on default port
    - No separation between application and database

---

## 🔧 Potential Improvements

1. Add load balancer for traffic distribution
2. Separate database server
3. Implement redundancy for all components
4. Add monitoring and backup systems
5. Use HTTPS with proper SSL certificates
