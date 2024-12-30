---
layout: post
title:  "The Bitwise Project"
date:   2024-12-30 21:18:00 +0200
categories: blog
---

# Bitwise.foo

I recently purchased the domain [bitwise.foo]—because, why not? For a while now, I’ve been building custom applications, and this felt like the perfect opportunity to put them to use. One of these applications is my login manager, a C++ application built on top of SQLite's C API.

[bitwise.foo]: https://bitwise.foo

You can check out the current version of Bitwise at **[dev.bitwise.foo](https://dev.bitwise.foo)**. Please note that it may take a few seconds to load, as it requires launching a Python environment on my web service provider's platform.

## About the Bitwise Project

Bitwise consists of several applications running across different environments. My motivation for this project was to run C/C++ applications through a web server. The first application I developed for Bitwise is the **Login Manager**, designed to be a fast and secure tool for handling user login credentials.

### New Features: Email Verification

To enhance security, I’ve added email verification for user registration. This ensures that only verified email addresses can create accounts, providing an additional layer of trust and reliability to the system.

### Infrastructure Challenges

Initially, my idea was to host all web services on my local server. However, my ISP blocked port 80 (the HTTP-standard port), and my external web hosting provider didn’t offer custom port redirects. To work around these limitations, I deployed a Python application on my external hosting provider. This setup allowed me to create an infrastructure that supports running C/C++ applications securely, albeit with some complexity.

### Bitwise Infrastructure: A Flowchart

The infrastructure of Bitwise addresses both functionality and security concerns. Below is a summary of how the system works:

1. **Secure External Communication**  
   The client communicates with the external web server over HTTPS, ensuring a secure connection.

2. **Local User Data**  
   Once a user session is established, user information is stored locally for fast retrieval.

3. **TCP Connection to Local Server**  
   The external web server establishes a TCP connection to the local server through a firewall, allowing only this specific connection.

4. **Containerized Local Server**  
   The local server uses container technology via Docker CLI to enhance security by isolating each application.

5. **API Application in a Container**  
   An API application runs inside a container to handle incoming TCP connections from the external web server. It uses its own public-private key infrastructure for encrypted communication.

6. **Login Manager Integration**  
   The API application communicates with the **Login Manager** using UDP (for lower latency) via a shared Docker network. The Login Manager is hosted in a separate container, and its database is stored in a Docker volume to ensure persistence during container management.

7. **Response Flow**  
   When a login request is processed, the result travels back through the API to the external web server. The server then processes the logic and presents the response to the user.

This layered setup ensures security while maintaining flexibility and performance.

## Moving Forward

The Bitwise project has been a fascinating journey, combining web hosting challenges, containerization, and secure C++ application development. Adding email verification for user registration is just one of many improvements I have planned. I look forward to expanding this infrastructure and sharing more updates as the project evolves.

Stay tuned!

---
*Visit [bitwise.foo](https://bitwise.foo) or the current development version at [dev.bitwise.foo](https://dev.bitwise.foo) for more updates on the proje
