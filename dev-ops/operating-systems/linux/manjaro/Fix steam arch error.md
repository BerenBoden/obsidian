1. They used `ls -l` to list the details of the `libGL.so` and `libGL.so.1` files in the `/usr/lib32/` directory. These files are symbolic links that point to the actual driver file (`libGL.so.331.20`).
    
2. They then listed the details of the actual driver file (`libGL.so.331.20`) and found that it was an old version of the driver.
    
3. They used the `pacman -Qo` command to find out which package owns the `libGL.so.331.20` file. The output indicated that no package owns the file, which means it was likely left over from a previous installation and is no longer needed.
    
4. They removed the `libGL.so.331.20` file and the symbolic links that pointed to it.
    
5. They reinstalled the 32-bit Nvidia drivers using the `pacman -Sy lib32-nvidia-libgl` command.
    
6. After doing this, they were able to run Steam, which suggests that the issue was resolved.
    

Here's how you could do something similar:

1. Use `ls -l` to list the details of your driver files:

bashCopy code

`ls -l /usr/lib32/libGL.so ls -l /usr/lib32/libGL.so.1`

2. If these files are symbolic links that point to an old driver file, use `pacman -Qo` to find out which package owns the file:

bashCopy code

`pacman -Qo /usr/lib32/your-driver-file`

3. If no package owns the file, remove it and any symbolic links that point to it:

bashCopy code

`sudo rm /usr/lib32/your-driver-file sudo rm /usr/lib32/libGL.so sudo rm /usr/lib32/libGL.so.1`

4. Reinstall your graphics drivers. The exact command will depend on your graphics card and the drivers it uses. For Nvidia, you might use something like:

bashCopy code

`sudo pacman -Sy lib32-nvidia-libgl`