### *Applications from go install not working*
1. **Check Go Environment Variable**:
    - First, verify the output of `go env GOPATH` by running it standalone. Just type `go env GOPATH` in your terminal and press Enter. It should return the path to your GOPATH.
2. **Correct Syntax to Add to PATH**:
    - The correct way to modify the `PATH` variable in Bash is by using the `export` command. You should also use command substitution with `$()` to get the output of `go env GOPATH`.
    - The command should look like this:
        `export PATH=$PATH:$(go env GOPATH)/bin`
    - This command appends the Go bin directory to your existing `PATH`.
3. **Making the Change Permanent**:
    - To ensure this change persists across sessions, add the export command to your `.bashrc` or `.bash_profile` file.
    - You can do this by editing `.bashrc` (e.g., using `nano ~/.bashrc`) and adding the `export` line to the end of the file.
    - After saving the file, you can apply the changes immediately by running `source ~/.bashrc`.
4. **Verify the Change**:
    - After updating the `PATH`, you can verify it by running `echo $PATH`. You should see the Go bin directory appended at the end.