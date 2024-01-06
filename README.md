# Docker gphoto-sync
These are my personal notes on implementing gphoto-sync using docker compose.  I based my work on a few references - which I recommend you review if you want to reapply what I am doing.  These are my personal notes and not meant for mass consumption, but if this helps you - please feel free to use this as reference.  Your milage may vary so don't blame me if this does not work for you.



## Authenticating your Google Account

 1.    Create a new Google Cloud project
> Go to https://console.cloud.google.com/cloud-resource-manager and create a new project.

> For help with this step, see the Google Cloud documentation for creating a project https://cloud.google.com/resource-manager/docs/creating-managing-projects

 2.   Go to https://console.cloud.google.com/apis/library?project=_ select a project, then search for the Photos Library API and enable it for this project.

> The Google Cloud help for enabling APIs can be found here: https://cloud.google.com/apis/docs/enable-disable-apis

3.    Go to https://console.cloud.google.com/, make sure the correct project is selected from the top dropdown menu, then click on APIs & Services in the sidebar, and finally click Credentials in the sidebar. Create an Create OAuth client ID. When asked to choose the application type, select "Desktop App".

> You can see the complete procedure for setting up OAuth 2.0 with your new project on https://support.google.com/cloud/answer/6158849

4.    Once the client ID is created, download it as client_secret.json (so rename it, as it will have a longer name)

5.    Place the client_secret.json you just downloaded and renamed at /config

6.    Afterwards you need to sign into the application once which cannot be done headlessly (instead, run the "Syncing" command manually once)

7.    Afterwards you can call the "Syncing" command any time you wish, as long as the container is running (e.g. by using cron on your docker host).


## Docker Compose
```
version: '3'

services:
  gphotos-sync:
    container_name: gphotos-sync
    image: rix1337/docker-gphotos-sync:latest
    restart: unless-stopped
    volumes:
     - /path/to/config:/config:rw 
     - /path/to/download:/storage:rw 
```

## License
The docker compose file and my documentation are licenced under the MIT license.  Linked references are licensed per the linked files. (C) 2024 ViChanzo


## References
- https://www.linuxuprising.com/2019/06/how-to-backup-google-photos-to-your.html
- https://github.com/gilesknap/gphotos-sync
- https://hub.docker.com/r/rix1337/docker-gphotos-sync
- https://github.com/perkeep/gphotos-cdp
