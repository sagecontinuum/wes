# Waggle Edge Stack (WES)
#### Lead: [Yongho Kim](mailto:yongho.kim@anl.gov)(Edge Scheduling) [Joe Swantek](mailto:joseph.swantek@northwestern.edu)(Software Infrastructure.)

### Overview:
Waggle nodes host a Linux-based Node Controller (NC) and an Edge Processor (EP) for processing sensor data in-situ. The WES includes the [operating system image and Waggle services](https://github.com/waggle-sensor/waggle-image-v2) running on the [NC](https://github.com/waggle-sensor/nodecontroller) and [EP](https://github.com/waggle-sensor/edge_processor) as well as the ML run-time libraries and tools. It also manages cybersecurity, certificate management, and manages system resources, such as power, memory, and cores. It constantly updates its state with the cloud server to fetch and perform any task scheduled from SES.

### Requirements:

WES maintains the network connection to the cloud server. If the connection is lost, WES continues running autonomously, executing any scheduled tasks previously provided by the SES.  When connections are restored, Waggle nodes synchronize their configuration with Beehive-Admin. One of the WES services manages fetching new EP jobs and handling their execution and managing system resources. WES should also report status on running applications as well as housekeeping data, including hardware failures (e.g., loss of sensors).

### Use Cases:
#### Administrator deploys a Waggle Node:
* Following instructions, admin installs and powers up node
* Basic configuration scripts connect to BDR, and SES
* Client-side WES components connect to SES and BDR configuration management and health check dashboard
#### Developer logs into EC:
* After getting an authentication token from the SES, a developer logs into an EC node directly, via SSH
* Developers connect to local services via SSH on the EC and test code.
### Milestones:
* Publish design document, including events and logging
* Initial version of WES deployed on Chameleon Sage nodes
* Integrate ECR performance tools
* The capability for "To-Edge" and "From-Edge" events/triggers demonstrated on initial Sage deployment.
* V1.0 WES toolbox for developers and administrators released
