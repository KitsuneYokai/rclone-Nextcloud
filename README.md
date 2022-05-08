# rclone-Nextcloud
This is a guide to mount your OneDrive, GoogleDrive ...  via rclone to your Nextcloud Instance.

### 1. Connect to your server
------------
Type `ssh <user>@<serverip>` into your terminal, or use [PuTTY](https://www.putty.org/ "PuTTY") to ssh into your nextcloud server.

### 2. Installing the requirements
------------
- Install and enable the **External storage support** app in your nextcloud dashboard
- Type `sudo apt-get update && sudo apt-get upgrade -y` inside your terminal to update your server

- Install rclone with `sudo apt-get install rclone`

### 3. Configure rclone
------------
- Type `rclone config` inside your terminal.
Something like this should appear:
```
xxx@nextcloud ~$ rclone config
2022/05/08 18:35:04 NOTICE: Config file "/home/xxx/.config/rclone/rclone.conf" not found - using defaults
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q>
```
Now write `n` inside your config to create a new config.

- Enter your desired name, for example:
```
new> onedrive
```
Select a provider from the list.
```
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
 1 / 1Fichier
   	\ (fichier)
 2 / Akamai NetStorage
   	\ (netstorage)
 3 / Alias for an existing remote
   	\ (alias)
 4 / Amazon Drive
  	 \ (amazon cloud drive)
 5 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, Ceph, China Mobile, Digital Ocean, Dreamhost, IBM COS, Lyve Cloud, Minio, Netease, RackCorp, Scaleway, SeaweedFS, StackPath, Storj, Tencent COS and Wasabi
   	\ (s3)
 6 / Backblaze B2
   	\ (b2)
 7 / Better checksums for other remotes
   	\ (hasher)
 8 / Box
   	\ (box)
 9 / Cache a remote
   	\ (cache)
10 / Citrix Sharefile
   	\ (sharefile)
11 / Compress a remote
   	\ (compress)
12 / Dropbox
   	\ (dropbox)
13 / Encrypt/Decrypt a remote
   	\ (crypt)
14 / Enterprise File Fabric
   	\ (filefabric)
15 / FTP Connection
   	\ (ftp)
16 / Google Cloud Storage (this is not Google Drive)
   	\ (google cloud storage)
17 / Google Drive
   	\ (drive)
18 / Google Photos
   	\ (google photos)
19 / Hadoop distributed file system
   	\ (hdfs)
20 / Hubic
   	\ (hubic)
21 / In memory object storage system.
   	\ (memory)
22 / Jottacloud
   	\ (jottacloud)
23 / Koofr, Digi Storage and other Koofr-compatible storage providers
   	\ (koofr)
24 / Local Disk
   	\ (local)
25 / Mail.ru Cloud
   	\ (mailru)
26 / Mega
   	\ (mega)
27 / Microsoft Azure Blob Storage
   	\ (azureblob)
28 / Microsoft OneDrive
   	\ (onedrive)
29 / OpenDrive
   	\ (opendrive)
30 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   	\ (swift)
31 / Pcloud
   	\ (pcloud)
32 / Put.io
   	\ (putio)
33 / QingCloud Object Storage
   	\ (qingstor)
34 / SSH/SFTP Connection
   	\ (sftp)
35 / Sia Decentralized Cloud
   	\ (sia)
36 / Storj Decentralized Cloud Storage
   	\ (storj)
37 / Sugarsync
   	\ (sugarsync)
38 / Transparently chunk/split large files
   	\ (chunker)
39 / Union merges the contents of several upstream fs
   	\ (union)
40 / Uptobox
   	\ (uptobox)
41 / Webdav
   	\ (webdav)
42 / Yandex Disk
   	\ (yandex)
43 / Zoho
   	\ (zoho)
44 / http Connection
   	\ (http)
45 / premiumize.me
   	\ (premiumizeme)
46 / seafile
   	\ (seafile)
Storage> 
```
I am going to choose **Microsoft OneDrive** for this example, so i enter 28.
```
Storage> 28
```
Now rclone asks for a `client_id` / `client_secret` and a `region`. We can just leave those empty by pressing `ENTER` thrice.
After that rclone asks you if you want to edit the config in advanced mode.
We can simply press `n` and continue.
```
Edit advanced config?
y) Yes
n) No (default)
y/n> n
```
- Now its gonna get a little bit tricky, since the server is most likely opperating headless, we need to answer with `n` at the next question.
```
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
*
y) Yes (default)
n) No
y/n> n
```
Something like this should apear:
```
Option config_token.
For this to work, you will need rclone available on a machine that has
a web browser available.
For more help and alternate methods see: https://rclone.org/remote_setup/
Execute the following on the machine with the web browser (same rclone
version recommended):
    rclone authorize "onedrive"
Then paste the result.
Enter a value.
config_token> 
```
Now you have to get to another pc/laptop and run `rclone authorize "onedrive"` in order to get a login token (of course you need to have rclone installed on this machine aswell).
After you typed `rclone authorize "onedrive"` a new Tab will open and asks you to log in.
After you logged in to your account, you will see a white screen displaying **Success!**
If you see the text you can return to the rclone terminal and copy the displayed token. The token looks something like this: 
```
{"access_token":"EwHKkOboBojB97g9bhi6CVT8C7cr7iCiVh9VkJ5eqHMItbdO+xJyrsPbfcWB3dCnVLlfQkRhYAMl7q5ufHFy7gHhYf1utUdUbX7p+lI57P4MRVYfuBuNlvIKmgMnGiRKx2mpvuXRKLwUCxYM4opA0T1yJRW7M2ATthYKXc+sStw5qEPEWXUe5JZBlBuQLHKCUdi9OxULGHJRrSwToMsRrTwl09A4iy0qPiARGIEmJ/b6k4hXeWJRtIRgCMcpz+8UKU0sykQg1pgKFzIAhdCWOIkV4L6ctx0MxiyUL8Kc0ELlH9svgJt6mKDo8h0r4jVB57Bl/x9lDWkgO1sob4QHPVt3dt8f7qnq9SoDpO/tN2TKE67UxNU3Jr2K3+OoMcR+lpHZQKzGYN3jhwaF+nK96+3v3VduLeYx3ZTgT5DD89Ca1a681CdwbdF4REP1kNca+oByKnxRuO4hntCDi/GFGBcadW0qTH0wO8Yyjt5Yp3OLoaiSr1iaQrBAM1a3hfOAbQVA7BbV5iTbsC6JSqN9/n01CgZx4mGcwdfyBQPcWIhjFCxXb+dX4SPZg3gC","token_type":"JOBjOjs08Z0s8","refresh_token":"M.nxmGwcLfmBWp8EuDQererervfdvConojN8ZUb97bohLSboWU77ncbkjJY8Pi8Me6XmTSjZJowsZRHfOxoLKASpbbPKtBEYDK6TpEvXIuBDan9wBzZNM8ps$","expiry":"2022-05-08T20:09:38.585256+02:00"}
```
Now paste your token inside the Nextcloud terminal.
If everything went smoothly this should appear:
```
Choose a number from below, or type in an existing value
 1 / OneDrive Personal or Business
   	\ "onedrive"
 2 / Root Sharepoint site
   	\ "sharepoint"
 3 / Type in driveID
  	\ "driveid"
 4 / Type in SiteID
   	\ "siteid"
 5 / Search a Sharepoint site
   	\ "search"
Your choice> 
```
You most likely want to use option `1 / OneDrive Personal or Business`, so press 1 and continue.

- Next you get asked to select a drive. You most likely only have 1 drive, so choose 0. If you have more then 1drive listed, then select the drive you want to connect to.
```
Found 1 drives, please select the one you want to use:
0:  (personal) id=H75S8js9778GX083
Chose drive to use:>  0
```
Next rclone may find the root of your drive.
Just press `y` to continue.
```
Found drive 'root' of type 'personal', URL: https://onedrive.live.com/?cid=H75S8js9778GX083
Is that okay?
y) Yes
n) No
y/n>  y
```
Now check if the config is correct and click `y` one last time.
```
#####################################
[onedrive]
token ={"access_token":"EwHKkOboBojB97g9bhi6CVT8C7cr7iCiVh9VkJ5eqHMItbdO+xJyrsPbfcWB3dCnVLlfQkRhYAMl7q5ufHFy7gHhYf1utUdUbX7p+lI57P4MRVYfuBuNlvIKmgMnGiRKx2mpvuXRKLwUCxYM4opA0T1yJRW7M2ATthYKXc+sStw5qEPEWXUe5JZBlBuQLHKCUdi9OxULGHJRrSwToMsRrTwl09A4iy0qPiARGIEmJ/b6k4hXeWJRtIRgCMcpz+8UKU0sykQg1pgKFzIAhdCWOIkV4L6ctx0MxiyUL8Kc0ELlH9svgJt6mKDo8h0r4jVB57Bl/x9lDWkgO1sob4QHPVt3dt8f7qnq9SoDpO/tN2TKE67UxNU3Jr2K3+OoMcR+lpHZQKzGYN3jhwaF+nK96+3v3VduLeYx3ZTgT5DD89Ca1a681CdwbdF4REP1kNca+oByKnxRuO4hntCDi/GFGBcadW0qTH0wO8Yyjt5Yp3OLoaiSr1iaQrBAM1a3hfOAbQVA7BbV5iTbsC6JSqN9/n01CgZx4mGcwdfyBQPcWIhjFCxXb+dX4SPZg3gC","token_type":"JOBjOjs08Z0s8","refresh_token":"M.nxmGwcLfmBWp8EuDQererervfdvConojN8ZUb97bohLSboWU77ncbkjJY8Pi8Me6XmTSjZJowsZRHfOxoLKASpbbPKtBEYDK6TpEvXIuBDan9wBzZNM8ps$","expiry":"2022-05-08T20:09:38.585256+02:00"}
drive_id = H75S8js9778GX083
drive_type = personal
#####################################
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```
Congratulations you configured rclone 🎉

### 4. Mount the drive
------------
- Type `sudo mkdir /mnt/Onedrive` inside your terminal, to create a new folder.

Next we are goint to make rclone run as a service
- Type `sudo nano /etc/systemd/system/rclone.service` and paste the code.
```
[Unit]
Description=OneDrive(rclone)
Wants=network-online.target
After=network.target network-online.target

[Service]
Type=notify
Restart=on-failure
ExecStart=/usr/bin/rclone --vfs-cache-mode writes mount <rclone_config_name>: <rclone_mout_path> --config /home/<username>/.config/rclone/rclone.conf>
ExecStop=fusermount -u <rclone_mout_path>

[Install]
WantedBy=default.target
```
Please edit `ExecStart` and `ExecStop` and replace `<rclone_config_name/rclone_mout_path/username>` also dont forget to remove the `< >` from rclone.service

||meaning|
| ------------ | ------------ |
|`rclone_config_name`|The name of the config you gave your drive when you created the config in rclone (e.x `onedrive`)|
|`rclone_mout_path`|This is the path where we want the files to be (e.x `/mnt/OneDrive`)|
|`username`|This is the username you used to ssh into your server. If you used the `root` user for whatever reason you have to change the whole path after `--config` to `/root/.config/rclone/rclone.conf` |

- After you save the edited file using `CRTL + X`
Type `sudo systemctl enable rclone && sudo systemctl start rclone`
If everything is altight you should get an output similar to this from `systemctl status rclone`

```
* rclone.service - OneDrive(rclone)
     Loaded: loaded (/etc/systemd/system/rclone.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2022-05-08 16:14:36 UTC; 2h 3min ago
   Main PID: 2774 (rclone)
      Tasks: 16 (limit: 77005)
     Memory: 183.1M
        CPU: 9min 10.249s
     CGroup: /system.slice/rclone.service
             `-2774 /usr/bin/rclone --vfs-cache-mode writes mount onedrive: /mnt/OneDrive --config /home/xxx/.config/rclone/r>

May 08 16:14:36 nextcloud systemd[1]: Starting OneDrive (rclone mount)...
May 08 16:14:36 nextcloud systemd[1]: Started OneDrive (rclone mount).
```

If you see that the rclone service is running fine, your files should be mounted over at `/mnt/OneDrive`

### 5. Add the drive to nextcloud
------------
- Head over to `Settings>External Storage` in your nextcloud panel

- Add your storage like shown in the picture below (keine=none).

![](https://raw.githubusercontent.com/KitsuneYokai/rclone-Nextcloud/main/setup_storage_NC.png?token=GHSAT0AAAAAABUIE62TAVIQJEUHM42UUWRSYTYBSGA)

------------
that should be it. 
If there are any questions just open an issue and i´ll see what i can do to help. Otherwise you can reach me through discord `KitsuneYokai#7746`
