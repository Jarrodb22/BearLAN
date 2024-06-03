### Setup OS
 On a fresh debian install, you will want to set up the OS as i lay out below. With the os setup this waay you can run these containers easily using data from your shared drives. If you intend to use local directories you can skip this step.

 **Login as root**<br>
 `apt update && apt upgrade -y`
 `apt install sudo -y`<br>
 Logout

 **Login as user and install cifs**<br>
 `sudo apt install cifs-utils`

 **Make directory for mount**<br>
 `sudo mkdir /mnt/{directory} (repeat for each mount location)`

 **Edit fstab**<br>
 `sudo nano /etc/fstab`<br>
 and connect the mounts to your shares by inputting this line in the fstab.
 ```
 //{share ip}/{sharen name} /mnt/{directory} cifs credentials=/root/smbcredentials,uid=1000,gid=1000,noauto,x-systemd.automount 0 0
 ```
 you will need to do this for each drive location. In my instance i only use one and it is /mnt/media.

 **Create credentials file**<br>
 `sudo nano /root/smbcredentials`<br>
 paste in the following
 ```
 user=#username
 password=#password
 ```

 **Lastly mount all drives and reboot**<br>
 `sudo mount -a`
 `reboot`<br>
### Setup SSH with VS Code
 
### Install Docker
 Again, just a little extra bit of setup i like to do befor moving onto the portainer install is to set my network configuration to my liking.

 **Edit interfaces**<br>
 `sudo nano /etc/network/interfaces`<br>
 you will want to find this portion and edit it to your liking
 ```
 # The primary network interface
 auto enp0s5
 iface enp0s5  inet static
 address 192.168.2.236
 netmask 255.255.255.0
 gateway 192.168.2.254
 dns-domain sweet.home
 dns-nameservers 192.168.2.254 1.1.1.1 8.8.8.8
 ``` 
 save and reboot.<br>

 **Docker Setup**<br>
 `sudo spt install curl -y`<br>
 `curl -fsSL https://get.docker.com -o get-docker.sh`<br>
 `sudo sh get-docker.sh`<br>

 **Add user to docker**<br>
 `sudo usermod -aG docker {user}`<br>
### Add Template
**Create Directories**<br>
 `git clone <repository-url> <target-directory>`<br>
 `cd /docker`<br>

 (You will get the whole directory, but this is ultimately like other templates, you just import the file structure premade. I think its better this way because i like to operate from VS CODE which I find to be better than portainer, however you can install portainer just as well)<br>
 
**Template Links**<br>
 |Architechture|OS|Repository URL|
 |:-----:|:----:|:-----------------:|
 |AMD64|Debian|https://github.com/Jarrodb22/docker/tree/main|

 You're done! Now just click App Templates and deploy applications!