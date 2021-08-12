# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- Kali
  - **Operating System**:Debian Kali 5.4.0
  - **Purpose**:Penetration Testing
  - **IP Address**:192.168.1.90
- Elk
  - **Operating System**:Ubuntu 18.04
  - **Purpose**:ELK Stack (Kibana with Elasticsearch)
  - **IP Address**:192.168.1.100
- Target 1
  - **Operating System**:Debian GNU/Linux 8
  - **Purpose**:Wordpress web server
  - **IP Address**:192.168.1.110
- Target 2
  - **Operating System**:Debian GNU/Linux 8
  - **Purpose**:Wordpress web server
  - **IP Address**:192.168.1.115
  Capstone
  - **Operating System**:Ubuntu 18.04
  - **Purpose**:Web Server
  - **IP Address**:192.168.1.105

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors Alert

**Excessive HTTP Errors Alert** is implemented as follows:
  - Metric: **WHEN count() GROUPED OVER top 5 'http.response.status_code'**
  - Threshold: **IS ABOVE 400**
  - Vulnerability Mitigated: **Enumeration/Brute Force**
  - Reliability: High Reliability - By filtering on 400 and 500 error codes at a high rate will help to detect web server attacks.

#### HTTP Request Size Monitor Alert
HTTP Request Size Monitor Alert is implemented as follows:
  - Metric: **WHEN sum() of http.request.bytes OVER all documents**
  - Threshold: **IS ABOVE 3500**
  - Vulnerability Mitigated: **Code injection in HTTP requests (XSS and CRLF) or DDOS**
  - Reliability: Medium Reliability.  Alert might create false positives.  
      It could be triggered by a large amount of legitimate HTTP traffic

#### CPU Usage Monitor Alert
CPU Usage Monitor Alert is implemented as follows:
  - Metric: **WHEN max() OF system.process.cpu.total.pct OVER all documents**
  - Threshold: **IS ABOVE 0.5**
  - Vulnerability Mitigated: **Malicious software using large amounts of resources**
  - Reliability: Highly Reliabile - Can also be used to judge 
    legitimate uses of resources to improve exisiting processes.


