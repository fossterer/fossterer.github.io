# Hosting Services Series - Part 6 - Google Calendar and Contacts alternative 

## Update from Tue Nov 10 2026 - Contacts (CardDAV)

1. Logged into [contacts.google.com](contacts.google.com). Selected all contacts and exported as single .vcf ('for Android and iOS' or such option)
2. Once the file was saved onto disk, deleted all contacts in the website - On my phone, all contacts disappeared in some time
3. With '[Radicale](https://radicale.org/v3.html)' up from Docker, opened port ':5232' from web browser  - I have Tailscale setup so I could access with the published IP address of RaspberryPi
4. Deleted all default Collections and chose 'Upload an Address Book or Calendar' from the bottom right - Uploaded the single VCF as it is
5. On my Android phone, I had [Davx5](https://radicale.org/v3.html#supported-clients) installed - Already configured it to the same [Tailscale](https://tailscale.com/download) offered IP address + port - Soon after choosing Refresh, all contacts reappeared on my phone. In addition to the _This Device_ and _My GMail account_, a 3rd - 'Davx5 Address Book' appeared now.

## Update from Tue Nov 10 2026 - Calendar (CalDAV)

No surprises - same as the above

1. Logged into [calendar.google.com](calendar.google.com). Selected my 2 calendars and exported as single zip of iCal that contained 2 .ics
2. Once the file was saved onto disk, extracted 2 ICS files, deleted both calendars in the website - On my phone, all events disappeared in some time
3. With '[Radicale](https://radicale.org/v3.html)' up from Docker, opened port ':5232' from web browser  - I have Tailscale setup so I could access with the published IP address of RaspberryPi
4. Again, I  chose 'Upload an Address Book or Calendar' from the bottom right - Uploaded the single ICS as it is. I explicitly chose not to ignore the other
5. On my Android phone, I had [Davx5](https://radicale.org/v3.html#supported-clients) installed - Already configured it to the same [Tailscale](https://tailscale.com/download) offered IP address + port - Soon after choosing Refresh, I went to the calendar app on my phone and chose 'Settings'. A new section appeared under the Gmail account with a sub-heading - 'On this device'. The other one says 'Google' which we know is empty now.

----

* Protocols for calendar data and contacts data are open source - CalDAV and CardDAV
* Is it tough to build i.e DIY?
* 2 popular servers that can be setup using Docker on Raspberry Pi - *Radicale* and *Baikal*
* Then install *Davx5* app on Android phone and configure the self hosted server URL. This enables the native Android calendar app to show events and the native Android contacts app to show contacts
