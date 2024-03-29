To set a static IP address on a Raspberry Pi running Ubuntu before the first boot, you'll need to modify the network configuration files on the SD card directly. Here's a step-by-step guide:

# Write the Ubuntu Image to the SD Card:

Download the official Raspberry Pi Ubuntu image from the Ubuntu website.
Use a tool like Etcher or the dd command to write the downloaded Ubuntu image to an SD card. Make sure the SD card is correctly inserted into your computer.
Mount the Boot Partition:

After writing the image, your SD card will have two partitions: one for boot and one for the root filesystem.
Mount the boot partition (the smaller one) to your computer. You can do this by inserting the SD card into your computer and finding the mounted partition. The boot partition is usually named "boot" on Ubuntu images.
Configure the Static IP Address:

# Configure network setting

Within the mounted boot partition, locate and open the network-config file. If it's not present, create the file.
Edit the network-config file to specify your static IP configuration. You can use a text editor like Nano or Notepad.
Example network-config:

```
  eth0:
    addresses: [192.168.1.10/24]  # Set your desired IP and subnet mask
    gateway4: 192.168.1.1  # Set your router's IP address
    nameservers:
      addresses: [8.8.8.8, 8.8.4.4]  # Use your preferred DNS servers
```
Customize the IP address, subnet mask, gateway, and DNS addresses according to your network.

# Configure SSH:

Within the mounted root partition, navigate to the /etc/ssh directory.

Create a file named ssh in this directory. This will enable SSH on the first boot.

For example, on a Unix-based system, you can run the following command to create the file:

bash
Copy code
touch /path/to/mounted/root/etc/ssh/ssh

# Set the Admin Password:

If you want to set the admin password, you need to modify the password hash for the 'ubuntu' user in the /etc/shadow file.

To set the password, you should generate a password hash using a tool like mkpasswd or openssl.

Here's an example of how to use mkpasswd to generate a password hash and set it for the 'ubuntu' user:

bash
Copy code
mkpasswd -m sha-512
You will be prompted to enter the desired password. After entering the password, mkpasswd will display the password hash. Copy the generated hash.

Open the /etc/shadow file in a text editor within the mounted root partition and find the line for the 'ubuntu' user.

Replace the existing password hash with the one you generated.


# Eject the SD Card:

Safely eject the SD card from your computer.
Insert the SD Card into Raspberry Pi:

Insert the SD card with the modified network-config file into your Raspberry Pi.
Boot the Raspberry Pi:

Power on your Raspberry Pi. It will boot using the network configuration you specified in network-config.
The Raspberry Pi should now have the static IP address you configured in network-config. You can SSH into it or access it using the static IP you set.

Please note that this method is specific to the official Ubuntu images and the use of cloud-init. It may not work with all Raspberry Pi distributions, so make sure to check the documentation for the specific image you are using.
