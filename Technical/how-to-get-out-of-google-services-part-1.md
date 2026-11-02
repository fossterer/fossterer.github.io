# How to get out of Google Services - Part 1 - Gmail, Google Drive etc.

It was convenient, once upon a time, to put everything on Google.
Because, why not? It's free, right? Right?

https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmedia.tenor.com%2FcoMsDIkgwcgAAAAC%2Fpadme.gif&f=1&nofb=1&ipt=1e2b12974a163198aafc9ea06b7b025355aa0fe4ede3236783cf24f274e665b7&ipo=images

## GMail

Of course, it has to start here. How do we go about it?
In 3 steps:

1. Setup Thunderbird app on your Desktop and connect your GMail account
2. Get a new email account at [mailbox.org](https://mailbox.org) and connect the same in your Thunderbird app
3. Drag and drop folders of emails to keep from GMail to new account at mailbox.org. Done! Forget about GMail forever!

**Tip:** Get a Raspberry Pi and think of [Docker Compose](https://en.wikipedia.org/wiki/Docker_(software)) for everything you install hereafter. Or don't! You do you

## Google Photos

Immich! Don't even think of anything else

1. Setup [Immich](https://immich.app/) on your Raspberry Pi
2. Setup [Cloudflared](https://github.com/cloudflare/cloudflared) for public access.
   1. You may also setup [Tailscale](https://tailscale.com/) for private access, like VPN
   2. **Tip:** Cloudflared does not handle individual files more than 100 MB. Tailscale comes to rescue
3. Delete all your Google photos :P
   1. Or if you have something important than that photo of a bridge I took 8 years ago, download them and point the Immich mobile app to those albums

## Google Drive

Just download them all to your laptop.

Your monthly backups to a portable hard drive over rsync becomes the second copy.

One day think of setting up [Seafile](https://www.seafile.com/en/home/) on Raspberry Pi.

Cloudflared + Tailscale setup still holds

## Google Keep

Lists and reminders. Where would I be without these?

Give [Kept](https://github.com/ericerkz/kept) a try. Built with Angular, it has got both an Android and iOS app it says

## Google Contacts

[Radicale](https://radicale.org/v3.html) to the rescue! Once the server is running on Raspberry Pi, for Android, setup [DavX5](https://www.davx5.com/)
