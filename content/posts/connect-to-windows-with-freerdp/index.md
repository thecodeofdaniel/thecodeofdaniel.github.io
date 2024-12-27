+++
title = 'Remotely Connect to Windows within Linux'
date = 2024-12-27T11:23:33-08:00
draft = false
categories = ["Linux", "Windows"]
+++

When wanting to connect to Windows, there is `Remote Desktop Client` for Windows
and `Microsoft Remote Desktop` for Mac. However, what about for Linux? In this
tutorial I'll be using [FreeRDP](https://github.com/FreeRDP/FreeRDP), a command
line tool, to do so!

## Installation

Install the correct package based on you display server. You can find out using
this command:

```bash
echo $XDG_SESSION_TYPE
```

If you're using Ubuntu/Debian, you can install it using `apt`.

- If you're on X11 use:

  ```bash
  sudo apt install freedrp2-x11
  ```

- If you're on Wayland use:

  ```bash
  sudo apt install freedrp2-wayland
  ```

## Connecting to Windows

Once it's installed, we can connect to it like this:

```bash
xfreedrp /u:<user_name> /p:<password> /v:<public_ip_of_windows_vm> /f
```

- The `/f` command is for fullscreen.

However, if you're logging into a domain. Use the following

```bash
xfreedrp /u:<domain>\\<user_name> /p:<password> /v:<public_ip_of_windows_vm> /f
```

If you get a prompt saying `Certificate Name Mismatch` as below. Simply type
`Y` to trust the certificate.

![FreeRDP Image](./imgs/01.png "FreeRDP Command Line")

To get a list of other options following the docs [here](<https://github.com/FreeRDP/FreeRDP/wiki/CommandLineInterface-(possibly-not-up-to-date,-check-application-help-text-for-most-up-to-date-version)#other-options>)
