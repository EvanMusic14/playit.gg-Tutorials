# playit.gg-auto-restarter
## Setup Uptime Kuma
- Create an `Uptime Kuma` monitor for an ip or url run through `playit.gg`
- Setup a notification for the monitor you created
  - Notification Type: `Webhook`
  - Post url: `http://localhost:3000/webhook` or if you have uptime kuma on a different system, change the url to the ip address of the system where you will create the webhook receiver
  - Request Body: `Preset - application/json`
  - Save

## Create node project
- Start your project in `/var/www/playit-webhook-receiver`
- Initialize your node project: `npm init -y`
- Make a single js file: `vim index.js`
- Paste code from github `index.js` file
  - The code listens on port 3000 for status message sent from `Uptime Kuma` and will restart the playit.gg service if the status is down

## Edit sudoers
This step is needed to allow the command to run in the index.js without requiring a password
- Edit the sudoers file using visudo: `sudo visudo`
- Add `ALL ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart playit.service` to the bottom of the file
  - This give anyone permission to run the systemctl restart command on the playit service

## Create a system service for you webhook receiver to run in
This will make sure that your receiver is always running on system startup and can easily be managed with `systemctl`
- Create a `.service` file: `vim playit-webhook-receiver.service`
- Paste contents from `webhook-receiver.service` in github
  - This comes with everything your service needs to run the index.js file
- Copy your `playit-webhook-receiver.service` to the services: `sudo cp playit-webhook-receiver.service /lib/systemd/system`
  - This places your services with all the other to allow it to be managed by the system
