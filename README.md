Enhanced pianobar for macOS
================

This is rolle's awesome fork of pianobar settings for macOS.

With pianobar-macos you can achieve a working Command Line Interface music player that plays radio from [Pandora](https://www.pandora.com), scrobbles songs to [Last.fm](http://www.last.fm) and more. All this with CLI!

## Features

- Always connects to Pandora regardless of country (SOCKS5 and Tor)
- Notifies songs played with pianobar
- Loved songs in Pandora are automatically Loved songs in Last.fm when marked loved `+` key
- Displays currently playing album art in notification
- Scrobbles playing songs to Last.fm in real time and permanently when played 50% of the song
- Displays info about changing stations, displays station name in notification
- macOS Media keys support
- Displays lyrics while playing from [LyricWiki](http://lyrics.wikia.com/)

## Installation

1. [terminal-notifier](https://github.com/julienXX/terminal-notifier) - Send User Notifications on macOS 10.8 and later from the command-line:
   ```bash
   brew install terminal-notifier
   ```
2. Python3
   ```bash
   brew install python3
   ```
3. [pylast](https://github.com/pylast/pylast)
   **important:** You will need the latest version, install with:
   ```bash
   pip3 install pylast
   ```
4. pync
   ```bash 
   pip3 install pync
   ```
5. [pianobar](https://github.com/PromyLOPh/pianobar) - pianobar is a free/open-source, console-based client for the personalized online radio Pandora.
   ```bash
   brew install pianobar
   ```

## Usage

1. Install requirements (steps above)
2. Clone this repo and move everything to `~/.config/pianobar`:

   ```bash
   cd $HOME; git clone https://github.com/ronilaukkarinen/pianobar-macos; cd pianobar-macos; mkdir -p $HOME/.config/pianobar; cp -Rv $HOME/pianobar-macos/* $HOME/.config/pianobar/; rm -rf $HOME/pianobar-macos
   ```

3. Edit:

   ```bash
   nano $HOME/.config/pianobar/config
   ```
   
   Add following:

   ```ini
   #tls_fingerprint = 2D0AFDAFA16F4B5C0A43F3CB1D4752F9535507C0
   user = your@email.com
   password = YouReXtraHardPassWORd
   event_command = /Users/rolle/.config/pianobar/events.py

   # Get working proxies (if outside USA) (most of these don't seem to work after 06/2021):
   # Lists:
   # http://www.freeproxylists.net/?c=US&pt=&pr=&a%5B%5D=0&a%5B%5D=1&a%5B%5D=2&u=90
   # http://proxydb.net/?protocol=http&protocol=https&country=US
   # http://free-proxy.cz/en/proxylist/country/US/all/ping/all
   # https://www.proxynova.com/proxy-server-list/country-us/
   # https://www.us-proxy.org/
   
   #control_proxy = http://localhost:9050
   
   # Or with SOCKS5 and tor (recommended and most reliable!):
   # 1) Install: https://github.com/robertkrimen/pianobarproxy/pull/2#issuecomment-853703139
   # (In case that repo gets removed:) Just stumbled upon the same issue. FYI you who google about this like me: You can install pianobarproxy successfully with go get github.com/brendanhoran/pianobarproxy or go install github.com/brendanhoran/pianobarproxy@latest. So sad this isn't merged.
   # 2) brew install tor
   # 3) (New Terminal:) tor
   # 4) (New Terminal:) pianobarproxy -socks5 :9050
   # 5) (New terminal:) pianobar
   
   proxy = http://localhost:9090
   ```

4. Rename `events.py.sample` to `events.py`:
   ```bash
   mv $HOME/.config/pianobar/events.py.sample $HOME/.config/pianobar/events.py
   ```

5. Edit `events.py` and fill in the Last.fm variables at the top of the script.
6. Make sure everything is executable:
   ```bash
   cd $HOME/.config/pianobar && sudo chmod +x *.py && sudo chmod +x *.sh && sudo chmod +x *.rb
   ```
7. Create folder for covers:
   ```bash
   mkdir -p $HOME/.config/pianobar/.covers
   ```   
8. To add Mac media keys support, setup [PianoKeys](https://github.com/shayne/PianoKeys) (**Note:** If you use macOS Sierra or later, please use this alias:
   ```bash
   alias pianobar='osascript -e '"'"'tell application "Terminal" to do script "pianokeys"'"'"' && pianobar'
   ```
   
   See [Issue #10](https://github.com/shayne/PianoKeys/issues/10))
9. Run:
   ```bash
   pianobar
   ```

## Alias for tor + pianobar-proxy + pianobar

``` shell
alias pianobar='unset PYTHONPATH && osascript -e '"'"'tell application "Terminal" to do script "pianokeys"'"'"' && osascript -e '"'"'tell application "Terminal" to do script "tor"'"'"' && sleep 3 && osascript -e '"'"'tell application "Terminal" to do script "pianobarproxy -socks5 :9050"'"'"' && sleep 3 && pianobar'
```

## Troubleshooting

If Last.fm happens to be down, pianobar won't load any music. You should disable `event_command` line during the downtime or wait it out.

If you don't get any notifications or scrobbles, try `which python` and change the first line from your events.py to the path, for example:

``` python
#!/Library/Frameworks/Python.framework/Versions/2.7/bin/python
```

## Screenshots

![](https://i.imgur.com/VUDCm9o.png "Screenshot")

![](https://i.imgur.com/kYZSMQ7.png "Screenshot")

![](https://i.imgur.com/vdnwoYX.png "Screenshot")
