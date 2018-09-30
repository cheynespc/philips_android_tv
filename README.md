# philips_android_tv
Tools to control Philips 2016 Android TVs

The API is roughly the same as JointSpace (http://jointspace.sourceforge.net/) with the following
differences:

* Uses HTTPS over port 1926
* Uses /6/ instead of /1/ as the API path
* All calls have to have digest auth to be successful
* An initial pairing to determine the user/pass is required
* Channel and Source methods aren't available

Channel status and changes are performed using a different protocol that isn't described here.

The tool here will do the initial pairing. The credentials can then be used in a standard way
(digest) in other tools.

Cheynespc EDIT Below


OK Everyone after hours and hours and hours of learning,

I have successfully used this post to connect my Philips TV (Model: 43PUT6801/79 Man: June 2017) to my Home Assistant installation.

Achieved Results
With this custom component I am able to connect the TV via a network connection, and am able to view what station it is on and what channel it is, I can also control the volume and power on/off on the TV as well as change the channel source to any other live TV station.

Challenges
Some of the biggest challenges I faced when attempting this was, initially, that I had no knowledge of Python or Linux really for that matter.

My System
My current setup is a Raspberry Pi 3B running Hassio and Home assistant (HA) version 0.78 at the time of writing this.

Steps and Sources of Information:

I was interested in having my Philips TV communicate with Home Assistant.

This was a exciting feature as my current setup has xBox One, ps4, and kodi- libreelec media players sending what is playing to HA. The only device I had one way communication with was the Philips TV, as I was using an IR blaster to control this. Using a "Broadlink IR RM3 mini" and using this home assistant component. Using this I was able to control the TV as if I was using a remote control. However this was not telling my media player to current state of the TV which all other media devices were able to do. There had to be a solution.

See below for my Broadlink media player configuration.yaml before getting this custom component to work:

Media_player:

- platform: broadlink
  name: Philips IR Control
  host: 10.0.4.122
  mac: '78:0f:77:47:90:9e'
  ircodes_ini: 'broadlink_media_codes/philips.ini'
  ping_host: 10.0.4.123
I tried the original joint space API with the Philips TV but my Tv was not supported as I purchased it 2017 and it is Android TV. Found this link in feature requests Philips Android TV component

Reading through this forum I noticed many people were having similar issues with various 2016+ Philips TVs.

The following steps provide this to work.

Gaining a username and password from the Philips 2016+ TV (Android)

Read through the forums and found a Python script to retrieve username and password from the TV.
Script from Post number 2 https://github.com/suborb/philips_android_tv

This script was a little bit confusing for me at first, as it is called philips.py and the custom component Python script is called philips2016.py. from post 2 Hope this clarifies the two different script names.

2 Different Scripts

The Pairing Script

The script by Suborb on github philips.py from post 1 gives the access to retrieve TV credentials from the Philips TV.

Clone git the script to the Linux system ( see here for detailed info about git clone ).
git clone https://github.com/cheynespc/home_assistant.git

Change the directory to philips_android_tv
cd philips_android_tv

Make sure the TV is on and is connected to a network

Find the IP address of the TV using whatever method you need to e.g. router, Fing, etc

Run the Python script
python philips.py --host TVHOSTIP_ADDRESS_HERE pair

1

Check the TV for the code that appears on the bottom right hand corner
and use this code for the next step

Go to a web browser and enter: http://YOURIPADDRESS:1926

Enter the code as directed

You will be presented with a username and password similar to this:

55.png728x62 2.96 KB
Save these details for later.

The Custom Component Media Player Python Script

The script philips_2016.py from martijnv worked best for my setup
the custom component media player and should be copied into the HA directory:

/config/custom_components/media_player .

Add Custom Media Player to Configuration.yaml

These details will be used in your configuration.yaml file when entering the media player information.

Set up media player in configuration.yaml (Philips 2016+)

 media_player:
 - platform: philips_2016
   name: Philips TV
   host: 192.168.x.x
   username: user from pairing
   password: pass from pairing
Save your configuration.yaml and reboot the system.

After system has rebooted check your states and locate the media player you just created
I reason for 2 of the same player is that I have a universal media player also that mimics the active mediaplayer in use see this link

Hope this helps.
