# Steam Deck As Gamepad For A Desktop Cemu Host
![image](https://github.com/user-attachments/assets/8c7106e3-739d-426e-8ec9-5416ade554fb)

Inspired by this [question](https://www.reddit.com/r/cemu/comments/1i4s8os/comment/mbz0jo7/), this writeup assumes you're running Cemu on your Desktop PC, with your Steam Deck acting as a GamePad for it. It should work for any Cemu host that can support Apollo.

## Host Requirements:
- [Apollo](https://github.com/ClassicOldSong/Apollo), a fork of Sunshine, is the easiest way to set up a virtual second monitor for game streaming for your host. The virtual display allows us to use a dedicated full screen display for the game pad. If you already have a second physical monitor, or have some sort of virtual display solution already set up, you may use Sunshine, or whatever streaming solution you prefer.

## Steam Deck Requirements
- [Moonlight](https://github.com/moonlight-stream/moonlight-qt/releases) is the only choice for Steam Deck as of this time of writing since Artemis isn't on Linux yet. The easiest way is to use the Discover Store in Desktop Mode to install it.
- [Steam Gyro DSU](https://github.com/kmicki/SteamDeckGyroDSU) is required to capture motion data from the deck directly.

## Optional: Set up a new application in Apollo
1. Sign in to your Apollo dashboard.
2. Go to Applications
3. Click on Add New.
4. Under Application Name, set it to "Cemu", without the quotes.
5. Under Command, put in the path to Cemu's executable. If your path to it has spaces, enclose it with quotes.
6.  If you already have a second physical monitor, you can skip this step. Check "Always use Virtual Display".
7. Click on Save.

## Client Setup
### Moonlight Setup
1. Open Moonlight on the Steam Deck.
2. Go to Settings (tap the gear icon near the upper right corner).
3. Ensure "Optimize mouse for remote desktop instead of games" is checked.

### Get your Steam Deck's IP Address
Optional: Give your Steam Deck a static IP address. The exact procedure will not be discussed here since it will depend on your home network's setup and your router. Doing this means setting up motion controls only needs to be done once.

Get your Steam Deck's IP address by going to "Settings > Internet > Connection Status". Make note of this when setting up motion controls.

## Host setup
Make sure all other game controllers are disconnected to simplify the following steps. You may get unexpected results if other controllers are connected. These steps assume an otherwise default setup for Apollo and Moonlight.

### Set up motion controls
The goal is to set up Cemu to use the Steam Deck's DSU server (courtesy of Steam Gyro DSU) to supply motion data that Cemu can use. If you've set up a static IP for your Steam Deck, these steps should only need to be done once.

1. Start Cemu.
2. Go to Options > Input settings
3. On Emulated controller, select Wii U Gamepad
4. In Controller, click on +.
5. In the new window that appears: On API, select DSUController. Don't worry about the Controller option yet.
6. In IP, use the IP address set in the previous step. Leave the port as is.
7. In Controller, select your controller. If it says "Searching for controllers...", you may need to wait for a few seconds for it to show.
8. Click on Add.
9. Make sure under Controller, the newly added DSUController is selected.
10. Click on Settings
11. Check Use motion, then click on OK.

### Set up Moonlight controls.
These steps should only need to be done once.

1. Start Cemu.
2. Go to Options > Input settings
3. On Emulated controller, select Wii U Gamepad
4. In Controller, click on +.
5. In the new window that appears: On API, select Xinput.
6. Under Controller, select the only controller that appears.
7. Click on Add. Cemu should automatically populate the buttons for you.
8. Click on the dropdown menu next to Controller, and make sure that both DSUController and XInput are listed. Cemu will make use of both "controllers" simultaneously for your selected controller.

### Set up Gamepad audio.
By default, once an Apollo stream starts, all of the host's audio is captured by the Steam Streaming Speakers. The goal is to force Cemu to output the "TV" audio to your host's speaker's instead of whatever is currently set to default, while only the gamepad audio is captured by the virtual speakers. Note that I have not tested the microphone functionality.

1. Start Cemu
2. Go to Options > General settings
3. Go to the Audio tab.
4. Under the TV section, in the Device dropdown menu, select your PC's speakers.
5. Under the Gamepad section, in the Device dropdown menu, select "Speakers (Steam Streaming Speakers)".

## Starting a Game.
1. Start Cemu
2. Ensure that in Options, Separate GamePad view is checked.
3. On your Steam Deck, start Moonlight, then start either the Cemu application we set up earlier, or the Virtual Desktop app. This should create a virtual second monitor.
4. Find the GamePad view window, then drag it over to the second monitor. Then maximize it.
5. Start your game. Once your game has loaded, click on the GamePad view window, then press Alt + Enter to set it to full screen.
6. Click the main emulator window on your main monitor, then press Alt + Enter to put that in full screen too.

## Troubleshooting
### Cemu can't detect any controllers when setting up the DSUController.
Make sure your Steam Deck and your host are on the same network.

### Motion controls used to work, but don't work anymore.
It's possible your Steam Deck's IP address has changed. We're going to remove the DSUController we've set up, and replace it with a new one with your Deck's new IP address.

1. Go to Options > Input settings
2. Under the Controller dropdown, select the DSUController, then click on -.
3. Perform the [Get your Steam Deck's IP Address](https://github.com/pogisanpolo/SteamDeckAsCemuGamepad?tab=readme-ov-file#get-your-steam-decks-ip-address), and [Set up motion controls steps](https://github.com/pogisanpolo/SteamDeckAsCemuGamepad?tab=readme-ov-file#set-up-motion-controls) again.
