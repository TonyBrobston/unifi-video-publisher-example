# unifi-video-publisher-example

### How to test that this works:
  - Get Home Assistant up and running
  - Modify a log file
  - Tear everything down
  - Other things to consider

##### Get Home Assistant up and running:
  1. Clone this repo to your machine: `git clone git@github.com:TonyBrobston/unifi-video-publisher-example.git`
  2. Get into the directory: `cd unifi-video-publisher-example`
  3. Start the docker services: `docker-compose up` (these logs are worth reading through)
  4. Once all four docker services are up and running navigate to http://localhost:8123 (or substitute "localhost" for the ip of the machine these services are running on)
  5. Create a home assistant account

##### Modify a log file:
1. Observe the Notifications in the Menu Bar.
2. Add a log line to Unifi Video's log file: `docker exec -it unifi-video-publisher-example_unifi-video_1 bash -c "echo '1577042817.781 2019-12-22 13:26:57.781/CST: INFO   [uv.analytics.motion] [AnalyticsService] [FCECDAD8B870|House West] MotionEvent type:start event:28345 clock:10377014318 in AnalyticsEvtBus-11' >> /var/log/unifi-video/motion.log"`
3. You should now see a Motion Event notification.

##### Tear everything down:
  1. Hit `ctrl + c` on your running docker-compose window
  2. Run `docker-compose rm` to remove all containers created by this docker-compose

##### Other things to consider:
  1. You should change the Mqtt Broker username/password (`your_mqtt_broker_username_here` and `your_mqtt_broker_password_here` should not be used long term). This will involve changing a few things (all of these will use the same username/password):
      - This is used by Home Assistant to connect to the mqtt-broker: https://github.com/TonyBrobston/unifi-video-publisher-example/blob/master/home-assistant/secrets.yaml
      - This is used by the unifi-video-publisher service (`logs-to-mqtt-broker` tool) to connect to the Mqtt Broker https://github.com/TonyBrobston/unifi-video-publisher-example/blob/master/logs-to-mqtt-publisher/options.json#L23
      - This is used by the Mqtt Broker to create its own the username/password (this will be done differently): https://github.com/TonyBrobston/unifi-video-publisher-example/blob/master/mosquitto/config/passwordfile With the docker-compose still running you'll need to execute this command with your username/password substituted in: `docker exec unifi-video-publisher-example_mqtt-broker_1 ash -c "mosquitto_passwd -b /mosquitto/config/passwordfile your_mqtt_broker_username_here your_mqtt_broker_password_here"`
  2. An automation that notifies yourself about motion is not a meaningful action, maybe turn a light on?
  3. Read through the entirety of this repo and try to make sense of things, I did my best to keep it concise as to not lose the consumers. If you have confusion feel free to create an issue and describe the confusion clearly and I'll see what I can change to make it more clear.
