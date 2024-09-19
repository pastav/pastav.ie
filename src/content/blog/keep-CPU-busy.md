---
title: Keep CPU busy with a Systemd service
author: Pastav
pubDatetime: 2024-09-19T18:17:19Z
slug: keep-cpu-busy-stressng
featured: true
draft: false
tags:
  - stress-ng
  - linux
  - OCI
ogImage: ""
description: How to create a Systemd service to keep processor busy in Linux
---

In order to test/burn-in your server, we need to keep the processor utilization at a specific percentage for a long period of time. Running a solo script to keep the system busy can lead to termination of the script randomly and thus failure of the job.

Here, we’ll create a bash script that keeps the processor busy at a specific percentage and run the script as a service which restarts whenever the service fails. This ensures that your processor will always be utilized at the given percentage, so you can test/burn-in or keep your server busy so that cloud providers do not reclaim it 🙂

Stress-ng is an amazing library which can be used to stress a number of components of a server, and is used to here keep the processor busy. If you don’t have it, you can install it using:
```console
sudo apt install stress-ng
```
Here are the steps:

1. Create a bash script with the following code and save it as `busy.sh`:
    ```bash
    #!/bin/bash
    stress-ng --cpu 0 --cpu-load 30 -t 0
    ```
    You can edit the `--cpu-load` to keep your desired CPU load percentage. We’re going with 30% here.

2. Make the bash script executable by running the following command on your terminal:
    ```bash
    chmod +x busy.sh
    ```
3. Create a new service in Systemd in this directory `/etc/systemd/system`:
    ```bash
    [Unit]
    Description=keep CPU busy
    [Service]
    Type=simple
    User=root
    Group=root
    ExecStart=/home/ubuntu/busy.sh #path to the created busy.sh
    Restart=on-failure
    RestartSec=5
    [Install]
    WantedBy=multi-user.target
    ```
4. Reload the service files to include the newly created service:
    ```bash
    sudo systemctl daemon-reload
    ```

5. Start your keep-busy service:
    ```bash
    sudo systemctl start busy.service
    ```

6. Check the status of your service:
    ```bash
    sudo systemctl status busy.service
    ```

7. To start your service on every reboot:
    ```bash
    sudo systemctl enable busy.service
    ```

8. To stop your service:
    ```bash
    sudo systemctl stop busy.service
    ```

9. To disable starting of the service on every reboot:
    ```bash
    sudo systemctl disable busy.service
    ```

You can use `htop` command to check if stress-ng is running at the specified CPU percentage utilization.

![something](@assets/images/busy_htop.png)

Enjoy keeping busy and don’t let cloud providers claim your server.

Best,\
Pastav

