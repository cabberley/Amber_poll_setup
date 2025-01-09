# Amber_poll_setup
Info to setup Amber Poll Container


# Beta Testing atm

1. Edit your Home Assistant
- configuration.yaml and add "api:" on a line by iteself to enable the Home Assistant RestAPI interface.
- copy the "amber_poll_package.yaml" to your Home Assistant "/packages" directory, (edit the names of the sensors to suit your desire!, but ensure you have a similar "amber_5min_feed_in_slot_0" structure, the posts to Home asistant will assume the last character is 0-11 for the 12 * 5 min time slots.
3. On your Docker Host:
- create a config directory for your container, i.e. "/configs/amber_poll"
- Create a "data" and "config" directory under that config directory
- Copy the config.json file from this repository and edit with your Home assistant, Amber and Docker details. Take note of the example "amber_5min_general_price_slots" values, the code will iterate a 0 to 11 for the last character.
- Your sensors in your amber_poll_package and the config.json should align!

Create a new docker container and pull the image.
- dockerhub cabberley1/amber_poll:latest
- at a minium bind/map the paths:
  /opt/amber/config to the config directory you created earlier
  /opt/amber/data to the data directory you created above

# Note 1:
The config Json file sets the timing of attempts to get the latest 5 minute prices from Amber. In the example you will see it tries timne the containers time = the seconds in the second field when the minutes also match. It uses a CRON like pattern. Once the App has confirmed an actual value for the current 5min it will stop polling the Amber API until the next 5 min cycle starts.

# Note 2: 
The Amber RestAPIs that we use for all the data requests do have hard limits. Currently, Amber restricts you to 50 RestAPI calls each 5 minutes. So be aware of this particularly if you are running multiple systems requesting and hitting the API!!
