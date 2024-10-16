+++
title = 'How to run and create desktop entries for AppImages'
date = 2024-10-14T15:34:56-07:00
draft = false
description = "How to run and create desktop entries for AppImages on Linux"
categories = ["Linux"]
tags = ["AppImage", "Desktop Entry"]
thumbnailAlt = "AppImage Icon"
+++

Linux offers various methods for installing software packages. While many
applications are available through your distribution's official repositories,
alternative package formats like Snap, Flatpak, and AppImage provide additional
options. In this tutorial, we'll focus on AppImages, demonstrating how to
install the `Cursor` application and create a desktop shortcut for easy access.

## Install and run the appImage

Install the package and make the file executable.

```bash
chmod +x cursor-<foo>.AppImage
```

Then run the image.

```bash
./cursor-<foo>.AppImage
```

- or double click on the file within your file manager.

And that's it! Now let's make a shortcut to that file.

## Create a desktop shortcut

Download the following image. This will be the icon for our app.

<img src="./cursor.png" alt="image info" width="100" />

Move the appimage and icon to the `/opt/cursor` directory.

```bash
sudo mkdir /opt/cursor
sudo mv ~/Downloads/cursor-<foo>.AppImage /opt/cursor/cursor.appimage
sudo mv ~/Downloads/cursor.png /opt/cursor/cursor.png
```

Create the file for the desktop entry.

```bash
sudo touch /usr/share/applications/cursor.desktop
```

Paste in the following to the file.

```bash
[Desktop Entry]
Name=Cursor
Exec=/opt/cursor/cursor.appimage
Icon=/opt/cursor/cursor.png
Type=Application
Categories=Development;
```

Now you should be able to find the app in your applications menu!

## Create a command within the terminal

Create a link from appimage location to the bin directory.

```bash
sudo ln -s /opt/cursor/cursor.appimage /usr/local/bin/cursor
```

Now there's an issue, if you close the terminal from where you run the command,
the application will close. In order to fix this we need to run this command.

```bash
setsid cursor <path_to_directory> &
```

Now you'll be able to close the terminal and the application will still run.

### Create an alias

You can run the command above each time or create an alias inside your
`.<bash|zsh>rc` file.

```bash
cursor() {
  if [ $# -eq 0 ]; then
    echo "Usage: cursor <path_to_directory>"
    return 1
  fi
  setsid /usr/local/bin/cursor "$@" &
}
```

- You can name the function differently if you plan to use the cursor appimage
  itself.
