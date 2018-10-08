# raspberry-pi-google-voice-assistant
using a raspberry pi as a google voice assistant
# Prerequisites
- Raspberry pi
- Adafruit I2S MEMS Microphone Breakout - SPH0645LM4H * 2
- Adafruit I2S 3W Class D Amplifier Breakout - MAX98357A
- Adafruit Perma-Proto HAT for Pi Mini Kit - With EEPROM
- Copper wire
- Soldering iron

# To Build
## Software
### Activating the microphone
1. clone the project onto the pi
'''

git clone https://github.com/shivasiddharth/custom-voice-hat 
'''
2. turn on I2 support in config.txt with
'''

sudo nano /boot/config.txt 
'''
remove the hashtag infront of
'''

#dtparam=i2s=on 
#dtparam=i2c_arm=on 
#dtparam=spi=on 
#dtparam=audio=on  
'''
then add
'''

dtparam=i2c_vc=on
'''
save and exit then reboot

3. Check for the hats folder
'''

cd /proc/device-tree/
'''

4.Open a new terminal and change directory to eepromutils
'''

cd /home/pi/custom-voice-hat/eepromutils  
'''

5.Make the EEPROM flasher executable and Flash the EEPROM with the eeprom file
'''

sudo chmod +x ./eepflash.sh 
sudo ./eepflash.sh -w -f=voicehat.eep -t=24c32
'''

6.Update Raspberry Pi kernels
'''

sudo apt-get update 
sudo apt-get install raspberrypi-kernel  
'''

7. Reboot

8. Check if your Hat is recognized
'''

cd /proc/device-tree/
'''
A new folder called HAT should appear if it has

9. move you pi into the audio recognition scripts
'''

cd /home/pi/custom-voice-hat/audio-config/scripts/
'''

10. make the scripts executable
'''

sudo chmod +x ./custom-voice-hat.sh 
sudo chmod +x ./install-i2s.sh  
'''

11. Run the scripts
'''

sudo ./custom-voice-hat.sh 
sudo ./install-i2s.sh 
'''
Reboot the pi.

12. Test the microphone by downloading Audacity or a similar voice recording software

### Setting up Google SDK for Raspberry Pi
1. Open the [Actions Console](https://console.actions.google.com)
2. Click on Add/import project
3. Create a new project by clicking the create project box
4. After creating a new project find the device registration tab near the bottom of the sidebar. Leave page open and open a new page
5. Enable [Google Assistant api](https://console.developers.google.com/apis/api/embeddedassistant.googleapis.com/overview) by searching up google assistant clicking on the item and clicking enable.
6. Click credentials on the side bar and go to OAuth consent screen. Enter your email adress and save the page.
7.set [activity controls](https://myaccount.google.com/activitycontrols) for you account to ensure the google assistant works properly.
Enable:
- Web and activity
- Device information
- Voice and audio activity
8. Go back to the device registration tab open before
9. Fill out all fields for your device and click register model
10. Download the OAuth2.0 Credentials and transfer the Json file onto the raspberry pi
'''

scp ~/Downloads/client_secret_client-id.json pi@raspberry-pi-ip-address:/home/pi/
'''
11. Skip the specific traits page copy down model id as it will be used later
12.Use a virtual environment to isolate the SDK
'''

sudo apt-get update
sudo apt-get install python-dev python-virtualenv
virtualenv env --no-site-packages
env/bin/python -m pip install --upgrade pip setuptools wheel
source env/bin/activate
'''
13. Download the package
'''

sudo apt-get install portaudio19-dev libffi-dev libssl-dev
'''
14. Use pip to install the latest version of the Google Assitant library python package in the virtual environment
'''

python -m pip install --upgrade google-assistant-sdk[samples]
'''
15.Install or update the authorization tool
'''

python -m pip install --upgrade google-auth-oauthlib[tool]
'''
16. Generate the credentials needed to run the sample codes and tools
'''

google-oauthlib-tool --scope https://www.googleapis.com/auth/assistant-sdk-prototype \
      --save --headless --client-secrets /path/to/client_secret_client-id.json
'''
17. Click on the URL shown and log in with same Gmail account as the one used for previous steps
18. Enter the code shown after logining in with Gmail into the terminal
19. Run the sample code
'''

googlesamples-assistant-pushtotalk --project-id my-dev-project --device-model-id my-model
'''
replace my-dev-project with the project id found in the [Actions console](https://console.actions.google.com/u/0/) select the project created previously, select the gear icon and select project settings

replace my-model with the model id from the previous step

Your Google Assistant should now be working



## Hardware
### Ada fruit
Solder all of the Ada fruit parts onto the Adafruit perma-proto hat according to the circuit diagram and picture provided.
