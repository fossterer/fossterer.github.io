# Hosting Services Series - Part 7 - Peer-to-Peer Sharing password vault without central server

**Syncthing + Keepass on every personal device**
- Unlike other efforts in this series, the current one does not utilize a server
- So, no installation on RaspberryPi at all. This keeps my security posture aligned with my goals as always ie. no central server which can be manipulated to expose my credentials
- Services like [Vaultwarden](https://vaultwarden.com/) can be considered if you do want a central server for storing your credentials. Otherwise, read on!

**On Device 1**
- My personal laptop always had (1) [KeePassXC - Cross-platform Password Manager](https://keepassxc.org/)
- I now setup (2) [Syncthing](https://docs.syncthing.net/users/contrib.html#docker) as [docker container from LinuxServer](https://docs.linuxserver.io/images/docker-syncthing/). See? _Docker for everything!_
- My laptop is now ready to share my KBDX file created through KeePassXC

**On Device 2**
- On my Android device, I installed (3) [KeePass2Android](https://keepass.info/download.html). Next step is to make my KBDX file reach this Android device
- Then as seen on the [Syncthing - Community Contributions](https://docs.syncthing.net/users/contrib.html#android) page, I setup (4) [Syncthing-Fork](https://github.com/researchxxl/syncthing-android)
- Finally, added both devices on each other, accepted prompts and then added folder on "Android device (Device 2)" using the same "Folder ID" seen on "Device 1".

  Done!
