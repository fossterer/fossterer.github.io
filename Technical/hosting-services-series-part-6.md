# Hosting Services Series - Part 6 - Google Calendar and Contacts alternative 

## Update from Tue Nov 10 2026 - Contacts (CardDAV)

1. Logged into contacts.google.com. Selected all contacts and exported as single .vcf ('for Android and iOS' or such option)
2. Once a file was saved onto disk, deleted all contacts - On my phone, all contacts disappeared in some time
3. With 'Radicale' up from Docker, opened port ':5232' from web browser  - I have Tailscale setup so I could access with the published IP address of RaspberryPi
4. Deleted all default Colelctions and chose 'Upload an Address Book or Calendar' from the bottom right - Uploaded the single VCF as it is
5. On my Android phone, I had Davx5 installed - Already configured it to the same Tailscale offered IP address + port - Soon after choosing Refresh, all contacts reappeared on my phone

----

* Protocols for calendar data and contacts data are open source - CalDAV and CardDAV
* Is it tough to build i.e DIY?
* 2 popular servers that can be setup using Docker on Raspberry Pi - *Radicale* and *Baikal*
* Then install *Davx5* app on Android phone and configure the self hosted server URL. This enables the native Android calendar app to show events and the native Android contacts app to show contacts
