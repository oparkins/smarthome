# Smart Home Ansible Setup
Ansible Files for a Smart Home Setup

It is intended to be a good starting place with setting up [Home
Assistant](https://www.home-assistant.io/) (HASS) and a MQTT broker. The idea
was to have a repo that did almost all of the setup work for you, but I had to
recently move and was not able to add all the features I wanted. Missing
features are:
- [Nginx setup](https://github.com/oparkins/smarthome/issues/1)
- [ddclient](https://github.com/oparkins/smarthome/issues/2)
- [LetsEncrypt Setup](https://github.com/oparkins/smarthome/issues/3)
- [Auto connect MQTT and HASS](https://github.com/oparkins/smarthome/issues/4)
- [Auto compile Tasmota](https://github.com/oparkins/smarthome/issues/5)

Once I am done moving around (eta. June), I will flush out this repository.

## Features
The docker-compose file will automatically install:
- Docker
- docker-compose
- Eclipse Mosquitto Server
- Home Assistant

**This has been only tested on a Raspberry Pi 4 at the moment.**

## Usage

Setup your system and be sure that you have sudo privileges. This Ansible
playbook is meant to run on a clean system that isn't responsible for anything
else. You will need to have `ssh` enabled.

Edit the `inventory.yml` file to have the settings that you want. Then run the
following command:

```
ansible-playbook site.yml -K  -i inventory.yml
```

The `-K` will ask for your sudo password so that Ansible can do administrative
actions. If you need to change the user account, use the `-u` flag.

After the script as ran, you will need to configure some of MQTT. Once I moved I
will automate this part, but for now it has to be done manually.

### Add HASS MQTT Password

To add a MQTT password for HASS, go to the machine and type:
```
# docker exec -it srv_mqtt_1  /bin/sh
# cd /mosquitto
# mosquitto_passwd passwd hass
```

Save that password as you will need it next.

### Configure HASS to connect to MQTT

Go to `<your server IP>:8123` and go through the setup dialogs. Once you reach
the integrations part, search for MQTT. Then use the following settings:

```
Broker: localhost
Port: 1883
Username: hass
Password: <Your password saved above>
```

### Adding Devices

I'll expand on this part when I setup my house again, but you can follow these
excellent instructions from Matt Virus:
http://hackspace.io/projects/projects-smartplugs/