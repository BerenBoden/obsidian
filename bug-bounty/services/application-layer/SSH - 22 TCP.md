##### ***SCP (Secure Copy Protocol)***
Copy a file from local to remote host:  
    `scp local_file username@remote_host:remote_directory/`
Copy a file from remote to local host:  
    `scp username@remote_host:remote_file local_directory/`
Copy a local directory to a remote host:  
    `scp -r local_directory/ username@remote_host:remote_directory/`
Copy a file from a remote host to local host using a specific port:  
    `scp -P 2222 username@remote_host:remote_file local_directory/`
Copy a file between two remote hosts from your local machine:  
```
scp username1@remote_host1:/path/to/file username2@remote_host2:/path/to/destination
```