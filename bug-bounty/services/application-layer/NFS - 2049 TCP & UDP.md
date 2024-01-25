NFS (Network File System) typically uses multiple ports for its operation, and it makes use of RPC (Remote Procedure Calls) for its services. One key component in this setup is the [[RPCBind - 111 TCP & UDP]] service. RPCBind is responsible for mapping RPC program numbers to network port numbers, helping to direct incoming requests to the correct service.

NFS can operate over both TCP and UDP, depending on the configuration and version of NFS being used.

- **UDP**: Older versions of NFS primarily used UDP because it is a lighter-weight protocol that generally offers faster data transfers. However, UDP lacks features like reliable delivery and in-order arrival of data packets.
    
- **TCP**: Newer versions of NFS, especially NFSv4 and later, are more likely to use TCP as the default. TCP is more reliable and better suited for connections over the internet or less reliable networks. It ensures that data packets arrive in the correct order and handles re-transmission of lost packets, providing a more reliable data transfer than UDP.
##### ***Resources***
https://en.wikipedia.org/wiki/Network_File_System
