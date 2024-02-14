+++
title = 'How to setup a VAC (Virtual Audio Cable) on Linux and use OBS with it'
date = 2024-02-13T23:13:03-08:00
draft = false
description = "Setting up a VAC (Virtual Audio Cable) on Linux to setup a virtual microphone. And applying OBS filters to virtual microphone"
categories = ["Linux", "OBS", "Sound"]
tags = ["Wireplumber", "Pipewire", "PulseAudio"]
+++

# Why?

Filters like noise supression only work when your recording. However, what if you need those filters when speaking live? Such as when NOT on Zoom or Discord. Those filters won't apply in that case. However if we use a virtual mic, we can!

## Create Virtual Mic

- Let's capture the sound from our actual mic, this would be called a `sink`

  ```bash
  pactl load-module module-null-sink sink_name=Source sink_properties=device.description="SinkForVirtualMic"
  ```

- Let's monitor that sink and direct it to the virtual mic, this would be called a `source`

  ```bash
  pactl load-module module-virtual-source source_name=VirtualMic master=Source.monitor
  ```

- Now in your __Sound__ options, you'll see `SinkForVirtualMic` from outputs and see `VirtualMic` from inputs

## Use Filters from OBS

- [Here](https://www.youtube.com/watch?v=Clcq7fk6L1k) is a youtube video that does a good explanation on using the OBS filters. He uses Windows here, but the setup is the same.

### Head into OBS

- Go into the properties of your actual microphone and switch to `Monitor Off` to `Monitor and Output`
  - You'll hear yourself when doing this

- Then go to OBS settings > Audio > Advanced > Click `Monitoring Device` and select `SinkForVirtualMic`
  - You'll no longer hear yourself, meaning we've redirected our input (voice) to this sink

- Select the filters you want and apply them your actual microphone. 

- Then select your __Virtual Mic__ for the input
  - Your input (voice) is now filtered!
