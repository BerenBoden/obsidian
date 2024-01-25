### *Extract and save fuzzed domains with JQ*
To extract and save the found fuzzed domains from ffuf output in a clean format with each domain on a new line, you can use a tool like `jq` to parse the JSON output and extract the URLs. Here's a step-by-step guide:

1. **Install `jq`**:
- If you don't already have `jq` installed, you can usually install it via your package manager. For instance, on Debian-based systems:
	`sudo apt-get install jq`
2. **Parse the Output File**:
- Assuming the output from ffuf is saved in a file named `juice.txt` in JSON format, you can use the following `jq` command to parse it:
	`jq -r '.results[].url' juice.txt > domains.txt`
- This command extracts the `url` field from each entry in the `results` array and saves them to `domains.txt`.
3. **Resulting File**:
- After running this command, `domains.txt` will contain a list of the fuzzed domains, each on a new line, like:
	`http://192.168.122.217:3000/profile http://192.168.122.217:3000/api http://192.168.122.217:3000/redirect ...`