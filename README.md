# gcp-bitwarden-cloudflared

Google Cloud Platform VM instance with Container Optimized OS
Bitwarden with rclone backups to Google Drive

```shell
gcloud compute instances create bitwarden \
  --machine-type e2-micro \
  --zone us-central1-a \
  --image-project cos-cloud \
  --image-family cos-stable \
  --boot-disk-size=30GB \
  --scopes compute-rw
```

SSH to VM instance

Make folders
```shell
mkdir -p $HOME/backups
mkdir -p $HOME/rclone
mkdir -p $HOME/docker/bitwarden/data
```

Copy/Create rclone.conf for Google Drive in $HOME/rclone
```shell
chmod 0600 $HOME/rclone/rclone.conf
```

```shell
cd $HOME
git clone https://github.com/krimil/gcp-bitwarden-cloudflared.git .
cd gcp-bitwarden-cloudflared
cp .env-dist .env
```

Create cloudflared tunnel in Cloudflare Zero Trust dashboard.  Add token to .env



## Extra steps
From within your compute vm console, type the command toolbox. From within toolbox, find the utilities folder within bitwarden_gcloud. toolbox mounts the host filesystem under /media/root, so go there to find the folder. It will likely be in /media/root/home/<google account name>/gcp-bitwarden-cloudflared/setup - change directory to that folder.

Next, use gcloud to add the reboot-on-update.sh script to your vm's boot script metadata with the add-metadata command:
```shell
gcloud compute instances add-metadata bitwarden-rs --zone=us-central1-a --metadata-from-file startup-script=reboot-on-update.sh
```

