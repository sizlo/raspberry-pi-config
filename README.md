# raspberry-pi-config

Config for all the apps I run on my Pi. This is all intended to be used on a Raspberry Pi 4 Model B 8GB.

> [!NOTE]  
> All `docker compose` commands in this README assume you are runing from the root of this repository. You can use the `-f` flag if running from somewhere else, e.g `docker compose -f $HOME/raspberry-pi-config/docker-compose.yml pull` 

## Pi setup

- Install OS on Raspberry Pi with [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
    - I used Raspberry Pi OS Lite (64-bit) - This is a lightweight headless OS
    - Configure Raspberry Pi Imager to apply OS customization settings
        - Enable ssh with password authentication
        - Set hostname to `pi`
        - Create a username and password (note to author: these are stored in Bitwarden if you've forgotten them)
        - Configure Wi-Fi network if required (note to auther: you did not do this, your Pi is connected with ethernet)
- Boot up Raspberry Pi and ssh in: `ssh <user>@pi.local`
- Install Docker
    - Run the install script: `curl -sSL https://get.docker.com/ | sudo sh`
    - Allow non root user to run docker: `sudo usermod -aG docker <user>`
- Clone this repo
- Set up environment files for secrets of each app
    - [vaillant-poller](https://github.com/sizlo/vaillant-poller?tab=readme-ov-file#run) - `$HOME/vaillant-poller-secrets.env`
    - [chores](https://github.com/sizlo/chores?tab=readme-ov-file#required-environment-variables-for-running-on-a-raspberry-pi) - `$HOME/chores-secrets.env`
- Run the apps: `docker compose up -d`
    - Because `restart: always` is configured in the compose file, the apps will be restarted if they crash, or the Pi is rebooted. The apps can be shutdown with: `docker compose down`

## App updates

Update all apps: `docker compose pull`

Update specific app: `docker compose pull <app name>`

After being pulled, apps must be restarted with `docker compose down [app name]` and `docker compose up [app name] -d`.
