# unifi-video-publisher-example

### How to test that this works:
  - Get Home Assistant up and running
  - Subscribe Home Assistant to the Mqtt Broker
  - Setup an Automation
  - Publish an event to the Mqtt Broker
  - Tear everything down

##### Get Home Assistant up and running:
  1. Clone this repo to your machine: `git clone git@github.com:TonyBrobston/unifi-video-publisher-example.git`
  2. Get into the directory: `cd unifi-video-publisher-example`
  3. Start the docker services: `docker-compose up` (these logs are worth reading through)
  4. Once all four docker services are up and running navigate to http://localhost:8123 (or substitute "localhost" for the ip of the machine these services are running on)
  5. Create a home assistant account (these creds will only matter if this instance will be long-lived)

##### Subscribe Home Assistant to the Mqtt Broker
  1. Navigate to the menu > Configuration > Integrations > click the "+" to add a new Integration
  2. Choose "MQTT" and enter:
    - Broker: `mqtt-broker`
    - Port: `1883`
    - Username: `username`
    - Password: `password`
  3. Click "Submit" and then "Finish"

##### Setup an Automation
  1. Navigate to the menu > Configuration > Automations > click the "+" to add a new Automation
  2. Click "Skip"
  3. Name your Automation: `New Automation`
  4. Create a trigger:
    - Trigger type: `MQTT`
    - Topic: `motion/House West`
    - Payload: `start`
  5. Delete the Action
  6. Click the save icon
  7. Add the Action back:
    - Action type: `Call service`
    - Service: `automation.turn_off`
    - Name of automation to turn off: `automation.new_automation`
  8. Click the save icon
  9. Navigate back to the Automations page

##### Publish an event to the Mqtt Broker
  1. Assuming you're running in docker go to the command line and run: `docker exec -it unifi-video-publisher-example_unifi-video_1 bash`
  2. Now that you're inside this running container, while observing the Automations page, run: `echo "motion|House West|start" >> /var/log/unifi-video/motion.log`, you should see `New Automation` turn off.

##### Tear everything down
  1. Hit `ctrl + c` on your running docker-compose window
  2. Run `docker-compose rm` to remove all containers created by this docker-compose
