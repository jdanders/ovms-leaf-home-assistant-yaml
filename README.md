# ovms-leaf-home-assistant-yaml
YAML config for MQTT config of OVMS for Nissan LEAF

To use, copy ovms.yaml to the same directory as your `configuration.yaml` file. Replace `[mqtt-user]/[CAR-ID]` with the appropriate values. The add the package to your list of packages under the main `homeassistant` section:

```yaml
homeassistant:
  packages:
    ovms: !include ovms.yaml
```

I've only ever used Home Assistant as a manual python install, so I'm not sure how installs work for other configurations.

# Features

These yaml files provide OVMS MQTT sensors, switches, buttons, and location tracking.

I've converted units to US-friendly units, and time measurements to reasonable units. I've also tried to select decent icons for most sensors.

Besides the various sensors, there are also switches and buttons. These use the console commands to perform various actions. You can add more buttons if there are other commands you'd like to run.

The location tracker can be added by adding the first automation in automations.yaml to your automations. The second automation will disable remote climate control when the car starts.

I have a smart plug (must be 15A tolerant!) connected to my 110V charger so HA can control the charging. With this set-up, I disable all the controls on charging within the car and leave it all to Home Assistant. I typically let it get charged based on the state of my solar supply, otherwise there are two override switches: one to charge to 80% despite solar power, and one to charge to 100%.

Finally, the ovms.entitylist.yaml can be used to add all the entities to the dashboard.

I gathered topics from OVMS source code, documentation and monitoring what's published. There are two types of commented out topics:
* `#` only: This is available but is something I didn't think would be useful to have in Home Assistant
* `# NO LEAF #`: This topic should be available according to documentation or code, but I haven't seen my LEAF publish to it

# Other resources

* https://github.com/mdallaire/ovms-hass-configs

This repo connects availability of everything to the connected state of the v3 server. This is useful if you want to know if data is valid. My approach is to add the event sensor to my Home Assistant page with a last updated indicator.

To add availability, add this to each sensor:

```yaml
  availability:
      topic: "ovms/[mqtt-user]/[CAR-ID]/metric/s/v3/connected"
      payload_available: "yes"
      payload_not_available: "no"
```

This might be helpful as well:
## Caveats

All of these sensors and automations depend on MQTT and the MQTT refresh settings of your OVMS.

Some sensors tend to update more frequently than other and as such you can see some incoherence in your sensors. This is not a Home-Assistant problem but a OVMS

If you use a SIM card with data you have to find a balance between frequent updates and data usage costs.
As a reference I use the following settings with my OVMS Server V3 configuration.

**This is for reference only, it works for me and the resulting cost is OK for me with my provider (which is not hologram.io). YOU are responsible for your settings and the resulting data cost**

```
Update intervals
...connected: 60
...idle: 600
...on: 15
...charging: 60
...awake: 15
...sendall: 600
```
