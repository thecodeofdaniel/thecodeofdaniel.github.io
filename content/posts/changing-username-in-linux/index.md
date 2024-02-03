+++
title = 'Changing Username in Linux'
date = 2024-02-03T03:18:37-08:00
draft = false
+++

# Before You begin!

- You should login as another user or become root
  - To create a new user

    ```
    # usermod -aG sudo <other_user>
    ```

## Let's get Started!

- Change the username

  ```
  # usermod -l <new_name> <old_name>
  ```

- Change the groups `<old_name>` to new username

  ```
  # groupmod -n <new_name> <old_name>
  ```

  - Confirm this by looking into old home directory of `<old_name>`

    ```
    ls -ld /home/<old_name>
    ```

  - The owner and group owner should now be owned by `<new_name>`

## Change Username for Home Directory

- Your home directory still may have previous name, here's the fix...

  ```
  # usermod -d /home/<new_name> -m <new_name>
  ```

  - Check to confirm

    ```
    ls -ld /home/<new_name>
    ```

## Edit Username in GDM login screen

- When you log back in, you may still have previous name, here's the fix...

  ```
  # usermod -c "<new_name>" <new_name>
  ```

### And we're DONE!