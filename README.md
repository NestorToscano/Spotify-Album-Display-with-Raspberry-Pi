# rpi-spotify-matrix-display

<p align="center">
  <img src="./spotipy.jpg" alt="My Project" width="500"/>
</p>

A Spotify display for 64x64 RGB LED matrices based on the software from [rpi-spotify-matrix-display](https://github.com/kylejohnsonkj/rpi-spotify-matrix-display) that I built as a personal gift project for someone special to me. I originally wanted a clean way to display live Spotify album art and playback information on an LED matrix, and after seeing a few similar projects online, I decided to try building my own version around a Raspberry Pi Zero W and a 64x64 RGB matrix panel.

The overall project took a decent amount of troubleshooting and tweaking, especially because a lot of the software dependencies and setup guides were somewhat outdated by the time I worked on it. Even though the original project provided a good starting point, there were still several issues related to package deprecations, Pi OS compatibility, matrix configuration, and hardware modifications that I had to work through myself before everything was fully functional.

## Hardware
- Adafruit 64x64 RGB LED matrix (2.5mm pitch)

- Adafruit Matrix Bonnet  
  - Modified for PWM and address E line support to properly run a 64x64 matrix

- Raspberry Pi Zero W with headers soldered on

- 32GB microSD card running Raspberry Pi OS Lite

- 5V 10A switching power supply

- 3D printed case  
  - https://www.thingiverse.com/thing:6687509#google_vignette
  - Screws and mounting hardware

I chose the Pi Zero W mainly because of its compact size and built-in wireless support, even though it was definitely pushing the limits of what the board comfortably handles for a project like this. The 64x64 matrix itself also draws a decent amount of power, so using a proper 5V 10A PSU was important to avoid instability or brightness issues.

## Spotify Pre-Setup
1. Go to https://developer.spotify.com/dashboard
2. Create an account or sign in
3. Select **Create App** (name/description does not really matter)
4. Add `http://127.0.0.1:8080/callback` under Redirect URIs
5. Save the app, then open **Settings**
6. Copy the generated Client ID and Client Secret for later

This part was fairly straightforward, but it is required since the project uses Spotify’s API to pull current playback information, album art, and track metadata in real time.

## Pi Setup
For the software side of the project, I mainly followed the setup guide from the project's wiki:

https://github.com/kylejohnsonkj/rpi-spotify-matrix-display/wiki/raspberry-pi-full-setup-guide

Although the guide helped a lot, several parts of the setup process were outdated by the time I attempted this build. Some packages either no longer existed under the same names or had dependency issues on newer Raspberry Pi OS releases. Packages like `libopenblas-dev` and some Python development libraries required workarounds or manual fixes before the software would fully install and compile correctly.

I also had to spend time debugging matrix driver issues and tuning the display configuration to get stable performance on the Pi Zero W. Since the board is relatively slow compared to newer Pis, I adjusted several matrix settings to reduce overhead and improve responsiveness.

To make the display easier to move around and reconnect at different locations, I also used `sudo nmtui` to configure multiple saved Wi-Fi networks directly through the terminal interface.

## Configuration
Most of the project configuration is handled through the `config.ini` file, and I included my own sample configuration as a reference.

For the matrix settings, I used the documentation from:

https://github.com/hzeller/rpi-rgb-led-matrix#changing-parameters-via-command-line-flags

I added my Spotify API credentials directly into the configuration file and modified the matrix parameters specifically for the Raspberry Pi Zero W and Adafruit bonnet setup. Since the Pi Zero is already fairly resource-limited, I found that additional GPIO slowdown was unnecessary for my configuration.

Because the Adafruit bonnet does not fully support this matrix configuration out of the box, I also had to solder extra connections for the PWM signal and address E line to properly drive the full 64x64 display.

Beyond the configuration file itself, I also made some direct edits inside `impl/controller_v3.py` for additional customization and behavior changes.

## Final Thoughts
Overall, this ended up being one of those projects that sounded relatively simple at first but turned into a lot of debugging and experimentation once I actually started putting everything together. A large portion of the work honestly came from solving compatibility problems and trying to get older software working reliably on newer systems and hardware.

Even though it took several days of troubleshooting, dependency fixing, recompiling libraries, and testing different matrix settings, I was happy with how the final result turned out. The display runs reliably now and makes a pretty unique way to visualize Spotify playback in real time.

## Acknowledgements
Huge thanks to Kyle Johnson and the original rpi-spotify-matrix-display project for providing the software base and original setup documentation that this project was built from.
