## Volume Management & Disk Usage

### Overview
This procedure entails creating a directory, mounting a new volume (or loop device for practice), and checking the configuration using disk management commands.

### Steps
1. #### Create the Directory
    ```
    sudo mkdir -p /mnt/devops_data
    ```
    `-p ` Ensures that the directory is created only if it doesn’t exist.

2. #### Using a Loop Device for Local Practice

    ```
    dd if=/dev/zero of=/tmp/devops_volume.img bs=1M count=100
    ```
3. #### Attach It as a Loop Device:

    ```
    sudo losetup /dev/loop0 /mnt/devops_volume.img
    ```
4. #### Format It as ext4:

    ```
    sudo mkfs.ext4 /dev/loop0
    ```
5. #### Mount the volume to the directory

    ```
    sudo mount /dev/loop0 /mnt/devops_data
    ```
6. #### Verify disk usage and mounted volume

    ```
    df -h
    mount | grep devops_data
    ```
### OUTPUT
After running df -h, you should see:

  
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/loop0      100M  1M   99M  1%   /mnt/devops_data
    
And after running mount | grep devops_data:

   
    /dev/loop0 on /mnt/devops_data type ext4 (rw,relatime)
  

## Notes
- Ensure you have sudo permissions to execute these commands.
- Unmount the volume when done
    ```
    umount /mnt/devops_data
    losetup -d /dev/loop0
    ```
- In the absence of an actual volume, the loop device method is employed for practice.
- The losetup command configures a loopback device to simulate a storage device.

## Conclusion
This assignment assists with understanding disk use, volume management, and storage mounting in Linux systems.




