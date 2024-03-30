+++
title = 'How to connect to the wifi via the terminal'
date = 2024-03-30T16:16:14Z
draft = false
+++


## Intro

Today, I was connecting to the wifi in an Airbnb, and had to connect to the wifi. Because of my infinite wisdom, I didn't have a gui app to connect to the wifi, so I had to use the terminal. Because of this, you get a free lesson, so good for you!

## How to connect

1. Enable iwd
    ```bash
    systemctl enable iwd
    ```

2. Run iwctl
    ```bash
    iwctl
    ```
3. Connect to the network
    1. Scan for networks

        You should see something like this:

        ```
        [iwd]# station wlan0 get-networks

                                     Available networks                             
        --------------------------------------------------------------------------------
              Network name                      Security            Signal
        --------------------------------------------------------------------------------
          >   Tropical Riverhouse               psk                 ****
              Crazyinlove                       psk                 ****
              Peacock                           psk                 ****

        ```

    2. Connect to the network
        
        ```
        [iwd]# station wlan0 connect "Tropical Riverhouse"
        Type the network passphrase for Tropical Riverhouse psk.
        Passphrase: *********  <-- password here
        Terminate
        [iwd]#
        ```

    3. Done!
        
        Try pinging something e.g. ```bash ping archlinux.org```

