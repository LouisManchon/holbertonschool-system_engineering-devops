# Secured and Monitored Web Infrastructure

## 📊 Infrastructure Diagram

[Check  task2_diagram.md](./task2_diagram.md)

---

## 📝 Infrastructure Explanation

### 1. User Request Flow (Secured)

When a user accesses ```https://www.foobar.com```:

1. HTTPS Request: Browser sends encrypted request to firewall
2. Firewall Protection: External firewall filters malicious traffic
3. SSL Termination: Load balancer decrypts traffic using SSL certificate
4. Internal Firewall: Second firewall protects internal network
5. Traffic Distribution: Load balancer routes to available server6
6. Request Processing: Server handles request (static/dynamic)
7. Database Operations: Application communicates with MySQL
8. Monitoring: All components send metrics to monitoring system
9. Response Path: Encrypted response sent back through firewalls

### 2. Components Breakdown

New Security Components:

1. Firewalls (3x):
    - External Firewall: Filters incoming traffic before load balancer
    - Internal Firewall: Protects servers from internal threats
    - Database Firewall: Controls replication traffic between MySQL servers
    - Purpose: Prevent unauthorized access, filter malicious traffic, enforce security policies

2. SSL Certificate:
    - Let's Encrypt Wildcard: Covers www.foobar.com and subdomains
    - HTTPS Benefits:
        - Encrypts all client-server communication
        - Prevents man-in-the-middle attacks
        - Improves SEO ranking (Google preference)
        - Builds user trust (padlock icon in browser)

3. Monitoring System:
    - SumoLogic Agents: Installed on each server and load balancer
    - Data Collection:
        - Server metrics (CPU, memory, disk I/O)
        - Web server stats (requests, response times)
        - Database performance (queries, connections)
        - Application logs (errors, warnings)
    - Monitoring QPS:
        - Configure agent to track nginx.requests metric
        - Set up dashboard with QPS graph
        - Create alerts for abnormal spikes/drops

Existing Components (Enhanced):

- Load Balancer: Now terminates SSL and distributes decrypted traffic
- Web Servers: Configured for HTTPS (port 443)
- Databases: Replication traffic passes through dedicated firewall

---

## ⚠️ Infrastructure Issues

### 1. SSL Termination at Load Balancer

Problem: Decrypted traffic between load balancer and servers

- Security Risk: Internal traffic vulnerable to interception
- Compliance Issue: May violate PCI-DSS or other security standards
- Solution: Implement end-to-end encryption (SSL to servers)

### 2. Single Write-Capable MySQL Server

Problem: Primary database is single point of failure

- Downtime Risk: Any primary failure halts all write operations
- Performance Bottleneck: All writes go through single server
- Solution: Implement MySQL InnoDB Cluster for multi-primary setup

### 3. Monolithic Server Configuration

Problem: Each server runs all components (web, app, DB)

- Resource Contention: Components compete for CPU/memory
- Scaling Difficulty: Can't scale individual components independently
- Security Risk: Database exposure if web server compromised
- Solution: Separate tiers (web servers, app servers, DB servers)

### 4. Additional Security Concerns

- Certificate Management: Single SSL certificate creates single point of failure
- Firewall Configuration: Complex rules may impact performance
- Monitoring Overhead: Agents consume server resources
