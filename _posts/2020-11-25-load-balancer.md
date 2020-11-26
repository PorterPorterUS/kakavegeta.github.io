### Load Balancer

Load balancers distribute incoming client requests to computing resources such as application server or database. Then load balancer returns the response from the computing resources to client. Load balancers are effective at:

- preventing requests from going to unhealthy servers
- preventing overloading resources
- helping to eliminate a single point of failure.

Also, loader balancers have following benefits:

- SSL termination: decrypt incoming requests and encrypt server responses so backend servers have no need to perform those expensive operations.
- Session persistence: if the web apps do not keep track of sessions, load balancer can issue cookies and route a specific client's request to same instance.

Load balancers can route traffic based on various metrics, including:

- Random 

- Latest loaded

- Session/Cookie

- Round robin or weighted round robin

- Layer 4

  Layer 4 load balancers look at info at transport layer to decide how to distribute request, which generally involves the source, destination IP address, and ports in the header, but not the content of packet. Layer 4 load balancers forward network packets to and from the upstream server, perform network address translation.

- Layer 7

  Layer 7 load balancers look at application layer to decide how to distribute request. This can involve content in the header, message, and cookies. Layer 7 load balancer terminate network traffic, read message, make a load-balancing decision, then opens a connection to selected server. For example, a layer 7 load balancer can direct video traffic to servers that host videos while directing more sensitive user billing traffic to security-hardened servers. 



