# Creating a ZFS Pool on Ubuntu with RAID-Z1

Follow these steps to create a ZFS pool with RAID-Z1 redundancy on Ubuntu. RAID-Z1 provides data redundancy and requires a minimum of three drives.

**Note**: When using drives of different sizes in a ZFS pool, there are important considerations to keep in mind:
- The capacity of the pool will be limited by the size of the smallest drive.
- Usable space in a redundancy configuration (e.g., RAID-Z1) will be determined by the capacity of the smallest drive multiplied by the number of drives minus one (for the parity).
- Performance may be influenced by the speed of the slowest drive.
- Drive replacement should involve using a drive of at least the same size as the failed drive.

1. **Install ZFS Utilities**:
   - Install the ZFS utilities on your Ubuntu system:
     ```bash
     sudo apt install zfsutils-linux
     ```

2. **List Your Drives Using `lsblk`**:
   - Open a terminal and run the `lsblk` command to identify the device names of the drives you want to use for your ZFS pool. For example:
     ```bash
     lsblk
     ```
   - Take note of the device names (e.g., `/dev/sdX`, `/dev/sdY`, `/dev/sdZ`) for the drives you want to include in your ZFS pool.

3. **Create a ZFS Pool with RAID-Z1**:
   - Replace `/dev/sdX`, `/dev/sdY`, and `/dev/sdZ` with the actual device names of your drives.
   - You can choose a name for your pool (replace "poolname" with your preferred name) and specify a mount point (e.g., `/poolname`) using the `-m` option.
   - RAID-Z1 provides data parity, allowing one drive to fail without data loss:
     ```bash
     sudo zpool create poolname raidz1 /dev/sdX /dev/sdY /dev/sdZ -m /poolname
     ```
   - This command creates a ZFS pool with RAID-Z1 configuration using the specified drives and mounts it at `/poolname`.

4. **Verify Your ZFS Pool**:
   - You can verify that the pool has been created by running:
     ```bash
     sudo zpool list
     ```
   - This command will display a list of ZFS pools on your system, including the one you just created.

5. **Access Your ZFS Filesystem**:
   - Your ZFS pool is now accessible at the mount point you specified (e.g., `/poolname`). You can navigate to this location to access and manage your ZFS filesystem.

Replace "poolname" and the device names (`/dev/sdX`, `/dev/sdY`, `/dev/sdZ`) with your preferred names and actual device names from step 2.
