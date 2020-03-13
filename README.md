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
  1. username and password should not be used long term
  2. An automation that notifys is not a meaningful action, maybe turn a light on?
  3. Read through the entirety of this repo and try to make sense of things, I did my best to keep it concise as to not lose the consumers. If you have confusion feel free to create an issue and describe the confusion clearly and I'll see what I can change to make it more clear.
