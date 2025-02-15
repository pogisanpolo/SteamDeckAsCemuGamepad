# Steam Deck As Gamepad For A Desktop Cemu Host
![image](https://github.com/user-attachments/assets/8c7106e3-739d-426e-8ec9-5416ade554fb)

Inspired by this [question](https://www.reddit.com/r/cemu/comments/1i4s8os/comment/mbz0jo7/), this writeup assumes you're running Cemu on your Desktop PC, with your Steam Deck acting as a GamePad for it. These instructions currently assume a Windows host, and are untested on non-Windows Apollo hosts. If you have a second physical monitor, or a virtual display solution of some sort, you can use these instructions as a baseline for your own solution.

# What's Supported?
- GamePad Audio on the Steam Deck, and TV Audio on the host.
- GamePad touch screen.
- Motion Controls.

# What's not supported
- GamePad camera. The Deck doesn't have a built-in camera, and even if it did, it doesn't look like Cemu supports GamePad camera input yet anyway.
- GamePad Microphone. Moonlight currently doesn't have microphone passthrough. You'll need to have a separate microphone device on your host instead.

## Host Requirements:
- [Apollo](https://github.com/ClassicOldSong/Apollo), a fork of Sunshine, is the easiest way to set up a virtual second monitor for streaming the GamePad from your host. The virtual display allows us to use a dedicated full screen display for the GamePad. If you already have a second physical monitor, or have some sort of virtual display solution already set up, you may use whatever streaming solution you prefer.

## Steam Deck Requirements
- [Moonlight](https://github.com/moonlight-stream/moonlight-qt/releases) is the only choice for Steam Deck as of this time of writing since Artemis isn't on Linux yet. The easiest way is to use the Discover Store in Desktop Mode to install it.
- [SteamDeckGyroDSU](https://github.com/kmicki/SteamDeckGyroDSU) is required to capture motion data from the Steam Deck, since the underlying streaming protocol used by Apollo doesn't transmit motion data.

## Optional: Set up a Cemu shortcut in Apollo
1. Sign in to your Apollo dashboard.
2. Go to Applications
3. Click on Add New.
4. Under Application Name, set it to "Cemu", without the quotes.
5. Under Command, put in the path to Cemu's executable. If your path to it has spaces, enclose it with quotes.
6. Check "Always use Virtual Display". If you already have a second physical monitor, you can skip this step. 
7. Click on Save.

## Steam Deck Setup
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
The goal is to set up Cemu to use the Steam Deck's DSU server (from SteamDeckGyroDSU) to supply motion data that Cemu can use. If you've set up a static IP for your Steam Deck, these steps should only need to be done once.

1. On your Steam Deck, start Moonlight, then start a stream.
2. Start Cemu.
3. Go to Options > Input settings
4. On Emulated controller, select Wii U Gamepad
5. In Controller, click on +.
6. In the new window that appears: On API, select DSUController. Don't worry about the Controller option yet.
7. In IP, use the IP address set in the previous step. Leave the port as is.
8. In Controller, select your controller. If it says "Searching for controllers...", you may need to wait for a few seconds for it to show.
9. Click on Add.
10. Make sure under Controller, the newly added DSUController is selected.
11. Click on Settings
12. Check Use motion, then click on OK.

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
By default, once a stream starts, all of the host's audio is captured by the Steam Streaming Speakers, which is then forwarded to the Deck, while muting it's output on the host side. The goal is to force Cemu to output the "TV" audio to your preferred speaker's, while only the GamePad audio is captured by the virtual speakers. 

1. Start Cemu
2. Go to Options > General settings
3. Go to the Audio tab.
4. Under the TV section, in the Device dropdown menu, select your desired speakers.
5. Under the Gamepad section, in the Device dropdown menu, select "Speakers (Steam Streaming Speakers)".

## Starting a Game.
When I refer to the "second monitor" for these instructions, I'm referring to whatever monitor your Steam Deck is streaming from. If you have an unusual setup such as having a second physical monitor that you intend to use as your Wii U TV display for some reason or another, adapt these instructions for your setup accordingly.

1. Start Moonlight on your Steam Deck, and either:
    - Pick the Cemu app if you set up the Cemu shortcut in Apollo earlier. This should start a stream and start Cemu at the same time, and possibly start a second virtual monitor.
    - Pick the Virtual Desktop app if you did not set up the Cemu app in Apollo, and don't have a second physical monitor. This will create a virtual second monitor.
    - Pick the Desktop app if you already have a second physical monitor.
2. Start Cemu if you didn't do so already in step 1. Ensure that in Options, Separate GamePad view is checked.
3. Find the GamePad view window, then drag it over to the second monitor. Then maximize it. If this is your first time using a second monitor, drag the window past the right side of your screen. It should "overflow" into the second monitor, which should be showing on the Steam Deck.
4. Start your game.
5. (Optional) Once your game has loaded, click on the GamePad view window, then press Alt + Enter to set it to full screen.
6. (Optional) Click the main emulator window on your main monitor, then press Alt + Enter to put that in full screen too.

## Troubleshooting
### Cemu can't detect any controllers when setting up the DSUController.
Make sure your Steam Deck and your host are on the same network.

### My Steam Deck is simply duplicating the same screen, and I've checked "Always use Virtual Display".
It's possible your OS has set your second monitor to duplicate. On Windows, the Windows + P button combination should show the possible display settings. Make sure it's set to "Extend" if it's currently set to Duplicate.

If the chosen option is "PC screen only", the virtual display settings may not have been saved, or you've chosen the wrong app in Moonlight. Make sure that you click on Save after checking "Always use Virtual Display", and you've selected the correct app in Moonlight.

If both are true, it may be an issue with Apollo itself, in which case, reach out to the developer for further assistance.

### Motion controls used to work, but don't work anymore.
If you have not set a static IP address on your Steam Deck, possible it's IP address has changed. We're going to remove the DSUController we've set up, and replace it with a new one with your Deck's new IP address.

1. Go to Options > Input settings
2. Under the Controller dropdown, select the DSUController, then click on -.
3. Perform the [Get your Steam Deck's IP Address](https://github.com/pogisanpolo/SteamDeckAsCemuGamepad?tab=readme-ov-file#get-your-steam-decks-ip-address), and [Set up motion controls steps](https://github.com/pogisanpolo/SteamDeckAsCemuGamepad?tab=readme-ov-file#set-up-motion-controls) again. You shouldn't need to set up the rest of the Moonlight controls again.

## Additional Resources
Have an unorthodox setup, plan on adapting these instructions to a non-Steam Deck handheld, or can't use Apollo for some reason? These tools act as the basis for the solutions above. These tools are not comprehensive, and other solutions may exist out there.

- [Sunshine.](https://github.com/LizardByte/Sunshine) The one and only, and the base of other streaming services like Duo and Apollo. While lacking certain features of the latter two services, it has the fundamentals needed to support other solutions.
- [Virtual Display Driver](https://github.com/VirtualDisplay/Virtual-Display-Driver), which is used by [SudoVDA](https://github.com/SudoMaker/SudoVDA), which in turn is used by Apollo, to create virtual displays.
- Less of a tool, and more of a guide, this [DSU Controller Guide](https://github.com/breeze2/dsu-controller-guides) can help you in setting up motion controls for your setup.
