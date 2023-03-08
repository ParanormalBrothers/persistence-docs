# Run in the background

## Run the Node in the Background

While you've probably used the `persistenceCore start` command in a terminal, you might question yourself what happens if you exit the window, stop the node by mistake, or need to restart your server. The answer is **services**. Imagine a service as a container for processes. It allows the Persistence Core-1 node to run in the background while providing us with access to logs. Not to mention it's starting automatically on system (re)boot. In this section we will cover `systemd`.

Running a service using `systemd` is straightforward. While `systemd` itself provides many configuration options, we only need a few to run the Persistence Core-1 node. Let's get started.

1. Since `systemd` is installed by default, there are no installation steps.
2.  First, we need to create a service file in `/etc/systemd/system`. Let's name it `core-1.service`.

    ```bash
    touch /etc/systemd/system/core-1.service # create the file
    nano /etc/systemd/system/core-1.service # open the file (to edit it)
    ```
3.  Now, with the file created and open, we need to populate it with the correct options to run the Persistence Core-1 Node.

    ```bash
    [Unit]
    Description=Persistence Mainnet Node # description of the process running inside the service
    After=network-online.target # wait for internet connection before starting the process

    [Service]
    User=$USER # your system user name
    ExecStart=$(which persistenceCore) start # command to execute (this will run the node)
    Restart=always # always maintain the service running by restarting it in case it crashes
    RestartSec=3 # seconds to wait before restarting
    LimitNOFILE=4096 # maximum number of processes allowed to run in parallel

    [Install]
    WantedBy=multi-user.target # a special target unit for setting up a multi-user system
    ```
4.  Next, we need to reload systemd and enable the service.

    ```bash
    systemctl daemon-reload
    systemctl enable core-1.service
    # output: Created symlink /etc/systemd/system/multi-user.target.wants/core-1.service → /etc/systemd/system/core-1.service
    ```
5.  All that's left to do is to start it.

    ```bash
    systemctl start core-1.service
    ```
6.  Although you may think nothing has happened, the service is running in the background. To access its logs and see if it's running as it should use the following command:

    ```bash
    journalctl -u core-1.service -f
    ```
7.  To check the status of the service, you can run:

    ```bash
    service core-1.service status
    ```

**Additional commands:**

```bash
systemctl stop core-1.service
systemctl restart core-1.service
```
