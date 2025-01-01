+++
title = 'Disable Audio Devices in Pipewire/Wireplumber on Linux'
date = 2024-02-08T05:55:15-08:00
draft = false
description = "How to disable output and input devices in Pipewire/Wireplumber on Linux"
categories = ["Linux", "Audio"]
tags = ["Wireplumber", "Pipewire"]
+++

## Find the device name

List the devices for handling audio input/output

```bash
pactl list cards short
```

Find the **name** of the current audio input/output YOUR using

```bash
wpctl status
```

- It will be listed at the bottom
- To get more info on the audio inputs/outputs (sources/sinks)

  ```bash
  wpctl inspect <id_value>
  ```

## Create the config file

Once you've found the device, create a configuration file in
`~/.config/wireplumber/main.lua.d`.

If it doesn't exist, run

```bash
mkdir -p ~/config/wireplumber/main.lua.d/
```

Name the file `<value>-disable-devices.lua` with the value not conflicting
others in that directory

```bash
touch  ~/config/wireplumber/main.lua.d/<value>-disable-devices.lua
```

## Write the code

Here's how I disabled my devices

```lua
-- Disables HDMI
hdmi = {
  matches = {
    {
      { "device.name", "equals", "alsa_card.pci-0000_c5_00.1" },
    },
  },
  apply_properties = {
    ["device.disabled"] = true,
  },
}

-- Disables audio jack
audioJack = {
  matches = {
    {
      { "device.name", "equals", "alsa_card.pci-0000_c5_00.6" },
    },
  },
  apply_properties = {
    ["device.disabled"] = true;
  },
}
```

Here's how I disabled one of my outputs from my device (Audio Interface)

```lua
-- Disables digital output from Audio Interface
scarletOutput = {
  matches = {
    {
      { "node.name", "equals", "alsa_output.usb-Focusrite_Scarlett_Solo_USB_Y7ANZA324ED720-00.iec958-stereo" },
    },
  },
  apply_properties = {
    ["node.disabled"] = true;
  },
}
```

Here's how to apply those rules

```lua
table.insert(alsa_monitor.rules, hdmi)
table.insert(alsa_monitor.rules, audioJack)
table.insert(alsa_monitor.rules, scarletOutput)
```

## Run the configuration

```bash
systemctl --user restart wireplumber
```

Now you don't have to see those devices from your sound settings! :D
